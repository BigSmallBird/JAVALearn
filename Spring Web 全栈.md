# Spring Web 全栈

![image-20250826110111011](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250826110111011.png)

![image-20250826110150985](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250826110150985.png)

## Maven 入门：

### Apache Maven 是做什么的？：

Maven 是一个 **项目管理和构建自动化工具** 。但实际对于我们来说，我们最关心的是它的项目构建功能。Spring 是必须学的框架，那么 Maven 是 Java 必须掌握的工具。

> Maven 提供了一个命令行工具，可以把工程打包成 Java 支持的格式。（比如 jar），并且支持部署到中央仓库里面，这样使用者只需要通过工具就可以快捷的运用他人写的代码，只需要你添加依赖

### Maven 系统架构：

![img](https://style.xxx.com/img/ham/course/j4/mvn.svg)

从这个架构里面可以看到借助于中央仓库，我们可以把 Java 代码任意的共享给别人。
Maven 使用惯例优先于配置规则，他要求在没有制定之前，所有的项目都有如下结构

| 目录                            | 目的                    |
|:-----------------------------:|:---------------------:|
| $}basedir}                    | 存放 pom.xml 和所有的子目录    |
| $(basedir)/pom.xml            | Maven 的项目配置文件         |
| $(basedir)/src/main/java      | 项目的 java 源代码          |
| $(basedir)/src/test/resources | 项目的资源，比如说 property 文件 |
| $(basedir)/src/test/java      | 项目的测试类，比如 JUnit 代码    |
| $(basedir)/src/test/resources | 测试使用的资源               |

这里的 ${basedir} 代表的是 Java 工程的跟路径，在我们这里就是工程的根目录，一个 Maven 项目在默认情况下会产生 JAR (Java 的一种压缩格式文件) 另外，编译后的 classes 会放在 ${basedir}/target/classes 下面。JAR 会放在${basedir}/target 下面

### Maven 命令：

```mvn
mvn clean compile
```

编译命令， Maven 会自动扫描 src/main/java 下面的代码，并且完成编译工作，执行完，会在根目录下生成 target/classes 目录存放 class 文件

```mvn
mvn clean pakege
```

编译并打包命令，这个命令是 compile 和 package 的集合，也就是说会先执行 compile 命令，然后在执行 jar 打包命令，这个的结果会把所有的 java 文件和资源打包成为一个 jar ， jar 是 java 的一个压缩格式，方便我们灵活的运用多个代码

```mvn
mvn clean install
```

执行安装命令，这个命令是 compile 和 package ， install 的集合，也就是，它会先执行 compile 命令，然后执行打包命令， 然后再执行 install 命令安装到本地的 Maven 仓库里，这个目录就是 ${user_home}/.m2

```mvn
mvn compile exec:java -Dexec.mainClass=${main}
```

这个命令的意思是在 compile 执行后， 执行运行 Java的命令，具体执行哪个 Java 类 是由 -Dexec.mainClass= ${main} 参数指定，比如我们想执行 com.xxx.Test 类：

```mvn
mvn compile exec:java -Dexec.mainClass=com.xxx.Test
```

### 总结：

Maven 可以通过插件扩展支持更多的命令，其学习重点是掌握它的配置文件：pom.xml 

## Maven 入门：

关于 Maven 核心概念： 

![image-20250902202221517](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250902202221517.png)

这 五个 概念都运用在 Maven 的配置文件。 Maven 的配置文件是一个强约定的 XML 格式文件，他的文件名一定是 pom.xml。

![image-20250902202758505](C:\Users\15956\AppData\Roaming\Typora\typora-user-images\image-20250902202758505.png)

学习 Maven 我们只需要能够认识 pom.xml 内容就可以，

### 1. POM（Project  Object Model）

一个 java 项目里面所有的配置都放置在 POM 文件里面，大概有一下行为：

* 定义项目的类型，名字
* 管理依赖关系
* 定制插件的

