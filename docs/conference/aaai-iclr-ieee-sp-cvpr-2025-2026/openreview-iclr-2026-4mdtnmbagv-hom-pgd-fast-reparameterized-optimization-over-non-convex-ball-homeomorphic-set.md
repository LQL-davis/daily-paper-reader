---
title: "Hom-PGD+: Fast Reparameterized Optimization over Non-convex Ball-Homeomorphic Set"
title_zh: "Hom-PGD+: 非凸球同胚集上的快速重参数化优化"
authors: "Chenghao Liu, Enming Liang, Minghua Chen"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=4MdTNmBAGv"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 学习同胚映射将约束集边界映射为单位球以实现高效优化
tldr: 该论文针对非凸约束集（如星形集）优化计算成本高的难题，提出Hom-PGD+方法。它利用可逆神经网络学习非凸集到单位球的光滑同胚，将原问题重参数化为无约束优化，避免复杂的投影或预言计算。理论证明该方法收敛且计算高效，实验显示在合成和实际问题上均优于现有投影类方法，为非凸边界约束优化提供了新的有效工具。该方法成功将带边界约束的非凸优化转化为易求解的形式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 非凸约束集优化常需昂贵投影，阻碍高效求解，本方法旨在克服此局限。
method: 使用可逆神经网络将球同胚非凸约束集映射到单位球，实现无投影优化。
result: 实验证明Hom-PGD+在合成与实际问题中性能优越，收敛更快且计算成本低。
conclusion: Hom-PGD+为边界约束非凸优化提供了高效新范式，拓展了非凸优化应用。
---

## Abstract
Optimization over general non-convex constraint sets poses significant computational challenges due to their inherent complexity. 
In this paper, we focus on optimization problems over non-convex constraint sets that are homeomorphic to a ball, which encompasses important problem classes such as star-shaped sets that frequently arise in machine learning and engineering applications. 
We propose \textbf{Hom-PGD$^+$}, a fast, \textit{learning-based} and \textit{projection-efficient} first-order method that efficiently solves such optimization problems without requiring expensive projection or optimization oracles.
Our approach leverages an invertible neural network (INN) to learn the homeomorphism between the non-convex constraint set and a unit ball, transforming the original problem into an equivalent ball-constrained optimization problem. This transformation enables fast projection-efficient optimization while preserving the fundamental structure of the original problem. We establish that Hom-PGD$^+$ achieves an $\mathcal{O}(\epsilon^{-2})$ convergence rate to obtain an $\epsilon + \mathcal{O}(\sqrt{\epsilon_{\rm inn}})$-approximate stationary solution, where $\epsilon_{\rm inn}$ denotes the homeomorphism learning error. 
This convergence rate represents a significant improvement over existing methods for optimization over non-convex sets. Moreover, Hom-PGD$^+$ maintains a per-iteration computational complexity of $\mathcal{O}(W)$, where $W$ is the number of INN parameters. Extensive numerical experiments, including chance-constrained optimization popular in power systems, demonstrate that Hom-PGD$^+$ achieves convergence rates comparable to state-of-the-art methods while delivering speedups of up to one order of magnitude.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **研究背景与动机**：许多机器学习与工程优化问题（如电力系统中的机会约束优化）涉及非凸约束集。在这类集合上执行投影或查询边界往往计算成本极高，成为制约优化效率的主要瓶颈。当约束集满足“球同胚性”（如常见的星形集）时，存在将其拓扑转化为单位球的可能，但如何高效构建并利用这种转化仍然没有满意的解决方案。
- **核心问题**：如何快速、低计算开销地求解定义在非凸、但与球同胚的集合上的带约束优化问题，避免使用昂贵的投影算子或优化预言（oracle）。
- **整体含义**：本文提出了一种基于学习的、投影高效的一阶优化范式，通过可逆神经网络学习同胚映射，将原问题重参数化为球上的等价问题，从而在理论上保证收敛，并在实践中取得数量级的加速。这一思路为解决带复杂几何边界的非凸优化提供了新的工具。

## 2. 论文提出的方法论

### 核心思想
- 借助可逆神经网络（INN）学习一个光滑同胚映射 $f$，将非凸可行域 $ \mathcal{X} $（球同胚集）连续、可逆地映射到单位球 $B$。
- 原问题 $\min_{x \in \mathcal{X}} g(x)$ 被转换为球上的等价问题 $\min_{z \in B} g(f^{-1}(z))$，从而将难处理的非凸边界约束转化为简单的球约束，避免了直接在 $\mathcal{X}$ 上投影或边界检测。

### 关键技术细节
- **同胚学习误差**：设 INN 学习到的映射为 $\hat{f}$，与真实同胚的误差用 $\epsilon_{\rm inn}$ 度量，最终影响近似解的质量。
- **算法流程（Hom-PGD+）**：
  1. 阶段一（学习阶段）：利用采样点训练 INN，使其逼近从 $\mathcal{X}$ 到单位球 $B$ 的同胚映射 Ê。
  2. 阶段二（优化阶段）：在球约束优化问题上使用投影梯度法（或其他一阶方法）迭代，每次迭代仅需向球内投影（投影成本极低），并通过 INN 反变换获得原始空间中的点，用于计算梯度。
