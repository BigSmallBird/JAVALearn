# SSM 框架 
## MyBatis 简介
当下 MyBatis 是主流的持久层框架，具体一点就是操作数据库的框架；MyBatis 非常轻量，只需要简单的 XML 或者注解就可以完成数据映射和操作数据。  
### ORM介绍
ORM 对象映射关系，是一种程序设计技术用于实现面向对象编程语言里不同类型系统的数据之间的转换  
ORM 把数据库映射成为对象：
```java
数据库的表（table） --> 类（class）
记录（record,行数据） --> 对象（object）
字段（field）--> 对象属性（attribute）
```
有了 ORM 能力，就很容易把 SQL 对象转换为 java 对象了，自定义 SQL 的能力很重要，工程师需要更加精细的掌握 SQL ，优化每一个 SQL 语句。  
在 Java 领域里面所有的数据框架都是基于 JDBC 的，这个是底层；
### 集成 MyBatis 
#### 注意事项：
添加依赖：
1. Spring Web
2. MyBatis Framework
3. MySQL Driver

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

#### 配置 MySQL 连接  
配置好 MyBatis 环境后，大概率会出现这样的报错：
```bash
***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
  If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
  If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).

```
错误原因：  
因为没有填写正确的数据源（datasource）配置，SpringBoot 项目启动的时候不知道连接什么数据库。  

#### 配置数据源
必须配置数据源，项目才知道需要连接那个数据库  
##### 基本格式：
```properties
spring.datasource.url=jdbc:mysql://mysql数据库地址:数据库端口/数据库名称?serverTimezone=GMT%2B8
spring.datasource.username=用户名
spring.datasource.password=密码
```
> 参数 serverTimezone=GMT%2BB 这个参数表示设置数据库的时区  

##### 配置前的准备  
创建 comment 的数据库：  
```sql
mysql -e 'CREATE DATABASE comment;'
```

## 组件概要设计  
### 概要设计
从软件工程设计角度来说，我们希望所有产品先做出模型设计，然后再考虑数据库设计。  
对于一个评论系统来说，分析我们可以发现，这里有三个功能点：  
* 用户登录
* 写评论 
* 回复评论   
现在我们可以开始抽象对象，设计领域模型，大致能够得出需要一个**用户模型**和**评论模型**这两个模型  
> 这里有一个难点，就是**回复评论**这个动作，我们需要设计一个 Tree 结构才能满足数据存储。当多个人回复同一条评论 A 的时候，我们可以把回复评论当作是评论 A 的子，这样就可以形成 Tree 结构了


