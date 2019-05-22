---
title: drools规则格式说明
date: 2019-05-12 02:27:31
categories: 
    - drools学习
tags: 
    - 规则引擎
    - rules engine
    - Drools
---

# drools学习笔记

## 规则引擎的优势

> ### Declarative Programming

> Rules make it easy to express solutions to difficult problems and get the solutions verified as well. Unlike codes, Rules are written in less complex language; Business Analysts can easily read and verify a set of rules.

### 声明式编程

规则可以简单的表示一个复杂问题的解决方案，并且验证解决方案。不像编码，规则不需要写复杂的语言，业务分析可以简单的读取并且验证

### 逻辑与数据分离

数据存放在对象域中，业务逻辑存放在规则中，根据项目的类型，这种分离的类型可以是有利的。

> ### Logic and Data Separation

> The data resides in the Domain Objects and the business logic resides in the Rules. Depending upon the kind of project, this kind of separation can be very advantageous.

### 速度和可扩展性

编写drools的Rete OO算法是经过验证的算法，在Drools的帮助下，你的程序可以变得有扩展性，如果有频繁改变请求，可以添加新的规则而无需修改现有的规则

> ### Speed and Scalability

> The Rete OO algorithm on which Drools is written is already a proven algorithm. With the help of Drools, your application becomes very scalable. If there are frequent change requests, one can add new rules without having to modify the existing rules.

### 知识集中化

通过使用规则，你可以创建一个可以执行基于知识的知识库。是业务政策中单一事实，理想情况下，规则可以作为一个文档可以读的。

> ### Centralization of Knowledge

> By using Rules, you create a repository of knowledge (a knowledge base) which is executable. It is a single point of truth for business policy. Ideally, Rules are so readable that they can also serve as documentation.

<!--more-->
### 工具集成

像Eclipse这样的工具提供了编辑和管理规则并且得到实时反馈，验证和内容帮助。审计和调试工具也是可以获得的。

> ### Tool Integration

> Tools such as Eclipse provide ways to edit and manage rules and get immediate feedback, validation, and content assistance. Auditing and debugging tools are also available.

------

## 规则文件

规则文件是一个以`.drl`扩展的文件，在DRL文件中，你可以有多个规则(rules),查询(querys)和函数(functions),同样也有一些资源的申明像:import,globals和attributes，被设计和使用在你的规则和查询中。

规则文件的文件结构：

```
package package-name

imports

globals

functions

queries

rules
```

文件中元素的顺序并不重要，但是`package`元素必须放在文件的开头处，文件出现的第一个元素必须是`package`元素

### 规则的粗略结构

```java
rule "name"
    attributes
    when
        LHS
    then
        RHS
end
```

非常简单，大部分的标点都是可以不需要的，双引号的`"name"`都是可选的，LHS是规则的条件部分，RHS是规则的特定方言语义代码执行的部分，为什么叫特定方言语义代码，是因为它并不是使用的是一些Java的常用语言，而是一种方言，但是都是通过Java来执行的。

## 关键字

从drools5引入了硬关键字和软关键字两种概念，关键字的概念，有点类似Java中的保留字。

硬关键字是保留的，你不能在你的域对象(domain Object)，属性(properties)，方法(methods)，函数(functions)等其他元素中使用硬关键字

一下就是在定义规则中避免使用的硬关键字列表：

* `true`
* `false`
* `null`

软关键字只有在正确的上下文位置中被识别，你可以在任意的位置中使用这些关键字，但是可能的话请尽量避免使用这些软关键字，具体的软关键字列表如下：

- `lock-on-active`
- `date-effective`
- `date-expires`
- `no-loop`
- `auto-focus`
- `activation-group`
- `agenda-group`
- `ruleflow-group`
- `entry-point`
- `duration`
- `package`
- `import`
- `dialect`
- `salience`
- `enabled`
- `attributes`
- `rule`
- `extend`
- `when`
- `then`
- `template`
- `query`
- `declare`
- `function`
- `global`
- `eval`
- `not`
- `in`
- `or`
- `and`
- `exists`
- `forall`
- `accumulate`
- `collect`
- `from`
- `action`
- `reverse`
- `result`
- `end`
- `over`
- `init`

如果你必须使用硬关键词，那么你可以通过如下方法对其进行转义：

```java
Holiday( `true` == "yes" ) // please note that Drools will resolve that reference to the method Holiday.isTrue()
```

## 注释

### 单行注释

单行注释使用`//`。实例如下

```java
rule "Testing Comments"
when
    // this is a single line comment
    eval( true ) // this is a comment in the same line of a pattern
then
    // this is a comment inside a semantic code block
end
```

> `#`号注释已经不能使用了。

### 多行注释

多行注释，结构如下：

![多行注释](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/multi_line_comment.png)

示例如下:

```java
rule "Test Multi-line Comments"
when
    /* this is a multi-line comment
       in the left hand side of a rule */
    eval( true )
then
    /* and this is a multi-line comment
       in the right hand side of a rule */
end
```

## 错误信息

从drools5开始采用了标准化的错误信息，你可以很轻松快速的找到和解决问题，并且提供了一些关于处理问题的提示。

### 信息格式

![错误信息格式](D:\文档\drools学习笔记\error_message.png)

**1st block**：错误码标识。

**2nd block**: 行和列信息。

**3rd block**: 问题的文字描述。

**4th block**: 通常用来表明发生错误的规则(rule)，函数(function)，模板(template)和查询(query)，该块并不是强制性的

**5th block**:识别发生错误的模式，该块并不强制。

### 错误信息描述

#### 101:没有可行的替代方案(No viable alternative)

#### 102:输入不匹配(Mismatched input)

#### 103:谓词失败(Failed predicate)

#### 104:尾部分号不被允许(Trailing semi-colon not allowed)

#### 105:过早退出(Early Exit)

## package

![包的铁路图解](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/package.png)

### import

![import铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/global.png)

import 有点像Java的import的声明一样，需要给出对象的完全的限定路径，和你需要在规则中使用的对象名称。drools可以相同名字的Java包中自动导入类。通过也会自动导入java.lang.下面的类。

### global

![global铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/global.png)

