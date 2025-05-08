### 关键词

#短时突触可塑性  
#时空处理  
#状态依赖计算  
#循环神经网络  
#神经网络动力学  
#short-term_synaptic_plasticity  
#spatiotemporal_processing  
#state-dependent_computation  
#recurrent_neural_networks  
#neural_network_dynamics

### 文章信息

```
作者: Dean V. Buonomano, Wolfgang Maass  
标题: State-dependent computations: spatiotemporal processing in cortical networks 
期刊: Nature Reviews Neuroscience  
发表日期: 2009年1月15日  
DOI: 10.1038/nrn2558  
```

### 背景问题

大脑能够无缝地处理和整合感官刺激的时空特征，这种能力对于识别自然刺激至关重要。然而，处理时空刺激的通用计算框架仍然难以捉摸。最近的理论和实验研究表明，时空处理源于传入刺激与神经网络内部动态状态之间的相互作用，包括其持续的尖峰活动和“隐藏”的神经元状态，如短时突触可塑性（STSP）。本文旨在探讨状态依赖计算如何从皮层网络的时间依赖特性中产生，并解释时空处理的计算机制。

### 文章简介

本文综述了状态依赖计算在时空处理中的作用，提出了一种基于神经网络内部动态状态的计算框架。研究表明，时空处理源于传入刺激与网络内部状态（包括活跃状态和隐藏状态）之间的相互作用。活跃状态由网络的持续活动定义，而隐藏状态则由短时突触可塑性等时间依赖的神经元特性决定。本文还讨论了状态依赖网络、液体状态机和回声状态网络等模型，并提出了这些模型在时空处理中的应用。研究结果表明，​**时空处理依赖于网络内部状态的动态变化，而不仅仅是外部刺激的特性**。这些发现为理解大脑如何处理复杂时空信息提供了新的见解。

### Results 内容脉络

本文通过分析神经网络内部状态与外部刺激的相互作用，系统地探讨了状态依赖计算在时空处理中的作用。研究的主要结果如下：

1. ​**状态依赖计算的框架**：时空处理依赖于网络内部状态的动态变化，包括活跃状态和隐藏状态。活跃状态由网络的持续活动定义，而隐藏状态则由短时突触可塑性等时间依赖的神经元特性决定。这种状态依赖计算框架解释了大脑如何处理复杂时空信息。
    
2. ​**活跃状态与隐藏状态的作用**：活跃状态通过网络的持续活动编码当前和过去的信息，而隐藏状态则通过短时突触可塑性等机制提供对近期刺激历史的记忆。这两种状态的结合使得网络能够处理复杂的时空刺激。
    
3. ​**状态依赖网络模型**：本文讨论了状态依赖网络、液体状态机和回声状态网络等模型，这些模型通过利用网络内部状态的动态变化来处理时空信息。这些模型展示了如何通过状态依赖计算实现复杂的时空处理任务。
    
4. ​**时空处理的神经机制**：研究表明，时空处理依赖于网络内部状态的动态变化，而不仅仅是外部刺激的特性。这种机制使得大脑能够处理复杂的时空信息，如语音识别和运动检测。
    
5. ​**未来研究方向**：本文还提出了未来研究的方向，包括进一步探索状态依赖计算在复杂时空处理中的应用，以及开发更符合生物学现实的神经网络模型。

**输入响应神经活动比作水波**
[[Buonomano 和 Maass - 2009 - State-dependent computations spatiotemporal proce.pdf#search=This general point can be intuitively understood by making an analogy between neural networks and a liquid 20 . A pebble thrown into a pond will create a spatiotemporal pattern of ripples, and the pattern produced by any subsequent pebbles will be a complex nonlinear function of the interaction of the stimulus (the pebble) with the internal state of the liquid (the pattern of ripples when the pebble makes contact). ripples thus establish a shortlasting and dynamic memory of the recent stimulus history of the liquid.|Buonomano 和 Maass - 2009 - State-dependent computations spatiotemporal proce, p.114]]

Indeed, in some cases even if the stimulus (for example, a constant odour or steady auditory tone) is not varying in time, the neural trajectories that represent the network’s active state continue to change
[[Buonomano 和 Maass - 2009 - State-dependent computations spatiotemporal proce.pdf#search=change24,2|Buonomano 和 Maass - 2009 - State-dependent computations spatiotemporal proce, p.114]]

 In particular, linearly inseparable classes of external stimuli tend to become linearly separable once they are projected nonlinearly into a higher dimensional state space

it is not even known whether the spatial and temporal dimensions of stimuli are processed by the same or different networks