现在我们设计一下领域模型：  
Comment(id:long; //关联内容的id refid:String; //作者 author:User; //内容 content:String; //子评论 childern:List<Comment>; gmtCreated:LocalDateTime; getModified:LocalDateTime;)   
User(id:long;  //用户名 userName:String; //密码 pwd:String; //昵称 nickName:String; //头像 avatar:String; gmtCreated:LocalDateTime; gmtModified:LocalDtaeTime)  


由于评论组件可以运用在很多地方，比如文章，笔记， 博客等等，所以我们需要关联到某一个主键上，也就是这里的 refId  

在 Comment 模型里面，我们添加了一个 children 字段并且设定类型为 List<Comment>, 这个字段存储所有该评论被回复的数据。  

通过这个领域设计，我们可以确定这个可以满足产品的需求，南无我们可以开始设计表结构
> 模型是为了产品而设计的，但是表射界并不是面向对象的设计思路，所以直接设计表可能变成不满足需求了  

### 数据库设计  
通过模型我们来设计一下表结构，为了满足上述的模型，我们得需要设计两个表：User， Comment    
![user - comment](SSM/img/1.png)   

对于上面的关系:  
* ref_id 一般字符串的主键或者外键，我们都是设置 varchar(32), 这是一种约定  
* user_id 
* 我们在 Comment.java 模型里关联了 User 对象，但是在数据库表中，我们只需要做一个主键字段关联就可以了。  
* 一般来说 user_id 这种写法就说明关联的是 user 表的主键，这是一种约定  
* parent_id  
* 我们在 Comment.java 模型里面设计了 children 字段来包含所有的子评论，但是在数据库表中，我们只需要做一个父主键就可以了，因为实际查找数据的时候，是通过父找子的  

### User 表 
| 表字段      | 字段类型     | 允许 null | 字段描述              |
|-------------|-------------|-----------|-----------------------|
| id          | bigint      | 不允许    | 自增主键              |
| pwd         | varchar(32) | 不允许    | 密码                  |
| nick_name   | varchar(20) | 不允许    | 用户名，20位长度够了  |
| avater      | varchar(200)| 允许      | 头像url               |
| gmt_created | datetime    | 不允许    | 创建时间              |
| gmt_modified| datetime    | 不允许    | 修改时间              |


### Comment 表格  
| 字段名        | 字段类型      | 允许 NULL | 字段描述               |
| :----------- | :------------| :-------- | :--------------------- |
| id           | bigint       | 否        | 自增主键               |
| ref_id       | varchar(32)  | 否        | 关联外部内容的主键      |
| user_id      | bigint       | 否        | 关联用户表主键          |
| content      | varchar(1000)| 否        | 评论内容（最长1000字）  |
| parent_id    | bigint       | 是        | 父评论ID               |
| gmt_created  | datetime     | 否        | 创建时间                |
| gmt_modified | datetime     | 否        | 修改时间                |

## MyBatis 映射对象
我们现在来试试完成 MyBatis 工程配置

### DO 对象规则  
所有的 ORM 框架都需要有一个 Java 对象来映射数据库的表，并且是一一对应的，一般我们把这类对象称为 DO 对象，对象名称的规范是 `表名 + DO`   

user 表对应的类对象名就算 UserDO ；comment 表对应的类对象名称就是 CommentDO 
> 如果你的表名称是类似的 xxx_user 这个格式，可以使用 UserDO 或者 XXXUserDO   

### DO 对象包规则  
一般情况下，企业中都会把这个 DO 对象存放到 xxx.xxx.dataobject 包下，所以 DO 就算 dataobject 的缩写。   
 现在我们约定放到 com.xxx.comment 那么对应的 DO 包就算 `com.xxx.comment.dataobject`  

 ### DO 对象数据类型  
 DO 对象和普通的 POJO 类并无不同，唯一要注意的是属性类型要和数据库类型进行匹配，这个是 JDBC 制定的规范，成勇的数据类型映射如下：  

| JDBC 类型（数据库字段类型） | Java 数据类型 |
| :-----------------------: | :-----------: |
| varchar                   | String        |
| text                      | String        |
| integer                   | int           |
| double                    | double        |
| bigint                    | long          |
| datetime                  | Date          |

> 注意日期时间类型对应的一般是 java 的 java.util.Date 对象
### 创建 DO 对象  
现在我们把 user 表，映射为 UserDO 

| 表字段      | 字段类型     | 允许 null | 字段描述              |
|-------------|-------------|-----------|-----------------------|
| id          | bigint      | 不允许    | 自增主键              |
| pwd         | varchar(32) | 不允许    | 密码                  |
| nick_name   | varchar(20) | 不允许    | 用户名，20位长度够了  |
| avater      | varchar(200)| 允许      | 头像url               |
| gmt_created | datetime    | 不允许    | 创建时间              |
| gmt_modified| datetime    | 不允许    | 修改时间              |

**UserDO**
```java
package com.xxx.comment.dataobject;

import java.time.LocalDateTime;

@Data
public class UserDO {
  private long id;
  private String id;
  private String userName;
  private String pwd;
  private String nickName;
  private String avater;
  private LocalDatatime gmtCreated;
  private LocalDateTime gmtModified;
}
```

## MyBatis DAO 
在 java 工程化中，我们一般会把数据层的服务称为 DAO 层，DAO 层包含对数据库操作的接口和实现类。  

MyBatis 好的地方在于只需要**定义接口**就可以完成数据的**增删改查**，接口开发是 java 经常采用的方法。  
### 创建 DAO 层  
以评论组件为例子，我们可以先创建包。`com.xxx.comment.dao`  
### 定义 DAO 接口  
继续以 user 表为例子，我们创建一个 UserDAO 这样的接口  
> 注意需要放到 dao 包里面  
```java
package com.xxx.comment.dao;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserDAO{

}
```
这个接口的特殊之处在于添加了一个 `@Mapper` 注解 `import org.apache.ibatis.annotations.Mapper;`  
### 引用 DAO 
当我们完成 MyBatis DAO 的定义以后， Spring 启动的时候会自动加载这个接口并动态创建一个 Spring Bean，所以我们只需要按照 Spring Bean 的方式完成资源注入  
现在我们创建一个 UserController 用于处理用户的 Web 服务  

```java
package com.xxx.comment.control;

import com.xxx.comment.dao.UserDAO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class UserController{
  @Autowired
  private UserDAO userDAO;
}
```

## MyBatis 查询
我们在之前使用 SQL 语句插入了一条用户记录。如果我们想要查询，user 表的所有数据。我们执行 
```sql
select * from user
```
在 MyBatis 中，最强大的地方是在于和 SQL 语句天然集成的，也就是说我们完成前面几个步骤后，接下来就是补充 SQL 就完事了  
我们打开 UserDAO 这个接口，接下啦就是补充 SQL 。

### 添加接口的方式  

```java
package com.xxx.comment.dao;

import org.apache.ibatis.annotations.Mapper;
import UserDAO;

import java.util.List;

@Mapper
public interface UserDAO {
  public List<UserDO> findAll();
}
```
如上，我们新增一个 `findAll();`  
```java
public List<UserDAO> findAll();
```

查询多条记录的时候，使用 List作为返回类型；如果查询所有记录，方法名称一般定义为 findAll;  

### 添加 @select 注解  
```java
package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper
public interface UserDAO{
  @Select("SELECT 
          id,
          user_name as userName,
          pwd,
          nick_name as nickName,
          avatar,
          gmt_created as gmtCreated,
          gmt_modified as gmtModified
          FROM user")
  List<UserDO> findAll();
}
```


## MyBatis 插入
对于下面的 SQL 语句：
> 由于user.id 是自增主键，所以我们并没有对 ID 主键设定值  

```sql
INSERT INTO user
  (user_name, pwd, nick_name, avater, gmt_created, gmt_modified)
VALUES(
  'admin',
  'admin',
  '管理员',
  'https://url(xxxxx)',
  now(),
  now()
)
```
在 MyBatis 中同样支持插入操作，可以使用 @Insert 注解   
> insert 注解的完整包路径：`org.apache.ibatis.anntations.Insert`   

**我们还是按顺序进行插入**  

### 添加接口方式  
```java
int insert(UserDO UserDO);
```
#### insert 方法介绍
执行 SQL 插入语句的时候，会返回插入行数，一般成功返回 1， 所以我们设置返回类型位 int   
> 如果想判断插入操作是否成功，我们可以使用 `返回值 > 0` 进行判断  

```java

package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;
@Mapper
public interface UserDAO{
  
  int insert(UserDO UserDO);

  @Select("SELECT id, user_name as userName, xxxx FROM {table_name}")
  List<UserDO> findAll();
}
```

#### 添加 @Insert 注解 

```java
package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper
public interface UserDAO {

  @Insert("INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified) VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())")
  int insert(UserDO UserDO;

  @Select("SELECT id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified FROM user")
  List<UserDO> findAll();
}
```
如上，我们改造了 SQL语句，我们把 VALUE 的值改成了 `#{变量名}` 的形式，这个就是 MyBatis 获取动态值的一种方式，执行的时候会自动从上下文参数（这里比如 UserDO）里面获取这个变量的值  
`VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())`  
比如 #{userName} 实际上执行的是 user.getUserName() 这个方法来获取 userName 变量值，MyBatis 会自动更新生产正式的 SQL 语句到数据库里面执行，这样就完成了动态存储。   

#### API 测试  
> 现在使用 controller 进行测试

```java
package com.youkeda.comment.control;

import com.youkeda.comment.dao.UserDAO;
import UserDO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class UserController {

  @Autowired
  private UserDAO UserDAO;

  @GetMapping("/users")
  @ResponseBody
  public List<UserDO> getAll() {
    return userDAO.findAll();
  }

  @PostMapping("/user")
  @ResponseBody
  public UserDO save(@RequestBody UserDO UserDO) {
    userDO.insert(userDO);
    return userDO;
  }
}
```
> 一般数据插入我们都使用 POST 请求，同时使用 JSON 方式传递数据，为了接受 JSON 参数，需要在参数冲添加 @RequestBody 注解

#### 查看主键值  
对于上面的返回子，应该会发现输出的内容并没有刚刚插入主键的值，很多我们是需要知道这个值的。  

我们可以通过 @Insert 注解继续添加一个 @Options 主键，Options 的完整路径是：
```java
import org.apache.ibatis.annotations.Options
```
我们添加入代码：
```java
package com.xxx.comment.dao;
import UserDO;

import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Options;
import org.apache.ibatis.annotations.Select;

import java.util.List;

@Mapper
public interface UserDAO {
  @Insert("INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified) VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())")
  @Options(userGeneratedKeys = true, keyColumn = "id", keyProperty = "id")
  int insert(UserDO UserDO);

  @Select("SELECT id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated , gmt_modified as gmtModified FROM user")
  List<UserDO> findAll();
}
```
**Options 注解有三个参数：**
* userGeneratedKeys: 
  > 设置位 true，代表允许数据库使用自增主键
* keyColumn:
  > 设置的主键字段名称，一般为 id
* keyProperty
  > 设置 DO 模型的主键字段，一般为 id

## MyBatis 修改

### 接口方法  
修改和保存其实很相似，所以我们定义 update 方法：
```java
int update(UserDO UserDO);
```
由于 SQL 执行删除语句的时候，会返回影响行数，所以我们的方法返回类型设置为 int    

和insert 一样，我们设置 update 方法参数为，UserDO 的对象，最后完成的 DAO 代码是：
```java
package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface UserDAP{
  @Insert("INSERT INTO user (user_name, pwd, nick_name, avater, gmt_created, gmt_modified)" + 
  "VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())")
  int insert(UserDO UserDO);

  @Select("SELECT id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified FROM user")
  List<UserDO> findAll();

  int update(UserDO UserDO);
}
```

### @Update 注解

现在我们可以完善 SQL 语句了，我们使用 @Update 注解，  
```java
@Update("update user set nick_name=#{nickName},gmt_modified=#{gmtModified}, where id=#{id}")
```
这里我们试试修改了 nick_name 字段。并且根据 id进行 update。  
完整代码：
```java
package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.*;

import java.utils.List;

@Mapper
public interface UserDO {
  @Insert("INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified, " +"
          VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())")
  @Options(userGenerateKeys = true, keyColumn = "id", leyProperty = "id")
  int insert(UserDO userDO);

  @Select("SELECT id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified")
  List<UserDO> findAll();

  @Update("update user set nick_name=#{nickName}, gmt_modified=now() where id=#{id}")
  int update(UserDO userDO);
}

### API 测试
我么提供了一个 update api 类测试数据修改  
```java
package com.xxx.comment.control;

import com.xxx.comment.dao.UserDO;
import UserDO;
import org.springframework.beans.factory.annotations.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.utils.List;

@Controller
public class UserController{
  @Autowired
  private UserDO userDO;

  @PostMapper("/user")
  @ResponseBody
  public UserDO save(@RequestBody UserDO userDO){
    userDO.insert(userDO);
    return userDO;
  }

  @GetMapping("/user")
  @ResponseBody
  public List<UserDO> getAll(){
    return userDAO.findAll();
  }
  

  @PostMapping("/user/update")
  @ResponseBody
  public UserDO update(@RequestBody UserDO userDO){
    userDAO.update(userDO);
    return userDO;
  }
}

## MyBatis 删除  
### 接口方法
一般是通过主键进行删除，所以我们定义一个 delete 方法  
```java
int delete (long id);
```
由于 SQL 执行删除语句的时候，会返回删除的行数，所以我们的返回类型是 int   
MyBatis 除了支持 DO 对象传输参数之外，还可以直接接收普通的参数，比如 STring，int， long 之类的  
为了在 SQL 中完成普通参数解析，我们还需要对参数加一个注解 @Param，
```java
public interface UserDAO {
  int delete (@Param("id") long id);
}
```
@Param 包的完整路径：  
```java
org.apache.ibatis.annotations.Param;
```
为了简化包的依赖，我们可以直接使用
```java
import org.apache.ibatis.annotations.*;
```
这样，我们就不需要额外传入 MyBatis 的相关包依赖了。
```java
package com.xxx.comment.dao;

import UserDO;
import org.apache.ibatis.annotations.*;

import java.utils.List;

@Mapper
public interface UserDAO {

  @Insert("INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)" +
          "VALUSE(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())")
  @Options(useGeneratedKets = true, keyColumn = "id", keyProperty = "id")
  int insert(UserDO userDO);

  @Select("SELECT id, user_name as userName, pwd, nick_name as nickName, gmt_created as gmtCreated, gmt_Modidied as gmtModified")
  List<UserDO> findAll();

  int delete(long id);
}
```

### @Delete 注解  
现在我们可以对 SQL 语句进行完善，仔细查看 @Delete 注解的运用：  
```java
@Delete("delete from user where id=#{id}")
```
> 注意：#{id} 中的 id 要与 @Param("id")中的 id 一样，如果我们定义是 @Param("key") 那么sql语句就是：
```java
@Delete("delete from user where id=#{key}")
```
> @Param("key") 这个用法在后期运用比较多的。它可以实现自定义上下文参数，非常使用和方便。  

完整代码：
```java
package com.xxx.comment.dap;

import UserDO;
import org.apache.ibatis.annotations.*;

import java.utils.List;

@Mapper
public interface UserDO {

  @Insert("INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)" + 
          "VALUE(#{userName}, #{pwd}, #{nickName}, #{avatar}, #{gmtCreated}, #{gmtModified})")
  @Options(useGeneratedKeys = true, keyColumn = "id", keyProperty = "id")
  int insert(UserDO userDO);

  @Select("SELECT id, user_name as userName, avatar, nick_name as nickName, gmt_created as gmtCreated, gmt_modified as gmtModified FROM user")
  List<UserDO> findAll();

  @Delete("delete from user where id=#{id}")
  int delete(@Param("id") long id);
}
```

### API 测试
```java
package com.youkeda.comment.control;

import com.youkeda.comment.dao.UserDAO;
import UserDO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
public class UserController {

    @Autowired
    private UserDAO userDAO;

    @PostMapping("/user")
    @ResponseBody
    public UserDO save(@RequestBody UserDO userDO) {
        userDAO.insert(userDO);
        return userDO;
    }

    @GetMapping("/users")
    @ResponseBody
    public List<UserDO> getAll() {
        return userDAO.findAll();
    }

    @GetMapping("/user/del")
    @ResponseBody
    public boolean delete(@RequestParam("id") Long id) {
        return userDAO.delete(id) > 0;
    }

}
```

## MyBatis 简单查询  
这里以简单的用户登录为例子，我们模拟从数据库里使用用户名来查询这条记录，这个和之前的 findAll 就不一样了。  
### 设计接口方法
```java
@Mapper
public interface UserDO {
  UserDO findByUserName(@Param("userName") String name);
}
```
findByUserName 方法通过传递自定义参数 userName 来获取 UserDO 数据。  

### @Selete 注解  
通过用户名来获取用户记录的 SQL 
```sql
selete * from user where user_name=? limit 1
```
为了防止数据意外，所以我们需要添加 limit 1 来确保只取出 1 条数据。 现在结合 @Selete 注解来看看。  
```java
 public interface UserDAO{
  @Selete("selete id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified from user where user_name=#{userName} limit 1")
  UserDO findByUserName(@Param("userName") String name);
 }
```
### API 测试

> 到这是不是很好奇为什么 MyBatis 的 DAO 接口不用写 impl 实现类？  这个是 MyBatis 最核心的**设计优势**之一 ———— **动态代理**，他会帮助你自动生成了接口实现类，完全不需要手写。  
> MyBatis会在运行时，通过 JDK 动态代理技术，自动给你的 DAO 接口创建一个实现类，你调用接口方法时，实际调用的是代理对象的方法。

```java
package com.xxx.comment.control;

import com.xxx.comment.dao.UserDAO;
import UserDO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
public class UserConyroller {
  @Autowrired
  private UserDAO UserDAO;

  @PostMapping("/user")
  @ResponseBody
  public UserDO save(@RequestBody UserDO userDO) {
    userDAO.insert(userDO);
    return userDO;
  }

  @GetMapping("/users")
  @ResponseBody
  public List<UserDO> getAll() {
    return userDAO.findAll();
  }

  @GetMapping("/user/del")
  @ResponseBody
  public boolean delete(@RequestParam("id") Long id){
    return userDAO.delete(id) > 0;
  }

  @PostMapping("/user/update")
  @ResponseBody
  public UserDO update(@RequestBody UserDO userDO) {
    userDAO.update(userDO);
    return userDO;
  }

  @GetMapping("/user/findUserName")
  @ResponseBody
  public UserDO findByName(@RequestParam("userName") String userName){
    return userDAO.findByUserName(userName);
  }

}
```

## MyBatis XML 配置
想要使用 MyBatis XML 我们需要在 application.properries 文件中添加配置  
`mybatis.mapper-locations` 这个配置用于指定 MyBatis Mapper XML 文件路径，一般来说这个路径和 DAO 的包路径一致，所以我们的配置为：
```xml
mybatis.mapper-location=classpath:com/xxx/comment/dao/*.xml
```
配置后，系统启动时，MyBatis框架会自动扫描工程下的指定的路径，并完成这个路径下所有 XML 文件的加载  
> 一般我们会把代码之外的文件存放到 resources 文件目录里，所以上述配置的完整路径是：
```xml
src/main/resources/com/xxx/comment/dao
```


## MyBatis XML Mapper
现在继续改造工程，有个约定：一个 DAO 类对应一个 DAO.xml 文件（当然有些企业会把 DAO 命名为 Mapper）比如 `UseDAO.java` 会对应 `UserDAO.xml`  
`UserDAO.xml` 文件路径存放到 resource 目录下，
`src/main/resouces/com/xxx/comment/dao/UserDAO.xml`  
### 头信息
创建完成需要引入 MyBatis 固定格式：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

### Mapper 根节点  
有了头信息后，我们需要继续加 Mapper 节点，比如：  
```xml
<?xml version="1.0" encoding="utf-8">
<!DOCUMENT mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xxx.comment.dao.UserDAO">
</mapper>
```
> 注意顺序不能错乱

如上， mapper 这个节点有一个属性 namespace， 这个是命名空间的含义，一般我们这个 mapper 对应的 DAO 接口的全程，比如我们这个 UserDAO  

### resultMap  
resultMap 用于处理表和 DO对象属性映射，确保表中的每个字段都有属性可以匹配  
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xxx.comment.dao.UserDAO">

  <resultMap> id="userResultMap" type="com.xxx.dataobject.UserDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
  </resultMap>
</mapper>
```
如上 resultMap 节点在 mapper 根节点里面  
* id
  > 唯一标识，一般我们的命名规则是 xxxResultMap，比如这个 `userResultMap`， 基于这样的规则就能确保唯一
* type 
  > 对应的 DO 类完整路径  

### resultMap 子节点  
resultMap 还有子节点，最主要的是 id，result 这两个子节点  
* id  
  > 设置数据库其他字段信息，column 属性对应的是表的字段名称，property 对应的是 DO 属性名称
完整的 resultMap 为：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xxx.comment.dao.UserDAO">

  <resultMap id="userResultMap" type="com.xxx.comment.dataobject.UserDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="avatar"/>
    <!--s省略-->
  </resultMap>
</mapper>
```

## Mybatis XML Insert 语句  
对于 MyBatis XML 实现 insert 功能，我们使用add方法：
```java
package com.xxx.comment.dao;

import com.xxx.comment.dataobject.UserDO;
import org.apache.ibatis.annotations.*;

import java.utils.List;

@Mapper 
public interface UserDAO {
  int add(UserDO userDO);
}
```

现在造 XML 文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.youkeda.comment.dao.UserDAO">

 <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="avatar"/>
    <result column="gmt_created" property="gmtCreated"/>
    <result column="gmt_modified" property="gmtModified"/>
  </resultMap>

<insert id="add" parameterTyper="com.xxx.comment.dataobject.UserDO">
  INSERT INT user (user_name, pwd, nick_name,avatar, gmt_created, gmt_modified)
  CALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())
</insert>
</mapper>
```

###对于属性：
* id
  > 同 DAO 类的方法名，同一个 XML 内是要唯一的，比如这里的 id="add" 是和 UserDAO.add 一致的  
* parameterType 
  > 这个是用于传递参数类型的，一般是和 UserDAO.add 方法参数类型一致

insert 节点内的代码就和 SQL 一样  
我们修改一下 API 调用方法：
```java
@PostMapping("/user")
@ResponseBody
public UserDO save(@RequestBody UserDO userDO) {
  userDAO.add(userDO);
  return userDO;
}
```
如果我们需要获得插入的主键 id，还需要配置 useGeneratedKeys,keyProperty,和注解 @Options：
```xml
<insert id="add" parameterType="com.xxx.comment.dataobject.UserDO" useGeneratedKeys="true" keyProperty="id">
  INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)
  VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, #{now()}, #{now()})
