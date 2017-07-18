#### WEB SSO

* ##### 什么是SSO？

  SSO英文全称Single Sign On，[单点登录](http://baike.baidu.com/item/%E5%8D%95%E7%82%B9%E7%99%BB%E5%BD%95)。SSO是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。它包括可以将这次主要的登录映射到其他应用中用于同一个用户的登录的机制。它是目前比较流行的企业业务整合的解决方案之一。

  那么 什么是单点登录呢？就是例如你可以用你的qq上面登陆QQ游戏QQ邮箱一样 不需要重新登录，

* ##### WEB登录的本质是什么？

  WEB的登录使用的是HTTP这个无状态的协议。一定需要增加状态 不然就乱套了，这个时候 Session 会话产生了 Session是保存在服务器端的，看下图

  ## ![207.18.01](/Users/mac/Desktop/207.18.01.png)

  ![07.18.02](/Users/mac/Desktop/07.18.02.png)

* ##### 拓展到多个系统SSO

  * 难点：系统的架构，语言可能不同，域名也不同

    * 方案一 共享cookie

      Cookie 不能跨域

      ​	—abc.com 产生的cookie不能被xyz.com访问

      解决：使用域名映射

      ​	abc.com——>abc.sso.com

      ​	xyz.com——>xyz.sso.com

      Session 需要共享到每一个服务器（非常麻烦）

      如果使用不同的语言构成，后台共享session 很难

    * 方案二 共享cookie 不共享session（使用签名）

      ![07.18.03](/Users/mac/Desktop/07.18.03.png)

      ![07.18.04](/Users/mac/Desktop/07.18.04.png)

      优点：登陆状态存放在客户端 服务器没有管理session的负担

      缺点：各个系统所用的签名必须相同；

      ​		密钥也必须相同

      ​		密钥还要定期的刷新 （维护成本太高）

    * 方案三：使用独立的认证中心

      ![07.18.05](/Users/mac/Desktop/07.18.05.png)

      这里看出来 是认证中心提供的登陆form ，登陆成功创建了全局会话；返回一个令牌（ticket）![07.18.09](/Users/mac/Desktop/07.18.09.png)

      在A服务器创建了局部会话下次访问的时候就不用去认证中心认证了；

      ![07.18.06](/Users/mac/Desktop/07.18.07.png)

      ​

      ![07.18.08](/Users/mac/Desktop/07.18.08.png)

* ##### CAS（Central Authentication Service）

  * 耶鲁大学发起的开源SSO
  * 几个核心概念
    * CAS Server／CAS Client
    * Service
    * TGT：Ticket Granting Ticket
    * ST：Service
  * 安全性问题
    * 非常依赖SSL，通信都应该使用https来进行
    * Ticket安全性
      * 只能使用一次
      * 过一段时间就会失效
      * 需要足够的随机性

* 使用另一种方式的CAS

  * 1.用户访问www.a.com/pageA

  * 2.系统A发现用户没有登录，重定向到认证中心

  * 3.认证中心验证用户提供的用户名密码，登陆成功

  * 4.认证中心形成一个XML文件，对消息做签名，然后发送给A

  * 5.系统A对XML消息做验证，发现合法，认为登陆过了，就可以访问了

    ![7.18.10](/Users/mac/Desktop/7.18.10.png)

    ![7.18.11](/Users/mac/Desktop/7.18.11.png)

    ​

  ​