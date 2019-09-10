Groth09
======
[Linear Algebra with Sub-linear Zero-Knowledge Arguments](http://www0.cs.ucl.ac.uk/staff/J.Groth/MatrixZK.pdf)

����������·��ͨѶ��Ϊ`O(N^1/2)`��G�㣬�����ν�������`O(logN)`�ν�����֤��������Ϊ`O(N/logN()`��ECCָ������֤������Ϊ`O(N)`��ECC�ˡ�

������NAND-gates������ţ���ɵ�2���Ƶ�·��ͨѶ��Ϊ`O(N^1/2)`��G�㣬֤������������֤����������`O(N)`��ECC�ˡ�


Ҫ֤���Ķ�����ʲô��
z=SIGMA(i=[1,m], x_i*y_i)
x_i��y_i�Ǿ���n��F(p)����Ԫ�ص�������*��ʾһ��˫����ӳ�䡣
Fp * Fp -> Fp���ƺ�ָ�ڻ����������Ʊ��֡�
x_i��y_i���Ѿ�commit��Fr��������˼��֤�����Ѿ��ύcommitment����
z��һ���Ѿ�commit��Fr��

��û����������ʽ��ô��R1CS��Ӧ������
R1CS��ÿ��Լ����<A,X> * <B,X> = <C,X>��
<A,X>��ʾ�ڻ���A,B,C�ǳ���������X�Ǳ���������

[Vandermonde matrix](https://en.wikipedia.org/wiki/Vandermonde_matrix)

3.1 ̬ͬCommitments
x_1, ...x_n��n��F(p)��
h, g_1,....g_n, h�����ѡ��ĵף�G��
c=h^r * g_1^x_1 * g_2^x_2 * ...

com(x;r) * com(x',r') = com(x+x', r+r')

3.2 ����Multi-exp����������

3.3 Schwartz-Zippel����
һ��mԪn�ζ���ʽ������F(p)�������ÿ��������ֵ
������ĸ���С��n/p��

ͨ�����ڶ���ʽ��Ȳ����У�����������Ԫ����ʽpoly1, poly2������������ѡ���x_1,...x_m����Zp,��poly1(x_1,...x_m)-poly2(x_1,...x_m)=0����ô����������ʽ����ȵĸ���Ϊmax(d1,d2)/p��

4 �������������

T��ʾת�þ���
`XI`,`YI`,`Z`��committed `n*n`�ľ���
`x_i`,`y_i`,`z`��committed `n`��������row����
`sz'`�Ǳ�����committed Fr��
��������ai��

6����̬
T(z) = SIGMA(i=[1,m], a_i*X_i*T(y_i))

ͳһд��
sz=SIGMA(i=[1,m], x_i*y_i)
����*��һ��˫����ӳ�䡣�����Ǳ�׼�ڻ���Ҳ������y��t��hadamard����Ȼ���x���ڻ���t����֤��ѡ��Ĺ���������

4.1,4.2,4.3������Ϊʲô������ôת���Լ���ôת����

���ԭ����Ƿ����Ѷ������תΪһ�����̡�ת���ķ�ʽ���£�
���ǵ�x,y,z����committed��ֵ����B����һ����սr��
Ҫ��A����<z,r>������ͨ��x,y��committment֤��<z,r>��

���Ҫ��groth09֤��r1cs������ͨ�õ�·����ô��Ҫ��ϸ�о���3�ڡ�

������Ҫ֤����
sz=SIGMA(i=[1,m], x_i*y_i)
����*��һ��˫����ӳ�䡣�����Ǳ�׼�ڻ���Ҳ������y��t��hadamard����Ȼ���x���ڻ���t����֤��ѡ��Ĺ���������
ע������x_i, y_i�Ǹ�������

5.1 
���ȿ���m=1��Ҳ����ֻ֤��һ���ڻ���Ҳ��֤��z=<x,y>������һ����ʽ��z=<x,yt>����Ϊ��֪y��t��commitment�������׼����yt��commitment�������������ʽʵ������һ���ġ�
x,y��������z�Ǳ���
commitments��
a=com(x,r), b=com(y,s), c=com(z,t)
r,s,t��3�����Fr��
prover��x,y,z,r,s,t��Ҫ֤��z=<x,y>
P����a,b,c

P->V
ѡ���������d_x, d_y�������r_d,s_d,t_1,t_0��
a_d=com(d_x,r_d)
b_d=com(d_y,s_d)
c_1=com(x*d_y+d_x*y,t_1)
c_0=com(d_x*d_y,t_0)
����a_d, b_d, c_1, c_0

V->P
���������սe(Fr)

P->V
f_x=ex+d_x
f_y=ey+d_y
r_x=er+r_d
s_y=es+s_d
t_z=e^2*t+et_1+t_0
����f_x, f_y, r_x, s_y, t_z

V��֤:
a^e * a_d == com(f_x, r_x)
b^e * b_d == com(f_y, s_y)
c^(e^2) * c_1^e * c_0 == com(f_x * f_y , t_z)


5.2
m>1��Ҳ��֤��z=<x_1,y_1> + <x_2,y_2> + ... <x_m, y_m>
commitments��
a_1=com(x_1,r_1),a_2=com(x_2,r_2) ... a_m=com(x_m,r_m)
b_1=com(y_1,s_1),b_2=com(y_2,s_2) ... b_n=com(y_m,s_m)
c=com(z,t)

P->V
ѡ�����Fr t_1,t_2,...t_(2m-1)������t_(m-1)=t
����c_1=com()

V->P
���������սe(Fr)

P->V
˫������
a'
b'
c'
P����
x'
r'
y'
s'
t'
z'
����a',b',c'

V��֤


suxm�汾
������SIGMA()��ʾ��ͣ�x o y��ʾhadamard����<x, y>��ʾ�ڻ�����x_i��ʾx�����ĵ�i��Ԫ�ء�

A��������x,y,z������x,y,z����m*n�ľ�������x_i��y_i��z_i����һ������Ϊn��������
AҪ֤��z = x o y��

A����a_i, b_i��c_i��i��1��m��
����a_i = com(x_i), b_i = com(y_i), c_i = com(z_i)��

B��սk, t��k��һ������Ϊm��������t��һ������Ϊn��������
��z' = SIGMA(<k_i * x_i, y_i o t>)
A����z'��c'=com(z')������c'��
A��5.2�ķ���֤��z'��

��z'' = <SIGMA(k_i * z_i), t>)������Ȼ��z''=z'�����A���蹫��c''=com(z'')��
��������t�ǳ���������A������Ҫ��5.2�ķ���֤��z''���˴�������һ�����򻯵ķ�����hyrax ��¼a2��֤����

