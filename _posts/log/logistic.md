#### 局部加权回归

* 我们都知道第一个数据集可以用一次线性回归，第二个呢使用2次的线性回归，有的时候数据的分布比较弯曲,就如下面第三个数据集，这个时候怎么去实现回归？

![屏幕快照 2017-12-25 上午8.20.06](/Users/mac/Desktop/Github/null-zxm.git.io/_posts/log/屏幕快照 2017-12-25 上午8.20.06.png)

这个时候就要使用局部加权回归了,

* 局部加权回归:就是如果你想要知道$x=x_{0}$时候$y$为多少呢？这个时候可以取在$x=x_{0}$附近的一些数据拟合为一条直线,然后带入求出$y$

  * 如何选择附近？我们认为,越接近$x_{0}$的哪些数据越有价值，离的远的就没有价值了,那么这个时候就要有一个权值,选择权值函数$w^{(i)}=exp(-\frac{(x^{(i)}-x_{0})^{2}}{2\Gamma ^{2}})$

  * 那么损失函数$L(\theta)=\sum w^{(i)}(h_{\theta}(x^{i})-y^{(i)})^{2}$

  * 理解这个损失函数：观察权值函数可以看出来，如果$x_{0} ,x^{(i)}$差值较大那么得到的值会趋向于0，如果差值很小那么值就会趋向1，这个函数被称为指数衰减函数，其中的$\Gamma$是波长参数,控制下降的速度，

  * 解极值.

    令$W=\begin{bmatrix}w^{(1)} & 0 &\cdots   & 0\\  0&   w^{(2)}& & 0\\  \vdots &   & \ddots  & \vdots \\ 0&  \cdots & 0 &  w^{(m)} \end{bmatrix}$

    $h_{\theta}(x^{i})=\theta^{T}x$

    $y=\begin{bmatrix}y^{(1)}\\ y^{(2)}\\ \vdots \\y^{(3)}\\ \end{bmatrix}$

    $X=\begin{bmatrix}x_{0}^{(1)} &x_{1}^{(1)} &\cdots &x_{n}^{(1)}\\ x_{0}^{(2)} &x_{2}^{(1)} &\cdots &x_{n}^{(2)}\\ \vdots  &\vdots &\cdots &\vdots\\ x_{0}^{(4)} &x_{4}^{(1)} &\cdots &x_{n}^{(4)}\\ \end{bmatrix}$

    那么损失函数$L(\theta)=(X\theta-y)^{T}W(X\theta-y)$

    对$\theta$求导那么$\triangledown _{\theta}L(\theta)=(X\theta-y)^{T}W(X\theta-y)$

    ​				$=\triangledown _{\theta}(\theta ^{T}X^{T}WX\theta -\theta ^{T}X^{T}Xy-y^{T}WX\theta +y^{T}Xy)$	

    ​				$=2X^{T}WX\theta-X^{T}Xy-X^{T}Xy$

    ​				$=2X^{T}WX\theta-2X^{T}Xy$

    驻点为$\triangledown _{\theta}L(\theta)=0\Rightarrow X^{T}WX\theta=X^{T}Xy$

    ​				$\Rightarrow \theta=(X^{T}WX)^{-1}X^{T}Xy$

    这样就可以求出$\theta$然后带入求出y



#### Logistic回归

* Logistic回归其实是一个分类的模型,对于那些$y\in \{0,1\}$的就不适合使用线性回归了，考虑下面的一个函数$g(z)=\frac{1}{1+e^{-z}}$图像是

  ![屏幕快照 2017-12-25 上午10.00.50](/Users/mac/Desktop/Github/null-zxm.git.io/_posts/log/屏幕快照 2017-12-25 上午10.00.50.png)

  对于这个函数可以求得!

  ![屏幕快照 2017-12-25 上午10.20.49](/Users/mac/Desktop/Github/null-zxm.git.io/_posts/log/屏幕快照 2017-12-25 上午10.20.49.png)

这个图像我们可以处理分类问题。可以从图上面可以看出来如果0<g(z)<1的那么可以当成概率来看吗？，所以我们可以假设

* $h_{\theta}(x)=g(\theta^{T}x)=\frac{1}{1+e^{-\theta^{T}x}}$

* 假设 $P(Y=1|x;\theta)=h_{\theta}(x)$

  ​	 $P(Y=0|x;\theta)=1-h_{\theta}(x)$

所以由极大似然估计推：

* $l(\theta)=\prod_{i=1}^{m}h_{\theta}(x)^{y^{(i)}}(1-h_{\theta}(x))^{1-y^{(i)}}$

* 取对数可得到

  $L(\theta)=lnl(\theta)=\sum_{i=1}^{m}y^{(i)}ln(h_{\theta}(x))+(1-y^{(i)})ln(1-h_{\theta}(x))$

  现在要求的是$L(\theta)$的最大值 利用梯度上升可以得到

  ![屏幕快照 2017-12-25 上午10.45.16](/Users/mac/Desktop/Github/null-zxm.git.io/_posts/log/屏幕快照 2017-12-25 上午10.45.16.png)

  所以 $\theta_{j} :=\theta_{j}+\alpha(y^{(i)}-h_{\theta}(x^{(i)}))x_{j}^{(i)}$

