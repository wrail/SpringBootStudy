# SpringBoot再靠近一点

> 在SpringBoot（一）中我们对SpringBoot有了一些了解，这次我们再靠近它一下。

在此之前，如果不知道yml或properties配置文件的写法，请先对yml或properties有一些了解.[比较不错的入门学习地址](https://www.jianshu.com/p/97222440cd08)，当然你也可以去参照官网。

- [x] 默认已经对yml和properties有了一定的了解（一点点了解就够了）下边我就直接演示了。

## 配置文件改怎么弄

> 在Spring中，要配置一个Bean，有两种方法，一种是xml，另一种就是注解，对属性的注入，也是可以使用xml和注解两种方式，Spring5.x大力支持在类级别上配置一些东西。接下来我们就看看怎么在SpringBoot中配置一些东西。

### 在yml中配置各种类型
新建实体类
User
```
package com.wrial.boot.bean;


import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;
@Data
@Component
@ConfigurationProperties(prefix = "user")//从配置文件中找前缀为person的配置信息
public class User {

    private String username;
    private Integer age;
    private Date birth;
    private Boolean man;
    private List<Object> list;
    private Map<String,Object> map;
    private MoreInfo moreInfo;

}

```
MoreInfo

```
package com.wrial.boot.bean;

import lombok.Data;

@Data
public class MoreInfo {
    private String address;

}


```
yml
```
user:
  username: wrial
  age: 20
  birth: 1999/11/27
  man: true
  list:
    - 1
    - 2
    - 3
  moreInfo:
    address: 太空
  map: {k: v}

```

test类

```
package com.wrial.boot;

import com.wrial.boot.bean.User;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

//SpringBoot中的单元测试，可以分方便的注入属性
@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
public class BootApplicationTests {

    @Autowired
    private User user;

    @Test
    public void testUser() {
        log.info("UserInfo->{}", user.toString());

    }

}


```

info结果

```
INFO 18404 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=Administrator, age=20, birth=Sat Nov 27 00:00:00 CST 1999, man=true, list=[1, 2, 3], map={k=v}, moreInfo=MoreInfo(address=太空))
```
可以发现属性注入是成功的。

> 一个增加配置注释的依赖

```
  <!--配置文件处理器，在配置文件的时候更方便会有提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
        </dependency>
```

### 在properties中配置各种类型

可以看到我们在增加了上边的依赖就会有很多的提示信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418210823603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

在properties中配置比yml中简单一些，我在下边随便配置一点东西

```
user.map.k1=1
user.list={123,456}
user.map.k2=2
user.username=lili

```
我没有删除我的yml，你能猜到最后会输出什么结果？

```
INFO 19232 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=lili, age=20, birth=Sat Nov 27 00:00:00 CST 1999, man=true, list=[{123, 456}], map={k1=1, k2=2, k=v}, moreInfo=MoreInfo(address=太空))
```
可以看到properties如果和yml有重合部分，那就使用properties的部分，如果不重合就合并了。

如果在properties中有中文比如下边例子
```
user.map.k1=1
user.list={123,456}
user.map.k2=2
user.username=张飞

```
然后看看日志，可以发现中文是乱码的，那为什么我们在yml中可以使用中文呢
```
INFO 19200 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=ï¿½ï¿½ï¿½ï¿½, age=20, birth=Sat Nov 27 00:00:00 CST 1999, man=true, list=[{123, 456}], map={k1=1, k2=2, k=v}, moreInfo=MoreInfo(address=太空))
```
像这种问题都是因为编码和解码的不一致导致的，Idea默认的字符集编码是UTF-8，而properties是ASCII码。因此需要调节编码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418213249529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

然后返回properties中，把乱码的数据改回汉字，然后再运行。
```
2019-04-18 21:34:03.809  INFO 8380 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=张飞, age=20, birth=Sat Nov 27 00:00:00 CST 1999, man=true, list=[{123, 456}], map={k1=1, k2=2, k=v}, moreInfo=MoreInfo(address=太空))
```

### Spring中的配置方式

```
package com.wrial.boot.bean;


import lombok.Data;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;
@Data
@Component
public class User {

    @Value("${user.username}")//S属性注入
    private String username;
    @Value("#{10*2}")//SPEL
    private Integer age;
    private Date birth;
    @Value("false")//直接注入
    private Boolean man;
    private List<Object> list;
    private Map<String,Object> map;
    private MoreInfo moreInfo;

}
```
结果
```
INFO 12228 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=张飞, age=20, birth=null, man=false, list=null, map=null, moreInfo=null)
```
### Spring传统配置和SpringBoot配置的区别
> 总的来说，SpringBoot比Spring的属性注入更强大，SpringBoot的是基于Spring并对Spring进行了一些改进和升级。没有重不重要之分，SpringFreamWork的地位还是很高的。

从网上找的一张图，感觉总结的很好

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418221502442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)

