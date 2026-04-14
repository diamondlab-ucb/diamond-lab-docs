# First Steps

After [activating the environment](activation.md), try these commands to
verify everything is working.

## Check Available Tools

```bash
# Verify the environment is active
echo $CONDA_DEFAULT_ENV
# Should print: diamond

# Check a few tools
snakemake --version
seqkit version
checkm2 --version
```

## Check Database Paths

```bash
echo $DIAMOND_LAB_DB
# Should print: /groups/diamond/databases/lab_env_db

ls $GTDBTK_DATA_PATH
ls $CHECKM2DB
```

## Run a Pipeline

To run meta-pipeline-MAGDrep on a set of MAG FASTA files:

```bash
meta-pipeline-MAGDrep run -i /path/to/your/mags/ -o /path/to/output/
```

For SLURM cluster execution:

```bash
meta-pipeline-MAGDrep run -i /path/to/your/mags/ -o /path/to/output/ --profile slurm
```

See the [meta-pipeline-MAGDrep page](../pipelines/meta-pipeline-MAGDrep.md)
for full usage details.
