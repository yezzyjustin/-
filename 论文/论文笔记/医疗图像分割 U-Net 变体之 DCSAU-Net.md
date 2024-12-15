https://arxiv.org/pdf/2202.00972.pdf
https://github.com/xq141839/DCSAU-Net

DCSAU-Net 整体结构还是 U-Net 形式，包含 encoder 和 decoder 结构。
(1) encoder 由 compact split-attention (CSA)组件和 deeper convolution (DC)组件组成。CSA 组件加强了不同 kernel 大小的卷积之间的特征连接。DC 组件的设计采用残差风格，以避免随着层的增加而梯度消失问题，同时扩大感受域。
(2)对于 decoder，作者实现了受 u 形结构启发的特征融合。

**CSA 组件结构**
![[Pasted image 20240327161928.png]]

结合 CSA 结构图看，input 特征会按通道分成两部分，而不是直接输入到不同的路径。这种按通道分成两部分的设计可以使得每个部分能够分别经过不同的卷积操作和注意力机制处理，以捕获和强调不同通道的特征表示。最终，这些处理过的特征图将被融合在一起，以实现更丰富和多样化的特征表达，从而提高网络在医学图像分割任务中的性能和准确性，

(1) Global Average Pooling 操作在 Compact Split-Attention (CSA) 模块中用于将输入特征维度由 (h，w，c)变成 (c)。这个操作通过对输入特征图在空间维度上进行平均池化，将每个通道上的特征值取平均，最终将整个特征图转换为一个具有个元素的向量。这有助于提取全局的空间信息，并且减少了特征图的空间维度，使得网络更加高效地捕获通道间的关键信息。
(2) CSA 模块中 Densec'其实就是全连接层;
(3) CSA 模块中 Softmax 之后需要将一个具有 c'个元素的向量，经过一个逆全局平均池化 (Inverse Global Average Pooling)操作变成维度是 (h, w, c)的特征图
(4)然后就是两个特征维度是 (h, w, c)的分支，concat 合并特征，得到维度是 (h, w, c)的特征。
(5)最后再将 add 原始输入特征，进行残差跳层连接。

**DC 组件**
如下图所示，其实一个 DC 组件也是残差结构。DeeperConvolution (DC)模块是通过采用深度可分离卷积 (depthwise separable convolution)结构来实现的。在 DCSAU-Net 中，DC 模块采用了更大的卷积核大小 (7 x 7)的深度可分离卷积，以扩大网络的感受野并保留主要特征，同时不增加参数数量。

![[Pasted image 20240327162642.png]]
