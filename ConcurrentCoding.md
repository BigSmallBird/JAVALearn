# Lambda 表达式
## 无类型参数

Lambda 表达式在基本结构:  
      f -> {}  
f 为参数变量 ，-> 为语法符号  {} 为语句块

Lambda 表达式在功能上等同一个匿名方法
```java
public void unknow(f) {
    System.put.println(f.getName());
}
```
> 相当于方法名与 public void 修饰被省略掉了

## 识别类型
对于 f 的类型 编辑器会根据上下问进行识别  
比如：  
```java
List<Fruit> fruits = Arrays.aslist(.....);  

friut.forEach(f -> {
    System.out.println(f.getName());
});
```

forEach 方法循环遍历 fruits 集合，每次循环时 f 变量指代当前元素  
由于 fruits 变量的类型使用泛型语法定义为 ```List<Fruit>```， 表示集合中的类型是 Fruit  
所以， f 变量自然是 Fruit， 就可以调用 Fruit 类的方法。  

所以，Lambda 表达式需要配合上下文没跟其他方法一样，而不是一个对立的语句  
比如这样就是错误的
```java
public static void main(String[] args) {
    args[0] -> {
        System.out.println(args[0]);
    }
}
```

Lambda 表达式又有一些新的写法：  
**多个参数**  
箭头 (->) 前表示参数变量，有多个参数的时候，必须使用小括号包裹：  
（）
```java
(student1, student2) -> {}
```
**无参数**  
箭头 (->) 前表示参数变量，没有参数的时候，必须使用小括号： （）
```java
() -> {}
```
**单条执行语句**  
箭头 (->) 后的执行语句就一条的时候，可以不加大括号 {} 包裹
```java
s -> System.out.prinln(s);
```
**有参类型**  
大多数情况下，使用 Lambda 表达式的目的是为代码尽量简洁，所以尽量省略参数变量类型。
如果代码比较复杂，为了容易阅读，维护，也可以指定参数类型  
```java
fruits.forEach((Fruit f) -> {
    System.out.println(f.getName());
}); 
```

## 引入外部变量
Lambda 表达式 {} 内的执行语句，除了能引用参数变量以外， 还可以引用外部变量。  
```java
List<Fruit> fruits = Arrays.asList(......);  
String message = "水果名称";

fruits.forEach(f -> {
    System.out.println(message + f.getName());
});

message = "";
```
可以看见 报错了  
因为 在 Lambda 表达式中引入的 外部变量默认被修饰为 final 对象，后期无法被修改。  
比如这里的 message = "" final 修饰的变量只能被实际赋值一次。  
然后就是，参数2不能与局部变量同名。  

## 双冒号 (::) 操作符  
Java8  支持双冒号 “::” ,也是一种 lambda 表达式，  
前面例子中，只有一条执行语句的 Lambda 表达式：  
```java
List<String> names = Arrays.asList(xxxxxx);

names.forEach(n -> {
    System.out.println(n);
});
```
可以简化成为：   
```java
names.forEach(System.out::println);
```
这段代码重点是使用 :: ,系统每次遍历取得元素 n ，会自动作为参数传递给 System.out.println 方法打印  
**System.out::println 等同于 n -> {System.out.println(n);}**

:: 语法干脆省略了参数变量，所以读这个代码需要相互对照，自行脑补  
### 语法含义
System.out 类名（静态调用方法）或实例对象变量名（非静态方法）   
:: 语法符号  
println 方法名  
双冒号 :: 语法，不能独立使用，需要配合特定的方法  
### 不同用法  
* **用法一** ：静态方法调用  
使用 LambdaTest::println 替代 f -> LambdaTest.print(f), 简化代码  

* **用法二** ：调用非静态方法  
print() 方法不再被标识为 static， 于是需要实例对象来调用。  
  ```java
  fruits.forEach(new LambdaTest()::print);
  ```  
  只是简写了:   
  ```java
  LambdaTest lt = new LambdaTest();  
  fruits.forEach(lt::print);
  ```  
  实际上， System.out.println 语句就是一个调用非静态方法 println(), 因  
为 System.out 代指的是一个实例对象。 

* **用法三** ：多参数  
比如  
   ```java
   Collections.sort(students, (student1, student2) -> {
    return student1.getAge() - studnet2.getAge();
   });
   ```
   就是一个多参数情况。可以把比较过程定义成为一个方法：  
   ```java
   private static int compare(Student s1,Student s2) {
    ...
   }
   ```
   那么排序过程可以简写为：  
   ```java
   Conllections.sort(students, SortTest::compare);
   ```
   > 注意，系统会自动获取上下文参数，并按上下文定义的 **顺序** 传递给指定的  
   方法。所谓顺序就是 Lambda 表达式 () 中的顺序

* **用法四** ：父类用法  
super 关键字的作用是在子类中引用父类的属性或者方法。同样， :: 语法可以用 super  
关键字调用父类的静态方法
  ``` java
  import java.util.ArrayList;
  import java.ulit.List;

  public class LambdaTest extends LambdaExample {
    public static void main(String[] args) {
        List<Fruit> fruits = Attays.asList(
            new Fruit("香蕉"),
            new Fruit("苹果"),
            new Fruit("梨子"),
            new Fruit("西瓜"),
            new Fruit("荔枝") 
        );

        LambdaTest at = new LambdaTest();
        at.print(fruits);
    }

    public void print(List<Fruit> fruits){
        fruits.forEach(super::print);
    }
  }

  class LambdaTest {
    public void print(Fruit f){
        System.out.println(f.getName());
    }
  }
  ```

# Stream API
java8 的新特性：Stream 流。
Stream 的主要作用是对集合 Collection 中的数据进行各种操作，增强   
了集合对象的功能。

## 流迭代 forEach

在java 里， Stream是一个接口（interface），当然也有多个实现类。但是 java 是面向接口编程的，我们暂时不需要关心具体有哪些实现类，先会使用调用 Stream API  
> 接口中提供了操作数据的方法，通常叫做 API

### 创建流

创建流的方式有很多：  

* 直接创建：
  ```java
  import java.util.stream.Stream;

  Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
  ```

