# Pump 迁移到 DEX 的流程说明

## 阶段说明

通过 `get_pump_stage` 函数可以获取当前的阶段状态:

- Stage 1 (初始阶段): 未达到迁移阈值
- Stage 2 (竞争阶段): 达到迁移阈值，等待最后买入者获胜
- Stage 3 (迁移阶段): 最后买入者等待时间已到，可以执行迁移

## 前端处理流程

### Stage 1: 初始阶段
- 显示 Buy 和 Sell 按钮
- 显示当前池子状态(价格、储备量等)
- 显示距离迁移阈值还差多少

### Stage 2: 竞争阶段
- 禁用 Sell 按钮
- 保留 Buy 按钮
- 显示最后买入者信息(通过 `get_last_buyer` 获取):
  - 地址
  - 买入数量
  - 买入时间
- 显示获胜倒计时(当前时间 - 最后买入时间)

### Stage 3: 迁移阶段
- 隐藏 Buy/Sell 按钮
- 显示 "Claim Migration Right" 按钮(仅最后买入者可见)
- 显示 "Unfreeze Token" 按钮(所有用户可见)

## 示意图
mermaid
graph TD
A[初始阶段] -->|达到阈值| B[竞争阶段]
B -->|等待时间结束| C[迁移阶段]
subgraph "Stage 1"
A1[显示Buy/Sell按钮]
A2[显示池子状态]
A3[显示距离阈值差额]
end
subgraph "Stage 2"
B1[禁用Sell按钮]
B2[显示最后买入者]
B3[显示获胜倒计时]
end
subgraph "Stage 3"
C1[显示Claim按钮]
C2[显示Unfreeze按钮]
end
