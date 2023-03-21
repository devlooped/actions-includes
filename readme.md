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
```

When the action runs for the first time, it will turn the 
above content into:

```Markdown
<!-- include header.md -->
This comes from the included header!
<!-- header.md -->

# This my actual content

<!-- include https://github.com/devlooped/sponsors/raw/main/footer.md -->
This comes from the included footer from a URL!
<!-- https://github.com/devlooped/sponsors/raw/main/footer.md -->
```

The action is idempotent, so it is safe for it to run on pushes of the 
same files it changed via the includes (since no further changes will 
be detected).

> NOTE: the included path must be relative to the including file. 

## Limitations

* Nested includes are not supported for now (the `include` keyword inside the HTML 
  comment "directive" will be stripped on inclusion).
* File processing order matches what the following equivalent command on the 
  working directory returns via PowerShell:
  
  ```
  pwsh> gci -include [include] -exclude [exclude] [-recurse]
  ```

  This knowledge can be used to workaround the nested includes limitation by 
  running the action multiple times with different sets of files.


[![Sponsor this project](https://raw.githubusercontent.com/devlooped/sponsors/main/sponsor.png "Sponsor this project")](https://github.com/sponsors/devlooped)
&nbsp;

[Learn more about GitHub Sponsors](https://github.com/sponsors)