`global`用来定义全局变量，`global`能够使应用对象（application object）用于规则中，通常他们为规则的使用提供服务和数据，特别在规则结果中使用程序服务。并且从规则结果中返回数据。像日志(logs)和值(value)。或则让规则与程序进行交互，执行回调。`global`不会被插入到工作寄存器中，规则引擎无法判断`global`中的值是否发生了变化，也无法跟踪这些变化，所以在使用`global`变量要十分小心，如果乱用`global`，可能会使程序造成意想不到的错误。所以`global`最好是对常量不变量进行定义。

1. 在你的drl文件中使用全局变量，示例：

```java
global java.util.List myGlobalList;

rule "Using a global"
when
    eval( true )
then
    myGlobalList.add( "Hello World" );
end
```

2. 设置全局变量到工作寄存器中，最好的实践就是在进行完事实断言（asserting of fact）之前就设置完所有的全局变量到工作寄存器中。例：

```java
List list = new ArrayList();
KieSession kieSession = kiebase.newKieSession();
kieSession.setGlobal( "myGlobalList", list );
```

> 在定义全局变量中，全局变量并没有被设计成在规则与规则之间传递数据，也不是以此目的来使用全局变量，如果你想要在rules之间传递数据，断言数据作为事实(assert data as fact)到工作寄存器中。

> 更改全局变量的数据一定要小心，因为Drools engine不知道`global`全局变量的修改，所以无法对其做出反应。

## function

![function铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/function.png)

`function`与Java的帮助有所不同，不能像Java中的帮助类那样做任何的事情。`fcuntion`最主要的功能就是将所有的逻辑放在一起。对逻辑基本一样，只是参数有些不同的过程进行封装。

一个普通的`function`示例：

```java
function String hello(String name) {
    return "Hello "+name+"!";
}
```

尽管`function`不是java中的一部分，但是他还是被使用。参数可以定义可以不定义，返回值跟普通的方法差不多。

drools支持帮助类的静态方式调用如`Foo.hello()`,同时也支持函数导入的方式，例如：

```java
import function my.package.Foo.hello
```

你可以在语义代码块和结果中使用函数，例如:

```java
rule "using a static function"
when
    eval( true )
then
    System.out.println( hello( "Bob" ) );
end
```

## 类型声明(Type Declaration)

![元_数据](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/meta_data.png)

![输入声明](D:\文档\drools学习笔记\type_declaration.png)

类型的声明有两个目标：允许一个新类型的声明，允许对类型的元属性的声明。

* **Declaring new types**
* **Declaring metadata**

### 声明新类型(Declaring New Types)

使用`declare`对类型定义的开始，使用`end`表明新类型声明结束，新的事实(Fact)必须有一个字段列表。否则engine 就会去查找在类路径已存在的事实类(fact class)，如果没有找到，就会报错。

*示例：定义一个新的事实类型(fact type):Address*

```java
declare Address
   number : int
   streetName : String
   city : String
end
```

属性的类型可以是一个有效的java类型，用户创建的其他的类，或者之前声明的其他的事实声明。

声明一个包含其他的事实类型的类型，如:

```java
declare Person
    name : String
    dateOfBirth : java.util.Date
    address : Address
end
```

如果你不想在属性类型声明中使用完全限定名称的话，你可以使用`import`,例如

```java
import java.util.Date

declare Person
    name : String
    dateOfBirth : Date
    address : Address
end
```

当你生成一个新的事实类型，他将会在编译阶段生成一个字节码来实现Java类表示这个事实类型，他将生成一个one-to-one JavaBean映射到事实声明中。一下为通过事实声明生成的Java类的示例:

```java
public class Person implements Serializable {
    private String name;
    private java.util.Date dateOfBirth;
    private Address address;

    // empty constructor
    public Person() {...}

    // constructor with all fields
    public Person( String name, Date dateOfBirth, Address address ) {...}

    // if keys are defined, constructor with keys
    public Person( ...keys... ) {...}

    // getters and setters
    // equals/hashCode
    // toString
}
```

由于他是一个简单的Java类，所以，我们可以规则中使用他，就像其他的事实(fact)一样。

```java
rule "Using a declared Type"
when
    $p : Person( name == "Bob" )
then
    // Insert Mark, who is Bob's mate.
    Person mark = new Person();
    mark.setName( "Mark" );
    insert( mark );
end
```

### 声明一个枚举类型(Declaring enumerative types)

一个简单的枚举类型的声明。

```java
declare enum DaysOfWeek
   SUN("Sunday"),MON("Monday"),TUE("Tuesday"),WED("Wednesday"),THU("Thursday"),FRI("Friday"),SAT("Saturday");

end
```

编译器将会编译一个有效的Java enum，包含静态方法`valueOf()`和`values()`,同样也编译了实例方法`ordinal()`,`compareTo()`和`name()`。

复杂的枚举也是部分支持的，同样可以生成常用类型的内部字段，注意在drools 6.x版本后，不再支持使用其他声明和枚举类型的字段。

```java
declare enum DaysOfWeek
   SUN("Sunday"),MON("Monday"),TUE("Tuesday"),WED("Wednesday"),THU("Thursday"),FRI("Friday"),SAT("Saturday");

   fullName : String
   
end
```

在规则中使用枚举:

```java
rule "Using a declared Enum"
when
   $p : Employee( dayOff == DaysOfWeek.MONDAY )
then
   ...
end
```

### 声明元数据（Declaring Metadata）

元数据可以分配到不同的drools机构中:fact types,fact attributes 和 rules中，drools中使用`@`介绍元数据。格式如下:

```java
@metadata_key( metadata_value )
```

drools允许任意元数据属性的声明，但是在引擎中一些元数据有特殊的意义。而另外的可以在运行时的查询中可以获得。可以在事实类型(`fact types`)中声明，也可以在事实属性(`fact attributes`)中声明。放在fact attributes之前，他将被分配到`fact type`中，在`fact attributes`后声明，就会分配到该`fact attributes`中.

```java
import java.util.Date

declare Person
    @author( Bob )
    @dateOfCreation( 01-Feb-2009 )

    name : String @key @maxLength( 30 )
    dateOfBirth : Date
    address : Address
end
```

