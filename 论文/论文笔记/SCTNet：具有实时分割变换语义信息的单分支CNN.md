SCTNet: Single-branch CNN with transformer semantic information for real-time segmentation

https://arxiv.org/abs/2312.17071
https://github.com/xzz777/SCYNet

SCTNet 在保留轻量级单分支 CNN 高效性的同时，还拥有语义分支的丰富语义表示。考虑到 transformer 提取长距离上下文的卓越能力，SCTNet 将 transformer 作为仅用于训练的语义分支。借助于提出的 transformer 类 CNN 块 CFBlock 和语义信息对齐模块，SCTNet 可以在训练中从 transformer 分支捕获丰富的语义信息。

![[Pasted image 20240327152133.png]]

Training Phase 众所周知，Transformer 在捕获全局语义上下文方面表现出色。另一方面，CNN 已被证明比变换器更适合于对分层局部信息进行建模。受 Transformer 和 CNN 优点的启发，我们探索配备一个具有这两种优点的实时分割网络。我们提出了一个单分支 CNN，它学习将其特征与强大的 Transformer 的特征对齐。这种特征对齐使单分支 CNN 能够提取丰富的全局上下文和详细的空间信息。具体而言，SCTNet 采用了一个仅作用在训练阶段的 Transformer 作为语义分支来提取强大的全局语义上下文，语义信息对齐模块监督卷积分支以对齐来自 Transformer 的高质量全局上下文。
Inference Phase 为了避免两个分支的巨大计算成本，在推理阶段只部署了 CNN 分支。利用 transformer 对齐的语义信息，单分支 CNN 可以生成准确的分割结果，而无需额外的语义或昂贵的密集融合。更具体地说，输入图像被送入到单分支层次卷积主干中，解码器头拾取主干中的特征并进行简单的拼接进行像素分类.