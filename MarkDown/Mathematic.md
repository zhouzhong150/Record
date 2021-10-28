[toc]

# s.t. = subject to



### P-norm

$X = [x_1,x_2,...,x_n]$， 那么向量 $X$ 的 $L_p$ 范数定义如下:
$$
||x||_p = \sqrt[p] {\sum_{i}|x_i|^p}
$$
$L_0$

$L_1$

$L_2$

...







## Vector



scalar product = dot product

$a·b = a_1b_1+a_2b_2 + ...+a_nb_n$



out product

$a\times b =  |a|·|b|·sin<a,b>$



 



### Conjugate Transpose

can be motivated by nothing that complex numbers can be usefully representated by 2 $\times$ 2 real matrices, obeying matrix addition and multiplication.







#### Gradient & Divergence & Curl

- $\bigtriangledown$ 

- $\bigtriangledown .$

- $\bigtriangledown \times$

#### Possion's equation

- ##### Laplacian operator: The Div of Gradient $\bigtriangledown . \bigtriangledown$ \ $\bigtriangleup$ \ $\bigtriangledown^{2}$

Passion`s equation:
$$
\bigtriangledown^{2} \varphi = f
$$






### FFT(Fast Fourier Transform)

https://www.cnblogs.com/zwfymqz/p/8244902.html

点值表示法

欧拉公式

分治法

$O(n^2)$ - > $O(nlogn)$

回到系数表示法

![img](https://images2017.cnblogs.com/blog/1101696/201802/1101696-20180211213536607-322408834.png)





### CG(Conjugate Gradient)





### preconditioning





### Nesterov Method

Momentum的改进

Momentum改进自SGD算法，让每一次的参数更新方向不仅仅取决于当前位置的梯度，还受到上一次参数更新方向的影响：









### Singular matrix & nonsingular matrix

如果A(n×n)为奇异矩阵（singular matrix）<=> A的秩Rank(A)<n.

如果A(n×n)为非奇异矩阵（nonsingular matrix）<=> A满秩，Rank(A)=n.





### Hessian matrix

在工程实际问题的优化设计中，所列的目标函数往往很复杂，为了使问题简化，常常将目标函数在某点邻域展开成泰勒多项式来逼近[原函数](https://baike.baidu.com/item/原函数)。

## 利用黑塞矩阵判定多元函数的极值

### 定理

设n多元实函数

![img](https://bkimg.cdn.bcebos.com/formula/dfa63f4ba875eaae5be84d45fc4ce387.svg)

 在点

![img](https://bkimg.cdn.bcebos.com/formula/daf80378ed2d4a03dc5a765556ba40ec.svg)

 的邻域内有二阶连续偏导，若有：

![img](https://bkimg.cdn.bcebos.com/formula/02374c98dfc3aa3221cf80240fb703cf.svg)

并且

![img](https://bkimg.cdn.bcebos.com/formula/0a08658635702aa55f5c99fdb34f6654.svg)

则有如下结果：

（1）当A[正定矩阵](https://baike.baidu.com/item/正定矩阵)时，

![img](https://bkimg.cdn.bcebos.com/formula/dfa63f4ba875eaae5be84d45fc4ce387.svg)

 在

![img](https://bkimg.cdn.bcebos.com/formula/daf80378ed2d4a03dc5a765556ba40ec.svg)

 处是极小值；

（2）当A[负定矩阵](https://baike.baidu.com/item/负定矩阵)时，

![img](https://bkimg.cdn.bcebos.com/formula/dfa63f4ba875eaae5be84d45fc4ce387.svg)

 在

![img](https://bkimg.cdn.bcebos.com/formula/daf80378ed2d4a03dc5a765556ba40ec.svg)

 处是[极大值](https://baike.baidu.com/item/极大值)；

（3）当A[不定矩阵](https://baike.baidu.com/item/不定矩阵)时，

![img](https://bkimg.cdn.bcebos.com/formula/daf80378ed2d4a03dc5a765556ba40ec.svg)

 不是极值点。

（4）当A为[半正定矩阵](https://baike.baidu.com/item/半正定矩阵)或半负定矩阵时，

![img](https://bkimg.cdn.bcebos.com/formula/daf80378ed2d4a03dc5a765556ba40ec.svg)

 是“可疑”[极值点](https://baike.baidu.com/item/极值点)，尚需要利用其他方法来判定。 [3] 

### 实例

求三元函数

![img](https://bkimg.cdn.bcebos.com/formula/4d7b07099b2d7632cf3bf6025bd89212.svg)

的极值。

解：因为

![img](https://bkimg.cdn.bcebos.com/formula/6e6962824c7ec9ed23e1578de0186ff0.svg)

，故该三元函数的驻点是

![img](https://bkimg.cdn.bcebos.com/formula/81fa642189da96faebbfa49a3af98896.svg)

。

又因为

![img](https://bkimg.cdn.bcebos.com/formula/28bdb88b07bc586e43e281e9bee12d4b.svg)

，

故有：

![img](https://bkimg.cdn.bcebos.com/formula/afa0880845e53fbd23362421c747fa41.svg)

因为A是正定矩阵，故

![img](https://bkimg.cdn.bcebos.com/formula/81fa642189da96faebbfa49a3af98896.svg)

是极小值点，且极小值

![img](https://bkimg.cdn.bcebos.com/formula/c48dc89309cf810dcba890fd9048c3ad.svg)
