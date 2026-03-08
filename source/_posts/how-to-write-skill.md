---
title: How to write skill
date: 2026-03-07 13:54:00
tags: 
- Ai
- 学习笔记
categories: 
- Ai
excerpt: Skill是AI Agent的能力扩展机制，包含了prompt（包括指令、流程、知识库）、MCP的工具集。这里简单记录了如何去写一个好的skill。
---

Skill是Ai Agent的能力扩展机制，包含了prompt（包括指令、流程、知识库）、MCP的工具集。这里简单记录了如何去写一个好的skill。

>⚠ 不同的Ai Agent的skill扩展机制不一样，细节差别请参考官方文档；

# Skill结构
一个完整的的skill结构如下：
``` bash
skill-name/
├── SKILL.md                  # [必需] 入口文件：frontmatter + body
├── scripts/                  # [可选] 可执行脚本
├── references/               # [可选] 参考文档
```
一个简单skill的示例（只包含`explain-code/SKILL.md`文件）：
```
---
name: explain-code
description: Explains code with visual diagrams and analogies. Use when explaining how code works, teaching about a codebase, or when the user asks "how does this work?"
---

When explaining code, always include:

1. **Start with an analogy**: Compare the code to something from everyday life
2. **Draw a diagram**: Use ASCII art to show the flow, structure, or relationships
3. **Walk through the code**: Explain step-by-step what happens
4. **Highlight a gotcha**: What's a common mistake or misconception?

Keep explanations conversational. For complex concepts, use multiple analogies.
```

结构的简要说明：
- SKILL.md: skill的主文件，唯一的必选文件，包括YAML格式的Frontmatter和Markdown格式的主体内容。
	- Frontmatter：这部分包含名称`name`、描述`description`，被`---`收尾包含。这部分在每次对话过程中都会加载到上下文中，所以建议控制在100词内，其中`description`将会决定Ai Agent会不会匹配到Skill，所以这部分需要尽量明确，减少歧义；
	- 主体内容：Ai Agent调用Skill时，需要执行的指令、流程以及遵循的知识部分，这部分在Skill被调用时，才会被加载，建议控制在5k词以内；
- scripts：可执行的脚本，被“SKILL.md”调用执行的工具部分，可以是一段调用操作系统中的应用程序或者接口的脚本，也可以是一个调用远程服务的接口实现；只会被执行，不会读入，0 token成本；
- references：Skill的知识库参考文件，在“SKILL.md”中指定这部分包含的内容，以及何时加载它，比如:

# 如何去写一个好的Skill
## 上下文的控制
skill是给AI读取的，且读取的时候会占用有限的上下文空间，所以skill要尽可能的去控制文档内容的大小。
- ❎不要加入更新日志、作者信息等人类阅读的文档，也不要加入编写skill时的测试代码等；
- ✅SKILL.MD的frontmatter（name + description）控制在100字以内；
- ✅SKILL.MD的内容主体控制在5,000字以内；
- ✅有时可以选择用简洁的代码来代替复杂的文字解释；
## Ai自由度的控制
Ai大模型是基于海量数据训练出来的，从高端的量子物理到日常的烹饪食谱，所以在写Ai的prompt的时候，通常会用一句话来定义Ai的角色，这就是为了限制Ai的逻辑链路。所以，我们在写skill的时候，要约束Ai的边界，但是为了保障技能的灵活度，又要给与Ai一定的自由度。
- 高自由度：文字指令，比如：理解用户的需求、进行内容的创作；
- 中自由度：模板或者伪代码，比如：输出的格式模板；
- 低自由度：工具脚本，比如：初始化文件&目录、可用性/格式的校验；

**badcase**：
- ❎给一个限制要求的任务太多自由度

> -- skill 内容主体 -- 
>
> 请生成一个 openai.yaml 文件，包含 display_name 和 short_description。
>
> --- 结果 ---
>
> short_description 可能超过 64 字符限制，大小写可能不一致
- ❎给一个创造性的任务太多约束
> -- skill 内容主体 -- 
>
> 第一段必须以"昨天"开头，第二段必须包含"本质上"，最后一段以"慢慢来"结尾。
>
> --- 结果 ---
>
> 生成的文本像填词游戏

**判断标准**
- 不确定产生的问题严重性约大 -> 自由度低
- 实现方式越多 -> 自由度高

# 参考
- [如何写出好的Skill](https://github.com/datawhalechina/hello-agents/blob/main/Extra-Chapter/Extra08-%E5%A6%82%E4%BD%95%E5%86%99%E5%87%BA%E5%A5%BD%E7%9A%84Skill.md) - 从零开始构建智能体
- [使用 skills 扩展 Claude](https://code.claude.com/docs/zh-CN/skills) - Claude Code官方文档
