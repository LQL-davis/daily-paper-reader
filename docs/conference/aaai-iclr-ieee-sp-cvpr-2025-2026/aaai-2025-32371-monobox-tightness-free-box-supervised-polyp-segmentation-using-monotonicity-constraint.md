---
title: "MonoBox: Tightness-Free Box-Supervised Polyp Segmentation Using Monotonicity Constraint"
title_zh: "MonoBox: 基于单调性约束的无紧密框监督息肉分割"
authors: "Qiang Hu, Zhenyu Yi, Ying Zhou, Fan Huang, Mei Liu, Qiang Li, Zhiwei Wang"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/32371/34526"
tags: ["query:boundary-opt"]
score: 4.0
evidence: 单调性约束优化跨边界的分割响应
tldr: 传统框监督分割方法假设框边缘紧密贴合目标边界，这在实际标注中难以满足，导致性能下降。本文提出MonoBox，利用单调性约束取代传统多示例学习损失，强制从前景到背景的响应沿特定方向单调递减。在息肉分割数据集上的实验表明，MonoBox在不精确标注框下仍能精确分割目标，性能显著优于现有框监督方法，且接近全监督水平。该工作为边界不精确的弱监督分割提供了新的优化思路。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 框监督分割中紧密框假设不现实，导致边界定位不准。
method: 提出单调性约束替代多实例学习损失，优化边界处的响应变化。
result: 在息肉分割上，MonoBox在不精确标注下性能接近全监督。
conclusion: MonoBox为弱标注下的边界优化提供了有效方案。
---

## Abstract
We propose MonoBox, an innovative box-supervised segmentation method constrained by monotonicity to liberate its training from the user-unfriendly box-tightness assumption. In contrast to conventional box-supervised segmentation, where the box edges must precisely touch the target boundaries, MonoBox leverages imprecisely-annotated boxes to achieve robust pixel-wise segmentation. The 'linchpin' is that, within the noisy zones around box edges, MonoBox discards the traditional misguiding multiple-instance learning loss, and instead optimizes a carefully-designed objective, termed monotonicity constraint. Along directions transitioning from the foreground to background, this new constraint steers responses to adhere to a trend of monotonically decreasing values. Consequently, the originally unreliable learning within the noisy zones is transformed into a correct and effective monotonicity optimization. Moreover, an adaptive label correction is introduced, enabling MonoBox to enhance the tightness of box annotations using predicted masks from the previous epoch and dynamically shrink the noisy zones as training progresses. We verify MonoBox in the box-supervised segmentation task of polyps, where satisfying box-tightness is challenging due to the vague boundaries between the polyp and normal tissues. Experiments on both public synthetic and in-house real noisy datasets demonstrate that MonoBox exceeds other anti-noise state-of-the-arts by improving Dice by at least 5.5% and 3.3%, respectively.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- 框监督分割（BSS）方法大多依赖“紧密框假设”（box-tightness assumption），即要求标注框的边缘必须精确贴合目标边界。
- 在息肉分割等医学影像任务中，由于息肉边界模糊、与正常组织对比度低、尺寸小，临床医生难以画出紧密的框，导致大量非紧密（噪声）标注。
- 现有基于多示例学习（MIL）的 BSS 方法对框的紧密性高度敏感，噪声会错导区级标签，使分割模型性能显著退化。
- 本文旨在提出一种脱离紧密框假设的框监督分割方法，使模型能够利用不精确的框标注，仍实现稳健的像素级息肉分割。

## 2. 论文提出的方法论
### 整体思路
- 将框边缘附近的区域定义为“非置信区域”（unconfident regions），其余为“置信区域”。
- 置信区域沿用传统的 MIL 一致性约束；非置信区域则舍弃易被噪声误导的 MIL 损失，代之以一种软性但可靠的**单调性约束（Monotonicity Constraint, MC）**。
- 同时引入**标签校正（Label Correction, LC）**，利用上一轮预测的掩码动态改善框的紧密性，并随着训练推进逐步收缩非置信区域。

### 关键技术细节
- **代理图（Proxy map）优化**：使用 `p[i,j] = max(m[i,:]) * max(m[:,j])` 将分割图解耦为与框填充掩码同形态的代理图，使框级约束可直接作用。
- **区域划分**：根据框的宽高和非置信尺度 λ，生成四个方向（左、右、上、下）的非置信区域掩码 `R_lu, R_ru, R_tu, R_bu`，其余部分为置信区域 `R_c`。
- **单调性约束**：
  - 在非置信区域，沿从框内向框外的方向，要求代理图的响应单调递减（即一阶导数不应大于零）。
  - 构造一阶差分核（如 `[1, -1]^T` 等）分别作用于四个方向，计算梯度图，并通过 `max(p'_x-, 0)` 等形式作为损失，约束正向梯度为 0。
  - MC 损失为四个方向约束之和：`L_MC = L_l_MC + L_r_MC + L_t_MC + L_b_MC`。
- **标签校正**：
  - 每 t 个 epoch，将模型预测的掩码转化为连通域的最小外接框，与原始 GT 框按 IoU≥τ 匹配，合并以得到更紧的修正框。
  - 匹配不上的框保持原样；每执行一次校正，将非置信尺度 λ 减半，逐步减小噪声区域。
