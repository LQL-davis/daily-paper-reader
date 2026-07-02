---
title: Shape Abstraction via Marching Differentiable Support Functions
title_zh: 基于行进可微支持函数的形状抽象
authors: "Park, Sunkyung, Lee, Jeongmin, Lee, Dongjun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Park_Shape_Abstraction_via_Marching_Differentiable_Support_Functions_CVPR_2025_paper.pdf"
tags: ["query:boundary-opt"]
score: 7.0
evidence: 通过可微支持函数优化形状抽象边界
tldr: 形状抽象将物体表示为基元集合，但高精度与通用性难以兼得。本文利用可微支持函数表示凸形状，通过行进和组合优化技术高效调整基元边界，并提供可微接触特征，在形状逼近和接触相关下游任务中展现了突出的精度与应用潜力。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 855, \"height\": 645, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 704, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 748, \"height\": 374, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 663, \"height\": 449, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 868, \"height\": 521, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 872, \"height\": 648, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 788, \"height\": 366, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1814, \"height\": 657, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 862, \"height\": 335, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 811, \"height\": 389, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 824, \"height\": 443, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 789, \"height\": 593, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 734, \"height\": 696, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 750, \"height\": 246, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-park-shape-abstraction-via-marching-differentiable-support-functions-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1813, \"height\": 338, \"label\": \"Table\"}]"
motivation: 形状抽象需要高精度及通用性，现有基元表示存在不足。
method: 利用可微支持函数表示凸形状，结合行进与组合优化技巧调整边界。
result: 在形状逼近和下游接触任务中取得优异性能，支持可微接触特征。
conclusion: 可微支持函数为形状边界优化提供了新途径，增强了形状抽象的实用价值。
---

## Abstract
Shape abstraction, simplifying shape representation into a set of primitives, is a fundamental topic in computer vision. The choice of primitives shapes the structure of world understanding, yet achieving both high abstraction accuracy and versatility remains challenging. In this paper, we introduce a novel framework for shape abstraction utilizing a differentiable support function (DSF), which offers unique advantages in representing a wide range of convex shapes with fewer parameters, providing smooth surface approximation and enabling differentiable contact features (gap, point, normal) essential for downstream applications involving contact-related problems. To tackle the associated optimization and combinatorial challenges, we introduce two techniques: differentiable shape parameterization and hyperplane-based marching to enhance accuracy and reduce DSF requirements. We validate our method through experiments demonstrating superior accuracy and efficiency, and showcase its applicability in tasks requiring differentiable contact information.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：形状抽象是计算机视觉的基础任务，即将复杂形状简化为一组基元（如超二次曲面、立方体、多面体等），以便于结构化理解、编辑和物理仿真。现有基元在表达能力和通用性上仍有局限：超二次曲面仅能表示对称形状，多面体需大量参数表示光滑曲面。
- **核心问题**：如何在保持高抽象精度的同时，用更少的基元表示更广泛的凸形状，并为接触相关下游任务（如接触定位、可微仿真）提供可微的接触特征（间隙、点、法向）。
- **整体含义**：提出以可微支持函数（DSF）作为新的基元，结合可微形状参数化与基于超平面的行进策略，在精度、效率和可微接触能力上全面优于现有方法。

### 2. 论文提出的方法论
#### 2.1 核心思想
利用DSF表示凸形状，其支持函数形式为
\[
h(x) = \left( \sum_{i=1}^{N_v} \max\{ (v_i - c)^T x, 0\}^p \right)^{1/p},
\]
其中 \(v_i\) 为顶点，\(c\) 为可学习的中心，\(p\) 为平滑度参数。该函数可微，且其梯度给出支撑点，从而能计算可微接触特征。

#### 2.2 关键技术细节
- **可微形状参数化**：将中心 \(c\) 参数化为顶点插值，通过 softmax 类函数确保中心在顶点凸包内；平滑度参数 \(p\) 通过 tanh 限制在 \((2, p_{\max})\)。形状参数 \(\phi = (v_{1:N_v}, \alpha_{1:N_v}, \tilde{p})\)，使得支持函数对形状参数可微。
- **DSF 拟合优化**：给定目标几何体的方向采样 \(\{x_i, \hat{h}_i\}\)，通过非线性最小二乘优化 \(\sum_i (h(x_i,\phi) + c^T x_i - \hat{h}_i)^2\) 拟合 DSF，通常采用整数 \(p\) 近似以降低计算量。
- **基于超平面的行进策略**：从 SDF 数据中提取 DSF 集合，避免网格分解-拟合的误差累积。提出两种方式：
  - **区域缩减**：从 SDF 内部点出发，迭代添加半空间（超平面）切除外部区域，形成凸区域后拟合 DSF；已拟合区域外的点被移除，并利用重叠参数控制基元间的重叠，减少基元数量。
  - **区域扩张**：初始化超平面构成球体，采样点并根据点到半空间距离动态调整超平面偏移，忽略局部凹陷，最终得到较大凸区域并拟合 DSF。
  - **混合方法**：先用区域缩减生成高精度基元，当 SDF 残差大于阈值时切换区域扩张，平衡精度与基元数量。
