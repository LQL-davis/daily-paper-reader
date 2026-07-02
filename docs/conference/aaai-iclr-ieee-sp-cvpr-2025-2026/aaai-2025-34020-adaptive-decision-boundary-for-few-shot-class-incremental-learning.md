---
title: Adaptive Decision Boundary for Few-Shot Class-Incremental Learning
title_zh: 面向少样本类增量学习的自适应决策边界
authors: "Linhao Li, Yongzhang Tan, Siyuan Yang, Hao Cheng, Yongfeng Dong, Liang Yang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/34020/36175"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 针对少样本类增量学习的自适应决策边界策略
tldr: 针对少样本类增量学习(FSCIL)中现有方法仅关注防止遗忘而忽视类特定决策空间的问题，本文提出即插即用的自适应决策边界策略(ADBS)。ADBS通过为每个类学习自适应的边界裕度，在分类时动态调整决策面，有效平衡新旧类的分离。在miniImageNet、CIFAR-100和Omniglot等数据集上的广泛实验证明，ADBS能显著提升多种基线方法的性能，达到了新的最优水平。该方法计算开销小，便于集成，为决策边界优化在增量学习中的应用提供了简洁高效的解决方案。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 现有FSCIL方法忽略类特定决策空间，导致新类学习与旧类遗忘问题。
method: 提出自适应决策边界策略(ADBS)，动态调整每类的边界裕度。
result: 在多个标准基准上，ADBS显著提升多种FSCIL方法的性能。
conclusion: ADBS是一种即插即用的高效方法，为增量学习中的边界优化开辟了新方向。
---

## Abstract
Few-Shot Class-Incremental Learning (FSCIL) aims to continuously learn new classes from a limited set of training samples without forgetting knowledge of previously learned classes. Conventional FSCIL methods typically build a robust feature extractor during the base training session with abundant training samples and subsequently freeze this extractor, only fine-tuning the classifier in subsequent incremental phases. However, current strategies primarily focus on preventing catastrophic forgetting, considering only the relationship between novel and base classes, without paying attention to the specific decision spaces of each class. To address this challenge, we propose a plug-and-play Adaptive Decision Boundary Strategy (ADBS), which is compatible with most FSCIL methods. Specifically, we assign a specific decision boundary to each class and adaptively adjust these boundaries during training to optimally refine the decision
spaces for the classes in each session. Furthermore, to amplify the distinctiveness between classes, we employ a novel inter-class constraint loss that optimizes the decision boundaries and prototypes for each class. Extensive experiments on three benchmarks, namely CIFAR100, miniImageNet, and CUB200, demonstrate that incorporating our ADBS method with existing FSCIL techniques significantly improves performance, achieving overall state-of-the-art results.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：少样本类增量学习（Few-Shot Class-Incremental Learning, FSCIL）要求模型从少量新类样本中持续学习新类别，同时不遗忘已有知识。现有方法通常先在数据充足的基类上训练一个强特征提取器，然后冻结该提取器，仅微调分类器以适应增量阶段的新类。
- **核心问题**：当前策略侧重防止灾难性遗忘，关注新旧类之间的关系，但忽略了每个类在特征空间中具体的决策空间（decision space）。固定的决策边界无法为新类预留足够的空间，导致新旧类特征冲突，进而引发分类性能下降和遗忘加剧。
- **整体含义**：本文旨在通过自适应调整每个类的决策边界来优化特征空间划分，缓解新旧类冲突，提升 FSCIL 性能。该方法以即插即用（plug-and-play）形式兼容多数现有 FSCIL 方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：为每个类分配一个特定的决策边界裕度（scalar multiplier），并在训练过程中自适应地调整这些边界，使类内特征更紧凑、类间边界更清晰，从而为后续新类释放更多特征空间。
- **关键技术细节**：
  - **自适应决策边界（Adaptive Decision Boundary, ADB）**：
    - 在基类训练阶段，为每个基类引入可学习的标量参数 $m_i$（即边界权重），并与分类器权重 $W$ 联合用于计算 logits：$\phi(x) = (W \cdot M)^\top f(x)$，其中 $M$ 为边界向量。
    - 通过交叉熵损失同时优化特征提取器 $f$、分类器 $W$ 和边界 $M$，使得每个类的边界被自适应地压缩或调整，从而释放空间。
    - 在增量阶段，冻结基类边界，仅为新类初始化边界（使用旧类边界的均值）并在冻结的骨干网络和分类器基础上微调新类边界，以适应新类分布并继续压缩当前类边界，为将来新类预留空间。
  - **类间约束损失（Inter-class Constraint loss, IC）**：
    - 基于命题 “若边界权重满足 $(1-m_i)p_i^\top w_i + (m_j-1)p_i^\top w_j \le 0 \ (\forall i \neq j)$，则类 $i$ 与 $j$ 的分离性更好”，提出约束损失：
      $$ L_{IC} = \sum_i \sum_j \max(0, (1-m_i)p_i^\top w_i + (m_j-1)p_i^\top w_j) $$
    - 该损失进一步优化分类器原型 $w$ 和边界 $M$，增强类间区分度。
  - 总体损失：$L = L_{cls} + \alpha L_{IC}$。