可以看一下下图，![img](https://style.xxx.com/img/ham/course/j4/pomxml.svg)

分别看一下上图的内容

* Maven 坐标
* Maven 工程属性
* Maven 依赖
* Maven 插件

> 其实 HTML 语言也是 XML 格式，不过 XML 格式会严格遵守 **标记语言** 的要求，那就是有开始和结束标签，比如 <version> 1.0 </version>, 我们在定义 pom.xml 时候，，如果没有写完整的开始，那就会出错。

### 1.1 Maven 坐标

```maven
<groupId> com.xxx.course </groupId>
<artifactId> app </artifactId>
<packaging> jar </packaging>
<version> 1.0-SNAPSHOT </version>
```

这四个标签组成了 Maven 的坐标，所谓的坐标就是一种位子信息，Maven 的坐标决定了这个 Maven 工程部署后存在 Maven 仓库的文件位置，所以这个坐标信息不许指定。

#### groupId

groupId 就像一个文件夹一样，他的命名和 java 的包比较一致，这里一般只用小写的引文字母和字符 **.**  比如  com.xxx.course 。 一般来说一个公司会设置自己的 groupId，避免和其他公司重合，个人开发者也一样

#### artifactId

artifackId 在一个 groupId 里面应该是唯一的，不能使用中文或者特殊字符

从规范上来说只能使用小写的英文字母 ,  **.** , **_** 。 比如 app 或者 member.shared

#### packaging

Maven 工程执行完后会把整个工程打包成 packaging 指定的文件格式，默认情况下 packaging 的值是 jar ，所以如果 pom.xml 文件中没有声名这个标签，那就是 jar

packaging 有这几种格式

* jar
* war
* ear
* pom

#### version

version 基本上遵守了软件工程中对版本号的约定。在 Maven 里，会把一个工程分为两个状态

* SNAPAHOT 这个单词翻译过来就是 快照，实际上就是当前的程序还处于不稳定的阶段，随时可以再修改，所以在版本号的后面会加上 SNAPAHOT 关键字
* RELEASE 和 SNAPAHOT 是对立的，它表示稳定，一般正式发布的时候会把 version 改为 RELEASE

当了解了工程状态后，看看版本号的约定：在软件工程里面，我们会用三位数字来表示版本号，大概是 X.X.X 的格式，

**版本号的规则：**

* 第一位代表主版本号
  
  > 主版本号一般是由团队约定的

* 第二位是新增功能
  
  > 比如 iOS 13.1.2 的 1 代表的就是 apple 在13 这个版本里面 有1 次新增功能的行为

* 第三位代表是 bugfix 后的版本
  
  > bugfix 是修护代码缺陷，比如上面的例子，2就是 apple 在13 里修复了两次bug

有时候只有两位版本号，那就是没有主版本号。

软件的版本大多数时候都是 1.0.0 开始，在开发状态下面就是 1.0.0-SNAPSHOT,注意这个格式 [version]-SNAPSHOT

> 最后，就是在执行 mvn package, mvn install 命令生成的 jar 文件名是 [artifactId] - [version].jar 

### 1.2 Maven 属性配置

Maven 的属性配置就是来做参数设置的，如下的配置是我们经常看见的：

```maven
<properties>
    <java.version>1.8</java.version>
    <maven.compiler.source>${java.version}</maven.compiler.source>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>${java.version}</maven.compiler.target>
</properties>
```

他的格式是在 properties 标签内部，这个是固定的格式。properties 内的标签可以自定义，但是一般来说只能是小写

#### java.version

> 代表设置一个参数：java.version 他的值是 1.8

#### maven.compiler.source

> 这个参数是指定 Maven 编译时候源代码的 JDK 版本， ${java.version} 这个值有点特殊，它是一个动态的值，${key} 这个语法会动态找到 key 这个参数配置的值，所以上面的例子中 ${Java.version} 的实际值是 1.8

#### project.build.sourceEncoding

> 这个参数的作用是工程代码源文件的文件编码格式，一般情况下，我们都设置成 UTF-8

#### maven.compiler.target

> 这个参数是按照这个值进行编译源代码，比如这里是 JDK1.8

## Maven 入门：

有了 Maven 坐标，我们就可以通过 Maven 的依赖管理来云南用他人的代码。

### 依赖管理 dependencies

dependency 就是用于指定当前工程依赖其他代码库的。Maven 会自动管理 jar 依赖

> 一旦我们在 pom.xml 里面声名了 dependency 信息，会先去本地用户目录下面的 .m2 文件夹内查找对应的文件， 如果没有找到就会触发从中央厂库下载行为，并且保存到 .m2 文件夹内

我们只需要在 pom.xml 里面添加标签即可。首先声名一个父标签 dependencies，然后就可以作者这个标签内部添加依赖，：

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>
```

一个 pom.xml 只能存在一个 dependencies 标签，但是可以存在多个 dependency 标签。

仔细读读这个 dependency 标签，你会发现，dependency 标签的内容其实就是 Maven 坐标，所以说只要有坐标，我们就可以建立依赖

我们演示一下如何编写这个 dependency 依赖管理，

当然可以在 dependencies 内添加多个依赖，比如说继续添加 okhttp3 依赖：

```xml
<dependenies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
    <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.2.2</version>
    </dependency>
</dependenies>
```

> 一般来说我们把其他人写的代码库叫做 三方库， 自己写的叫做 二方库。

### 中央仓库：

前面我们提到过 Maven 会把所有的 jar 都放在中央仓库里面，我们可以通过 

https://search.maven.org/ 这个网站来搜索 jar 。可惜这个网站部署在外国，我们可以访问阿里云镜像服务器：https://maven.aliyun.com/mvn/search

### 间接依赖：

间接依赖是 mvn 核心要素。如果一个 remote 工程依赖了 okhttp 库，而当前工程 locale  依赖了 remote 工程。这个时候 locale 工程也自动依赖 okhttp 

### 插件体系  plugins:

插件体系让 Maven 变得高度课制定，但要了解插件格式，因为不同的插件有不同的作用。

```xml
<build>
    <plugins>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
    </plugins>
</build>
```

这里声名了一个 maven-compiler-plugin 插件用于执行 maven compile 的，你会发现 maven 的插件其实也是存放在中央仓库的坐标，也就是一切都是 jar 。

## Hello Spring:

搞定前置技术条件，我们来配置，创建一个 Spring。目前我们使用的是 Spring5

Spring5 的 Maven 坐标：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.6-RELEASE</version>
</dependency>
```

Spring 强调的是**面向接口编程**，所以大多数情况Spring代码都会有接口实现类。

Application:

```java
package com.XXX;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import com.xxx.service.MessageService;

@ComponentScan
public class Application {
    public static void main(String[] args){
        Applicationtext context = new AnnotationConfigApplicationContext(Application.class);
        MessageService msgService = context.getBean(MessageService.class);
        System.out.pritln(msgService.getMessage());
    }
}
```

在该代码里面，有以下问题：

* 注解
* Spring Bean
* Spring 扫描
* Spring 声名周期 

### 面对接口的理解：


上面两种方法其实都是调用 Message Service 实现的 getMessage().

在 Spring 当中  ，如果想调用 MessageService 就可以直接在上下文里面获取，而不需要关心它的实现类，如何实例化实现类，从而达到真正的耦合。

> 当代码越多的时候，Spring 的效率就会体现出来，比原生 Java 块很多倍

## Java 注解 （Annotation）：

```java
@Service 
public class MessageServiceImpl implements MessageService{

    public String getMessage() {
        return "Hello World";
    }
}
```

这段代码比平常多了一个注解 @Service

### Annotation（注解）：

本质上来说 Annotation 是 Java 推出的一种注解机制，和普通的注解有个明显的区别， Annotation 是可以在编译运行阶段读取的，注解很明显不可以。

能够在编译，运行阶段读取信息，这就给我们很多的扩展空间，而且不会污染源代码。我们可以借助它来实现一些增强功能，在 Spring 当中就重度使用了 Annotation， 通过运行阶段动态获取 Annotation 从而完成很多自定义的行为。

从另外的角度，Annotation 也是 Java 类，


我们要掌握5小点：

> Annotation 类里面可以继续引用其他的 Annotation 类，所以一个 Annotation 是由多个 Annotation 组合而成

#### Target：

java.lang.annotation.Target 自身也是一个注解，它只有一个数组性质，用于设定改注解的目标范围，比如说可以用于类或者方法等。因为是数组，所以可以同时设定多个范围

具体可以作用的类型配置在 java.lang.annotation.ElementType 枚举类中

* ElementType.TYPE
  
  > 可以作用于类，接口类，枚举类上

* ElementTpye.FIELD\
  
  > 可以作用于类属性上面

* ElementType.METHOD
  
  > 可以作用在方法上

* ElementType.PARAMETER
  
  > 可以作用在参数上

如果想同时作用在类和方法上面：

```java
@Target({ElementType, ElementType.METHOD})
```

> 如果某个 Annotation 的 Target 设定为 METHOD, 那么你就智能在方法上面应用它，其他地方不行

1.1 ElementType.type

```java
@Service
public class MessageServiceImpl implements MessageService {

    public String getMessage() {
        return "Hello World";
    }
}
```

1.2 ElementType.TYPE

```java
@Service
public class MessageServiceImpl implements MessageService {

    public String getMessage() {
        return "Hello World!";
    }
}
```

1.2 ElementType.METHOD

```java
public class MessageServiceImpl implements MessageService {
    @ResponseBody
    public String getMessage() {
        return "Hello World!";
    }
}
```

1.3 ElementType.FIELD

```java
public class MessageServiceImpl implements MessageService{
    @Autowired
    private WorkspaceService workspaceService;
}
```

1.4 ElementsType.PARAMETER

```java
public class MessageServiceImpl implements MessageService{

    public String getMessage(@RequestParam("msg")String msg) {
        return "Hello " + msg;
    }
}
```

#### Retention:

java.lang.annotation.Retention 自身也是一个注释，它用于声名该注解的生命周期，简单来说就是在 java 编译，运行的哪个环节有效，它的值定义在 java.lang.annotation.RetentionPolicy 枚举里面，由三个值：

**SOURCE: **也就是纯注解作用，**CLASS：** 也就是在编译阶段是有效的**RUNTIME: **在运行是有效

```java
@Retention(RetentionPolicy.RUNTIOME)
```

这个代码表示在运行期间有效

> 如果是我们自己定义的Annotation，一般我们都会设置成 RUNTIME

#### Documented：

java.lang.annotation.Document 它的作用是将注解中的元素包含到 JavaDoc文档中，一般都会添加这个注解。

#### @interface：

@interface 就是声名当前的 java 类型是 Annotation

### Annotation 属性：

```java
String value() default "";
```

Annotation 的属性有点像类的属性一痒，它约定了属性的类型，和属性名称，default 代表的是默认值。

有了就可以正式引用：

```java
import org.springframework.stereotype.Service;

@Service
public class Demo {

}
```

注意 Annotation 也是 java 类，所以一样需要 import

上面的 Service可以写成：

```java
import........

@Service(value="Demo")
public class Demo {

}
```

因为我们说过 value 是默认属性名称是可以缩写的，所以上面的代码等同于

```java
import....

@Service("Demo")
public class Demo {

}
```

> Annotation 属性是可以有多个的，

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Document
public @interface RequestParam {

    @AliasFor("name")
    String value() default "";

    @AliasFor("value")
    String name() default "";

    boolean required() default true;

    string defaultValue() default ValueConstants.DEFAULT_NONE;
}
```

@AliasFor("name") 这个是别名的意思，就是说这个属性用这个别名也可以访问到，所以下面的代码是一样的意思

```java
@RequestParam("key")
@RequestParam(value="key")
@RequestParam(name="key")
```

具体代码：

```java
package xxxxxx

import xxxxxxxx

@Service
public class MessageServiceImpl implements MessageService {

    public String getMessage() {
        return "Hello World";
    }
}
```

## Spring Bean：

IoC （控制反转）容器是 Spring 框架核心组件

> IoC 是面向对象编程的设计原则，可以降低计算机代码间的耦合度

在 Spring 框架中，主要通过依赖注入来实现 IoC

在 Spring 里面，所有的 Java 对象都会通过 IoC 容器转变成 Bean （Spring 对象的一种称呼，以后我们都用 Bean 来表示 Java 对象）构成应用程序主干和有 Spring  IoC 容器管理的对象称为 beans ，beans 和它们之间的依赖反映在容器使用的配置元数据里面，基本上所有的 Bean 都是由接口 + 实现类完成的，用户想要获取 Bean 的实例直接聪 IoC 容器获取就可以了，不需要关心实现类

Spring 主要由两种配置元数据的方式，一种是基于XML 一种是基于 Annotation 方案的，目前主流是基于 Annotation 的，所以我们这里也是以 Annotation 为基础方案来的。

org.springframework.context.ApplicationContest 接口类定义容器的对外服务，通过这个接口，我们可以轻松的从 IoC 容器中得到 Bean 对象。我们在启动 java 程序的时候必须先启动 IoC

Annotation 类型满足的 IoC 容器：

```java
org.springframework.context.annotation.AnnotationConfigApplicationContext
```

我们如果要启动 IoC 容器，可以运行一下代码：

```java
ApplicationContext context = 
    new AnnotationConfigApplicationContext("fm.douban");
```

这个代码的含义就是启动 IoC 容器，并且会自动加载包 fm.douban 下的 Bean，只要引用了 Spring 注解的类都可以被加载（前提是在这个包内）

AnnotationConfigApplicationContext 这个类构造函数由两种：

```java
AnnotationConfigApplicationContext(String... basePackages)  //根据包名实例化
AnnotationConfigApplicationContext(Class class)  //根据自定义包扫描行为进行实例化
```

## Spring Bean 的注解有以下几种：

* org.springframework.stereotype.Service
* org.springframework.stereotype.Component
* org.springframework.stereotype.Controller
* org.springframework.stereotype.Repository

只要我们在类上引用这类注解，就会被 IoC容器加载

* @Component 注解是通用的 Bean 注解，其余三个都是扩展自 Component
* @Service 代表的是 Service Bean
* @Controller 作用于 Web Bean
* @Repository 作用于持久化相关 Bean

实际上 这四个注解都会被 IoC 容器加载，一般情况下，我们会使用 @Service ；如果是 Web 服务就用 @Controller

```java
package fm.douban;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import fm.douban.service.SongService;
import fm.douban.model.Song;

/**
 * Application
 */

public class Application {

    public static void main() {
          ApplicationContext context = new       AnnotationConfigApplicationContext("fm.douban");
          SongService sonService = context.getBean("sonService.class");
          Song song = songService.get("001");
          System.out.println(song.getName());
    }
}
```

## Spring Bean:

依赖注入的第一步是完成容器的启动，第二步就是真正的完成依赖注入

**依赖注入是一种编程思想**，简单来说就是一种获取其他实例化的规范

比如豆瓣：

* 我们可以通过一个专辑获取这个专辑包含的歌曲，

* 我们新加了一个 SubjectService 和它的实现类 SubjectServiceImpl， 用来完成获取专辑的服务，在这个接口里面，我们定义了一个 get 方法，传入参数为 subjectId，我们期望得到编辑的信息（包括专辑包含的歌曲）
  

* 对于 Song，Subject 这两个 POJO 类，在这个模型，我们如果带着 Subject 的 id 去循环历遍所有的歌曲，筛选出来的都是专辑包含的歌曲，所以我们在 SongService 里面计入了 list 方法。用于查询专辑歌曲。

* 回到 SubjectServiceImpl 类，如果我们像获得完整的专辑信息，就要引入 SongService 的实例。调用歌曲：
  
  ```java
  public class subjectServiceImpl implements SubjectService {
  
      private SongService songService;
  
      // 缓存所有的专辑数据：
       private static Map<String, Subject> subejectMap = new HashMap<>();
  
      static {
          Subject subject = new Subject();
          // 略初始化
          subjectMap.put(subject.getId(), subject);
      }
  
      @Override
      public Subject get(String subjectId) {
          Subject subject = subjectMap.get(subjectId);
          //调用 SongService 获取专辑
          List<Song> songs = songService.list(subjectId);
          subject.setSongs(songs);
          return subject;
      }
  
      public void setSongService(SongService songService) {
          this.songService = songService;
      }
  }
  ```
  
  * 我们如何获取 Song Service 的实例？
    
    ```java
    import fm.douban.model.Song;
    import fm.douban.model.Subject;
    import fm.douban.service.SongService;
    import fm.douban.service.SubjectService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Service;
    
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    
    @Service 
    public class SubjectServiceImpl implements SubjectService {
    
        @Autowired
        private SongService songService;
    
        //缓存所有的数据
        private static Map<String, Subject> subjectMap = new HashMap<>();
        static {
            Subject subject = new Subject();
            subject.setId("s001");
            //初始化
            subjectMap.put(subject.getId, subject);
        }
    
        @Override
        public Subject get(String, subjectId) {
            Subject subject = subjectMap.get(subjectId);
            //调用 SongService 获取专辑
            List<Song> song = songService.list(subjectId);
            subject.setSongs(songs);
            return subject;
        }
    }
    ```


在改动之前，我们要使用 SubjectService 的地方，都需要编程实例化对象：

```java
SubjectService subjectService = new SubjectServiceImpl();
SongService songService = new SongService();
subjectServce.setSongService(songService);
```

但是如果 SubjectService 的依赖太多了怎么办？那就要 new 很多个服务实例了。

所以，大量的编码 new 出服务实例，会导致整个项目极其容易出错，包括各种错误。

我们可以加注释来解决，是让 Spring 系统 自动管理各种实例。

所谓管理就是用 @Service 注解把 SubjectServiceImpl 和 SongServiceImpl 等等所有服务实现，用 @Autowired 注解标记，告诉 Spring 这里需要注入实现类的实例。

项目启动过程中，Spring 会自动 实例化服务实现类，然后急救上诉的错误

@Service 和 @Autowired 是相辅相成的，如果 SongServiceImpl 没有加 @Service 就意味着没有标记成 Spring Bean， 那么即使加上 @Autowired 也无法注入实例；而 private SongServiceImpl；属性忘记加 @Autowired ，Spring Bean 也无法注入实例。

每个 Annatation 都有各自的功能，Spring 检查代码中有注解，就会自动完成特定的功能。

## Spring Resource:

Spring 的文件处理方案， Spring Resource  

对于平常的代码读取操作：

```java
import java.io.File

/**
* Test
*/


public class Test {
    public static void main(String[] args) {

    File file = new File("mywork/readme.md");
    if(file.exists()){
        System.out.println(readme.md is exists);      
    }
    File file2 = new File("src/main/sources/nani.jpg");
    if(file2.exists()){
        System.out.println("nani.jpg is exists");

    }
  } 
}
```

看起来两个文件都可以加载，但是运行后你灰发现 src/main/sources/nani.jpg 无法打开，当我们下载 app-1.0-SNAPSHOT.jar 解压后会发现 src/main/java 文件都没了。

这就是java文件系统和计算机文件系统的差距,工程目录最后是要编译称为 jar 文件的， jar 文件是从 包路径 开始的。Maven 工程编译之后，会自动去掉 src/main/java ， src/main/resources 目录的

如何读取 jar 内部文件？

# classpath

在 java 虚拟机里面，我们一般吧文件路径称为 classpath，所以读取内部文件就是从 classpath内部读取，classpath 制定的文件就不能解析成 File 对象了 但是可以解析成为 InputStream, 我们借助 JavaIO 就可以读取。

Classpath 类似虚拟机目录，它的根目录是从 / 开始代表是 src/main/java 或者 src/main/resources 目录

我们来用 classpath 来读取一下 resources 下面的 data.json 文件。

> java 有很多丰富的三方库，我们可以用 commons-io 来读文件

```pom.xml
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```

Test

```java
public calss Test {

    public static void main(String[] args){
    //读取 classpath 的内容
    InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
    //适用 common-io 读取文件
    try {
    String content = IOUtils.toString(in, "utf-8");
    System.out.println(content);
    } catch (IOException e){
        e.printStackTrace();get
    }
  }
}
```

对于代码`InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");`

这代码就是从 java 运行加载器里面查找文件 Test.class 指的当前的 Test。java 编译后的 java class 文件

# Spring Resource

Spring 擅长就是封装各种服务

在 Spring 当中定义了一个 org.springframework.core.io.Resource 类来封装文件，这个类的优势在于可以支持普通的 File 也可以支持的 classpath 文件。

并且在 Spring 中通过 org.springframework.core.io.ResourceLoader 服务来提供任意文件的读写，你可以在任意的 Spring Bean 中引入 ResourceLoader

```java
@Autowried
private  ResourceLoader loader;
```

创建一个FileService

```java
public interface FileService{
    String getContent(String name);
}
```

类的实现：

```java
import fm.douban.service.FileService;
import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.io.InputStream;



@Service 
public class FileService implements FileService{

    @Autowired
    private ResourceLoader loader;
    @Override
    public String getContent(String name){
    try{
        InputStream in = loader.getResource(name).getInputStream();
        return IOUtils.toString(in, "utf-8");  
    }catch(IOException e) {
        return null;
    }
  }
}
```

服务调用：

```java
FileService fileService = context.getBean(FileService.class);
String content = fileService.getContent("classpath:data/urls.txt");
System.out.println(content);
```

也可以用来读本地文件：

```java
String content2 = fileService.getContent("file:maywork/readme.md");
```

同时 Resource 还可以进行远程文件的加载：

```java
String content2 = fileService.getContent("https://www.zhihu.com/question/34786516/answer/822686390");
System.out.println(content2);
```

# Spring Bean 的生命周期（Lifecycle）

为了管理 Bean， Spring Bean 提供了生命周期管理能力，着将极大的提高工程化的能力

> 生命周期： ![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-12-21-38-image.png)

我们在以后遇见运行的问题可能会用到

大部分的时候，我们只需要掌握 init 方法即可， init 方法的名称可以是任意名称，我们以 SubjectService 为例

```java
import javax.annotation.PostConstruct;


@Service
public class SubjectServiceImpl implements SubjectService {
    @PostConstruct
    public void init() {
    System.out.println("The device is running");
}
}
```

有了 init 方法，我们就可以吧之前的 static 代码块的内容移动到 init 里面：

```java
@Service 
public class SubjectServiceImpl implements SubjectService {
    @PostConstruct
    public void init() {
    Subject subject = new Subject();
    subject.srtId("s001");
    subject.setName("成都");
    subject.setMusician("赵雷");
    subjectMao.put(subject.getId(), subject);
}
}
```

# Spring Boot：

面向微服务的框架： Spring Boot

Spring Boot 可以认为是一个 Spring 的快速入手的方案，，她极大的 降低了 Java web 工程的创建和运行，部署的难度，

Spring Boot 的核心还是 Spring （所以你无需单独管理 Spring 的 Maven 依赖），只是多了一些工程化方案

* 比如说 Java web 容器的嵌入式集成 （所以有了 Spring boot， 就不再需要额外部署 Tomcat 这种类服务器）， Spring Boot 集成了 Tomat

* Spring booy 还自定义了工程打包格式，通过这个直接把一个 Java Web 工程转换成为普通的 Java 工程，启动一个  main 方法就可以把 Spring 工程启动 ，极大的降低了开发难度

* Spring Boot 默认集成了很多第三方框架和服务，比如数据库连接， NoSQL， 安全等等，开发者不用关心复杂的 Maven 依赖，开箱可用

* Spring Boot 害提供了标准的属性配置文件， 支持应用的参数动态配置

Spring Boot 强调的是 开箱即用 ，这是一种重要的软件工程思想。

Spring Boot的版本很快，一般选择 RELEASE 版本。

例如：

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.0.RELEASE</version>
</parent>
```

# Spring Contoller

> 通过 Spring Boot 我们已经快速搞定 Spring MVC 工程。Spring MVC 是 Java Web 的一种实现框架 。Java Web 的范畴就是 Servleet 技术，所有的 Java Web 都实现了 Servlet API的定义

官方 Servlet 规范理解起来有困难， 所以我们可以先去学习如何运用 Web 技术描述连掌握之后再去理解原理，目前来说并不需要可以的去学习 Servlet ，随着技术的普及， Servlet 也不是唯一的标准了

对于 Web 服务：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-16-37-07-image.png)

> 所有的的网页都是这样的一个过程。在 Spring Boot 方案里面，一个网页请求到服务器之后，首先我们进入的是 Java Web 服务器。然后进入 Spring Boot 应用最后 匹配到某个 Spring Controller 然后路由到某个具体的 Bean 方法执行返回结果，输出给客户端。

从这个流程，我们只要掌握 Spring Controller 就可以自己提供 Web 服务

Spring Contoller 技术的三个核心：

* Bean 的配置：Controller 注解运用

* 网络资源加载： 加载网页

* 网址路由的配置： RequestMapping 注解的运用

## Controller 注解：

Spring Controller 本身也是 Spring Bean ，只是它多提供了 Web 能力，我们只需要在类上提供一个 @Controller 注解就可以。

```java
@Controller
public class HelloControl {

}
```

> 虽然 Spring 大部分都是基于接口开发， 但是没有接口也是可以工作的。Spring Controller 一般情况下不需要特意的实现接口

## 加载网页

在Spring Boot 应用里面，一般把网页存放在 src/main/resources/static 目录下面

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-16-50-42-image.png)

在 controller 中，会自动加载 static 下的 html 内容，所以通过 Spring Boot 建设网站很简单

```java
@controller
public class HelloControl{
    public String say(){
        return "hell.html"
    }
}
```

注意上面的 say 方法，

* 定义的返回类型是 String

* return "hello.html"返回的是 html 路径

> 执行这段代码的时候，Spring Boot 实际上是在加载 src/main/resources/static/hello.html

我们知道，resources 属于 classpath 类型文件，Spring Boot 很强大，自动帮我们做了加载，所以我们只需要写 hello.html

```html
<!DOTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meat name="viewport" content="width=device-width, inital-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <h1>Hello Spring</h1>
</body>
</html>
```

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-17-09-00-image.png)

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-17-09-10-image.png)

## RequstMapping

对于 Web 服务器来说，必须要实现的一个能力就是解析 URL，并且提供资源给内容调用者，这个过程叫做路由。

Spring MVC 完美的支持了路由的能力。并且简化了路由配置，只需要提供 Web 访问的 方法上添加一个 @RequestMapping 注解就可完成配置了

```java
@Controller
public class HelloControl {
    @RequestMapping("/hello")
    public String say() {
        return "html/hello.html";
    }
}
```

# Get Request:

在 Http 网络里面，最常用的两个协议是

* get

* post

平常我们浏览网站，看视屏，看图片使用的都是 get 协议，现在让我们看一下如何使用 Spring MVC 来支持 Http 服务端 get 协议

通过 get 协议，我们可以动态的渲染网页，在上节课的基础上，我们还需要掌握get 请求参数的解析，通过获取参数，我们可以获取特定的代码逻辑

* 比如我们访问 url 获取内容

* 当我们把参数 wd 换成 xxx,就会显示相关内容

我们会发现，同一个网址，只是换了参数值，内容就会发生变化，这项技术就是 URL 参数解析，这也是 get request 必须掌握的能力。

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-22-21-11-55-image.png)

## 获取 Http URL 参数

每个 Http URL 都会有自定义的参数， 像上面的百度 URL 中的 wd ，这个就是百度自定义的。

### 需求：

现在我们自定义一下歌单的参数。我们希望可以根据歌单参数访问到不同的歌单界面。

如果需要解决这个需求，我们需要定义一个能够表达歌单的参数。前面定义的歌单领域模型的时候，我们知道歌单的 Id 是可以作为索引到唯一一个歌单的，所以我们可以用这个参数来作为我们的 URL 那摩结合之前，我们的 URL 可以是

```URL
https://域名/songlist?id=xxxx
```

## 定义参数：

在 Spring MVC 里面，定义一个 URL 参数很重要，只需要我们在方法上添加对应的参数和参数注解就可以了，

```java
@Controller
public class SongListControl {
    @RequestMapping("/songlsiy")
    public String index(@RequestParam("id") String id){
        return "html/songList.html";
    }
}
```

上面的代码比比之前多了：`@RequestParam("id") String id` 注意 RequestParam 注解的参数 “id” 这个值必须要和 URL 里面的 paramkey一致，因为我们在 url 中定义的是 id 所以这里写 id

如果 URL 是 https://xxx/songListId=xxx

那么就是：

```java
@RequestMapping("/songList")
public String index (@RequestParam("listId") String id){
    return "html/songList.html";
}
```

RequestParam 的注解包在：

```java
org.springframework.web.bind.annotation.RequestParam`
```

由于 Spring MVC 的注解都是在 org.springframework.web.bind.annotation 包内，所以我们 import 的时候，世界用 import org.springframework.web.bind.annotation.*

## 操作参数：

现在我们完善一下代码，我们知道访问不同的歌单，打开的就是不同的歌单界面，所以我们要模拟一下行为，根据歌单选择页面。

在上面的代码里面，我们添加一个分支，如果没有找到歌曲，就返回404.

```java
@Controller
public class SongListControl{
    @RequestMapping("/songList")
    public String index(@RequestParam("id") String id){
        if("2323".equals(id){
            return "html/songList.html";
        }else {
            return "html/404.html";
        }        
    }
}
```

Spring Controller 作为服务端框架 的核心作用：自动解析请求 URL ， 自动找到对应的方法并执行。

## 获取多个参数：

获取多个参数很简单，就是添加参数就可以了。比如：

```java
@Controller
public class SongListControl{
    @RequestMapping
    public String index(@RequestParam("id") String id, @RequestParam("pageNum") int name){
        return "html/songList.html";
    }
}
```

基础的 boolean, int , String 数据类型是可以直接自动转换的，所以上面的页数参数使用了 int

## 总结：

当我们掌握了 Spring Request 请求，基本上就打开了 Web大门，所有的网站和程序都是通过这个技术搭建的，只是不同系统的代码逻辑不同。

# @GetMapping：

我们在一开始学的 @RequestMapping 注解用于解析 URL 请求路径，这个注解默认是支持所有的 Http Method 这样不是很安全，一般我们还是会明确制定 method ，比如说 get 请求 

我们可以使用 @GetMapping 来替换注解 @RequestMapping 我们的包路径是一样的，我们之前的代码可以写成

```java
@GetMapping("/songlist")
public String index(@RequestParam("id") String id,@RequestParam("pageNum") String pageNum){
    return "html/songList.html";
}
```

## 非必须传递参数：

默认情况下，访问的 URL 中必须包含我们在 Request 服务里面设定参数

但是如果不想传递参数可以这样：

```java
@GetMapping("/songlist")
public String index(@RequestParam(name="pageNum", required = false) int pageNum, @RequestParam("id") String id ){
    return "html/songList.html";
}
```

写法：

@RequestParam(name="pageNum", required = false) int pageNum, @RequestParam("id") String id)

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-23-10-53-21-image.png)

