SERNet-Former: Semantic Segmentation by efficient residual network with attention-boosting gates and attention-fusion networks
[GitHub - serdarch/SERNet-Former](https://github.com/serdarch/SERNet-Former/tree/main)

	在 2 D 场景的语义分割中识别物体得挑战是双重的：在图像通过网络处理的过程中，标记的物体可能会失去其空间信息或丰富的特征属性。最近的最新网络也试图客服给定图像输入的全局和局部上下文之间语义信息的不一致性。
	
	增加卷积层的数量并不能总是能与其计算成本正比地提高效率。
	设计注意力增强门AbGs和注意力增强模块AbMs，以激发并将丰富的特征空间信息融合到现代网络中。相应地，通过将注意力增强门引入到选定的基准网络，作者的Head网络得到了提升，因为他们有效地提高了在生成特征图时激发语义信息的概率。注意力增强模块将AbGs与Head网络的空间语境融合，从而产生了一种新颖的高效残差网络。

**The efficient residual network head: Encoder with attention-boosting modules**
注意力增强门意在激发可能未通过 Relu 层过滤的通道丰富语义信息，sigmoid 函数的输出可能增加生成和评估可能未通过残差网络激活的特征图的可能性。

在这方面，sigmoid 函数，它在注意力网络中广泛运用，被选为操作符以增加获取和处理通道和特征基础的丰富语义信息的可能性，这些语义信息在常用的 baseline 架构中可能未被激活，门操作 AbG 可以在如下方程中被迭代：
![[Pasted image 20240314105451.png]]


![[Pasted image 20240314105735.png]]

相应地，注意力增强模块，AbM，将获取并处理过的基于特征的语义信息与残差网络的空间上下文融合在一起，如图 2（a）所示，该乘积通过逐元素加法融入到残差网络中，并与卷积层的过滤输出相结合。

AbMs 在每一个第 n 个卷积层的末尾被添加到基准中，它作为一个数学运算符，用于激发并融合富含特征的空间语义信息。
