探索标准的transformer块可以简化到什么程度
1--skip connections
2--projection/value matrices
3--sequential sub-blocks
4--normalisation layers

信号传播理论：·信号传播理论有助于我们分析一个网络结构设计的是否良好
	         ·随着网络的加深，对不同的输入已经无法区分，这显然不是一个良好的网络
残差网络：·残差块
          ·跳跃链接
残差降权的思想：·剪枝操作，评估每个权重的重要性
              ·downweight the residual branch relative to the skip branch
无残差结构skipless：·在mlp和cnn中，非线性激活函数-->更线性，即使没有跳跃链接，也可                                     以实现良好的信号传播
				  ·使用标准优化器，无残差结构会损失速度

![[Pasted image 20240202154840.png]]

两种LN顺序

![[Pasted image 20240202155030.png]]

![[Pasted image 20240202155503.png]]