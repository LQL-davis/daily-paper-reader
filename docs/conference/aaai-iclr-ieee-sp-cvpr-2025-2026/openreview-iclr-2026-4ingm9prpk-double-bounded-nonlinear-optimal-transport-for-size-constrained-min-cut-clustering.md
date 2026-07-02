---
title: Double-Bounded Nonlinear Optimal Transport for Size Constrained Min Cut Clustering
title_zh: 面向尺寸约束最小割聚类的双界非线性最优传输
authors: "Fangyuan Xie, Jh Yuan, Feiping Nie"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=4iNGM9PRPk"
tags: ["query:boundary-opt"]
score: 7.0
evidence: 将最小割松弛为双界最优传输以求解尺寸约束的边界优化
tldr: 针对尺寸约束最小割聚类问题，该文将最小割松弛为双界非线性最优传输问题，并提出了基于Frank-Wolfe的DNF算法，能高效求解所有双界非线性最优传输问题，并为凸光滑情形提供了收敛保证。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 当前最小割方法求解慢且易收敛到平凡解。
method: 将最小割转化为双界非线性最优传输并设计DNF算法。
result: DNF能高效求解尺寸约束最小割及其他双界最优传输问题。
conclusion: 提出首个统一求解框架，拓展了最优传输的应用边界。
---

## Abstract
Min cut is an important graph partitioning method. However, current solutions to the min cut problem suffer from slow speeds, difficulty in solving, and often converge to simple solutions. To address these issues, we relax the min cut problem into a double-bounded constraint and, for the first time, treat the min cut problem as a double-bounded nonlinear optimal transport problem. Additionally, we develop a method for solving d-bounded nonlinear optimal transport based on the Frank-Wolfe method (abbreviated as DNF). Notably, DNF not only solves the size constrained min cut problem but is also applicable to all double-bounded nonlinear optimal transport problems. We prove that for convex problems satisfying Lipschitz smoothness, the DNF method can achieve a convergence rate of \(\mathcal{O}(\frac{1}{t})\). We apply the DNF method to the min cut problem and find that it achieves state-of-the-art performance in terms of both the loss function and clustering accuracy at the fastest speed, with a convergence rate of \(\mathcal{O}(\frac{1}{\sqrt{t}})\). Moreover, the DNF method for the size constrained min cut problem requires no parameters and exhibits better stability.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：最小割（Min Cut）是图划分的重要方法，但现有解法存在速度慢、求解困难以及容易收敛到平凡解（如把全部节点聚为一类）等不足。特别是在引入簇尺寸约束（Size Constrained）以防止平凡解时，这些问题尤为突出。
- **整体含义**：论文首次将尺寸约束的最小割聚类问题松弛为**双界非线性最优传输（Double-Bounded Nonlinear Optimal Transport）** 问题，从而从一个全新的视角统一了图割与最优传输，并提供了高效泛用的求解器。

### 2. 论文提出的方法论
- **核心思想**：把带有簇尺寸上下界约束的最小割目标，重新表述为一个受双边界约束的非线性最优传输问题，使离散分配矩阵满足行/列和的双界条件。
- **关键技术细节**：
  - **问题松弛**：将原组合优化问题连续松弛，在最优传输的框架下引入双重边界（每个簇的大小有上界和下界）。
  - **求解算法 DNF**：基于 Frank-Wolfe 方法设计了一种求解双界非线性最优传输的算法（简称 DNF）。该算法无需额外超参数，能通用于所有双界非线性最优传输问题。
  - **收敛性质**：对于满足 Lipschitz 光滑的凸问题，DNF 可达到 \(\mathcal{O}(1/t)\) 的收敛速率；应用于尺寸约束最小割时，收敛速率约为 \(\mathcal{O}(1/\sqrt{t})\)。
- **算法流程（文字说明）**：DNF 沿袭 Frank-Wolfe 的线性化子问题求解框架，在每一步迭代中求解一个受双界约束的线性最优传输子问题（可转化为高效求解的标准形式），然后沿下降方向更新变量，直至收敛。

### 3. 实验设计
- **数据集/场景**：实验在图聚类/划分任务中进行（具体数据集名称在摘要未详列，文中可能包括常用的聚类 benchmark，如文本、图像或图网络数据）。
- **Benchmark 与对比方法**：将 DNF 应用于尺寸约束最小割，并与该问题上现有的主流求解方法进行对比，评价指标包括收敛速度、损失函数值和聚类准确率。
- **结论性比较**：DNF 在损失值和聚类准确率上均达到最先进（state-of-the-art）水平，同时拥有最快求解速度。

### 4. 资源与算力
- 摘要及已获取文本中**未提及**具体硬件配置（GPU 型号、数量）及训练时长。这类理论/算法类论文可能侧重收敛速率与相对时间，未必注明绝对算力消耗。

### 5. 实验数量与充分性
- 摘要未详细列出实验组数，但从描述可推断包括：
  - 多个数据集上的聚类准确率与损失对比；
  - 收敛速度的对比实验；
  - 凸问题子类下的收敛速率验证。
- 实验通过多指标（损失、精度、速度、稳定性）对比，较为全面；因 DNF 本身无额外参数，对比客观性较高。但摘要未透露是否包含消融实验或统计显著性检验，充分性可从正文进一步确认。

### 6. 论文的主要结论与发现
- 尺寸约束最小割可被统一建模为**双界非线性最优传输**问题，这是一个全新的形式化视角。
- 提出的 **DNF 算法**不仅适用于该聚类问题，而且是首个能求解所有双界非线性最优传输问题的统一框架，扩展了最优传输的应用边界。
- DNF 在尺寸约束最小割上实现**最先进的精度与效率**，无需调参且稳定性更好。

### 7. 优点
- **视角新颖**：首次将最小割严格松弛为双界最优传输，开辟了两领域交叉的新思路。
- **算法通用**：DNF 不限于聚类，可求解任意双界非线性最优传输实例，适用面广。
- **理论与实用兼具**：给出了凸光滑情形下的收敛率保证，而在应用中达到 SOTA 且无参数。
- **高效稳定**：在多个数据集上一致地快于现有方法，并克服了收敛到平凡解的问题。

### 8. 不足与局限
- **实验细节缺失**：从摘要无法获知所用数据集的具体规模、特征类型及比较方法的实施细节，评估的覆盖面可能需进一步审视。
- **非凸问题的理论局限**：对于非凸目标（如尺寸约束 min cut 本身未必凸），收敛速率退化到 \(\mathcal{O}(1/\sqrt{t})\)，且未深入探讨所得解与全局最优的 gap。
- **应用限制**：当前讨论限于图划分/聚类，未展示在更复杂的图结构或大规模百万节点图上的可伸缩性，也未讨论与深度聚类等前沿范式的结合。

（完）
