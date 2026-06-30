[//]: # (title: Classes)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5-done.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6.svg" width="20" alt="Sixth step" /> <strong>Classes</strong><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

Kotlin supports object-oriented programming with classes and objects. Objects are useful for storing data in your program.
Classes allow you to declare a set of characteristics for an object. When you create objects from a class, you can save
time and effort because you don't have to declare these characteristics every time.
<br />Kotlin 通过类和对象支持面向对象编程。对象用于在程序中存储数据。
类允许您为对象声明一组特征。当您从类创建对象时，可以节省时间和精力，因为您不必每次都声明这些特征。

To declare a class, use the `class` keyword: 
<br />要声明一个类，请使用 `class` 关键字：

```kotlin
class Customer
```

## Properties
属性

Characteristics of a class's object can be declared in properties. You can declare properties for a class:
<br />类的对象特征可以通过属性来声明。您可以为类声明属性：

* Within parentheses `()` after the class name.
  <br />在类名后的括号 `()` 内。
```kotlin
class Contact(val id: Int, var email: String)
```

* Within the class body defined by curly braces `{}`.
  <br />在用花括号 `{}` 定义的类体中。
```kotlin
class Contact(val id: Int, var email: String) {
    val category: String = ""
}
```

We recommend that you declare properties as read-only (`val`) unless they need to be changed after an instance of the class
is created.
<br />我们建议您将属性声明为只读(`val`)，除非需要在类实例创建后更改它们。

You can declare properties without `val` or `var` within parentheses but these properties are not accessible after an 
instance has been created.
<br />您可以在括号内声明不带 `val` 或 `var` 的属性，但这些属性在实例创建后将无法访问。

> * The content contained within parentheses `()` is called the **class header**.
> <br />括号 `()` 内的内容称为**类头**。
> * You can use a [trailing comma](coding-conventions.md#trailing-commas) when declaring class properties.
> <br />声明类属性时，可以使用尾随逗号。
>
{style="note"}

Just like with function parameters, class properties can have default values:
<br />与函数参数一样，类属性也可以有默认值：
```kotlin
class Contact(val id: Int, var email: String = "example@gmail.com") {
    val category: String = "work"
}
```

## Create instance
创建实例

To create an object from a class, you declare a class **instance** using a **constructor**.
<br />要从类创建对象，您可以使用**构造函数**声明类**实例**。

By default, Kotlin automatically creates a constructor with the parameters declared in the class header.
<br />默认情况下，Kotlin 会自动创建一个构造函数，其参数在类头中声明。

For example:
    <br />例如：
```kotlin
class Contact(val id: Int, var email: String)

fun main() {
    val contact = Contact(1, "mary@gmail.com")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-class-create-instance"}

In the example:
<br />例如：

* `Contact` is a class.
  <br />`Contact` 是一个类。
* `contact` is an instance of the `Contact` class.
  <br />`contact` 是 `Contact` 类的一个实例。
* `id` and `email` are properties.
  <br />`id` 和 `email` 是属性。
* `id` and `email` are used with the default constructor to create `contact`.
  <br />`id` 和 `email` 与默认构造函数一起使用来创建 `contact`。

Kotlin classes can have many constructors, including ones that you define yourself. To learn more about how to declare 
multiple constructors, see [Constructors](classes.md#constructors-and-initializer-blocks).
<br />Kotlin 类可以拥有多个构造函数，包括您自己定义的构造函数。
要了解有关如何声明多个构造函数的更多信息，请参阅[构造函数](classes.md#constructors-and-initializer-blocks)。

## Access properties
访问属性

To access a property of an instance, write the name of the property after the instance name appended with a period `.`:
<br />要访问实例的属性，请在实例名称后添加属性名称，并在其后加上句点 `.`：

```kotlin
class Contact(val id: Int, var email: String)

fun main() {
    val contact = Contact(1, "mary@gmail.com")
    
    // Prints the value of the property: email
    println(contact.email)           
    // mary@gmail.com

    // Updates the value of the property: email
    contact.email = "jane@gmail.com"
    
    // Prints the new value of the property: email
    println(contact.email)           
    // jane@gmail.com
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-access-property"}

> To concatenate the value of a property as part of a string, you can use string templates (`$`).
> For example:
> <br />要将属性值作为字符串的一部分连接起来，可以使用字符串模板(`$`)。
> 例如：
> ```kotlin
> println("Their email address is: ${contact.email}")
> ```
>
{style="tip"}

## Member functions
成员函数

In addition to declaring properties as part of an object's characteristics, you can also define an object's behavior 
with member functions.
<br />除了将属性声明为对象特征的一部分之外，您还可以使用成员函数定义对象的行为。

In Kotlin, member functions must be declared within the class body. To call a member function on an instance, write the 
function name after the instance name appended with a period `.`. For example:
<br />在 Kotlin 中，成员函数必须在类体内部声明。 要调用实例上的成员函数，请在实例名称后添加函数名，并在函数名后加一个句点 `.`。例如：

```kotlin
class Contact(val id: Int, var email: String) {
    fun printId() {
        println(id)
    }
}

fun main() {
    val contact = Contact(1, "mary@gmail.com")
    // Calls member function printId()
    contact.printId()           
    // 1
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-member-function"}

## Data classes
数据类

Kotlin has **data classes** which are particularly useful for storing data. Data classes have the same functionality as 
classes, but they come automatically with additional member functions. These member functions allow you to easily print 
the instance to readable output, compare instances of a class, copy instances, and more. As these functions are
automatically available, you don't have to spend time writing the same boilerplate code for each of your classes.
<br />Kotlin 拥有**数据类**，它们特别适合用于存储数据。数据类与普通类具有相同的功能，但它们自动包含额外的成员函数。
这些成员函数使您可以轻松地将实例打印为可读的输出、比较类的实例、复制实例等等。由于这些函数是自动提供的，因此您无需为每个类编写相同的样板代码。

To declare a data class, use the keyword `data`:
<br />要声明数据类，请使用关键字 `data`：

```kotlin
data class User(val name: String, val id: Int)
```

The Kotlin compiler only uses the properties defined inside the [primary constructor](classes.md#primary-constructor)
when generating member functions. If you declare properties in the data class body, they aren't included in the output
of the generated functions.

The most useful predefined member functions of data classes are:
<br />数据类中最常用的预定义成员函数有：

| **Function**       | **Description**                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| `toString()`       | Prints a readable string of the class instance and its properties.                       |
| `equals()` or `==` | Compares instances of a class.                                                           |
| `copy()`           | Creates a class instance by copying another, potentially with some different properties. |

| **函数**            | **介绍**                            |
|-------------------|-----------------------------------|
| `toString()`      | 打印类实例及其属性的易读字符串。                  |
| `equals()` 或 `==` | 比较类的实例。                           |
| `copy()`          | 通过复制另一个类实例来创建一个新的类实例，可能具有一些不同的属性。 |

See the following sections for examples of how to use each function:
<br />以下各节提供了如何使用每个函数的示例：

* [Print as string](#print-as-string)
  <br />[以字符串形式打印](#print-as-string)
* [Compare instances](#compare-instances)
  <br />[比较实例](#compare-instances)
* [Copy instance](#copy-instance)
  <br />[复制实例](#copy-instance)

### Print as string
打印为字符串

To print a readable string of a class instance, you can explicitly call the `toString()` function, or use print functions 
(`println()` and `print()`) which automatically call `toString()` for you:
<br />要打印类实例的可读字符串，您可以显式调用 `toString()` 函数，或者使用打印函数(`println()` 和 `print()`)，它们会自动为您调用 `toString()`：

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    //sampleStart
    val user = User("Alex", 1)
    
    // Automatically uses toString() function so that output is easy to read
    println(user)            
    // User(name=Alex, id=1)
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-data-classes-print-string"}

This is particularly useful when debugging or creating logs.
<br />这在调试或创建日志时尤其有用。

### Compare instances
比较实例

To compare data class instances, use the equality operator `==`:
<br />要比较数据类实例，请使用相等运算符 `==`：

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    //sampleStart
    val user = User("Alex", 1)
    val secondUser = User("Alex", 1)
    val thirdUser = User("Max", 2)

    // Compares user to second user
    println("user == secondUser: ${user == secondUser}") 
    // user == secondUser: true
    
    // Compares user to third user
    println("user == thirdUser: ${user == thirdUser}")   
    // user == thirdUser: false
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-data-classes-compare-instances"}

### Copy instance
复制实例

To create an exact copy of a data class instance, call the `copy()` function on the instance.
<br />要创建数据类实例的精确副本，请对该实例调用 `copy()` 函数。

To create a copy of a data class instance **and** change some properties, call the `copy()` function on the instance 
**and** add replacement values for properties as function parameters.
<br />要创建数据类实例的副本并更改某些属性，请对该实例调用 `copy()` 函数；
并将属性的替换值作为函数参数添加。

For example:
<br />例如：

```kotlin
data class User(val name: String, val id: Int)

fun main() {
    //sampleStart
    val user = User("Alex", 1)

    // Creates an exact copy of user
    println(user.copy())       
    // User(name=Alex, id=1)

    // Creates a copy of user with name: "Max"
    println(user.copy("Max"))  
    // User(name=Max, id=1)

    // Creates a copy of user with id: 3
    println(user.copy(id = 3)) 
    // User(name=Alex, id=3)
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-data-classes-copy-instance"}

Creating a copy of an instance is safer than modifying the original instance because any code that relies on the
original instance isn't affected by the copy and what you do with it.
<br />创建实例的副本比修改原始实例更安全，因为任何依赖于原始实例的代码都不会受到副本及其操作的影响。

For more information about data classes, see [Data classes](data-classes.md).
<br />有关数据类的更多信息，请参阅[数据类](data-classes.md)。

The last chapter of this tour is about Kotlin's [null safety](kotlin-tour-null-safety.md).
<br />本次巡回的最后一章是关于 Kotlin 的[空安全](kotlin-tour-null-safety.md)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true"}

Define a data class `Employee` with two properties: one for a name, and another for a salary. Make sure that the property
for salary is mutable, otherwise you won't get a salary boost at the end of the year! The main function demonstrates how
you can use this data class.
<br />定义一个名为 `Employee` 的数据类，它包含两个属性：一个用于姓名，另一个用于薪水。
请确保薪水属性是可变的，否则年底你将无法获得加薪！主函数演示了如何使用此数据类。

|---|---|
```kotlin
// Write your code here

fun main() {
    val emp = Employee("Mary", 20)
    println(emp)
    emp.salary += 10
    println(emp)
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-exercise-1"}

|---|---|
```kotlin
data class Employee(val name: String, var salary: Int)

fun main() {
    val emp = Employee("Mary", 20)
    println(emp)
    emp.salary += 10
    println(emp)
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true"}

Declare the additional data classes that are needed for this code to compile.
<br />声明编译此代码所需的其他数据类。

|---|---|
```kotlin
data class Person(val name: Name, val address: Address, val ownsAPet: Boolean = true)
// Write your code here
// data class Name(...)

fun main() {
    val person = Person(
        Name("John", "Smith"),
        Address("123 Fake Street", City("Springfield", "US")),
        ownsAPet = false
    )
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-exercise-2"}

|---|---|
```kotlin
data class Person(val name: Name, val address: Address, val ownsAPet: Boolean = true)
data class Name(val first: String, val last: String)
data class Address(val street: String, val city: City)
data class City(val name: String, val countryCode: String)

fun main() {
    val person = Person(
        Name("John", "Smith"),
        Address("123 Fake Street", City("Springfield", "US")),
        ownsAPet = false
    )
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true"}

To test your code, you need a generator that can create random employees. Define a `RandomEmployeeGenerator` class with 
a fixed list of potential names (inside the class body). Configure the class with a minimum and maximum salary (inside 
the class header). In the class body, define the `generateEmployee()` function. Once again, the main function demonstrates
how you can use this class.
<br />为了测试你的代码，你需要一个能够生成随机员工的生成器。
定义一个名为 `RandomEmployeeGenerator` 的类，并在类体中定义一个固定的潜在员工姓名列表。
在类头中配置该类的最低和最高薪资。在类体中定义 `generateEmployee()` 函数。
同样，主函数演示了如何使用这个类。

> In this exercise, you import a package so that you can use the [`Random.nextInt()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.random/-random/next-int.html) function.
> For more information about importing packages, see [Packages and imports](packages.md).
> <br />在本练习中，您将导入一个包，以便使用 [`Random.nextInt()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.random/-random/next-int.html) 函数。
> 有关导入包的更多信息，请参阅[包和导入](packages.md)。
>
{style="tip"}

<deflist collapsible="true" id="kotlin-tour-classes-exercise-3-hint-1">
    <def title="Hint 1">
        Lists have an extension function called <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/random.html"><code>.random()</code></a>
        that returns a random item within a list.
        <br />列表有一个名为 <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/random.html"><code>.random()</code></a> 的扩展函数，
        该函数返回列表中的一个随机元素。
    </def>
</deflist>

<deflist collapsible="true" id="kotlin-tour-classes-exercise-3-hint-2">
    <def title="Hint 2">
        <code>Random.nextInt(from = ..., until = ...)</code> gives you a random <code>Int</code> number within specified limits.
        <br /><code>Random.nextInt(from = ..., until = ...)</code> 会生成指定范围内的随机 <code>Int</code>。
    </def>
</deflist>

|---|---|
```kotlin
import kotlin.random.Random

data class Employee(val name: String, var salary: Int)

// Write your code here

fun main() {
    val empGen = RandomEmployeeGenerator(10, 30)
    println(empGen.generateEmployee())
    println(empGen.generateEmployee())
    println(empGen.generateEmployee())
    empGen.minSalary = 50
    empGen.maxSalary = 100
    println(empGen.generateEmployee())
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-exercise-3"}

|---|---|
```kotlin
import kotlin.random.Random

data class Employee(val name: String, var salary: Int)

class RandomEmployeeGenerator(var minSalary: Int, var maxSalary: Int) {
    val names = listOf("John", "Mary", "Ann", "Paul", "Jack", "Elizabeth")
    fun generateEmployee() =
        Employee(names.random(),
            Random.nextInt(from = minSalary, until = maxSalary))
}

fun main() {
    val empGen = RandomEmployeeGenerator(10, 30)
    println(empGen.generateEmployee())
    println(empGen.generateEmployee())
    println(empGen.generateEmployee())
    empGen.minSalary = 50
    empGen.maxSalary = 100
    println(empGen.generateEmployee())
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-solution-3"}

## Next step
接下来

[Null safety](kotlin-tour-null-safety.md)
<br />[空安全](kotlin-tour-null-safety.md)
