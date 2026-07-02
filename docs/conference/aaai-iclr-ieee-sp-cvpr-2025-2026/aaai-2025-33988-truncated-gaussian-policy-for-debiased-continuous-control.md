---
title: Truncated Gaussian Policy for Debiased Continuous Control
title_zh: 截断高斯策略实现无偏连续控制
authors: "Ganghun Lee, Minji Kim, Minsu Lee, Byoung-Tak Zhang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33988/36143"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 使用截断高斯策略减轻边界动作偏差
tldr: 连续控制任务中，高斯策略的无界支持会产生向边界动作采样的偏差，损害如近端策略优化等算法的训练。本文分析此问题并引入具有固边界的截断高斯策略以减轻偏差，实验表明该简单替换能稳定提升策略性能，为有界连续控制提供了高效直接的解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 现有高斯策略因无界支撑导致连续控制中的边界动作偏差，影响训练效果。
method: 提出截断高斯策略，直接内置动作边界以消除采样偏差。
result: 在多种连续控制任务中，截断策略稳定提升了强化学习算法的表现。
conclusion: 利用有限支撑的高斯策略可有效解决边界动作偏差，改善连续控制。
---

## Abstract
In continuous domains, reinforcement learning policies are often based on Gaussian distributions for their generality. However, the unbounded support of Gaussian policy can cause a bias toward sampling boundary actions in many continuous control tasks that impose action limits due to physical constraints. This "boundary action bias'' can negatively impact training in algorithms like Proximal Policy Optimization. Despite this, it has been overlooked in many existing research and applications. In this paper, we revisit this issue by presenting illustrative explanations and analysis from the sampling point of view. Then, we introduce a truncated Gaussian policy with inherent bounds as a minimal alternative to mitigate the bias. However, we find that the plain truncated Gaussian policy may lay the counter-bias, preferring interior actions: to balance the bias, we ultimately propose a scale-adjusted truncated Gaussian policy, where the distribution scale shrinks if the location is near the boundaries. This property makes boundary actions deterministic more than in plain truncated Gaussian, but still less than in original Gaussian. Extensive empirical studies and comparisons on various continuous control tasks demonstrate that the truncated Gaussian policies significantly reduce the rate of boundary action usage, while scale-adjusted ones successfully balance the bias and counter-bias. It generally outperforms the Gaussian policy and shows competitive results compared to other approaches designed to counteract the bias.

---

## 论文详细总结（自动生成）

以下是基于提供的论文内容生成的详细中文总结，按要点依次展开。

# 论文总结：基于截断高斯策略的无偏连续控制

## 1. 核心问题与整体含义（研究动机与背景）
- **研究动机**：连续控制任务中，强化学习策略通常采用高斯分布进行动作采样，因其通用性和数学便利性（可微性、位置-尺度参数直观等）。然而，高斯分布的无界支持（unbounded support）与动作空间的物理约束（如动作限制在 $[-1, 1]$）存在矛盾。
- **核心问题**：当采样动作超出边界时，常用的处理方法（如在PPO中直接裁剪到最近边界）会导致**边界动作偏差（boundary action bias）**，即策略过度倾向于采样边界动作，扭曲了真实动作概率分布，并可能形成“探索 → 边界动作 → 概率升高 → 进一步偏向边界”的反馈循环，最终损害训练效果，造成过早收敛或策略退化。
- **整体含义**：论文提出用有界支持的截断高斯分布替代标准高斯分布，以最小改动消除裁剪带来的偏差，并进一步通过尺度调整平衡可能导致的“内部动作偏好”，从而实现无偏、高效的连续控制策略。

## 2. 方法论：核心思想、关键技术细节与算法流程
### 2.1 标准高斯策略的边界偏差形式化
- 定义边界动作偏差量 $B_f$ 为高斯概率密度函数 $f(x;\mu,\sigma)$ 在动作边界 $[l, u]$ 之外的面积之和：
  $$B_f = \int_{-\infty}^{l} f(x) dx + \int_{u}^{\infty} f(x) dx$$
