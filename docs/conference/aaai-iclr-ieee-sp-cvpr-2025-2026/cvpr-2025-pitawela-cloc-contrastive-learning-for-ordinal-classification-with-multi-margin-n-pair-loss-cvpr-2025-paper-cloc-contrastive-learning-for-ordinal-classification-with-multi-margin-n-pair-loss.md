---
title: "CLOC: Contrastive Learning for Ordinal Classification with Multi-Margin N-pair Loss"
title_zh: "CLOC: 用于有序分类的多间隔N对损失对比学习"
authors: "Pitawela, Dileepa, Carneiro, Gustavo, Chen, Hsiang-Ting"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Pitawela_CLOC_Contrastive_Learning_for_Ordinal_Classification_with_Multi-Margin_N-pair_Loss_CVPR_2025_paper.pdf"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 为有序分类决策边界优化多级间隔
tldr: 针对有序分类中类别间距重要性不一的问题，提出CLOC方法，利用多间隔n对损失优化不同类别间的间隔，使得模型能更准确地学习有序表示，并在多个有序分类数据集上取得优越性能。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1797, \"height\": 549, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 858, \"height\": 405, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 849, \"height\": 409, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 734, \"height\": 499, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1707, \"height\": 487, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1399, \"height\": 512, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 971, \"height\": 517, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 441, \"height\": 348, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-pitawela-cloc-contrastive-learning-for-ordinal-classification-with-multi-margin-n-pair-loss-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1203, \"height\": 343, \"label\": \"Table\"}]"
motivation: 现有有序分类方法忽视各边界重要性差异。
method: 提出多间隔对比学习损失MMNP以优化不同类别间隔。
result: CLOC在有序分类任务中显著提升性能并保持有序结构。
conclusion: 为有序分类提供了更精细的边界优化方案。
---

## Abstract
In ordinal classification, misclassifying neighboring ranks is common, yet the consequences of these errors are not the same.For example, misclassifying benign tumor categories is less consequential, compared to an error at the pre-cancerous to cancerous threshold, which could profoundly influence treatment choices. Despite this, existing ordinal classification methods do not account for the varying importance of these margins, treating all neighboring classes as equally significant. To address this limitation, we propose CLOC, a new margin-based contrastive learning method for ordinal classification that learns an ordered representation based on the optimization of multiple margins with a novel multi-margin n-pair loss (MMNP).CLOC enables flexible decision boundaries across key adjacent categories, facilitating smooth transitions between classes and reducing the risk of overfitting to biases present in the training data.We provide empirical discussion regarding the properties of MMNP and show experimental results on five real-world image datasets (Adience, Historical Colour Image Dating, Knee Osteoarthritis, Indian Diabetic Retinopathy Image, and Breast Carcinoma Subtyping) and one synthetic dataset simulating clinical decision bias.Our results demonstrate that CLOC outperforms existing ordinal classification methods and show the interpretability and controllability of CLOC in learning meaningful, ordered representations that align with clinical and practical needs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：在有序分类（ordinal classification）中，不同相邻类别之间的错误严重程度并不相同（例如良性肿瘤之间的误分类影响轻微，但癌前→癌性的误分类会显著影响治疗），现有方法却将所有相邻边界视为同等重要。
- **整体含义**：提出 CLOC（Contrastive Learning for Ordinal Classification）方法，通过可学习的多间隔（multi-margin）对每个相邻边界赋予不同的重要性，从而更符合现实需求，提升有序分类的准确性、可解释性和可控性。

### 2. 论文提出的方法论
- **核心思想**：利用对比学习框架，引入**多间隔 n-pair 损失（Multi-Margin N-Pair Loss, MMNP）** 来学习有序表示。该损失通过为不同排名的负样本分配累积间隔，推开排名距离更远的样本，从而在嵌入空间中保持类别间的有序结构。
- **关键技术细节**：
  - 定义 C-1 个可学习的间隔参数 \( m_H = \{ m_h \} \)，其中 \( h \in H \) 表示相邻排名的配对。
  - MMNP 损失形式（对锚点 \( z \) 的所有正样本 \( z_j \) 和负样本 \( z_k \) 计算）：
    \[
    \ell_{MM}(z, y, m_h) = \sum_{(z_j,y_j)\in S^+} \sum_{(z_k,y_k)\in S^-} \max(0, m_{y,y_k} + \cos(z, z_k) - \cos(z, z_j))
    \]
    其中 \( m_{y,y_k} \) 是锚点排名 \( y \) 到负样本排名 \( y_k \) 的累积间隔。
  - **两阶段训练**：
    - **阶段一**：联合学习编码器 \( e_\theta \)、分类器 \( c_\phi \) 和间隔 \( m_H \)，同时优化交叉熵损失与 MMNP 损失；采用 softplus 激活、随机初始化间隔、早停等措施防止间隔坍塌。
    - **阶段二**：冻结间隔，仅微调编码器和分类器，继续优化目标函数以细化表示。
