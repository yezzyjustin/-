 Rethinking Attention: Exploring shallow feed-forward neural networks as an alternative to attention layers in transformers
 Https://arxiv.Com/abs/2311.10642

https://github.com/vulus98/Rethinking-attention

能否直接使用更轻量级的浅层 MLP 来模拟注意力机制的计算？
	1）注意力替换（ALR）：仅用 MLP 替换多头注意力（MHA）块，保留残差链接和归一化层
	2）残差链接替换注意力层（ALRR）：MHA 模块以及残差链接被 MLP 替换
	3）注意力头分离替换（ASLR）：ALR 的变体，该方法用单独的 MLP 替换 MHA 模块的每个单独头
	4）编码器层替换（ELR）：完全使用 MLP 替换编码器层
![[Pasted image 20240313155502.png]]

其中 ALR 和 ALRR 的设计灵感是将注意力层的性能提升和残差链接的性能提升分离开，而 ASLR 则是用来模拟多头注意力层中每个单独头的操作，即直接使用 MLP 来代替多头注意力 MHA，而 ELR 作为最高的抽象级别，直接将整个编码器块替换为 MLP 网络。