# 🌟 gaze-stars

[English](README.md) | [中文版](README.zh-CN.md)

这个 GitHub Action 可以通过查询 GitHub API 以获取你标星的仓库们，然后按星标列表排序生成 README，生成的内容会在开头自动加入目录，方便快速跳转到任意章节。

你可以参考我的仓库 [zhuozhiyongde / Stargazer](https://github.com/zhuozhiyongde/Stargazer)

## 文档

### 前置要求

-   一个空仓库
-   一个个人 GitHub API 密钥

### 配置方法

| Variable          | Description                                                    | Default                                    |
| ----------------- | -------------------------------------------------------------- | ------------------------------------------ |
| `api-token`       | 个人 GitHub API 密钥，用于避免速率限制，[了解更多](#api-token) | /                                          |
| `github-username` | 生成星标列表的 GitHub 用户名                                   | /                                          |
| `git-name`        | 用于 Git 提交的名称                                            | `Github Action`                            |
| `git-email`       | 用于 Git 提交的邮箱                                            | `actions@users.noreply.github.com`         |
| `git-message`     | 用于 Git 提交的提交信息                                        | `chore(updates): updated entries in files` |
| `sort-by`         | 排序方式，`stars` 或 `updated`                                  | `stars`                                    |
| `template-path`   | 自定义 `README.md` 模板，[了解更多](#template-path)            | `template/template.md`                     |
| `output-path`     | 输出文件名                                                     | `README.md`                                |

#### `api-token`

个人 API 访问令牌是从 API 获取星标而不触发速率限制的必需项。

您需要生成一个 [个人 API 令牌](https://github.com/settings/tokens/new)，然后在仓库的 `Settings -> Secrets and variables -> Actions -> Secrets -> Repository secrets` 中添加。

请注意，由于需要提交最终的 `README.md` 到你的仓库，你还需要在 `Settings -> Security -> Actions -> General -> Actions permissions -> Workflow permissions` 中启用 `Read and write permissions` 权限。

#### `template-path`

默认模板路径为 `template/template.md`，您可以自定义模板路径，模板路径为相对路径，相对于仓库根目录。

生成 README 时，生成的内容会替换模板中的 `[[GENERATE HERE]]` 字样，对于其他部分不会有变动，所以你可以根据需要自定义模板。

#### `output-path`

默认输出文件名 `README.md`，您可以自定义输出文件名，输出文件名相对于仓库根目录。

## 示例

```yml
name: Stargazer

on:
    push:
        branches:
            - main
    workflow_dispatch:
    schedule:
        - cron: '0 0 * * *'

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

## 许可证

[MIT](LICENSE)
