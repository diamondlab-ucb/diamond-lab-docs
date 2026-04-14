# Database Overview

The Diamond Lab maintains two tiers of shared databases.

## Environment Databases

**Location:** `/groups/diamond/databases/lab_env_db/`

These are databases that are **direct dependencies of tools and pipelines** in
the shared environment. They are version-tracked in a manifest file and
automatically configured when you activate the environment.

Examples: GTDB reference data (for GTDB-Tk), CheckM2 DIAMOND database,
CheckM1 marker gene HMMs.

See [Environment Databases](environment-dbs.md) for the full list.

## Lab Databases

**Location:** `/groups/diamond/databases/`

These are broader lab resources used across various projects and analyses.
They are not tied to a specific tool in the shared environment.

Examples: 16S reference databases, KEGG, UniProt, Silva, HMM model
collections, reference genome sets, test data.

See [Lab Databases](lab-dbs.md) for the full list.

## Key Difference

| | Environment Databases | Lab Databases |
|---|---|---|
| **Location** | `lab_env_db/` subdirectory | Top-level `databases/` subdirectories |
| **Purpose** | Tool dependencies | General lab resources |
| **Version tracking** | `manifest.yml` | Documented in these docs |
| **Auto-configured** | Yes (via `activate.sh`) | No (use paths directly) |
