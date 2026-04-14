# Adding Pipelines

## For Pipeline Developers

To add a new lab-developed pipeline to the shared environment:

### Prerequisites

- Pipeline must be in its own GitHub repository
- Pipeline must be installable via `pip install -e .` (i.e., has a
  `pyproject.toml` or `setup.py`)
- Pipeline dependencies should be compatible with the current environment
  (or clearly documented if they conflict)

### Process

1. **Open an issue** on the
   [diamond-lab-env](https://github.com/diamondlab-ucb/diamond-lab-env) repo
   describing the pipeline, its dependencies, and any database requirements.

2. A maintainer will:
    - Clone the repo into `pipelines/`
    - Add the pip entry to `envs/diamond.yml`
    - Add any conflicting tools to sibling environments with wrappers
    - Download required databases to `lab_env_db/` and update the manifest
    - Rebuild the environment and test
    - Add a documentation page for the pipeline

3. **Provide documentation content** for the pipeline's usage within the lab
   environment (similar to the
   [meta-pipeline-MAGDrep page](../pipelines/meta-pipeline-MAGDrep.md)).
