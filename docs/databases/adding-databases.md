# Adding Databases

!!! note "Maintainers only"
    This page is for environment maintainers (sdiamond, pengfanz).

## Adding an Environment Database

When a new tool or pipeline requires a database:

1. Download the database to `/groups/diamond/databases/lab_env_db/<name>/`
2. Update the manifest:

    ```bash
    vim /groups/diamond/databases/lab_env_db/manifest.yml
    ```

    Add an entry with: tool name, version, install date, size, source URL,
    path, and the environment variable name the tool expects.

3. Update `activate.sh` in the lab-env repo to export the new environment
   variable.

4. Update this documentation site.

## Adding a Lab Database

Lab databases go directly in `/groups/diamond/databases/`:

1. Create a directory: `/groups/diamond/databases/<name>/`
2. Download or generate the data
3. Update the [Lab Databases](lab-dbs.md) page in these docs
