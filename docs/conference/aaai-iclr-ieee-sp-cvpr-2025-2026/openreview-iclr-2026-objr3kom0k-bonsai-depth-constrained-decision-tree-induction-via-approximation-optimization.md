---
title: "BONSAI: Depth-Constrained Decision Tree Induction via Approximation Optimization"
title_zh: "BONSAI: 通过近似优化的深度受限决策树归纳"
authors: "Sungbum Jun, Cheolwoo Lee"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=objR3KOm0k"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 通过近似优化优化决策树分裂边界
tldr: 提出BONSAI算法，通过近似优化在深度限制下全局优化决策树的分裂边界，克服了传统贪心方法的局部最优问题，提高了决策树的精确性和可解释性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 传统决策树贪心方法可能得到次优分裂。
method: 将决策树归纳形式化为近似优化问题并高效求解。
result: BONSAI在深度受限下取得更优的决策边界。
conclusion: 为决策树边界优化提供全局优化新范式。
---

## Abstract
We propose BONSAI (Branch Optimization for Numerical Splits for Accurate and Interpretable models), a novel decision tree algorithm that optimizes split decisions over numerical attributes under a depth limit. In contrast to conventional greedy methods such as CART and C4.5, which construct trees via locally top-down splits, BONSAI formulates tree induction as an approximation optimization problem based on a new slot-node structure that can deal with both categorical and numerical features. To improve the performance and interpretability, BONSAI combines a compact optimization formulation with efficient search-space reduction techniques, thereby avoiding the combinatorial overhead of traditional mixed-integer programming (MIP) approaches. Empirical evaluations on benchmark datasets show that BONSAI achieves superior predictive accuracy, model compactness, and interpretability compared to both greedy and existing optimization-based methods.

---

## 论文详细总结（自动生成）

基于提供的论文元数据及摘要内容，以下是对论文《BONSAI: Depth-Constrained Decision Tree Induction via Approximation Optimization》的详细结构化总结。由于未能获取完整PDF，部分细节（如具体数据集、算力等）依现有信息推断或注明缺失。

### 1. 论文的核心问题与整体含义
- **研究动机**：传统决策树算法（如CART、C4.5）采用贪心、自顶向下的局部最优分裂策略，易导致次优的分裂边界，尤其当树深度受限时，模型的预测精度与可解释性会受损。
- **整体含义**：论文提出BONSAI算法，将深度受限决策树的归纳过程建模为一个近似优化问题，直接寻求数值特征分裂边界的全局较优解，旨在突破贪心方法的局部性限制，同时保持决策树固有的可解释性。

### 2. 论文提出的方法论
- **核心思想**：通过构造一种新型的“槽-节点”（slot-node）结构统一处理类别与数值特征，并将整棵树的构建转化为一个近似优化问题，从而全局地优化所有分裂决策。
- **关键技术细节**：
  - 采用紧凑的优化公式，避免传统混合整数规划（MIP）方法的组合爆炸。
  - 引入高效的搜索空间缩减技术，提升求解效率。
- **算法流程（文字描述）**：先基于槽-节点结构生成候选分裂边界集合，再通过近似优化目标函数（权衡拟合精度与模型复杂度）同时确定各节点的分裂规则，最终输出一个深度受约束的决策树。该流程在优化阶段即考虑了树的整体结构，而非逐层贪心添加。

### 3. 实验设计
- **数据集 / 场景**：论文在多个标准基准数据集（benchmark datasets）上进行了评估，具体数据集名称未在摘要中列出。
- **对比方法（benchmark）**：
  - 传统贪心方法：如CART、C4.5。
  - 现有基于优化的方法：其他非贪心的决策树构建算法（如MIP类或全局优化方法）。
- **评估指标**：预测准确性、模型紧凑度（如叶节点数或树大小）以及可解释性。

### 4. 资源与算力
- 论文摘要及元数据中**未明确说明**所使用的硬件资源（如GPU型号、数量）或训练耗时。因此，算法在大规模数据上的计算效率与资源消耗暂无法从现有信息判断。

### 5. 实验数量与充分性
- 根据摘要描述，实验覆盖了**多个基准数据集**，并与**两类代表性方法**（贪心类与优化类）进行了比较，具备一定的广度。
- 摘要未提及消融实验、统计显著性检验或超参数敏感性分析的具体细节，故从现有信息**无法确认实验是否包含完备的消融或鲁棒性验证**。
- 整体看，实验设计在对比对象和评估维度上较为客观、公平，但实验总量和细致度需原文支撑。

### 6. 论文的主要结论与发现
- BONSAI在深度受限条件下，能够在数值特征上找到更优的分裂边界，从而获得**更高的预测精度**。
- 相较于贪心及现有优化方法，BONSAI产生的决策树**更紧凑、可解释性更强**，同时避免了传统MIP方法的高计算开销。
- 该工作为决策树的边界优化提供了一种**全局优化的新范式**。

### 7. 优点
- **方法创新**：将树归纳形式化为近似优化，引入槽-节点结构，概念新颖。
- **性能与可解释兼顾**：在提升精度的同时控制树复杂度，契合高风险领域对可解释AI的需求。
- **效率设计**：通过搜索空间缩减与紧凑公式，缓解了全局优化的计算瓶颈。
- **统一处理**：对类别与数值特征的内在统一框架。

### 8. 不足与局限
- **信息不完整**：由于仅有摘要与元数据，大量实验细节（数据集、消融分析、计算开销、超参数影响）未知，结论的泛化性与可靠性难以全面评估。
- **可能的局限**：深度约束本身是超参数，算法在无约束或极深树场景下的表现未提及；近似优化所引入的近似误差对最终解质量的影响未量化；对大规模高维数据的扩展性存疑。
- **对比偏倚风险**：若对比方法未进行充分的超参数调优，可能夸大BONSAI的优势，但现有信息无法判断。

（完）
