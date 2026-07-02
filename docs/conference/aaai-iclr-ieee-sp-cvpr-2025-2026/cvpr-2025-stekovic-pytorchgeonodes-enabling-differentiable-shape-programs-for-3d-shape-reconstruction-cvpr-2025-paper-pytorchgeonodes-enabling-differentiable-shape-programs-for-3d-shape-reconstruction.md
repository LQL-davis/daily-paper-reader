---
title: "PyTorchGeoNodes: Enabling Differentiable Shape Programs for 3D Shape Reconstruction"
title_zh: PyTorchGeoNodes：实现可微形状程序用于三维重建
authors: "Stekovic, Sinisa, Artykov, Arslan, Ainetter, Stefan, D'Urso, Mattia, Fraundorfer, Friedrich"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Stekovic_PyTorchGeoNodes_Enabling_Differentiable_Shape_Programs_for_3D_Shape_Reconstruction_CVPR_2025_paper.pdf"
tags: ["query:boundary-opt"]
score: 7.0
evidence: 梯度优化三维形状参数以重建边界
tldr: 传统形状优化难以同时处理离散和连续参数，本文提出PyTorchGeoNodes，将Blender过程模型解析为可微PyTorch代码，结合遗传算法优化形状程序参数，实现从图像到三维物体重建的端到端可微优化，在保持可解释性的同时达到高精度。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1460, \"height\": 652, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1384, \"height\": 340, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1627, \"height\": 753, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 823, \"height\": 300, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 670, \"height\": 302, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 837, \"height\": 656, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 696, \"height\": 553, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-stekovic-pytorchgeonodes-enabling-differentiable-shape-programs-for-3d-shape-reconstruction-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1670, \"height\": 1043, \"label\": \"Table\"}]"
motivation: 形状程序的优化潜力被忽视，现有方法难以梯度优化离散连续混合参数。
method: 将Blender过程模型转换为可微PyTorch代码，支持梯度与遗传算法联合优化。
result: 在三维重建中取得高精度，且能恢复语义参数，支持编辑。
conclusion: 可微形状程序为三维重建提供了灵活高效的优化框架，拓展了形状边界调整的途径。
---

## Abstract
We propose PyTorchGeoNodes, a differentiable module for reconstructing 3D objects and their parameters from images using interpretable shape programs. Unlike traditional CAD model retrieval, shape programs allow reasoning about semantic parameters, editing, and a low memory footprint. Despite their potential, shape programs for 3D scene understanding have been largely overlooked. Our key contribution is enabling gradient-based optimization by parsing shape programs, or more precisely procedural models designed in Blender, into efficient PyTorch code. While there are many possible applications of our PyTochGeoNodes, we show that a combination of PyTorchGeoNodes with genetic algorithm is a method of choice to optimize both discrete and continuous shape program parameters for 3D reconstruction and understanding of 3D object parameters. Our modular framework can be further integrated with other reconstruction algorithms, and we demonstrate one such integration to enable procedural Gaussian splatting. Our experiments on the ScanNet dataset show that our method achieves accurate reconstructions while enabling, until now, unseen level of 3D scene understanding.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：3D 场景理解中常见的表示（体素、网格、隐式场、高斯泼溅、模型检索）各有局限，如对遮挡敏感、内存大、缺乏可编辑性等。
- **核心问题**：形状程序（Shape Programs）是一种可解释、可编辑、内存占用极低的过程化建模方式，但如何从图像或扫描数据中自动估计其参数（包含连续与离散值）一直未被充分探索。
- **研究意义**：本文旨在使形状程序可微分，从而能够通过梯度优化高效、准确地从单/多视图 RGB‑D 数据中恢复物体三维结构及其语义参数，实现更高层的场景理解。

### 2. 方法论：PyTorchGeoNodes 与优化策略
- **核心思想**：设计一个“编译器”，将 Blender 中通过几何节点（Geometry Nodes）创建的形状程序自动转换为可微的 PyTorch 代码，使形状生成过程对连续参数可导。
- **关键实现细节**：
  - 支持多种节点类型的可微重现（数学运算、条件切换、几何原语、变换、复制、合并等），底层依托 PyTorch 与 PyTorch3D。
  - 直接将 Blender 设计好的节点图解析为等价的计算图，输出可微的 3D 网格。