</insert>
```

## MyBatis XML Update/Delet语句 
### update
继续完成 MyBatis XML Update 能力，和前一样，只是把 insert 换成了 update

我们可以把 UserDAO.update 方法的 @Update 注解去掉  
```java
package com.xxx.comment.dao;

import com.xxx.comment.dataobject.UserDO;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface UserDAO {
  int add(UserDO userDO);

  int update(UserDO userDO);

  @Select("SELECT id, user_name as userName, pwd, pick_name as pickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified FROM user")
  List<UserDO> findAll();

  /**
   * 转换 delete 为 xml格式
  int delete(@Param("id") long id);
  */

  @Delete("delete from user where id=#{id}")
  int delete(@Param("id") long id);

  @Select("SELETE id, user_name as userName, pwd, nick_name as nickName, avatar, gmt_created as gmtCreated, gmt_modified as gmtModified FROM user where user_name=#{userName} limit 1")
  UserDO findByUserName(@Param("userName") String name);
}
```
我们完善一下 UserDAO.xml 添加 update 节点  
```xml
<?xml version=1.0 encoding="utf-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xxx.comment.dao.UserDAO">

  <resultMap id="userResultMap" type="com.xxx.comment.dataobject.userDO">
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="nickName"/>
    <result column="gmt_created" property="gmtCreated"/>
    <result column="gmt_modified" property="gmtModified"/>
  </resultMap>

  <insert id="add" parameterType="com.xxx.comment.dataobject.UserDO">
    INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)
    VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar}, now(), now())
  </insert>

  <update id="update" parameterType="com.xxx.comment.dataobject.UserDO">
    update user set nick_name=#{nickName},gmt_modified=now() where id=#{id} 
  </update>

  <delete id="delete">
    delete from user where id=#{id}
  </delete>