#### 预定义类级别注释(Predefined class level annotations)

预定义的的注释有自己预定义的含义，并被engine解释成相应的语义后执行。

##### @role(<fact|event>)

`@role`注释告诉`engine`怎么处理那种类型的实例，是通过普通的`fact`，还是`event`的方式。

* `fact`：默认将那种类型作为`fact`进行处理。
* `event`：声明类型作为`event`进行处理。

下面是一个`fact type`为`StockTick`被声明为`event`进行处理的例子。

```java
import some.package.StockTick

declare StockTick
    @role( event )
end
```

与`fact`的声明一样，同样可以在`DRL`本身中进行声明`fact type`。并且设置为一个`event`角色。

```java
declare StockTick
    @role( event )

    datetime : java.util.Date
    symbol : String
    price : double
end
```

##### @typesafe(< boolean >)

默认是开启了类型安全的情况下进行编译的。`@typesafe(false)`提供了一种通过允许回退的手段来覆盖这种行为，为了类型的不安全评估，所有的约束都将作为`MVEL`约束被生成并动态执行。这个可能在处理没有泛型和混合类型集合的集合时是重要的。

##### @timestamp(< attribute name >)

每一个`event`都会有一个相关的`timestamp`。当`event`被插入到工作寄存器中的时候，会从`Session Clock`中读取一个`timestamp`并且会分配给`event`。如果用户想去从`event`中获取`timestamp`，而不是直接从`Session Clock`中去读取他。就可以使用`@timestamp`。

```java
@timestamp( <attributeName> )
```

具体使用如下:

```java
declare VoiceCall
    @role( event )
    @timestamp( callDateTime )
end
```

##### @duration(&lt;attribute name&gt;)

drools提过两种事件语义(event semantics):时间点事件(point-in-time event)和基于时间间隔的时间(interval-based event)。时间点的事件的间隔时间为0的话，他将被作为时间间隔事件。所有事件的默认持续时间(duration)为0。

```java
@duration( <attributeName> )
```

示例如下:

```java
declare VoiceCall
    @role( event )
    @timestamp( callDateTime )
    @duration( callDuration )
end
```

##### @expires(&lt;time interval&gt;)

> **警告**：这个标记只能用在engine是以`STREAM`mdel 运行下。

设置event过期的时间。

```java
@expires( <timeOffset> )
```

`timeoffset`设置模板如下:

```java
[#d][#h][#m][#s][#[ms]]
```

`[]`表示可选参数，`#`表示需要填写的数字

示例如下:

```java
declare VoiceCall
    @role( event )
    @timestamp( callDateTime )
    @duration( callDuration )
    @expires( 1h35m )
end
```

##### @propertyChangeSupport

可以对符合JavaBean标准的`fact`，进行注释`@propertyChangeSupport`,引擎可以自己监听`fact`的改动。不推荐使用在drools4 API方法insert()中使用Boolean参数，并且drools-api已经不存在这个参数了。

示例如下

```java
declare Person
    @propertyChangeSupport
end
```

##### @propertyReactive

是类型的属性可反应

##### @serialVersionUID

设置`seialVersionUID`提升`kie session`的兼容性。如下

```java
declare MyClass
  @serialVersionUID( 42 )
  name : String
end
```

#### 属性级别预定义注释(Predefined attribute level annotations)

##### @key

使用`@key`有两个主要的影响。

* 标记`@key`的属性会被作为主要的标识符所使用。在生成Java类的`equals()`和`hashcode()`方法时，标记了`@key`的属性会被加入到比较中。
* drools会生成一个包含有`@key`属性作为构造函数的参数。

例如：

```java
declare Person
    firstName : String @key
    lastName : String @key
    age : int
end
```

包含`@key`的属性作为构造函数的参数如下：

```java
Person person = new Person( "John", "Doe" );
```

##### @position

`@position`可以指定属性的位置。在对位置参数进行操作的时候我们需要使用参数名来映射到该参数的位置上，但是我们同样可以使用位置进行映射，如Person(name == "mark") 可以使用位置映射为Person("mark";),`;`号对于engine知道这是位置参数是重要的。否则我们可以在假设他是布尔表达式可以在分号后面被解释。你可以使用分号混合使用位置和命名参数的方式，

未使用`@position`

```java
declare Cheese
    name : String
    shop : String
    price : int
end
```

默认的顺序就是属性的声明的顺序，可以使用`@position`覆盖默认的顺序

```java
declare Cheese
    name : String @position(1)
    shop : String @position(2)
    price : int @position(0)
end
```

`@position`的注释可以在类路径上的原始POJOs上。目前仅仅是类的字段可以注释，继承的类也支持。但是目前不支持接口方法。

```java
Cheese( "stilton", "Cheese Shop", p; )
Cheese( "stilton", "Cheese Shop"; p : price )
Cheese( "stilton"; shop == "Cheese Shop", p : price )
Cheese( name == "stilton"; shop == "Cheese Shop", p : price )
```

`@position`也能够被继承，如果父级的`@position`与子级中的`@position`的值一致,则父级有优先权的声明次序。如果没有声明`@postition`那么他就会在声明了`@position(value)`,`value`值存在的空隙中进行位置的声明。例如:

```java
declare Cheese
    name : String
    shop : String @position(2)
    price : int @position(0)
end

declare SeasonedCheese extends Cheese
    year : Date @position(0)
    origin : String @position(6)
    country : String
end
```

在这个例子中，字段的次序为：price (`@position(0)`)，year(`@position(0)`)，name，shop(`@position(2)`)，country，origin(`@position(6)`)

#### 为已存在的类型声明元数据(Declaring Metadata for Existing Types)

对以存在的类型进行元数据声明，唯一不同的就是在不需要声明字段。

例如，通过`import`方式进行声明

```java
import org.drools.examples.Person

declare Person
    @author( Bob )
    @dateOfCreation( 01-Feb-2009 )
end
```

除了使用import导入已存在的类型，还可以在`declare`应用类型的完全限定名称。如下

```java
declare org.drools.examples.Person
    @author( Bob )
    @dateOfCreation( 01-Feb-2009 )
end
```

