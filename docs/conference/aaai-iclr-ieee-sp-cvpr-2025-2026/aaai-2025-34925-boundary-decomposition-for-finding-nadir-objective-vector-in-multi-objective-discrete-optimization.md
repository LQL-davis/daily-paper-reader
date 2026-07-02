---
title: Boundary Decomposition for Finding Nadir Objective Vector in Multi-Objective Discrete Optimization
title_zh: 多目标离散优化中基于边界分解寻找最差点向量
authors: "Ruihao Zheng, Zhenkun Wang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34925/37080"
tags: ["query:boundary-opt"]
score: 6.0
evidence: 应用边界分解寻找最差点向量
tldr: 精确求解多目标离散优化中的最差点向量对决策至关重要但极具挑战。本文提出BDNC算法，通过边界分解将问题转化为双层优化，其中下层边界子问题可利用任意单目标精确求解器，理论保证有限时间收敛，为多目标决策提供了一种通用且高效的框架。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 准确获取最差点向量对多目标决策至关重要，但现有方法在理论保证或计算成本上存在不足。
method: 提出BDNC算法，采用边界分解将原问题转化为各目标的双层优化，并利用边界子问题标量化求解。
result: 算法在有限时间内收敛到精确最差点，适用于大规模离散优化问题。
conclusion: 边界分解为多目标优化提供了一种通用且可靠的新途径，显著提升了求解的准确性与效率。
---

## Abstract
The exact nadir objective vector of a multi-objective discrete optimization problem (MODOP) is crucial for decision-making but remains challenging to find. Existing methods for tackling this issue have limitations in theoretical guarantees or high computational costs. This paper applies boundary decomposition to the MODOP and proposes an exact algorithm called BDNC. BDNC is designed to address a bilevel optimization problem for each objective with finite-time convergence guarantees. The lower-level optimization problem, termed the boundary subproblem, is a scalarization of the MODOP. It can be solved using any suitable single-objective exact solver. According to the theoretical foundations of boundary decomposition, some specific settings of the boundary subproblem can ensure alignment with the nadir objective vector under mild conditions. The upper-level optimization problem evaluates a potential setting using the optimal solution to the lower-level one. It employs our proposed novel pruning method to efficiently identify the specific settings. Moreover, BDNC can leverage a trade-off provided by the decision-makers, potentially facilitating the decision-making process. Experiments on various MODOPs demonstrate that BDNC exhibits superior and reliable performance in terms of runtime compared to existing exact methods.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在多目标离散优化问题（MODOP）中，精确确定最差点向量（nadir objective vector）对决策至关重要，但现有精确方法存在理论保证不足或计算成本过高的问题。
- **整体含义**：论文旨在提出一种高效、有理论保证的精确算法来计算最差点向量，支持多目标决策中的偏好设定和搜索指导。

## 2. 论文提出的方法论

- **整体框架**：基于边界分解（boundary decomposition）构建双层优化问题（BLOP），并对每个目标分别求解。
- **核心思想**：
  - **下层问题（LLOP）**：求解一个标量化的边界子问题，该子问题通过边界权重向量、参考点和参数α定义，可将原多目标问题转化为单目标问题，并可利用任意合适的单目标精确求解器。
  - **上层问题（ULOP）**：在(m−1)维单位超立方内搜索特定的边界权重向量，使得下层问题的最优解对应某个目标上的临界点（即Pareto前沿上该目标值最大的点）。
  - 通过不断从搜索空间中剪除无效区域（不能包含当前找到的最优点投影的区域），最终确定精确的最差点。
- **关键技术细节**：
  - 边界子问题的目标函数形如 $\max_{1\le j\le m} \{ w_{ij} [ (1-\alpha) f_j(x) + \frac{\alpha}{m} \sum_{k=1}^m f_k(x) - z_r^j ] \}$，其中 $w_i$ 满足特定条件。
  - 参数α与决策者可接受的权衡上限μ相关：$\alpha = m / ((m-1)\mu + m)$，允许用户指定可容忍的目标间边际替换率。
  - 搜索空间通过(m−1)维矩形表示，采用“懒惰剪枝”方法：仅当矩形被选中时才进行细分，并用命题1、2、3判定无效区域并删除。
  - 算法保证在Pareto前沿有限时有限步收敛。