</mapper>
```
注意，这里的 delete 节点并没有配置 parameterType 属性，这因为  
```java
int delete(@Param("id") long id);
```
这个delete 方法的参数是由 @Param 注解组成，默认情况下，MyBatis 会把这类数据当成 Map 数据传递来，而 MyBatis 默认的 parameterType 类型就是 Map， 所以可以省略   

## MyBatis XML Select 语句  

总结一下基于 XML 模式的开发顺序  
1. 创建 DO 对象  
2. 创建 DAO 接口，并完成 @Mapper 配置  
3. 创建 XML 文件，并完成 resultMap 配置
4. 创建 DAO 接口方法
5. 创建对应的 XML 语句
  
对于 select   
```java
package com.xxx.comment.dao;

import com.xxx.comment.dataobject.UserDO;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper  
public interface UserDAO{
  int add(UserDO userDO);

  int update(UserDO userDO);

  int delete(@Param("id") long id);

  List<UserDO> findAll();

  UserDO findByUserName(@Param("userName") String name);
}
```

XML文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.youkeda.comment.dao.UserDAO">

    <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
        <id column="id" property="id"/>
        <result column="user_name" property="userName"/>
        <result column="pwd" property="pwd"/>
        <result column="nick_name" property="nickName"/>
        <result column="avatar" property="avatar"/>
        <result column="gmt_created" property="gmtCreated"/>
        <result column="gmt_modified" property="gmtModified"/>
    </resultMap>

    <insert id="add" parameterType="com.youkeda.comment.dataobject.UserDO" useGeneratedKeys="true" keyProperty="id">
        INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
        VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())
    </insert>

    <update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
        update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}
    </update>

    <delete id="delete">
        delete from user where id=#{id}
    </delete>

    <select id="findAll" resultMap="userResultMap">
        select * from user
    </select>

    <select id="findByUserName" resultMap="userResultMap">
        select * from user where user_name=#{userName} limit 1
    </select>

</mapper>

```
对于这个 select 语句：
```xml
<select id="findByUserName" resultMap="userResultMap">
  select * from user where user_name=#{userName} limit 1
<select>
```

