# +Mᐁ includes

A GitHub action that resolves includes (by default in markdown files) and 
embeds the included files.

## Including Content

In your content files:

```html
<!-- include [RELATIVE_PATH or PUBLIC_URL][#ANCHOR] -->
```

The optional `#anchor` allows including fragments of files. Anchors are 
defined as:

```html
<!-- #ANCHOR -->
```

with an optional "closing" anchor defined exactly the same as the starting one.
If there is no closing anchor, the included content is from anchor declaration
to the end of the file. For example:

```html
<!-- #badges -->
[...] //some shields.io badges you use everywhere
<!-- #badges -->
```

Which can be included with:

```html
<!-- include common.md#badges -->
```

You can exclude a file from processing by having as the first (or last) line:

```html
<!-- exclude -->
```

## Action Usage

```
- name: +Mᐁ includes
  uses: devlooped/actions-include@v6
  with:
    # files to include for processing, i.e. '*.md'
    # Default: *.md
    include: ''

    # files to exclude from processing, i.e. 'header.md'
    # Default: ''
    exclude: ''

    # whether to recurse into subdirectories
    # Default: true
    recurse: true|false

    # Whether to validate include links. If false, a warning will be 
    # generated instead of an error.
    # Default: true
    validate: true|false
```

## Example

To run the action and automatically create a PR with the resolved includes:

```yml
on: 
  push:
    branches:
      - 'main'
    paths:
      - '**.md'    

jobs:
  includes:
    runs-on: ubuntu-latest
    steps:
      - name: 🤘 checkout
        uses: actions/checkout@v2

      - name: +Mᐁ includes
        uses: devlooped/actions-include@v1

      - name: ✍ pull request
        uses: peter-evans/create-pull-request@v3
        with:
          base: main
          branch: markdown-includes
          delete-branch: true
          labels: docs
          commit-message: +Mᐁ includes
          title: +Mᐁ includes
          body: +Mᐁ includes
```

Note how you can trivially create PRs that update the changed 
files so the includes are always resolved automatically whenever 
you change any of the monitored files. 

## How it works

You can include content from arbitrary external files with:

```Markdown
<!-- include header.md -->

# This my actual content

<!-- include https://github.com/devlooped/sponsors/raw/main/footer.md -->
# Sponsors 

<!-- sponsors.md -->
[![Clarius Org](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/clarius.png "Clarius Org")](https://github.com/clarius)
[![Christian Findlay](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MelbourneDeveloper.png "Christian Findlay")](https://github.com/MelbourneDeveloper)
[![C. Augusto Proiete](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/augustoproiete.png "C. Augusto Proiete")](https://github.com/augustoproiete)
[![Kirill Osenkov](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/KirillOsenkov.png "Kirill Osenkov")](https://github.com/KirillOsenkov)
[![MFB Technologies, Inc.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MFB-Technologies-Inc.png "MFB Technologies, Inc.")](https://github.com/MFB-Technologies-Inc)
[![SandRock](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/sandrock.png "SandRock")](https://github.com/sandrock)
[![Eric C](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/eeseewy.png "Eric C")](https://github.com/eeseewy)
[![Andy Gocke](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/agocke.png "Andy Gocke")](https://github.com/agocke)


<!-- sponsors.md -->

[![Sponsor this project](https://raw.githubusercontent.com/devlooped/sponsors/main/sponsor.png "Sponsor this project")](https://github.com/sponsors/devlooped)
&nbsp;

[Learn more about GitHub Sponsors](https://github.com/sponsors)

<!-- https://github.com/devlooped/sponsors/raw/main/footer.md -->
