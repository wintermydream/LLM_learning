#### 文章信息

```
作者: Ábel Ságodi, Guillermo Martín-Sánchez, Piotr Sokół, Il Memming Park  
标题: Back to the Continuous Attractor  
会议: 38th Conference on Neural Information Processing Systems (NeurIPS 2024)  
代码仓库：https://github.com/catniplab/back_to_the_continuous_attractor
```

#### 背景问题

连续吸引子（Continuous Attractor）是神经科学中用于解释连续变量（如方向、位置等）在神经网络中持久存储的理论工具。然而，连续吸引子在数学上具有结构不稳定性，即微小的扰动会破坏其连续固定点的结构，这限制了其在生物系统中的实用性。本文旨在探讨连续吸引子的近似形式是否在功能上具有鲁棒性，并解释其在模拟记忆任务中的表现。

#### 文章简介

本文通过理论分析和数值实验，研究了连续吸引子在受到扰动后的行为。研究发现，尽管连续吸引子在数学上具有不稳定性，但其近似形式（如慢流形）在功能上具有鲁棒性。通过持久流形理论，本文证明了在扰动后，系统仍然会保留一个与原始连续吸引子拓扑等价的慢流形，并且该流形在行为相关的时间尺度上表现出与连续吸引子相似的功能。研究结果表明，​**连续吸引子作为理解模拟记忆的框架仍然具有重要价值**，即使在实际系统中无法观察到理想的连续吸引子。

### Results 内容脉络

本文通过理论分析和数值实验，系统地研究了连续吸引子的近似形式及其在模拟记忆任务中的表现。研究的主要结果如下：

1. ​**连续吸引子的结构不稳定性**：连续吸引子在数学上具有结构不稳定性，微小的扰动会破坏其连续固定点的结构。然而，扰动后的系统仍然会保留一个与原始连续吸引子拓扑等价的慢流形。
    
2. ​**持久流形理论的应用**：通过持久流形理论，本文证明了在扰动后，系统仍然会保留一个与原始连续吸引子拓扑等价的慢流形。该流形在行为相关的时间尺度上表现出与连续吸引子相似的功能。
    
3. ​**慢流形的功能鲁棒性**：研究发现，慢流形在功能上具有鲁棒性，能够在行为相关的时间尺度上有效地存储和操作连续变量。这一发现表明，连续吸引子作为理解模拟记忆的框架仍然具有重要价值。
    
4. ​**任务优化的循环神经网络**：通过训练循环神经网络（RNN）执行模拟记忆任务，研究发现，网络在训练过程中会自然地找到与慢流形相似的解决方案。这些解决方案在功能上表现出与连续吸引子相似的行为，进一步支持了连续吸引子的功能鲁棒性。
    
5. ​**广义化分析**：研究还发现，慢流形的广义化能力与其拓扑结构密切相关。具有固定点的慢流形在长时间尺度上表现出更好的记忆性能，而具有极限环的慢流形则在短时间内表现出更好的记忆性能。这一发现为理解不同任务中记忆性能的变异性提供了新的见解。

### 可能的原理

#### 1. 神经异质性作为扰动的“缓冲器”，维持慢流形的存在

- **理论依据**：本文的持久流形定理（Persistent Manifold Theorem，Theorem 1）表明，对于一个正常双曲（normally hyperbolic）的连续吸引子，在小的扰动（ϵ>0）下，存在一个与原始流形拓扑等价的慢流形$\mathcal{M}_\epsilon$。这种慢流形虽然不再是平衡点连续体，但仍保持不变性和吸引力（Sec. 3.1）。扰动的大小ϵ决定了慢流形上的流动速度和与原始吸引子的接近程度。
- **神经异质性的作用**：Wu等人发现，神经异质性（例如时间常数$\tau_L$​和$\tau_\Gamma$τ的lognormal分布）能够打断网络的同步时空模式，减少时间率波动（temporal rate fluctuations），并使网络活动更紧密地跟随输入。这种异质性可以看作一种“内源性扰动”，但与随机噪声不同，它是有序的、结构化的。结合本文的理论，这种异质性可能通过调节扰动效应，使网络动态保持在慢流形附近，从而避免完全的分岔崩溃（例如从连续吸引子变为单一固定点）。
- **机制解释**：异质性通过增加神经元响应的多样性，分散了扰动对网络整体动态的影响。假设扰动$\mathbf{p}(\mathbf{x})$作用于递归网络$\dot{\mathbf{x}} = \mathbf{f}(\mathbf{x}) + \epsilon \mathbf{p}(\mathbf{x})$，异质性可能通过调整$\mathbf{f}(\mathbf{x})$的局部特性（如时间尺度分离），使慢流形$\mathcal{M}_\epsilon$​的形成更鲁棒。这种“缓冲”效应确保了即使在外部扰动或突触可塑性波动下，网络仍能维持一个稳定的表征轨迹。

