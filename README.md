Jhome框架是我在项目工作中积累的技术框架主要用的技术是：
------------------------------------------------------------------------------
    Spring全家桶 （SpringBoot Spring SpringCloud ）
    Shrio 安全框架
    PRC远程通信协议（ grpc  Thrift ） 
    分布式两阶段补偿事务（transaction）
    NIO（Netty）
    POI(文档操作)
    redisson（Redis框架）
    rabbitmq（消息队列）
    mybatis
    。。。。 
    
分布式集中认证、授权；单点登录 升级 
------------------------------------------------------------------------------
1,登陆成功跳转问题 登陆成功后跳转到原有地址？
  (已修复，后期根据当前Session 中存储的返回地址 跳转)
  
2,支持单点登陆（分部署集中认证和授权解决分布式系统中的用户登陆和授权的问题，但是在微服务系统中，无法实现一次登陆多次使用）？
（已修复 
  a 用户在B段登陆 后使用JWP 对 SessionID jwt 生成Token 
  B 将带有的Token地址跳转到A 消费段
  c  A段过滤器拦截 解析Token 调用Subject.getSession 方法 验证Token合法性，并在返回的Session中设置认证标识
   session.setAttribute("AUTHENTICATED_SESSION_KEY",boolean.class);
  d 本次返回 true
  e 等待下次过滤连执行 重新创建subject 根据 AUTHENTICATED_SESSION_KEY 存在 返回isAuthenticated为true)
  
3,解决频繁从账户系统活用Session信息，只有获取授权每次从账户系统中获取
  （已修复 通过远程接口从缓存中读取数据后写入到请求头中， 框架不需要要每次通过远程接口请求获取Session，每次请求获取一次）

4,重定向 B系统在A系统登录成功，在去请求A系统登录页面，直接跳回到B系统
（已修复 待验证）

5，分布式权限控制
（demo）
6，单点登陆成功后，在提供服务B端退出，消费A段无法退出
（正在修复，消费A段退出，先调用A段的退出把Subject 认证状态isAuthenticated
 变成false,在调用服务B段退出设置B段的Subject的isAuthenticated，刷新当前页即可 服务B段退出，A段做做感知判断）
  
解决思路： 
   1，系统A登陆；   
   2，A系统拦截器进行拦击，通过Shiro判断当前Session是否存在是否登陆，如果没有登陆执行拦截器中的拒绝方法；   3，获取当前系统的请求地址，通过调用远程接口会话创建当前Session（存储当期系统的返回路径信和其他信息）。
   4，远程端分配该SessionID 给系统B, 返回系统A分配SessionID给A系统
   5，系统A跳转到系统B进行登陆。
   6，系统B登陆
   7，对系统B进行拒绝访问拦截（判断当前请求是否登陆提交）执行登陆executeLogin 如果已经登陆属于重定向的请求，直接返回原有地址；否则重新登陆
   
 
Rabbit NIO 升级计划 
------------------------------------------------------------------------------  
待续。。。。。。。。。。。。。。。
