# PDF Slide Knowledge Summary

中文 | [English](README.en.md)

从 PDF 版课程幻灯片中创建、扩展、检查考试导向知识总结的 skill，适合由 PowerPoint 导出的中文大学课程 PDF。经测试 Claude Code 环境下也可用。

## 功能

- 生成带页码引用的 Markdown 复习笔记
- 中文为主，重要概念补充英文术语
- 按章节整理考点、公式、概念、例题、易错点和考试技巧
- 两-tier 公式策略：人工校验公式用行内 LaTeX，OCR 公式以截图兜底
- 结构化验证循环：覆盖范围、页码引用、公式正确性、Markdown 渲染
- 保持课程目录清洁，根目录仅保留 PDF、说明书和最终汇总
- 支持 PPTX 输入（通过转换 PDF 提取文本）

## 文件结构

```text
.
├── README.md
├── README.en.md
└── skills/
    └── pdf-slide-knowledge-summary/
        ├── SKILL.md              # 技能入口（必读）
        ├── agents/
        │   └── openai.yaml
        └── docs/
            └── agent/
                ├── formulas.md      # 公式检测、OCR、LaTeX 校验规则
                ├── verification.md  # 验证循环与审计清单
                ├── workflows.md     # 新 PDF 追加、提取模式、并行策略
                └── history.md       # 历史审计迭代经验
```

## 环境说明

部分技术约束针对 **Windows 11 + Python 3.14** 环境制定：

- `python` 而非 `python3`（Windows 上 `python3` 可能是 Microsoft Store 占位程序）
- 终端默认 GBK 编码 → 必须写 UTF-8 文件，禁止 `print()` PDF 中文文本到终端
- `pypdf` 在 Python 3.14 / Windows 上兼容性优于 `pdfplumber`
- 公式 OCR 流水线需 Conda Python 3.12（pix2tex C 扩展在 3.14 上编译失败）

其他约束（目录清洁、公式策略、验证流程）适用于所有平台。

## 使用方式

### cc switch

在 cc switch 中添加自定义仓库时，使用：

```text
Repository: TaNgent-Owl/pdf-slide-knowledge-summary
Branch: main
Subdirectory: skills
```

这里必须填写 `Subdirectory: skills`。本仓库按"一个仓库可包含多个 skill"的结构组织，实际 skill 位于 `skills/pdf-slide-knowledge-summary/`。

### 手动安装

也可以将 `skills/pdf-slide-knowledge-summary/` 复制到 skills 目录中，然后在总结或审核 PDF 幻灯片时调用 `pdf-slide-knowledge-summary`。

示例提示词：

```text
使用 pdf-slide-knowledge-summary skill，将这些课程 PDF 总结成考试导向、带页码引用的 Markdown 知识汇总。
```

## 适用范围

面向课程幻灯片 PDF 和复习笔记整理。不适合通用论文摘要、作业求解或生成演示文稿。

## 来源说明

`SKILL.md` 的最初版本改写自以下仓库：

https://github.com/Li-Baichuan-James/summarize-slides-skill

本仓库在此基础上加入了 PDF 课程幻灯片总结的实战经验，包括 Windows UTF-8 提取注意事项、公式 OCR 局限与 LaTeX 校验、Markdown 表格安全检查，以及迭代式审核规则。

初始改写之后的更改均为 AI 生成。
