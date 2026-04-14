# Maintainer Guide

**Current maintainers:** sdiamond, pengfanz

## Home Base

The central directory for managing all lab infrastructure is:

```
/groups/diamond/software/lab_env_docs/
```

This contains the `CLAUDE.md` reference file, design specs, and implementation
plans. Start Claude Code sessions from this directory for full context on
pipelines, databases, environments, and documentation.

## Repository Locations

| Repo | Local Path | GitHub |
|------|-----------|--------|
| diamond-lab-env | `/groups/diamond/software/lab-env/` | diamondlab-ucb/diamond-lab-env |
| diamond-lab-docs | `/groups/diamond/software/diamond-lab-docs/` | diamondlab-ucb/diamond-lab-docs |
| lab_env_docs | `/groups/diamond/software/lab_env_docs/` | — (local planning docs) |

## Common Operations

### Update the environment after editing a YAML file

```bash
cd /groups/diamond/software/lab-env
# Edit envs/diamond.yml or envs/diamond-checkm1.yml
make update
```

### Full rebuild from scratch

```bash
cd /groups/diamond/software/lab-env
make rebuild
```

### Add a new pipeline

```bash
cd /groups/diamond/software/lab-env/pipelines
git clone https://github.com/diamondlab-ucb/<repo>.git
# Edit envs/diamond.yml to add: - -e /groups/diamond/software/lab-env/pipelines/<repo>
make update
git add envs/diamond.yml
git commit -m "feat: add <pipeline-name> to shared environment"
git push
```

### Add a new tool

```bash
# Edit envs/diamond.yml to add the conda package
cd /groups/diamond/software/lab-env
make update
git add envs/diamond.yml
git commit -m "feat: add <tool-name> to shared environment"
git push
```

### Add a wrapper script for a conflicting tool

1. Create a new sibling YAML in `envs/`:

    ```yaml
    name: diamond-<toolname>
    channels:
      - conda-forge
      - bioconda
    dependencies:
      - <tool-package>
    ```

2. Create a wrapper in `bin/`:

    ```bash
    #!/bin/bash
    conda run --no-banner -n diamond-<toolname> <tool-command> "$@"
    ```

3. `chmod +x bin/<wrapper-name>`

4. Build the sibling environment:

    ```bash
    mamba env create -f envs/diamond-<toolname>.yml -p /groups/diamond/software/conda_envs/diamond-<toolname>
    ```

5. Commit and push.

### Add a database

1. Download to `/groups/diamond/databases/lab_env_db/<name>/`
2. Update `/groups/diamond/databases/lab_env_db/manifest.yml`
3. Add the environment variable to `activate.sh`
4. Update the documentation site

### Deploy documentation

```bash
cd /groups/diamond/software/diamond-lab-docs
source .venv/bin/activate
mkdocs gh-deploy
deactivate
```

## File Permissions

The `/groups/diamond/` directory uses the setgid bit (group `diamond`). New
files and directories inherit the `diamond` group. Ensure new files are
group-readable:

```bash
chmod g+r <file>
chmod g+rx <directory>
```
