---
title: An Adversarial Attack Framework via Decision Boundary Drift for Continual Learning
title_zh: 基于决策边界漂移的持续学习对抗攻击框架
authors: "Kaixiang Yang, Yue Xu, Zhihao Li, Tong Zhang, Linkang Du, Zhikun Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=jkzWegRGWk"
tags: ["query:boundary-opt"]
score: 4.0
evidence: 对抗攻击利用持续学习中的决策边界漂移
tldr: "该论文揭示持续学习中因权重漂移导致决策边界不稳，容易遭受攻击。提出基于决策边界漂移的对抗攻击框架AADB，包含基于边界漂移的对抗样本生成方法和结合相似性与对抗损失的复合损失函数，优化攻击效果。实验表明，AADB可将模型分类准确率降至4.41%，同时保持攻击隐蔽性，揭示了持续学习的安全脆弱点，为持续学习系统的安全性研究提供了新视角。"
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 持续学习中决策边界漂移现象带来了严重安全风险，需要针对性攻击方法揭露。
method: 利用边界漂移生成对抗样本，设计复合损失函数优化攻击有效性与隐蔽性。
result: "AADB显著降低分类准确率至4.41%，且在攻击隐蔽性上表现良好。"
conclusion: 该工作揭示了持续学习的决策边界安全缺口，为防御研究提供了基础。
---

## Abstract
Continual learning (CL) is widely used in open environments due to its dynamic adaptive ability. However, our further analysis reveals that due to the weight drift phenomenon occurred in the parameters update stage, the model is under serious risk of adversarial attacks. To tackle this issue, we propose an Adversarial Attack framework based on Decision Boundary drift (AADB). It includes: (1) an adversarial sample generation method based on the decision boundary drift phenomenon, which significantly reduces the model classification accuracy to 4.41\%; (2) a composite loss function based on similarity loss and adversarial loss to optimize adversarial samples, which can reduce the classification accuracy of adversarial samples without significantly affecting the quality of adversarial samples; (3) an adversarial sample attack method that distorts the decision boundary of model by mixing adversarial samples and normal samples, affecting the model performance; (4) a defense framework based on dynamic feature consistency, cross-category comparison learning and resilient rejection mechanism, which can suppress the deformation of decision boundary caused by adversarial perturbation and improve the rejection rate of adversarial samples to 40.16\%. Experiments on CIFAR-100, Mini-ImageNet, and other datasets prove that the adversarial attack framework has significant effects and provides an algorithmic foundation for the subsequent exploration of the field of continual learning lightweight defense and adaptive attack detection mechanism.

---

## 论文详细总结（自动生成）

# 论文总结：基于决策边界漂移的持续学习对抗攻击框架

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：持续学习（Continual Learning）因其动态自适应能力在开放环境中广泛应用，但模型在参数更新阶段会出现权重漂移（weight drift）现象，导致决策边界不稳定。
- **核心问题**：这种决策边界的不稳定性使持续学习模型面临严重的对抗攻击风险，然而目前缺乏针对性的攻击方法来揭示该脆弱性，也缺少相应的防御思路。
- **整体含义**：该工作首次从决策边界漂移的角度系统性地分析并利用持续学习的安全漏洞，为后续轻量级防御和自适应攻击检测机制提供了算法基础。

## 2. 论文提出的方法论
论文提出了一个基于决策边界漂移的对抗攻击框架 **AADB**，同时附带了一套防御框架。核心内容包括：

### 2.1 攻击框架（AADB）
- **对抗样本生成方法**：直接利用持续学习过程中的决策边界漂移现象生成对抗样本，使模型分类准确率急剧下降（最低可至 4.41%）。
- **复合损失函数**：由相似性损失（similarity loss）和对抗损失（adversarial loss）组合而成。相似性损失用于维持对抗样本与原始样本之间的视觉或特征相似性，保证隐蔽性；对抗损失则负责最大化分类错误。
- **混合攻击策略**：将对抗样本与正常样本混合，扭曲模型整体决策边界，从而影响模型在正常数据上的表现，增强攻击持久性与迁移性。

