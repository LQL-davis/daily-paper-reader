<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-07-08
- 运行时间：2026-07-08 21:42:24 UTC
- 运行状态：成功
- 本次总论文数：9
- 精读区：6
- 速读区：3

### 今日简报（AI）
矢量渲染与点云分解双满分突破：今日精读《NURBS Splatting》和《SuperFlex》，前者用可微NURBS曲面统一矢量图形渲染，后者以可变形超二次曲面精准拆解复杂点云。  
速读中流体方程范数增长的理论界也值得关注，但最亮眼的还是实时避碰中GPU加速的有号距离场方法。  
若你对三维重建或图形引擎感兴趣，建议从这两篇满分论文入手，体验几何建模与可微框架碰撞出的新工具。
- 详情：[/202607/08/README](/202607/08/README)

### 精读区论文标签
1. [NURBS Splatting: A Unified Differentiable Rendering Framework for Vector Graphics](/202607/08/2606.31764v1-nurbs-splatting-a-unified-differentiable-rendering-framework-for-vector-graphics)  
   标签：评分：9.0/10、query:boundary-opt
   evidence：可微渲染支持基于梯度的NURBS曲线边界优化
2. [SuperFlex: Deformable Superquadrics for Point Cloud Decomposition](/202607/08/2607.01015v1-superflex-deformable-superquadrics-for-point-cloud-decomposition)  
   标签：评分：9.0/10、query:boundary-opt
   evidence：利用可变形超二次曲面优化形状参数以拟合点云
3. [A Geometric Local Parameterization Method for Generalized Hele-Shaw Free Boundary Problems with Source Terms](/202607/08/2607.02880v1-a-geometric-local-parameterization-method-for-generalized-hele-shaw-free-boundary-problems-with-source-terms)  
   标签：评分：9.0/10、query:boundary-opt
   evidence：针对Hele-Shaw自由边界问题发展数值方法，直接优化边界形状
4. [Generalizable turbulence closures across bluff-body shapes by PINN-based solver-agnostic training](/202607/08/2607.04491v1-generalizable-turbulence-closures-across-bluff-body-shapes-by-pinn-based-solver-agnostic-training)  
   标签：评分：9.0/10、query:boundary-opt
   evidence：通过PINN训练可泛化的湍流封闭模型，用于钝体形状，直接优化空气动力学中的边界层预测模型。
5. [Stability Annealing Selects the Implicit Bias of Smoothed Sign Descent: A Rate-Indexed Barrier Path on Separable Data](/202607/08/2607.06013v1-stability-annealing-selects-the-implicit-bias-of-smoothed-sign-descent-a-rate-indexed-barrier-path-on-separable-data)  
   标签：评分：9.0/10、query:boundary-opt
   evidence：通过平滑符号下降的隐式偏差优化决策边界
6. [Domain-Decomposed Randomized Neural Networks for Partial Differential Equations in Unbounded Domains](/202607/08/2606.31342v1-domain-decomposed-randomized-neural-networks-for-partial-differential-equations-in-unbounded-domains)  
   标签：评分：8.0/10、query:boundary-opt
   evidence：通过域分解优化随机神经网络求解带边界条件的偏微分方程

### 速读区论文标签
1. [On the sharpness of bounds on the rate of growth of Lebesgue norms of the velocity in Navier-Stokes flows](/202607/08/2607.02739v1-on-the-sharpness-of-bounds-on-the-rate-of-growth-of-lebesgue-norms-of-the-velocity-in-navier-stokes-flows)  
   标签：评分：8.0/10、query:boundary-opt
   evidence：通过优化问题最大化Navier-Stokes速度范数的界尖锐性，使用黎曼共轭梯度法求解
2. [GPU-Accelerated Polygonal Signed Distance Functions for Real-Time Collision Avoidance](/202607/08/2607.04310v1-gpu-accelerated-polygonal-signed-distance-functions-for-real-time-collision-avoidance)  
   标签：评分：7.0/10、query:boundary-opt
   evidence：利用障碍物边界边的GPU加速有符号距离函数，实现实时碰撞避免，使能了边界约束下的高效优化。
3. [Separation Capacity of Scattering Networks on Low-Dimensional Datasets](/202607/08/2607.06048v1-separation-capacity-of-scattering-networks-on-low-dimensional-datasets)  
   标签：评分：7.0/10、query:boundary-opt
   evidence：优化散射网络架构以最大化分离能力，从而增强分类的决策边界。


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