> 到这里可能大家会问，方法定义参数的顺序和 URL 的参数顺序有没有关系，没有关系。参数顺序无所谓

## 输出 JSON 数据：

我们之前的例子都是返回 HTML 的内容，但是有时候作为服务端，我们只想返回数据，目前来说通用的 Web 数据格式就是 JSON ，在 Spring 当中配置 JSON 数据很简单。

```java
@GetMapping("/api/foos")
@ResponseBoby
public String getFoos(@RequestParam("id") String id){
    return "ID:" + id
}
```

字符串也是 JSON 数据的一种，我们在 getFoos 方法上面，添加了 @ResponseBody 注解，这个注解的包和 RequestParam 包一样，

我们调用 URL ： https://xxx/api/foos?id=100

我们试试返回 java 对象：

```java
public class User{
    private String id;
    private String name;

    //get,set...//
}

public class UserControl{
    //缓存 User 数据
    private static Map<String, User> users = new HashMap();
    /**
     * 初始化数据
     */
    @PostContruct
    public void init(){
        User.user = new User();
        user.setId("100");
        user.setName("xxx");
        users.put(user.getId(), user);
    }

    @GetMapping("/api/user")
    @ReponseBody
    public User getUser(@RequestParam("id") String id) {
        return uesrs.get(id);
    }
}
```

