# Environment Databases

Databases that support tools and pipelines in the shared environment. These
are automatically configured when you activate the lab environment.

## Current Databases

| Database | Version | Size | Tool | Env Variable |
|----------|---------|------|------|-------------|
| GTDB | r226 | ~85 GB | GTDB-Tk | `$GTDBTK_DATA_PATH` |
| CheckM2 DB | 2.0.0 | ~3 GB | CheckM2 | `$CHECKM2DB` |
| CheckM1 DB | 2015-01-16 | ~1.4 GB | CheckM1 | `$CHECKM_DATA_PATH` |

## Accessing Databases

After activating the environment, database paths are available as environment
variables:

```bash
source /groups/diamond/software/lab-env/activate.sh

# Use in scripts or commands
echo $GTDBTK_DATA_PATH    # /groups/diamond/databases/lab_env_db/gtdb-r226
echo $CHECKM2DB           # /groups/diamond/databases/lab_env_db/checkm2
echo $CHECKM_DATA_PATH    # /groups/diamond/databases/lab_env_db/checkm1
```

Most tools read these variables automatically — you don't need to pass
database paths manually.

## Version Manifest

The full version manifest is at:

```
/groups/diamond/databases/lab_env_db/manifest.yml
```

It tracks the version, install date, source URL, and size of each database.