松散绑定就比如说一些内置的驼峰转换（在ConfigurationProperties中带有而@Value就没有）

@Value的好处就是可以使用SPEL

使用JSR303校验在类上加@Valid，[JSR303介绍](https://www.jianshu.com/p/554533f88370)

@Value不支持一些复杂类型，比如map集合等

### Spring5推荐的配置方式

spring5推荐用Java代码配置bean。在bean目录下建立一个person类，并且在boot目录下建一个config，并添加SpringBootConfiguration.java作为配置类。

Perosn.java
```
package com.wrial.boot.bean;

import lombok.Data;
import org.springframework.context.annotation.Bean;

@Data
public class Person {

    private String name;
    private Boolean man;

}
```
SpringBootConfiguration.java
```
package com.wrial.boot.config;

import com.wrial.boot.bean.Person;
import com.wrial.boot.service.TestSpringXmlService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * Configuration注解也就是来替换Spring中的添加组件（以前是xml）
 * */
@Configuration
@Slf4j
public class SpringBootConfiguration {

    //这里的方法名就是Person的id
    @Bean
    public Person person(){
        Person p = new Person();
        p.setName("wrial");
        p.setMan(true);
        log.info("PersonSpringBootConfiguration被加载{}，{}",p.getName(),p.getMan());
        return p;
    }

}
```
测试类
```
package com.wrial.boot;

import com.wrial.boot.bean.Person;
import com.wrial.boot.bean.User;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
public class BootApplicationTests {

    @Autowired
    ApplicationContext application;

    @Test
    public void testSpringXml() {
        //对应配置类的方法名
        Person person = (Person) application.getBean("person");
        log.info("Person-->{}", person.toString());
    }

}

```
输出信息
```
INFO 8368 --- [           main] c.w.boot.config.SpringBootConfiguration  : PersonSpringBootConfiguration被加载wrial，true
INFO 8368 --- [           main] com.wrial.boot.BootApplicationTests      : Person-->Person(name=wrial, man=true)
```
> 如果有老的项目迁为springboot项目，那岂不是还得重写敲代码，在springboot也对spring的传统配置方式进行了支持，可以把我们以前的xml文件通过@ImportResource（在启动类上）来使用xml方式。

### 对传统Spring的兼容
在resources下我们新建一个springtest.xml文件，我们在里边配置一个类到我们的Ioc容器中去。

为了保持项目的基本结构，那就在boot下新建一个service包，然后新建一个TestSpringXmlService.java
```
package com.wrial.boot.service;

import org.springframework.stereotype.Service;

@Service
public class TestSpringXmlService {
}
```
SpringXmltest.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--将目标类加入到Spring容器-->
<bean id="service" class="com.wrial.boot.service.TestSpringXmlService"></bean>

</beans>
```
```
package com.wrial.boot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

//在原来的的启动类加ImportResource
@ImportResource(value = {"classpath:springtest.xml"})
@SpringBootApplication
public class BootApplication {

    public static void main(String[] args) {
        SpringApplication.run(BootApplication.class, args);
    }

}

```
测试类
```
package com.wrial.boot;

import com.wrial.boot.bean.Person;
import com.wrial.boot.bean.User;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
@Slf4j
public class BootApplicationTests {

    @Autowired
    private User user;

    @Autowired
    ApplicationContext application;

    @Test
    public void testUser() {
        log.info("UserInfo->{}", user.toString());
    }

    @Test
    public void testSpringXml() {
        //判断在xml中有没有service中的bean
        boolean b = application.containsBean("service");
        log.info("SpringXml--->{}", b);
        
    }
}

```

```
INFO 18960 --- [           main] com.wrial.boot.BootApplicationTests      : SpringXml--->true

```
### 在properties中的骚操作
> 在Spring中有没有感觉${}无处不再，当然，在配置文件中也可以用，接下来我们看看吧
使用${}在配置文件中生成随机数，配置项的拼接,如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419180324333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
简单的使用：
```
user.map.k1=${random.int}
user.list={123,456}
user.map.k2=2
user.username=张飞${user.map.k1}
```

结果（有些信息是在其他文件中配置的不用管）
```
2019-04-19 18:05:04.388  INFO 9160 --- [           main] com.wrial.boot.BootApplicationTests      : UserInfo->User(username=张飞1002747015, age=null, birth=null, man=null, list=[{123, 456}], map={k1=-454305280, k2=2}, moreInfo=null)
```

<hr>

SpringBoot的配置方式掌握了吗？

[博客链接](https://blog.csdn.net/qq_42605968)：可能有写不对的地方（应该还挺多的，欢迎指出，这也是一个检测有没有真正掌握知识的方法）。

代码已上传至[GitHub](https://github.com/wrail)
