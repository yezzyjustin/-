https://arxiv.org/abs/2203.03952
![[v2-4bc53deb00ba2201c8dd8bc88a3a0a5a_b.jpg]]

具体来说，提出了带有位置嵌入的全局循环卷积（GCC），这是一种轻量级的卷积运算，它拥有全局感受野，同时产生与局部卷积一样的位置敏感特性。

![[v2-b50e639f0c12fd75af1b751f6c179016_b.jpg]]

将 GCC 和 squeeze-exictation操作结合起来，形成一个类似于元模型的模型块，它还具有类似于Transformer的注意力机制。上述块可以即插即用的方式使用，以替换 ConvNets 或Transformer中的相关块。
