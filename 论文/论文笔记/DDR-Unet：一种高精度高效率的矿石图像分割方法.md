DDR-Unet-A high-accuracy and efficient ore image segmentation method

BCE Loss 损失函数不适合矿石图像分割，因为其图像中存在严重的类不平衡问题，属于矿石颗粒的像素数量远远小于属于背景的像素数量，这使得 BCE Loss 损失函数偏向于多数类。提出根据每个像素的类别调整其权重的自适应损失函数

$L\text{oss}=-(\frac{\left| {{Y}_{-}} \right|\sum{_{j\in {{Y}_{+}}}\log {{{\hat{p}}}_{j}}}}{\left| {{Y}_{+}}\cup {{Y}_{-}} \right|}+\frac{\left| {{Y}_{+}} \right|\sum{_{j\in {{Y}_{-}}}\log (1-{{{\hat{p}}}_{j}})}}{\left| {{Y}_{+}}\cup {{Y}_{-}} \right|})$

![[Pasted image 20240311175916.png]]