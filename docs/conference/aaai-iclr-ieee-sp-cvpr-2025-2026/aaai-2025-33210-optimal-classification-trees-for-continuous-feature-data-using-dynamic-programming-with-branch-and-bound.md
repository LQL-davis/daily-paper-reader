---
title: Optimal Classification Trees for Continuous Feature Data Using Dynamic Programming with Branch-and-Bound
title_zh: 使用动态规划和分支定界的连续特征数据最优分类树
authors: "Cătălin E. Brița, Jacobus G. M. van der Linden, Emir Demirović"
date: 2025-04-11
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/33210/35365"
tags: ["query:boundary-opt"]
score: 9.0
evidence: 计算最优分类树直接优化决策边界
tldr: 针对连续特征数据上的最优分类树计算问题，本文提出一种基于动态规划与分支定界的新算法。通过新的剪枝技术和高效深度二子树计算子程序，该方法可扩展至更高深度的树。实验表明，该算法在训练性能上优于现有方法，为决策边界优化提供了可证明最优解。
source: AAAI-2025-Accepted
selection_source: conference_retrieval
motivation: 现有最优分类树方法难以扩展到深度3以上，多依赖粗糙的连续特征二值化。
method: 提出动态规划结合分支定界算法，设计避免次优分裂的剪枝技术，并实现高效深度二子树计算。
result: 实验表明该方法可计算更深的最优分类树，且在训练性能上优于最新技术。
conclusion: 算法实现了连续特征上可证明最优分类树的可扩展计算，推动了决策边界优化。
---

## Abstract
Computing an optimal classification tree that provably maximizes training performance within a given size limit, is NP-hard, and in practice, most state-of-the-art methods do not scale beyond computing optimal trees of depth three. Therefore, most methods rely on a coarse binarization of continuous features to maintain scalability. We propose a novel algorithm that optimizes trees directly on the continuous feature data using dynamic programming with branch-and-bound. We develop new pruning techniques that eliminate many sub-optimal splits in the search when similar to previously computed splits and we provide an efficient subroutine for computing optimal depth-two trees. Our experiments demonstrate that these techniques improve runtime by one or more orders of magnitude over state-of-the-art optimal methods and improve test accuracy by 5% over greedy heuristics.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义

- **研究背景**：决策树因其可解释性与捕捉非线性关系的能力，在可解释人工智能（XAI）中受到青睐。然而，在给定大小限制下构建训练误差最小的最优分类树（ODT）是NP-hard问题。
- **现存挑战**：现有方法大多依赖对连续特征进行粗糙的二值化处理以维持可扩展性，但会损失最优性；少数能直接处理连续特征的方法（如Quant-BnB）也难以扩展至深度3以上的树。
- **论文目标**：提出一种直接基于原始连续特征数据、利用动态规划与分支定界的最优分类树算法，旨在突破深度可扩展性瓶颈，并显著提升训练与测试性能。

### 2. 方法论：核心思想、关键技术细节与算法流程

论文提出 ConTree 算法，核心在于动态规划与分支定界的结合，并设计了多种针对连续特征的剪枝技术与高效深度二子树子程序。

- **动态规划递归框架**  
  用递归函数 `CT(D, d)` 表示在数据集 D 和深度限制 d 下最小化误分类数。分支节点调用 `Branch(D, d, f)`，对每个特征 f 寻找最优分裂阈值 τ：
  \[
  \text{Branch}(D, d, f) = \min_{\tau \in S_f} \left[ \text{CT}(D(f \le \tau), d-1) + \text{CT}(D(f > \tau), d-1) \right]
  \]
  其中 \(S_f\) 为特征 f 上所有可能阈值（按排序后相邻值的中间点）。

- **基于相似性的下界剪枝原理**  
  利用“相似性下界”思想：若已知数据集 D\_old 的最优分数 θ\_old，对于新数据集 D\_new，有
  \[
  \theta_{D_{new}} \ge \theta_{D_{old}} - |D_{old} \setminus D_{new}|.
  \]
  在排序的连续特征上，两个不同阈值 τ 与 τ' 之间的观测转移数量可通过索引差直接计算，使得该下界可在 O(1) 时间内获得。

- **三种新型剪枝技术**
  - **邻域剪枝（Neighborhood Pruning, NB）**：当计算出某分裂点 u 的误分类分数 θ\_u 后，利用下界消除与 u 的索引距离小于 Δ = max(1, θ\_u − UB) 的所有分裂点，其中 UB 为当前最优解。
  - **区间收缩（Interval Shrinking, IS）**：在更新上界后，利用已计算分裂点的最优值进一步缩窄候选阈值区间，并结合“子树零误分”的特性安全删除某些极端阈值。
  - **子区间剪枝（Sub-interval Pruning, SP）**：若已知候选区间两侧已计算分裂点的左右子树最低误分之和已超当前上界，则整个区间内不可能存在更优解，直接跳过。

