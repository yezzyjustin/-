FCMNet: Frequency-awarecross-modality attention networks for RGB-Dsalientobject detection

RGB 图像传达丰富的详细信息，如纹理和颜色，深度图包含更多的轮廓。常规注意力机制无法维持来自不同模态的所有频率成分。设计 FACMA 模块来自动提取和强化不同模式下的互补信息，在 FACMA 模块中，利用两个空间频率通道注意（SFCA）子模块从空间和频率域捕获互补信息。
![[Pasted image 20240311211116.png]]
![[Pasted image 20240311211205.png]]

DCT 是 2 D 离散余弦变换，比普通信道注意力能保留更丰富的特征，数学定义如下：
![[Pasted image 20240311211358.png]]