* 由数组转换：
  ```java
  String[] fruitArray = new String[] {"苹果", "哈密瓜", "香蕉", "西瓜", "火龙果"};
  Stream<String> stream = stream.of(fruitArray);
  ```

* 由集合转换：
  ```java
  List<String> fruits = new ArrayList<>();
  fruits.add("苹果");
  fruits.add("哈密瓜");
  fruits.add("香蕉");
  fruits.add("西瓜");
  fruits.add("火龙果");
  Stream<String> stream = fruits.stream();
  ``` 

  > 无论那种方法创建，由于源数据的数据顺序是有序的，所以流的元素也是有序排列的

### 迭代流

Stream 提供了 forEach 的API 进行迭代。
```java
Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
stream.forEach(System.out::println);
```
> 为了系统能够自动识别 Lambda 表达式里面在参数类型，也必须使用泛型话音未落指定 Stream 中在对象类型。另外 集合在forEach() 与流的 forEach() 方法不一样，仅仅名字相同

## 数据流过滤 filter

有一批小学生数据

| name   | 平均分 | 违规次数 |
| ------ | ------ | -------- |
| 司音   | 75     | 1        |
| 白浅   | 80     | 0        |
| 荀飞盏 | 95     | 8        |
| 墨渊   | 79     | 0        |
| 夜华   | 90     | 0        |
| 霓漫天 | 81     | 0        |

优秀学生要求期末考试平均分不低于 80 无违规记录   
小学生模型：  
```java
public class Pupil {
    private String namel
    private int avarageScore;
    private int violationCount;
}
```
如何统计优秀学生？  
* 传统实现
```java
List<Pupil> pupils = new ArrayList<>();
private static void init(String param){
    .....
} 

List<Pupil> qualified = new ArrayList<>();
for (Pupil pupil : pupils){
    if (pupil.getAverageScore() >= 80 && pupil.getViolationCount() < 1){
        qualified.add(pupil);
    }
}

for (Pupil pupil : qualified) {
    System.out.println(pupil/getName());
}
``` 

### 新特性实现
```java
List<Pupil> pupil = new ArrayList<>();
private static void init(){
    ....
}
pupils.stream()
    .fulter(pupil -> pupil.getAverageScore() >= 80 && pupil.getViolationCount < 1)
    .forEach(pupil -> {System.out.println(pupil.getName);});
```

#### filter() API

从方法名称我们可以理解：对数据流对象进行过滤。    
 
 pipil ->  pupil.getAverageScore() >= 80 && pupil.getViolationCount < 1  

方法参数就是一个 Lambda 表达式，箭头后面是条件语句，判断数据需要符合的条件  

## 流的设计思想

数据流的操作过程，可以看成一个管道，管道有多个节点构成，每个节点完成一个操作。   
数据输入这个管道，按照 顺序 经过各个节点。最后输出成为 console。  

![数据流示意图](E:/project/xxx/Concurrent_Coding/image/j5-2-4-1.svg)

.filter().forEach() 组成了一个管道，每个方法都是一个管道的节点。方法调用的顺序构成了管道节点的顺序。  


## 流数据映射  map

对于一组数字：  
```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 7, 63, 2, 3, 5);
```
计算每个数字的平方输出  
使用 Stream API 很简单：  
```java
numbers.stream()
    .map(num -> {
        return num * num
    })
    .forEach(System.out::println);
```

这里用到的是 map() 方法。  

![map流程示意图](E:/project/xxx/Concurrent_Coding/image/image.png)

数据流 -> map(映射数据 —> 源数据 ....)  数据流->  

map() 方法通常称作 映射， 其作用就是用新元素替代掉流中原来相同位置的元素。相当于每个对象都经历一次转换。

### 映射到新数据

map() 方法的参数就是一个 Lambda 表达式， 在语句块中对流中的每个数据对象进行计算，处理，最后用 return 语句返回对象，就是转换后的对象  

**优点** 
映射后的对象类型，可以与流中原始数据的类型不一样。  
在流中，可以用字符串替代原来的整数。这就极大的提高了灵活性，扩展性，让流的后继操作可以更加方便。  

**特例写法** 
少数情况下，如果替换的语句简单，系统能够自动识别需要返回的值，代码可以简写为：```.map(num -> num * num)```


## 流数据排序

对于数据的排序处理，我们使用 Lambda 表达式进行优化：  
```java
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

Collections.sort(students, (student1, student2) -> {
    return student1.getRollNo() - student2.getRollNo();
});

students.forEach(s -> System.out.println(s));
```

使用 Stream API 就更简单了：  
```java
students.stream()
    .sorted((student1, student2) -> {
        return student1.getRollNo() - student2.RollNo();
    })
    .forEach(System.out::println);
```

sorted 就是完成排序的方法。把排序规则写成一个 Lambda 表达式传给词方法即可。
> 核心的排序规则 Lambda 表达式是一样的。  

对于排序，无论是什么集合排序 Conlleaction.sort() 还是流排序 sorted，都需要一个返回值，对于返回为 **非正数** 表示俩个相比的元素需要交
换位置，返回正数则不需要。


## 数据流摘取
对于一组数字：  
```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 7, 63, 2, 3, 5);
```
找出最大的三个

答案：  
```java
numbers.stream()
    .sorted((n1, n2) -> n2 -n1)
    .limit(3)
    .forEach(System,out::println);
```

limit() 方法返回的是流前 n 个数据 当然 你不能为负数。  
> 只能摘取流的开头  

## 流的设计思想 二

与普通 java 相比， Stream 的特点是，编程的重点从对象的运用与数据计算的转换  
如果使用普通的 java 代码，重点是使用对象完成各种各样的逻辑，加上语法代码也比较多，导致整个代码比较繁杂。  

使用了 Stream API， 系统会自动完成很多操作，加上大幅的简化了语法，开发者的注意力重点就变成了数据计算步骤，不用太关心变量类型，变量赋值，对象的转换等。编程的重点会变得更加清晰。  

Stream 的这种变化特征是： 函数式风格。即弱化了对象的严格，完整的语法，重心变成了通过函数完成数据计算。  

![函数式编程](E:/project/xxx/Concurrent_Coding/image/image1.png)

数据集 -> 数据流 -> 数据计算  

## 流合并  