### 已经声明的类型的参数构造方法（Parametrized Constructors for declared types）

类型声明如下:

```java
declare Person
    firstName : String @key
    lastName : String @key
    age : int
end
```

编译器将会隐式的建立3个构造函数，一个是没有参数的构造函数，一个是只包含了`@key`的构造函数，第三个是包含所有的构造函数。如下：

```java
Person() // parameterless constructor
Person( String firstName, String lastName )
Person( String firstName, String lastName, int age )
```

### 无类型安全的类(Non typesafe classes)

具体说明请参见[`@typesage`](#@typesafe(< boolean >))

### 从应用程序代码中访问声明了的类型(Accessing Declared Types from the application code)

声明的类型，一般是用在规则之间的。当然应用程序的又可以访问和处理规则声明的事实(fact)。在对规则的管理中，特别在包裹drools engine 和提高级别，通过了用户的接口。

drools提供了处理大多普通的事实(fact)的API接口，使得应用程序能够做一些想做的操作。

最重要的是知道他被声明在那个包当中，

```java
package org.drools.examples

import java.util.Date

declare Person
    name : String
    dateOfBirth : Date
    address : Address
end
```

声明的类的实现是在`Kie base`编译阶段生成的，所有他不能被应用程序直接引用，但是可以通过drools提供的接口，来处理这些声明的事实(fact)。如下:

```java
// get a reference to a KIE base with a declared type:
KieBase kbase = ...

// get the declared FactType
FactType personType = kbase.getFactType( "org.drools.examples",
                                         "Person" );

// handle the type as necessary:
// create instances:
Object bob = personType.newInstance();

// set attributes values
personType.set( bob,
                "name",
                "Bob" );
personType.set( bob,
                "age",
                42 );

// insert fact into a session
KieSession ksession = ...
ksession.insert( bob );
ksession.fireAllRules();

// read attributes
String name = personType.get( bob, "name" );
int age = personType.get( bob, "age" );
```

这个API还提供了其他的有帮助的方法，例如一次性设置所有的属性，从`map`中读取值，或则一次性读取左右的数据到`map`中。

### 类型声明`extends`(Type Declaration 'extends')

在DRL规则中，也可以声明一个子类型，通过`extends`声明子类型，就像在Java中使用一样，声明类型请看前章节[类型声明](#类型声明(Type Declaration))，示例如下：

```java
import org.people.Person

declare Person end

declare Student extends Person
    school : String
end

declare LongTermStudent extends Student
    years : int
    course : String
end
```

## 规则(rule)

![规则接口](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/rule.png)

当在`LHS`里面条件满足的话，就执行在`RHS`中的语句，`LHS`(left hand side),`RHS`(right hand side)。

规则不是在某个时间，或者特定的评审顺序(evaluation sequence)被执行，他是可以在任何时间下，只要满足条件，他都会被执行。

`rule`必须有一个`name`。`name`必须是唯一的。如果在相同的DRL文件中定义了两个相同名字的rule，他会在载入的时候报错。如果在这个包添加一个以存在规则名称的DRL，它会替换之前的的规则。如果规则名要使用空格，请给规则名添加一个双引号。最好是使用双引号。

属性(`attribute`)可选的，最好是每个`attribute`一行

跟在`when`和`then`后面的`LHS`和`RHS`最好换行。规则允许嵌套(nested)。

规则语法如下：

```java
rule "<name>"
    <attribute>*
when
    <conditional element>*
then
    <action>*
end
```

一个简单的规则:

```java
rule "Approve if not rejected"
  salience -100
  agenda-group "approval"
    when
        not Rejection()
        p : Policy(approved == false, policyState:status)
        exists Driver(age > 25)
        Process(status == policyState)
    then
        log("APPROVED: due to no objections.");
        p.setApproved(true);
end
```

### 规则属性(Rule Attributes)

规则属性提供了以声明的方式影响规则的行为。一些非常简单，而另一些作为复杂子系统的一部分例如`ruleflow`。

![规则属性的铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/rule_attributes.png)

`no-loop`

​	默认值:false

​	类型:boolean

​	当规则的结构事实导致该结果被再次执行，从而进入了无限的循环中。设置`no-loop`为真可以跳过激活的其他规则的创建。

`ruleflow-group`

​	默认值:N/A

​	类型:String

​	`ruleflow`是drools的一个功能，他能够让你控制规则的执行，在该组下的规则只有在该组处于激活状态的情况下才会被执行。

`lock-on-active`

​	默认值:false

​	类型:boolean

`salience`

​	默认值：0

​	类型：Integer

​	没有规则都有一个默认值为0的`salience`，该值可以为正，可以为负。在激活序列中，`salience`高的将会被赋予高的优先权。

​	drools提供动态的`salience`，你可以是使用绑定参数进行使用。

例如

```java
rule "Fire in rank order 1,2,.."
        salience( -$rank )
    when
        Element( $rank : rank,... )
    then
        ...
end
```

`agenda-group`

​	默认值:MAIN

​	类型:String

​	agenda groups 运行用户划分agenda，从而提供更多的执行控制，只有在获取了焦点的`agenda-group`中的规则才被允许执行。

`auto-focus`

​	默认值:false

​	类型:boolean

​	当设置了`auto-focus`为true并且规则的`agneda-group`没有获取焦点的规则被激活，他会获取焦点，并且规则运行被执行。

`activation-group`

​	默认值：N/A

​	类型：string

​	在`activation-group`中的规则，只会被执行，确切的说，在这个`activation-group`中的第一个规则会被执行，并且取消这个组等待激活的规则，阻止他们执行

`dialect`

​	默认值:由包指定

​	类型:String

​	可能的值："java"或者"mvel"

​	`dialect`种类被用于在`LHS`和`RHS`代码块中的代码表达式，目前只有两种`dailect`："java"和“MVEL”。吐过在规则中定义了`dailect`，那么他会覆盖包级别的`dailect`。

`date-effective`

​	默认值：N/A

​	类型：String(包含了日期和时间定义)

​	只有当前的日期和时间在`data-effective`之后，规则才能够被激活。

`date-expires`

​	默认值：N/A

​	类型：String(包含了日期和时间定义)

​	如果当前的日期和时间在`data-expires`属性之后，规则将不会被激活。

`duration`

​	默认值：没有默认值

​	类型：long

​	如果该规则为真，那么规则将会在指定的时间之后执行。

*一些属性的例子*

```java
rule "my rule"
  salience 42
  agenda-group "number 1"
    when ...
```

### 定时器和日期(Timers and Calendars)

规则现在支持基于`interval`和`cron`的定时器，来代替现在已经已经遗弃的`duration`属性。

*简单的定时器的使用*

```java
timer ( int: <initial delay> <repeat interval>? )
timer ( int: 30s )
timer ( int: 30s 5m )

timer ( cron: <cron expression> )
timer ( cron:* 0/15 * * * ? )
```

间隔定时器由`int:`表示，cron定时器由`cron:`表示，`cron`表达式遵行标准的Unix cron 表达式

*一个cron示例*

```java
rule "Send SMS every 15 minutes"
    timer (cron:* 0/15 * * * ?)
when
    $a : Alarm( on == true )
then
    channels[ "sms" ].insert( new Sms( $a.mobileNumber, "The alarm is still on" );
end
```

每匹配一次，当匹配成功之后，就会将由计时器的规则设置为激活，根据定时器的设置，它的结果会重复执行，一旦条件不再匹配，他就会停止。

在使用`fireUnitHalt`控制返回之后，结果同样会被执行，此外，engine同样会对工作寄存器的变化做出反应，

>Consequences are executed even after control returns from a call to fireUntilHalt. Moreover, the Engine remains reactive to any changes made to the Working Memory. For instance, removing a fact that was involved in triggering the timer rule’s execution causes the repeated execution to terminate, or inserting a fact so that some rule matches will cause that rule to fire. But the Engine is not continually active, only after a rule fires, for whatever reason. Thus, reactions to an insertion done asynchronously will not happen until the next execution of a timer-controlled rule. Disposing a session puts an end to all timer activity.

如果engine使用被动模式运行(使用`fireAllRules()`代替`fireUnitHalt()`)。他不会触发定时器调用返回的结果，除非在使用`fireAllRules`调用，但是可以通配置`keSession`的`TimedExcutingOption`改变默认行为，示例如下：

```java
KieSessionConfiguration ksconf =KieServices.Factory.get().newKieSessionConfiguration();
ksconf.setOption( TimedRuleExecutionOption.YES );
KSession ksession = kbase.newKieSession(ksconf, null);
```

还可以对自动执行的规则进行更细粒度的控制，要这样做，必须在`TimedExcutionOption`中设置一个`FILTERED`，允许定义一个回调用来处理这些规则，如下:

```java
KieSessionConfiguration ksconf =KieServices.Factory.get().newKieSessionConfiguration();
conf.setOption( new TimedRuleExecutionOption.FILTERED(new TimedRuleExecutionFilter() {
    public boolean accept(Rule[] rules) {
        return rules[0].getName().equals("MyRule");
    }
}) );
```

在时间间隔定时器中，我们可以为延时(delay)和间隔时间(interval)设置成表达式来取代固定值，要这样做的话有必要使用表达式定时器(通过`expr`表示)，例如

*一个表达式定时器例子*

```java
declare Bean
    delay   : String = "30s"
    period  : long = 60000
end

rule "Expression timer"
    timer( expr: $d, $p )
when
    Bean( $d : delay, $p : period )
then
end
```

定时的时间可以是String类型的，也可以是数字型的，数字型的是以毫秒为基本的单位。

固定值和表达式的定时器都有3个可选参数："start","end"和"repeat-limit"。使用一个和多个参数必须放在时间的后面，用`;`进行分隔。并且多个参数用`,`进行分隔。

```java
timer (int: 30s 10s; start=3-JAN-2010, end=5-JAN-2010)
```

`start`和`end`的值可以是日期，表示时间或长整形的字符串，或者是一个任意的数字，他的在Java中的转换如下：

```java
new Date( ((Number) n).longValue() )
```

相反，`repeat-limit`只能够是整型，它表示被允许重复执行的最大值，如果`repeat-limit`和`end`都设置的话，无论谁先满足条件，该定时器都会停止。

`start`定义了定时器的开始阶段，定时器的开始阶段是`start`加上`delay`。并且定时执行的开始时间如下：

```java
start + delay + n*period
```

```java
timer ( int: 30s 1m; start="3-JAN-2010" )
```

定时器实行时间在2010年1月3号的凌晨，每分钟第30秒开始执行，所以如果在2010年2月3号启动系统，那么定时器不会立即执行，而是会在保留到在午夜的第一个30秒中开始执行。

如果系统被暂停了(例如session被序列化并且在之后时间中反序列化)，规则会重新执行一次，无论错过多少次执行，之后定时器会继续按照时间执行。

Calendar用于控制规则什么时候触发，Calendar API是以Quartz为模型的

*适配一个Quartz Calendar*

```java
Calendar weekDayCal = QuartzHelper.quartzCalendarAdapter(org.quartz.Calendar quartzCal)
```

`Calendar`注册到`kieSession`中:

```java
ksession.getCalendars().set( "weekday", weekDayCal );
```

`Calendar`可以在普通的规则和带有计时器的规则中使用，`Calendar`可以包含一个或者多个以`,`分隔的字符串名称。

*使用了`Calendar`和`Timer`的规则示例*

```java
rule "weekdays are high priority"
   calendars "weekday"
   timer (int:0 1h)
when
    Alarm()
then
    send( "priority high - we have an alarm" );
end

rule "weekend are low priority"
   calendars "weekend"
   timer (int:0 4h)
when
    Alarm()
then
    send( "priority low - we have an alarm" );
end
```

### Left Hand Side(when)synax

`LHS`是条件快的通用表示，他表示条件区域，`LHS`可以由零到多个元素，如果`LHS`里面没有元素的话，他表示条件一直为true，并且在创建新的工作寄存器的`session`时，会被激活一次。

![`LHS`铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/lhs.png)

*没有条件的规则*

```java
rule "no CEs"
when
    // empty
then
    ... // actions (executed once)
end

// The above rule is internally rewritten as:

rule "eval(true)"
when
    eval( true )
then
    ... // actions (executed once)
end
```

多个匹配模式可以显式的使用`and`进行连接，同时也可以使用隐式的方式连接。

```java
rule "2 unconnected patterns"
when
    Pattern1()
    Pattern2()
then
    ... // actions
end

// The above rule is internally rewritten as:

rule "2 and connected patterns"
when
    Pattern1()
    and Pattern2()
then
    ... // actions
end
```

> **提示**：使用`and`和`or`连接多个模式是不能使用绑定的，因为绑定只能够绑定到单个的事实(fact)上面，使用`and`进行连接的话，会导致绑定不知道绑定到哪一个事实(fact)上面:
>
> ````java
> // Compile error
> $person : (Person( name == "Romeo" ) and Person( name == "Juliet"))
> ````

#### Pattern(Conditional element)

pattern元素是一个重要的条件元素，他可以匹配每一个插入到工作寄存器中的事实(fact)。

pattern包含零个或多个约束和可选择的绑定模式。

![pattern铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/Pattern.png)

没有任何约束条件，模式会与声明的事实进行匹配（fact defined），他会与工作寄存器中的所有事实(fact)进行匹配。

```java
Person()
```

类型可以是一些实施对象的实际类，也可以是他的超类，或者接口。

```java
Object() // matches all objects in the working memory
```

`()`的内部是用来定义约束条件的。例如：

```java
Person( age == 100 )
```

> **提示**：处于兼容性的考虑，允许使用`;`字符，但不建议这么做

##### 匹配绑定(pattern binding)

要使用匹配到的对象，需要使用参数来绑定对象，如`$p`。

````java
rule ...
when
    $p : Person()
then
    System.out.println( "Person " + $p );
end
````

#### 约束(part of pattern)

约束是一个表达式，并且返回`true`或者`false`。

```java
Person( 5 < 6 )  // just an example, as constraints like this would be useless in a real pattern
```

约束的表达式是对Java表达式的增强，并且有一些不同，例如`equals()`语义由`==`代替。

##### 在Javabean中的属性访问(Property access on java beans(POJOS))

Java Beans 中的属性可以直接使用，也可以调用`Get`方法（如果是boolean类型的is方法）。例如：

```java
Person( age == 50 )

// this is the same as:
Person( getAge() == 50 )
```

drools使用的标准的JDK`Introspector`，所以允许的标准的Java bean的格式。

> **提示**：建议直接使用属性而不是使用get方法获取值，直接使用可以获取更好的索引

> **警告**：不要改变对象的状态，因为drools engine 在匹配中缓存了对象。
>
> ```java
> public int getAge() {
>     age++; // Do NOT do this
>     return age;
> }
> ```
>
> ```java
> public int getAge() {
>     Date now = DateUtil.now(); // Do NOT do this
>     return DateUtil.differenceInYears(now, birthday);
> }
> ```
>
> 在后面这个例子中，如果要更新在工作寄存器中的事实，需要使用`fireAllRules`进行更新。

> **提示**：如果engine没有找到对象的`getter`方法，那么，编译器将会采取使用对象的名称作为方法并且不带参数。
>
> ```java
> Person( age == 50 )
> 
> // If Person.getAge() does not exists, this falls back to:
> Person( age() == 50 )
> ```

允许内嵌的的属性访问，也会被索引：

```java
Person( address.houseNumber == 50 )

// this is the same as:
Person( getAddress().getHouseNumber() == 50 )
```

> **警告**:在有状态的会话中的时候，在使用内嵌的访问器的时候要注意，因为工作寄存器不知道内嵌的值，并且不知道它是否改变了。要么在对放在工作寄存器的父级对内嵌的属性设置为不可变的，要么，你如果要修改内嵌的属性，那么引用它所有的外部属性被标记为已更新，例如上面的例子中，当`houseNumber`改变了，那么`address`和`person`都要被标记为已更新。

##### Java表达式(java expression)

你可以使用Java表达式中返回`boolean`的表达式作为约束，同样Java表达式也可以混合用增强的表达式。例如属性访问(property access)。

```java
Person( age == 50 )
```

在任何的逻辑和数学表达式中，可以使用`()`表示判断级别优先级。

```java
Person( age > 100 && ( age % 10 == 0 ) )
```

可以重用Java方法：

```java
Person( Math.round( weight / ( height * height ) ) < 25.0 )
```

> 在`LHS`应该使用可读的方法，而不是使用对规则有影响的方法。
>
> ```java
> Person( incrementAndGetAge() == 10 ) // Do NOT do this
> ```

> 在规则的调用之中不应该改变事实(fact)的状态，除非这些`fact`已被标记了已更新。
>
> ```java
> Person( System.currentTimeMillis() % 1000 == 0 ) // Do NOT do this
> ```

所有的Java运算符都优先使用，看下面的运算符优先列表。

> **警告**：所有的运算符都是普通的Java语义，除了`==`和`!=`。
>
> `==`运算符有`equals()`的语义。
>
> ````java
> // Similar to: java.util.Objects.equals(person.getFirstName(), "John")
> // so (because "John" is not null) similar to:
> // "John".equals(person.getFirstName())
> Person( firstName == "John" )
> ````
>
> `!=`运算符有`!equals()`的语义
>
> ```java
> // Similar to: !java.util.Objects.equals(person.getFirstName(), "John")
> Person( firstName != "John" )
> ```

可以尝试类型强制，但是类型强制也可以会报错。

```java
Person( age == "10" ) // "10" is coerced to 10
```

##### `,`分隔的`and`(Comma seperated AND)

可以使用`,`分隔作为`AND`的一种含蓄的表示方式。

```java
// Person is at least 50 and weighs at least 80 kg
Person( age > 50, weight > 80 )
```

```java
// Person is at least 50, weighs at least 80 kg and is taller than 2 meter.
Person( age > 50, weight > 80, height > 2 )
```

> **说明**：尽管`&&`和`,`有相同的语义，但是他们优先级不一样。`&&`运算符比`||`优先，`&&`和`||`优先于`,`。
>
> 在顶级的约束的中，`,`应该是首选，他可以更好的可读，并且系统可以优化它。

使用`,`嵌入在负责的约束表达式中，例如`()`：

```java
Person( ( age > 50, weight > 80 ) || height > 2 ) // Do NOT do this: compile error

// Use this instead
Person( ( age > 50 && weight > 80 ) || height > 2 )
```

##### 绑定变量(binding variables)

属性绑定变量：

```java
// 2 persons of the same age
Person( $firstAge : age ) // binding
Person( age == $firstAge ) // constraint expression
```

`$`符号只是一个惯例，在区分变量与属性之间非常有用。

> **提示**：考虑到兼容性，允许但是不建议将约束绑定和约束表达式混合使用。
>
> ```java
> // Not recommended
> Person( $age : age * 2 < 100 )
> ```
>
> ```java
> // Recommended (separates bindings and constraint expressions)
> Person( age * 2 < 100, $age : age )
> ```
>

被绑定的变量在使用`==`运算符的时候提供了非常快的执行速度，因为他使用的是hash索引

##### 统一(unification)

drools不允许使用绑定相同的值。这在派生查询统一中是非常重要的一个反面，使用特定的统一符号`:=`在处理位置参数的的时候。

```java
Person( $age := age )
Person( $age := age)
```

本质上，统一会声明一个绑定在第一出现中，同时约束在绑定字段相同的值。

##### 嵌套对象的分组访问(Grouped accessors  for nested object)

它发生在一个需要访问嵌套对象的多个属性的时候。例如：

```java
Person( name == "mark", address.city == "london", address.country == "uk" )
```

有嵌套对象的访问可以使用符号`.(...)`来进行分组，同时是规则具有好的可读性。例如：

```java
Person( name == "mark", address.( city == "london", country == "uk") )
```

注意这个`.`号，这个是用来区分嵌套对象和约束的。

##### Inline casts and coercion

在处理嵌套对象时，需要使用它的子类型，可以通过`#`符号处理，如下：

```java
Person( name == "mark", address#LongAddress.country == "uk" )
```

上面的例子通过address连接到LongAddress，如果无法连接到的话，实例化会返回一个false，同时Person的判断会被认为是false。同时完全限定名称也是支持的。例如:

```java
Person( name == "mark", address#org.domain.LongAddress.country == "uk" )
```

可以使用同样的方法进行多个级联的连接。例如：

```java
Person( name == "mark", address#LongAddress.country#DetailedCountry.population > 10000000 )
```

此外，还只是使用`instanceof`操作符，我们可以更进一步的使用它的字段。

```java
Person( name == "mark", address instanceof LongAddress, address.country == "uk" )
```

特定的文字支持(Special literal supported)

除了普通的Java文字（包括Java5 开始的enums），这个文字也支持

###### 时间文字

时间格式`dd-MM-yyyy`默认支持的。你可以通过修改系统属性`drools.datefomart`来自定义可替换的时间格式。系统属性可以包含时间格式（例如：`drools.dateformat="dd-MM-yyyy HH:mm"`）。例外一种修改时间格式就是使用`drools.defaultlanguage`和`drools.defaultcountry`系统属性修改本地语言。例如地区是泰国，可以设置`drools.defaultlanguage=th`和`drools.defaultcountry=TH`。

*时间文字限制*

```java
Cheese( bestBefore < "27-Oct-2009" )
```

##### List和Map访问(List and map access)

通过索引直接访问List

```java
// Same as childList(0).getAge() == 18
Person( childList[0].age == 18 )
```

也是直接访问Map

```java
// Same as credentialMap.get("jsmith").isValid()
Person( credentialMap["jsmith"].valid )
```

##### 组合关系条件缩写(Abbreviated combined relation condition)

允许在一个字段中使用超过一个限制连接符`&&`或`||`,通过`()`分组是允许的。

![Abbreviated combined relation condition](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/abbreviatedCombinedRelationCondition.png)

![Abbreviated combined relation condition withparentheses](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/abbreviatedCombinedRelationConditionGroup.png)

```java
// Simple abbreviated combined relation condition using a single &&
Person( age > 30 && < 40 )
```

```java
// Complex abbreviated combined relation using groupings
Person( age ( (> 30 && < 40) ||
              (> 20 && < 25) ) )
```

```java
// Mixing abbreviated combined relation with constraint connectives
Person( age > 30 && < 40 || location == "london" )
```

##### 特殊的DRL操作符(Sepcial DRL operators)

*特殊的DRL操作符*

![特殊的DRL操作符](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/operator.png)

强制使用正确的值。

###### 操作符`< <= > >=`

通过自然排序，这些操作符可以使用在属性中。例如，对于时间字段，`<`表示之前(before)，对于String字段，

他表示按字母顺序降低。

```java
Person( firstName < $otherFirstName )
```

```java
Person( birthDate < $otherBirthDate )
```

仅仅适用于可比属性。

###### null-safe引用操作符

`!.`操作符允许你用安全的方式引用。

```java
Person( $streetName : address!.street )
```

例外一种写法：

```java
Person( address != null, $streetName : address.street )
```

###### 操作符`matches`

可以通过任意的java正则表达式进行匹配，正则表达式可以是字符串文字，也可以是一个变量。

```java
Cheese( type matches "(Buffalo)?\\S*Mozzarella" )
```

> **提示**：如Java中一样，使用`\\`进行转义

###### 操作符`not matches`

如果不匹配正则表达式就返回为true，说明参考`matches`。

```java
Cheese( type not matches "(Buffalo)?\\S*Mozzarella" )
```

仅仅适用于String的属性，同时`null`的`not matches`也会返回为true

###### 操作符`contains`

判断一个集合和元素是否包含特定的值

```java
CheeseCounter( cheeses contains "stilton" ) // contains with a String literal
CheeseCounter( cheeses contains $var ) // contains with a variable
```

仅仅适用于`Collection`属性

操作符`contains`也可以使用`String.contains()`替换。

```java
Cheese( name contains "tilto" )
Person( fullName contains "Jr" )
String( this contains "foo" )
```

###### 操作符`not contains`

```java
CheeseCounter( cheeses not contains "cheddar" ) // not contains with a String literal
CheeseCounter( cheeses not contains $var ) // not contains with a variable
```

> **提示**：考虑到向后兼容性，支持使用`exclude`代替`not contains`。

```java
Cheese( name not contains "tilto" )
Person( fullName not contains "Jr" )
String( this not contains "foo" )
```

###### 操作符`memberof`

`memberof`用来检查字段是否是一个`collection`或`element`中的一员，集合必须是一个变量。

```java
CheeseCounter( cheese memberOf $matureCheeses )
```

###### 操作符`not memberof`

```java
CheeseCounter( cheese not memberOf $matureCheeses )
```

这个操作符与`matches`基本类似，但是他是检查一个单词是否与给出的值有相同的声音（使用英语发音），他是基于Soundex算法（参考：http://en.wikipedia.org/wiki/Soundex）。

```java
// match cheese "fubar" or "foobar"
Cheese( name soundslike 'foobar' )
```

###### 操作符`str`

该操作符用来检查字符串是否以某些值开头或结尾的，同时它也用来检查字符串的长度。

```java
Message( routingValue str[startsWith] "R1" )
```

```java
Message( routingValue str[endsWith] "R2" )
```

```java
Message( routingValue str[length] 17 )
```

操作符`in`和`not in`（复合值限制）

该操作符可以与多种可能值进行匹配，目前`in`和`not in`仅支持这一点，操作数使用`,`分割，其中的值可以是变量，文字，返回值或合格的标识符。

![in或not in铁路图](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/LanguageReference/compoundValueRestriction.png)

```java
Person( $cheese : favouriteCheese )
Cheese( type in ( "stilton", "cheddar", $cheese ) )
```

###### 内联操作符`eval`(已弃用)

```java
Person( girlAge : age, sex = "F" )
Person( eval( age == girlAge + 2 ), sex = 'M' ) // eval() is actually obsolete in this example
```

###### 操作符优先级

| Operator type                        | Operators                 | Notes                                                        |
| ------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| (nested / null safe) property access | `.``!.`                   | Not normal Java semantics                                    |
| List/Map access                      | `[ ]`                     | Not normal Java semantics                                    |
| constraint binding                   | `:`                       | Not normal Java semantics                                    |
| multiplicative                       | `\*``/``%`                |                                                              |
| additive                             | `\+``-`                   |                                                              |
| shift                                | `<<``>>``>>>`             |                                                              |
| relational                           | `<``>``⇐``>=``instanceof` |                                                              |
| equality                             | `==``!=`                  | Does not use normal Java (*not*) *same*semantics: uses (*not*) *equals* semantics instead. |
| non-short circuiting AND             | `&`                       |                                                              |
| non-short circuiting exclusive OR    | `^`                       |                                                              |
| non-short circuiting inclusive OR    | `|`                       |                                                              |
| logical AND                          | `&&`                      |                                                              |
| logical OR                           | `||`                      |                                                              |
| ternary                              | `? :`                     |                                                              |
| Comma separated AND                  | `,`                       | Not normal Java semantics                                    |



## drl规则格式

```java
package org.drools.examples.helloworld//程序的包名
 
import org.drools.examples.helloworld.HelloWorldExample.Message;//数据类的类名（POJO形式）

global java.util.List list
 
rule "Hello World" //规则名
    dialect "mvel"
    when
        m : Message( status == Message.HELLO, message : message )
    then
        System.out.println( message );
//        modify ( m ) { setMessage( "Goodbyte cruel world" ),
//                       setStatus( Message.GOODBYE ) };
    modify ( m ) { message = "Goodbye cruel world",
                   status = Message.GOODBYE };
end

rule "Good Bye"
    dialect "java"
    when
        Message( status == Message.GOODBYE, message : message )
    then
        System.out.println( message );
end

```

### package

​	包名，该包名就是drl规则作用在哪个业务逻辑下所在的包名。

### import 

​	JavaBean的位置。

### global

​	全局变量，与java 的全局变量解释基本类似，变量类型要用全名称。包括所在的包。

### rule

​	规则名称。

### when 

​	判断条件，满足条件就执行后面的then中的方法。条件可以是多个

### then 

​	满足条件，then中的方法就会执行。

### end

​	规则的结束，标志这条规则在此处结束。



## 判断条件

 	m : Message( status == Message.HELLO, message : message )

对数据进行判断是以类名开始。“()“中为判断条件,其他的操作也在括号中进行处理。包括参数的映射，对参数进行赋值





## KieSession 两种状态

### stateless KIE session

无状态会话，不利用推理，形成最简单的用例。 无状态会话可以被调用，就像一个函数传递一些数据然后接收一些结果。 无状态会话的一些常见用例包括但不限于：

* 验证
  * 基本相关的验证，如身份验证，资格验证
* 计算
  * 对数据进行相关的计算，比如金钱。
* 路由和过滤
  * 将传入的消息（如电子邮件）过滤到文件夹中。
  * 将传入的消息发送到目的地。

### stateful KIE session

有状态会话是长期存在的，并允许随着时间的推移进行迭代更改。 有状态会话的一些常见用例包括但不限于：

* 监听
  * 对相关的数据进行监听和分析
* 诊断
  * 错误发现等
* 后勤
  * 包裹跟踪和交付配置
* 合规
  * 验证市场交易的合法性。

## 执行控制

### 议程（Agenda）

Agenda是具有RETE特性，他负责管理维护规则的执行，首先他会去寻找规则，如果找到了规则，就把规则提交给规则运行时（ruleruntime），同时Agenda负责管理提交的顺序。如果没有匹配到相应的规则，就直接推出。下面Agenda的基本循环控制图：

![Agenda循环控制](https://docs.jboss.org/drools/release/7.14.0.Final/drools-docs/html_single/UserGuide/Two_Phase.png)

## 重要笔记

### java对象对等性

在drool中，如果你需要真理维持系统（Truth Maintenance System）工作的话，你需要事实对象(Fact object)`JavaBeans`必须正确的重写(override)`equals()`和`hashcode()`这两个方法。



## 参考资料

[java规则引擎总结](http://www.iigrowing.cn/java_gui_ze_yin_qing_zong_jie.html)

[Drools学习笔记](https://geosmart.github.io/2016/08/22/Drools%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/)

