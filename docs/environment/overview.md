# Environment Overview

## Design Philosophy

The Diamond Lab environment provides a **single, shared conda environment**
containing all lab-developed pipelines and the tools they depend on. The
design principles are:

1. **One activation** — `source activate.sh` and everything works
2. **Reproducible** — version-locked dependencies and databases
3. **Independent** — does not depend on system-wide `/shared/` software
4. **Grows organically** — tools are added only when a pipeline needs them,
   not pre-loaded "just in case"

## How It Works

The environment is defined by YAML files in the
[diamond-lab-env](https://github.com/diamondlab-ucb/diamond-lab-env)
repository:

- `envs/diamond.yml` — the main environment with all tools
- `envs/diamond-checkm1.yml` — sibling environment for CheckM1 (Python
  version conflict)

When you activate the environment, a shell script:

1. Activates the `diamond` conda environment
2. Adds wrapper scripts to your PATH (for tools in sibling environments)
3. Sets environment variables pointing to the correct database versions
4. Prints a summary of what's available

## What's Not Included

The lab environment does not include every bioinformatics tool. It contains
what our pipelines need. The cluster's system-wide tools at `/shared/software/`
remain available when the lab environment is not active.