- **公式与约束**：整体目标为
  \[
  \theta^*,\phi^*,m_H^* = \arg\min_{\theta,\phi,m_H} \frac{1}{N} \sum_{(x,y)\in D} \left[ \ell_{CE}(y, \hat{y}) + \ell_{MM}(z, y, m_h) \right] \quad \text{s.t. } m_h > \rho \ \forall h \in H
  \]
  \( \rho \) 为最小间隔约束，用于关键边界的手动控制。

### 3. 实验设计
- **数据集**：
  - 真实图像数据集：Adience（年龄估计，8 类）、Historical Colour Image Dating（HID，5 年代）、Knee Osteoarthritis（KOA，5 级）、Indian Diabetic Retinopathy Image（IDRiD，5 级）、Breast Carcinoma Subtyping（BRACS，7 类）。
  - 合成数据集：模拟临床诊断偏差（对 IDRiD 训练集注入标签噪声）。
- **Benchmark 与方法对比**：
  - 有序分类方法：ORCNN、POE、GOL、MWR、RnC。
  - 对比学习方法：SimCLR、SupCon、DINO、RnC（兼具有序与对比）。
- **评估指标**：准确率（Accuracy）和平均绝对误差（MAE）。

### 4. 资源与算力
- 所有实验在 **单块 RTX 4090 GPU** 上运行。
- 两阶段各训练最多 500 个 epoch，使用早停（阶段一训练准确率达 95% 时停止，阶段二 10 个 epoch 无提升停止）。
- 论文未提及具体训练时长，亦未说明 GPU 数量（推断为单卡）。

### 5. 实验数量与充分性
- **主实验**：在 5 个真实数据集上对比 8 种方法（表 1），全面评估 CLOC 的性能。
- **可控性实验**：固定关键边界的间隔，观察相邻错误率与整体准确率的变化（表 2），验证人工干预效果。
- **鲁棒性实验**：模拟 IDRiD 数据集上的诊断偏差，对比学习间隔与固定间隔的 CLOC 及其他方法（表 3），显示固定间隔的强鲁棒性。
- **消融实验**（表 4）：
  - 所有间隔固定为 1；
  - 学习单一共享间隔；
  - 学习 C-1 个独立间隔（CLOC 完整版）。
  同时对比两阶段与仅阶段一的效果，验证多间隔独立学习与两阶段策略的必要性。
- **可视化与可解释性实验**（图 5）：UMAP 降维对比 CE、SupCon、RnC 和 CLOC 的嵌入，展示 CLOC 保持有序结构且关键边界间隔更大。
- **额外分析**：间隔学习过程中有无防坍塌措施的影响（图 4），以及大规模类别数、计算时间的讨论（附录）。
- **评价**：实验涵盖广泛数据集、多种基线、控制变量及消融，设计客观公平；指标选择合理，结果充分支持结论。

### 6. 论文的主要结论与发现
- CLOC 在所有数据集上均优于现有有序分类和对比学习方法，尤其在医学相关任务中提升显著。
- 可学习的多间隔使不同相邻边界具备差异化重要性，最大间隔常出现在关键决策边界（如 IDRiD 的 grade1↔grade2）。
- 通过固定关键边界的间隔，可以有效降低该边界的错误率（代价为整体准确率轻微下降），体现了可控性。
- 在模拟偏差场景下，固定临界间隔的 CLOC 显著比所有对比方法更具鲁棒性。
- 多间隔学习提升了表示的可解释性，为理解模型决策边界提供了定量工具。

### 7. 优点
- **新颖的损失设计**：MMNP 通过累积间隔自然保持有序结构，同时为每个相邻边界赋予独立可学习的重要性，优于固定间隔或单一间隔的方法。
- **灵活且可控**：支持手动设置关键边界间隔，能适应高风险场景对特定错误的优先抑制。
- **可解释性强**：学习到的间隔值可直接反映类间分离难度，提供决策洞察。
- **两阶段训练**：有效防止间隔坍塌，保证训练稳定。
- **全面的实验验证**：覆盖多个领域数据集、多种基线，并包含可控性、鲁棒性、消融和可视化分析。

### 8. 不足与局限
- **大规模类别数时的性能下降**：论文指出在类别极多的数据集上性能不如预期（附录），可能由于优化过多间隔的复杂性增加。
- **计算时间未明确对比**：虽提及优于某些方法，但未给出所有方法的训练时间量化比较。
- **可解释性的局限**：间隔值本身可能受视觉特征显著度、标注者谨慎程度等混杂因素影响，不能单独作为因果关系解释，仅作为辅助解释工具。
- **仅限图像数据**：实验集中在图像数据集，未验证在表格或文本有序任务上的适用性。
- **手动调节的局限性**：需要领域知识预先指定关键边界，并非完全自动化。
- **训练策略依赖**：对间隔初始化和早停策略敏感，可能需针对新任务进行调整。

（完）