Stream API 包括 filter, map, sorted 都统称为 聚合操作。  
，聚合操作就是把集合中的对象做整体性的计算  
> 一般来说，计算，操作，处理 这几个词表达都是同一个意思，都是比较宽泛含义，

**案例**  
对 1 - 10 的十个正整数求和：  
```java
List<Integer> number = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```
对于 Stream API 完成的计算代码是：  
```java
import java.util.Arrays;

int sun = numbers.stream()
    .reduce((a, b) -> a + b)
    .get();

System.out.println(sum);
```

reduce 方法的作用就是合并所有的元素，终止计算出一个结果。注意这里的终止的意思，是流已经达到终点结束了，不能再继续流动了。

reduce() 方法的返回值是一个比较复杂的对象，需要调用 get() 方法返回最终结果。
> get 返回值类型，也需要根据系统自动推断
 
reduce() 方法的参数有点复杂  
* a 在第一次执行计算语句 a + b 时，指代流的第一个元素；然后充当缓存作用以保存放本次计算结果，此后计算语句时，a 的值就是上一次计算结果并继续充当缓存
 存放本次计算结果
* b 参数第一词执行执行计算语句时指代流的第二个元素。此后依次指代流的每个元素。
  
reduce() 方法的第一个参数（a）有多重作用，并且系统是自动完成参数（本例的 a, b）赋值的，所以仍然体现了 Stream 编程的重点仍然是计算（a + b）

## 流合并 二

reduce() 方法不仅计算整数值，也是同样可以操作对象的。  
```java
List<Student> students = new ArrayList<>();
List<Student> students = new ArrayList<>();
students.add(new Student("赵祯", 92));
students.add(new Student("曹丹姝", 60));
... ...
... ...
```
计算学生分数，并打印，如果需求比较复杂，就要使用 student 对象了，
```java
Student result = students.stream()
    .reduce(
        (a, b) -> {
            a.setMidtermScore(a.getMidtermScore() + b.getMidtermScore());
            return a;
        }
    )
    .get();

System.out.println(result.getName() + " - " + result.getMidtermScore());
```
但是输出的结果就是 -777 出现了 bug  
出现这个 bug 的原因是，第一个 Student 对象由于充当了缓存角色，正确性被破坏。  
### 解决方法：
reduce() 提供了另一个参数形式，自己可以 new 一个对象充当缓存角色，而不是使用 流中的原始对象。
```java
Student result = student.stream()
        .reduce(new Student("",0),
            (a, b) -> {
                a.setMidtermScore(a.getMidtermScore() + b.getMidtermScore());
                return a;
            }
        );
System.out.println(result.getName() + " - " + result.getMidtermScore());
```
reduce() 参数变成了两个：  
* 第一个参数是作为缓存角色的对象
* 第二个参数，是 Lambda 表达式，完成计算，格式是一样的。    
  * 那么 a 变量不再指代流中的第一个元素，专门指代缓存角色的对象，即方法第一个参数对象
  * b 变量依次指代流的每一个元素，包括第一个元素。
  * a，b 职责清晰

## 流收集 
forEach() 方法和 reduce() 方法都是流的终点，在实际工作中，整体功能比较复杂的情况下，使用流对集合
进行计算后，可以把结果元素放到一个新的集合中
> 例如可以把流传递给 Thymeleaf 模板

对于一族数据： 
```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 7, 63, 2, 3, 5);
```
找出最大的前 3 个数字放入一个新的集合中，用 - 组合成字符串打印。  
```java
import java.util.stream.Collectors;

List<String> numResult = numbers.stream()
        .sorted((n1, n2) -> n2 - n1)
        .limit(3)
        .map(a -> "" + a)
        .colecte(Collectors.toList());
    
String string = String.join("_" + numResult);
System.out.printyln(string);
```

collect() 方法的作用就是收集元素，但是元素收集去哪?  
Collectors.toList() 是一个静态方法，作为参数告诉 collect() 方法存入一个 List 集合。所以 collect() 方法的返回值类型就是 List。

> java.util.stream.Collectors 是流工具包的收集器

为了把最终的结果转换成为字符串，调用 map() 方法把流中数据映射成为字符串（“” + a） 所以 collect() 方法的返回值类型就是 ```List<String>``` 而不是 ```List<Integer>```  collect(Collectors.toList()) 是一个标注的方法。

## 并行流

 Stream API 的设计很想一个管道：  
    ``` 数据集 --> 数据流 --> 数据操作（过滤 —-> 计算 —-> .....）```
管道的显著特点是每个节点是依次执行的，下一个节点必须等待上一个节点执行完毕。这种执行方式，通常叫做 **串行** 。  
> 无论多少个节点都排成一个队伍 

### 性能问题
如果计算过程愈来愈复杂，数据量愈来愈大，**串行** 工作模式性能会愈来愈差。我们现在已经进入大数据时代，在实际工作的数据，可能是巨大的，计算是复杂的。  
**串行** 工作模式的性能很难被优化。因为这种模式无法发挥 **多核 CPU** 的优势。  

### 解决办法

为了充分发挥 多核CPU 的优势，可以把串行设计成**并行**计算模式。  
当然 **并行** 并不是简单的将一个队伍变成三五个队伍。所谓 **并行** 就是利用多线程，变成同时执行。多线程可以充分发掘多核 CPU 的优势。  

使用并行流的代码很简单，不再调用 stream() 方法，改为调用 parallelStream() 方法即可。其它的计算方法是一样的。

parallelStream() 以并行的方式执行任务,同时也支持流的收集，合并等计算。结合下图理解与串行运算的不同：  
![并行流示意图](E:/project/xxx/Concurrent_Coding/image/image2.png)

### 不适合使用并行计算的场景

流中的每个元素之间，有逻辑依赖关系的时候，不合适使用并行计算。

## 并行流的性能意外

实际上，并行计算模式的性能并不是任何情况下都优于串行模式。  
1. 硬件差   
   CPU 核数很低，特别是单核的情况下，并行计算模式不一定好。因为并行流需要等待 CPU 资源。  
2. 任务简单  
   数据量小，任务简单的情况下，并行计算模式不一定更好。
> 并行的使用要注意元素之间是否存在依赖

## 设计模式的作用

