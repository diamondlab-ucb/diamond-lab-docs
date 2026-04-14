# Activating the Environment

## One-Time Activation

Run this command in your terminal:

```bash
source /groups/diamond/software/lab-env/activate.sh
```

You'll see a welcome banner confirming the environment is active. All shared
tools and pipelines are now available in your current shell session.

## Auto-Activation (Optional)

To activate the environment every time you log in, add this line to your
`~/.bashrc`:

```bash
# Activate Diamond Lab shared environment
source /groups/diamond/software/lab-env/activate.sh
```

!!! note
    Auto-activation means the lab environment is always active. If you
    primarily use your own conda environments, you may prefer to activate
    manually when needed.

## Deactivation

```bash
conda deactivate
```

This returns you to your base conda environment (or no environment).

## Using Your Own Environments

The lab environment does **not** replace your personal conda environments.
You can still create and use your own environments as before. The lab
environment is an additional shared resource.

If you need a tool that isn't in the shared environment, you have two options:

1. **Request it** — see [Adding Tools](../contributing/adding-tools.md)
2. **Install it yourself** — in your own conda environment, as you always have

## Troubleshooting

**"conda: command not found"**

Conda needs to be initialized for your shell. Run:

```bash
source /shared/software/miniconda3/latest/etc/profile.d/conda.sh
```

Then try the activation command again. To make this permanent, add the line
above to your `~/.bashrc` before the lab environment activation line.

**"Failed to activate 'diamond' conda environment"**

The environment may not be built yet. Contact sdiamond or pengfanz.
