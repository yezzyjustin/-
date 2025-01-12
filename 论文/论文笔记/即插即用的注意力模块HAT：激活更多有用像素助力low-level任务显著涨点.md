
Activating more pixels in image super-resolution transformer
https://github.com/XPixelGroup/HAT
https://arxiv.org/pdf/2205.04437.pdf

<font color="#ff0000">为了激活更多的输入像素以实现更好的重建效果，本文构建了一种新颖的混合注意力Transformer (HAT)方法。该方法结合了通道注意力和基于窗口的自注意力机制，充分利用它们在利用全局统计信息和强大的局部拟合能力方面的互补优势。此外，为了更好地聚合跨窗口信息，作者引入了一个重叠的交叉注意力模块，增强了相邻窗口特征之间的交互作用。在训练阶段，还采用了相同任务的预训练策略，以进一步挖掘模型的潜力以获得更好的性能。</font>

![[Pasted image 20240314132533.png]]

	如上图所示，整体网络由三个部分组成，包括浅层特征提取、深层特征提取和图像重建。这种架构设计在之前的研究中得到了广泛应用。具体而言，对于给定的低分辨率 (LR)输入，首先利用一个卷积层提取浅层特征。然后，采用一系列残差混合注意力组 (RHAG)和一个 3 x 3 的卷积层进行深层特征提取。在此之后，图中添加了一个全局残差连接，将浅层特征和深层特征融合起来，然后通过重建模块重建高分辨率结果。
	此外，每个 RHAG 包含多个混合注意力块 (HAB)，一个重叠的交叉注意力块 (OCAB)和一个带有残差连接的 3 x 3 卷积层。对于重建模块，采用像素洗牌 (pixel-shuffle)方法来上采样融合的特征。同样地，本文中简单地使用 L 1 损失来优化网络参数。

<font color="#00b050">Hybrid Attention Block</font>

<font color="#002060">	<font color="#002060"><font color="#000000">	<font color="#000000">HAB 用于结合不同类型的注意力机制来激活更多的像素，以实现更好的重建效果。该模块由两个关键组成部分组成:窗口自注意力机制 (Window-based Self-Attention)和通道注意力机制 (Channel Attention)。</font></font></font></font>

<font color="#002060">		<font color="#000000">	在 HAB 模块中，首先将输入特征进行归一化处理，然后利用窗口自注意力机制对特征进行处理。窗口自注意力机制将输入特征划分为局部窗口，并在每个窗口内计算自注意力。这样可以捕捉到局部区域的关联信息。接下来，通过通道注意力机制，全局信息被引入，用于计算通道注意力权重。通道注意力机制能够利用全局信息对特征进行加权，从而激活更多的像素</font></font>
<font color="#002060">	</font>
<font color="#002060">	<font color="#000000">	HAB 模块的输出是窗口自注意力机制和通道注意力机制的加权和，并通过残差连接与输入特 HAB 征相加。这种设计使得网络能够同时利用局部和全局信息，从而实现更好的重建效果。</font></font>

<font color="#00b050">Overlapping Cross-Attention Block</font>
![[Pasted image 20240314132846.png]]

Overlapping Cross-Attention Block (OCAB)模块则通过引入重叠交叉注意力层，在窗口自注意力中建立了窗口之间的交叉连接，以增强网络的表征能力。该模块的设计可以更好地利用窗口内部的像素信息进行查询，从而提高重建任务的性能。