# 基础提示方法（中文译本）

> 原文：[Basic Prompting](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-basic-usage.md)，上游 MIT 许可。本文件为中文译文，示例输出沿用原文。

上一篇介绍了提示词并给了一个基础例子。本篇再给更多用法示例，并引入后续进阶指南会用到的关键概念。

学概念最好的方式就是看例子。下面看几个精心设计的提示词能完成哪些不同类型的任务。

目录：
- [文本摘要](#文本摘要)
- [信息抽取](#信息抽取)
- [问答](#问答)
- [文本分类](#文本分类)
- [对话](#对话)
- [代码生成](#代码生成)
- [推理](#推理)
- [Python 笔记本](#python-笔记本)

---

## 文本摘要

自然语言生成里的经典任务之一是文本摘要。摘要可以覆盖很多不同风格和领域。语言模型最有前景的应用之一，就是把文章和概念压缩成易读的简短总结。先用提示词试一个基础摘要任务。

比如我想了解抗生素，可以用这样的提示词：

*提示词：*
```
Explain antibiotics

A:
```

*输出：*
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.
```

这里的 `A:` 是问答场景常用的显式提示格式，用来告诉模型后面还有预期内容。这个例子里还看不出加不加它的差别，留到后面再讲。假设上面这段信息太多，想再压成一句话，可以这样指令模型：

*提示词：*
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.

Explain the above in one sentence:
```

*输出：*
```
Antibiotics are medications used to treat bacterial infections by either killing the bacteria or stopping them from reproducing, but they are not effective against viruses and overuse can lead to antibiotic resistance.
```

先不纠结上面输出的准确性（可靠性问题后续指南会讲），模型确实把段落压成了一句话。指令还可以写得更巧，那是后面章节的内容。可以先停下来自己多试几次，看能不能拿到更好的结果。

---

## 信息抽取

语言模型虽然是为自然语言生成及相关任务训练的，但做分类和其他 NLP 任务也很在行。

下面这条提示词从给定段落里抽取信息。

*提示词：*
```
Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.

Mention the large language model based product mentioned in the paragraph above:
```

*输出：*
```
The large language model based product mentioned in the paragraph above is ChatGPT.
```

结果还有很多可以优化的空间，但已经很实用了。

到这里应该能看出来：只要告诉模型做什么，就能让它执行不同任务。这正是 AI 产品开发者已经在用的强大能力。

段落来源：[ChatGPT: five priorities for research](https://www.nature.com/articles/d41586-023-00288-7)。

---

## 问答

想让模型给出针对性答案，最好的办法之一是改进提示词格式。前面讲过，提示词可以组合指令、上下文、输入和输出指示来改善结果。这些要素不是必须的，但指令越具体，结果通常越好。下面是一条结构更清晰的提示词示例。

*提示词：*
```
Answer the question based on the context below. Keep the answer short. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```

*输出：*
```
Mice.
```

上下文来源：[Nature](https://www.nature.com/articles/d41586-023-00400-x)。

---

## 文本分类

到目前为止，我们都在用简单指令完成任务。作为提示词工程师，你需要把指令写得越来越好。但不止于此：更难的场景里，光给指令不够，还要考虑上下文和提示词里能用到的其他要素，比如输入数据或示例。

先用文本分类演示一下。

*提示词：*
```
Classify the text into neutral, negative or positive. 

Text: I think the food was okay. 
Sentiment:
```

*输出：*
```
Neutral
```

我们给了分类指令，模型回了 `Neutral`，是对的。这本身没什么问题。但如果我们要求标签严格按指定格式返回——比如不要 `Neutral`，要小写 `neutral`——怎么做到？办法不止一种。这里关键是具体：能给提示词的信息越多，结果越好。可以给示例来规定正确行为，再试一次：

*提示词：*
```
Classify the text into neutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment: neutral 

Text: I think the food was okay. 
Sentiment:
```

*输出：*
```
neutral
```

完美。这次模型返回了想要的小写标签。看来提示词里的示例帮模型把输出写得更规范。再看一个例子，找出问题在哪：

*提示词：*
```
Classify the text into nutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment:
```

*输出：*
```
Neutral
```

问题出在哪？（提示：注意指令里的拼写。）

---

## 对话

提示词工程最有意思的用法之一，是规定 LLM 的行为方式、意图和身份。做客服聊天机器人这类对话系统时尤其有用。

比如我们建一个回答偏技术、偏科学风格的对话系统。注意我们是通过指令明确告诉它该怎么表现的，这有时叫**角色提示（role prompting）**。

*提示词：*
```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```

*输出：*
```
Sure! Black holes are regions of spacetime where the gravitational force is so strong that nothing, not even light, can escape from it. They are created when a very massive star dies and its core collapses in on itself, forming a singularity of infinite density. The intense gravity of the singularity pulls in all the matter and radiation around it, creating the black hole.
```

这个 AI 科研助手听起来是不是太技术了？好，我们改一下行为，让回答更通俗。

*提示词：*
```
The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI: 
```

*输出：*
```
Sure! Black holes are created when a very massive star runs out of fuel and collapses in on itself. The gravity of the star is so strong that nothing, not even light, can escape its pull. This creates a point in space-time with an intense gravitational force called a black hole.
```

算是有进步了。你可以继续改，再加几个示例，效果可能更好。

---

## 代码生成

LLM 很擅长的一类应用是代码生成，Copilot 就是典型例子。用好提示词可以完成大量代码任务，先看几个例子。

先写一个跟用户打招呼的简单程序。

*提示词：*
```
/*
Ask the user for their name and say "Hello"
*/
```

*输出：*
```
let name = prompt("What is your name?");
console.log(`Hello, ${name}!`);
```

可以看到，我们甚至没指定用什么语言。

再升一点难度，看看在提示词上多花点心思，LLM 能有多强。

*提示词：*
```
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```

*输出：*
```
SELECT StudentId, StudentName 
FROM students 
WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```

很能打。这里我们给了数据库表结构信息，让它生成一条合法的 MySQL 查询。

---

## 推理

当下 LLM 最难的任务之一需要某种推理能力。推理是我最看好的方向之一，因为 LLM 能由此长出各种复杂应用。

数学能力相关任务已有一些进步。但要注意：当前 LLM 做推理任务仍然吃力，需要更高级的提示技巧，下一篇指南会讲。这里先看几个基础算术例子。

*提示词：*
```
What is 9,000 * 9,000?
```

*输出：*
```
81,000,000
```

再试个难一点的。

*提示词：*
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

A: 
```

*输出：*
```
No, the odd numbers in this group add up to an odd number: 119.
```

答错了。我们改进提示词来修正。

*提示词：*
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even. 
```

*输出：*
```
Odd numbers: 15, 5, 13, 7, 1
Sum: 41 
41 is an odd number.
```

好多了。顺便说一句，我多试了几次，系统有时还是会错。如果把更好的指令和示例结合起来，结果可能更准。

本篇后续还会补充更多常见应用示例。后续指南会讲更高级的提示工程概念，用来在这些乃至更难的任务上提升表现。

---

## Python 笔记本

| 说明 | 笔记本 |
|--|--|
| 学习用 `openai` 和 `LangChain` 库完成各类常见任务 | [Getting Started with Prompt Engineering](../notebooks/pe-lecture.ipynb) |

---

[上一节：提示词入门（中文译本）](./prompts-intro.zh-CN.md)

[下一节：进阶提示技巧（原文，未翻译）](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/guides/prompts-advanced-usage.md)