你会发现，Spring MVC 会自动的把对象转换成 JSON 字符串输出到网页，这也是 Spring MVC 比较厉害的地方，一般我们会把这种输出 JSON 数据的方法称为 API

## Thymeleaf 入门

Web 开发离不开动态页面开发，很早就有企业使用 JSP 技术来开发网页

，随着技术的升级替换，目前来说最主流的方案就是 Tymeleaf ，Thymeleaf 是一个模板框架，它可以支持多种格式的内容动态渲染非常强大，它天然和 HTML 是相容的。

Web 工程师 基本上也必须掌握一门模板框架的，要不然无法办法成功完成动态网页开发。

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-23-17-00-42-image.png)

如上图，我们通过模板引擎，可以把 Java 对象数据 + 模板页面 动态的渲染出一个真实的 HTML 页面来。

上面的例子，如果 name 变成其他歌名的名单，那么页面渲染后也自然就会变成新的歌单名称

模板引擎在所有的 Web 编程语言里都有类似的方案，所以掌握一个框架，学其他的也会很简单。它的机制简单来说就是 **数据+模板+引擎**渲染出真实的页面

## 如何初始化 Thymeleaf：

### 添加 Maven 依赖：

由于现代的工程都是基于 Spring Boot 来搭建的，所以我们使用 Thymeleaf 只要添加依赖就可以了。

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### 数据传递：

Spring MVC 把页面数据层封装的非常完美，只要我们在放啊参数里面引入一个 Model 对象，就可以通过这个 Model 对象传递数据到页面里面。

导入 Model:

```java
import org.springframework.ui.Model;
```

```java
@Controller
public class SongListControl {

    @Autowired
    private SongListService songListService;

    @RequestMapping("/songlist")
    public String index(@RequestParam("id")String id, Model model){
        SongList songList = songListService.get(id);
        model.addAttribute("songList",songList);
        return "songList";
    }
}
```

## 模板文件：

Spring MVC 中对于模板文件是有固定的存放位置，放置在工程的 src/main/resource/templates

所以上面的 return “songList”； 其实会自动查找 src/main/resources/templates/songList.html 文件，系统会自动分配路径，不需要写成 return “songList.html"

Thymeleaf 模板文件也是 html 作为文件格式的，所以它也是最容易学习的模板

```html
<!DOTYPE html>
<html lang-"en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>豆瓣歌单</title>
    </head>
    <body>
        <h1 th:text="${songList.name}"></h1>
    </body>
</html>
```

模板文件的最后虽然也是  .html ，大部分内容跟 HTML 文件很像，但它放置在 src/main/resources/templates 目录下面，而且里面可以写变量 : th:text="${...}"  所以它不是 html 文件，而是 thymeleaf 模板

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-23-17-26-59-image.png)

## 特别的：

只要在工程 src/main/resources/templates 下面放置了文件，就表示默认使用 thymeleaf ，这就是文件的模板

> 放在 src/main/resources/static 里面的是静态文件

### Thymeleaf 变量：

搞定工程环境后，我们就开始学习模板语法：

Thymeleaf 模板支持语法很牛逼，相当于是一门动态变成语言，所以很多语言的特性它都有，比如变量，循环，条件等。

#### 模板变量：

由于 Thymeleaf 是完全兼容 HTML 的， 所以为了不破坏 HTML 结构， Thymeleaf 采用了自定义 HTML 属性的方式来生成动态内容。

th:text 这个属性就是 Thymeleaf 自定义的 HTML 标签属性， th 是 Thymeleaf 缩写。

th:text 的语法作用是会动态的替换掉 Html 标签里面内部内容比如：

```html
<span th:text"${msg}"> hello </span>
```

这个代码的意思就是用 msg 的变量值替换了 span 标签内的 Hello 字符串，比如说 msg 内容都是 你好

```html```
<span>你好</span>

```
在来看看如何读取变量，上面的 th 属性内 ${msg} 这个语法就是表示获取模板中的变量 msg，语法格式 :

${xxx}

比如：

```java
@Controller
public class DemoControl {

    @RequestMapping("/demo")
    public String index(Model model){
        String str = "你好";
        model.addAttribute("msg", msg);
        return "demo";
    }
}
```

对于方法 model.addAttribute("msg", msg) 这个方法

* 第一个参数设置就是上下文变量

* 第二个参数设置的是变量名

## 对象变量：

模板语言还可以支持对复杂对象的输出， 我们完全可以使用 .  把属性调用出来。比如我们的歌单对象 SongList 

```java
import org.springframework.ui.Model;

@Controller
public class DemoControl{
    @RequestMapping("/demo")
    public String index(Model model) {
        SongList songList = new SongList();
        songList.setId("000001");
        songList.setName("xxx");
        model.addAtrribute("sl", songList);
        return "demo";
    }
}
```

我们在模板里面可以通过 th:text="sl.name" , th:text="sl.id" 分别得到 name ,id 值

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
  </head>
    <body>
        <span th:text="${sl.id}"></span>
        <span th:text="${sl.name}"></span>
    </body>
</html>
```

对象包含的属性 都可以使用 . 出来，前提是对象必须是 POJO 类

## Thymeleaf 循环语句：

Thmeleaf 的 for 循环也是使用标签属性来完成的， th:each 代表的就是循环语句。

```html
<ul th:text="song : ${songs}">
    <li th:text="${song.name}">歌曲名单 </li>
</ul>
```

基本上都是遵循上述语法。

```java
@RequestMappingh("/demo")
public String index(Model model){
    List<Song> songs = new ArrayList();
    Song song = new Song();
    song.setId("00001");
    song.setName("xx");
    songs.add(song);

    song = new Song();
    song.setId("00002");
    song.setName("xxx");
    songs.add(song);

    model.addAtrribute("songs", songs);
    return "demo";
}
```

## 打印列表索引

```html
<ul th:each="song, it: ${songs}">
    <li>
        <span th:text="${it.content}"></span>
        <span th:text="${song.name}"></span>
    <li>
</ul>
```

## Tymeleaf 表达式：

作为一个动态语言，必不可少的是动态. Tymeleaf 表达式 对于 动态数据处理更加方便。

* 字符串处理

* 数据转换

### 字符串处理：

我们在很多网站会看到时评时间的显示效果，

00:00/45:00

大家很清楚这个是视频从 0 分 0 秒 开始，总共 45 分钟。 在 Tymeleaf 中这样的显示要借助 + 完成字符串拼接。

```html
<span th:text="'00:00/' + ${totalTime}"></sapn> 
```

这里面我们使用 ‘00:00/’ 使用 “ 包围 00:00/ 这个文本的作用是把这个文本变成 java 字符串，两个字符串就可以使用 + 进行拼接称为新的字符串。我们的 Contorl 代码也需要进行调整。

```java
@RequestMapping("/demo")
public String index(Model model){
    String totalTime = "45:00";
    model.addAttribute("totalTime", totalTime);
    return "demo";
}
```

如果不加 ‘ 包围住 00:00 

```html
<span th:text="00:00/ + ${totalTime}"></span>
```

那么就会报一下错误：

```java
Wed Jan 08 14:40:15 CST 2020
There was an unexpected error (type=Internal Server Error, status=500).
Could not parse as expression: "00:00/+${totalTime}" (template: "demo" - line 7, col 9)
```

## 字符串拼接优化：

Thymeleaf 做字符串的拼接还做了优化工作。我们可以使用 上面的代码 | 包围住字符串，这样 | 包围字符串，这样就不需要在 文字后面附加 ‘...' + '...' 

```html
<span th:text="|00:00/${totalTime}|"></span>
```

## 数据转换：

Thymeleaf 默认集成了大量的工具方便的进行数据转化，一般我们使用最多的就是 dates

如果 你象想处理LocalDate 和 LocalDateTime 类，你可以在 pom.xml 里面添加如下依赖：

```xml```
<dependency> <groupId>org.thymeleaf.extras</groupId>
 <artifactId>thymeleaf-extras-java8time</artifactId>
 <version>3.0.4.RELEASE</version>
</dependency>

