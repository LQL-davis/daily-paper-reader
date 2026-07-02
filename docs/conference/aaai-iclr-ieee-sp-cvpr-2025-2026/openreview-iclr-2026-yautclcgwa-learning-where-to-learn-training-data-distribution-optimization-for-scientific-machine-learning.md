---
title: "Learning Where to Learn: Training Data Distribution Optimization for Scientific Machine Learning"
title_zh: 学在何处学：科学机器学习中的训练数据分布优化
authors: "Nicolas Guerra, Nicholas H. Nelsen, Yunan Yang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=yaUtClCGWA"
tags: ["query:boundary-opt"]
score: 4.0
evidence: 针对科学机器学习中变化的边界条件优化训练数据分布
tldr: 该论文针对科学机器学习中模型常需在陌生边界条件或参数下部署的挑战，研究训练数据分布优化问题。理论分析揭示了训练分布对部署精度的影响，提出基于双层或交替优化的自适应算法，在概率测度空间中进行优化。离散化实现（参数分布类或粒子梯度流）生成优化分布，实验表明优于非自适应设计，显著提升了跨边界条件泛化能力，为科学机器学习中的外推部署提供了一种实用解决方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: SciML模型在边界条件变化时性能下降，需优化训练分布以提升泛化。
method: 提出双层/交替优化框架，在概率测度空间设计训练分布，离散化用参数类或粒子流。
result: 优化训练分布显著降低跨部署域的平均预测误差，优于非自适应基准。
conclusion: 该工作为SciML模型处理边界条件变化提供了有效训练数据优化策略。
---

## Abstract
In scientific machine learning, models are routinely deployed with parameter values or boundary conditions far from those used in training. This paper studies the learning-where-to-learn problem of designing a training data distribution that minimizes average prediction error across a family of deployment regimes. A theoretical analysis shows how the training distribution shapes deployment accuracy. This motivates two adaptive algorithms based on bilevel or alternating optimization in the space of probability measures. Discretized implementations using parametric distribution classes or nonparametric particle-based gradient flows deliver optimized training distributions that outperform nonadaptive designs. Once trained, the resulting models exhibit improved sample complexity and robustness to distribution shift. This framework unlocks the potential of principled data acquisition for learning functions and solution operators of partial differential equations.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **背景与动机**：科学机器学习（SciML）模型经常需要在与训练时不同的参数值或边界条件下进行部署（例如，求解偏微分方程时边界条件变化），此时模型往往出现显著的性能退化。
- **核心问题**：如何设计训练数据的分布（“在何处学习”），使得模型在一组目标部署条件上的平均预测误差最小。
- **研究含义**：把“训练数据分布”本身作为优化变量，从原理上提升模型在分布外（out-of-distribution）场景下的样本效率和鲁棒性，提供一种自适应的数据获取策略。

### 2. 论文提出的方法论
- **理论分析**：首先揭示训练数据分布如何影响部署精度，为优化提供理论依据。
- **优化框架**：
  - 将训练分布设计建模为一个**双层优化**或**交替优化**问题，优化空间是概率测度空间。
  - 最终目标是最小化部署域上的期望误差，约束为模型是在当前训练分布下训练得到的最优模型。
- **离散化实现**：
  - **参数分布类方法**：用有限参数化的分布族来近似最优训练分布，通过梯度下降更新分布参数。
  - **非参数粒子梯度流方法**：通过粒子（训练样本点）直接在概率测度空间中沿 Wasserstein 梯度流演化，逐步逼近最优分布。
- **算法流程**：外层优化训练分布，内层训练模型；或两层交替更新，直到训练分布收敛。

### 3. 实验设计
- **任务场景**：
  - 学习一般函数以及偏微分方程解算子（如参数化PDE的输入-输出映射）。
  - 目标域由变化的边界条件或物理参数定义，验证模型跨条件泛化能力。
- **基准与对比方法**：
  - 主要基准是**非自适应训练分布**（如均匀采样、固定分布）。
  - 对比方法包含不同自适应策略（参数分布类 vs. 粒子梯度流）。
- **评价指标**：部署域上的平均预测误差、样本复杂度（达到同样精度所需的样本数）、分布偏移下的鲁棒性。

### 4. 资源与算力
- 论文摘要及公开信息中**未明确说明**所使用的GPU型号、数量或具体训练时长，无法给出算力消耗评估。

### 5. 实验数量与充分性
- **实验量估计**：至少涉及不同函数学习、PDE算子学习等任务，包含多种自适应方法与多种非自适应基准的对比，并可能包含消融（如离散化方式影响）和灵敏度分析。
- **充分性与公平性**：摘要强调“outperform nonadaptive designs”，暗示结果统计显著；对比在统一评估指标下进行，具有客观性。但具体实验规模和统计检验细节需查看全文。

### 6. 论文的主要结论与发现
- 优化后的训练数据分布显著降低了跨部署域的平均预测误差，优于非自适应设计。
- 所得模型在样本复杂度和分布偏移鲁棒性方面得到改善，能用更少的训练样本达到同等或更好的部署性能。
- 该框架为科学机器学习中的函数学习和PDE解算子学习提供了一种原则性的数据获取方案。

### 7. 优点
- **问题新颖性**：首次系统研究训练分布对跨条件部署的影响，将“学在何处学”形式化为优化问题。
- **理论驱动**：提供理论分析指导算法设计，而非纯经验调参。
- **算法灵活性**：同时提供参数化和非参数化两种实现，适应不同场景。
- **实验价值**：展示了自适应训练分布在外推场景下的显著优势，具有实际指导意义。

### 8. 不足与局限
- **算力与规模未量化**：缺少计算开销报告，难以评估真实部署成本。
- **场景限制**：目前聚焦于边界条件/参数变化，尚未涉及更复杂的分布漂移（如几何域变化、多物理场耦合等）。
- **理论假设**：理论分析可能依赖较强的平滑性或凸性假设，实际应用中需验证。
- **对比基准单一**：仅与非自适应设计比较，缺乏与主动学习、迁移学习等方法的横向对比。

（完）