设计模式本身并不是 Java 特有的，它是经过时间沉淀形成的解决问题的方法。  

## 单例模式

在实际中我们会遇到唯一的现象，例如：一个班只有一个班主任，一般的思维都会去定义 班主任 与 班级 两个类：
```java
public class ClassMaster {
    private String id;
    private String name;
    private String gender;
}

public class Classes {
    public static void main(String[] args) {
        ClassMaster classMaster = new ClassMaster;
        ClassMaster classMaster2 = new ClassMaster;
    }
}
```
但是这样违背了要求不能一个类只存在一个班主任实例

单例模式就是来解决这样的问题：保证一个类仅仅有一个实例  
要做这一点，核心就是将构造方法设置成为私有。
```java
public class ClassMaster {
    private String id;

    private String name;

    private String gender;

    private ClassMaster() {

    }
}
```
把构造方法设置成为私有，意味着，除了 ClassMaster 自己，其他任何类都不能实例化 ClassMaster 对象。

### 特殊的实例化
```java
public class ClassMaster {
    private String id;
    private String name;
    private String gender;
    private static ClassMaster instance = new ClassMaster();

    private ClassMaster() {} 
}
```
在 ClassMaster 类中定义一个 ClassMaster 类型的变量，赋值为 new 出来的自己的实例。
> 必须使用 static 修饰，否则会产生死递归的错误

也就是说，不允许其他类实例化 ClassMaster(私有化构造方法)，只有自己能实例化一个唯一的自己， 这种可以保证只有一个实例对象的方式，叫做**单例**设计

###访问实例
类 new 出一个实例的目的，是要给其他类使用，所以还要有一个方法，允许其他类使用这个单例实例。
```java
public class ClassMaster {
    private String id;
    private String name;
    private String gender;

    //唯一实例
    private static ClassMaster instance = new ClassMaster();

    private ClassMaster(){

    }
    // 外部可以通过这个方法访问唯一的实例
    public static ClassMaster getInstance() {
        return instance;
    }
}

过程：
// 私有化静态单例
ClassMaster instance = new ClassMaster()
// 私有构造方法
ClassMaster()
// 公共的静态访问方法
ClassMaster getInstance()
```

### Spring 中的单例

不仅仅是逻辑功能要求只有一个单例，有时候技术角度出发为了节省系统资源，也需要用到单例模式。  
在 Spring 中，变量使用 @Autowried 注解，能够实现**自动**注入实例对象。   
实际上，任何**自动**注入实例对象，默认只有一个实例对象，是单例的。例如：可能多个 Service 和 Control 等都需要等到用户服务，那这些类中都会定义 
```java
@Autowired
private UsersService usersService;
```

Spring 会保证只生成一个 UsersServiceImpl 实例，注入到多个 Service 或者 Control里面，不会为每一个 Service 或者 Control 分别 new 出多个 UsersServiceImpl 实现类的实例。因为 Spring 也应用了 单例的思想。  

## 简单工厂模式
另外一种很常见的模式是简单工厂模式。程序里面的“工厂”是产生实例的地方。

在复杂情况，比如餐馆，水果店，甜品店，都想根据客人的，口味提供送水果的服务时，用伪代码写：
```java
public class 餐馆 {
  public static void main(String[] args) {
    if (客人.get口味() == "甜") {
      西瓜 w = new 西瓜();
    } else if (客人.get口味() == "酸") {
      柠檬 l = new 柠檬();
    } else if (客人.get口味() == "臭") {
      榴莲 d = new 榴莲();
    }
  }
}

public class 甜品店 {
  public static void main(String[] args) {
    if (客人.get口味() == "甜") {
      西瓜 w = new 西瓜();
    } else if (客人.get口味() == "酸") {
      柠檬 l = new 柠檬();
    } else if (客人.get口味() == "臭") {
      榴莲 d = new 榴莲();
    }
  }
}
```
但是这种方法实现的时候存在核心问题就是：  
1. **代码重复**：业务添加困难
2. **耦合紧密**：修改困难

>所谓 耦合 指的是某个类的代码，包含了其他类的逻辑细节。包含的越多，耦合越紧密，也容易相互影响。

### 解决方法
简单工厂可以解决这个问题。顾名思义，程序里面的工厂，是生产实例对象的。实现简单的工厂只需要两步。   
1. 从具体的产品类抽象出接口。记住 java 强调面向接口编程。意味着工厂应该生产**一种**产品，而不是一个产品
2. 把生产实例对象的过程，收拢到工厂类实现。

如图：  
![简单工厂模式图](E:/project/xxx/Concurrent_Coding/image/image3.png)

FruitFactory(Fruit getFruit(Customer customer)) --> Customer   
Fruit <--|   
|<--WaterMelon and Lemon and Durian  

核心的工厂实现：  
```java
public class FruitFactory {
    public static Fruit getFruit(Customer customer){
        Fruit fruit = Fruit getFruit(Customer customer){
            Fruit fruit = null;
            if("sweet".equals(customer.getFlavor())){
                fruit = new Watermelon();
            } else if ("acid".equals(customer.getFlavor())) {
                fruit = new Lemon();
            } else if ("smelly".equals(customer.getFlavor())) {
                fruit = new Durian();
            }

            return fruit;
            }
        }
    }
```
### 命名
一般工厂命名为 XXXXFacory

### 抽象类的应用

刚刚为了显示轿车的品牌，接口中定义了 getBrand() 方法。所以每个轿车的实现类都需要写：  
```java
private String brand;
public String getBrand() {
    return brand;
}
public void setBrand(String brand) {
    this.brand = brand;
}
```
这样违背了简约代码的基本原则

我们可以通过简单工厂模式进行设计  
![简单工厂模式](E:/project/xxx/Concurrent_Coding/image/image4.png)

接口 car 与实现类之间，加上一个 AbstractCar ,轿车类不再直接实现接口，而是继承 AbstractCar.

## 抽象工厂模式

