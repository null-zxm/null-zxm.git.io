###最小二乘法

* 从$Ax=b$ 来讲这个算法. 都知道$Ax=b$的可以理解为$b$是$A$的列向量的线性组合那么也就是说，只有$b$在矩阵A的列空间上面的时候才会有解.如果$b$不在矩阵A的列空间上面的时候就会没有解.那么这个时候就需要一个最优解，大家都知道一个点距离一个平面最近的距离是在这个平面的投影.那么如果是n维呢？那我们就取向量$b$在A列空间上面的投影向量为最接近的向量，那么得到的x就是最优解了
* 取这个投影向量得到的解是$\hat{x}$那么投影向量就是$A\hat{x}$假设那个投影向量和b连接的向量为$e$那么$e=b-A\hat{x}$显然可知e正交A列空间所以$A^{T}e=0$所以可得到$A^{T}b=A^{T}A\hat{x}$推出$\hat{x}=(A^{T}A)^{-1}A^{T}b$那么这样就可以求出$\hat{x}$这个最优解了
  * 利用这个最优解可以解决哪些线性回归的问题，因为一些数据是不能拟合的所以要寻找最拟合的直线，这个时候就可以利用最小二乘法来解决这个问题，当然也可以用梯度下降法来解决。

### 梯度下降

* 什么是梯度下降？
  * (百度百科)梯度的本意是一个向量（矢量），表示某一函数在该点处的方向导数沿着该方向取得最大值，即函数在该点处沿着该方向（此梯度的方向）变化最快，变化率最大（为该梯度的模）。
  * (自己的理解)在我看来梯度就是给你的就是一个方向，就是告诉你,你的目标(极值)是在你的上方还是下方,就是说下一步你要往上还是下走,那么梯度下降法的意思就是始终往想要去的方向走去,一步一步慢慢找到你的目标,那么这个步的大小的选择很重要,如果太小了,那么想要到达目标的时间有点长，如果太大了可能到不了目的地或者离开的有点远,举个例子，你的步伐上5个单位，可是这个时候目的地在你上方2个单位，这个时候你往上走去，下一刻你发现过头了，这个时候又往下走，来来回回最近的时候也还是离目标2个单位.

### 例子（由Living area 和 Bedrooms 预测房价）

![屏幕快照 2017-12-24 下午4.07.50](/Users/mac/Desktop/屏幕快照 2017-12-24 下午4.07.50.png)

* 下面对于每一个数据用相关符号表示：$x_{i}^{(j)}​$表示第$j​$个数据的第$i​$个特征$\theta _{i}​$表示为第$i​$个特征的参数所以$h(\theta)=\theta _{0}+\theta _{1}x_{1}+\theta _{2}x_{2}​$可以取$\theta=\begin{bmatrix}\theta_{0} \\\theta_{1} \\\theta_{2} \end{bmatrix}​$ $x=\begin{bmatrix}1\\ x_{1}\\ x_{2}\end{bmatrix}​$那么$h(\theta)=\beta^{T} x​$

* 利用极大似然估计解释最小二乘

  * 对于 $y^{(i)}=\theta^{T}x^{(i)}+\epsilon^{(i)}$可以理解为对于第$i$个样本我们得到的值和真实的值的误差为$\epsilon^{(i)}$对于这个误差我们可以将其假设服从均值为0，方差为$\sigma ^{2}$的高斯分布那么由中心极限定理可以得

  * $p(\epsilon ^{i})=\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(\epsilon ^{(i)})^{2}}{2\sigma ^{2}})$

  * $p(y^{(i)}|x^{(i) };\theta)=\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(y^{(i)}-\theta ^{T}x^{(i)})^{2}}{2\sigma ^{2}})$

  * $L(\theta )=\prod_{i=1}^{m}p(y^{(i)}|x^{(i) };\theta)=\prod_{i=1}^{m}\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(y^{(i)}-\theta ^{T}x^{(i)})^{2}}{2\sigma ^{2}})$

  * $l(\theta )=logL(\theta )=log\prod_{i=1}^{m}\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(y^{(i)}-\theta ^{T}x^{(i)})^{2}}{2\sigma ^{2}})=\sum_{i=1}^{m}log\frac{1}{\sqrt{2\pi }\sigma }exp(-\frac{(y^{(i)}-\theta ^{T}x^{(i)})^{2}}{2\sigma ^{2}})=mlog\frac{1}{\sqrt{2\pi }\sigma }-\frac{1}{\sigma ^{2}}\cdot\frac{1}{2}\sum_{i=1}^{m}(y^{(i)}-\theta ^{T}x^{(i)})^2$

  * $J(\theta )=\frac{1}{2}\sum_{i=1}^{m}(y^{(i)}-\theta ^{T}x^{(i)})^2$

  * 上面的式子推导出要求得$J(\theta )$最小值就可以了

    * 求极值点

      目标函数$J(\theta )=\frac{1}{2}\sum_{i=1}^{m}(y^{(i)}-\theta ^{T}x^{(i)})^2=\frac{1}{2}(X\theta-y)^{T}(X\theta-y)$


    * $\bigtriangledown_{\theta }J(\theta )=\bigtriangledown_{\theta }(\frac{1}{2}(X\theta-y)^{T}(X\theta- y))$
    * $=\bigtriangledown_{\theta }(\frac{1}{2}(\theta ^{T}X^{T}-y^{T})(X\theta -y))$
    * $=\bigtriangledown_{\theta }(\frac{1}{2}(\theta ^{T}X^{T}X\theta -\theta ^{T}X^{T}y-y^{T}X\theta +y^{T}y))$
    * $=\frac{1}{2}(2X^{T}X\theta -X^{T}y-(y^{T}X)^{T})=X^{T}X\theta -X^{T}y=0$
    * 可以得到 $\theta=(X^{T}X)^{-1}X^{T}y$
    * 这个时候如果$X^{T}X$不可逆或者防止过拟合可以令 $\theta=(X^{T}X+\lambda I)^{-1}X^{T}y$

    ####梯度下降（重点)

    * 初始化$\theta$（随机）

    * 沿着负梯度方向迭代，

      $\theta =\theta - \alpha\cdot \frac{\partial J(\theta )}{\partial \theta } $

      可以得到$\frac{\partial J(\theta )}{\partial \theta_{j}}=(h_{\theta }(x)-y)x_{j}$

      所以批量梯度下降算法：

      Repeat until convergence{

      ​	$\theta_{j} :=\theta_{j} + \alpha\sum(y^{(i)}-h_{\theta}(x^{(i)}))x_{j}^{(i)}$

      }

    * 随机梯度下降算法

      随机梯度算法呢就是先选取一个样本使$\theta$往前走一步，之后再用另外一个样本继续迭代，虽然并不是每一次都是靠近的，但是大体的趋势是向极值靠拢的。

      Loop{

      ​	for i = 1 to m,{	

      ​	$\theta_{j} :=\theta_{j} + \alpha(y^{(i)}-h_{\theta}(x^{(i)}))x_{j}^{(i)}$

      ​	}

      }

    * mini-batch梯度下降

      取n个样本的平均梯度作为方向，这样距离极值点近而且速度不会太慢，

      Loop{

      ​	for t= 1 to m step=n,{	

      ​	$\theta_{j} :=\theta_{j} +\sum_{i=t}^{t+n} \alpha(y^{(i)}-h_{\theta}(x^{(i)}))x_{j}^{(i)}$

      ​	}

      }