- **算法流程**（BDNC）：
  1. 配置下层求解器，确定归一化参考点 $z^{r1}$ 和 $z^{r2}$，初始化潜在区域 $L$ 为整个搜索空间。
  2. 对每个目标 $i=1$ 到 $m$：
     - 当 $L$ 非空时，循环：
       - 从 $L$ 中按一定规则选择一个矩形 $R_s$。
       - 以其最大顶点构造边界权重向量，求解对应边界子问题得到 Pareto 最优解。
       - 若该解为新解，则按命题1确定无效区域；否则按命题3确定无效区域。
       - 从 $L$ 中移除被无效区域覆盖的矩形。
     - 循环结束即获得该目标的临界点。
  3. 输出各临界点组合即成最差点向量。

## 3. 实验设计

- **测试问题**：
  - 多目标分配问题（MOAP）
  - 多目标背包问题（MOKP）
  - 一般多目标整数线性规划（MOILP）
  - 多目标最短路径问题（MOSPP）
- **问题规模**：考虑3、4、5三种目标数，以及不同的变量数规模，共 24 种配置，每种随机生成 30 个实例，总共 720 个实例。
- **对比方法**：
  - KS (Kirlik and Sayın, 2015)
  - KL (Köksalan and Lokman, 2015)
  - FD&IS (Özpeynirci, 2017)
- **评价指标**：运行时间（秒），每个目标最大时限 3600 秒。采用中位运行时间、排名、Wilcoxon 秩和检验进行统计比较。

## 4. 资源与算力

- 文中未提及使用GPU，算法为精确组合优化方法，使用 MATLAB R2022a 实现，求解器为内置的 `intlinprog`。
- 硬件为双路 Intel Xeon Gold 6248R CPU（3.00 GHz），128 GB RAM，无GPU加速。
- 训练时长不适用，仅有求解时间记录，每个目标最长 3600 秒。

## 5. 实验数量与充分性

- 共 4 类问题 × 24 种配置 × 30 个实例 = 720 组主实验，规模较大。
- 还包含额外的消融实验和参数μ影响分析（参见附录），但正文未详述，仅提及。
- 对比方法为领域内最先进的精确方法，比较基准合理。
- 实验设计充分且公平，统计检验使用 Wilcoxon 秩和检验，并给出了中位数、最优/最差时间、排名和显著差异的汇总。

## 6. 论文的主要结论与发现

- BDNC 在所有问题上平均排名最优（1.2），整体运行时间显著低于三种对比方法，尤其在 5 目标大规模问题上，BDNC 是唯一能在规定时间内求解大部分实例的算法。
- BDNC 的运行时稳定性（最差情况）优于对比方法，展现了较强的鲁棒性。
- 对比方法 KS 对目标数敏感，FD&IS 表现最差；KL 在少数小规模实例略有优势，但整体不及 BDNC。
- 边界分解结合懒惰剪枝有效缩小了搜索空间，使算法兼具理论保证和高效率。

## 7. 优点

- **理论保证**：有限步收敛至精确最差点，且支持决策者偏好（μ参数）以得到满意的 nadir。
- **通用性**：下层求解器可替换任意单目标精确求解器，适应不同问题结构。
- **高效剪枝**：提出的 (m−1) 维矩形表示和懒惰剪枝大幅减少冗余计算。
- **实验全面**：涵盖多种问题类型、目标数和规模，对比方法先进，统计检验严谨。
- **稳定性**：最差情况运行时间远低于对比方法，异常值少。

## 8. 不足与局限

- **只验证了离散线性/整数规划类问题**，未测试非线性、非凸或约束复杂的问题。
- **对决策变量数 n 的 scalability 未深入分析**，虽然结果表现良好，但未单独讨论变量规模对运行时间的影响。
- **未与启发式方法进行全面比较**（仅在相关工作中提及其局限），无法体现精确方法在求解质量上的绝对优势。
- **懒惰剪枝的具体实现细节**（例如矩形选择规则）可能对性能有影响，文中提到了可替换但未系统比较不同规则。
- **仅使用 CPU 单线程**，未探讨并行化潜力，虽然算法可对各目标并行求解，但实验并未体现并行加速。
- **μ参数的选取**：当决策者不提供时取大值，但极大μ可能影响效率，附录中的分析程度不明确。

（完）
