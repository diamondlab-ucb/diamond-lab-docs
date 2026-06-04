# meta-pipeline-ORFanno

Structural annotation of prokaryotic MAGs at scale: ORF prediction, small
ORFs, ncRNA, tRNA, CRISPR, mobile elements, and prophage/plasmid detection
— from a directory of FASTA files to cross-MAG summary tables and
concatenated FASTA exports.

**Repository:** [SDmetagenomics/meta-pipeline-ORFanno](https://github.com/SDmetagenomics/meta-pipeline-ORFanno)

!!! note "Standalone conda env (not yet in the shared `diamond` env)"
    ORFanno currently has its own conda env at
    `/groups/diamond/software/conda_envs/orfanno/`. You activate **that env**
    directly, not the lab `activate.sh`. Folding ORFanno into the shared
    `diamond` env is a v1.0 follow-up; until then it lives alongside the
    shared env rather than inside it.

## Usage

```bash
# Activate the orfanno conda env (separate from the lab diamond env)
source /shared/software/miniconda3/latest/etc/profile.d/conda.sh
conda activate /groups/diamond/software/conda_envs/orfanno

# One-time: download Rfam + geNomad databases (skip for `fast` mode — see below)
meta-pipeline-ORFanno db update --db-dir /path/to/orfanno_dbs

# Run
meta-pipeline-ORFanno annotate -i /path/to/mags/ -o /path/to/output/ --mode full
```

Input is a directory of MAG FASTA files (`.fna` / `.fa` / `.fasta`, plain
or gzipped). Each filename stem becomes the `mag_id` used throughout all
outputs; duplicate stems abort the pipeline at startup before any cluster
jobs run.

## Annotation modes

Three presets, picked with `--mode`:

- **`full`** — ORF + smORF + tRNA + **rRNA (cmscan via Rfam_rRNA.cm)** +
  ncRNA (cmscan via Rfam_no_rrna.cm) + CRISPR + MGE + geNomad. Deepest
  annotation; appropriate for publication-quality runs on **up to a few
  thousand MAGs**. Needs Rfam + geNomad databases.

- **`streamlined`** — ORF + tRNA + **rRNA (barrnap by default;
  `--rrna-tool=cmscan` opt-in)** + CRISPR + MGE. Drops the Rfam-other-ncRNA
  scan from full mode; keeps full structural annotation surface (ORFs,
  rRNAs, tRNAs, CRISPR, mobile elements). Mid-scale runs (5k–50k MAGs).
  No Rfam required by default; `--rrna-tool=cmscan` re-introduces a
  Rfam_rRNA.cm dependency.

- **`fast`** — ORF + tRNA + **rRNA (barrnap)** + CRISPR. Same as
  streamlined minus MGE — the cheapest deployable profile. Intended for
  very large **50k–300k-MAG** runs on a SLURM cluster where cmscan would
  be the wall-time bottleneck. **No Rfam or geNomad databases required.**

| Mode | rRNA tool (default) | tRNA tool (default) | Other ncRNA | smORF/MGE/geNomad | DBs needed | Typical scale |
|------|--------------------|--------------------|-------------|-------------------|------------|---------------|
| `full` | cmscan (Rfam_rRNA.cm) | tRNAscan-SE | yes (Rfam_no_rrna.cm) | yes | Rfam + geNomad | ≤ few k |
| `streamlined` | barrnap | aragorn | no | mge only | none | 5k–50k |
| `fast` | barrnap | aragorn | no | no | none | 50k–300k |

## tRNA + rRNA tool selection

Both detectors are tool-selectable independently of `--mode`. The flags
take precedence over the per-mode defaults.

`--trna-tool {trnascan-SE | aragorn}` — per-mode default: `full=trnascan-SE`, `streamlined/fast=aragorn`. aragorn is ~165× faster on bacterial MAGs with equivalent sensitivity.

`--rrna-tool {barrnap | cmscan}` — per-mode default: `full=cmscan`, `streamlined/fast=barrnap`. barrnap is much faster (full-length rRNAs only); cmscan also finds fragments and 5.8S.

```bash
# Override per-mode defaults — e.g. force the conservative tools in streamlined mode
meta-pipeline-ORFanno annotate -i mags/ -o out/ --mode streamlined \
  --trna-tool trnascan-SE --rrna-tool cmscan
```

### Behavior change in v0.x (streamlined-mode default)

Streamlined mode now defaults to **barrnap** for rRNA detection (was cmscan).
Existing pipelines relying on the cmscan output exactly can restore prior
behavior with `--rrna-tool=cmscan`. Streamlined mode also now includes the
`mge` (ISEScan) step by default — use `--skip mge` to drop it.

## SLURM execution (biotite)

```bash
meta-pipeline-ORFanno annotate -i mags/ -o output/ --mode fast --profile slurm
```

The SLURM profile pins every submission to one of biotite's 33 **64-CPU /
768 GB nodes** (`node-64-768g-[1-33]`) via an explicit nodelist, and packs
16 MAGs per node-group submission so the heavy concurrent phase
(`orf_prediction` at 4 threads each) fully utilizes the 64 cores.

With biotite's QOS (10 concurrent running, 200 submitted per user), this
gives **~160 MAGs in flight** at any moment for fast/streamlined runs.

**Per-mode tuning:**

```bash
# Full mode — cmscan is heavier; drop group size to 8
meta-pipeline-ORFanno annotate -i mags/ -o output/ --mode full --profile slurm \
  --config group_components=8
```

The cross-MAG aggregation rules (`concat_*`, `summary_table`) run as
`localrules` on the submission/driver node, so they do not consume your
10-node budget.

**Measuring per-node utilization:** `scripts/benchmark_slurm_utilization.py`
scrapes `sacct` and emits per-job CPU% (target ≥80% on the allocated node
during peak). See `config/profiles/slurm/README.md` for full design details.

## Output

Top-level files in the output dir:

- `all_features.tsv` — cross-MAG feature table (one row per ORF / tRNA /
  rRNA / CRISPR feature / etc.). All modes write to the same schema.
- `all_rrna_{5s,16s,23s}.fna` — full-length rRNA exports (present in any
  mode that detects rRNA).
- `all_ncrna.fna` — general ncRNA exports (full / streamlined only;
  `fast` does not produce this).
- `all_orf_{proteins.faa,dna.fna}`, `all_trna.fna`,
  `all_crispr_{repeats,spacers}.fna`, `all_is_elements.fna`,
  `all_prophage.fna`, `all_plasmid.fna` — present per the active mode.
- `summary_table.tsv` — per-MAG summary (genome stats + per-step feature
  counts).
- `scaffold_name_mapping.tsv` — every input contig is renamed to
  `{mag_id}_scaffold_N` so cross-MAG outputs have globally-unique contig
  IDs; this file is the audit trail mapping new IDs back to originals.

## Full documentation

See the [pipeline README](https://github.com/SDmetagenomics/meta-pipeline-ORFanno)
for all options, output schemas, and developer details.