简单工厂适合创建一种对象。但是有时候也会遇见复杂的问题，需要创建一个系列，多种产品的时候，简单工厂就不适合了。  
例如餐馆，水果超市，商品总类太多，如果是简单工厂模式，那么工厂代码会是这样的：
```java
public class 餐馆 {
    public static void main(String[] args) {
        水果 a1 = 水果工厂.取得水果("舔");
        果汁 a2 = 饮料工厂.取得饮料("碳酸");
    }
}
```
但是 水果与 饮料属于零食系列，比如五金店不会卖水果，意思就是要知道哪些工厂需要搭配，这就导致餐馆与多个工厂耦合太紧，不利于扩展。

###  解决方法
对于一批，多种类型的对象需要创建的场景，使用抽象工厂模式会更好。  
简单工厂的主要把多个产品抽象，使用一个工厂统一创建；那么抽象工厂的主要作用就是把多个工厂也进一步进行抽象。  
![抽象工厂模式](E:/project/xxx/Concurrent_Coding/image/image5.png)

实际上就是进一步抽象出来了工厂接口（SnacksFactory），然后多出了一个 SnacksFactoryBuilder

### 工厂接口
工厂接口即规定工厂应该提供什么样子的产品，所以包括了所有工厂的方法：
```java
public interface SnacksFactory {
    public Fruit getFruit(Customer customer);
    public Drink getDrink(Custoner customer);
}
```

但是又个问题，水果工厂是不提供饮料的，但是水果工厂可以实现工厂接口后，又必须实现 getDrink() 方法，这个时候直接返回 null 即可。
```java
public class FruitFactory implements SnacksFactory {
    public Fruit getFruit(Customer customer){
        Fruit fruit = null;
        if("sweet".equals(customer.getFlavor())) {
            fruit = new Watermelon();
        }else if("acid".equals(customer.getFlavor())){
            fruit = new Lemon();
        }else if("smelly".equals(customer.getFlavor())){
            fruit = new Durian();
        }
        return fruit;
    }

    public Drink getDrink(Customer customer){
        return null;
    }
}
```
> 水果工厂不能实现 getDrink 的方法 就 return null

### 工厂的工厂
SnacksFactoryBuiler 称为工厂的工厂， 工厂用来生产产品实例， SnacksFactoryBuilder 用来生成工厂实例。

```java
public class SnacksFactoryBuilder {
    public SnacksFactory buildFactory(String choice) {
        if (choice.equalsIgnoreCase("fruit")) {
            return new FruitFactory();
        } else if (chioce.equalsIgnoreCase("drink")) {
            return new DrinkFactory();
        }
        return null;
    }
}
```
从简单工厂都抽象工厂，完成了对产品的抽象和对工厂的抽象。  
> 那么能不能弄个**生产工厂的工厂的工厂**？ 不能，因为太过复杂的代码不易维护。

### 与简单工厂不同的是:
SnacksFactoryBuilder 的 buildFactory() 方法并不是 static 的。  
因为复杂场景下尽量不要使用 static 方法。实际上 静态方法叫做类方法， 对象方法叫做实例方法。

## 工厂模式结合 Spring 工程
工厂模式不建议使用 static 方法的另外个原因是：  
在使用 spring 框架的时候， 可以为 SnacksFactroyBuilder 加上 @Component 注解，可以让框架管理实例：  
```java
@Component 
public class SnacksFactoryBuiler {
    public SnacksFactory buildFactory(String choice) {

    }
} 
```
> 简单工厂模式的工厂类也可以去掉 static，加注解

相应的，任何需要使用工厂的地方，只需要 @Autowired 注解让框架自动注入实例即可，非常方便：
```java
@Service
public class XxxxServiceImpl implements XxxxService {
    @Autowired
    private SnacksFactoryBuilder snacksFactoryBuilder;
}
```
这样可以让工厂模式的代码与 Spring 互为一体，扩展性更好，易于维护。

## 观察者模式
有一个项目可以抓取信息派送到指定邮箱，但是这个项目只能为自己服务，难以扩展，比如天气信息不能发送多个邮箱，不能发短信。

### 基本思路：
提供一个天气服务端，谁想了解天气信息，从这个服务端订阅就行了；当天气发生变化的时候，会自动通知客户端。  
这种 订阅/通知 的场景，非常适合 观察者模式来实现。  
#### 1. 观察什么  
观察者模式的核心是要知道观察什么，什么对象发生变化了需要发出通知？  
在天气预报的项目里面，显然，天气信息是核心，天气发生变化了，需要通知大家都知道。所以，我们先抽象出天气信息对象。  
```java
import java.util.Observable;

public class WeatherData extends Observable {
    // 城市
    private String cityName;
    // 时间
    private String time;
    // 温度
    private String temp;

    // 城市固定了就不要变了
    public WeatherDate(String cityName){
        this.cityName = cityName;
    }
    
    // 打印天气信息
    public String toString() {
        return cityName + "," + LocalDate.toString() + " " + time + ", 气温: " + temp + "摄氏度。";
    } 

    public String getCityName() {
        return cityName;
    }

    public String getTime() {
        return time;
    }
    
    public String getTemp() {
        return temp;
    }

}
```
可以看见，天气类 WeatherData 继承了 Observable 类。  
Observable 类是 java 提供的，继承了就表示是核心的，需要被观察的类.  
> 记住这个写法：extend Observable
> 
这个设计与之前的模型不同的是，一个 WeatherData 代表了一个城市的天气，初始化后就不能被更改。所以去掉所有的属性的 set 方法  
WeatherData 继承了 Observable 所以为被观察者  

#### 2. 数据变化后发起通知  
时间和温度是监听的重要信息，所以增加一个新方法（上面的暂时忽略）专门来处理。  
```java
import java.yitl.Observable;

public class WeaterData extends Observable {
    /**
     * 一个城市的气温在某个时刻变化
     */
    public void changeTemp(String time, String temp) {
        if(time == null || temp == null){
            // 输入有问题，不处理
            return;
        }

        // 原数据不同，发生变化
        if(!time.equals(this.time) || !temp.equals(this.temp)) {
            //标记被观察者发生变化
            super.setChanged();
            this.time = time;
            this.temp = temp;
            // 发送信息
            super.notifyObservables("xxx");
        }
    }
}
```