- 当 $\mu$ 偏离中心时偏差增大；当 $|\mu|$ 超出边界时，$\sigma$ 的作用减弱，偏差难以通过减小尺度消除。

### 2.2 截断高斯策略（Truncated Gaussian Policy）
- **核心思想**：直接使用有界支持的截断高斯分布（truncated Gaussian）来采样动作，避免裁剪，使PDF与真实动作概率保持一致。
- **分布公式**：
  $$f(x;\mu,\sigma,l,u) = \frac{1}{\sigma} \cdot \frac{\phi\!\left(\frac{x-\mu}{\sigma}\right)}{\Phi\!\left(\frac{u-\mu}{\sigma}\right) - \Phi\!\left(\frac{l-\mu}{\sigma}\right)}$$
  其中 $\phi$ 和 $\Phi$ 分别为标准正态的PDF和CDF。
- **参数限制**：为防止位置参数 $\mu$ 超出边界导致梯度不稳定，策略网络的输出 $g(s)$ 经过tanh缩放和偏移：
  $$\mu = \frac{u-l}{2} \cdot (\tanh(g(s)) + 1) + l$$
- **效果**：消除了裁剪带来的概率扭曲，但实验发现普通截断高斯可能**过度补偿**，导致“反偏差”——过分偏好内部动作，使边界动作难以采样。

### 2.3 尺度调整的截断高斯策略（Scale-Adjusted Truncated Gaussian Policy）
- **核心思想**：在位置 $\mu$ 靠近边界时，对尺度 $\sigma$ 进行折扣，使分布更集中于边界，平衡边界动作的采样概率。
- **尺度调整函数**：
  $$\sigma' = \sigma \cdot d(\mu; l, u)$$
  $$d(\mu; l, u) = k \cdot \frac{\sqrt{ \left( \frac{u-l}{2} \right)^k - \left( \mu - \frac{u+l}{2} \right)^k }}{(u-l)/2} \cdot (1 - d_{\min}) + d_{\min}$$
  其中 $k$ 控制峰度（形状），$d_{\min}$ 为最小折扣率，防止尺度归零。
- **特性**：当 $\mu$ 位于中心时 $d=1$（尺度不变），靠近边界时尺度减小，使得边界动作采样变得更确定，但仍保留一定的随机性；相比标准高斯，边界动作的概率更高，但不会像标准高斯那样将所有概率堆积在边界上。

