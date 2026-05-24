# Database Versions

## Environment Databases

Current versions of databases in the shared environment. These are tracked
in the manifest at `/groups/diamond/databases/lab_env_db/manifest.yml`.

| Database | Current Version | Last Updated | Notes |
|----------|----------------|--------------|-------|
| GTDB | r232 | 2026-05-23 | GTDB-Tk reference data |
| CheckM2 DB | 2.0.0 | 2026-04-14 | DIAMOND protein database |
| CheckM1 DB | 2015-01-16 | 2026-04-14 | Marker gene HMMs |

## Update History

### 2026-05-23 — GTDB-Tk database upgrade

- GTDB r232 installed (replaces r226 as the active reference)
- GTDB-Tk binary upgraded from 2.5.2 to 2.7.2
- R226 retained on disk at `lab_env_db/gtdb/release226/` as a dormant
  fallback

### 2026-04-14 — Initial Setup

- GTDB r226 installed
- CheckM2 DB 2.0.0 installed
- CheckM1 DB installed
