# MkDocs Material 自动根据目录生成导航

## 目标

让 MkDocs 自动扫描 `docs/` 目录中的 Markdown 文件并生成导航，这样以后新增文章时，不需要每次手动修改 `mkdocs.yml` 里的 `nav:`。

---

## 1. 删除 `mkdocs.yml` 中手动配置的 `nav`

进入项目目录：

```bash
cd /home/cmq/workspace/projects/notes
```

建议先备份：

```bash
cp mkdocs.yml mkdocs.yml.backup
```

如果 `nav:` 位于文件最后，可以执行：

```bash
sed -i '/^nav:/,$d' mkdocs.yml
```

检查：

```bash
tail -n 15 mkdocs.yml
```

确认已经没有类似下面的内容：

```yaml
nav:
  - 首页: index.md
```

MkDocs 在没有手动 `nav:` 时，会自动扫描 `docs/` 目录中的 Markdown 文件并生成导航。

---

## 2. 按目录分类文章

推荐结构：

```text
docs/
├── index.md
├── linux/
│   ├── index.md
│   ├── ssh.md
│   └── zsh.md
├── docker/
│   ├── index.md
│   └── compose.md
└── python/
    ├── index.md
    └── venv.md
```

创建分类目录：

```bash
mkdir -p docs/linux docs/docker docs/python
```

---

## 3. 新增文章

例如创建 Zsh 笔记：

```bash
nano docs/linux/zsh.md
```

内容示例：

```markdown
# Zsh

这里写 Zsh 相关笔记。
```

创建 SSH 笔记：

```bash
nano docs/linux/ssh.md
```

内容示例：

```markdown
# SSH

这里写 SSH 相关笔记。
```

创建 Docker Compose 笔记：

```bash
nano docs/docker/compose.md
```

内容示例：

```markdown
# Docker Compose

这里写 Docker Compose 相关笔记。
```

以后新增文章时，只需要在合适的目录中新建 `.md` 文件即可。

---

## 4. 每个分类建议创建 `index.md`

例如：

```bash
nano docs/linux/index.md
```

内容：

```markdown
# Linux

这里记录我的 Linux 相关笔记。
```

这样进入 `Linux` 分类时，会有一个分类首页。

同理：

```bash
nano docs/docker/index.md
nano docs/python/index.md
```

---

## 5. 本地预览

启用虚拟环境：

```bash
cd /home/cmq/workspace/projects/notes
source .venv/bin/activate
```

启动 MkDocs：

```bash
mkdocs serve
```

如果需要从局域网其他电脑访问：

```bash
mkdocs serve -a 0.0.0.0:8000
```

---

## 6. 更新 GitHub Pages

确认本地显示正常后：

```bash
git add .
git commit -m "更新笔记"
git push
```

GitHub Actions 会自动重新构建并部署网站。

网站地址：

```text
https://inibd.github.io/notes/
```

---

## 7. 以后新增文章的固定流程

```text
创建或修改 .md
        ↓
本地 mkdocs serve 预览
        ↓
git add .
        ↓
git commit -m "更新笔记"
        ↓
git push
        ↓
GitHub Pages 自动更新
```

---

## 8. 注意事项

### 自动导航的排序

MkDocs 默认会按照文件名/目录名自动生成导航，因此可以通过文件名控制一定程度的顺序。

如果以后需要：

- 自定义目录显示名称
- 自定义排序
- 隐藏某些页面
- 更灵活地控制导航

可以考虑安装 `mkdocs-awesome-nav` 插件。

### 文件名和标题

文件名：

```text
docs/linux/zsh.md
```

页面标题建议在文件第一行写：

```markdown
# Zsh
```

文件名决定路径，Markdown 一级标题决定页面显示标题。
