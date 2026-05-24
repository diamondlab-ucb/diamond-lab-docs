# Environment Variables

Variables set by `source /groups/diamond/software/lab-env/activate.sh`.

## General

| Variable | Value | Description |
|----------|-------|-------------|
| `DIAMOND_LAB_DB` | `/groups/diamond/databases/lab_env_db` | Root path to environment databases |

## Database Paths

| Variable | Value | Tool |
|----------|-------|------|
| `GTDBTK_DATA_PATH` | `$DIAMOND_LAB_DB/gtdb/release232` | GTDB-Tk |
| `CHECKM2DB` | `$DIAMOND_LAB_DB/checkm2` | CheckM2 |
| `CHECKM_DATA_PATH` | `$DIAMOND_LAB_DB/checkm1` | CheckM1 |

## PATH Modifications

The activation script prepends the wrapper script directory to `PATH`:

```
/groups/diamond/software/lab-env/bin
```

This makes wrapper scripts (e.g., `checkm1`) available as commands.