在 changeTemp() 中，如果天气数据与原来不同，则会被标记变化并发出通知。  
父亲 Observable 提供的方法 setChanged() 就是标记被观察者对象发送了变化。  
父亲 Observable 提供的方法 notifyObsevers() 就是发出通知; 如果需要发送额外（不在被观察者对象里面的信息）信息，在参数里面传入信息对象就可以了，可以是 **任意** 对象。需要自己根据具体的需求场景而定。
> 本例子只是简单演示发送了一个字符串消息“温度变化已通知”
> > 如果不想发送额外信息，那就 super.notifyObservable(null)

这里新增一个 changeTemp() 方法的主要原因是，天气信息有多个字段组成，所以最好有一个统一的变更数据的方法，防止混乱。如果只有一个数据属性，直接把发通知的这段逻辑写在 setter 方法里面。  

#### 3. 谁接受通知  
需要了解天气类，就是接受通知的类，通常叫做观察者。  
> 观察者观察数据的变化  

观察者需要实现 Observable 接口，也就是 java 提供的， 实现次接口表示作为观察者。  
```java
import java.util.Observable;
import java.util.Observer;

public class WeaterObserver implements Observer {
    private String name;

    @Overried
    public void update(Observable o, Object arg){
        System.out.print(this.name + "观察到天气变化为：");
        System.out.print(o.toString());
        System.out.print(" " + arg); 
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

作为观察者，实现 Observer 接口后，要自己实现 update() 方法，方法签名是接口定义好的，属于固定写法。  
* 第一个参数就是被观察对新，被观察对象都要继承自 Observable  
* 第二个参数就是额外的信息，具体来说就是调用 super.notifyObservers() 是传入什么对象， arg 的值就是什么对象

> 如果不想额外传值，写为 super.notifyObservers(null), 那么这里的 arg 值就是 null

update() 方法的作用就是接受通知。实际上，系统在 super.notifyObservers() 发出通知后，及调用所有的观察者的 update() 方法，完成通知过程。  

当天气发生变化的时，只需要调用 changeTemp() 方法即可改变天气数据。  
```java
public class WeaterTest {
    public static void main(String[] args){
        // 在天气发生变化的时候发邮件的观察者
        WeatherObserver w1 = new WeatherObserver();
        w1.setName("邮箱观察者");

        // 在天气变化后发短信的观察者
        WeatherObserver w2 = new WeatherObserver();
        w2.setName("短信观察者");

        WeatherData data = new WeatherData("安徽");

        //添加观察者
        weatherData.addObserver(w1);
        weatherData.addObserver(w2);

        // 气温变化
        weatherData.changeTemp("11:21", "32.2");
        weatherData.changeTemp("12:22", "34.2");
    }
}
```
观察者可以有多个。观察者对象与被观察者对象谁先 new 出来都可以，但是必须先调用 addObserver() 把观察者对象实例添加到被观察者（天气数据）实例中，然后调用自定义的 changeTemp() 方法变更天气，才能触发自动通知。  

### 总结  
这里主要学会使用 Observable 父类和 Observer 接口提供的几个方法。

![观察者模式](E:/project/xxx/Concurrent_Coding/image/image6.png)

这个与工厂模式的思想相同的是，观察者模式让 **观察者** 和 **被观察者** 双方的耦合度降低
> 观察者不需要知道数据变化后需要通知给谁，发出通知即可；而且不需要知道谁收到了通知

## 为什么需要那么对线程

计算机中，每个软件运行一次，即启动了一个进程。 而每个统筹方法工序，就是一个进程。统筹方法工序的工作模式，称为多线程。  
> 普通工序按顺序执行，称之为单线程。相当于一个进程中只有一个线程。

又比如，我们已经学过 Spring，启动一个 SpringBoot 项目，就是启动了一个进程。而一个个用户访问首页的过程，都是一个个进程。 SpringBoot 的项目工作模式是多线程的。  
如果没有多线程的话，一个个用户只能按顺序。

## 继承 Tread 类
情景 ：张三，李四 到银行取钱。银行有多台 ATM 。   
### 实现
#### 线程类
可以继承 Java 里面的 Thread 类实现线程类。
> Thread 的完整类名是 java.lang.Thread。java.lang 包里面的所有类都可以省略 import
```java
public class Person extends Therad {
    @Override
    public void run() {
        try {
            System.out.println(getName() + "，开始取钱");
            Tread.sleep(200);
        } catch (InterruptedException e) {
            e.printStrackTrace();
        }
        System.out.println(getName() + "，取钱完成");
    }
}
```
继承 Tread 方法后，需要重写 run() 方法，注意必须是修饰为 public void，方法是没有参数的。  
> 加上 @Override 注解，会让系统自动检测 public void run() 方法定义有没有写错 

线程类的作用就是完成一段相对独立的任务。银行上班提供服务是进程，某用户取钱的小任务是线程。  
这里用 Thread.sleep(200) 模拟取钱的过程。 sleep() 方法 (注意是静态方法) 的作用是让线程睡眠，暂时不再继续执行，交出 CPU，让 CPU 去执行其他任务。 sleep() 方法参数就是毫秒数。 200 表示 200 毫秒， 超过这段时间后继续执行程序。  

### 运行线程
线程需要调用 start() 方法才能启动。  
```java
public class Bank {
    public static void main(String[] args){
        Person thread1 = new Person();
        thread1.setName("张三");

        Person thread2 = new Person();
        thread2.setName("李四");

        thread1.start();
        thread2.start();
    } 
}
```
类图：  
![类图](E:/project/xxx/Concurrent_Coding/image/image7.png)  

Thread 父类中有 name 属性，但是 private ，所以可以调用 setName() 方法来设置线程名称，通过 getName() 方法为线程设置名字，通过 getName() 就知道是哪个线程在运行。
> 当然，根据具体的需求，线程类也可以增加更多其他属性。只要不遗漏 run() 方法。

我们需要了解的是，线程类的 run() 方法是系统调用的 start() 后自动执行的，编程时不需要调用 run() 方法。但永远无法知道系统在什么时候调用，是立即执行还是延迟一会执行调用，都是系统自动决定，无法干涉。  

不仅不能确定系统什么时候调用 run() 方法，也不能确定调用顺序一定是代码中的 start() 方法的书写顺序。  

我们需要记住这个特性，但是不要纠结。多线程机制执行很复杂。甚至与 CUP 有关。

## 实现 Runnable 接口

继承 Thread 类定义多线程程序后，缺点就比较明显了，它无法在继承其他类，因为 java 是单继承，只允许继承一个类，这会导致程序的课扩展性大大降低。  
所以定义多线程程序，优先采用第二种方法：实现 java.lang.Runnable 接口。  
> 尽量采用面向接口编程的方法  

仍然以张三李四取钱的多线程任务未例子，代码需要修改， Person 改为实现 Runnable 接口：
```java
public class Person implements Runnable {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        try {
            System.out.println(name + " 开始取钱");
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(name + " 取钱完毕");
    }
}
```

在 Runnable 里面有一个待实现的接口 run() 方法，要自己补充属性了。  
无论是 Thread 类还是 Runnable 接口，run() 方法都是系统 **适时** ， **自动** 执行的，大家只需要在方法类实现业务功能就好。  
> Thread.sleep() 这个静态方法仍然可以使用，记得参数是毫秒级别。

实现了 Runnable 接口的线程类，还需要包装到 Thread 类的实例中运行:
```java
public class Blank {
    public static void main(String[] args) {
        Person person1 = new Person();
        person1.setName("张三");
        Thread thread1 = new Thread(person1);

        Person person2 = new Person();
        person2.setName("李四");
        Thread thread2 = new Thread(person2);
        
        thread1.start();
        thread2.start();
    }
}
```

Thread 实例 (new Thread(person1)) 相当于调度器，触发线程任务执行，线程里面的实例 (new Person()) 就相当于任务。任务是不能自己启动的，需要被调度。  
 通过类图进行理解:
 ![类图](E:/project/xxx/Concurrent_Coding/image/imag8e.png)

 ## 线程安全

 情景： 春运期间的火车票总是很紧张，江西省南昌市火车站有 4 个售票窗口，列车只剩 30 章车票了。
 根据需求分析应该有三类：火车站，售票窗口，票。  
但是与之前不同，火车票的总数量是有限的，无论多少窗口售票，都是从总的 30 张票中扣减。

### 车票类  
车票类的作用是控制车票的总数。每售卖一次，票数都减一。  
```java
public class Tiket {
    private int count = 30;