#### 2. 增强局部敏感性与慢流形上的输入驱动动态

- **理论依据**：本文的快慢分解分析（Sec. 3.2）表明，慢流形上的动态由切向慢流（tangent slow flow, $\dot{\mathbf{y}} = \epsilon \mathbf{g}(\mathbf{y}, \mathbf{z}, \epsilon)$和法向快流（normal fast flow, $\dot{\mathbf{z}} = \mathbf{h}(\mathbf{y}, \mathbf{z}, \epsilon)$组成。慢流形上的慢流动速度与扰动大小ϵ成正比，而法向快流确保了流形的吸引力。
- **神经异质性的贡献**：Wu等人提出，神经异质性增强了网络对输入的局部敏感性，使活动更倾向于“输入驱动的瞬态动态”。在慢流形框架下，这种敏感性可能体现在切向慢流$\mathbf{g}(\mathbf{y}, \mathbf{z}, \epsilon)$对输入的响应能力上。异质性通过引入多样化的神经元响应时间（例如$\tau_L$和$\tau_\Gamma$​的分布），使网络能够动态调整慢流形上的轨迹，保持对外部信号的连续跟踪。
- **机制解释**：在工作记忆或空间导航等任务中，输入（如角速度或目标位置）需要被整合并维持。异质性可能通过增加网络的动态复杂性，使慢流形上的流动不仅缓慢且稳定，还能灵活适应输入变化。这与本文观察到的“幽灵”流形行为一致，即扰动后的网络仍保留类似连续吸引子的有限时间行为（finite-time behavior）。

#### 3. 减少试验间变异性，提升慢流形的稳定性

- **理论依据**：本文通过训练递归神经网络（RNNs）验证了慢流形的普遍性（Sec. 4）。实验表明，训练后的网络动态约束在一个慢不变流形上，且流形上的固定点数量和拓扑结构影响记忆误差（angular error）和泛化能力（Sec. S7.7）。慢流形的稳定性依赖于法向快流的吸引性和切向慢流的低速性。
- **神经异质性的作用**：Wu等人发现，增加异质性（如$\sigma_{\tau_L}​​$从3ms增至8ms）显著降低了归一化均方根误差（NRMSE），表明网络对噪声和扰动的鲁棒性增强。这种减少的试验间变异性（trial-to-trial variability）可能直接映射到慢流形的稳定性上。异质性通过分散神经元活动的时间特性，防止网络陷入不稳定状态（如同步振荡或噪声主导的漂移），从而稳定慢流形上的轨迹。
- **机制解释**：在生物系统中，突触权重波动和在线学习信号的随机性是常见的扰动源。本文的理论表明，慢流形能够在这些扰动下保持吸引性，而异质性可能通过“平滑”这些扰动效应，减少流形上轨迹的抖动（jitter），确保信号表征的连续性和稳定性。例如，在环吸引子（ring attractor）中，异质性可能减少固定点数量的剧烈变化（如从连续体变为少量孤立点），维持一个更平滑的慢流形。

#### 4. 拓扑鲁棒性与异质性支持的泛化能力

- **理论依据**：本文发现，不同的连续吸引子近似（如线吸引子、环吸引子）在扰动后保留了拓扑等价的慢流形（Sec. 2）。例如，环吸引子的分岔可能导致多个固定点，但这些固定点仍排列在一个不变的环形流形上（Fig. 2）。这种拓扑鲁棒性使得慢流形在功能上接近理想的连续吸引子。
- **神经异质性的贡献**：Wu等人的研究表明，异质性不仅限于时间常数，还包括非均匀输入连接和尖峰阈值多样性，这些都有助于形成一致的刺激表征。结合本文的结果，异质性可能通过增加网络的拓扑复杂性，支持慢流形在不同扰动下的拓扑一致性。例如，在RNN训练中，异质性可能使网络更容易收敛到一个具有适当固定点分布的慢流形（如Fig. S18C和D所示的LSTM和GRU结果）。
- **机制解释**：神经异质性可能通过提供一种“冗余”机制，使慢流形的拓扑结构对参数变化不敏感。在生物系统中，这种冗余可能表现为神经元群体的多样性，使网络能够适应不同的任务需求（如记忆保持或动态更新），从而增强其泛化能力和鲁棒性。