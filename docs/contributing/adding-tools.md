# Adding Tools

## Requesting a Tool

If you need a bioinformatics tool added to the shared environment, contact
sdiamond or pengfanz with:

1. **Tool name and version**
2. **Why you need it** — which analysis or pipeline will use it
3. **Installation method** — conda package name, pip package, or source URL

## What Happens Next

A maintainer will:

1. Test whether the tool can be added to the main `diamond` environment
   without dependency conflicts
2. If it conflicts, set it up in a sibling environment with a wrapper script
3. Update `envs/diamond.yml` (or create a new sibling env YAML)
4. Rebuild the environment
5. Update this documentation

## Tools from /shared/

The cluster provides many tools at `/shared/software/`. The lab environment
is independent of these — if you need a tool that's available system-wide,
it still needs to be explicitly added to the lab environment for it to be
available after activation.

Alternatively, you can `conda deactivate`, use the system tool, and
re-activate the lab environment when done.