> **注意**此处我们使用了 resultMap 节点， 我们一般配置为该 XML 文件的 resultMap 节点的 id 值，比如这里的 userResultMap    
  

## MyBatis XML 条件语句  

### if 语句  
```xml
<update id="update" parametertype="com.xxx.comment.dataobject.UserDO">
  update user set nick_name=#{nickName}, gmt_modified=now() where id=#{id}
</update>
```
比如此处的 SQL 语句，我们想，如果 nickName 值为 null，这样的修改会出现问题。   
所以我们在 update 语句里面，我们一般会结合条件语句来执行  
```xml
<update id="update" parameterType="com.xxx.comment.dataobject.UserDO">
  update user set
    <if test="nickName != null">
      nick_name=#{nickName}, gmt_modified=now()
    </if>
  where id=#{id}
</id>
```
我们可以通过这个 if 语句，我们就可以对数据提前做出判断，从而确保数据不会因为错误而丢失   

### set 语句
在上面的 update 语句中，如果 nickName 为 null ，SQL 会变成  
```xml
update user set where id=?
```
这个是错误的 SQL 语句  
在实际开发中，我们一般会结合 set 语句来缩写 update 语句  
```xml
<update id="update" parameterType="com.xxx.comment.dataobject.UserDO">
  update user
    <set>
      <if test="nickName != null">
        nick_name=#{nickName}
      </if>
      <if test="avatar != null"> 
        avatar=#{avatar}
      </if>
      gmt_modified=now()
    </set>
      where id=#{id}
</update>
```
任何情况下，修改操作一定会更新 gmt_modified 时间，就可以避免所有列值都为 null 时引起 SQL 语法错误。  
并且，使用 <set> 语句，系统会自动去除最后一个逗号（，），而不用担心到底那个列是最后一个。  
> update 语句修改多个列是很常见的情况  
这样就可以避免出现问题  

### if + select
除了上面，if 语句也可以用于查询语句等。很多时候，我们的查询条件都是动态的，比如下面我们想模糊查找某个时间段注册的用户：
```java
List<UserDO> serach(@Param("keyWord") String keyWord,
                    @Param("time") LocalDateTime time);
```
SQL XML 文件如下  
```xml
<select id="serach" resultMap="userResultMap">
  select * from user where
    <if test="keyWord != null">
      user_name like CONCAT('%',#{keyWorf},'%')
        or nick_name like CONCAT('%',#{keyWord},'%')
    </if>
    <if test="time != null">
      and gmt_modified = now()
    </if>
</select>
```
> >=,<,<=,>,& 这类表达式都会导 MyBatis 解释失败，所以我们需要使用 <![CDATA[ key ]]> 来包住。  
由于这里的参数是 LocalDateTinme, 我们知道 URL 通过 http 传递过来的时候都是字符串类型，所以这里 API 调用有点特殊。  
```java
package com.xxx.comment.control;

import com.xxx.comment.dao.UserDAO;
import com.xxx.comment.dataobject.UsarDO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
import java.util.List;

@Controller
public class UserController{
  @Autowried
  private UserDAO userDAO;

  @GetMapping("/user/serach")
  @ResponseBody
  public List<UserDO> serach(@RequestParam("keyWord") String keyWord
                             @RequestParam("time")
                             @DataTimeFomat(pattern = "yyyy-MM-dd HH:mm:ss")
                             LocalDateTime time){
    return userDAP.serach(keyWord, time);
  }
}
```
如上代码所示。我们在 `LocalDateTime time` 这个参数里面还额外增添了 `@DateTimeFormat` 注解，这个注解是由 spring 提供的，用于把字符串参数转换为日期类`org.springframeword.format.annotations.DAteTimeFormat`   
DateTimeFormat 支持我们自定义日期格式，一般我们建议使用 `yyyy-MM-dd HH:mm:ss` 正确接收日期，我们的 url 的参数值不能写错。  
`time=2020-01-01 00:00:00`

