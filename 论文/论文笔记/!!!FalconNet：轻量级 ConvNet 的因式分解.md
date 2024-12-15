FalconNet: Factorization for the Light-weight ConvNets
https://arxiv.org/abs/2306.06365

设计具有小参数量和 flops 的轻量化 cnn 是一个突出的研究问题，现存在的问题主要有：
1. 缺乏架构一致性，导致冗余和容量比较受阻，以及架构选择和性能增强之间的因果关系不明确。
2. 单分支深度卷积的使用损害了模型的表示能力
3. depth-wise 占了参数和 flops 的很大比例

![[Pasted image 20240327135522.png]]

**stem:** 是指模型的起始部分。希望在开始时通过多个 Conv 层来捕捉更多细节，而不是使用具有相对较大步幅的单个 Conv 层。在步幅为 2 的第一个 3 x 3 规则 conv 层之后，作者使用 3 x 3 dw-conv 层来捕获低电平模式，然后使用 1 x 1 pw-conv 和另一个 3 x 3 dw-conv 层进行二次采样。还有一个 short cut 方式，如图2所示。

**stage**:stage1-4 由几个重复的基本块组成，例如内部评级。根据 1:1:3: 1 的阶段计算比率，每个阶段中的区块数设置为【3，3，9，3】。遵循塔原理并考虑减少参数，每个阶段中的通道维度设置为【32，64，128，256]。

**Subsampling Layers**：在 stage 之间添加单独的子采样层。使用 2 x 2 Conv 层，具有输入通道维度组和 2 的步幅，用于将空间分辨率减半。2 x 2 的对流层也使通道尺寸加倍。随后安排一个 BN 层来稳定训练。

**head**：在第 4 stage 之后，是模型的最后一部分。首先使用 1 x 1 转换器层来进一步交互信息，然后利用全局平均池化来获得特征向量，随后将其输入到最后一个 FC 分类器中，并获得最终输出。

![[Pasted image 20240327135948.png]]

Basic Block 是轻量化神经网络的关键组件。如图 3 所示，不同的 BasicBlock 通常可以抽象为 Meta Basic Block (图 3 a)，Meta Basic Block 由空间算子和通道算子 (即 PW-Conv)交替组成。Meta Basic Block 的框架为模型提供了基本的表征能力，这意味着将这些空间算子实例化为不可学习的 Identity 映射仍然可以实现有效的性能，而模型性能的差异本质上来自 Meta Basic Block 不同的结构实例化，例如，FasterNetBlock 将第一个空间算子实例化为 PConv，并将另外两个空间算子初始化为同一映射，如图3a 所示。

通过进行广泛的实验可以观察到第一个和最后一个算子层对模型性能的增强没有好处，而两个通道算子层之间的第二空间算子层是显著的。

![[Pasted image 20240327140037.png]]

因此，作者进一步将 Meta Basic Block 简化为 Meta Light Block (图 4 a)，它由两个 PW-Conv 层 (具有扩展比入)和介于两者之间的单个空间算子层组成。