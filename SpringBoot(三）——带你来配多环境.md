# 带你来配多环境
> 多环境就是你有可能会对这个项目的一些东西做一些更改，那做更改如果每次都重写全部配置的话显然不符合SpringBoot的核心思想，因此在SpringBoot中有下边几种配置多环境的方法。
## properties文件中配置多环境
> 在properties中配置多环境有下边几个重要的地方

1. 格式（必须是application-xx.properties）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419183448379.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
2. 在application.properties中选择指定的环境
application-dev.properties
```
#开发环境的properties
server.port=8889
```
application-test.properties
```
#测试环境的properties
server.port=8810
```
application.properties
```
server.port=8080//默认端口
spring.profiles.active=dev #激活生产环境配置
```
启动项目
```
  INFO 13772 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8889 (http) with context path ''
```
> 多环境的配置是很经常用的，在后边配置多个同类型的数据库等中都有遇到
## yml/yaml中配置多环境
> 在properties配置多环境和yml中有一些不一样，可以说yml更简单

yml配置文件
```
server:
  port: 8080
spring:
  profiles:
    active: dev
---
server:
  port: 8858
spring:
  profiles: test
---
server:
     port: 8998
spring:
  profiles: dev
```
在yml中可以用’---‘分割工作区，在profiles中配置名称。

使用方式：
1. 如上边代码，可以在第一个工作区中指定激活那个profile
2. 在cmd 中，运行jar时，后边加上java -jar --spring.profiles.active=xxx 
3. 在idea启动加载jar的配置中加--spring.profiles.active=xxx，和cmd达成jar运行效果是一样的。
4. 虚拟机参数-Dspring.profiles.active=xxx 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419201159624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
虚拟机参数是VM option，下边那个框是程序参数，下图是运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419200557232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
> 并且在使用的过程中发现（如上图）cmd运行的命令优先于在yml文件中指定激活的profile。

值得注意的问题：
-  SpringBoot扫描properties/yml/yaml文件的优先级排序
> 项目路径下的config包下的>项目路径下的>classpath:/config下的>classpath:下的
- yml的优先级小于properties
> properties的优先级高于yml
- 在打包后，还可以在发布命令java -jar xxx.jar --spring.config.location xx 来改变默认加载的配置文件位置

但是它们都秉承一个原则：就是相同配置按照优先级，不同配置取并集。