```
这个库会自动添加一个新的工具类 temporals

工具类和变量不同， 变量的是 ${变量名}  而工具是 #{工具类}

## dates / temporals:

dates 与 temprols 支持的方法是一样的，只是 LocalDate 和 LocalDateTime 

我们一般使用 dates / temprols 用于处理日期类到字符串的转换，比如显示 年月日

```html
<p th:text="${#dates.format(dtaeVar, 'yyyy-mm-dd')}"></p>
<p th:text="${#dates.format(dateVar, 'yyyy年mm月dd日')}"></p>
```

如显示年月日 时分秒:

```html
<p th:text="${#date.format(dateVar, 'yyyy-MM-dd HH:mm:ss')}"></p>
<p tg:text="${#date.format(dateVar, 'yyyy年MM月dd日 HH时mm分ss秒')}"></p>
```

Java 代码

```java
@RequestMapping("/demo")
public String index(Model model) {
    Date dateVar = new Date();
    model.addAttribute("dateVar", dateVar);
    return "demo";
}
```

如果是日期类 LocalDate / LocalDateTime 那么就把 #date 换成 #temprols 

```java
@RequestMapping("/demo")
public String index(Model model){
    LocalDateTime dateVar = LocalDateTime.now();
    model.addAttribute("dateVar", dateVar);
    return "demo";
}
```

## String:

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-29-22-39-38-image.png)

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-10-29-22-40-25-image.png)

<a href="[Arrays (thymeleaf 3.0.11.RELEASE API)](https://www.thymeleaf.org/apidocs/thymeleaf/3.0.11.RELEASE/org/thymeleaf/expression/Arrays.html)" target="_blank_">文档</a>

## 内联表达式：

尽管我们使用 th:text 很方便，但是我们更喜欢直接把变量写入 HTML 里面，

```html
<span>Hello [[${msg}]]</span>
```

上面的 [[变量]] 就是内联表达式，直接在 HTML 中调用变量。

```java
@requestMapping("/demo")
public String index(Model model){
    String msg = "丫丫";
    model.addAttribute("msg", msg);
    return "demo";
}
```

现在我们新写一下：

```html
<p>[[${#date.format(dateVar, 'yyyy-MM-dd HH-mm-ss')}]]</p>
```

th:text 与 [[]] 两种写法都被允许

## Tyymeleaf 表单：

我们需要完成一个简易版的图书管理器

### 图书模型：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-01-13-58-38-image.png)

上面的 UML 写成 Java 代码是：

```java
public class Book {
    private long id;
    private String namel
    private String author;
    private String desc;
    private String isbn;
    private double price;
    private String pictureUrl;
}
```

> 我们把 id 主键的类型设置为 long ，这是因为 long 类型的 id 更加容易搜索引擎。如果期望被搜索引擎关注到，那么就可以把 id 设置为 long 否则还是用 String 因为 long 很容易被机器猜到，有更容易被爬虫爬取

### 页面开发：

```html
<form>
  <div>
    <label>书的名称:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的作者:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的描述:</label>
    <textarea></textarea>
  </div>
  <div>
    <label>书的编号:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的价格:</label>
    <input type="text" />
  </div>
  <div>
    <label>书的封面:</label>
    <input type="text" />
  </div>
  <div>
    <button type="submit">注册</button>
  </div>
</form>
```

为了能够访问到这个页面。我们还要配置一下 Spring Control

```java
@Controller
public class BookControl {
    @GetMapping("/book/add.html")
    public String addBookHtml(Model model){
        return "addBook";
    }
}
```

### 保存书籍：

```java
@Controller
public class BookControl {
    private static List<Book> books = new ArrayList<>();

@GetMapping("/book/add.html")
public String addBookHtml(Model model){
        return "addBook";
    }

@PostMapping("/book/save")
public String saveBook(Book book){
        books.add(book);
        return "saveBookSuccess";
    }
}
```

### 新增一个 templates/saveBookSuccess.html 文件

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍成功</h2>
</body>

</html>
```

### form 表单：

我们还需要改一下 html form ，需要指定 form 的 action 属性值就是后端的请求路径。由于我们是写的 / 开头，浏览器会自动把请求地址识别为 http://domain/user/reg 

除了 form 属性要调整，还要修改 input 的 那么属性， 属性 和 Book 类的属性要一样。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加书籍</title>
</head>

<body>
  <h2>添加书籍</h2>
  <form action="/book/save" method="POST">
    <div>
      <label>书的名称:</label>
      <input type="text" name="name">
    </div>
    <div>
      <label>书的作者:</label>
      <input type="text" name="author">
    </div>
    <div>
      <label>书的描述:</label>
      <textarea name="desc"></textarea>
    </div>
    <div>
      <label>书的编号:</label>
      <input type="text" name="isbn">
    </div>
    <div>
      <label>书的价格:</label>
      <input type="text" name="price">
    </div>
    <div>
      <label>书的封面:</label>
      <input type="text" name="pictureUrl">
    </div>
    <div>
      <button type="submit">注册</button>
    </div>
  </form>
</body>

</html>
```

## Spring Validation:

现在我们完成了书籍的添加逻辑。在实际工作中对于数据的保存是离不开数据验证的，比如说 name 必须输入， isbn 必须满足验证要求。Spring 对于数据验证支持做的很好。我们可以借助 Spring Valiidation 来处理表单数据的验证。

## JSR 380：

JSR 是 java Specification Requests 的缩写。意思是 java 规提案。是指向 JCP 提出的新增一个技术规范的正式请求。任何人都可以提交 JSR， 以向 Java 平台新增的 API 服务。 JSR 已经成为 Java 界的一个重要标准。

JSR 380 其实是 Bean Validation ,这个就是 Bean 验证规范，。这里面的 Bean 就是我们一直在说的实例化 POJO类， 比如前面的 Book JSR 380提案的规范可以通过下面的依赖添加到工程里面

```html
<dependency>
  <groupId>jakarta.validation</groupId>
  <artifactId>jakarta.validation-api</artifactId>
  <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

Spring Validation 也是 JSR 380 提案的一个实现方案。

随着 Spring Boot 版本提高。有的版本自动加入了这两个 Spring Valication 的依赖包。

## Valication 注解：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-01-16-12-36-image.png)

```java
public class User {
    @NotEmpty(message = "名称不能为空 null")
    private String name;

    @Min(value = 18, message = "你的年龄必须大于等于 18 岁")
    @Max(value = 150, message = "你的年龄必须小于 150 岁")

    private in tage;

    @NotEmpty(message = "邮箱必须填入")
    @Email(message = "邮箱不正确")
    private String email;
    
}
```

大多数情况下， 我们建议使用 NotEmpty 代替 NotNull NotBlank

校验的注解是可以进行累加，如上面的 @Min @Max ，系统会按顺序执行椒盐。

## 创建一个表单页：

创建一个 user/addUser.html

```html
<!DOCTYPE html>
<html lang="en" xmls:th="http://www.thymeleaf.org">

<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>
<body>
    <h2>添加用户</h2>
    <form action="/user/save" method="POST">
        <div>
            <lable>用户名称：</lable>
            <input type="text" name="name">
        </div>
        <div>
            <lable>年龄：</lable>
            <input type="text" name="age">
        </div>
        <div>
            <lable>邮箱：</lable>
            <input type="text" name="email">
        </div>
        <div>
            <button type="submit">保存</button>
        </div>
    </form>
</body>

</html>
```

## mapping:

我们还要在 control 类里面设置 mapping

```java
@Controler

public String UserContrl{

    @GetMapping("user/add.html")
    public Stringt addUser() {
        return "user/addUser";
    }
}
```

## 执行验证

前面 adduser.html 页面里面的 form action 配置是 /user/save 所以我们增加一个这个请求的 Control 代码

就可以执行校验了， 在 Spring MVC 当中执行校验很简单。

```java
@Controller
public class UserControl {

    @GetMapping(/user/add.html)
    public String addUser() {
        return "user/addUser";
    }

    @PostMapping("/user/save")
    public String addUser(){
        return "user/addUser";
    }

    @PostMapping("/user/save")
    public String saveUser(@Valid User user, BingingResult error) {
        if(errors.hasErrors() {
            return "user/addUser";
        }

        return "user/addUserSuccess";
    }
}
```

大家仔细看 saveUser 这个方法的参数，在第一个参数 user 那，我们添加了参数注解 @Valid，然后我们新增了第二个参数 errors(它的类型是 BindingResult), 顺序不要写错。

* 如果失败，返回添加用户页面

* 如果成功，显示成功页面

addUserSuccess.html

创建一个 user / addUserSuccess.html 模板文件

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>添加用户</title>
</head>

<body>
  <h2>添加用户成功</h2>
</body>

</html>
```

```

```

## Thymeleaf 布局 (Layout)

大多数网站都有导航，底部等物件，一个网站里面访问页面总是会显示会显示相同的导航，底部

layout 解决的是模板复用的问题，比如常见的网站是下面这样的。

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-09-16-23-16-image.png)

按照这个布局，我们可以把导航和底部做成布局组件，每个页面套用就可以了

我们推荐使用 th:include + th:replace 方案来完成布局开发

## layout.html:

现在我们来继续完成一下 bookstore ，创建一个 layou.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>布局</title>
    <style>
        .header {background-color: #f5f5f5;padding: 20px;}
        .header a {padding: 0 20px;}
        .container {padding: 20px;margin:20px auto;}
        .footer {height: 40px;background-color: #f5f5f5;border-top: 1px solid #ddd;padding: 20px;}
    </style>
</head>
<body>
<header class="header">
    <div>
        <a herf="/book/list.html">图书管理系统</a>
        <a herf="/user/list.html">用户管理</a>
    <div>
</header>

<div class= "container" th:include="::content">页面正文内容</div>
<footer class="footer">
    <div>
        <p style="float: left">© xxxx.com 2017</p>
        <p style="float: right">
            Powered by me
        </p>
    <div>
</footer>
</body>
```

在上面的代码里面，我们添加了 header ， container ， footer  三个节点。重点在 container 这个节点上面，我们使用了一个 th:include="::content" 语法

## th:include"::content"

这个语法里面  ::content  是选择器 ，这个选择器 指的就是加载当前页面的 th:fragment  的值

当页面渲染的时候，布局会合并 content 这个 fragment 的值。

当页面渲染的时候，布局会合并 content 这个 fragment 内容一起渲染，下面我们会配置 fragment

## user/list.html

现在我们继续改造一下 user.list.html 让这个页面也支持布局：

```html
<!DOCTYPE html>
<html lang="en" xmls:th="http://www.thymeleaf.org"
                            th:replace="layout">
<div th:fragment="content">
    <h2>用户列表</h2>
    <div>
        <a href="user/add.html">添加用户</a>
    <div>
    <table>
        <thead>
        <tr>
            <th>
                用户名
            </th>
            <th>
                用户年龄
            </th>
            <th>
                用户邮箱
            </th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="user : ${users}">
            <td th:text="${user.name}"></td>
            <td th:text="${user.age}"></td>
            <td th:text="${user.email}"></td>
        </tr>
        </tbody>
    </table>
</div>
</html>
```

## th:replace="layout"

这里指定了布局的名称，这个一但声名后，页面会被替代换成 layout 的内容，layout指的是 templates/layout.html

## th:fragment="content"

```html
<div th:fragment="content">
</div>
```

fragment 是指片段的意思，当页面渲染的时候，可以通过选择器使用这个片段。在上面 layout.html 文件的 th:include="::content" 指定的就是这个值

## Spring Boot ComponentScan

我们知道了 IOC 的工作模式，知道了 Spring 框架通过 解析 属性的注解，自动把需要的 Bean 实例注入到属性中。

那摩我们可能遇见这样的错误：

```java
Field subjectService in fm.douban.app.control.SongListControl required a bean of type 'fm.douban.service.SubjectService' that could not be found.
```

意思是， 找不到需要注入的 bean ：

fm.douban.service.SubjectService, 导致启动失败。这是为什么？

在前面我们知道，加上 @SpringBootApplication 注解的类是启动类，是整个系统的启动入口。

本演示里面。fm.douban.app.AppAplication 类是启动类。而 Spring Boot 框架就会默认 扫描 fm.douban.app 包（启动类所在的包）及其所有子包（fm.douban.app.*）

但是 fm.douban.service; fm.douban.service.impl 不是 fm.douban.app 的子包，所以不会自动扫描，也不会自动实例化 Bean 自然不会实例化 SongListImpl 

## 解决方式：

当然 Spring 架构有机制解决这个问题。

为**启动类**的**注释** @SpringBootAppAplication 添加参数， 告知系统需要额外扫描的包

```java
@SpringBootAppApplication(scanBasePackages={"fm.douban.app","fm.douban.service")
public calss AppAplication{
    public static void main(STring args[]){
        SpringApplication.run(AppApplication.class, args);
    }
}
```

* 参数名是：scanBasePackage;

* 参数值是一个字符串数组，用于指定多个需要额外扫描的包，需要把所有的待扫描的包的前缀都写入。

## 另外一种写法：

如果不是 Spring Boot 启动类，可以使用独立的注解， @ComponentScan ，作用也是一样的。用于指定多个需要额外自动扫描的包。

```java
@ComponentScan({"fm.douban.app","fm.douban.service")
public ......
```

## 额外知识：

演示过程中，为了简单起见。本节课的 control 类不是 @Controller, 而是 @RestController

使用 @RestController 类，所有的方法都不会渲染 Thymeleaf 模板，而是以数据的形式返回。等同于 @Controller 的类方法上面加上 @ResponseBody 注解

但是两个包是不一样的

## Spring Boot Logger:

在上节课的代码，SongListControl 实现了一个初始化的方法 init() ，加上 @PostConstruct 注解，在初始化后检查依赖的 SubjectService 是否成果注入。

> @PostConstruct 注解 简单来说 就是在对象创建 + 依赖注入完成后自动执行初始化方法

```java
@PostConstruct
public void init(){
    System.out.println("SongListControl 启动了");
    if(subjectService != null) {
        System.out.println("subjectService 实例化注入成功");   
    } else {
        System.out.println("subjectService 实例化注入失败");
    }
}
```

但是实际上，系统启动成功后， console 并未看见成功提示，为什么？

因为在 Spring 这种比较复杂的系统里面，System.out.println() 打印的内容会输出到什么地方，是不确定的。所以企业级的项目里面，都是用日志系统来记录信息。

## 日志系统的优势：

1. 日志系统可以轻松的控制日志是否输出.例如想淘宝这样的大型的网址。在开发阶段需要打印的调试信息，但是发布到正式环境就不能打印了，因为每天大量的访问量。大量的调试信息导致磁盘撑爆。这时候需要掌握日志的输出。而不是修改代码

2. 日志系统可以灵活的配置日志的细节，例如输出个格式，通常在日志输出时。需要自动附带日志发生时间，打印日志的类名等信息，这样方便观察日志分析问题。

## 使用日志：

### 1.配置：

修改 Spring Boot 系统的标注配置文件： application.properties （在项目的 src/main/resouces/目录下），增加日志级别配置：

```java
logging.level.root=info
```

表示所有日志（root）都为（info）级别。

我们也可以为不同的包定义不同的级别，例如：

```java
logging.level.fm.douban.app=info
```

就表示 fm.douban.app 包括其子包中所有的类都输出 info 级别的日志。

常用的日志优先级：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-17-23-24-55-image.png)

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-17-23-25-17-image.png)

## 编码：

配置完成后，编码很简单，需要实例化日志对象就可以打印了。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RestController;
import javax.annotation.PostConstruct;

@RestController
public class SongListControl {
    private static final Logger LOG = LoggerFactory.getLogger(SongListControl.class);

    @PostConstruct
    public void init(){
        LOG.info("SongListControl init");
    }
}
```

先定义一个类 LOG ，然后在  LOG.info() 方法的参数中输入日志内容

主要这个方法名（info()）与日志级别一一对应：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-17-23-51-01-image.png)

如果想要输出警告信息就调用 LOG.warm()方法

## 日志按级别输出：

配置为 logging.level.root=error 时 warm() , info() ,debug() 三个方法是无效的，都不会在 Console 打印日志内容

当修改为 logging.level.warm() 时，warm() 会变的有效，也可以打印日志内容了

这样，就可以通过修改 **一个配置**，并不修改每行日志打印代码，即可方便的调节日志输出内容。

## 代码演示:

定义类变量 LOG 的语句属于固定写法，只需要在不同的类中修改 getLogger() 方法的参数为当前的类名即可，这样就能换识别每段日志的来源。

> 修饰为 static final 的目的就是尽量复用，一个类无论多少个实例，只需要一个日志对象，日志对象一旦定义也不需要修改

## Spring Boot Properties:

在前面我们使用 https://start.spring.io/ 创建Spring Boot 的工程。创建完毕即运行，这是因为 Spring Boot 框架中已经免除了大部分手动配置

但是对于一些特定的情况，还是需要我们进行手动或配置，框架为我们提供了 application.properties 配置文件，让我们自定义配置，来对默认的配置进行修改

我们已经使用过了 application.properties 配置文件，这是一个固定位置，在项目的 src/main/resources/ 目录下。固定名字的文件。框架会自加载并解析这个配置文件。

## 配置文件格式：

application.properties 配置文件的格式也很简单。每一行是一条配置。配置项名称 = 配置项值。

```properties
logging.level.root=info
logging.level.fm.douban.app=info
```

> 注意等行两边不能加空格

为了方便维护，书写配置文件时遵循如下约定：

* 配置项目名称能准确表达作用，含义，以点   .   分割单词，

* 相同前缀的配置项不能写在一起

* 不同前缀的配置项之间要空一格

## 配置的意义：

配置的主要作用是把可变的内容从代码中剥离出来，做到不修改代码的情况下，方便的修改这些可变的或常变的内容。这个过程称为避免硬编码，做到解耦。

## 自定义配置：

我们可以在 application.properties 配置文件中加入自定义的配置项。

```properties
song.name=God is a girl
```

框架会自动加载并自动解析整个文件。

代码是怎么使用自定义配置项的？

```java
import org.springframework.beans.factory.annotation.Value;

public class SongListControl {
    @Value("${song.name})
    private String songName;
}
```

只需要 @Value 注释即可，注意写法，花括号中的配置项名称，与配置文件中的保持一致。

项目启动的时候， Spring 系统会自动把 application.properties 配置文件中 song.name 的值，复制给 SongListControl 对象实例的 songName 变量。

## Cookie:

Cookie 是网络编程中使用最广泛的一项技术。主要用于身份辨别。客户端（浏览器）与网站服务端通信的过程：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-20-12-43-11-image.png)

从图中看，服务端即需要返回cookie给客户端，也要读取客户端提交的 Cookie 的。本接就学习服务端 Spring 工程如何使用 cookie 的，有读和写两种操作。

## 读 Cookie：

为 Control 类的方法添加一个 HttpServletRequest 参数通过 request.getCoookies() 取得 cookie 数组。然后再循环遍历数组即可。

> 系统会自动传入方法参数所需要的 HttpServletRequest 对象

（省略循环）

```java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;


@RequestMapping("/songlist")
public Map index(HttpServletRequest request){
    Map returnData = new HashMap();
    returnData.put("result", "this is song list");
    returnDate.put("author", songAuthor);

    Cookie[] cookie = request.getCookies();
    returnData.put("cookies", cookie);

    return returnData;
}
```

cookie相关知识链接还有。其他知识</a>

看完之后我们可以知道当前的代码可能会造成 cookie 值读取后序列化失败，我们优化一下代码。

```java
@RestController
public class SongListController {

    @Value("${song.author}");
    private String songAuthor;

    @RequestMapping("/songlist")
    public Map<String, Object> index(HttpServletRequest request){
        Map<String, Object> returnData = new HashMap<>();
        returnData.put("result", "This is song list");
        returnData.put("author", songAuthor);

        Cookie cookies = request.getCookies();
        List<Map<String, String>> cookieList = new ArrayList<>();
        if(cookies != null && cookies.length > 0){
            for(Cookie cookie : cookies){
                Map<String, String> cookieMap = new HashMap<>();
                cookieMap.put("name", cookie.getNmae());
                cookieMap.put("value", cookie.getValue());
                cookieMap.put("domain", cookie.getDomain()); // Cookie 的 domain（对应你之前问的知识点）
                cookieMap.put("path", cookie.getPath()); // Cookie 的路径
                cookieMap.put("maxAge", String.valueOf(cookie.getMaxAge())); // 过期时间（秒）
                cookieMap.put("secure", String.valueOf(cookie.getSecure())); // 是否仅 HTTPS 传输
                cookieMap.put("httpOnly", String.valueOf(cookie.isHttpOnly())); // 是否防 XSS
                cookieList.add(cookieMap);
            }
        }
        returnData.put("cookies", cookiesList);
        return returnData;
    }
}
```

## 使用注解读取 cookie:

如果知道 cookie 的名字，就可以通过注解的方式读取，不需要再遍历 cookie 的数组了，更加方便。

为 control 类的方法增加一个 @CookieValue("xxx")  String xxx 参数即可，注意使用时要填入正确的 cookie 值的 cookie 名字

> 系统会自动解析并传入同名的 cookie

```java
import org.springframework.web.bind.annotation.CookieValue;

@RequestMapping("/songlist")
public Map index(@CookieValue("JSESSIONID") String jSessionId) {
    Map returnData = new HashMap();
    returnData.put("result", "this is song list");
    returnData.put("author", songAuthor);
    returnData.put("JSESSIONID", jSessionId);

    return retrunData;
}
```

> 注意：如果系统解析不到指定名字的 cookie，使用此注解就会报错。必须谨慎使用

## 写 Cookie

同样，我们为 control 类的方法添加一个 HttpServletResponse 参数，调用 response.addCookie() 方法添加 Cookie 实例对象即可。

> 系统会自动传入方法参数所需要的 HttpServletRespones 对象

```java
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRespones;

@RequestMapping("/songlist")
public Map index(HttpServletRespones respones){
    Map returnData = new HashMap<>();
    returnData.put("result", "this is song list");
    returnData.put("name", songName);

    Cookie cookie = new Cookie("sessionId", "CookieTestInfo");
    //设置的是 cookie 的域名，我是在哪个域名下面生成的 cookie 值
    cookie.setDomain("youkead.com");
    //是 cookie 的路径，一般是写到 / ，不会写到其他路径
    cookie.setPath("/");
    //设置cookie 的最大存活时间。 -1 代表浏览器有效时间，也就是浏览器关闭掉，这个cookie 就没了
    cookie.setMaxAge(-1);
    //设置是否只有服务端修改，浏览器不能修改，
    cookie.setHttpOnly(false);
    response.addCookie(cookie);

    returnData.put("message", "add cookie successful");
    return returnData;
} 
```

# Spring Session API

上节我们知道了 Cookie 放在客户端，可以存储用户登录信息，主要用于辨别用户身份。

如果真的要把用户 ID ，登录状态放到 cookie 里面，会带来安全隐患，因为网络上面很不安全，cookie 可以进行拦截和伪造，

采用 Session 会话机制可以解决这个问题。**用户ID，登录状态** 等重要信息不纯放在客户端，而是存放在服务器里面。从而避免了安全隐患。通讯过程如下：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-24-19-29-12-image.png)

使用会话机制时，Cookie 作为 session id 的载体与客户端通信。上一节的演示代码里面，cookie 中的 JSESSIONID 就是这个作用

> 名字为 JSESSIONID 的 cookie ，是专门用来记录用户 session 的 JSESSIONID 是标准的，通用的名字

在了解 Session 与 Cookie 的关系之后，我们来学习如何使用 Session，页分为读写两操作

## 读操作：

与 cookie 相似，从 HttpServletRequest 对象中获取 HttpSession 对象，使用的语句是 request.getSession().

但是不同的是， 返回结果不是数组，是对象。在 attribute属性中用 key -> value 的形式存储多个数据。

假设存贮登录的数据 key 是 userLoginInfo ，那么语句就是 session,getAttribute("userLoginInfo").

## 登录信息类：

登录信息实例对象因为要在网络上传输，就必须实现序列化接口 Serializable ，否则不实现的话就会报错。

登录信息类需要根据具体的需要设计属性字段。下列代码的两个属性仅供演示。

```java
import java.io.Serializable;


public class UserLoginInfo implements Serializable {
    private String userId;
    private String userName;
}
```

## 操作代码：

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSesion;

@RequestMapping("/songlist")
public Map index(HttpServletRequest request, HttpServletResponse response){
    Map returnData = new HashMap();
    returnData.put("result", "this is song list");
    // 取得 HttpSession 对象
    HttpSession session = request.getSession();
    // 读取登录信息
    UserLoginInfo userLoginInfo = (UserLoginInfo)session.getAttribute("userLoginInfo");
    if (userLoginInfo == null){
        returnData.put("loginInfo", "not login");
    }else {
    // 已登录
    returnData.put("loginInfo", "already login");
    }

    return returnData;
}
```

这里实际上读取不到数据，因为还没有写入

## 写操作：

假设登录成功，怎么记录登录信息到 Session 呢？

既然从 HttpSession 对象中读取登录信息用的是 getAttribute() 方法， 那么写入登录信息就用 setAttribute（） 方法。

下列代码演示使用 Session 完成登录过程，略去了校验用户名和密码。

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;


@RequesyMapping("/loginmock")  // mock 模仿，嘲笑

public Map loginMock(HttpServletRequest request, HttpServletResponse response ){
    Map returnData = new HashMap();

    // 假设对用户名和密码验证成功
    // 仅演示登录信息对象
    UserLonginInfo userLoginInfo = new UserLoginInfo();
    userLoginInfo.setUserId("123456789");
    userLoginInfo.setUserName("ss");
    //取得 HttpSession 
    HttpSession session = request.getSession();
    // 写入登录信息
    session.setAttribute("userLoginInfo", userLoginInfo);
    returnData.put("message", "login successful");

    return returnData;
}
```

### 模拟验证登录:

```java
@RestController
public class TestLogin {
    private Static final Logger LOG = LoggerFactory.getLogger(TestLogin.class);
    private static final String SESSION_LOGIN_KEY = "IS_LOGIN";

    @Value("${user.name}")
    private String userName;
    @Value("${user.password}")
    private String userPassword;

    @PostConstruct
    public void init() {
        LOG.info("UserName: {}, Password: {}", userName, userPassword);
    }

    @GetMapping("/login")
    public Map loginMock(@RequestParam("userNmae") String Name, @RequestParam("password") String Password
    , HttpServletRequest request, HttpServletResponse response){
        Map<String, Object> responseData = new HashMap<>();
        HttpSession session = request.getSession();
        if (Name.equals(userName) && Password.equals(userPassword)) {
            session.setAttribute(SESSION_LOGIN_KEY, true);

            responseData.put("result", true);
            responseData.put("message", "Login successful");
        } else {
            session.setAttribute(SESSION_LOGIN_KEY, false );

            responseData.put("result", false);
            responseData.put("message", "Login failed");
        }
        return responseData;
    }

    @RequestMapping("/status")
    public Map statusMock(HttpServletRequest request, HttpServletResponse response) {
        Map<String, Object> responseData = new HashMap<>();
        HttpSession session = request.getSession(false);

        Boolean isLogin = (session != null) && (Boolean.TRUE.equals(session/getAttribute(SESSION_LOGIN_KEY)));

        responseData.put("isLogin", isLogin);
        return responseData;
    }
}
```

# Spring Session 配置：

上面我们了解了 Session 的操作，没有涉及 cookie。 系统灰自动把默认的 JSESSIONID 放到cookie里面。

但是 cookie 作为session id 的载体，也可以修改属性。

## 前置知识：配置：

前面我们知道了 application.properties 是 springBoot 的标准配置文件，配置了一些简单的属性。同时， Spring Boot 页提供了编程式的配置方式，主要用于配置 Bean

### 基本格式：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringHttpSessionConfig {
    @Bean
    public TestBean testBean() {
        return new TestBean();
    }
}
```

在类上添加 @Configurarion 注解，就表示这个是配置类，系统灰自动扫描并且处理。

在方法上面添加 @Bean 注解，表示把此方法返回的对象实例注册成为 Bean

>  跟 @Service 等注解在类上面注解一样。都表示注册 Bean

## Session 配置：

### 依赖库：

先在 pom.xml 文件中添加依赖库：

```java
<!-- spring session 支持 -->
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-core</artifactId>
</dependency>
```

### 配置类：

在类上额外添加一个注解：@EnableSpringHttpSession, 开启 session。然后注解两个 Bean 

* CookieSerializer: 读取 Cookie 中的 Sessionid 信息

* MapSessionRepository: Session 信息在服务器上的存储仓库

```java
import org.springframework.session.MapSessionRepository;
import org.springframework.session.config.annocation.web.http.EnableSpringHttpSession;
import org.springframework.session.web.http.CookieSerializer;
import org.springframework.session.web.http.Default.CookieSerializer;


import java.util.concurrent.ConcurrentHashMap;

@Configuration
@EnableSpringHttpSession
public class SpringHttpSessionConfig {
    @Bean
    public CookieSerializer cookieSerializer() {
        DefaultCookieSerializer serializer = new DefaultCookieSerializer();
        serializer.setCookieName("JSESSIONID");
// 用正则表达式配置匹配的域名，可以兼容 localhost、127.0.0.1 等各种场景
        serializer.setDomainNamePattern("^.+?\\.(\\w+\\.[a-z]+)$");
        serializer.setCookiePath("/");
        // 声名周期
        serializer.setCookieMaxAge(24* 60 * 60);
        return serilizer;
    }

    @Bean
    public MapSessionRepository sessionRepository() {
        return new MapSessionRepository(new ConcurrentHashMap<>());
    }
}
```

# Spring Request 拦截器：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-11-30-19-22-46-image.png)

拦截器必须实现 HandlerInterceptor 接口。可以在三个点进行拦截。

* Contorller 执行方法之前。这个是最常见的拦截点。例如是否登录的验证就要在 preHandle() 方法中处理

* Controller 方法执行之后。例如记录日志，统计方法执行时间等。就要在 postHandle() 方法中实现

* 整个请求完成后，不常用的拦截点。例如统计整个请求的执行时间的时候用，比如在 afterCompletion 方法中处理

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandleInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class InterceptorDemo implements HandleInterceptor {
    @override 
    public boolean preHandle (HttpServletResponse response, HttpServletRequest request
        , Object handler) throws Exception{
        return true;
    }

    @Override 
    public void postHandle (HttpServletRequest request, HttpServletResponse response, Object handler
           ModelAndView modelAndView) throws Exception{

    }
    @Override 
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response
            HttpServletRequest request, Object handler, Exceptionex) throws Exception {

    }
}
```

preHandle() 方法的参数中有 HttpServletRequest 和 HttpServletResponse, 可以像 control 中一样使用 Session

## 二, 实现 WebMvcConfigurer

创建一个类实现 WebMvcConfigurer, 并且实现 addInterceptors() 方法。这个步骤用于**管理**拦截器。

> 注意：实现类要加上 @Configurer, 并实现 addInterceptors( ) 方法。让框架能够自动扫描并处理

管理拦截器，比较重要的是为拦截器设置的拦截范围。常用 addPathPatterns("//**")表示拦截所有 URL。

当然也可以调用 excludePatterns( ) 方法排除某些 URL， 例如登录页本身就不需要登录。需要排除。

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebAppConfigurerDemo implements WebMvcConfigurer {
    @Override
    public void addIterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserInterceptor()).addPathPatterns("/**");
    }
}
```

通常的拦截器，会放到一个包里面（例如 interceptor）而用于管理拦截器的配置类，会放到另外一个包里面。（config）

# 用户登录 demo:

## POST 请求：

前面的例子都是针对 GET 请求时的情况。 从服务端获取数据，是 GET 请求。向服务端提交或者写入数据， 是 POST 请求

例如：注册，登录，都是向服务端写入数据（用户名，密码），由读物端完成验证，存储数据等操作，所以是 POST 请求。

在 control 中使用 @PostMapping 注解把方法定义为 Post 请求：

```java
import org.springframework.web.bind.annotation.PostMapping;

@PostMapping(Path = "/authenticate")
public String loginAction(@RequestParam String name, @RequestParam String
    password) {
    return user.toString();  
}
```

```html
<form action="" method="post">
```

@RequestMapping 注解放在 control 上面，表示整个类的所有方法统一加上一个前缀。

## 页面跳转：

使用拦截器判断当前登录状态时，需要实现

* 如果已经登录，则继续实现

* 如果没有登录，需要强制跳转到登录页面

跳转操作：

```java
String loginPageUrl = "/login";
response.sedRedirect(loginPageUrl);
```

# 使用 MongoDB：

MongoDB 是基于分布式文件存储的数据库，特点是可扩展，高性能，为 WEB 应用提供支持

## 基本概念：

MongoDB 是一个软件，在具体使用前，还需要创建一个 “库”， 库中存放具体的数据表。

>  一个软件中可以创建多个 “库” ，用来隔离和区分不同的范围，不同的作用域是数据
> 
> > 例如多个学校共用一个数据库软件，那么每个学校都有自己的“库”
> > 
> > 每个学校的数据里面有相同的学生表，那么不同的学生就隔离了，避免混乱

## 创建数据库：

当你第一次向一个不存在的数据库集合写入数据的时候， MongoDB 会自动创建这个数据库。

同时可以使用命令创建：

### 手动创建数据库：

进入 Mongo shell ：

在终端：

```shell
mongo
```

输入创建命令：

```shell
use practice
```

回车创建了一个名为 ： practice 的数据库

> 看见 “switched to db practice 说明创建成功

退出 Mongo Shell

```shell
exit
```

即可退回终端

# Spring Data Mongo DB 配置：

在开启Mongo DB 服务后 我们可以在 Spring boot中创建 Mongo DB 服务：

## pom.xml 依赖：

为了支持 Mongo DB 我们需要增加一个选项：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-12-11-19-29-55-image.png)

