SwiGLU: GLU Variants improve transformer
在 transformer 中，计算包含两部分，一部分是 Multi-head attention 以及 Add&Norm，另一部分为 Feed Forward 及 Add&Norm

FFN 是分两步的思想，通过这样的结构能增加模型的泛化能力，常见的 FFN 为：
$FFN(\text{x},{{W}_{1}},{{W}_{2}},{{b}_{1}},{{b}_{2}})=\max (0,x{{W}_{1}}+{{b}_{1}}){{W}_{2}}+{{b}_{2}}$

no-bias 形式：
$FF{{N}_{\operatorname{Re}\text{LU}}}(\text{x},{{W}_{1}},{{W}_{2}})=\max (x{{W}_{1}},0){{W}_{2}}$

GELU 和 Swish 形式：
$FF{{N}_{GELU}}(\text{x},{{W}_{1}},{{W}_{2}})=GELU(x{{W}_{1}}){{W}_{2}}$

$FF{{N}_{S\text{wish}}}(\text{x},{{W}_{1}},{{W}_{2}})=S\text{wis}{{\text{h}}_{1}}(x{{W}_{1}}){{W}_{2}}$

**GLU-FFN**
对于门控线性单元 GLU，输入传入两个前向网络 xW+b 和 xV+c，第一个网络输出用 sigmoid 激活后与第二个输出做积：
![[Pasted image 20240327133905.png]]

GLU 可以理解为第一部分通过激活函数实现门控，加权到第二部分上，常见的 GLU：
![[Pasted image 20240327133944.png]]
FFN 也具有两步走的计算属性，将 GLU 引入 FFN 中，GLU 的输出再与最外层的 W 2 相乘：
![[Pasted image 20240327134029.png]]
GLU-FFN 相比 FFN 多引入了一个权重矩阵，实验使用时，相比 FFN，W, V 的第二维，W 2 的第一维维度设置为 2/3