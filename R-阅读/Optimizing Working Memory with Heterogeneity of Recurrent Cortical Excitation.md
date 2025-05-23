#### 关联文章：
[[Stability of working memory in continuous attractor networks under the control of short-term plasticity]]

#### 关键词

#工作记忆  
#异质性兴奋性耦合  
#持久神经元活动  
#参数化工作记忆  
#扩散过程  
#working_memory  
#heterogeneous_excitatory_coupling  
#persistent_neuronal_activity  
#parametric_working_memory  
#diffusion_process

#### 文章信息

```
作者: Zachary P. Kilpatrick, Bard Ermentrout, Brent Doiron  
标题: Optimizing Working Memory with Heterogeneity of Recurrent Cortical Excitation  
期刊: Systems/Circuits  
发表日期: 未提供  
DOI: 未提供  
代码仓库：未提供
```

#### 背景问题

工作记忆（Working Memory, WM）是大脑在延迟期间维持和操作信息的关键过程。传统观点认为，工作记忆的信息维持依赖于持久神经元活动，特别是通过局部兴奋和广泛抑制的网络架构来实现。然而，神经元输入中的噪声波动会导致持久活动的扩散，从而随着时间的推移降低记忆的准确性。本文旨在探讨**异质性兴奋性耦合**如何影响持久神经元活动的扩散，并优化参数化工作记忆的表现。

#### 文章简介

本文通过研究**异质性兴奋性耦合**在参数化工作记忆中的作用，探讨了如何通过调整网络架构来优化工作记忆的表现。研究发现，**空间异质性的兴奋性耦合**能够稳定持久活动的离散状态，减少持久活动在网络中的扩散。然而，这种异质性也会**粗化刺激表示空间**，限制参数化工作记忆的存储容量。研究进一步表明，**粗化误差和扩散误差之间存在权衡**，使得在特定网络异质性下，信息传递达到最优。对于较长的延迟时间，最优的吸引子数量可能少于可能的刺激数量，这表明记忆网络可以通过**欠表示刺激空间**来优化性能。

### Results 内容脉络

本文通过研究**异质性兴奋性耦合**对参数化工作记忆的影响，系统地探讨了持久神经元活动的扩散机制及其对信息存储的影响。研究的主要结果如下：

1. ​**异质性兴奋性耦合减少持久活动的扩散**：在具有空间异质性兴奋性耦合的网络中，持久活动的扩散速率显著降低。这种异质性通过稳定离散的网络状态，减少了持久活动在网络中的随机漂移。
    
2. ​**异质性耦合粗化刺激表示空间**：尽管异质性兴奋性耦合减少了持久活动的扩散，但它也限制了网络能够稳定存储的刺激数量。这种粗化效应导致网络在表示刺激空间时存在一定的误差。
    
3. ​**粗化误差与扩散误差的权衡**：研究发现，**粗化误差**和**扩散误差**之间存在权衡关系。在特定的网络异质性下，这种权衡使得信息传递达到最优。对于较长的延迟时间，最优的吸引子数量可能少于可能的刺激数量，这表明记忆网络可以通过**欠表示刺激空间**来优化性能。
    
4. ​**无结构异质性的影响**：研究还探讨了**无结构异质性**对网络性能的影响。结果表明，尽管无结构异质性会引入额外的噪声，但具有**结构化异质性**的网络仍然能够保持较高的信息传递效率，表明结构化异质性对噪声具有鲁棒性。
    
5. ​**信息传递的最优化**：通过将工作记忆网络建模为**噪声信道**，研究发现，在特定的网络异质性下，信息传递达到最优。这种最优化是通过平衡**粗化误差**和**扩散误差**来实现的，进一步验证了异质性兴奋性耦合在优化工作记忆中的关键作用。