或者手动创建：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

## 项目配置：

修改 src/main/resources/applicatin.properties 文件，增添配置项：

```properties
# mongodb 服务器的 IP
spring.data.mongodb.host=xxxx
# mongo DB 服务端口号
spring.data.mongodb.port=xxxx
# 创建的数据库即用户和密码
spring.data.mongodb.database=priactice
```

## Spring Data CRUD:

对数据库的操作一定要放在 @Servie 类中，而不是放到 @Controller 类中； 且 @Controller 类可以调用 @Service 类的方法，反之则不行。。这是 SpringMVC 的经典架构思想

* @Service 类主要是用于不变的核心业务逻辑。

* @Controller 类与前端页面紧密配合，调用  @Service 服务读写数据，从而响应前端请求

### 新增数据：

新增数据就是向数据库中插入一条数据。在 java 中，所有东西都是对象，所以，所谓的数据就是对象。

```java
import org.springframework.data.core.MongoTemplate;


    @Autowird
    private MongoTemplate mongoTemplate;

    private void test() {

        Song song = new Song();
        song.setSubjectId("s001");
        song.setLyrics("...");
        song.setName("成都");

        mongoTemplate.insert(song);    
}
```

我们使用 @Autowired 可以让系统自动注入 MongoTemplate 的实例。

