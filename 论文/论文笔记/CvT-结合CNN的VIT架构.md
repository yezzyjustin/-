https://arxiv.org/abs/2103.15808
https://github.com/leoxiaobin/CvT

CvT 能够将 cnn 的理想特性（位移，缩放和失真的不变性）引入 VIT，同时保持 transformer 的优点（动态注意力，全局上下文和更好的泛化能力），由于卷积的引入，可以移除 position embedding。
![[Pasted image 20240316171427.png]]
1. 使用 Convolutional Token Embedding 层将输入图像 (或 2 D 重构的 token 图)进行处理，该层由卷积实现，外加层归一化。这使得每个阶段能够逐渐减少 token 的数量同时增加 token 的维度，从而实现空间下采样和增加特征的丰富性，类似于 CNN 的设计。与其他基于 Transformer 的架构不同，CvT 不会将 position embedding 与 token 相加，这得益于卷积操作本身就建模了位置信息。
2. 堆叠的 Convolutional Transformer Block 组成了每个阶段的其余部分。Convolutional Transformer Block 的结构如图 2 b 所示，其中的 Convolutional Projection 为深度可分离卷积，用于 Q、K 和 vembedding 的转换，代替常见的矩阵线性投影。此外，classtoken 仅在最后阶段添加，使用 MLP 对最后阶段输出的分类 token 进行类别预测。