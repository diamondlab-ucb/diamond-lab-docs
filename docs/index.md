# Diamond Lab Documentation

Welcome to the Diamond Lab shared bioinformatics infrastructure documentation.

## What is this?

The Diamond Lab maintains a **shared bioinformatics environment** — a single
conda environment containing our lab-developed pipelines, the tools they
depend on, and version-tracked databases. Instead of each lab member installing
software individually, you activate one environment and everything is ready.

## Quick Start

```bash
source /groups/diamond/software/lab-env/activate.sh
```

That's it. You now have access to all shared tools, pipelines, and databases.

## What's Available

- **Pipelines:** [meta-pipeline-MAGDrep](pipelines/meta-pipeline-MAGDrep.md) — MAG quality assessment, taxonomy, and dereplication
- **Databases:** GTDB r226, CheckM2 DB, CheckM1 DB ([full list](databases/environment-dbs.md))
- **Tools:** Snakemake, SeqKit, CheckM2, GTDB-Tk, skani, and more ([full list](environment/tools.md))

## New to the lab?

Start with [Activating the Environment](getting-started/activation.md), then
try [First Steps](getting-started/first-steps.md).