只需要调用 名哦Template.insert() 方法就可以把对象存放到数据库里面。

```bash
{"songResult":{"id":"5e55db3a461b9b3c3e1d6a56",
"name":"成都","lyrics":"...","subjectId":"s001"}}
```

## 查询语句：

```bash
mongoTemplat.findById(songId, Song.class)
```

注意：findById() 方法第一个参数是主键 id ，第二个参数是具体类，写法是 类名   .class

## 修改数据：

修改数据的语句会复杂点，因为修改的操作包括连个部分

* 修改的那条数据

* 哪个字段修改成什么值

所以，修改操作也分为修改条件和修改字段。

```bash
// 修改数据 id = 1
Query query = new Query(Criteria.where("id").is("1"));
// 把歌名修改为 new name 
Update updateData = new Update();
updateData.set("name", "new name");
// 执行修改后，修改的返回结果是一个对象
UpdateResult result = mongoTemplate.updateFirst(query, updateData, Song.class);
// 修改的记录数大于 0， 表示修改成功
Syste.out.println(result.getModifiledCount());
```

先使用条件对象 Criteria 构建条件对象 Query 实例，然后再调用修改对象方法 Update的方法 .set（） 设置参数具体是哪个字段。

最后调用：

mongoTemplate.updateFirst(query, updateData, Song.class) 方法完成修改；第 3 个参数是具体的类。

