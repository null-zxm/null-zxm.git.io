#### 牛顿法和拟牛顿法

* 今天看到书上说到牛顿法，啊哈哈这个我会啊,不就是$x^{(i+1)}=x^{(i)}-\frac{f(x^{(i)})}{f^{'}(x^{(i)})}$一次一次的迭代，为什么这样呢？可以用几何法很简单的，然而我看到的是$x^{(i+1)}=x^{(i)}-\frac{f^{'}(x^{(i)})}{f^{''}(x^{(i)})}$还有$\theta :=\theta-H^{-1}\triangledown _{\theta }f(\theta)$。OK！！一脸懵逼，开始了翻书的旅程了。下面主要是理解‘统计学习方法’这本书的附录中的内容。

* 牛顿法

  * 我原来停留的知识点只是针对一维的。是利用一介泰勒展开来证明的：

    对$l(x)$在$\theta_{k}$处展开可以得到：

    ​	$l(\theta)=l(\theta^{k})+g_{k}^{T}(\theta-\theta^{k})+\frac{1}{2}(\theta-\theta^{k})^{T}H(\theta^{k})(\theta-\theta^{k})$

    这里的$g_{k}=g(\theta^{(k)})=\triangledown _{\theta}l(\theta^{(k)})$

    ​	$		H(\theta^{(k)})$是$l(\theta)$的海赛矩阵

    ​	$H(x)=\begin{bmatrix}\frac{\partial ^{2}l}{\partial \theta _{i}\partial \theta _{j}}\end{bmatrix}_{n*n}$

我们要求极值点,那么这个时候必要条件是

​		$\triangledown _{\theta}l(\theta)=0$

所以对上面展开式两边求导,可以得到

​		$\triangledown _{\theta}l(\theta)=g_{k}+H_{k}(\theta-\theta^{(k)})$												———A-(1)

​		那么如果下一次迭代$\theta^{(k+1)}$就是极值了,那么取$\theta=\theta^{(k+1)}$则

​		$\triangledown _{\theta}l(\theta^{(k+1)})=g_{k}+H_{k}(\theta^{(k+1)}-\theta^{(k)})$得到

​		$g_{k}+H_{k}(\theta^{(k+1)}-\theta^{(k)})=0$ 所以

