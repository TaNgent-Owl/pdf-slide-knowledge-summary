# PDF 幻灯片知识总结 Skill

中文 | [English](README.en.md)

这是一个 Codex skill，用于从 PDF 版课程幻灯片中创建、扩展和检查考试导向的知识总结，尤其适合由 PowerPoint 导出的中文大学课程 PDF。
> 经测试Claude Code环境下也可用

## 功能

这个 skill 重点支持：

- 生成带页码引用的 Markdown 复习笔记
- 中文为主，并为重要概念补充英文术语
- 按章节整理考点、公式、概念、例题、易错点和考试技巧
- 处理 PPT 导出 PDF 中常见的公式 OCR 低置信度问题
- 通过循环检查覆盖范围、页码引用、公式正确性和 Markdown 渲染
- 维护课程资料目录的清洁，避免临时文件污染根目录

## 文件结构

```text
.
├── README.md
├── README.en.md
└── skills/
    └── pdf-slide-knowledge-summary/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

## 使用方式

### cc switch

在 cc switch 中添加自定义仓库时，使用：

```text
Repository: TaNgent-Owl/pdf-slide-knowledge-summary
Branch: main
Subdirectory: skills
```

这里必须填写 `Subdirectory: skills`。本仓库按“一个仓库可包含多个 skill”的结构组织，实际 skill 位于 `skills/pdf-slide-knowledge-summary/`。

### 手动安装

也可以将 `skills/pdf-slide-knowledge-summary/` 复制到 Codex skills 目录中，然后在总结或审核 PDF 幻灯片知识总结时要求 Codex 使用 `pdf-slide-knowledge-summary`。

示例提示词：

```text
使用 pdf-slide-knowledge-summary skill，将这些课程 PDF 总结成考试导向、带页码引用的 Markdown 知识汇总。
```

## 适用范围

本 skill 面向课程幻灯片 PDF 和复习笔记整理。它不适合通用论文摘要、作业求解或生成演示文稿。

## 来源说明

`SKILL.md` 的最初版本来源于并改写自以下仓库，以适配本地课程总结流程：

https://github.com/Li-Baichuan-James/summarize-slides-skill

本仓库在此基础上加入了 PDF 课程幻灯片总结中的项目经验，包括 Windows 上 UTF-8 文本提取注意事项、公式 OCR 局限、Markdown 表格安全检查，以及迭代式总结审核规则。

初始改写之后，本仓库的后续更改均为 AI 生成。
