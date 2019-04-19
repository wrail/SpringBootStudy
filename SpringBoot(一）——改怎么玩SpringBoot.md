# 该怎么玩SpringBoot

> 在此之前把SpringBoot也了解一些，深深浅浅都有，写东西的话是足够的，但是总感觉对SpringBoot的了解不够，因此就感觉很难受，终于下定决心，重新开始SpringBoot。

## SpringBoot入门程序Hello Spring Boot

我使用的工具是IDEA，当然Eclipse也不错，下边就开始第一个SpringBoot程序。

这个是我以前写过的很详细的第一个SpringBoot程序的教程：[很详细的图文SpringBoot的几种创建方式和解释](https://blog.csdn.net/qq_42605968/article/details/87935200)

创建好的SpringBoot项目就是这个样子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418173855353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

文件目录：

- boot下是Java代码区
- resource下static可以放前端代码（静态资源），templates下是模板页面，比如freemaker等，然后application.properties/yml是配置文件

[这些SpringBoot的最基础讲解都在我的博客专栏中有](https://blog.csdn.net/qq_42605968/column/info/34349)

在我们这个新的SpringBoot下我在Maven中引入了几个简单的依赖来测试我们的第一个SpringBoot

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--SpringBoot启动类的父工程（在其内部有各种各样的依赖）,可以看为一个依赖的集合,里边把我们能用到的依赖基本全包括进去了-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <!--项目的组织，分组，版本还有项目的名字，还有项目的描述-->
    <groupId>com.wrial</groupId>
    <artifactId>boot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>boot</name>
    <description>Demo project for Spring Boot</description>
    <!--JDK版本1.8-->
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <!--springboot的web启动器-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--一个test包-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <!--maven插件-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```
然后新建一个controller来进行测试

```
package com.wrial.boot.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
//等于Controller+ResponseBody：以JSON格式输出
@RestController
//Restful风格的父映射路径配置
@RequestMapping("/test")
public class HelloController {
     
    //请求地址
    @RequestMapping("/sayHello")
    public String sayHello() {

        return "Hello My First SpringBoot project！";

    }

}


```

出现这样一个就说明启动成功了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418175452178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

在这可以看到，SpringBoot的路径，启动方式，还有classloader的加载所需的时间，默认的端口是8080等等，这个会在后面的配置增加而增加

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418175638867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

然后访问http://localhost:8080/test/sayHello就可以看到页面输出

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418180117168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

这样一个简单的SpringBoot就运行起来了。

如果我们想要在本地运行一个SpringBoot工程，或者是在远程运行一个SpringBoot工程，那我们必须对项目进行打包，打包方式分为两种，一种是命令行打包，一种是IDEA的maven插件打包。值得一提的是在SpringBoot中只需要打成jar就可以了，在我们以前的SSM或SSH，或者是原生的都需要打为war才能在浏览器运行。

打完的Jar如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418181025539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

我们跟踪到目录下，使用命令行运行jar包

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418181238695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

使用java -jar 加上文件，将我们的SpringBoot启动起来

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418181617375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

然后还是那个url访问呢得到下面页面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418181741428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

并且SpringBoot部署的好处就是内嵌Tomcat，也就是说不用再装Tomcat了！

## 聊一聊SpringBoot的简单Demo

> 相比于各种框架的整合，SpringBoot可以让我们的开发变得更加迅捷，SpringBoot的实现可以说是JavaWeb开发的一个转折点。没有Spring基础的可以选择看，不影响使用SpringBoot。

SpringBoot的启动器，也就是我们上边的主方法。SpringApplication.run()就可以运行我们的web程序。这是为什么呢？

在我们学习注解的时候，我们可以使用注解赋值，可以使用注解来标识，因此我们也可以发现这个程序的启动离不开SpringBootApplication这个注解。接下来就随便聊一聊这个注解。

SpringBootApplication注解

```
//学了注解都知道，Target是注解对象修饰的范围
@Target(ElementType.TYPE)
//Retention定义了被保留的时间长短
@Retention(RetentionPolicy.RUNTIME)
//这个注解可以生成API（JavaDOc）当然功能不止这一个
@Documented
//标记注解
@Inherited
//SpringBoot配置类
@SpringBootConfiguration
//开启自动配置，一般和SpringBootConfiguration成对使用
@EnableAutoConfiguration
//和Spring一样，容器扫描
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM,
				classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

	...
}

```

如果你注解知识不了解可以参考：[link](https://www.cnblogs.com/gmq-sh/p/4798194.html)

重点来介绍下边几个注解

- SpringBootConfiguration

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {

}


```
可以看到在它里卖弄都是一些属性的注解，核心还是我们在Spring中见到过的Configuration注解。并且可以看下边代码还是使用的Component组件进行的配置。

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {

	@AliasFor(annotation = Component.class)
	String value() default "";

}


```

- EnableAutoConfiguration
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
...
}

```
可以发现下边有一个AutoConfiguration，还有一个Import（这个的实现就是Spring里的了，目的就是导入容器）。

AutoConfiguration在它下边的AutoConfigurationPackage中可以自动导包，但是必须要在SpringBootApplication注解下的包或类中。如果想看类是怎么加载的，可以进入AutoConfigruation下的Registrar中的register方法方法进行调试，可很明显的观察到具体情况。
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import(AutoConfigurationPackages.Registrar.class)
public @interface AutoConfigurationPackage {

}

```
再来看看这个Import里的内容AutoConfigurationImportSelector（自动配置导入选择器）

先对选择引入的这个方法进行分析，其余的暂时先不看

```
public String[] selectImports(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return NO_IMPORTS;
		}
		//加载元数据
		AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
				.loadMetadata(this.beanClassLoader);
		//获取自动配置类
		AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(
				autoConfigurationMetadata, annotationMetadata);
		return 
		
		//将自动配置需要的组件以String类型返回
		StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
	}


```
得到组件的路径名，就可以导入组件。

我们如果进入我们的自动配置类，可以看到很多类似于图片这样的自动配置选项，这都是经过在自动配置中使用Properties中对这些进行了加载。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418194354493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

> 上边这些都是表层的源码，以后会对Spring和SpringBoot源码的详细分析！

该怎么玩SpringBoot这由你说了算！

[博客链接](https://blog.csdn.net/qq_42605968)：可能有写不对的地方（应该还挺多的，欢迎指出，这也是一个检测有没有真正掌握知识的方法）。

代码已上传至[GitHub](https://github.com/wrail)


