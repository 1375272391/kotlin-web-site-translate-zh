[//]: # (title: Intermediate: Objects)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br /> 
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br /> 
        <img src="icon-5.svg" width="20" alt="Fourth step" /> <strong>Objects</strong><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In this chapter, you'll expand your understanding of classes by exploring object declarations. This knowledge will help 
you efficiently manage behavior across your projects.
<br />在本章中，你将通过探索对象声明来加深对类的理解。这些知识将帮助你高效地管理项目中的行为。

## Object declarations
对象声明

In Kotlin, you can use **object declarations** to declare a class with a single instance. In a sense, you declare the 
class and create the single instance _at the same time_. Object declarations are useful when you want to create a class to
use as a single reference point for your program or to coordinate behavior across a system.
<br />在 Kotlin 中，您可以使用**对象声明**来声明一个只有单个实例的类。从某种意义上说，您_同时_声明了类并创建了该实例。
当您想要创建一个类作为程序的单一引用点或用于协调整个系统的行为时，对象声明非常有用。

> A class that has only one instance that is easily accessible is called a **singleton**.
> <br />只有一个易于访问的实例的类称为**单例**。
>
{style="tip"}

Objects in Kotlin are ***lazy**, meanin*g they are created only when accessed. Kotlin also ensures that all
objects are created in a thread-safe manner so that you don't have to check this manually.
<br />Kotlin 中的对象是**延迟求值**的，这意味着它们只有在被访问时才会创建。Kotlin 还确保所有对象都以线程安全的方式创建，因此您无需手动检查线程安全。

To create an object declaration, use the `object` keyword:
<br />要创建对象声明，请使用 `object` 关键字：

```kotlin
object DoAuth {}
```

Following the name of your `object`, add any properties or member functions within the object body defined by curly braces `{}`.
<br />在对象名称之后，添加任何属性或成员函数，并在对象主体内用花括号 `{}` 定义。

> Objects can't have constructors, so they don't have headers like classes.
> <br />对象不能有构造函数，所以它们不像类那样有头文件。
>
{style="note"}

For example, let's say that you wanted to create an object called `DoAuth` that is responsible for authentication:
<br />例如，假设您想要创建一个名为 `DoAuth` 的对象，该对象负责身份验证：

```kotlin
object DoAuth {
    fun takeParams(username: String, password: String) {
        println("input Auth parameters = $username:$password")
    }
}

fun main(){
    // The object is created when the takeParams() function is called
    DoAuth.takeParams("coding_ninja", "N1njaC0ding!")
    // input Auth parameters = coding_ninja:N1njaC0ding!
}
```
{kotlin-runnable="true" id="kotlin-tour-object-declarations"}

The object has a member function called `takeParams` that accepts `username` and `password` variables as parameters
and prints a string to the console. The `DoAuth` object is only created when the function is called for the first time.
<br />该对象有一个名为 `takeParams` 的成员函数，它接受 `username` 和 `password` 变量作为参数，
并将一个字符串打印到控制台。`DoAuth` 对象仅在首次调用该函数时创建。

> Objects can inherit from classes and interfaces. For example:
> <br />对象可以继承自类和接口。例如：
> 
> ```kotlin
> interface Auth {
>     fun takeParams(username: String, password: String)
> }
>
> object DoAuth : Auth {
>     override fun takeParams(username: String, password: String) {
>         println("input Auth parameters = $username:$password")
>     }
> }
> ```
>
{style="note"}

#### Data objects
数据对象

To make it easier to print the contents of an object declaration, Kotlin has **data** objects. Similar to data classes,
which you learned about in the beginner tour, data objects automatically come with additional member functions: 
`toString()` and `equals()`.
<br />为了更方便地打印对象声明的内容，Kotlin 引入了 **data** 对象。与你在入门教程中学到的数据类类似，数据对象也自动带有额外的成员函数：
`toString()` 和 `equals()`。

> Unlike data classes, data objects do not come automatically with the `copy()` member function because they only have
> a single instance that can't be copied.
> <br />与数据类不同，数据对象不会自动带有 `copy()` 成员函数，因为它们只有一个实例，无法复制。
>
{type ="note"}

To create a data object, use the same syntax as for object declarations but prefix it with the `data` keyword:
<br />要创建数据对象，请使用与对象声明相同的语法，但要在前面加上 `data` 关键字：

```kotlin
data object AppConfig {}
```

For example:
<br />例如: 

```kotlin
data object AppConfig {
    var appName: String = "My Application"
    var version: String = "1.0.0"
}

fun main() {
    println(AppConfig)
    // AppConfig
    
    println(AppConfig.appName)
    // My Application
}
```
{kotlin-runnable="true" id="kotlin-tour-data-objects"}

For more information about data objects, see [](object-declarations.md#data-objects).
<br />有关数据对象的更多信息，请参阅 [](object-declarations.md#data-objects).

#### Companion objects
<br /> 伴生对象

In Kotlin, a class can have an object: a **companion** object. You can only have **one** companion object per class.
A companion object is created only when its class is referenced for the first time.
<br />在 Kotlin 中，一个类可以拥有一个对象：**伴生**对象。每个类只能有**一个**伴生对象。 
伴生对象仅在其所属类首次被引用时创建。

Any properties or functions declared inside a companion object are shared across all class instances.
<br />在伴生对象中声明的任何属性或函数都会在所有类实例之间共享。

To create a companion object within a class, use the same syntax for an object declaration but prefix it with the `companion`
keyword:
<br />要在类中创建伴生对象，请使用与对象声明相同的语法，但要在前面加上 `companion` 关键字。

```kotlin
companion object Bonger {}
```

> A companion object doesn't have to have a name. If you don't define one, the default is `Companion`.
> <br />伴生对象不必有名称。如果没有定义名称，则默认值为 `Companion`。
> 
{style="note"}

To access any properties or functions of the companion object, reference the class name. For example:
<br />要访问伴生对象的任何属性或函数，请引用类名。例如：

```kotlin
class BigBen {
    companion object Bonger {
        fun getBongs(nTimes: Int) {
            repeat(nTimes) { print("BONG ") }
            }
        }
    }

fun main() {
    // Companion object is created when the class is referenced for the
    // first time.
    BigBen.getBongs(12)
    // BONG BONG BONG BONG BONG BONG BONG BONG BONG BONG BONG BONG 
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-companion-object"}

This example creates a class called `BigBen` that contains a companion object called `Bonger`. The companion object
has a member function called `getBongs()` that accepts an integer and prints `"BONG"` to the console the same number of times
as the integer.
<br />这个例子创建了一个名为 `BigBen` 的类，其中包含一个名为 `Bonger` 的伴生对象。该伴生对象
有一个名为 `getBongs()` 的成员函数，它接受一个整数，并在控制台上打印与该整数相同的次数的 `"BONG"`。

In the `main()` function, the `getBongs()` function is called by referring to the class name. The companion object is created
at this point. The `getBongs()` function is called with parameter `12`.
<br />在 `main()` 函数中，通过引用类名来调用 `getBongs()` 函数。此时创建伴生对象。
`getBongs()` 函数被调用时传递了参数 `12`。

For more information, see [](object-declarations.md#companion-objects).
<br />更多信息请参见 [](object-declarations.md#companion-objects).

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="objects-exercise-1"}

You run a coffee shop and have a system for tracking customer orders. Consider the code below and complete the declaration
of the second data object so that the following code in the `main()` function runs successfully:
<br />你经营一家咖啡店，并有一个用于跟踪顾客订单的系统。请参考以下代码，并完成第二个数据对象的声明，以便 `main()` 函数中的以下代码能够成功运行：

|---|---|

```kotlin
interface Order {
    val orderId: String
    val customerName: String
    val orderTotal: Double
}

data object OrderOne: Order {
    override val orderId = "001"
    override val customerName = "Alice"
    override val orderTotal = 15.50
}

data object // Write your code here

fun main() {
    // Print the name of each data object
    println("Order name: $OrderOne")
    // Order name: OrderOne
    println("Order name: $OrderTwo")
    // Order name: OrderTwo

    // Check if the orders are identical
    println("Are the two orders identical? ${OrderOne == OrderTwo}")
    // Are the two orders identical? false

    if (OrderOne == OrderTwo) {
        println("The orders are identical.")
    } else {
        println("The orders are unique.")
        // The orders are unique.
    }

    println("Do the orders have the same customer name? ${OrderOne.customerName == OrderTwo.customerName}")
    // Do the orders have the same customer name? false
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-objects-exercise-1"}

|---|---|
```kotlin
interface Order {
    val orderId: String
    val customerName: String
    val orderTotal: Double
}

data object OrderOne: Order {
    override val orderId = "001"
    override val customerName = "Alice"
    override val orderTotal = 15.50
}

data object OrderTwo: Order {
    override val orderId = "002"
    override val customerName = "Bob"
    override val orderTotal = 12.75
}

fun main() {
    // Print the name of each data object
    println("Order name: $OrderOne")
    // Order name: OrderOne
    println("Order name: $OrderTwo")
    // Order name: OrderTwo

    // Check if the orders are identical
    println("Are the two orders identical? ${OrderOne == OrderTwo}")
    // Are the two orders identical? false

    if (OrderOne == OrderTwo) {
        println("The orders are identical.")
    } else {
        println("The orders are unique.")
        // The orders are unique.
    }

    println("Do the orders have the same customer name? ${OrderOne.customerName == OrderTwo.customerName}")
    // Do the orders have the same customer name? false
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-objects-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="objects-exercise-2"}

Create an object declaration that inherits from the `Vehicle` interface to create a unique vehicle type: `FlyingSkateboard`.
Implement the `name` property and the `move()` function in your object so that the following code in the `main()` function runs 
successfully:
<br />创建一个继承自 `Vehicle` 接口的对象声明，以创建一个独特的车辆类型：`FlyingSkateboard`。
在你的对象中实现 `name` 属性和 `move()` 函数，以便 `main()` 函数中的以下代码能够成功运行：

|---|---|

```kotlin
interface Vehicle {
    val name: String
    fun move(): String
}

object // Write your code here

fun main() {
    println("${FlyingSkateboard.name}: ${FlyingSkateboard.move()}")
    // Flying Skateboard: Glides through the air with a hover engine
    println("${FlyingSkateboard.name}: ${FlyingSkateboard.fly()}")
    // Flying Skateboard: Woooooooo
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-objects-exercise-2"}

|---|---|
```kotlin
interface Vehicle {
    val name: String
    fun move(): String
}

object FlyingSkateboard : Vehicle {
    override val name = "Flying Skateboard"
    override fun move() = "Glides through the air with a hover engine"

   fun fly(): String = "Woooooooo"
}

fun main() {
    println("${FlyingSkateboard.name}: ${FlyingSkateboard.move()}")
    // Flying Skateboard: Glides through the air with a hover engine
    println("${FlyingSkateboard.name}: ${FlyingSkateboard.fly()}")
    // Flying Skateboard: Woooooooo
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-objects-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="objects-exercise-3"}

You have an app where you want to record temperatures. The class itself stores the information in Celsius, but 
you want to provide an easy way to create an instance in Fahrenheit as well. Complete the data class so that
the following code in the `main()` function runs successfully:
<br />你有一个用于记录温度的应用程序。该类本身以摄氏度存储信息，但你也希望提供一种简便的方法来创建华氏度实例。
请完善数据类，使以下 `main()` 函数中的代码能够成功运行：

<deflist collapsible="true">
    <def title="Hint">
        Use a companion object.
        <br />使用伴生对象。
    </def>
</deflist>

|---|---|
```kotlin
data class Temperature(val celsius: Double) {
    val fahrenheit: Double = celsius * 9 / 5 + 32

    // Write your code here
}

fun main() {
    val fahrenheit = 90.0
    val temp = Temperature.fromFahrenheit(fahrenheit)
    println("${temp.celsius}°C is $fahrenheit °F")
    // 32.22222222222222°C is 90.0 °F
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-objects-exercise-3"}

|---|---|
```kotlin
data class Temperature(val celsius: Double) {
    val fahrenheit: Double = celsius * 9 / 5 + 32

    companion object {
        fun fromFahrenheit(fahrenheit: Double): Temperature = Temperature((fahrenheit - 32) * 5 / 9)
    }
}

fun main() {
    val fahrenheit = 90.0
    val temp = Temperature.fromFahrenheit(fahrenheit)
    println("${temp.celsius}°C is $fahrenheit °F")
    // 32.22222222222222°C is 90.0 °F
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-objects-solution-3"}

## Next step
接下来

[Intermediate: Open and special classes](kotlin-tour-intermediate-open-special-classes.md)
<br />[中级：公开类和特殊类](kotlin-tour-intermediate-open-special-classes.md)