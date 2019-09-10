Groth09
======
[Linear Algebra with Sub-linear Zero-Knowledge Arguments](http://www0.cs.ucl.ac.uk/staff/J.Groth/MatrixZK.pdf)

对于算术电路：通讯量为`O(N^1/2)`个G点，常数次交互或者`O(logN)`次交互。证明计算量为`O(N/logN()`次ECC指数，验证计算量为`O(N)`次ECC乘。

对于由NAND-gates（与非门）组成的2进制电路，通讯量为`O(N^1/2)`个G点，证明计算量和验证计算量都是`O(N)`次ECC乘。


要证明的东西是什么：
z=SIGMA(i=[1,m], x_i*y_i)
x_i和y_i是具有n个F(p)域中元素的向量。*表示一个双线性映射。
Fp * Fp -> Fp。似乎指内积，或者类似变种。
x_i和y_i是已经commit的Fr向量（意思是证明者已经提交commitment）。
z是一个已经commit的Fr。

我没搞清楚这个等式怎么和R1CS对应起来。
R1CS的每个约束是<A,X> * <B,X> = <C,X>。
<A,X>表示内积。A,B,C是常量向量，X是变量向量。

[Vandermonde matrix](https://en.wikipedia.org/wiki/Vandermonde_matrix)

3.1 同态Commitments
x_1, ...x_n，n个F(p)，
h, g_1,....g_n, h是随机选择的底（G）
c=h^r * g_1^x_1 * g_2^x_2 * ...

com(x;r) * com(x',r') = com(x+x', r+r')

3.2 给了Multi-exp的论文索引

3.3 Schwartz-Zippel定理
一个m元n次多项式，在域F(p)内随机给每个变量赋值
等于零的概率小于n/p。

通常用在多项式相等测试中，对于两个多元多项式poly1, poly2，如果对于随机选择的x_1,...x_m属于Zp,有poly1(x_1,...x_m)-poly2(x_1,...x_m)=0，那么这两个多项式不相等的概率为max(d1,d2)/p。

4 矩阵和向量方程

T表示转置矩阵
`XI`,`YI`,`Z`是committed `n*n`的矩阵
`x_i`,`y_i`,`z`是committed `n`个变量的row向量
`sz'`是标量，committed Fr。
公共参数ai。

6种形态
T(z) = SIGMA(i=[1,m], a_i*X_i*T(y_i))

统一写成
sz=SIGMA(i=[1,m], x_i*y_i)
其中*是一个双线性映射。可以是标准内积，也可以是y和t先hadamard积，然后和x做内积，t是验证者选择的公共向量。

4.1,4.2,4.3解释了为什么可以这么转换以及怎么转换。

大概原理就是反复把多个方程转为一个方程。转换的方式如下：
考虑到x,y,z都是committed的值，由B给出一组挑战r，
要求A计算<z,r>，并且通过x,y的committment证明<z,r>。

如果要用groth09证明r1cs或其他通用电路，那么需要仔细研究这3节。

最终需要证明：
sz=SIGMA(i=[1,m], x_i*y_i)
其中*是一个双线性映射。可以是标准内积，也可以是y和t先hadamard积，然后和x做内积，t是验证者选择的公共向量。
注意这里x_i, y_i是个向量。

5.1 
首先考虑m=1，也就是只证明一个内积，也即证明z=<x,y>，还有一种形式是z=<x,yt>。因为已知y和t的commitment，很容易计算出yt的commitment，因此这两种形式实际上是一样的。
x,y是向量，z是标量
commitments是
a=com(x,r), b=com(y,s), c=com(z,t)
r,s,t是3个随机Fr。
prover有x,y,z,r,s,t，要证明z=<x,y>
P公开a,b,c

P->V
选择随机向量d_x, d_y，随机数r_d,s_d,t_1,t_0。
a_d=com(d_x,r_d)
b_d=com(d_y,s_d)
c_1=com(x*d_y+d_x*y,t_1)
c_0=com(d_x*d_y,t_0)
发送a_d, b_d, c_1, c_0

V->P
发送随机挑战e(Fr)

P->V
f_x=ex+d_x
f_y=ey+d_y
r_x=er+r_d
s_y=es+s_d
t_z=e^2*t+et_1+t_0
发送f_x, f_y, r_x, s_y, t_z

V验证:
a^e * a_d == com(f_x, r_x)
b^e * b_d == com(f_y, s_y)
c^(e^2) * c_1^e * c_0 == com(f_x * f_y , t_z)


5.2
m>1，也即证明z=<x_1,y_1> + <x_2,y_2> + ... <x_m, y_m>
commitments是
a_1=com(x_1,r_1),a_2=com(x_2,r_2) ... a_m=com(x_m,r_m)
b_1=com(y_1,s_1),b_2=com(y_2,s_2) ... b_n=com(y_m,s_m)
c=com(z,t)

P->V
选择随机Fr t_1,t_2,...t_(2m-1)，但是t_(m-1)=t
计算c_1=com()

V->P
发送随机挑战e(Fr)

P->V
双方计算
a'
b'
c'
P计算
x'
r'
y'
s'
t'
z'
公开a',b',c'

V验证


suxm版本
下面用SIGMA()表示求和，x o y表示hadamard积，<x, y>表示内积。用x_i表示x向量的第i个元素。

A持有数据x,y,z，其中x,y,z都是m*n的矩阵，于是x_i，y_i，z_i都是一个长度为n的向量。
A要证明z = x o y。

A公布a_i, b_i，c_i，i从1到m。
其中a_i = com(x_i), b_i = com(y_i), c_i = com(z_i)。

B挑战k, t。k是一个长度为m的向量，t是一个长度为n的向量。
令z' = SIGMA(<k_i * x_i, y_i o t>)
A计算z'，c'=com(z')，公布c'。
A用5.2的方法证明z'。

令z'' = <SIGMA(k_i * z_i), t>)，很显然有z''=z'，因此A无需公布c''=com(z'')。
由于这里t是常量，所以A并不需要用5.2的方法证明z''，此处可以用一个更简化的方法（hyrax 附录a2）证明。