- **收敛性保障**：
  - 获得 $\epsilon + \mathcal{O}(\sqrt{\epsilon_{\rm inn}})$-近似平稳点所需的迭代复杂度为 $\mathcal{O}(\epsilon^{-2})$，与无约束光滑非凸优化的梯度复杂度同阶，且在 $\epsilon_{\rm inn}$ 足够小时近似质量可控。
- **计算复杂度**：单次迭代复杂度为 $\mathcal{O}(W)$，其中 $W$ 为 INN 的参数总量，具有线性缩放特性，远低于传统投影所需的 $\mathcal{O}(\text{poly}(d))$ 甚至指数级开销。

## 3. 实验设计

### 使用场景与数据集
- 包含合成非凸可行域（如各类星形集）上的优化问题。
- 一个显著的实际场景是**电力系统中的机会约束优化**，其天然形成星形或球同胚可行域。

### 基准方法与对比对象
- 摘要中提及与“state-of-the-art methods”对比。虽未列具体方法名，但根据问题背景，可能包括基于投影的方法（如交替投影、近似投影）、内点法以及针对特殊集合的专用求解器。
- 指标侧重于收敛速率与每次迭代/整体运行时间，体现速度提升。

### 实验衡量维度
- 收敛到指定精度所需的迭代次数与运行时间。
- Hom-PGD+ 带来最高一个数量级的加速效果。

## 4. 资源与算力

- 摘要及提供的内容**未明确说明**所采用的 GPU 型号、数量、训练时长等具体资源信息。
- 可推测 INN 训练需要一定量的 GPU 计算，但文中并未给出相应细节（如显存、epoch 数、训练时间），这对评估整体成本（尤其是学习阶段的开销）构成信息缺失。

## 5. 实验数量与充分性

- **实验组数**：摘要提到“Extensive numerical experiments”，暗示实验规模较大，覆盖合成与实际任务（机会约束优化）。但未给出具体组数、不同维度、不同可行域形状数量、消融实验等细节。
- **充分性评估**：由于缺乏详细实验配置（如数据集数量、随机种子、超参数选择、对比方法的具体实现），难以完整评估实验的客观性与公平性。不过，从“收敛速率相当、速度提升一个数量级”的表述看，对比应建立在相同精度要求下，有一定说服力。无更多细节时，需谨慎看待其结论的普适性。
- **潜在偏差风险**：无法排除作者在构造对比方法时选择了较弱的投影实现，或仅在特定问题类上获得优势。消融实验（如不同 INN 容量、同胚误差对最终优化质量的影响）未有描述。

## 6. 论文的主要结论与发现

- Hom-PGD+ 能够以 $\mathcal{O}(\epsilon^{-2})$ 的梯度复杂度，找到与同胚学习误差相关的近似平稳点，理论保证有力。
- 在实践中，该方法在收敛速度上与现有最先进方法持平或更优，而计算效率（运行时间）可提升一个数量级。
- 将基于学习的同胚映射引入约束优化，成功实现了“投影几乎免费”，为非凸边界约束优化开辟了高效的新范式。

## 7. 优点

- **方法论亮点**：
  - 巧妙的降维思路：通过同胚将复杂的几何约束转化为简单的球约束，避免了直接处理非凸集的困难。
  - 利用可逆神经网络实现映射，既保证了可微性，又使得前后向计算高效；且 INN 可离线训练，分摊优化成本。
  - 理论严密，给出了与同胚学习误差挂钩的收敛率，提供了对学习精度的可解释需求。
- **实验设计亮点**：
  - 面向实际应用（电力系统机会约束），案例具有代表性，体现了方法在工程约束优化中的潜力。
  - 展示了显著的加速效果，直接触及传统投影方法的痛点。

## 8. 不足与局限

- **理论局限**：
  - 方法有效性高度依赖约束集的**球同胚性**；对于非同胚于球的非凸集（如有孔洞的集合）无法直接应用，适用范围受拓扑限制。
  - 最终解的质量带有 $\mathcal{O}(\sqrt{\epsilon_{\rm inn}})$ 的误差项，若 INN 学习不充分（尤其在可行域边界复杂时），影响解的可行性或最优性。
- **实验覆盖不足**：
  - 未给出实验的具体规模、对比方法细节、消融实验等，难以判断结论的可复现性与公平性。
  - 未报告学习 INN 所需的训练代价（算力、数据需求），这可能成为某些轻量级应用的门槛。
- **潜在偏差与应用限制**：
  - INN 的训练依赖于从原始可行域的大量采样，若可行域本身缺乏显式描述，采样困难，学习可能失败。
  - 对于高维复杂星形集，INN 的表达能力与泛化能力尚未充分验证。
  - 仅在机会约束优化这一实际问题上展示，缺少其他领域（如鲁棒控制、安全强化学习）的实证，泛化性待考。

（完）
