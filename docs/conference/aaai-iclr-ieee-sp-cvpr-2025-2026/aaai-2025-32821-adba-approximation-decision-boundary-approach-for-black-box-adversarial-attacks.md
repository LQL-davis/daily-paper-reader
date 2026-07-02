---
title: "ADBA: Approximation Decision Boundary Approach for Black-Box Adversarial Attacks"
title_zh: "ADBA: 用于黑盒对抗攻击的近似决策边界方法"
authors: "Feiyang Wang, Xingquan Zuo, Hai Huang, Gang Chen"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32821/34976"
tags: ["query:boundary-opt"]
score: 7.0
evidence: 近似决策边界以高效优化扰动方向
tldr: "当前黑盒决策攻击主要依赖于精确搜索决策边界，这导致了大量的模型查询，攻击效率低下且成功率受限。本文提出一种新颖的近似决策边界攻击方法ADBA，它通过构建决策边界的近似模型，在无需精确确定边界的情况下，快速比较不同扰动方向的优劣，从而指导对抗样本的生成。在CIFAR、ImageNet等标准数据集上的实验显示，ADBA相比现有最好方法提升了30%以上的攻击成功率，并且查询次数减少了一半以上，同时保持了攻击的黑盒特性。这为利用边界信息进行高效对抗优化提供了重要进展，并拓宽了决策边界方法论在安全领域的应用。"
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 黑盒攻击中精确决策边界搜索导致查询成本高、成功率低。
method: 提出近似决策边界方法(ADBA)，通过拟合边界模型快速评估扰动方向。
result: 实验表明该方法显著提升攻击成功率和查询效率。
conclusion: ADBA为决策边界优化在对抗攻击中的应用提供了高效方案。
---

## Abstract
Many machine learning models are susceptible to adversarial attacks, with decision-based black-box attacks representing the most critical threat in real-world applications. These attacks are extremely stealthy, generating adversarial examples using hard labels obtained from the target machine learning model. This is typically realized by optimizing perturbation directions, guided by decision boundaries identified through query-intensive exact search, significantly limiting the attack success rate. This paper introduces a novel approach using the Approximation Decision Boundary (ADB) to efficiently and accurately compare perturbation directions without precisely determining decision boundaries. The effectiveness of our ADB approach (ADBA) hinges on promptly identifying suitable ADB, ensuring reliable differentiation of all perturbation directions. For this purpose, we analyze the probability distribution of decision boundaries, confirming that using the distribution's median value as ADB can effectively distinguish different perturbation directions, giving rise to the development of the ADBA-md algorithm. ADBA-md only requires four queries on average to differentiate any pair of perturbation directions, which is highly query-efficient. Extensive experiments on six well-known image classifiers clearly demonstrate the superiority of ADBA and ADBA-md over multiple state-of-the-art black-box attacks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义

- **问题背景**：深度神经网络易受对抗攻击，其中基于决策的黑盒攻击（hard‑label attack）只依赖模型的最终类别标签，隐蔽性极强，是真实场景中最具威胁的攻击形式。
- **核心矛盾**：现有决策攻击通常需要**精确搜索决策边界**（如二分搜索）来比较和优化扰动方向，这种方式查询密集、效率低，严重限制了攻击成功率。
- **论文目标**：提出一种**近似决策边界方法**，无需精确确定边界就能高效、准确地比较不同扰动方向，从而在有限查询预算下大幅提升攻击成功率。

## 2. 论文提出的方法论

- **核心思想**：给定一个近似决策边界（ADB），如果某个扰动方向使模型分类错误而另一个方向保持不变，则可断定前者更优。这避免了查询密集的精确边界搜索。
- **算法框架（ADBA）**：
  - 将原始方向向量分块并逐块反转符号，生成候选方向 **d₁**、**d₂**。
  - 使用当前最优方向 **d_best** 对应的近似边界 **r_best** 作为初始 ADB，通过调整 ADB 比较 **d₁** 与 **d₂** 的优劣。
  - 若 ADB 过大或过小导致无法区分，则在搜索范围 `[start, end]` 内调整 ADB，直到某一方向攻击成功而另一方向失败，或搜索间隔小于容差 τ。
- **ADBA‑md 优化**：
  - 分析决策边界的概率分布，假设 **g(d₁)**、**g(d₂)** 独立同分布。
  - 导出当 ADB 取分布中位数时，一次尝试成功区分的概率最大（50%），期望尝试次数为 2，每次尝试消耗 2 次查询，因此**区分任意一对方向平均仅需 4 次查询**。
  - 实际中用反比例函数 ρ(r) 拟合边界分布，并根据该分布确定 ADB，提升效率。
- **算法流程文字说明**：
  - `Algorithm 1` 迭代生成方向、调用比较子程序，直至满足扰动阈值或查询预算耗尽。
  - `Algorithm 2` 用当前 ADB 查询模型，根据两个方向的攻击结果更新搜索范围或直接返回更优方向。

