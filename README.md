# Prompt Engineering Guide 中文译本（zh-CN）

> **Upstream**: [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)
> **Original project**: Prompt-Engineering-Guide
> **Upstream author / account**: [DAIR.AI](https://github.com/dair-ai)（Elvis Saravia）
> **Upstream license**: MIT
>
> This repository is a Chinese localization (zh-CN) of the upstream guide. It is **not endorsed by or affiliated with** the upstream author. See [Differences from upstream](#-differences-from-upstream) below.

一句话：把经典的提示词工程指南翻成中文，另配一批国内模型能直接跑的场景提示词。

---

## 本仓与上游的差异（Differences from upstream）

- **翻译范围**：上游 README 介绍，以及《提示词入门》《基础提示方法》两篇指南完整译为中文；其余章节暂未翻译，保留上游原文链接。
- **中国场景补充**：[`extras/china-scenario-prompts.md`](./extras/china-scenario-prompts.md) 新增 10 条面向国内职场场景的提示词（写周报、改简历、写电商详情、改公文、做培训课件等），可在千问、DeepSeek、Kimi、豆包等国内模型上直接使用。
- **实测补充**：指南中的示例输出保留上游原文；本仓尚未把上游例子逐条放到国内模型上复测，引用时请注意这一点。
- **未改动部分**：上游 LICENSE 原文原样保留在根目录 [`LICENSE.md`](./LICENSE.md)；上游网页版、课程、代码笔记本等链接仍指向原项目。
- **命名说明**：仓库名 `prompt-engineering-guide-zh-cn`，description 标注 based on upstream；本仓不是上游首发，也不冒充官方中文版。

---

## 目录

### 已翻译

- [提示词入门（中文译本）](./guides/prompts-intro.zh-CN.md)
- [基础提示方法（中文译本）](./guides/prompts-basic-usage.zh-CN.md)

### 暂未翻译（原文见上游仓库）

- [Advanced Prompting（进阶提示）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-advanced-usage.md)
- [Applications（应用场景）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-applications.md)
- [ChatGPT 篇](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-chatgpt.md)
- [Adversarial Prompting（对抗提示）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-adversarial.md)
- [Reliability（可靠性）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-reliability.md)
- [Miscellaneous（杂项）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-miscellaneous.md)

### 补充内容

- [中国场景提示词 10 条](./extras/china-scenario-prompts.md)

---

## 引用上游

如果在工作或研究中参考了本指南，请按上游要求引用：

```
@article{Saravia_Prompt_Engineering_Guide_2022,
author = {Saravia, Elvis},
journal = {https://github.com/dair-ai/Prompt-Engineering-Guide},
month = {12},
title = {{Prompt Engineering Guide}},
year = {2022}
}
```

## 许可证

上游项目以 MIT License 发布，原许可证文件原样保留在本仓根目录 [`LICENSE.md`](./LICENSE.md)。