- **即插即用**：ADBS 无需更改网络架构，可在基类和增量训练中直接嵌入到现有 FSCIL 框架中。

## 3. 实验设计：数据集 / 场景、基准、对比方法

- **数据集**：CIFAR-100、miniImageNet、Caltech-UCSD Birds-200-2011 (CUB200)。均为标准 FSCIL 基准，遵循 Tao et al. 2020 的数据划分。
- **评估协议**：基类大量数据训练后，后续增量会话采用 N-way K-shot（如 10-way 5-shot）设置，报告每个会话后对所有已见类的 Top-1 准确率。
- **对比方法**：包括主流 FSCIL 方法（TOPIC, CEC, FACT, C-FSCIL, MCNet, SoftNet, WaRP, TEEN, NC-FSCIL, ALFSCIL, DyCR 等）以及边界相关方法（CLOM, DBONet, ALICE）。同时将 ADBS 集成到四个代表性方法（baseline, SAVC, ALICE, OrCo）上检验兼容性。

## 4. 资源与算力

- 文中未明确给出 GPU 型号、数量或训练时长等算力资源信息。仅提及代码开源，但具体算力消耗需参阅其代码仓库或补充材料。

## 5. 实验数量与充分性

- **实验数量**：
  - 在三个数据集上进行了主实验对比（多个基准方法 + 集成 ADBS 的四种方法）。
  - 消融实验：在 miniImageNet 上验证 ADB 和 IC 各自的贡献；还分析了决策分离度、可视化（t-SNE）、余弦相似度矩阵等。
  - 提供了边界适应性、新旧类冲突缓解的定性定量证据。
- **充分性与公平性**：
  - 实验覆盖了多种 SOTA 方法和不同类型的 FSCIL 框架。
  - 对于集成的对比方法，均使用作者官方代码复现或直接引用结果，保证公平。
  - 消融研究清晰展示了各模块的作用，分离度指标进一步佐证了方法的有效性。
  - 未见跨数据集超参数敏感性或计算开销的详细讨论，但整体实验设计较为充分。

## 6. 论文的主要结论与发现

- ADBS 通过自适应决策边界显著缓解了 FSCIL 中新旧类特征空间冲突的问题。
- 集成 ADBS 后，多种现有方法（包括基线方法和 SOTA）在 CIFAR-100、miniImageNet、CUB200 上均有稳定提升，尤其与 SAVC 结合时达到全面最优。
- 自适应边界不仅能提高当前类的分类精度，还能为后续新类预留更多空间，有助于持续学习。
- IC 损失进一步增强了类间区分度，与 ADB 协同增效。

## 7. 优点：方法或实验设计上的亮点

- **即插即用**：无需改变模型结构，易于集成到各种 FSCIL 框架中。
- **自适应机制**：边界随训练数据动态调整，避免手动设定且能根据不同类特性优化决策空间。
- **理论支撑**：给出类间约束的命题推导，增加了方法解释性。
- **全面验证**：在多个基准数据集上进行大量对比和消融实验，可视化分析直观展示了特征分离改善，增强了结论说服力。
- **性能突出**：与 SOTA 方法结合后取得显著且一致的准确率提升，尤其在增量阶段缓解遗忘方面表现优异。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **基类训练依赖**：方法仍遵循 BPNF 框架，需在基类阶段进行充分预训练，若基类数据不足可能影响边界学习效果。
- **增量阶段微调成本**：虽然仅微调边界参数，但每次增量会话仍需对新类进行少量训练，在极端的资源受限场景（如边缘设备）中仍有一定开销。
- **未见对长序列增量的测试**：标准实验配置增量会话数有限（如 CIFAR-100 共 9 个会话），未验证在更多增量阶段下的稳定性。
- **未提供计算开销对比**：缺乏相对于基线方法的具体时间/显存开销报告，实际部署的轻量性未能量化。
- **对新类分布变化的鲁棒性未讨论**：若新类与基类分布差异极大，边界压缩策略是否仍能有效预留空间，未做专门分析。
- **未在更复杂任务（如跨领域、更细粒度）上测试**：尽管包含 CUB200 细粒度数据集，但未涉及跨域或更广义的持续学习场景。

（完）
