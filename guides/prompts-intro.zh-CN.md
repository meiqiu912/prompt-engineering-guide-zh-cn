# 提示词入门（中文译本）

> 原文：[Prompting Introduction](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-intro.md)，上游 MIT 许可。本文件为中文译文，示例输出沿用原文。

提示词工程（Prompt Engineering）是一门较新的学科，研究如何开发和优化提示词，从而在各类应用和研究场景中更高效地使用语言模型（LM）。掌握提示词技巧，能帮你更好地理解大语言模型（LLM）的能力边界与局限。研究者用提示词工程提升 LLM 在问答、算术推理等常见和复杂任务上的表现；开发者用它设计稳健、有效的提示方法，让 LLM 和其他工具配合完成任务。

本篇讲标准提示词的基础用法，让你先大致知道怎么用提示词和大模型对话、给模型下指令。

除特别说明外，原文示例均在 `text-davinci-003`（OpenAI Playground）上测试，使用默认参数，例如 `temperature=0.7`、`top-p=1`。

目录：
- [基础提示词](#基础提示词)
- [关于 LLM 参数设置](#关于-llm-参数设置)
- [标准提示词格式](#标准提示词格式)
- [提示词的组成要素](#提示词的组成要素)
- [设计提示词的通用建议](#设计提示词的通用建议)

---

## 基础提示词

只用提示词就能做到不少事，但结果好坏取决于你给了多少信息。一条提示词可以包含传给模型的指令或问题，也可以带上输入、示例等其他细节。

先看一个最简单的例子。

*提示词*
```
The sky is
```

*输出：*
```
blue

The sky is blue on a clear day. On a cloudy day, the sky may be gray or white.
```

可以看到，模型只是根据上下文「The sky is」续了一段通顺的字符串。这种输出可能完全不是你想要的任务结果。

这个简单例子也说明：想要特定结果，必须给出更多上下文或更明确的指令。

我们改进一下：

*提示词：*
```
Complete the sentence: 

The sky is
```

*输出：*
```
 so  beautiful today.
```

这算更好吗？我们明确要求模型「补全句子」，结果就更贴合指令（「补全句子」）。这种为完成任务而设计最优提示词的方法，就是**提示词工程**。

上面只是今天 LLM 能力的一个基础例子。如今的 LLM 能完成从文本摘要、数学推理到代码生成等各种高级任务。

---

## 关于 LLM 参数设置

使用提示词时，你会通过 API 或网页界面和 LLM 打交道。有几个参数可以调整，从而得到不同结果。

**Temperature（温度）**：简单说，温度越低，结果越确定——总是选概率最高的下一个词。调高温度会增加随机性，鼓励更多样、更有创意的输出，相当于提高了其他候选词的权重。应用上，事实型问答适合用较低温度，让回答更事实、更简洁；诗歌等创意任务可以调高温度。

**Top_p**：这是一种带温度的采样技巧，叫核采样（nucleus sampling），同样控制生成的确定性。要精确、事实性的答案就调低；想要更多样的回答就调高。

一般建议：两个参数只调一个，不要同时动。

另外请注意：结果会随你使用的 LLM 版本而变化。

---

## 标准提示词格式

上面试了一条非常简单的提示词。标准提示词的格式如下：

```
<Question>?
```

很多问答数据集里用的是 QA 格式：

```
Q: <Question>?
A: 
```

在标准格式之上，有一个常用且有效的技巧叫**少样本提示（few-shot prompting）**，就是在提示词里给出示范例子。少样本提示词可以写成：

```
<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
<Answer>

<Question>?

```

对应的 QA 格式版本：

```
Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A:
```

注意：不一定要用 QA 格式，格式取决于手头任务。比如做一个简单分类任务，可以这样给示范：

*提示词：*
```
This is awesome! // Positive
This is bad! // Negative
Wow that movie was rad! // Positive
What a horrible show! //
```

*输出：*
```
Negative
```

少样本提示让模型具备上下文学习（in-context learning）能力：只给几个例子，模型就能学会任务。后续指南会看到更多实际用法。

---

## 提示词的组成要素

随着接触的例子和应用越来越多，你会发现一条提示词通常由若干要素组成。

一条提示词可以包含以下任意要素：

**指令（Instruction）**：你希望模型执行的具体任务或指令。

**上下文（Context）**：外部信息或补充背景，用来引导模型给出更好的回答。

**输入数据（Input Data）**：我们想要获取回答所针对的输入或问题。

**输出指示（Output Indicator）**：指明输出的类型或格式。

并非所有要素都必须出现，格式取决于任务。后续指南会给出更具体的例子。

---

## 设计提示词的通用建议

设计提示词时，记住下面这些建议。

### 从简单开始

刚开始写提示词时要记住：这是一个反复迭代的过程，需要大量试错才能拿到最优结果。可以先从 OpenAI、Cohere 这类 Playground 入手。

你可以先写简单提示词，再逐步补充要素和上下文来逼近更好的结果。因此给提示词留版本记录非常重要。读完整篇指南你会看到很多例子：具体、简单、简洁，往往比堆砌细节效果更好。

遇到包含多个子任务的大任务，可以先拆成更小的子任务，逐步叠加，避免一开始就把提示词设计搞得过于复杂。

### 写清指令

用命令式指令告诉模型你要做什么，比如「写」「分类」「总结」「翻译」「排序」等，就能为很多简单任务写出有效提示词。

同时要多试：不同的关键词、上下文、数据组合，看哪种最适合你的具体场景。通常上下文和任务越相关、越具体，效果越好。后续指南会讲到采样和补充上下文的重要性。

另一种做法是把指令放在提示词开头，并建议用 `###` 之类的清晰分隔符把指令和上下文分开。

例如：

*提示词：*
```
### Instruction ###
Translate the text below to Spanish:

Text: "hello!"
```

*输出：*
```
¡Hola!
```

### 保持具体

指令和任务要写得非常具体、明确。提示词越有描述性、越细致，结果越好；在你有期望的产出风格时尤其重要。不存在某个特定词或关键词能带来更好结果，关键是格式合理、描述清楚。在提示词里给例子，是拿到指定格式输出的有效办法。

设计时还要注意提示词长度有上限，想清楚需要多具体、多细致。不必要的细节堆上去未必是好事，细节应当和任务相关、能服务于任务本身。这一块需要多试多迭代。

举个例子，从一段文字里提取地点信息：

*提示词：*
```
Extract the name of places in the following text. 

Desired format:
Place: <comma_separated_list_of_places>

Input: "Although these developments are encouraging to researchers, much is still a mystery. “We often have a black box between the brain and the effect we see in the periphery,” says Henrique Veiga-Fernandes, a neuroimmunologist at the Champalimaud Centre for the Unknown in Lisbon. “If we want to use it in the therapeutic context, we need to understand the mechanism."
```

*输出：*
```
Place: Champalimaud Centre for the Unknown, Lisbon
```

输入文字来自 [Nature 这篇文章](https://www.nature.com/articles/d41586-023-00509-z)。

### 避免含糊

前面讲了要细致、要注意格式，但也容易掉进另一个坑：提示词写得过于取巧，描述反而不精确。通常更直接、更具体的表达效果更好。这和有效沟通很像：越直接，信息传达越到位。

比如你想了解提示词工程这个概念，可能会写：

```
Explain the concept of prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
```

这条提示词没说清用几句话、什么风格。虽然也能拿到还不错的回答，但更好的提示词应该非常具体、简洁、切题，比如：

```
Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
```

### 说「要做什么」，而不是「不要做什么」

设计提示词的另一条常见建议：尽量描述要做的事，而不是罗列禁止项。这样更具体，也更能把模型注意力聚焦到能带来好结果的细节上。

下面是一个电影推荐聊天机器人的例子。因为指令只强调「不要做什么」，模型还是踩了雷。

*提示词：*
```
The following is an agent that recommends movies to a customer. DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.

Customer: Please recommend a movie based on my interests.
Agent: 
```

*输出：*
```
Sure, I can recommend a movie based on your interests. What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
```

更好的提示词这样写：

*提示词：*
```
The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

Customer: Please recommend a movie based on my interests.
Agent:
```

*输出：*
```
Sorry, I don't have any information about your interests. However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
```

上面部分例子改编自 OpenAI 官方文章《[Best practices for prompt engineering with OpenAI API](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)》。

---

[下一节：基础提示方法（中文译本）](./prompts-basic-usage.zh-CN.md)
