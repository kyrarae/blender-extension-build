# Blender Extension Build

This GitHub action automates the build process for Blender extensions, producing a `.zip` artifact which can be installed into Blender.

## Description

The **Blender Extension Build** action allows for the automatic building of Blender extensions.
At the end of the process, the action stores the built extension as a `.zip` artifact using `actions/upload-artifact`.

## Inputs

### `name`

**Description**: Name of the extension. Used for the name of the artifact.
If empty, manifest ID name will be used.
**Default**: `''` (empty string)

> [!IMPORTANT]
> If the name is empty, the ID from `blender_manifest.toml` will be used. If you want to build legacy add-ons you either have to use a name or add a manifest. Older Blender versions will ignore the manifest.

### `version`

**Description**: Version tag used as suffix for the artifact (`name-*.*.*.zip`).
**Default**: `''` (empty string)

> [!IMPORTANT]
> If the version is empty, the version from `blender_manifest.toml` will be used. If you want to build legacy add-ons you either have to use a version or add a manifest. Older Blender versions will ignore the manifest.

### `build-location`

**Description**: Path to the directory which will be zipped to create the artifact.
**Default**: `'./'` (current directory)

### `checkout`

**Description**: Determines whether the action should checkout the repository.
**Default**: `'true'`

If you need to checkout the repository before and do some changes, select false so the action does not checkout again and overwrite the files.

### `exclude-files`

**Description**: A list of file paths, relative to `build-location`, to exclude from the build. Use a semicolon as a separator, e.g., `"file1;dir/file2;dir2/file3"`.
**Default**: `'.git'`

If your repository contains files and/or directories which should not be included in the build, you can specify them here, e.g., `.git;docs;README.md;tests;.github`.

## Outputs

The following outputs can be accessed via `${{ steps.<step-id>.outputs }}` from this action

### `artifact-name`

**Description**: The name of the generated artifact.
**Type**: `string`

**How to use it:**

In subsequent jobs: Use `actions/download-artifact` with this name to retrieve the files.

In the current workflow: Access the value via the `${{ env.artifact_name }}` environment variable.

## Usage

```yaml
name: Example

on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build Extension
        uses: kyrarae/blender-extension-build@main
        with:
          checkout: 'false'
          exclude-files: '.git;.github;.gitignore'
```