- **DSF 自退化**：拟合后删除体积过小或与 SDF 偏差过大的退化基元。

#### 2.3 应用中的可微性
由于 DSF 形状参数可微，接触特征（间隙、点、法向）对形状参数和物体位姿可微，可嵌入到形状编辑、接触定位和可微仿真等任务的梯度优化中。

### 3. 实验设计
- **数据集**：YCB 数据集（14 个物体）和 MuJoCo Menagerie 机器人连杆 CAD 数据（5 个机器人共 51 个连杆）。
- **对比方法**：
  - CvxNet（基于神经网络同时优化所有基元）
  - CoACD（网格分解）加上 DSF 拟合（CoDSF）
  - Marching-Primitives（MP，行进超二次曲面）
- **评估指标**：交集比（IoU）、Chamfer‑L1 距离、基元数量 \(N_c\)。
- **消融实验**：比较三种行进策略（区域缩减、区域扩张、混合）以及不同重叠参数对精度和基元数量的影响。

### 4. 资源与算力
- 文中**未明确说明**使用的 GPU 型号、数量或训练时长。仅提及 CvxNet 训练时 batch size 为 32、最大训练步数 5000，但未报告具体硬件资源。其余方法（DSF 拟合、行进算法）均为计算密集型但非 GPU 中心，可能无需大规模算力。

### 5. 实验数量与充分性
- 主要对比实验覆盖 6 个类别（5 种机器人 + YCB），每组对象数 5～23 个，合计 65 个对象以上。
- 消融实验以整体平均值报告，分析了行进策略和重叠参数的影响。
- 提供了形状编辑、接触定位、可微仿真三个应用案例，定性展示了方法实用性。
- **充分性与公平性**：
  - 对比的基线均为近期或经典方法，指标全面，评估方式合理。
  - 使用了相同的基元顶点数（\(N_v=20\)）和方向采样数（\(N_h=1742\)），对 DSF 拟合公平。
  - 但未比较更多基于学习的抽象方法（如不同超二次曲面方法），也未测试更大规模或零样本泛化场景，实验覆盖偏重于结构化人造物体，结论推广性有限。

### 6. 论文的主要结论与发现
- 提出的 DSF 抽象方法在 IoU、Chamfer‑L1 距离和基元数量上均优于 CvxNet、CoDSF、MP，实现了更少的基元、更高的精度。
- 混合行进策略在精度与基元数量间取得最佳平衡，大重叠参数进一步减少基元数量。
- 可微接触特性使抽象结果可直接用于形状编辑、接触定位和可微仿真等下游任务，展开新的应用前景。

### 7. 优点
- 基元表示能力强：DSF 以较少参数准确表示光滑凸形状，超越了超二次曲面的对称局限。
- 可微性贯穿始终：形状参数、接触特征均对位姿和参数可微，为基于梯度的任务优化提供解析梯度。
- 行进算法精巧：区域缩减和扩张组合有效处理凹形，避免传统网格分解带来的误差累积和过量分解。
- 实验设计详实，包含定量对比和消融分析，并展示了从感官数据到抽象再到物理仿真的完整流水线。

### 8. 不足与局限
- **实验覆盖面**：测试对象主要为机器人连杆和 YCB 物品，形状类型相对单一，未在自然场景扫描数据或更复杂自由形上验证。
- **依赖 SDF 输入**：需要已重建的 SDF，对初始数据质量敏感，且未结合视觉纹理等多模态信息。
- **参数敏感性**：重叠参数 \(o_d\)、平滑度约束等可能需要手动调整，未系统讨论其鲁棒性。
- **计算效率**：虽然基元数量少，但每个基元拟合和超平面迭代可能需要循环和采样，未报告抽象过程的耗时比较。
- **应用局限**：接触可微性依赖于 DSF‑DSF 接触求解器，未扩展到与环境中的非凸障碍物的通用接触查询。

（完）