​		$\theta^{(k+1)}=(\theta^{(k)}-H_{k}^{-1}g_{k}$

​		令$p_{k}=-H_{k}^{-1}g_{k}$	将其叫做下降的方向，

* 算法(牛顿法)

  IN:     $l(\theta)        $  ，$g(\theta)=\triangledown l(\theta)$，$H(\theta)$，$\varepsilon $

  Out：$\theta^{*}$

  * Loop{

    ​	for k= 0 to more,{

    ​		$g_{k}=g(\theta^{(k)})$	

    ​		if 	$\left \| g_{k} \right \|<\varepsilon$	out:		 $\theta^{*}=\theta^{(k)}$

    ​		else 	$H_{k}=H(\theta^{(k)})$	$p_{k}=-H_{k}^{-1}g_{k}$	$\theta^{(k+1)}=\theta^{(k)}+p_{k}$

    ​	}

  * }

* 拟牛顿法思路

  牛顿法缺陷：计算$H^{-1}$比较复杂，考虑到用一个n阶的矩阵来代替它

  在上面的A-(1)式子中令$\theta=\theta^{k+1}$

  ​	$g_{k+1}-g_{k}=H_{k}(x^{(k+1)}-x^{(k)})$

  令$y_{k}=g_{k+1}-g_{k}$		$\delta _{k}=x^{(k+1)}-x^{(k)}$

  那么：$y_{k}=H_{k}\delta _{k}$

  ​	   $H_{k}^{-1}y_{k}=\delta _{k}$

  如果$H_{k}$正定，搜索的方向为$p_{k}$ 那么我们可以给这个方向加一个步长(学习率)，就好像梯度下降一样那么

  ​	$\theta^{(k+1)}=\theta^{(k)}+\lambda p_{k}$

  所以在泰勒展开中可以写成

  ​	$l(\theta^{(k+1)})=l(\theta^{(k)})-\lambda g_{k}^{T}H_{k}^{-1}g_{k} $

  ​	因为$\lambda$>0且$g_{k}^{T}H_{k}^{-1}g_{k}$>0 则$l(\theta^{(k+1)})<l(\theta^{(k)})$所以下降方向正确的

  * 拟牛顿法就是找一个和$H_{k}^{-1}$近似的$G_{k}$首先，$G_{k}$要是一个正定的，并且使得,$G_{k+1}y_{k}=\delta _{k}$

    WHY？这个是要解决什么呢？

    * 首先，$H_{k}^{-1}$不好计算，而且如果$H_{k}$非正定那么就无法计算了，所以用$G_{k}$代替。

    那么$G_{k}$需要根据$y_{k}  $和$\delta _{k}$来更新 $G_{k+1}=G_{k}+\Delta G_{k}$

  * DPF算法

    * DPF对$G_{k+1}$的更新是利用$G_{k+1}$加上两个附加项构成的，就是

      ​			$G_{k+1}=G_{k}+P_{k}+Q_{k}$

    要$G_{k+1}y_{k}=\delta _{k}$ 那么

    $(G_{k}+P_{k}+Q_{k})y_{k}=\delta _{k}$

    令$P_{k}y_{k}=\delta _{k}$

    ​    $G_{k}y_{k}=-Q_{k}y_{k}$

    可以求出来：

       $P_{k}=\frac{\delta _{k}\delta _{k}^{T}}{\delta _{k}^{T}y_{k}}$

      $Q_{k}=-\frac{G_{k}y_{k}y_{k}^{T}G_{k}}{y_{k}^{T}G_{k}y_{k}}$

     $G_{k+1}=G_{k}+\frac{\delta _{k}\delta _{k}^{T}}{\delta _{k}^{T}y_{k}}-\frac{G_{k}y_{k}y_{k}^{T}G_{k}}{y_{k}^{T}G_{k}y_{k}}$

    * 算法

      IN: $l(\theta)$，$g(\theta)$，$\varepsilon$

      Out：$\theta^{*}$

    * 初始化 $\theta^{(0)}$和$G_{0}$

      Loop{

      ​	for k =0 to more{

      ​		$g_{k}=g(\theta^{(k)})$	

      ​		if 	$\left \| g_{k} \right \|<\varepsilon​$	out:		 $\theta^{*}=\theta^{(k)}​$	

      ​		else 	$p_{k}=-G_{k}^{-1}g_{k}$	

      ​				find $\lambda_{k}=minarg(l(\theta^{(k)}+\lambda p_{k}))$

      ​				$\theta^{(k+1)}=\theta^{(k)}+\lambda_{k} p_{k}$

      ​				$g_{k+1}=g(\theta^{k+1})$

      ​				if 	$\left \| g_{k+1} \right \|<\varepsilon$	out:		 $\theta^{*}=\theta^{(k+1)}$	

      ​				else		 $G_{k+1}=G_{k}+\frac{\delta _{k}\delta _{k}^{T}}{\delta _{k}^{T}y_{k}}-\frac{G_{k}y_{k}y_{k}^{T}G_{k}}{y_{k}^{T}G_{k}y_{k}}$

      ​	}

    * }

  * BFGS 算法，

    ​	这个算法 和DPF的思想差不多，不过是要满足

    ​			$B_{k+1}\delta _{k}=y_{k}$

    ​			$B_{k+1}=B_{k}+P_{k}+Q_{k}$

    ​		只要 令$P_{k}\delta_{k}=y _{k}$

       			 $B_{k}\delta_{k}=-Q_{k}\delta_{k}$

    ​		所以  $B_{k+1}=B_{k}+\frac{y _{k}y _{k}^{T}}{y _{k}^{T}\delta_{k}}-\frac{B_{k}\delta_{k}\delta_{k}^{T}B_{k}}{\delta_{k}^{T}B_{k}\delta_{k}}$

    * 算法

      IN: $l(\theta)$，$g(\theta)$，$\varepsilon$

      Out：$\theta^{*}$

    * 初始化 $\theta^{(0)}$和$_B{0}$

      Loop{

      ​	for k =0 to more{

      ​		$g_{k}=g(\theta^{(k)})$	

      ​		if 	$\left \| g_{k} \right \|<\varepsilon$	out:		 $\theta^{*}=\theta^{(k)}$	

      ​		else 	$B_{k}p_{k}=-g_{k}​$	 get $p_{k}​$

      ​				find $\lambda_{k}=minarg(l(\theta^{(k)}+\lambda p_{k}))$

      ​				$\theta^{(k+1)}=\theta^{(k)}+\lambda_{k} p_{k}$

      ​				$g_{k+1}=g(\theta^{k+1})$

      ​				if 	$\left \| g_{k+1} \right \|<\varepsilon$	out:		 $\theta^{*}=\theta^{(k+1)}$	

      ​				else		 $B_{k+1}=B_{k}+\frac{y _{k}y _{k}^{T}}{y _{k}^{T}\delta_{k}}-\frac{B_{k}\delta_{k}\delta_{k}^{T}B_{k}}{\delta_{k}^{T}B_{k}\delta_{k}}$

      ​	}

    * }

  * L-BFGS 算法

    ​	虽然使用了BFGS算法之后不用计算H了 但是$\delta _{k}$和$y_{k}$占的空间很大，会让程序很慢，所以选取$\delta _{k}$和$y_{k}$前面m个维度来计算$B_{k+1}$就够了