### 2.4 算法流程
- 将尺度调整截断高斯分布作为PPO的策略分布类：
  1. 网络输出位置参数 $\mu$（经由tanh约束）和独立的尺度参数 $\sigma$。
  2. 根据 $\mu$ 和动作边界计算调整后尺度 $\sigma'$。
  3. 从截断高斯分布 $\mathcal{TN}(\mu, \sigma', l, u)$ 中采样动作，无需裁剪。
  4. 使用PPO的损失函数进行策略优化，梯度和似然计算均基于该有界分布。

## 3. 实验设计
### 3.1 实验场景与基准测试
- **主要任务**：MuJoCo连续控制基准（v4），包含6个运动任务（HalfCheetah, Walker2d, Ant, Hopper, Humanoid, Swimmer）和2个操作任务（Pusher, Reacher），共8个任务。
- **高维动作空间任务**：HumanoidBench（h1hand-walk, h1hand-reach，动作维度61）和DeepMind Control Suite（dog-walk, dog-run，动作维度38）。

### 3.2 对比方法
- **标准高斯策略（Gaussian）**：使用PPO裁剪出界动作。
- **梯度修正（CAPG）**：修正策略梯度以考虑裁剪效果。
- **替代分布**：Beta分布（Beta）、对数正态分布/压缩高斯（Logit）。
- **本文方法**：普通截断高斯（TGaussian）和尺度调整截断高斯（SA-TGaussian）。
- 所有方法均基于PPO框架（ClearRL标准实现），初始尺度 $\sigma_{\text{init}} = 0.5$ 或 $1.0$。

### 3.3 评价指标
- **aBAR（平均边界动作率）**：单回合内动作落在动作范围顶部和底部1%内的比例。
- **标准差的演变**：观察策略尺度随训练的变化。
- **回合回报**：任务最终性能。

## 4. 资源与算力
- 论文中**未明确提及**所使用的GPU型号、数量或训练时间等具体算力信息。
- 从实验规模推测，可能是在常规深度学习服务器上运行，但原文无任何相关数据，此为实验透明性的一项不足。

## 5. 实验数量与充分性
### 5.1 实验组数统计
- **主要性能对比**：在8个MuJoCo任务上，以 $\sigma_{\text{init}} = 0.5$ 进行全方法对比（10种子平均），并补充了 $\sigma_{\text{init}} = 1.0$ 的设置（排除Beta）。
- **边界偏差实证研究**：绘制各方法的aBAR曲线、标准差曲线，并增设GEntmax和GBounds两个极端设置以分析偏差对性能的影响。
- **高维动作空间测试**：4个额外任务。
- **消融实验**：
  - 尺度调整参数 $d_{\min}$ 取值：0.001, 0.01, 0.1, 0.5, 2.0。
  - 峰度参数 $k$ 取值：1, 2, 3, 4。
- 总计超过10种任务 × 多种方法 + 多个消融维度，实验数量较为充分。

### 5.2 实验的客观性与公平性
- 所有方法均基于相同的PPO代码库和超参数（初始尺度、网络结构等），对比公平。
- 评估采用多种子平均（主要任务10种子，高维任务5种子），减少了随机性影响。
- 消融实验针对超参数进行系统扫描，探讨了任务依赖性。

## 6. 主要结论与发现
- **边界偏差广泛存在**：高斯和CAPG策略在多数MuJoCo运动任务中aBAR高达10%-30%，表明严重偏向边界动作。
- **有界分布能大幅降低aBAR**：截断高斯、Beta、Logit等方法将aBAR降至0%-2%，彻底消除了裁剪引入的偏差。
- **尺度调整达到最佳平衡**：普通截断高斯因过度偏好内部动作，性能有时不如高斯策略；而SA-TGaussian通过边界附近的尺度收缩，既能利用边界动作的优势，又避免过度极化，在整体归一化得分上排名第一（0.747），显著优于高斯（0.609）和CAPG（0.679）。
- **任务特性影响偏差的影响程度**：某些任务（如Swimmer）可能天然需要较多的边界动作，此时截断高斯反而可能有害，但SA-TGaussian通过调整可在多数任务上实现鲁棒提升。
- **高维动作空间中同样有效**：SA-TGaussian在61维和38维的动作空间中均取得最佳回报。

## 7. 优点（亮点）
- **方法简单且优雅**：截断高斯只是标准高斯的自然有界扩展，改动小，易于集成到现有PPO框架中。
- **深入的理论与实证分析**：对边界偏差的成因、与尺度参数的关系进行了形式化和图示化解释，并通过aBAR和标准差轨迹提供了清晰证据。
- **平衡偏差的设计**：提出尺度调整函数，在保留截断高斯避免反偏差的同时，允许策略在必要时可靠地采样边界动作，体现了对分布趋势的深刻理解。
- **全面的实验验证**：涵盖多个域、多种动作维度、多种对比方法，以及超参数消融，评估体系完善。

## 8. 不足与局限
- **算力信息缺失**：未报告任何关于GPU资源或训练时间的数据，影响实验可复现性的判断。
- **内部机制未深入探索**：论文明确承认“并未完全探索抑制边界动作如何导致更好性能的内部机制”，仅从现象层面解释了调控效果。
- **任务依赖性较强**：不同任务对边界动作的敏感程度差异大（如Swimmer偏爱边界动作，Ant偏爱内部动作），最优超参数 $k$ 和 $d_{\min}$ 需针对任务调整，泛化设定仍需研究。
- **性能提升幅度有时有限**：在某些任务上，有界分布与标准高斯的性能差异并不显著，论文推测RL本身具有一定分布适应能力，边界偏差可能并非所有性能差距的主因。
- **算法局限**：所提方法仅在PPO框架下验证，在其他连续控制算法（如SAC、TD3）中的表现尚未探索；且尺度调整函数的形式选择较为启发式，可能并非最优。

（完）
