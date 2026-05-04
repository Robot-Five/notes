# MkDocs + GitHub Pages

## 问题

如何把本地 Markdown 笔记仓库发布成 GitHub Pages，并且让后续改动可以自动构建和部署。

## 结论

- GitHub Pages 的发布源应当选择 `GitHub Actions`
- `MkDocs` 不能把 `docs_dir` 直接设成仓库根目录
- 更稳的做法是单独建一个 `docs/` 子目录
- 如果原始笔记不想迁移，可以在 `docs/` 下用符号链接接现有内容
- 首次启用 Pages 后，如果 `deploy` 返回 `404 Ensure GitHub Pages has been enabled`，通常重新触发一次 workflow 就够了

## 目录结构

当前做法：

- 仓库根目录保留真实内容
- `docs/` 作为 MkDocs 的构建入口
- `docs/index.md` 指向根目录 `README.md`
- `docs/topics` 指向 `topics/`
- `docs/algorithm` 指向 `algorithm/`
- `docs/archive` 指向 `archive/`
- `docs/assets` 指向 `assets/`

这样可以同时满足：

- 原始内容目录不需要大搬迁
- `MkDocs` 的构建目录合法
- GitHub Pages 能直接吃构建产物

## 关键配置

### `mkdocs.yml`

关键点：

- `docs_dir: docs`
- 首页用 `index.md`
- 导航显式列出主要入口

## GitHub Pages 设置

仓库设置里需要：

1. 仓库可见性允许启用 Pages
2. `Settings -> Pages`
3. `Build and deployment -> Source`
4. 选择 `GitHub Actions`

如果这里还是 `Deploy from a branch`，即使 workflow 存在，Pages 也不会按 Actions 方式接管部署。

## GitHub Actions 工作流

核心流程：

1. checkout 代码
2. 安装 Python
3. 安装 `mkdocs`
4. 执行 `mkdocs build`
5. 上传 `site/`
6. 使用 `deploy-pages` 发布

相关文件：

- `mkdocs.yml`
- `.github/workflows/pages.yml`
- `requirements.txt`

## 这次遇到的典型错误

### 1. `docs_dir` 配错

错误现象：

```text
The 'docs_dir' should not be the parent directory of the config file
```

原因：

- 把 `docs_dir` 设成了仓库根目录

修正：

- 改成单独的 `docs/`

### 2. `site_dir` 落在 `docs_dir` 里面

错误现象：

```text
The 'site_dir' should not be within the 'docs_dir'
```

原因：

- 当 `docs_dir` 是根目录时，默认 `site/` 也落在根目录里，导致构建目录被递归包含

修正：

- 同样是把 `docs_dir` 改成单独子目录

### 3. `deploy` 返回 404

错误现象：

```text
Failed to create deployment (status: 404)
Ensure GitHub Pages has been enabled
```

原因通常是：

- workflow 已经开始跑
- 但仓库的 Pages 还没切换到 `GitHub Actions`

修正：

- 在 `Settings -> Pages` 里切到 `GitHub Actions`
- 然后重新触发一次 workflow

## 最小使用流程

以后新增或修改笔记：

1. 改 Markdown
2. `git add`
3. `git commit`
4. `git push`
5. GitHub Actions 自动构建并部署

## 相关文件

- `README.md`
- `mkdocs.yml`
- `requirements.txt`
- `.github/workflows/pages.yml`
- `topics/tools/README.md`