- 总体损失：置信区使用 Dice 一致性损失，非置信区使用 MC 损失，二者联合优化。

## 3. 实验设计
### 数据集
- **公开合成噪声数据集**：由 ClinicDB、Kvasir-SEG、ColonDB、EndoScene、ETIS 组成。训练集包含 550 张（ClinicDB）和 900 张（Kvasir-SEG），其余为测试集。将原始掩码转为紧密框，再根据 `σ=0.2` 的高斯噪声随机平移和缩放坐标，模拟非紧密框。
- **院内真实噪声数据集**：18,656 张结肠镜图像，训练集 17,350 张（仅框标注），测试集 1,306 张（两位专家级像素标注）。框标注来自临床医生，自然呈现不紧密模式。

### 评估指标与基准
- 通过 0.5 阈值二值化获取二值掩码，计算 Dice、IoU、豪斯多夫距离（HD）。
- 对比方法均为抗噪 SOTA：SSD-Det、NCE+RCE、PolarT、FSRM。
- 基础 BSS 骨干网络：WeakPolyp 和 IBoxCLA，同时报告使用干净数据训练的上界（UB）和直接用噪声数据训练的下界（LB）。

## 4. 资源与算力
- 使用 **单个 NVIDIA GeForce RTX 3090 GPU**（24 GB 显存）进行训练。
- 优化器：AdamW，学习率与权重衰减均为 0.0001。
- 输入分辨率：352×352，batch size 16。
- 总训练 epoch 数：50，期间每 10 epoch 执行一次标签校正（共校正 4 次）。
- 论文未提及总训练时长，但明确给出了上述硬件配置和训练超参数。

## 5. 实验数量与充分性
- **主对比实验**：在真实噪声和合成噪声两个数据集上，与 4 种抗噪方法在两种 BSS 骨干网上进行完整对比（表 1），并可视化结果（图 4）。
- **消融实验**：针对单调性约束（MC）和标签校正（LC）进行 2×2 组合消融，验证各组件有效性（表 2），并结合可视化（图 5）。
- **非置信区域策略对比**：将 MC 与 “直接排除该区域” 和 “使用软标签” 两种策略对比（表 3），证明 MC 的优越性。
- **超参数分析**：对非置信尺度 λ、匹配 IoU 阈值 τ、校正间隔 t 分别在真实数据集上进行调参（表 4）。
- **噪声水平鲁棒性实验**：在 σ=0.1~0.4 的合成噪声下评估方法稳定性并与其他方法比较（图 7）。
- **通用性实验**：在 COCO 自然图像数据集上，以 BoxInst 为骨干，验证方法对自然场景和其他 MIL 框架的可扩展性（表 5）。
- 实验覆盖消融、对比、鲁棒性和跨领域验证，数量充分，对比方法选取合理，评估公平。

## 6. 论文的主要结论与发现
- MonoBox 有效解放了现有 BSS 方法对框紧密性的依赖，在非紧密框下显著提升分割精度。
- 单调性约束能够将不可靠的边界监督转化为可靠的单调递减优化，消除因过宽框造成的“刺状”误分割。
- 标签校正与单调性约束形成正向循环：MC 提供更准的预测，进而帮助 LC 提升框的紧密度，再反过来增强训练信号。
- 在合成噪声和真实噪声数据集上，MonoBox 均大幅超越其他抗噪方法（Dice 分别提升至少 5.5% 和 3.3%），且性能接近全监督上界。
- 方法可作为即插即用模块，轻松适配现有 MIL 框架的框监督分割方法。

## 7. 优点（方法与实验设计亮点）
- **创新约束**：利用前景到背景响应的单调性，无需依赖框紧密性，为边界不精确场景提供了有效监督范式。
- **模块化与通用性**：基于代理图优化，可方便地扩展到其他 MIL 方案（如投影损失），且在不同骨干网络和自然图像上均有效。
- **动态机制**：标签校正和非置信区域收缩机制使训练过程逐渐自适应，减少噪声影响。
- **贴近临床**：直接针对真实标注中普遍存在的非紧密框问题，在院内真实数据集上验证，应用价值高。
- **实验扎实**：对比方法众多，消融完整，噪声水平鲁棒性分析充分，且包含可视化定性分析。

## 8. 不足与局限
- **固定非置信尺度**：当前对所有样本使用统一的 λ 和统一的收缩比例，未能根据图像内容自适应调整噪声区域宽度，可能影响对极度不规则框的适应性。
- **依赖超参数**：虽然调参实验展示了稳定性，但 λ、τ、t 仍需手动设定，对数据分布敏感。
- **数据集局限**：公开数据集的“非紧密框”通过高斯噪声合成，可能与真实临床标注的噪声模式存在差异；院内真实数据集仅来自单一中心，泛化性有待多中心验证。
- **仅验证息肉分割**：尽管在 COCO 上做了泛化验证，但核心场景聚焦于息肉，在其它复杂任务（如多类、小目标）上的鲁棒性需进一步检验。
- **计算代价未量化**：未披露单次训练时长，虽然轻量但实际部署的时间开销不够明确。

（完）
