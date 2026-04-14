# meta-pipeline-MAGDrep

Quality assessment, taxonomic classification, and species-level dereplication
of metagenome-assembled genomes (MAGs) at scale (10 to 100,000+ genomes).

**Repository:** [SDmetagenomics/meta-pipeline-MAGDrep](https://github.com/SDmetagenomics/meta-pipeline-MAGDrep)

## Usage in the Lab Environment

After activating the lab environment, the pipeline is available directly:

```bash
source /groups/diamond/software/lab-env/activate.sh
meta-pipeline-MAGDrep run -i /path/to/mags/ -o /path/to/output/
```

All required databases are pre-configured via environment variables. You do
**not** need to download databases or specify database paths.

## Pipeline Steps

| Step | Tool | What It Does |
|------|------|-------------|
| `genome_stats` | SeqKit | Length, GC, N50, contig count |
| `checkm1` (optional) | CheckM1 | Marker-gene completeness/contamination |
| `checkm2` | CheckM2 | Neural-net completeness/contamination |
| `gtdbtk` | GTDB-Tk | Taxonomy via GTDB r226 |
| `dereplicate` | skani + scipy | All-vs-all ANI, species-level clustering |

## SLURM Execution

For cluster execution:

```bash
meta-pipeline-MAGDrep run -i mags/ -o output/ --profile slurm
```

## Selecting Steps

Run specific steps or skip steps:

```bash
# Only run genome_stats and checkm2
meta-pipeline-MAGDrep run -i mags/ -o output/ --steps genome_stats checkm2

# Skip the optional checkm1 step
meta-pipeline-MAGDrep run -i mags/ -o output/ --skip checkm1
```

## Required Databases

All configured automatically in the lab environment:

| Database | Env Variable | Path |
|----------|-------------|------|
| GTDB r226 | `$GTDBTK_DATA_PATH` | `lab_env_db/gtdb-r226/` |
| CheckM2 DB | `$CHECKM2DB` | `lab_env_db/checkm2/` |
| CheckM1 DB | `$CHECKM_DATA_PATH` | `lab_env_db/checkm1/` |

## Full Documentation

See the [pipeline README](https://github.com/SDmetagenomics/meta-pipeline-MAGDrep)
for complete documentation including all options and output format details.