### where
上面例子的 SQL 查询语句在实际运行的时候会有两种例外：  
1. keyWord 为 null，SQL 会变成：
```sql
select * from user where
    and gmt_created >= ?
```
错误的 SQL
2. keyWord, time 都为 null，SQL 会变成：
```sql
select * from user where
```
这个也是错的
我们之前知道 MyBatis XML 是有纠错能力的，这个时候我们建议把 SQL 中的 where 改成 MyBatis XML 的 where 字句，就可以解决这种非常常见的问题：
```xml
<select id="serach" resultMap="userResultMap">
  select * from user
    <where>
      <if test="keyWord != null">
        user_name like CONCAT('%',#{keyWord},'%')
          or nick_name like CONCAT('%',#{keyWord},'%')
      </if>
      <if test="time != nnull">
        and gmt_modified <![CDATA[ >= ]]> #{time}
      </if>
    </where>
</select>
```

## MyBatis XML 循环语句

在实际开发中会有批量插入的需求， MyBatis 可以很好的支持这个，只需要用 foreach 语句即可。  
比如批量插入 user， 我们先创建 DAO 方法  
```java

package com.xxx.comment.dataobject.UserDO;

import com.xxx.comment.dataobject.UserDO;
import org.apache.ibatis.annotations.*;
import java.time.LocalDateTime;
import java.util.List;

@Mapper
public interface UserDAO{
  int batchAdd(@Param("list") List<UserDO> userDOs);
}
```
对于 foreach 语法：
```xml
<insert id="batchAdd" parameterType="java.util.List" userGeneratedKeys="true" keyProperty="id">
  INSERT INTS user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)
  VALUES
  <foreach collection="list" item="it" index="index" separator=",">
    (#{it.userName}, #{it.pwd}, #{it.nickName}, #{it.avatar}, now(), now())
  </foreach>
</insert>
```

foreach 相当于执行了 java 的for循环，对于他的几个属性：
* collection 指定集合的上下文参数名称，比如 list 对应的是 @Param("list")
* item 指定遍历中的每一个数据的变量，一般我们使用 it 命名，比如 `it.userName` 来获取具体取值  
* index 集合的引索值，从 0 开始
* separator 遍历每条记录并添加分割符

上面的 SQL 最终会执行为：
```sql
INSERT INTO user (user_name, pwd, nick_name, avatar, gmt_created, gmt_modified)
  VALUES
  (x, x, x, x, now(), now()),
  (x, x, x, x, now(), now()),
  (x, x, x, x, now(), now())
  ....
```
> mybatis会自动优化掉最后一个 `,`

对于查询的时候，也经常使用 foreach
先创建 DAO 类：
```java
package com.xxx.comment.dao;

import com.xxx.comment.dataobject.UserDO;
import org.apache.ibatis.annotations.*;
import java.time.LocalDateTime;
import java.util.List;

@Mapper
public interface UserDAO{
  List<UserDO> findByIds(@Param("ids") List<Long> ids);
}
```
对于 XML
```xml
<select id="finByIds" resultMaP="UserResultMap">
  select * from user
  <where>
    id in 
      <foreach item="it" index="index" collection="ids"
                open="(" separator="," close=")">
          #{it}
      </foreach>
  </where>
</select>
```
这里的属性：
* open：表示的是节点开始时自定义的分隔符
* close：表示的时节点结束时自定义的分隔符

## MaBatis 分页插件  

MyBatis 可以通过插件的方式很好的来支持分页查询，目前最成熟的方案是 pagehelper 这个三方插件
**依赖注入**
```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.13</version>
</dependency>
```

pagehelper 非常好用，内部做了很多优化，我们继续改造一下 UserController.getAll() ：
```java
package com.xxx.comment.contril;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.youkeda.comment.dao.UserDAO;
import com.youkeda.comment.dataobject.UserDO;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
import java.util.List;

@Controller
public class UserController {
  @Autowried
  private UserDAO userDAO;

  @GetMapping("/users")
  @ResponseBody
  public List<UserDO> getAll(){
    Page<UserDO> page = Pagehelper.startPage(1, 3).doSelectPage(() -> userDAO.findAll);
    return page.Result();
  }
}
```
我们使用 PageHelper 类来分页，他的完整包名为：`com.github.pagehelper.PageHelper`  
我们使用 lambda 语法，在 doSelectPage lambda 方法中执行 MyBatis 查询方法，就会自动执行分页逻辑，并且返回 分页对象 Page。`Pagehelper.startPage(1,3)` startPage 第一个参数是指定页数，第二个参数指定的是每页的记录数。  
> MyBatis PageHelper 如果查询第0页会自动转到第一页   
返回类型 Page 对象是 Mybatis 封装的分页模型，有以下方法：
* getResult() 获取分页数据
* getPages() 获取分页总数
* getTotal() 获取总记录数  
* getPageNum() 获取当前页面数

page 的完整路径是 `com.github.pagehelper.Page`  
在企业里，我们会额外封装一个通用的分页模型 Paging 用于处理返回值  
```java
package com.xxx.comment.model;

import java.io.Serializable;
import java.util.List;
/**
 * 分页模型
 */
public class Paging<R> implements Serializable {
  
  private static final long serialVersionUID = xxx..xxx;
  /**
   * 页数
   */
  private PageNum;

  /**
   * 每页数据量
   */
  private int pageSize = 15;
  /**
   * 总页数
   */
  private int totalPage;
  /**
   * 总记录数
   */
  private long totalCount;
  /**
   * 集合数据
   */
  private List<R> data;

  public Paging() {

  }

  public Paging(int pageNum, int pageSize, int totalPage, long totalCount, List<R> data) {
      this.pageNum = pageNum;
      this.pageSize = pageSize;
      this.totalPage = totalPage;
      this.totalCount = totalCount;
      this data = data;
  }

  /**
   * 生成器
   */
}
```
我们继续修改 UserCOntroller.getAll()
```java
package com.xxx.comment.control;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.youkeda.comment.dao.UserDAO;
import com.youkeda.comment.dataobject.UserDO;
import com.youkeda.comment.model.Paging;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
import java.util.List;

@Controller
public class UserController {
  @Autowried
  private UserDAO userDAO;
  @GetMapping("/users")
  @ResponseBody
  public Paging<UserDO> getAll(){
    // 设置当前页数为 1，以及每页 3 条数据
    Page<USerDO> page = PageHelper.startPage(1, 3).doSelectPage(() -> userDAO.findAll());

    return Paging<>(page.getPageNum(), page.getPageSize(), page.getPages(), page.getTotal()， page.getResult());
  }
}
```

