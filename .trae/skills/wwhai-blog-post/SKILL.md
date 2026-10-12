---
name: wwhai-blog-post
description: Scaffold new Chinese blog posts for this Hexo site with standard front matter, summary line, and body skeleton. Use when asked to write, create, or scaffold a post under source/_posts/wwhai. Do not use for editing existing posts.
---

# 博客框架生成

为本站 `source/_posts/wwhai/` 下的中文博客快速生成规范的文档头与正文骨架。目标是让用户给一个标题或一段草稿，就能立刻得到一篇可直接渲染的文章框架。

## 一、文件位置与命名

- 默认目录：`source/_posts/wwhai/杂文集-踉跄/`
- 文件名：`YYYYMMDD标题.md`（如 `20261012公堂游戏.md`）。标题若较长可加连字符，如 `20250720-雨夜.md`。
- 图片目录约定：`source/uploads/<年份>/<英文短名>/`，正文引用写作 `/uploads/<年份>/<英文短名>/1.png`。

## 二、文档头模板（必须逐字对齐格式）

```yaml
---
title: '文章标题'
date:  2026-10-12 08:00:00
index_img: /uploads/2026/slug/1.png
tags:
- 文集-踉跄与世

categories:
- 文集-踉跄于世

author: wangwenhai
---
本文作者：[wangwenhai] # 概要：一句话概要。
<!-- more -->
```

规则：

- 键顺序固定：`title` / `date` / `index_img` / `tags` / `categories` / `author`。
- `date:` 后是两个空格再写时间，与仓库既有文件保持一致。
- `tags` 与 `categories` 之间必须空一行；列表项写在第 0 列（`- ` 顶格，不缩进）。
- 标题含冒号、引号等特殊符号时用单引号包裹；纯中文短标题可不加引号。
- 无配图时 `index_img` 用 `/static/<N>.jpg`（仓库已有 1–21 系列）。
- front matter 之后固定两行：作者概要行 + `<!-- more -->`，再空一行开始正文。
- 若原文已有 front matter，只补齐缺失字段，不覆盖已有内容。

## 三、归类选择

按内容体裁挑一组，写入 `tags` 与 `categories`（两者通常同值）：

| 内容类型 | tags / categories |
| --- | --- |
| 感悟、经历、回忆、记事（默认） | `文集-踉跄与世` / `文集-踉跄于世` |
| 纯文学散文、短篇 | `文集-原创散文` / `文集-原创散文` |
| 议论、随笔杂谈 | `杂文` / `杂文` |
| 职场、技术路线反思 | tags 写 `杂文` 与 `职场` 两项，categories 写 `职场` |

其他作者代笔时，`author` 改为真实作者名（如 `liubin`）。

## 四、正文骨架（按体裁选一种）

**A. 短篇散文（默认）** —— 正文整体包在蓝色放大容器里：

```html
<div style="font-size:150%; color: blue;">

第一段……

第二段……

</div>
```

**B. 长文带小标题** —— 适合经历、年度总结：

```markdown
# 文章标题

开篇一段，点明时间与缘由。

## 一、小节名

## 二、小节名
```

**C. 诗歌 / 顺口溜** —— 居中、加粗、逐行：

```html
<div style="text-align:center;font-size:150%; color: blue;">

第一句
<br>
第二句

</div>
```

**D. 配图** —— 正文内插图用其中一种：

```html
<img src="/uploads/2026/slug/1.png" alt="1.png" />
```

```markdown
![说明文字](/uploads/2026/slug/2.jpg)
```

结构上的习惯：结尾可用 `---` 分隔后接一句点题短句（如 `**希望我们现在、未来，都好。**`）。

## 五、生成流程

1. 先确认三件事：标题、体裁（默认散文）、归类（默认 `文集-踉跄与世`）。用户未说明时按默认走，不要反问。
2. 用当天日期生成 `date` 与文件名；据标题拟定英文短名作为图片目录 slug。
3. 套用第二节模板输出文档头，再按第四节选对应骨架。
4. **只填充用户提供的素材。不得虚构事实、细节、对话或经历** —— 需要展开时先向用户确认，宁可留占位。占位用 `【待补充】` 标记。
5. 生成后按第六节自检，并向用户报告写入路径。

## 六、自检清单

- [ ] 文件落在正确目录，命名为 `YYYYMMDD标题.md`
- [ ] front matter 六项齐全、顺序正确，`date:` 后两个空格
- [ ] `tags` 与 `categories` 之间有空行，列表项顶格
- [ ] 作者概要行与 `<!-- more -->` 存在
- [ ] 正文骨架与体裁匹配，标签闭合（`</div>`、`<br>`）
- [ ] 没有编造用户未提供的事实