本修改数据的演示中，使用了**约定**：主键不能修改，；且其他字段值设置为 NULL 表示不修改，值为长度为 0 的字符串 “” 表示清空该字段。

我们就可以使用一个 Song 对象区分哪些字段需要修改，哪些字段不需要修改，哪些字段需要清除值“等多种情况” 

## 删除数据：

删除数据只需要精确确定需要删除什么数据即可

调用 mongoTeplate.remove() 方法即可删除数据，参数是对象，表示需要删除哪些数据。

```java
Song song = new Song();
song.setId(songId);

DeleteResoult result = mongoTemplate.remove(song);
//删除记录大于 0 说明删除成功
System.out.println(result.getDeleteConunt());
```

创建一个对象并且设置好属性，作为删除条件，符合体哦见的数据都将被删除，可以设置更多的属性来提高精准度，但是通过主键来删除数据，是保证不误删的一个比较好的方式。

# Spring Data Query

对于查询数据，仅仅只根据主键是远远不够的，通常情况下需要根据多条件查询。

条件查询相对复杂，但是功能更加强大，

```java
List<Song> songs = mongoTemplate.find(query, Song.class);
```

因为可能查询多条数据，所以返回的结果是对象集合，第一个参数是查询对象 Query 实例；第二个参数就是查询什么样子的对象，写法是 类名.class ；

查询方法比较简单，但是查询操作的复杂性在于条件。需要构建好的 Criteria 条件对象的实例，来构建 Query 对象。

```java
Query query = new Query(criteria);
```

而构建 Criteria 条件对象，一般有两种情况。

* 单一条件，使用：
  
  ```java
  Criteria criteria = Criteria.where("条件字段名").is("条件值")
  ```
- 即可返回一个条件对象的实例。

- 组合条件，根据或（or）、且（and）的关系进行组合，多个**子条件**对象组合成一个**总条件**对象：
  
  - 或（or）关系：
    
    ```java
    Criteria criteria = new Criteria();
    criteria.orOperator(criteria1, criteria2);
    ```
  
  - 且（and）关系：
    
    ```java
    Criteria criteria = new Criteria();
    criteria.andOperator(criteria1, criteria2);
    ```
  
  - `orOperator()`和`andOperator()`的参数，都可以输入多个子条件，也可以输入子条件数组

当然，组合条件情况下，也可以多层组合，子条件也可以是组合而来的。

例如根据歌曲的专辑查询，并限定最多返回 **10** 条数据：

```java
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Criteria;

public List<Song> list(Song songParam) {
    // 总条件
    Criteria criteria = new Criteria();
    // 可能有多个子条件
    List<Criteria> subCris = new ArrayList();
    if (StringUtils.hasText(songParam.getName())) {
      subCris.add(Criteria.where("name").is(songParam.getName()));
    }

    if (StringUtils.hasText(songParam.getLyrics())) {
      subCris.add(Criteria.where("lyrics").is(songParam.getLyrics()));
    }

    if (StringUtils.hasText(songParam.getSubjectId())) {
      subCris.add(Criteria.where("subjectId").is(songParam.getSubjectId()));
    }

    // 必须至少有一个查询条件
    if (subCris.isEmpty()) {
      LOG.error("input song query param is not correct.");
      return null;
    }

    // 三个子条件以 and 关键词连接成总条件对象，相当于 name='' and lyrics='' and subjectId=''
    criteria.andOperator(subCris.toArray(new Criteria[]{}));

    // 条件对象构建查询对象
    Query query = new Query(criteria);
    // 仅演示：由于很多同学都在运行演示程序，所以需要限定输出，以免查询数据量太大
    query.limit(10);
    List<Song> songs = mongoTemplate.find(query, Song.class);

    return songs;
}
```

> 这里用 Song 对象来表示查询条件。作为服务接口，使用对象做参数，具备比较好的扩展性和兼容性。例如，如果增加一个查询条件，就不需要增加方法参数，只需要为参数对象增加属性即可；否则所有的调用查询接口方法的代码都需要做修改，影响面可能很大，扩展性和兼容性都不好。

请看下列代码演示：

代码演示

通常需求越复杂，组合条件就可能越复杂。大家需要仔细琢磨一下 `Criteria` 条件对象的运用，达到灵活运用的程度后，就可以根据需求任意组合，以满足多变的查询需求哦。

# Spring Data 分页

   分页是查询中最常用的功能，同时也为了防止一次查询的数据量太大而影响性能。

查询支持分页也比较简单，只需要调用 `PageRequest.of()` 方法构建一个分页对象，然后注入到查询对象即可。

`PageRequest.of()` 方法第一个参数是页码，注意从 `0` 开始计数，**第一页**的值是 `0` ；第二个参数是每页的数量。

```java
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;

Pageable pageable = PageRequest.of(0 , 20);
query.with(pageable);
```

对于分页来说，除了要查询结果以外，还需要查询总数，才能进一步计算出总共多少页，实现完整的分页功能。所以，还需要两个步骤：

- 调用 `count(query, XXX.class)` 方法查询总数。第一个参数是查询条件，第二个参数表示查询什么样的对象；
- 根据结果、分页条件、总数三个数据，构建分页器对象。

```java
import org.springframework.data.domain.Page;
import org.springframework.data.repository.support.PageableExecutionUtils;

// 总数
long count = mongoTemplate.count(query, Song.class);
// 构建分页器
Page<Song> pageResult = PageableExecutionUtils.getPage(songs, pageable, new LongSupplier() {
  @Override
  public long getAsLong() {
    return count;
  }
});
```

`PageableExecutionUtils.getPage()` 方法第一个参数是查询结果；第二个参数是分页条件对象；第三个参数稍微复杂一点，实现一个 `LongSupplier` 接口的匿名类，在匿名类的 `getAsLong()` 方法中返回结果总数。方法返回值是一个 `Page` 分页器对象，使用起来非常方便。

请看下列代码演示：

代码演示

这里做了一次重构，`SongService` 中的 `list()` 方法，用自定义的专用的参数类 `SongQueryParam` 替换原来的 `Song` 做方法参数。因为分页参数不属于歌曲模型，所以必须重构。

当然，为了分页，返回值也重构为 `Page<Song>` ，表示方法返回分页结果对象。

> 实际上，在项目设计的时候就必须考虑到分页功能。本课程只是为了演示方便，在前面的课程中就没有用参数类。

control 中调用 `SongService.list()` 方法得分页结果对象后，可以调用 `Page` 的各个方法取得数据集和前端分页器的各种数据值。详情请[点此查看文档](https://blog.csdn.net/u014236541/article/details/50983400)。

最常用的方法是 *getContent()* 取得本页的数据集：

```java
Page<Song> songResult = songService.list(queryParam);
List<Song> songs = songResult.getContent();
```

因为 *Page* 表示本页的数据对象是 *Song* ，所以 *songResult.getContent()* 返回一个列表，同样，泛型指定了对象也是 *Song* 。

`Page` 其它的方法是取得当前页码号、总页码数等，就不赘述了，对照上述文档的注释看即可。只是注释为英文，可能不习惯，需要一些耐心，配合有道词典等翻译软件理解。 **重要的是** ，自己动手编码看一下各个方法的返回值，对照文档见就更容易理解了。

# 购买云服务器安装 MongoDB

如果大家购买了云服务器，在使用之前，首先要安装 MongoDB 这个数据库软件，那么怎么安装呢？

答案是：使用 `Docker` 安装 MongoDB 软件。

`Docker` 是近年来非常流行的虚拟化技术，可以快速部署软件，这节课就不细讲了。本课程用 `Docker` 来安装 `MongoDB`，自动化程度较高、非常简便。

一般云服务器都是linux操作系统，不像windows有桌面，都是在终端中输入命令的方式操作软件。


购买云服务器的时候注意购买 **预先安装** 了 Docker 软件的服务器。

如果还 **没有** 购买云服务器，本节可以跳过，等需要的时候再看本节的内容也不晚。

## 一、安装 MongoDB 并启动

先安装 MongoDB 软件：

```shell
sudo docker run -itd --name mongo -p 27017:27017 registry.cn-hangzhou.aliyuncs.com/xxx_project/mongo:7.0.12
```

> 这里的 docker 命令就是 Docker 软件提供的操作命令

27017 是 MongoDB 服务的 **标准** 端口号。

> 输入命令回车即执行

## 二、验证是否启动成功

使用下列命令，登录进入 MongoDB 软件内，能登入就说明安装、启动成功了。

```shell
sudo docker exec -it mongo mongosh admin
```

会看到以下的界面：

![](C:\Users\15956\AppData\Roaming\marktext\images\2025-12-18-08-39-07-image.png)

就表示启动成功了。此时，光标停留在 `>` 后面，表示可以输入**数据库操作命令**了。

> 随着 MongoDB 软件不断升级版本，显示内容稍微有点出入，但不要紧

### 界面交互格式说明

当看到光标停留在 `>` 后面，才能输入数据库的管理命令，大多以 `db.` 开头。而回车后系统返回的信息，前面就没有箭头了。

## 三、创建数据库

MongoDB 是一个软件，要使用数据库，还必须先创建具体的数据库。

创建一个 `practice` 数据库，用于后继的学习。

```shell
use practice
```

> 这个命令也是在光标停留在 `>` 后面时输入的哦

这个命令同时完成创建和切换，看到系统输出 `switched to db practice` 就表示成功了。

## 四、退出登录

在 MongoDB 数据库**登录**状态下执行命令，可以退出 MongoDB ，回到终端。

```shell
exit
```

# 云服务器安全设置

## 如果有的同学已经学会购买云服务器，可以在云服务器上启动 Mongodb 。

此时需要检查一下云服务器的安全设置，必须开放 `27017` 端口。有的云服务商由于安全起见，默认关闭了 `27017` 端口。


### 请看演示视频：



