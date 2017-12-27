#### 指数族分布与广义线性模型

* 指数族分布】

  指数族分布 (The exponential family distribution),区别于指数分布（exponential distribution)。在概率统计中，若某概率分布满足下式，我们就称之属于指数族分布。

  ​	$p(y;\eta)=b(y)exp(\eta^{T}T(y)-a(\eta))$

  其中η是natural parameter, T(y)是充分统计量, exp−a(η))是起到归一化作用。 确定了T,a,b,我们就可以确定某个参数为η的指数族分布.

  可以这样去理解指数族分布：在参数是$\eta$的时候$y$成功的概率是$b(y)exp(\eta^{T}T(y)-a(\eta))$

  常见的有伯努利分布，高斯分布，多项分布（multionmal), 泊松分布等。下面介绍其中的伯努利分布和高斯分布。

  * 伯努利分布：$p(y:\phi )=\phi^{y}\cdot (1-\phi)^{1-y}$

  ​			$p(y:\phi )=exp[y log(\phi)+(1-y)log(1-\phi)]$

  ​			$=exp[ylog\frac{\phi}{1-\phi}+log(1-\phi)]$

  ​	那么 $b(y)=1$

  ​		$\eta=log\frac{\phi}{1-\phi}$

  ​		$T(y)=y$

  ​		$a(\eta)=-log(1-\phi)=log(1+e^{\eta})$

  ​		在这里可以推出 $\phi=\frac{1}{1+e^{-\eta}}$

  ​		

  * 高斯分布(假设 $\sigma ^{2}=1$)：$p(y;u)=\frac{1}{\sqrt{2\pi}}exp(-\frac{1}{2}(y-u)^2)$

  ​							$=\frac{1}{\sqrt{2\pi}}exp(-\frac{1}{2}y^{2})exp(uy-\frac{1}{2}u^{2})$

  ​							

  ​				那么 $b(y)=\frac{1}{\sqrt{2\pi}}exp(-\frac{1}{2}y^{2})$

  ​					$\eta=u$

  ​					$T(y)=y$

  ​					$a(\eta)=\eta^2/2$

  ​					在这里可以推出 $u=\eta$



* 广义线性模型.

  对于广义线性模型有3个假设

  		##### 1.  p(y|x;θ) 服从指数族分布

  		##### 2.  给了x, 我们的目的是为了预测T(y)的在条件x下的期望。一般情况T(y)=y, 这就意味着我们希望预	 测h(x)=E[y|x]

  		##### 3.  参数η和输入x 是线性相关的：η=θTx

  * 最小二乘：

    之前的博客有写过，最小二乘的推导使用了正态分布，其实就是指数族分布，

    ​				$h_{\theta}(x)=E[y|x;\theta]$

    ​					   $=u$

    ​					   $=\eta$

    ​					   $=\theta^{T}x$

  * Logistic 回归

    ​	Logistic 回归 返回的是 给的特征X 返回正例的概率 是伯努利分布：

    ​				$h_{\theta}(x)=E[y|x;\theta]$

    ​					   $=p(y=1|x;\theta)$

    ​					  $=\phi$

    ​					 $=\frac{1}{1+e^{-\eta}}$

    ​					$=\frac{1}{1+e^{-\theta^{T}x}}$

    ​					

  * Softmax 回归

    ​	Softmax 回归是一个多分类的模型，利用的是多项次分布也是一种指数族分布

    * 多项次分布

      假设有k个类别，那么$y\epsilon\{1,2,\cdots  ,k\}$ 相对应的参数$\phi _{1},\phi _{2},\cdots \phi _{k}$因为$\sum \phi_{i}=1$所以 参数只要k-1个就够了其中 $\phi_{k}=1-\sum_{i}^{k-1}\phi_{i}$

      这里定义一个函数和T()			$1(x)=\left\{\begin{matrix}1 &x=true \\ 0& x=fault\end{matrix}\right.$

      ​	$T(1)=\begin{bmatrix}1\\ 0\\ 0\\ \vdots \\ 0\end{bmatrix}$	$T(2)=\begin{bmatrix}0\\ 1\\ 0\\ \vdots \\ 0\end{bmatrix}$$\cdots$$T(k-1)=\begin{bmatrix}0\\ 0\\ 0\\ \vdots \\ 1\end{bmatrix}$$T(k)=\begin{bmatrix}0\\ 0\\ 0\\ \vdots \\ 0\end{bmatrix}$

    ​		那么 $p(y;\phi)=\phi_{1}^{1(y=1)}\phi_{2}^{1(y=2)}\cdots \phi_{k}^{1(y=k)}$

    ​				$=\phi_{1}^{1(y=1)}\phi_{2}^{1(y=2)}\cdots \phi_{k}^{1-\sum_{i=1}^{k-1}1(y=i)}$

    ​				$=\phi_{1}^{T(y)_{1}}\phi_{2}^{T(y)_{2}}\cdots \phi_{k}^{1-\sum_{i=1}^{k-1}T(y)_{i}}$

    ​				$=exp(T(y)_{1}log(\phi_{1})+T(y)_{2}log(\phi_{2})\cdots +(1-\sum_{i=1}^{k-1}T(y)_{i})log(\phi_{k}))$

    ​				$=exp(T(y)_{1}log(\phi_{1}/\phi_{k})+T(y)_{2}log(\phi_{2}/\phi_{k})\cdots T(y)_{k-1}log(\phi_{k-1}/\phi_{k})+log(\phi_{k}))$

    ​				$=b(y)exp(\eta^{T}T(y)-a(\eta))$

    ​		那么 $b(y)=1$

    ​					$\eta=\begin{bmatrix}log(\phi_{1}/\phi_{k})\\ log(\phi_{2}/\phi_{k})\\ \vdots \\ log(\phi_{k-1}/\phi_{k})\end{bmatrix}$

    ​					$a(\eta)=-log(\phi_{k})$

    ​					$\eta_{i}=log(\phi_{i}/\phi_{k})=\theta_{i}^{T}x$

    ​					$e^{\eta_{i}}=\phi_{i}/\phi_{k}​$

    ​					$\phi_{k}e^{\eta_{i}}=\phi_{i}$			

    ​					$\phi_{k}\sum_{i=1}^{k}e^{\eta_{i}}=\sum_{i=1}^{k}\phi_{i}=1$	

    ​					$\phi_{i}=\frac{e^{\eta_{i}}}{\sum_{j=1}^{k}e^{\eta_{j}}}$

    ​						$=\frac{e^{\theta_{i}^{T}x}}{\sum_{j=1}^{k}e^{\theta_{j}^{T}x}}$

    ​		 所以输出的

    ​					$h_{\theta}(x)=E[T(y)|x;\theta]$

    ​						$=\begin{bmatrix}\left.\begin{matrix}1(y=1)\\ 1(y=2)\\ \vdots \\ 1(y=k-1)\\\end{matrix}\right|x;\theta\end{bmatrix}$

    ​						$=\begin{bmatrix}\phi _{1}\\ \phi _{2}\\ \vdots \\ \phi _{k-1}\end{bmatrix}$

  损失函数为：

  ​		$l(\theta)=\sum_{i=1}^{m}log(p(y^{}(i)|x^{}(i);\theta)$

  ​			$=\sum_{i=1}^{m}log\prod_{l=1}^{k}(\frac{e^{\theta_{l}^{T}x^{(i)}}}{\sum_{j=1}^{k}e^{\theta_{j}^{T}x^{(i)}}})^{1(y^{(i)}=l)}$