    public void sell() {
        count--;
        System.out.println(Thread.currentThread().getName() + ": 卖出一张，还剩" + count + " 张票");
    }

    public int getCount() {
        return count;
    }
}
```

### 售票窗口类
售票窗口就是线程类，以多线程的方式售票。售票简单来说就是循环减一即可。当然，票数为 0 的时候不再售卖。  
这里假定打印票据需要点时间，售卖一张票的休息时间为 100 毫秒。
```java
public class TicketWindow implements Runnable {
    private Ticket ticket;

    public TicketWindow(Ticket ticket) {
        this.ticket = ticket;
    }

    @Override
    public void run() {
        while (ticket.getCount() > 0) {
            try{
                Thread.sleep(100);
                ticket.sell();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 火车站类
火车站类有四个窗口，所以启动四个线程。  
```java
public class TrainStation {
    public static void main(String[] args){
        Ticket ticket = new ticket();

        for (int i = 1; i <= 4; i++){
            TicketWindow  office = new TicketWindow(ticket);
            Thread thread =new Thread(office);
            thread.setName("售票窗口" + i);
            thread.start();
        }
    }
}
```

新知识，这里我们调用了 Thread.currentThread().getName();

Thread.currentThread() 返回的是当前正在运行的线程实例对象，因为一个车票的实例，是多个线程中的，需要知道具体是哪个窗口售卖车票，就可以使用这个方法。  

再调用线程的 getName() 方法可以取得当前线程实例的名称。这个名称可以使用 setName() 方法指定一个名称，如果没有指定，系统会默认指定一个名称。

**但是这会有很大的问题**  
1. 余量错乱，可能出现不同进程计算出来余量相同。  
2. 余量可能是负数

### 问题分析
前面章节知道， 线程的调用机制是很复杂的，是可以同时执行的，特别是多核 CPU 的情况下，多个进程同时（并行）执行的概率会很高。  
![并行理解图](E:/project/xxx/Concurrent_Coding/image/image9.png)

这种 **多线程** 运行 **同一个实例对象**（ticket）的情况下，**修改了同一个变量**（调用 sell() 方法同时执行 count-- 语句），后果是不可预料的所以会打出余量错乱甚至相同的情况。  

#### 问题一的解决方案
**多个线程**操作**同一个数据** 的时候，发生冲突的现象，就叫做**线程不安全**  
在java 中，可以用 synchrinized 关键字来解决余量错乱问题。  
synchronized 加载方法上紧跟着 public:
```java
public class Ticket {
    public synchronized void sell() {
        count--;
        System.out.println(Thread.currentThread().getName() + "：卖出一张，还剩" + count + " 张");
    }
}
```

synchronized 也叫做**同步锁**, 表示此方法是锁定的，同一时刻只能由一个线程执行此方法。  
![同步锁示意图](E:/project/xxx/Concurrent_Coding/image/image10.png)

先判断 sell() 方法有没有被上锁：
* 如果上锁，说明有其他线程正在调用 sell() 方法，必须等待其他线程对 sell() 方法调用结束后才能执行 sell() 方法；
* 如果没有上锁，则执行 sell() 方法。开始执行时对次方法加锁，不允许其他线程执行，方法调用完毕后解锁   

synchronized 相当于保护了关键方法，不允许同时执行，必须一个个执行。  
> 一个个执行不是安按照编码的顺序执行，而是由系统自动决定在解锁后有哪个线程执行方法。这种多个线程等待的过程叫做竞争。  

#### 问题二的解决方法

当然车辆余数为一的时候，四个进程可能同时判断  
ticket.getCount() > 0 条件成立时，所以真正执行 sell() 的时候会出现继续减 1，出现负数。  
所以，对于 sell() 方法来说，**必须保证逻辑的完整性，不能依赖其他类的条件判断，自己就不判断了。  
```java
public class Ticket {
    public synchronized void sell() {
        if(count > 0 ){
            count--;
        }
        System.out.pringln(Thread.currentThread().getName() + "：还剩" + count + " 张票");
    }
}
```


## synchronized 的使用场景  



## 线程池 
使用 Runnable 接口开发多线程程序，更符合面向对象的习惯，但是随之而来的问题是，对象太多。  
为了方便，只演示 4 为同学注册的场景。记得上面有代码：
```java
public class StudentIDTest {
    public static void main(String[] args) {
        for (int i = 1; i <= 4; i++){
            Student s = new Student();
            s.setName("学生" + i);
            Register register = new Register(s);
            Thread thread = new Thread(register);
            thread.sstart();
        }
    }
}
```

但是实际上，入学的人数肯定不是4个那么少。如果有一千位同学，那么意味着程序要额外 new Thread(register) 一千多次。对象除了创建需要消耗计算机 CPU，内存等资源，对象还会被销毁（系统自动做），销毁也是耗费资源的。  
那我们需要做一些优化了，要做到复用 Thread 对象。不必要每次都出出来创建新的对象。  
**Java 提供了 线程池 技术**
### 线程池基础概念  
所谓线程池，顾名思义，就像是一个池子，里面装满了线程，随用随取。线程可以被复用，一个线程可以执行 A 任务，也可以执行 B 任务。于是线程不需要频繁的创建和销毁。
> new Thread(register) 意味着一个线程对象只能执行一个任务，而线程池让线程与任务分离，不再紧密绑定。  

线程池的另外一个重要的概念是，线程池并不是无限大的（因为计算机的 CPU，内存等资源毕竟有限） 所以线程池中存在的线程数也是有限的，这就意味着能同时运行的任务数是有限的，其它过剩的任务就需要**排队**。 待任务完成，有空闲线程后，才执行继续的任务。
![线程池](xxx/Concurrent_Coding/image/image15.png)

### 线程池创建
线程池核心代码：
```java
import org.apache.commons.lang3.concurrent.BasicThreadFactory;
import java.util.concurrent.*;

public class StudentIDTest {
    // 线程工厂
    private static final ThreadFactory namedThreadFactory = new BasicThreadFactory.Builder()
        .namingPattern("studentReg-pool-%d")
        .daemon(true)
        .build();

    // 等待队列
    private static final BlockingQueue<Runnable> workQueue = new LinkedBlokingQueue<Runnable>(1024);

    // 线程池服务
    private static final ThreadPoolExecutor XECUTOR_SERVICE = new ThreadPoolExecutor(
        20,
        200,
        30,
        TimeUnit.SECONDS,
        workQueue,
        namedThreadFactory,
        new ThreadPoolExecutor.AbortPolicy()
    );

    public static void main(String[] args){

    }
}

这里的 BasicThreadFactory 需要一个依赖库，这个库也是经常使用的：
```
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-lang3</artifactId>
  <version>3.10</version>
</dependency>
```

创建线程池代码，基本上都是固定写法

### 创建线程池工厂实例
我们已近在上面学过工厂模式，顾名思义，线程工厂就是用生产线程池中的这些线程的。
```java
new BasicThreadFactory.Build()
    .namingPattern("studentReg-pool-%d")
    .daemon(true)
    .build();
```

唯一需要注意的是， namingPattern() 方法是定义线程名字的格式，相当于线程名模板，需要自己根据具体业务需求把"studnetReg" 该掉。  
studentReg 表示当前线程运行的是学生注册的任务。同学们自己替换成合适的，不要所有的任务都用 studentReg。 例如商品发布任务可以命名为 “offer-pool-%d”, 自己能理解意思就好。  
> Builder() 不是方法，是构造函数，BasicThreadFactory 类中有一个内部类 Builder。
new basicThreadFactory.Builder() 创建了内部类的实例对象。

### 创建线程等待队列实例  
线程池没有空闲的线程时，其他的任务，就需要在队列中等待。  
> 可以类比一下： 春运期间坐火车的人太多了，火车站候车厅容量很有限，很多乘客就在候车大厅外排队等候。  
如果机器性能很好， CPU 核数多（6，8核），内存大，队列可以大一些： new LinkedBlockingQueue<Ruunable>(2048)。 构造函数的参数表示能排队的任务个数。如果机器性能一般，CPU 核数少（1，2核），内存，队列就小一些：
new LingkedBlockingQueue<Runnable>(512)  
一般来说， new LinkedBlockingQueue<Runnable>(1024) 也还可以，算比较适中的。

### 创建线程池实例  
ThreadPoolExecutor 构造函数参数较多，：
```java
ThreadPoolExecutor(
    线程池初始化核心线程数量，一般是两位数，通常不大,
    线程池最大线程数，计算机性能好就大一些，否则小一些，通常不超过 200,
    线程池中的线程数超过核心线程数时，如果一段时间后还没有任务指派，就回收。想回收就填0，一般30,
    第三个参数的时间单位。30 + TimeUnit.SECONDS 表示30 秒,
    等待队列实例已经创建过了,
    线程工厂实例，已近创建过了,
    任务太多，超出队列的容量时，用什么样的策略处理。一般用 AbortPolicy 表示拒绝，让主程序自己处理
    );
```
多线程编程特别需要注意的就是：防止线程数过多把系统搞崩。所以用线程池可以做更加精确的控制，否则难以控制。   
而实际编程工作，需要想办法保证不要创建太多的任务，要有控制，而不是只要管创建任务扔进线程池。比如可以采用分页的思想，分批处理。一批只处理十几个。还要考虑执行时间，能不能快速结束。不要让计算机积累太多任务，保证线程等待队列能容纳。  

### 使用线程池运行任务
```java
publiic class StudentIDTest {
    public static void main(Stirng[] args){
        // 创建学生集合
        for (int i = 1; i <= 2000; i++){
            Student s = new Studnet();
            s.setName("学生", + i);
            Register register = new Register(s);
            // 传入 Runnable 对象，运行任务
            EXECUTOR_SERVICE>execute(register);
        }
    }
}
```
只要执行线程池对象的 execute() 方法，把实现了Runnable 接口实例对象传入即可。
### 等待线程池执行
使用 main() 函数运行多线程程序，往往会出现多线程还没有运行完成， main() 就退出了。
因为 for 语句后面没有代码了，main() 函数执行完毕，整个 JVM 就结束了。我们可以在最后加上 Thread.sleep