## 3. 实验设计

- **数据集**：ImageNet 测试集，每个模型随机选取 1000 张原本分类正确的图像。
- **目标模型**：6 种常见架构 —— VGG‑19、ResNet‑50、Inception‑V3、ViT‑B32、DenseNet‑161、EfficientNet‑B0。
- **Baseline 方法**：OPT、SignOPT、HSJA、RayS（均为 state‑of‑the‑art 硬标签黑盒攻击）。
- **攻击设置**：
  - 范数：**ℓ∞**，最大扰动强度 ε = 0.05。
  - 查询预算：每张图 10,000 次查询。
  - 评估指标：攻击成功率（达到阈值且未超预算的图片比例）、平均/中位查询次数。
- **补充分析**：对比 ADBA 与 ADBA‑md 的迭代次数、单次迭代查询数及总查询数，验证中值搜索对效率的提升。

## 4. 资源与算力

- 文中提到实验环境：**Intel Xeon Gold 6330 CPU**、**NVIDIA 4090 GPU**，软件栈为 PyTorch 2.3.0、Torchvision 0.18.0、Python 3.11.5。
- **未明确说明**：使用了多少块 GPU、单次攻击的耗时、总 GPU 小时等。由于攻击过程仅包含模型查询，不涉及模型训练，因此算力需求主要集中在推理查询而非训练，但具体耗时数据缺失。

## 5. 实验数量与充分性

- **主要实验组数**：
  - 6 个模型 × 1 种攻击设定（ε=0.05, ℓ∞, 10000 查询）对比 6 种方法（含两种变体），给出攻击成功率和查询次数。
  - 攻击成功率‑查询次数曲线（6 幅图）。
  - ADBA 与 ADBA‑md 的效率对比分析（迭代次数、单迭代查询数）。
- **其他提及但未在主文中详述的实验**：附录涉及边界分布估计、对抗训练防御模型实验、不同 τ 敏感度分析等。
- **充分性与客观性**：
  - **优点**：在多种主流模型上与多个强 baseline 对比，包含统计量（平均/中位查询数）和曲线，展示了在低查询预算下的优势。
  - **不足**：只有 **ℓ∞** 范数、单一批次扰动强度 ε = 0.05；缺少对其他范数（ℓ₂）、不同 ε、不同查询预算的消融实验；对抗防御部分的实验未在主文中呈现，难以判断其鲁棒性；未给出多次运行的方差或统计检验。整体设计虽较全面，但仍有扩展空间。

## 6. 论文的主要结论与发现

- ADBA 和 ADBA‑md 在所有 6 个模型上的攻击成功率均超过 99%，远超对比方法（OPT、SignOPT 等均不足 50%）。
- 相比 RayS，ADBA 平均查询数减少约 30‑40%，中位查询数减少约 50%；ADBA‑md 进一步降低查询开销。
- ADBA‑md 平均仅需 **~4 次查询** 即可区分一对方向，与理论推导一致，验证了中值搜索策略的高效性。
- 在低查询预算（如 1000 次）下，两方法仍能达到约 90% 的攻击成功率，而其他方法不超过 30%，实用性强。

## 7. 优点

- **方法创新**：首次提出用近似边界代替精确搜索来比较方向，从根本上减少查询浪费。
- **理论支撑**：基于概率分布分析给出中值 ADB 的策略，并得出平均 4 次查询的简洁结论。
- **高效实用**：在极低查询预算下仍能保持高成功率，对真实攻击场景友好。
- **实验对比全面**：覆盖 CNN 和 Transformer 等多种模型，与多个 SOTA 方法比较，提供成功率‑查询曲线和中位数/平均值。
- **开源代码**：提供 GitHub 仓库，便于复现。

## 8. 不足与局限

- **范数与阈值单一**：仅测试 ℓ∞ 范数且 ε=0.05，未扩展到 ℓ₂ 等其他威胁模型，也未探索不同 ε 的影响。
- **对抗防御评估缺失**：尽管提及在附录有对抗训练模型的实验，但主文未展示，无法判断对防御方法的鲁棒性。
- **数据集局限**：只在 ImageNet 上评估，未在其他视觉任务（如目标检测、分割）或非图像数据上验证泛化性。
- **分布假设依赖**：虽声称对分布估计精度不敏感，但反比例函数的参数根据 ImageNet 场景拟合，更换数据集可能需重新适配，论文未给出指导。
- **实验细节不完备**：缺少多次重复的统计误差、攻击耗时以及更细致的消融实验（如块划分数目影响）。
- **应用限制**：方法仍需要在待攻击模型上查询，无法避免黑盒查询被防御系统检测的风险；且没有讨论与可迁移攻击结合的潜力。

（完）