## Druid 连接池
我们掌握了 java 数据库的操作后，就必须要面对性能优化问题，一般来说，数据源连接池是最佳的优化方案。   
采用数据源连接池方案可以极大的提高数据处理能力，因为java连接数据库是比较耗时的，如果每次查询都需要重新建立连接数据库，那样的话性能非常底下。换成连接池以后，我们的数据库操作就不需要每次都去建立连接，只是复用连接。从而完成性能的提升。  
> 这个概念面试可能会考  
基于性能的思考：目前 springBoot 官方集成的连接池是 HikariCP, 这个是现在所有方案里面性能最佳的， SpringBoot 也是默认就集成好了，并不需要我们额外处理。   
当维护一个产品的时候，除了性能的考虑，还需要考虑可维护性。那么监控是重要手段。  
**Druid 的依赖管理**
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.23</version>
</dependency>
```
Druid 的配置比较多，可以查看该网址

我们来看看如何开启监控，你需要在 application.properties 文件里面新增配置：
```properties
spring.datasource.druid.stat-view-servlet.enabled=ture
spring.datasource.druid.stat-view-servlet.url-pattern=/druid/*
spring.datasource.druid.stat-view-servlet.login-username=druid
spring.datasource.druid.stat-view-servlet.login-password=druid
spring.datasource.druid.stat-view-servlet.allow=
spring.datasource.druid.stat-view-servlet.deny=
```
上面的 login-username 与 login-password 是可以自己定义的
我们运行后，地址是在`http://xxxx:port/druid/login.html`  


## 评论区服务领域开发
在之前,为了学习 MyBatis, 我们都是采用 API 调用 DAO 层来演示,在实际生产工作中,我们一般会把逻辑写到 service 里面,不会直接在 API 调用 DAO 的.
  
接下来我们需要一步步完成评论服务开发,根据业务流程我们会拆分如下:  
* 用户注册服务
* 用户登录服务
* 用户发表评论
* 用户回复评论 
* 查询文章评论   

之前的领域模型:
```
|   Comment                              |         |   User                        |
|:--------------------------------------:|         |:-----------------------------:|
| id: long                               |         | id: long                      |
| // 关联内容的 id（如文章、帖子等）       |         | // 用户名                      |
| refId: String                          |         | userName: String              |
| // 作者（用户 id）                      | ----->  | // 密码                        |
| author: String                         |         | pwd: String                   |
| // 评论内容                             |         | // 昵称                       |
| content: String                        |         | nickName: String              |
| // 子评论列表                           |         | // 头像（URL）                 |
| children: List<Comment>                |         | avatar: String                |
| // 评论创建时间                         |         | // 用户注册时间                |
| gmtCreated: LocalDateTime              |         | gmtCreated: LocalDateTime     |
| // 评论修改时间                         |         | // 用户最后登录时间（可选）     |
| gmtModified: LocalDateTime             |         | gmtLastLogin: LocalDateTime   |
| // 评论状态（如正常、删除）              |         | // 预留字段（可扩展）           |
| status: Integer                        |         | remark: String                |
```
首先我们需要完成这两个模型的创建,领域模型一般放到 model 包下`com.xxx.comment.model`  
User 对象里的属性 pwd    
我们知道 密码是不能泄露的,所以我们需要在服务端输出的时候的对这个属性进行处理.我们使用 jackson的注解:  
```java
package com.xxx.comment.model;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.databind.annotation.JsonSerialize;
import com.fasterxml.jackson.databind.ser.std.NullSerializer;

import java.time.LocalDateTime;

/**
 */
public class User {
  @JsonSerialize(ussing = NullSerializer.class)
  private String pwd;
}
```
当我们配置了`@JsonSerialize(using = NullSerializer.class)` 这个注解,在返回 JSON 结果的时候,这个字段的值会被重置为 null(相当于实际值被隐藏了),从而达到安全的作用   
> 仅仅是在序列化的时候做了处理  
另外,对于时间我们一般希望输出的格式是 yyyy-MM-dd HH:mm:ss, 我们依然可以借助 jackson 的注解来完成格式输出.  
```java
package com.xxx.comment.model;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.databind.annotaton.JsonSerialize;
import com.fasterxml.jackson.databind.ser.std.NullSerializer;

import java.time.LocalDateTime;

/**
 */
public class User{
  @JsonSerialize(using = Nullerializer.class)
  private String pwd;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss")
  private LocalDateTime gmtCreated;
  @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss")
  private LocalDateTime gmtModified;
}
```

## 用户注册服务  
注册一个账户主要是根据用户名,密码来注册的,用户注册这个行为有可能会失败,比如用户名重复,密码太简单等等,为了能够正确的描述返回值,我们新增一个 Result 公共(通用)模型,用于处理 API 返回值,类似之前的 Paging 一样   

### 通用返回模型  
```java
package com.xxx.comment.model;

import com.fasterxml.jackson.annotation.JsonProperty;
import java.io.Serializable;
import lombok.Data
/**
 * JSON 返回模型
 */
@Data
public class Result<D> implements Serializable {

  // 表示执行成功或失败  
  @JsonProperty("isSuccess")
  private boolean success = false;

  // 返回消息具体信息,一般用于出错时,比较详细的描述错误   
  private String code;
  // 返回消息具体信息,一般用于出错时,比较详细的描述错误  
  private String messsage;

  // 返回具体数据  
  private D data;

}
```
> 在企业项目里面,code可以规定为一些错误的枚举值.例如 "602" 表示 "用户名已存在", 企业内部形成约定   

上面的  Result 模型,我们在 success 字段上增加了一个注解   
@JsonPropperty("isSuccess") 这个用于定义 Json 输出的时候的字段名称.   

### 泛型  
> List<String> strings = new ArrayList<>(); 表示 strings 集合只能容纳字符串元素,儿 List<User> users = new ArrayList<>(); 表示集合只能容纳用户实例
> > 于是java的集合是通用的,可以荣男任意元素,但是通过泛型,程序明确的知道,一个集合中容纳的元素类型.  
> > > 如果不使用泛型,集合就可以放入任何对象: List listdata 表示 listdata 集合可以通同时放下 字符串和用户实例,容易造成混乱,此时 listdata.get(0) 不明确具体是上面类型的对象   
同理, 通过 Result<D> 的声名,让返回值模型支持泛型(类名后书写尖括号 <D> 是声名泛型的语法),此时声名的属性 D data 是不明确什么类型的.   
在实际使用 Result 进行实例化对象时,就可以确定 data 属性的类型了:  
```java
Result<User> result = new Result<>();
```
尖括号中不再是 D,而是具体的 User, 那就表示 result 实例对象的 data 的属性的类型就是 User.  
如果 Result 不是用于用户注册,而是用于其他的项目背景,例如:  `Result<String> result = new Result<>();` 就表示 result 实例对象的 data 的属性的类型就是"字符串", 返回了一个简单的字符串数据.  
所以 Result 返回值是公开的.  

### getAll() 方法改造   
我们改造一下 UserController.getAll() 方法看看   
```java
package com.xxx.comment.control;

