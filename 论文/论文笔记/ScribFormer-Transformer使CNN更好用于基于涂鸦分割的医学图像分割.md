ScribFormer: Transformer Makes CNN work better for scribble-based medical image segmentation

![[Pasted image 20240311213204.png]]

==混合 CNN-Transformer 编码器==
CNN 分支采用特征金字塔结构，随着级数增加，分辨率降低，通道数增加，每个卷积块包括下投影，空间，上投影卷积。下投影卷积降低空间维度，空间卷积提取局部特征，上投影卷积增加特征图大小。CNN 分支为 Transformer 分支提供局部特征，Transformer 分支处理全局信息。
==ACAM 分支==
意在识别训练网络中最相关区域，与语义分割模型兼容，通过结合通道和空间注意力调制来生成，提取次要特征并建模通道-空间关系。具体而言，特征的灵敏度通过空间平均池化和卷积层来建模，信道注意力调制中的高斯调制函数利用高斯函数的分布，其放大均值附近的权重，该机制增强了与主要特征相关联的区域的重要性。这种集成提升了模型处理复杂特征互连的能力，促进了从关键区域提取有价值信息的精确分割。
==混合监督学习==
1. 涂鸦监督学习，将部分交叉熵函数应用于涂鸦监督学习，忽略涂鸦注释中未标记的像素，涂鸦监督损失公式为：

${{L}_{ss}}(s,{{y}_{cnn}},{{y}_{trans}})=\frac{{{L}_{ce}}({{y}_{cnn}},s)+{{L}_{ce}}({{y}_{trans}},s)}{2}$

${{L}_{\text{ce}}}(y,s)=\sum\limits_{i\in {{\Omega }_{l}}}{\sum\limits_{k\in K}{-s_{i}^{k}\log (y_{i}^{k})}}$
   2. 伪监督学习，基于 cnn 和 transformer 分支之间感受野的差异，通过动态混合 cnn 分支预测 ycnn 和 transformer 分支预测 ytrans 来生成硬伪标签，然后分别用于监督两个预测，伪标签损失公式：
${{L}_{\text{pl}}}=average({{L}_{dice}}({{y}_{cnn}},Y),{{L}_{dice}}({{y}_{trans}},Y))$

$Y=\arg \max (\alpha \times {{y}_{cnn}}+\beta \times {{y}_{trans}}),\alpha ,\beta \in (0,1)$


   3. 一般一致性学习意在确保在数据级别上的平滑预测，即相同数据在不同变换和扰动下的预测应该是一致的，通过像素级的深特征和浅特征之间一种新的 ACAM 一致性评价加强特征级一致性，此外，该方法还可以引入隐式形状约束，ACAM 一致性损失公式为：
${{L}_{acam}}=\sum\limits_{i}{{{w}_{i}}\times {{L}_{ce}}(F({{E}_{i...4}}({{c}_{i}}),F({{c}_{5}})))}$

总的损失函数为：
${{L}_{\text{total}}}={{\lambda }_{1}}\times {{L}_{ss}}+{{\lambda }_{2}}\times {{L}_{pl}}+{{\lambda }_{3}}\times {{L}_{acam}}$
