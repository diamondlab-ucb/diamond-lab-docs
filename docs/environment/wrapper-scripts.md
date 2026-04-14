# Wrapper Scripts

Some bioinformatics tools have dependency conflicts that prevent them from
being installed in the main environment. These tools are installed in their
own isolated conda environments and accessed through **wrapper scripts**.

## How It Works

When you run a wrapped tool (e.g., `checkm1`), the wrapper script:

1. Activates the correct sibling conda environment
2. Runs the tool with all your arguments
3. Returns the output

This is completely transparent — you just run the command as normal.

## Current Wrapper Scripts

### `checkm1`

Runs CheckM v1.2.5 in the `diamond-checkm1` environment.

**Why isolated:** CheckM1 requires Python <3.12. The main environment uses
Python 3.12 for CheckM2 and other modern tools.

**Usage:**

```bash
checkm1 lineage_wf bins/ output/ -x fa -t 16
```

This is identical to running `checkm` directly, but uses the `checkm1`
command name to distinguish it from CheckM2.

## For Maintainers

Wrapper scripts live in `/groups/diamond/software/lab-env/bin/`. To add a new
wrapper, see the [Maintainer Guide](../contributing/maintainer-guide.md).