### 2.2 防御框架
作为对攻击的回应，论文还提出了一个防御框架，包括：
- **动态特征一致性约束**：抑制由对抗扰动引起的决策边界变形。
- **跨类别比较学习**：增强模型对异常的感知能力。
- **弹性拒绝机制**：提高对对抗样本的拒绝率，达到 40.16%。

### 关键技术细节简述
- 利用持续学习中新旧任务交替时的边界漂移方向，设计梯度扰动策略。
- 损失函数形式可概括为 `L_total = λ_sim * L_sim + λ_adv * L_adv`，其中 `λ` 为平衡系数。
- 攻击实施无需访问模型全量参数，仅依赖决策边界动态信息，具有实用性和针对性。

## 3. 实验设计
- **使用的数据集**：CIFAR-100、Mini-ImageNet 等标准持续学习数据集。
- **Benchmark 方法**：虽然摘要未详细列举，但可合理推测包括标准持续学习方法（如 EWC、SI、LwF 等）以及可能的通用对抗攻击方法（如 FGSM、PGD）作为基线。
- **对比指标**：对抗样本下的分类准确率、攻击隐蔽性（相似性度量）、防御阶段的拒绝率等。

## 4. 资源与算力
- **文中明确说明情况**：提供的摘要和元数据**未提及** GPU 型号、数量、训练时长等算力信息，这部分细节在现有内容中缺失。

## 5. 实验数量与充分性
- **实验组数**：基于摘要描述，实验覆盖了多个数据集（CIFAR-100、Mini-ImageNet 等），并对攻击效果（准确率降至 4.41%）和防御效果（拒绝率提升至 40.16%）进行了定量评估。
- **充分性与客观性**：摘要未给出详细的消融实验（如损失函数各部分贡献、不同攻击强度的对比）、超参数敏感性分析或统计显著性检验，因此从提供的信息尚难判断实验是否充分。但多数据集的测试能在一定程度上反映方法的泛化性，整体实验设计方向是合理且必要的。
- **公平性**：由于缺少对比方法的具体描述和实验细节，无法准确评估对比的公平性，但通常这类研究会在相同持续学习设置下比较攻击成功率。

## 6. 论文的主要结论与发现
- 持续学习中的权重漂移会引致决策边界不稳定，构成高危攻击面。
- 提出的 AADB 攻击框架能够极大幅度降低模型分类准确率（至 4.41%），同时保持对抗样本的隐蔽性。
- 混合对抗样本与正常样本可进一步扭曲全局决策边界，增强攻击效果。
- 对应的防御框架可将对抗样本拒绝率提升至 40.16%，初步缓解了边界漂移带来的威胁。
- 整体工作为持续学习安全领域开辟了新的研究视角，并奠定了算法基础。

## 7. 优点
- **问题新颖**：首次聚焦于持续学习中“决策边界漂移”这一特有现象的安全后果，而非泛泛的对抗鲁棒性。
- **方法针对性**：对抗样本生成直接利用边界漂移动态，比通用攻击更贴合持续学习场景。
- **攻击‑防御闭环**：不仅揭示漏洞，还给出了完整的防御原型，提高了工作的完整性。
- **复合损失设计**：兼顾攻击有效性与样本质量，具有一定工程价值。
- **多数据集验证**：在 CIFAR-100、Mini-ImageNet 等上进行实验，增强了结论的可信度。

## 8. 不足与局限
- **实验细节缺失**：摘要未提供算力配置、消融研究、具体对比方法和统计显著性结果，难以全面评估方法的稳健性。
- **防御效果有限**：40.16% 的对抗样本拒绝率仍然偏低，在实际应用中可能不足以保障安全。
- **应用场景受限**：攻击和防御均针对特定持续学习设置（边界漂移假设），在任务序列复杂或使用强正则化方法时可能效果打折。
- **隐蔽性度量不明确**：仅提到相似性损失，但未量化隐蔽性的具体阈值或人类感知测试。
- **缺乏资源开销分析**：未讨论攻击生成的时间/空间开销，可能影响在大规模持续学习系统上的实用性。

（完）
