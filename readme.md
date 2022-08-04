# +M▼ includes

A GitHub action that resolves includes (by default in markdown files) and 
embeds the included files.

Usage:

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

      - name: +M▼ includes
        uses: devlooped/actions-include@v1
        with: 
            include: '*.md'

      - name: ✍ pull request
        uses: peter-evans/create-pull-request@v3
        with:
          base: main
          branch: markdown-includes
          delete-branch: true
          commit-message: +M▼ includes
          title: "+M▼ includes"
```

Note how you can trivially create PRs that update the changed 
files so the includes are always resolved automatically whenever 
you change any of the monitored files. The `include: '*.md'` 
above wouldn't be required since it's the default.