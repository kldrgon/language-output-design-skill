# 测试方式

本目录用人工评测检查 `language-output-design` Skill 是否改变了 Agent 的输出文本。

测试对象是装载 Skill 后的回答，不是脚本、断言或标准答案。每份 prompt 只提供输入情境；评测者对照 Skill 的原则阅读输出。

## 操作步骤

1. 按仓库根目录 `README.md` 把 Skill 装到当前 Agent 环境。需要 Rule 时一并装上。
2. 新开一次对话，确认 Agent 已装载该 Skill。
3. 打开 `prompts/` 中的一份文件，把 **输入** 整段作为用户消息发给 Agent。不要把 **评测目标** 一起发给 Agent。
4. 阅读 Agent 产出的文本，对照该文件的评测目标和下方共用标准判断是否合格。

一次对话只测一份输入。换下一份时重新开对话，避免上文污染。

## 共用评测标准

先看输出是否帮助当前受众完成行动、判断、理解、记忆或查阅。再核对以下各项。

- 开头是否提供能组织后文的认知支点，而不是固定先给结论或先讲原理
- 主线是否先闭合，支线是否在主线之后展开并返回
- 会改变结论、使操作无效或带来风险的限制是否与相关结论或动作同时出现
- 连续因果用段落，独立核对项用列表，共同维度的横向比较用表格
- 示例是否展示机制、边界或步骤，而不是只给表面形式
- 是否用具体机制、条件、证据或结果代替空泛总结和同义改写
- 是否把“不是 X，而是 Y”当成默认句式

不合格时记下失败点对应的输入文件和现象。不要为了对齐某一份正反例而要求输出套用固定模板。

## 输入文件

| 文件 | 主要检查 |
| --- | --- |
| [prompts/01-concept-explanation.md](prompts/01-concept-explanation.md) | 概念解释是否建立关系与状态变化 |
| [prompts/02-procedure-ordinary.md](prompts/02-procedure-ordinary.md) | 操作步骤是否按执行顺序写清动作、条件和可观察结果 |
| [prompts/03-procedure-safety.md](prompts/03-procedure-safety.md) | 不可逆风险是否出现在删除动作之前 |
| [prompts/04-diagnostics.md](prompts/04-diagnostics.md) | 是否区分事实、推断和待验证项 |
| [prompts/05-decision.md](prompts/05-decision.md) | 是否按决定性条件给建议，而不是平均罗列优缺点 |
| [prompts/06-reference.md](prompts/06-reference.md) | 查阅类内容是否便于定位和比较 |
| [prompts/07-rewriting.md](prompts/07-rewriting.md) | 改写是否去掉空泛模式并保住有效信息 |
| [prompts/08-table-comparison.md](prompts/08-table-comparison.md) | 共同维度的数值比较是否使用表格，并说明差异含义 |
| [prompts/09-mixed-genres.md](prompts/09-mixed-genres.md) | 同一主题下文章、口语、通知是否分开且语体匹配 |
| [prompts/10-open-topic-set.md](prompts/10-open-topic-set.md) | 开放主题是否按教学、对话、操作分别组织 |
| [prompts/11-spoken-and-written.md](prompts/11-spoken-and-written.md) | 书面稿与口播、问答的信息和语体是否一致且可区分 |
