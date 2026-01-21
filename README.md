# 🌟 gaze-stars

[English](README.md) | [中文版](README.zh-CN.md)

This GitHub Action queries the GitHub API to retrieve your starred repositories and generates a README sorted by your stargazed list. The generated README includes a Table of Contents before the categorized entries so you can jump to any section quickly.

You can reference my repository [zhuozhiyongde / Stargazer](https://github.com/zhuozhiyongde/Stargazer)

## Documentation

### Prerequisites

- An empty repository
- A personal GitHub API token

### Configuration

| Variable          | Description                                                    | Default                                    |
| ----------------- | -------------------------------------------------------------- | ------------------------------------------ |
| `api-token`       | Personal GitHub API token to avoid rate limits [Learn more](#api-token) | /                 |
| `github-username` | GitHub username for generating starred list                   | /                                          |
| `git-name`        | Name used for Git commits                                      | `Github Action`                            |
| `git-email`       | Email used for Git commits                                     | `actions@users.noreply.github.com`         |
| `git-message`     | Commit message for Git commits                                 | `chore(updates): updated entries in files` |
| `sort-by`         | Sort by `stars` or `updated`                                  | `stars`                                    |
| `template-path`   | Custom `README.md` template path [Learn more](#template-path)  | `template/template.md`                     |
| `output-path`     | Output filename                                                | `README.md`                                |

#### `api-token`

A personal API access token is required to fetch starred repositories through the API without triggering rate limits.

You need to generate a [personal API token](https://github.com/settings/tokens/new) and add it under your repository's `Settings -> Secrets and variables -> Actions -> Secrets -> Repository secrets`.

Note: To commit the final `README.md` to your repository, you must also enable `Read and write permissions` under `Settings -> Code and automation -> Actions -> General -> Actions permissions -> Workflow permissions`.

#### `template-path`

The default template path is `template/template.md`. You can customize the template path (relative to repository root).

The generated content will replace the `[[GENERATE HERE]]` placeholder in the template while preserving other content.

#### `output-path`

Default output filename is `README.md`. You can customize the output path (relative to repository root).

## Example

```yml
name: Stargazer

on:
  push:
    branches:
      - main
  workflow_dispatch:
  schedule:
    - cron: "0 0 * * *"

jobs:
  stargazer:
    runs-on: ubuntu-latest
    steps:
      - name: Stargazer
        uses: zhuozhiyongde/gaze-stars@v1.1.0
        with:
          api-token: ${{ secrets.API_TOKEN }}
          github-username: ${{ github.repository_owner }}
          git-name: Github Action
          git-email: actions@users.noreply.github.com
          git-message: 'chore(updates): updated entries in files'
          sort-by: stars
          template-path: template/template.md
          output-path: README.md
```

## License

[MIT](LICENSE)
