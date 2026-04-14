# FAQ

## Can I still use my own conda environments?

Yes. The lab environment is a shared resource, not a replacement. Deactivate
it with `conda deactivate` and activate your own environment as usual.

## Can I install packages into the shared environment?

No. The shared environment is maintained by sdiamond and pengfanz to ensure
reproducibility. If you need a tool added, see
[Adding Tools](../contributing/adding-tools.md).

## Will activating the lab environment break my own setup?

No. It only affects your current shell session. Your personal environments,
PATH, and configuration are unchanged. `conda deactivate` restores your
previous state.

## What if a tool I need conflicts with the shared environment?

Contact the maintainers. Conflicting tools can be installed in a sibling
environment with a wrapper script, so they appear to work seamlessly. See
[Wrapper Scripts](../environment/wrapper-scripts.md).

## How do I know what version of a database is installed?

Check the database manifest:

```bash
cat /groups/diamond/databases/lab_env_db/manifest.yml
```

Or see the [Database Versions](../databases/versions.md) page.
