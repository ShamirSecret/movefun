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
- 显示最后买入者的地址缩写(可以通过 `get_last_buyer` 获取或者后端获取):
  - 地址
  - 买入数量
  - 买入时间
- 显示获胜倒计时(当前时间 - 最后买入时间)

# DEX收益均衡点通用公式推导题

## 问题描述
在基于恒定乘积模型的DEX中，最后一个买家可以获得池子里剩余代币总量的 `reward_ratio` 作为奖励。之后，池子会按照最后交易价格（`y_threshold/x_current`）转移到新的DEX中。需要推导一个通用公式，计算在什么价格（`y_threshold`）下，最后买家的总收益等于成本。

## 变量定义
- **x_initial**: 初始代币数量
- **y_virtual**: 虚拟MOVE数量 
- **y_threshold**: 最后买家买入后的MOVE数量（待求解）
- **reward_ratio**: 剩余代币奖励比例（例如 10% = 0.1）
- **min_price**: 最低买入价格

## 关键公式
1. **恒定乘积公式**:
   - \( k = x_{initial} \times y_{virtual} \)
   - \( x_{current} = \frac{x_{initial} \times y_{virtual}}{y_{threshold}} \)
   - 最终价格 \( P = \frac{y_{threshold}}{x_{current}} \)

2. **最后买家交易**:
   - 买入成本 \( \Delta b = y_{threshold} - y_{virtual} \)
   - 剩余代币数量 \( = x_{current} - \Delta a \)
   - 获得奖励数量 \( = (x_{current} - \Delta a) \times reward\_ratio \)

3. **新DEX池子**:
   - 初始价格 \( = \frac{y_{threshold}}{x_{current}} \)
   - 新的恒定乘积常数 \( k' = y_{threshold} \times x_{current} \)

## 求解要求
1. **推导出 `y_threshold` 的通用计算公式**:
   - 需考虑所有变量之间的关系
   - 新旧DEX池子的恒定乘积约束
   - 最低价格约束
   - 收益平衡条件

2. **分析各参数对均衡点的影响**:
   - **x_initial** 的影响
   - **y_virtual** 的影响
   - **reward_ratio** 的影响
   - **min_price** 的影响

## 计算步骤
1. **根据恒定乘积公式**，推导出当前代币数量 \( x_{current} \) 与 `y_threshold` 的关系。
2. **设定最后买家买入的代币量** \( \Delta a \)，并计算其买入成本 \( \Delta b \)。
3. **计算剩余代币数量** 和 **获得的奖励代币数量**。
4. **设定新DEX池子的恒定乘积**，并推导出收益均衡条件。
5. **求解 `y_threshold`**，使得买入成本与卖出收益相等。

请给出完整的数学推导过程和最终通用公式。

### Stage 3: 迁移阶段
- 隐藏 Buy/Sell 按钮
- 显示 "Claim Migration Right" 按钮(仅最后买入者可见)
- 显示 "Unfreeze Token" 按钮(所有用户可见)

