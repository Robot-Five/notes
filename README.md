# Notes

这个仓库用于维护长期笔记。

原则：

- 纯 Markdown 为主
- 图尽量用代码描述，放在 `assets/diagrams/`
- 每个主题一个目录，并维护自己的索引页
- 过时内容先归档，不直接丢
- 先保证可检索，再逐步优化结构

## 目录

- [topics/](./topics/README.md): 正在维护的主题
- [algorithm/](./algorithm/README.md): 已有算法笔记
- [archive/](./archive/README.md): 归档内容
- [assets/diagrams/](./assets/diagrams/README.md): `d2` 图文件

## 约定

1. 新主题放在 `topics/<topic>/`
2. 每个主题目录至少有一个 `README.md`
3. 文件名尽量直接描述问题，不要用 `note1.md` 这类名字
4. 图文件和正文分开存，正文里用相对路径引用
5. 大改前先提交 Git

## 初始主题建议

- `topics/linux/`
- `topics/robotics/`
- `topics/rl/`
- `topics/tools/`
