AS3ITransUNet：Spatial-spectral interactive transformer U-Net with alternating sampling for hyperspectral image super-resolution
https://github.com/liushiji666/AS3-ITransUNet
提出具有交替采样的空间光谱交互式 transformer，采用组重建策略减少 HSI 高光谱维度造成的计算复杂度。设计了交替上采样和下采样的 unet，将学习复杂映射关系的任务分配给了 unet 的每个阶段，为了充分提取 HSI 的空间频谱特征，提出了空间频谱交互变换器块并将其集成到 unet 的编码器和解码器中，SSIT 模块包含一个跨分支双向交互模块，进一步捕获空间和光谱维度之间的互补信息，同时提出了多级互补信息学习 MSCIL 来捕获相邻 HSI 组中的互补信息，用于恢复当前 HSI 中缺失的细节。

![[Pasted image 20240313163954.png]]

![[Pasted image 20240313164058.png]]

现有的 transformer 模型可以捕获长程依赖性，直接使用 HSI 超分光谱维度上的自注意力可能会将焦点集中在一些低置信度和信息量较少的空间区域，提出了 SSIT 块克服这个问题，它由嵌入，空间分支，谱分支，双向交互和前馈网络组成。

![[Pasted image 20240313164411.png]]