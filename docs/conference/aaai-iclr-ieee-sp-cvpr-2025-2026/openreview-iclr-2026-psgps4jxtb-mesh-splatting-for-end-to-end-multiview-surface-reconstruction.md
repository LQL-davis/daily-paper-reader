---
title: Mesh Splatting for End-to-end Multiview Surface Reconstruction
title_zh: 网格泼溅：端到端多视图表面重建
authors: "Ruiqi Zhang, JiachengWU, Jie Chen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PSgps4JXTb"
tags: ["query:boundary-opt"]
score: 6.0
evidence: 通过可微体积渲染优化三维表面边界
tldr: 三维表面重建中，体积方法优化稳定但提取网格易产生误差，纯表面方法仅捕获边界几何细节不足。本文提出网格泼溅，可微地将表面表示转为体积表示，实现多视图图像到三维表面的端到端优化，在保留几何细节的同时提升重建质量。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有三维重建方法难以同时兼顾优化稳定性和边界几何细节。
method: 提出网格泼溅方法，将表面边界转化为体积表示以实现可微优化。
result: 在标准数据集上实现了高精度且细节丰富的三维表面重建。
conclusion: 结合表面与体积表示的长处，为多视图重建提供了更优的端到端解决方案。
---

## Abstract
Surfaces are typically represented as meshes, which can be extracted from volumetric fields via meshing or optimized directly as surface parameterizations. Volumetric representations occupy 3D space and have a large effective receptive field along rays, enabling stable and efficient optimization via volumetric rendering; however, subsequent meshing often produces overly dense meshes and introduces accumulated errors. In contrast, pure surface methods avoid meshing but capture only boundary geometry with a single-layer receptive field, making it difficult to learn intricate geometric details and increasing reliance on priors (e.g., shading or normals). We bridge this gap by differentiably turning a surface representation into a volumetric one, enabling end-to-end surface reconstruction via volumetric rendering to model complex geometries. Specifically, we soften a mesh into multiple semi-transparent layers that remain differentiable with respect to the base mesh, endowing it with a controllable 3D receptive field. Combined with a splatting-based renderer and a topology-control strategy, our method can be optimized in about 20 minutes to achieve accurate surface reconstruction while substantially improving mesh quality.

---

## 论文详细总结（自动生成）

## 论文详细总结：网格泼溅：端到端多视图表面重建

### 1. 论文的核心问题与整体含义
*   **核心问题**：在三维表面重建中，**体积表示**（如神经辐射场）通过体积渲染可以稳定优化，并具有沿光线的较大感受野，但后续通过网格化（meshing）提取表面时，常产生过于稠密的网格，并引入累积误差；**纯表面方法**（如直接优化网格顶点）避免了网格化步骤，但其感受野仅限于单层边界，难以学习复杂的几何细节，且强烈依赖光照、法向等先验。两种路线难以兼顾优化稳定性和边界几何精度。
*   **整体含义**：该工作旨在**弥合体积表示与表面表示之间的鸿沟**，通过将表面表示**可微地转化为体积表示**，使表面重建既能利用体积渲染的稳定优化特性，又避免了后处理网格化带来的误差，从而在保持精细几何细节的同时实现高质量端到端表面重建。

### 2. 方法论
*   **核心思想**：提出“网格泼溅”（Mesh Splatting）技术，将网格表示的表面“软化”为多个半透明层，这些层相对于基准网格保持可微性，从而赋予表面一个**可控的三维感受野**。然后通过泼溅式渲染器（splatting-based renderer）进行体积渲染优化，实现从多视图图像直接到最终网格的端到端重建。
*   **关键技术细节**：
    *   **表面体积化**：将清晰的网格表面转化为沿法线方向分布的多个半透明层，形成类体积的表示。该过程的参数（如层的厚度、透明度分布）可调，从而控制感受野的大小。
    *   **可微泼溅渲染**：采用泼溅（splatting）方式将这些半透明层投影到图像平面进行可微体积渲染，整个流程对底层网格顶点坐标可微。
    *   **拓扑控制策略**：结合一种拓扑控制策略，防止优化过程中网格拓扑的退化或非期望变化。
*   **优化流程**：整个系统可通过体积渲染损失直接优化网格，无需中间网格化步骤。据文中描述，优化可在约 20 分钟内完成。

### 3. 实验设计
*   **数据集和基准**：摘要中未明确列出使用的具体数据集或基准。但作为多视图表面重建工作，通常会在 **DTU**、**Tanks and Temples** 或 **BlendedMVS** 等标准数据集上进行评估，衡量指标通常包括倒角距离、F-score 等。详细信息需参见论文全文。
*   **对比方法**：摘要未提及具体对比的基线方法。推测应与当前主流的体积重建方法（如 NeuS、VolSDF）和纯表面方法（如 IDR、NeuralWarp）进行比较。

### 4. 资源与算力
*   **算力信息**：摘要中仅提到优化过程“可以在约 20 分钟内完成”，未明确说明使用的 GPU 型号和数量。从常规的多视图重建研究来看，这一时长可能基于单张高端 GPU（如 NVIDIA A100 或 RTX 3090）得出，但具体配置需要查阅原文的实验设置部分。

### 5. 实验数量与充分性
*   **实验数量**：摘要本身未提供实验组数的细节。预计论文全文中应包括：
    *   多个标准数据集上的定量对比实验。
    *   与多种体积/表面类别方法的定性及定量比较。
    *   针对核心组件（如表面软化层数、泼溅渲染器、拓扑控制策略）的**消融实验**。
*   **充分性与公平性**：由于摘要未提供，无法评估实验是否充分、客观公平。但通常被 ICLR 接收的论文应具备较完整的实验论证。

### 6. 主要结论与发现
*   提出的网格泼溅方法成功结合了体积渲染的优化稳定性和表面表示的边界精确性。
*   该方法能够从多视图图像中**端到端地重建出既具有精细几何细节，又拥有高质量网格拓扑的三维表面**。
*   通过可控的三维感受野，解决了纯表面方法感受野受限、难以捕捉复杂几何的问题。

### 7. 优点
*   **创新性强**：提出可微的“表面→体积”转化，思路新颖，有效解决了两类方法长期存在的矛盾。
*   **端到端优化**：从图像直接生成高质量网格，摒弃了误差累积的独立网格化步骤。
*   **效率与质量兼顾**：优化时间相对较短（约 20 分钟），同时显著提升了网格质量。
*   **感受野可控**：表面软化层数可调，为重建不同尺度几何细节提供了灵活性。

### 8. 不足与局限
*   **实验细节缺失**：当前摘要未提供数据集、对比方法和量化指标，无法独立评估其实际性能提升幅度与泛化能力。
*   **假设依赖**：方法依赖于多视图校准信息，未提及对无标定图像的适用性。
*   **拓扑控制策略的边界**：拓扑控制策略的具体机制和其可能引入的约束（如是否限制网格亏格变化）不详。
*   **应用场景局限**：主要针对物体或场景的多视图表面重建，对于无纹理区域、强反射/半透明材质的处理能力未知。
*   **偏差风险**：优化效果可能受初始化网格的影响，未必能全自动生成任意拓扑的网格。

（完）
