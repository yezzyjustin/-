BRAU-Net++: U-Shaped hybrid CNN-Transformer network for medical image segmentation

==Preliminaries: Bi-Level Routing Attention==
双级路由注意力 BRA 是一种动态的，Query 感知的稀疏注意力机制，其核心思想是在粗糙粒度的区域 Level 过滤掉最不相关的 Key-Value 对，只保留大多数相关路由区域的一小部分，以实现细粒度的 Token-Token 注意力。与其他手工制作的静态稀疏注意力机制相比，BRA 更容易建模长依赖性，这一点与原始注意力相似，但是注意力的复杂度明显降低。

![[Pasted image 20240313203544.png]]

这个过程如图所示：
![[Pasted image 20240313203610.png]] ${{K}^{\text{g}}}=gather(K,{{I}^{r}}),{{V}^{g}}=gather(V,{{I}^{r}})$

$O=\text{soft}\max (\frac{Q{{({{K}^{g}})}^{T}}}{\sqrt{C}}){{V}^{g}}+LCE(V)$

其中，Kg，Vg 是收集到的 Key 和 Value 张量，函数 LCE 使用深度卷积进行参数化

==Architecture Overview==

![[Pasted image 20240313204122.png]]

BRAU-Net++的整体架构，包括编码器，解码器，Neck 和 SCCSA 模块。对于编码器，给定一个大小为 H·W·3 的输入医学图像，分成重叠的 patch，每个 patch 的特征维数为（定义为 C）的任意维度。通过 patch 嵌入将转换后的 patch 标记传递给多个 biformer 块和 patch 合并层以生成层次特征表达。具体而言，patch 合并用于降低特征图的分辨率并增加维度，而 biformer 块用于学习特征表示，对于 neck，特征图的分辨率和维度保持不变。

解码器基于对称的 transformer，由 biformer 块和 patch 扩展层组成，patch 层负责上采样和降低维度，通过 SCCSA 模块将提取的上下文特征与编码器中的多尺度特征融合，以补充下采样造成的空间信息损失并增强全局维度交互。最后，shiyongpatch 扩展层进行 4 倍上采样以恢复特征图的原分辨率 H·W，然后使用线性投影层生成像素级分割预测。

==Biformer Block==
![[Pasted image 20240313204959.png]]

==Skip Connection Channel-Spatial Attention==

![[Pasted image 20240313205101.png]]

==Loss Function==

![[Pasted image 20240313205142.png]]
