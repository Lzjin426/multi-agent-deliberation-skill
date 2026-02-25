# Gate And Controls

## 1) Activation Gate Checklist

在启动多 agent 辩论前，逐项判定：

1. 任务是否存在明确争议点或多种可行路径？
2. 是否可以定义可检验的胜负标准？
3. 关键结论是否可被证据反驳或验证？

满足 3/3 才开启辩论。否则默认单 agent。

## 2) Budget Controls

1. 默认轮次：2。
2. 最大轮次：3。
3. 每角色输出长度上限：按任务设定，避免篇幅偏置。
4. 总预算超限时：提前终止并输出“当前最佳+风险提示”。

## 3) Stop Rules

1. 连续两轮评分增益 <1%。
2. 没有新增证据。
3. 分歧集中于价值取向而非事实，可转人工决策。

## 4) Failure Modes And Mitigations

1. 群体偏差放大:
- Mitigation: 保持 A/B 独立初稿；引入独立 checker；强制反例位。

2. 角色塌缩:
- Mitigation: 强化角色差异提示；限制互看时机。

3. 一致但错误:
- Mitigation: Judge 不以一致性为证据，必须看证据映射。

4. 空转辩论:
- Mitigation: 强制轮次和停机阈值。

## 5) High-Stakes Escalation

在医疗、法律、金融等高风险场景：

1. 强制 human-in-the-loop 审核。
2. 允许输出“证据不足，不发布结论”。
3. 明确给出下一步验证计划。
