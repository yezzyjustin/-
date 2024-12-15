简单来说就是六个encoder和decoder的叠加，每个block都有自己独特的参数，不共享参数。
![[v2-169f71f41174a67c4a96b2ffafbbc1e5_720w.jpg]]

每一层的结构如下，每个encoder都可以近似看成是由multihead self attention+dense组成的，每个encoder的输入是512×m，输出是512×m。m是输入序列的长度。

每个decoder都可以近似看成是由multihead self attention+multihead attention+dense组成的，实际上就是自注意力机制。每个decoder的输入是两个序列（512×m，512t），输出是一个序列512×t。
![[v2-05c6fedf169482fc68f6b09057c685ff_720w.jpg]]
那什么是self attention呢？
![[v2-a26e1e6a26e5c5fb5f7ad8d5f8b2c51e_720w.jpg]]
![[v2-c70553f56bddea806bb2f9a4d2daf6a7_720w.webp]]
Multi-Head Attention结构
Encoder和Decoder结构中公共的layer之一是Multi-Head Attntion，其是由多个Self-Attention并行组成的。Encoder block只包含一个Multi-Head Attention，而Decoder block包含两个Multi-Head Attention（其中有一个用到Masked）
![[v2-5737d8f7769ed656e2d345bf4f9a7411_720w.jpg]]
Multi-Head Attention是基于Self-Attention的一种变体，MHA在SA的基础上引入了多头机制，将输入拆分为多个子空间，每个空间分别执行SA，最后将多个子空间的输出拼接在一起并进行线性变换，从而得到最终的输出。
对于MHA，之所以需要对Q,K,V进行多头划分，其目的是为了增强模型对不同信息的关注，具体来说，多组Q,K,V分别计算self-attention，每个头自然就会有独立的Q,K,V参数，从而让模型同时关注多个不同的信息，这类似于多通道机制。

Attention细节
在self-attention中，Q,K,V是在同一个输入（比如序列中的一个单词）上计算得到的三个向量，具体来说，我们可以通过对原始输入词的embedding进行线性变换（比如使用一个全连接层），来得到Q,K,V。这三个向量的维度通常都是一致的，取决于模型设计时的决策。
![[v2-4deef09c7e4202290e84f1e89ab17d2e_720w.png]]
在计算self-attention时，Q,K,V被用来计算注意力分数，即用于表示当前位置和其他位置之间的关系。注意力分数可以通过Q和K的点积来计算，然后将分数除以8，再经过一个softmax归一化处理，得到每个位置的权重，然后用这些权重来加权计算V的加权和，即得到当前位置的输出。