# Jhome框架总结项目开发中的技术要点：



### 即将升级计划
*  分段上传
*  分页组件
*  Netty 整合 RabbitMQ 形式实现

### jhome v1.1 框架升级内容  
该版本重点引入了Netty服务，构建分布式系统中WebSocket通信，服务之间采用NIO 通信通过时间rpc协议间接操作WebSocket,实现高性能的前后端通信；
*  引入爬虫框架（后续引入实战Demo）
*  引入grpc框架
*  引入Thrift框架
*  引入Netty框架
*  引入SpringCloud框架 
*  引入公共组件库包括（传统阻塞式 RabbitMq模式 、PIO Exl批量导入引擎、JWT单点登录Tokens生成库、分布式缓存锁、HttpClient、Memcached、其他）
*  引入Tcc 分布式事务框架 两阶段补偿提交（后续引入实战demo）
 
### jhome v1.0 框架升级内容 
该版本重点引入了Shrio框架，针对在分布式系统中，各个服务之间用户授、认证做了集中处理:

*  登陆成功跳转问题 登陆成功后跳转到原有地址:  (已修复，后期根据当前Session 中存储的返回地址 跳转)
*  支持单点登陆（分部署集中认证和授权解决分布式系统中的用户登陆和授权的问题，但是在微服务系统中，无法实现一次登陆多次使用）;
    （已修复）
    a、用户在B段登陆 后使用JWP 对 SessionID jwt 生成Token :
    b、将带有的Token地址跳转到A 消费段:
    c、A段过滤器拦截 解析Token 调用Subject.getSession 方法 验证Token合法性，并在返回的Session中设置认证标识，
        session.setAttribute("AUTHENTICATED_SESSION_KEY",boolean.class)。
    d、本次返回 true。
    e、等待下次过滤连执行 重新创建subject 根据 AUTHENTICATED_SESSION_KEY 存在 返回isAuthenticated为true)。

*  解决频繁从账户系统活用Session信息，只有获取授权每次从账户系统中获取;
     （已修复 通过远程接口从缓存中读取数据后写入到请求头中， 框架不需要要每次通过远程接口请求获取Session，每次请求获取一次）

*  重定向 B系统在A系统登录成功，在去请求A系统登录页面，直接跳回到B系统;
   （已修复 待验证）
*  单点登陆成功后，在提供服务B端退出，消费A段无法退出
   （正在修复，消费A段退出，先调用A段的退出把Subject 认证状态isAuthenticated
    变成false,在调用服务B段退出设置B段的Subject的isAuthenticated，刷新当前页即可 服务B段退出，A段做做感知判断）
     
### 技术选型

*  1.Spring全家桶 （SpringBoot Spring SpringCloud ）
*  2.Shrio 安全框架
*  3.NIO（Netty）
*  4.PRC远程通信协议（ grpc  Thrift ） 
*  5.分布式两阶段补偿事务（transaction）
*  6.POI(文档操作)
*  7.redisson（操作Redis缓存数据库框架）
*  8.rabbitmq（消息队列）
*  9.mybatis

### 本人微信号： daxu06661



