 Gold-YOLO: Efficient Object Detector via Gather-and-Distribute Mechanism
[arxiv.org/pdf/2309.11331.pdf](https://arxiv.org/pdf/2309.11331.pdf)
[Efficient-Computing/Detection/Gold-YOLO at master · huawei-noah/Efficient-Computing · GitHub](https://github.com/huawei-noah/Efficient-Computing/tree/master/Detection/Gold-YOLO)



为了避免不停迭代的间接融合不同 level 的信息，提出 gather-and-distribute 机制
![[Pasted image 20240327155551.png]]

其中 GD 主要分为三个模块：FAM 特征对齐模块，IFM 信息融合模块，Inject 信息注入模块。

**low-GD 模块**
low-GD 主要用于融合模型浅层的特征信息，此处为 B2, B3, B4, B5 的特征，包含上述说的 2 个模块
![[Pasted image 20240327160137.png]]
是一个 FAM 模块，用下图公式表达，即先将 B 2, B 3, B 4, B 5 统一到 B 4 的[h/4, w/4]的尺寸，然后在 channelEconcat,
Falign=Low-FAM ([B 2, B 3, B 4, B 5])

Low-IFM 模块
将 FAM 的输出先经过多层 RepBlock 提取信息，然后再按 channel 分成 2 部分，如下图公式，得到 inj_p3 和 inj_p4，作为后续 Inject 的输入，此处得到的特征图作为 global 信息。
Ffuse=RepBlock (Falign)
Finj_P3, Finj_P 4=Split (Ffuse)

Inject 模块
Inject 模块分成 Inject 和带 LAF 模块的 Inject. LAF 网络介绍见 2.5 即可。Low-inject 的具体过程用下图公式表达，其中 global 代表是用 FAM 模块将多层融合一起的特征，local 代表的是每层的特征，比如 B2
![[Pasted image 20240327160610.png]]

**LAF 模块**
LAF 是一个轻量的邻层融合模块，如下图所示，对输入的 local 特征 (Bi 或 pi)先和邻域特征融合，然后再走 inject 模块，这样 local 特征图也具有了多层的信息。
![[Pasted image 20240327160657.png]]