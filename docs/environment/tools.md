# Installed Tools

Tools in the shared `diamond` conda environment. This list grows as pipelines
are added.

## Pipelines

| Pipeline | Version | Description |
|----------|---------|-------------|
| meta-pipeline-MAGDrep | 1.3.0 | MAG quality assessment, GTDB taxonomy, species-level dereplication |

## Bioinformatics Tools

| Tool | Version | Used By |
|------|---------|---------|
| checkm2 | 1.1.0 | meta-pipeline-MAGDrep |
| diamond | 2.1.11 | meta-pipeline-MAGDrep (via CheckM2) |
| fastani | 1.34 | meta-pipeline-MAGDrep (via GTDB-Tk) |
| gtdbtk | 2.5.2 | meta-pipeline-MAGDrep |
| hmmer | 3.4 | meta-pipeline-MAGDrep (via GTDB-Tk) |
| pplacer | 1.1.alpha22 | meta-pipeline-MAGDrep (via GTDB-Tk) |
| prodigal | 2.6.3 | meta-pipeline-MAGDrep (via CheckM2, GTDB-Tk) |
| seqkit | 2.13.0 | meta-pipeline-MAGDrep |
| skani | 0.3.1 | meta-pipeline-MAGDrep |
| snakemake | 9.x | Workflow engine for all pipelines |

## Wrapper Scripts (Sibling Environments)

| Command | Actual Tool | Sibling Env | Reason |
|---------|-------------|-------------|--------|
| `checkm1` | CheckM 1.2.5 | diamond-checkm1 | Requires Python <3.12 |

## Python Stack

| Package | Version |
|---------|---------|
| python | 3.12 |
| scipy | latest |
| biopython | latest |
| click | latest |
| pyyaml | latest |