- **优化算法**：
  - 目标函数 `L(P)` = 倒角距离（形状与输入点云） + 遮挡惩罚 + 地面约束。
  - 由于参数中同时包含连续、整数、布尔值，无法单纯梯度下降，因此采用 **遗传算法 + 梯度优化** 的混合策略：
    - 用遗传算法探索全局搜索空间（交叉、变异），同时处理离散参数。
    - 在每一代选择阶段，对连续参数用 PyTorchGeoNodes 提供的梯度进行局部精调。
- **扩展：过程化高斯泼溅**
  - 将可训练的高斯基元绑定到形状程序的原语上，克隆部分共享参数。
  - 分两步优化：先估计形状程序参数，再在已确定的结构上优化高斯体，实现细节与外观的精细重建且保持对称性。

### 3. 实验设计
- **数据集与场景**：真实室内数据集 **ScanNet** 中的 RGB‑D 扫描，选取沙发、椅子、桌子三个家具类别。
- **Ground Truth 标注**：手动标注了 176 个物体实例的连续及离散形状参数。
- **对比方法**：
  - **GeoCode**：基于监督学习的方法，在合成数据上训练编码器‑解码器直接预测参数，再经梯度精化。
  - **坐标下降（Coordinate Descent）**：迭代式逐个参数枚举优化（离散）加梯度精调。
- **评价指标**：
  - 连续参数：绝对误差。
  - 离散参数：分类准确率。
  - 整体重建质量：倒角距离、旋转误差。

### 4. 资源与算力
- 文中 **未明确给出** 所用 GPU 型号、数量及具体训练/优化耗时。
- 仅提及使用了法国 GENCI‑IDRIS 的高性能计算资源，但无详细算力描述。

### 5. 实验数量与充分性
- **主要实验**：1 组定量对比表（Table 1，涵盖 3 类物体，两个基线及自身消融：有无梯度精化）。
- **定性结果**：多幅图中展示重建形状与真实扫描的叠合、高斯泼溅重建效果。
- **评估样本量**：每类数十个手动标注实例（总计 176），规模偏小但属于人工密集标注的常见情形。
- **公平性**：
  - 对比了数据驱动方法和传统优化方法，均在与 PyTorchGeoNodes 一致的输入和评价下进行。
  - 遗传算法与坐标下降的迭代次数进行了近似时间匹配，较为公平。

### 6. 主要结论与发现
- PyTorchGeoNodes 成功将 Blender 形状程序转换为可微模块，使梯度信息可流入连续参数。
- 遗传算法 + 局部梯度优化的组合能够有效处理混合类型参数，在真实 ScanNet 数据上参数恢复和重建精度均显著优于监督学习基线（受域差异困扰）与坐标下降（易陷入局部最优）。
- 与过程化高斯泼溅的集成展示了该框架的模块化优势，能保持结构对称性并恢复遮挡部分。

### 7. 优点
- **端到端可微**：将过程化建模首次引入梯度优化循环，无需训练数据即可泛化到真实场景。
- **语义可解读**：输出不仅是一张网格，还包括有物理意义的尺寸、部件存在性等参数，便于下游编辑与理解。
- **模块化与可扩展**：易于与高斯泼溅等先进表示结合，并已公开代码。
- **实验对比扎实**：同时对比了监督学习和纯优化两种路线，并用人工标注的真实数据进行评估。

### 8. 不足与局限
- **人工设计依赖**：形状程序需人手为每个新类别设计，不能自动从数据中学习结构，限制了开放场景的应用。
- **类别与样本有限**：实验仅覆盖沙发、椅子、桌子三种家具，标注数量少，泛化能力尚未在大规模多类别上验证。
- **计算效率未讨论**：遗传算法 + 梯度优化的迭代耗时、与实时性的差距均未分析。
- **几何细节近似**：原语级程序难以捕捉细小形变或非程序化的复杂细节，依赖高斯泼溅弥补外观，但结构细节仍受限。
- **初始条件依赖**：方法需要初始物体中心估计和二维分割掩码，前端预处理精度会影响最终结果。

（完）