- **深度二子树专用子程序（D2Split）**  
  当剩余深度预算为2时，不再进行递归调用，而是利用特征排序特性，在一次线性扫描中同时计算左右子树的最优第二层分裂。对给定根节点分裂点，先统计各类别在左右子数据中的计数 FQ，然后针对其他候选特征，按该特征顺序扫描数据并维护当前计数，从而在 O(|D|·|F|) 时间内算出最优深度二子树。（公式13中给出了四个叶节点计数的推导）

- **上界管理**  
  递归计算左子树后，为右子树设定紧致的上界 `UB_R = max(UB − θ_{w,L}, η)`，其中 η 为当前候选区间边缘至中点的最小距离，平衡探索与剪枝力度。

- **最优性间隙参数（max-gap）**  
  允许用户设定可接受的次优百分比，使算法在保证不超过该间隙的前提下进一步加速，实现训练时间与精度的折中。

### 3. 实验设计

- **数据集与场景**  
  使用UCI机器学习库中的16个真实数据集，样本数从720到196,045，类别数为2至12，特征数为3至27（如Avila, Bank, Magic, Skin等）。所有数据均包含连续特征。
- **基准方法（Benchmark）**  
  - **经典启发式**：CART（贪婪基准）  
  - **MIP与SAT方法**：OCT（Bertsimas & Dunn），RS-OCT，SAT-Shati  
  - **直接处理连续特征的最优方法**：Quant-BnB（唯一专门化算法）  
  - 内部对比：ConTree关闭深度二子程序、关闭各剪枝模块的消融版本。
- **评估指标**  
  运行时间（秒）、最优性间隙（%）、深度二、三、四以及更深层树的求解能力；同时提供了anytime性能曲线和外样本测试精度对比。

### 4. 资源与算力

- 论文未提及任何GPU使用，所有计算在CPU上进行。
- 硬件环境：Intel Xeon E5-6448Y 32核 2.1GHz 处理器，配备25GB RAM，操作系统为Linux Red Hat Enterprise 8.10。
- 运算时长：每次实验设4小时超时限制，并多次运行取平均（通常20次），以评估算法效率。
- 无GPU型号或并行训练的相关描述，所有方法均在给定CPU上运行。

### 5. 实验数量与充分性

- **实验组数丰富**：包括深度二、三、四求解能力对比、与Quant-BnB等最新方法的横向对比、剪枝模块逐一消融（NB、IS、SP）、深度二子程序有无对比、anytime性能分析、不同max-gap下的效率/精度折中、以及外样本精度对比（正文及附录）。
- **多角度验证**：不仅有运行时间对比，还包含了最优性间隙、解的质量随时间变化、内存占用（附录）、更深深度的可扩展性等。
- **公平性**：所有方法在相同硬件和超时条件下运行，比对的MIP方法均以CART解作为热启动，且重复运行以降低随机波动影响。实验设计客观、全面。

### 6. 主要结论与发现

- ConTree的三种剪枝技术平均可剪除99.6%的D2Split调用，且结合使用后进一步减少10%的调用量。
- 深度二专用子程序比朴素递归平均快320倍。
- 与最优连续特征处理的最新方法Quant-BnB相比，ConTree在深度三上平均快63倍，在深度四上首次实现了多个数据集的可证明最优解（而Quant-BnB不支持深度四）。
- 与MIP和SAT方法相比，ConTree在速度上有数量级的绝对优势，后者在4小时超时后仍存在较大最优性间隙。
- 在anytime表现上，ConTree通常能在搜索早期找到接近最优的解，大部分时间用于证明最优性。
- 在外样本精度上，相同大小约束下，ConTree深度三树的测试精度平均比CART高5%，比使用粗糙二值化的ODT高0.7%。

### 7. 优点

- **可证明最优性**：在连续特征上直接构建满足全局训练误差最小的决策树，避免了二值化带来的信息损失。
- **高度可扩展**：突破了以往方法仅能处理深度三的限制，可在合理时间内求解深度四甚至更深的树。
- **精巧的剪枝设计**：将连续特征排序特性与相似性下界完美结合，利用O(1)时间计算下界并嵌入多种互补剪枝，剪枝力度极大。
- **工程实现坚实**：提供了C++实现与Python包，带有max-gap参数供用户调节效率-精度平衡。
- **实验全面详实**：覆盖多个数据集、与多种前沿方法对比，并深入分析了各模块单独贡献。

### 8. 不足与局限

- **应用场景受限**：当前版本仅适用于二值轴对齐单阈值分裂的二叉分类树，尚未扩展至回归或多路分裂、类别特征的超集处理。
- **内存和更大数据扩展性**：尽管内存占用相对Quant-BnB有改善，但在极大数据集（如百万级以上）或更深层树上，时间和内存是否仍可控仍需检验。
- **超参数限制**：限制为深度限制，未引入节点代价或叶子数限制，稀疏性可能不足。
- **对比方法**：与基于SAT/MIP的方法的比较中，部分方法未深度调优或使用了不同的问题设定（如节点代价），最优性对比的基准可能未完全对齐。
- **没有讨论多分类与类别不平衡的优化目标**：优化目标仅使用0-1损失，未涉及如F1-score或成本敏感学习等实际中更关心的指标。

（完）
