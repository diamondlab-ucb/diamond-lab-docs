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

- **`full`** — ORF + smORF + tRNA + ncRNA (Infernal cmscan vs full Rfam) +
  CRISPR + MGE + geNomad. Deepest annotation; appropriate for
  publication-quality runs on **up to a few thousand MAGs**. Needs Rfam +
  geNomad databases.

- **`streamlined`** — ORF + tRNA + ncRNA (Infernal cmscan vs rRNA-only Rfam
  subset) + CRISPR. Same Infernal-based rRNA detection, smaller CM set;
  ~25 % faster than `full` per MAG. Mid-scale runs (5k–50k MAGs). Needs
  Rfam database.

- **`fast`** *(added in v1.4.0)* — ORF + tRNA + **rRNA via barrnap** +
  CRISPR. Replaces Infernal/cmscan with barrnap (HMM-based, full-length
  rRNAs only) — **~70× faster per MAG** than the cmscan path on benchmark
  data. **No Rfam or geNomad databases required.** Intended for large
  **50k–300k-MAG** runs on a SLURM cluster where cmscan would be the
  wall-time bottleneck. No general ncRNA, smORF, MGE, or geNomad.

| Mode | rRNA tool | tRNA tool | Other ncRNA | smORF/MGE/geNomad | DBs needed | Typical scale |
|------|-----------|-----------|-------------|-------------------|------------|---------------|
| `full` | cmscan (full Rfam) | tRNAscan-SE | yes | yes | Rfam + geNomad | ≤ few k |
| `streamlined` | cmscan (rRNA Rfam) | aragorn | rRNA only | no | Rfam | 5k–50k |
| `fast` | **barrnap** | aragorn | no | no | **none** | 50k–300k |

## tRNA tool selection

`--trna-tool` picks the tRNA detector independently of `--mode`. Choices:
`trnascan-SE` (the conservative default for `full`-mode publication
runs) or `aragorn` (the default for `streamlined` and `fast`). aragorn
is ~165× faster on bacterial MAGs with sensitivity that matches
tRNAscan-SE bac mode to within ±1–2 tRNAs on our benchmark fixture, so
it's the right default whenever you're optimizing for throughput.

```bash
# Override the mode default — e.g. force tRNAscan-SE in fast mode
meta-pipeline-ORFanno annotate -i mags/ -o out/ --mode fast --trna-tool trnascan-SE
```

## SLURM execution (biotite)

```bash
meta-pipeline-ORFanno annotate -i mags/ -o output/ --mode fast --profile slurm
```

The SLURM profile is tuned for biotite's QOS (10 concurrent running, 200
submitted per user) and heterogeneous nodes. Per-rule memory is set in the
profile; rules are grouped per-MAG so each whole-node SLURM submission
processes many MAGs.

**Per-run overrides for big jobs:**

```bash
# Pack many MAGs per node-acquisition (minimizes queue waits on a busy cluster)
meta-pipeline-ORFanno annotate -i mags/ -o output/ --mode fast --profile slurm \
  --config group_components=1000

# Target the large-memory partition for very wide groups
meta-pipeline-ORFanno annotate -i mags/ -o output/ --mode fast --profile slurm \
  --config group_components=1000 slurm_partition=high-memory
```

The cross-MAG aggregation rules (`concat_*`, `summary_table`) run as
`localrules` on the submission/driver node, so they do not consume your
10-node budget.

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
