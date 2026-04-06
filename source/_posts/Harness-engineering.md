---
title: Harness Engineering
date: 2026-03-29 08:40:19
tags: 
- Ai
- 学习笔记
- Agent
- Harness
categories: 
- Ai
excerpt: Harness是一种Ai Agent设计模式或者思路，通过规划、上下文管理以及校验检查机制让Ai Coding Agent可用运行长时间（long-running）任务的目标，来提升交付的确定性。
---

Harness原意是指驾驭马的挽具，在ai领域，被用来代指控制（类比：驾驭）AI大模型（类比：马）的一种工程管理思路，通过多Agent、规划、上下文管理以及校验检查机制让Ai Agent可用运行长时间（long-running）任务，来提升交付的确定性。

> Anthropic的工程团队通过Harness模式，显著提升了Claude在long-running类型的软件工程和前端设计的表现。他们通过引入受生成对抗网络（GANs：Generative Adversarial Networks）启发的多智能体架构，将任务拆解为规划者、生成者与评估者三个角色，有效解决了模型在复杂任务中容易出现的上下文焦虑和自我评价不准等问题。通过对比实验，这种多智能体协作模式在构建全栈应用（如游戏制作器、数字音频工作站）时，展现出了远超单模型生成的逻辑连贯性与视觉精致度。Anthropic的工程人员强调：随着模型能力的演进，开发者应不断精简框架，将精力投入到挖掘模型原生能力与新型协作模式的结合上。
> 
> -----《Harness design for long-running application development》

OpenAI的工程师同样发布了一篇关于Harness的博客《Harness Engineering: Leveraging Codex in an Agent-First World》。文中探讨了OpenAI内部团队如何通过“Harness Engineering”在5个月内由3名工程师通过AI Agent完成了一个100w行代码，合并了约1500个Pull Request，没有人工参与一行代码的编写。同时文中，也更具体的叙述了“Harness Engineering”的核心部分。
# Harness Engineering 核心要点
## 提升应用系统的可读性
应用系统的可读性，可以理解为通过增加AI Agent的能力（Skill），让应用研发相关联的文件、系统对AI Agent可见。人类在开发过程中，不只是在写代码，会打开浏览器进行端到端的测试；还会查看服务器日志，进行代码执行过程的跟进以及问题的排查。所以，如果期望AI Agent能干得更好，同样也要给予它具备独自验证、纠错的能力，就像人类做的那样。

## 使用代码仓库作为记录系统
AI Agent的调优，最核心的部分就是上下文工程的调优。OpenAI团队早些时候尝试设计一个大而全的`AGENT.md`，后来发现这是一个错误的方案，因为这样很容易会导致大模型的上下文大小超过限制，从而导致关键信息的丢失。现在，Open AI的团队将`AGENT.md`设计为一个**文件的索引目录**，只存有项目工程中的规则（rule）、规约（spec）等其他文档的路径地址，通过类似`Skill`的渐进式披露的方式，来控制上下文的大小。

## 提升Ai Agent的可读性
这里的可读性，只是为Ai Agent提供更多的“知识”，比如：会议记录，IM（钉钉、飞书、微信等）的聊天记录，外部文档（语雀、钉钉文档、飞书、Word等）等对于应用开发、设计、测试等有影响的知识，还包括开发人员之间口口相传或者沉睡在大脑深处的知识。将这些知识转化为Agent可感知、验证、校对的形式，将提升Agent的“杠杆效应”。
>“杠杆效应”（Leverage）是一个管理与工程学概念。它指的是通过投入较少的资源（时间、算力、人类干预），通过特定的机制获得远超常规的产出效果。Agent通过**反馈循环（Feedback Loop）**，实现`检查 -> 验证 -> 修改 -> 重构 -> 再次验证`的闭环，将人从“繁琐的执行任务”中解放出来。

## 架构规范
Agent在架构边界清晰、可验证的系统中，交付最为高效。在OpenAI的团队，定义了一个严格的分层架构：`Types → Config → Repo → Service → Runtime → UI`，且每层只能向下依赖。*注：这个架构不是万能的，只适用于TypeScript或者某些特定的应用开发。* 另外，架构设计，是可以被自定义的lint规则识别的，并且可以告知Agent正确的修复路径，来实现自我闭环。
同时，对于边界、规则以外的内容，可用让Agent自由实现，比如：Agent可用自由选择某一个具体组件来实现某一个功能，文中提到了，Agent偏好使用**Zod**（一个TypeScript库）来实现类型的声明与校验。

## 代码合并方式的变化
随着Agent的能力提升，代码的变更变得更加的高效。传统工程的代码合并的规范、规则的拦截点，人工代码CR的方式将成为整个流程的阻塞点、短板、瓶颈。可用通过Agent来审核Agent写的代码，并对审核问题进行修复。

## Agent制品
Agent不只是生成代码，还包括：测试代码、CI配置&发布工具、设计文档以及Change List、代码仓库的脚本、CR的评论&回复等。
人的职责将发生变化，承担更高层次的抽象、设计，讲用户的需求转变为可量化、执行的case。当Agent出现问题的时候，我们需要识别它是缺失的工具、规约还是文档，并在代码仓库中进行补充、完善。

## 不断提升的自主水平
当越来越多的研发节点被Agent取代后，Agent可用独自实现一个新的功能，包括不限制于：验证代码、复现bug、录制修复视频、执行修复、验证修复、提交Pull Request、合并变更等；

## 熵增与治理
完全自主的Agent也会产生新的问题。Agent会复制现有代码库中的实现方式，哪怕是一个错误、糟糕的实现，导致系统熵增变的越来越多。
Open AI的团队的解决方式是：将治理规则代码化，使系统具备“垃圾回收”的能力，并定期扫描质量等级，发起对应的重构变更。比如：他们会使用共享代码库，而不是手动编写工具类代码（StringUtils、DateUtils等），以便于代码收敛。

# 参考
- [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) by Engineering at Anthropic
- [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) by Ryan Lopopolo
- [Harness Engineering学习指南](https://github.com/deusyu/harness-engineering/tree/main) - by deusyu
- [Harness engineering is the next big thing, so I started a newsletter about it](https://www.reddit.com/r/ClaudeCode/comments/1s3b46t/harness_engineering_is_the_next_big_thing_so_i) - by paulcaplan
- [everything is a ralph loop](https://ghuntley.com/loop/) by Geoffrey Huntley