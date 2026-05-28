# Pipelines Overview

Lab-developed pipelines. Most live inside the shared `diamond` env and are
available after activation; some (currently meta-pipeline-ORFanno) live in
their own conda env alongside the shared one — see each pipeline's page.

## Pipelines

| Pipeline | Version | Description | Env | Repo |
|----------|---------|-------------|-----|------|
| [meta-pipeline-MAGDrep](meta-pipeline-MAGDrep.md) | 1.4.0 | MAG quality, taxonomy (GTDB r232) & dereplication | shared `diamond` | [GitHub](https://github.com/SDmetagenomics/meta-pipeline-MAGDrep) |
| [meta-pipeline-ORFanno](meta-pipeline-ORFanno.md) | 0.1.0 | Structural annotation of MAGs (ORF, tRNA, rRNA, CRISPR, MGE, geNomad); `fast` mode for 50k–300k-MAG runs | standalone `orfanno` env | [GitHub](https://github.com/SDmetagenomics/meta-pipeline-ORFanno) |
