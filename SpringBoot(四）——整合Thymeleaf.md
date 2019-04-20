# SpringBoot整合ThymeLeaf
> 这篇文章就来带大家使用SpringBoot整合Thymeleaf模板引擎
## Thymeleaf的介绍
> Thymeleaf是一个用于web或独立环境的一个模板引擎，也就类似于我们前边学的过的JSP。


### 什么是Thymeleaf
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190420150732276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
### Thymeleaf能干什么
> 它的作用就是将普通的前端页面和数据结合起来，这些页面可以通过这个模板引擎来拿到服务端的数据。简化开发过程，语法简单，功能强大。就如下图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042015104953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
这里就简单的说一下，如果需要仔细了解的话建议去查看官方文档，或者在下方评论，我会发给你官方文档的PDF版。
## Thymeleaf的快速入门
> 先来简单的用一用，然后再看它的语法规则

1. 引入依赖,只需要在基础包上引入下边的包
```

    <properties>
        <java.version>1.8</java.version>
        <thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
        <!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
        <!-- thymeleaf2   layout1-->
        <thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
    </properties>
    
       <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
    </dependencies>


```
2. 编写controller
```
 @RequestMapping(value = "/hello")
    public String hello(Model model){
        model.addAttribute("name","wrial");
        return "/success";
    }
```
3. 编写简单的测试页面 
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1 th:text="${name}">你好！</h1>
<h1>123</h1>
</body>
</html>
```
4. 运行测试 localhost:8081/test/hello
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190420160459982.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyNjA1OTY4,size_16,color_FFFFFF,t_70)
> 和前边的写的HTML进行对比，你发现了什么？

查看当前页面的源代码
```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1>wrial</h1>
<h1>123</h1>
</body>
</html
```
> 没错，就是在第一个h1上写的'你好！'没了，这是因为Thymeleaf是可以对文html进行修改和覆盖的！接下来就来看看Thymeleaf的常用语法。
## 常用语法
> 这里列举一些比较常用的语法，详细可从[官方文档中查看](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#standard-expression-syntax)
### ${..}:用来获取变量的值
```
 1）、获取对象的属性、调用方法,就比如说在controller中的Model里的值
 2）、使用内置的基本对象：上下文，上下文变量，request，response，session，servlet等等
    			#ctx : the context object.
    			#vars: the context variables.
                #locale : the context locale.
                #request : (only in Web Contexts) the HttpServletRequest object.
                #response : (only in Web Contexts) the HttpServletResponse object.
                #session : (only in Web Contexts) the HttpSession object.
                #servletContext : (only in Web Contexts) the ServletContext object
                ${session.foo}
  3）、内置的一些工具对象：比如Java中的集合，列表基本的类型等等
                #execInfo : information about the template being processed.
                #messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
                #uris : methods for escaping parts of URLs/URIs
                #conversions : methods for executing the configured conversion service (if any).
                #dates : methods for java.util.Date objects: formatting, component extraction, etc.
                #calendars : analogous to #dates , but for java.util.Calendar objects.
                #numbers : methods for formatting numeric objects.
                #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
                #objects : methods for objects in general.
                #bools : methods for boolean evaluation.
                #arrays : methods for arrays.
                #lists : methods for lists.
                #sets : methods for sets.
                #maps : methods for maps.
                #aggregates : methods for creating aggregates on arrays or collections.
                #ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

```
### *{...}:选择表达式
> 比如是一个列表，或对象等由多个属性构成的对象，就可以使用*{}取出里边的小项
```
    #从session取出user（使用${}）,使用*{}取出里边包含的数据（也可以选择取）
 <div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
 </div>
```
### #{}：消息表达式
```
    Message Expressions: #{...}：获取国际化内容
```
一般用于静态资源访问或用作国际化
### @{}：超链接表达式
```
<!-- Will produce 'http://localhost:8080/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html" 
   th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/3/details' (plus rewriting) -->
<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```
超链接写到@{}的大括号里，参数可以在加小括号中加，不仅可以直接写，也可以从对象中获取。多个参数如下使用逗号隔离开
```
	@{/order/process(execId=${execId},execType='FAST')}
```
### ~{}:片段引用表达式
在括号里面可以引用代码段。
### 普通属性
```
Literals（字面量）
      Text literals: 'one text' , 'Another one!' ,…(文本，也就是字符串)
      Number literals: 0 , 34 , 3.0 , 12.3 ,…（数字）
      Boolean literals: true , false（布尔类型）
      Null literal: null（空）
      Literal tokens: one , sometext , main ,…
```
### 文本
```
文本使用单引号括起来，也可以通过'+'实现文本拼接
```
### 运算
*  数学运算：也可以进行数学运算 + , - , * , / , %等等
* 布尔运算：and，or，！，not
*  比较：> , < , >= , <=，== , != 
*  三元运算： 
    If-then: (if) ? (then)
  
    If-then-else: (if) ? (then) : (else)
  
    Default: (value) ?: (defaultvalue)
 
### th:xxx
th:xx中xx可以是html标签的任何属性，这些属性都可以被Thymeleaf所替换，因此th属性的用法和html一致，因此在这就不多说明了，但是它是Thymeleaf的核心。

在网上找到一个对th常用的属性进行介绍的博客[点击进入](https://www.cnblogs.com/hjwublog/p/5051732.html#_label0)

这就是Thymeleaf，详细课参考[官方文档](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#literals)