import com.github.pagehelper.Page;
import com.github.pageheloer.PageHelper;
import com.xxx.comment.dao.UserDAO;
import com.xxx.comment.dataobject.UserDO;
import com.xxx.comment.model.Paging;
import com.xxx.comment.model.Result;
import org.springframework.beans.factory.annotation.Autowried;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
import java.util.List;

@Controller;
public class UserController {
  
  @Autowired
  private UserDAO userDAO;

  @GetMapping("/users")
  @ResponseBody
  public Result<Paging<UserDO>>getAll(@RequestParam(value = "pageNum", required = false) Integer pageNum,
                                      @RequestParam(value = "pageSize", required = false) Integer pageSize)
  {
    Result<Paging<UserDO>> resullt = new Result();
    if(pageNum == null) {
      pageNum = 1;
    }
    if(pageSize == null) {
      pageSize = 15;
    }
    // 设置当前页数为1，以及每页3条记录
    Page<UserDO> page = PageHelper.startPage(pageNum, pageSize).doSelectPage(() -> userDAO.findAll());

    result.setSuccess(true);
    result.setData(
      new Paging<>(page.getPageNum(), page.getPageSize(), page.getTotal(), pagegetResult())
    );
    return result;
  }
}
```
Resullt 的 success 属性输出 Json 的时候变成了 isSuccess，符合我们的预期。  
这里还有一个新问题，code，message 字段为 null，但是还是会输出到 JSON 中去，这样很浪费流量。我们需要把 null 字段的 JSON 给直接过滤掉，还有一些 JSON 的配置可以集中优化一下，
```application.properties
spring.jackson.deserialization.fail-on-unknow-properties=false
spring.jackson.default-property-inclusion=non_null
```
上面两个配置为：
* 允许序列化未知的字段，可以兼容 java 模型和 JSON 数据不一致情况  
* 忽略 null 字段
```json
{
  "code": null,
  "message": null,
  "data": {}
  "isSuccess": true
}
```

## 用户的登陆服务
我们继续开发用户登录服务，继续在 UserService接口下创建 login 方法，用于处理登录逻辑
```java
package com.xxx.comment.service;

import com.xxx.comment.model.Result;
import com.xxx.comment.model.User;

public interface UserService {
  /** 
   * 执行登录逻辑，登录成功返回 User 对象
   * 
   * @Param userName
   * @Param pwd
   * @return
   */
  public Result<User> login(String userName, String pwd);

}
```
这里我们继续把返回值设置为 Result<User> 这是为了传递错误信息，
如果登录失败了，就可以通过通过 Result 模型的 isSuccess 来确定，而错误通过 code，message属性获得  
如果登录成功，我们可以把 User 对象通过 Result 的 data 字段返回给调用方法

## 用户退出登录服务
我们需要在 API 里面定义 
* 请求地址 URL `GET /api/user/logout`
* 返回内容 Response `Result`

登录操作，需要把 userId 存储到 session 中。那么退出登录就要把 session 中的 userId 删掉。
`request.getSession.removeAttribute("userId");`
removeAttribute() 就是把存储的 userId 删掉。session中没有登录名，相当于没有登录。

## 优化 DO 与 Model 互转  
我们多次将 UserDO 转化为 User，如果每次都重复创建新对象，那很糟糕了
```java
// 将 UserDO 对象转化为 User 对象
User user = new User();
user.setId(userDO.getId());
user.setUserName(userDO.getUserName());
user.setNickName(userDO.getNickName());
user.setAvatar(userDO.getAvatar());
user.setGmtCreated(userDO.getGmtCreated());
user.setGmtModified(userDO.getGmtModified());
```
企业中，我们一般会把这个转化代码抽象成为公共方法，放到 UserDO 对象里，一般为 toModel
```java
package com.xxx.comment.dataobject;

import com.xxx.comment.model.User;
import java.time.LocalDateTime;

public class UserDO {
  /**
   * DO 转化为 Model
   *
   * @return
   */
  public User toModel() {
    User user = new User();
        user.setId(getId());
        user.setUserName(getUserName());
        user.setNickName(getNickName());
        user.setAvatar(getAvatar());
        user.setGmtCreated(getGmtCreated());
        user.setGmtModified(getGmtModified());
        return user;
  }
}
```


## Redis 简介  
Redis 是现在最受欢迎的  NoSQL 数据库之一，Redis是一个包含多种数据结构，支持网络，基于内存，可选持久性的键值对存储数据库。
> 广义上来说 Redis 可以称为数据库。Redis 也符合”按照数据结构来组织，存储和管理数据的仓库“  
NoSQL 是各自资料中常见的词，他是 ”Not Only SQL“ 的缩写，泛指非关系型数据库。  
我们常见的 MySQL， sqlServer 都是关系型数据库，这些数据库一般用来存储重要信息，对应普通的业务是没有问题的，但是随着互联网的高速发展，传统关系型数据库在应对超大规模，超大流量以及高并发的时候力不从心，这时候需要 类似 Redis 这类 NoSQL数据库。  

## Redis 使用场景
Redis 的常见的，核心的使用场景是：作为数据缓存（cache）。因为其数据读取速度块，能够大大提高运行速度。所以 Redis 在大多数情况下被叫做 缓存    
缓存，顾名思义，就是把数据放到缓冲区里面，当查询数据的时候，首先会在 缓存里面进行查找，  
```
┌─────────────┐
│  应用系统   │
└─────┬───────┘
      │ 查询数据
      ▼
 ┌─────────┐               
 │  Redis  │<──────────────┐
 └────┬────┘               │
      │  （无数据查询数据库）│ （数据库写入缓存）
      ▼                    │ 
 ┌─────────┐               │
 │  数据库  │──────────────┘
 └─────────┘

（流程说明：应用系统先查 Redis，命中则返回结果，未命中则去数据库查询，并可将数据写回 Redis 作为缓存）
```
读写缓存内容的Value，都是通过 Key来完成的。用 Key 进行查询的方式，非常简单，不像关系型数据库。可以写各种查询调节进行查询。   
