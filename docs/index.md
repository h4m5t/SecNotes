---
title: index
---

# SecNotes

记录安全学习笔记。持续更新。

本项目地址：https://github.com/h4m5t/SecNotes

## 文档构建

### 环境

GithubPages+Mkdocs+mkdocs-material+GitHub Actions


### with GitHub Actions

Using [GitHub Actions](https://github.com/features/actions) you can automate the deployment of your project documentation. At the root of your repository, create a new GitHub Actions workflow, e.g. `.github/workflows/ci.yml`, and copy and paste the following contents:

```
name: ci 
on:
  push:
    branches:
      - master 
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 
      - uses: actions/cache@v3
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material 
      - run: mkdocs gh-deploy --force
```

Now, when a new commit is pushed to either the `master` or `main` branches, the static site is automatically built and deployed. Push your changes to see the workflow in action.

If the GitHub Page doesn't show up after a few minutes, go to the settings of your repository and ensure that the [publishing source branch](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) for your GitHub Page is set to `gh-pages`.

Your documentation should shortly appear at `<username>.github.io/<repository>`.

### with MkDocs

If you prefer to deploy your project documentation manually, you can just invoke the following command from the directory containing the `mkdocs.yml` file:

```
mkdocs gh-deploy --force
```



