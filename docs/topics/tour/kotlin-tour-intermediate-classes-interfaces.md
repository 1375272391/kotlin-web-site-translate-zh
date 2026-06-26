[//]: # (title: Intermediate: Classes and interfaces)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br /> 
        <img src="icon-4.svg" width="20" alt="Fourth step" /> <strong>Classes and interfaces</strong><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In the beginner tour, you learned how to use classes and data classes to store data and maintain a collection of characteristics
that can be shared in your code. Eventually, you will want to create a hierarchy to efficiently share code within your 
projects. This chapter explains the options Kotlin provides for sharing code and how they can make your code safer and easier to maintain.
<br />在入门教程中，您学习了如何使用类和数据类来存储数据，并维护可在代码中共享的特征集合。最终，您需要创建一个层级结构，以便在项目中高效地共享代码。本章将介绍 Kotlin 提供的代码共享选项，以及它们如何使您的代码更安全、更易于维护。
## Class inheritance
类继承

In a previous chapter, we covered how you can use extension functions to extend classes without modifying the original source code.
But what if you are working on something complex where sharing code **between** classes would be useful? In such cases, 
you can use class inheritance.
<br />在前一章中，我们介绍了如何使用扩展函数在不修改原始源代码的情况下扩展类。
但是，如果您正在处理一些复杂的项目，需要在类之间共享代码，该怎么办呢？在这种情况下，您可以使用类继承。

By default, classes in Kotlin can't be inherited. Kotlin is designed this way to prevent unintended inheritance and make
your classes easier to maintain.
<br />默认情况下，Kotlin 中的类不能被继承。Kotlin 这样设计是为了防止意外继承，并使类更易于维护。

Kotlin classes only support **single inheritance**, meaning it is only possible to inherit from **one class at a time**.
This class is called the **parent**.
<br />Kotlin 类仅支持**单继承**，这意味着**一次只能继承一个类**。
这个类被称为**父类**。

The parent of a class inherits from another class (the grandparent), forming a hierarchy. At the top of Kotlin's class
hierarchy is the common parent class: `Any`. All classes ultimately inherit from the `Any` class:
<br />一个类的父类继承自另一个类（祖父类），从而形成一个继承层次结构。在 Kotlin 的类继承层次结构的顶端是公共父类：`Any`。所有类最终都继承自 `Any` 类。

![An example of the class hierarchy with Any type](any-type-class.png){width="200"}

The `Any` class provides the `toString()` function as a member function automatically. Therefore, you can
use this inherited function in any of your classes. For example:
<br />`Any` 类会自动提供 `toString()` 函数作为成员函数。因此，您可以在任何类中使用此继承函数。例如：

```kotlin
class Car(val make: String, val model: String, val numberOfDoors: Int)

fun main() {
    //sampleStart
    val car1 = Car("Toyota", "Corolla", 4)

    // Uses the .toString() function via string templates to print class properties
    println("Car1: make=${car1.make}, model=${car1.model}, numberOfDoors=${car1.numberOfDoors}")
    // Car1: make=Toyota, model=Corolla, numberOfDoors=4
    //sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-any-class"}

If you want to use inheritance to share some code between classes, first consider using abstract classes.
<br />如果你想使用继承在类之间共享一些代码，首先要考虑使用抽象类。

### Abstract classes
抽象类

Abstract classes can be inherited by default. The purpose of abstract classes is to provide members that other classes 
inherit or implement. As a result, they have a constructor, but you can't create instances from them. Within the child 
class, you define the behavior of the parent's properties and functions with the `override` keyword. In this way, 
you can say that the child class "overrides" the members of the parent class.
<br />抽象类默认可以被继承。抽象类的目的是提供其他类可以继承或实现的成员。因此，它们有构造函数，但不能直接创建实例。在子类中，
可以使用 `override` 关键字来定义父类属性和函数的行为。这样，就可以说子类“重写”了父类的成员。

> When you define the behavior of an inherited function or property, we call that an **implementation**.
> <br />当你定义继承的函数或属性的行为时，我们称之为**实现**。
> 
{style="tip"}

Abstract classes can contain both functions and properties **with** implementation as well as functions and properties 
**without** implementation, known as abstract functions and properties.
<br />抽象类可以包含**有**实现的函数和属性，也可以包含**没有**实现的函数和属性，后者被称为抽象函数和属性。

To create an abstract class, use the `abstract` keyword:
<br />要创建抽象类，请使用 `abstract` 关键字：

```kotlin
abstract class Animal
```

To declare a function or a property **without** an implementation, you also use the `abstract` keyword:
<br />要声明一个**没有**实现的函数或属性，也可以使用 `abstract` 关键字：

```kotlin
abstract fun makeSound()
abstract val sound: String
```

For example, let's say that you want to create an abstract class called `Product` that you can create child classes from
to define different product categories:
<br />例如，假设你想创建一个名为 `Product` 的抽象类，你可以从中创建子类来定义不同的产品类别：

```kotlin
abstract class Product(val name: String, var price: Double) {
    // Abstract property for the product category
    abstract val category: String

    // A function that can be shared by all products
    fun productInfo(): String {
        return "Product: $name, Category: $category, Price: $price"
    }
}
```

In the abstract class:
<br />在抽象类中：

* The constructor has two parameters for the product's `name` and `price`.
<br />构造函数有两个参数，分别用于产品`name`和`price`。
* There is an abstract property that contains the product category as a string.
<br />存在一个抽象属性，其中包含产品类别字符串。
* There is a function that prints information about the product.
<br />有一个函数可以打印有关产品的信息。

Let's create a child class for electronics. Before you define an implementation for the `category` property in the child class,
you must use the `override` keyword:
<br />让我们创建一个电子产品子类。在子类中定义 `category` 属性的实现之前，必须使用 `override` 关键字：

```kotlin
class Electronic(name: String, price: Double, val warranty: Int) : Product(name, price) {
    override val category = "Electronic"
}
```

The `Electronic` class:
<br />`Electronic` 类:

* Inherits from the `Product` abstract class.
<br />继承自 `Product` 抽象类。
* Has an additional parameter in the constructor: `warranty`, which is specific to electronics.
<br />构造函数中还有一个额外的参数：`warranty`，这是电子产品特有的。
* Overrides the `category` property to contain the string `"Electronic"`.
<br />覆盖 `category` 属性，使其包含字符串`"Electronic"`。

Now, you can use these classes like this:
<br />现在，你可以像这样使用这些类：

```kotlin
abstract class Product(val name: String, var price: Double) {
    // Abstract property for the product category
    abstract val category: String

    // A function that can be shared by all products
    fun productInfo(): String {
        return "Product: $name, Category: $category, Price: $price"
    }
}

class Electronic(name: String, price: Double, val warranty: Int) : Product(name, price) {
    override val category = "Electronic"
}

//sampleStart
fun main() {
    // Creates an instance of the Electronic class
    val laptop = Electronic(name = "Laptop", price = 1000.0, warranty = 2)

    println(laptop.productInfo())
    // Product: Laptop, Category: Electronic, Price: 1000.0
}
//sampleEnd
```
{kotlin-runnable="true" id="kotlin-tour-abstract-class"}

Although abstract classes are great for sharing code in this way, they are restricted because classes in Kotlin
only support single inheritance. If you need to inherit from multiple sources, consider using interfaces.
<br />虽然抽象类非常适合以这种方式共享代码，但它们也存在局限性，因为 Kotlin 中的类只支持单继承。如果需要继承自多个源，请考虑使用接口。

## Interfaces
接口

Interfaces are similar to classes, but they have some differences:
<br />接口与类类似，但也存在一些差异：

* You can't create an instance of an interface. They don't have a constructor or header.
<br />你不能创建接口的实例。接口没有构造函数或头文件。
* Their functions and properties are implicitly inheritable by default. In Kotlin, we say that they are "open."
<br />它们的函数和属性默认情况下是可隐式继承的。在 Kotlin 中，我们称它们是“开放的”。
* You don't need to mark their functions as `abstract` if you don't give them an implementation.
<br />如果你不给函数提供实现，就不需要将它们标记为`抽象`函数。

Similar to abstract classes, you use interfaces to define a set of functions and properties that classes can inherit and
implement later. This approach helps you focus on the abstraction described by the interface, rather than the specific
implementation details. Using interfaces makes your code:
<br />与抽象类类似，接口用于定义一组函数和属性，供其他类继承和实现。这种方法有助于您专注于接口所描述的抽象概念，而不是具体的实现细节。
使用接口可以使您的代码：

* More modular, as it isolates different parts, allowing them to evolve independently.
<br />模块化程度更高，因为它将不同的部分隔离开来，使它们能够独立发展。
* Easier to understand by grouping related functions into a cohesive set.
<br />将相关功能分组到一个统一的集合中，更容易理解。
* Easier to test, as you can quickly swap an implementation with a mock for testing.
<br />更容易测试，因为您可以快速地将实现替换为模拟进行测试。

To declare an interface, use the `interface` keyword:
<br />要声明接口，请使用 `interface` 关键字：

```kotlin
interface PaymentMethod
```

### Interface implementation
接口实现

Interfaces support multiple inheritance so a class can implement multiple interfaces at once. First, let's consider
the scenario where a class implements **one** interface.
<br />接口支持多重继承，因此一个类可以同时实现多个接口。首先，我们考虑一个类只实现**一个**接口的情况。

To create a class that implements an interface, add a colon after your class header, followed by the interface name
that you want to implement. You don't use parentheses `()` after the interface name because interfaces don't have a 
constructor:
<br />要创建一个实现接口的类，请在类头后添加一个冒号，后跟要实现的接口名称。接口名称后不需要使用括号 `()`，因为接口没有构造函数：

```kotlin
class CreditCardPayment : PaymentMethod
```

For example:
<br />例如：

```kotlin
interface PaymentMethod {
    // Functions are inheritable by default
    fun initiatePayment(amount: Double): String
}

class CreditCardPayment(val cardNumber: String, val cardHolderName: String, val expiryDate: String) : PaymentMethod {
    override fun initiatePayment(amount: Double): String {
        // Simulate processing payment with credit card
        return "Payment of $$amount initiated using Credit Card ending in ${cardNumber.takeLast(4)}."
    }
}

fun main() {
    val paymentMethod = CreditCardPayment("1234 5678 9012 3456", "John Doe", "12/25")
    println(paymentMethod.initiatePayment(100.0))
    // Payment of $100.0 initiated using Credit Card ending in 3456.
}
```
{kotlin-runnable="true" id="kotlin-tour-interface-inheritance"}

In the example:
<br />例如:

* `PaymentMethod` is an interface that has an `initiatePayment()` function without an implementation.
<br />`PaymentMethod` 是一个接口，它有一个 `initiatePayment()` 函数，但没有实现。
* `CreditCardPayment` is a class that implements the `PaymentMethod` interface.
<br />`CreditCardPayment` 是一个实现了 `PaymentMethod` 接口的类。
* The `CreditCardPayment` class overrides the inherited `initiatePayment()` function.
<br />`CreditCardPayment` 类重写了继承的 `initiatePayment()` 函数。
* `paymentMethod` is an instance of the `CreditCardPayment` class.
<br />`paymentMethod` 是 `CreditCardPayment` 类的一个实例。
* The overridden `initiatePayment()` function is called on the `paymentMethod` instance with a parameter of `100.0`.
<br />重写的 `initiatePayment()` 函数在 `paymentMethod` 实例上被调用，参数为 `100.0`。

To create a class that implements **multiple** interfaces, add a colon after your class header followed by the name of the interfaces
that you want to implement separated by a comma:
<br />要创建一个实现**多个**接口的类，请在类名后添加一个冒号，后跟要实现的接口名称，并用逗号分隔：

```kotlin
class CreditCardPayment : PaymentMethod, PaymentType
```

For example:
<br />例如:

```kotlin
interface PaymentMethod {
    fun initiatePayment(amount: Double): String
}

interface PaymentType {
    val paymentType: String
}

class CreditCardPayment(val cardNumber: String, val cardHolderName: String, val expiryDate: String) : PaymentMethod,
    PaymentType {
    override fun initiatePayment(amount: Double): String {
        // Simulate processing payment with credit card
        return "Payment of $$amount initiated using Credit Card ending in ${cardNumber.takeLast(4)}."
    }

    override val paymentType: String = "Credit Card"
}

fun main() {
    val paymentMethod = CreditCardPayment("1234 5678 9012 3456", "John Doe", "12/25")
    println(paymentMethod.initiatePayment(100.0))
    // Payment of $100.0 initiated using Credit Card ending in 3456.

    println("Payment is by ${paymentMethod.paymentType}")
    // Payment is by Credit Card
}
```
{kotlin-runnable="true" id="kotlin-tour-interface-multiple-inheritance"}

In the example:
<br />例如:

* `PaymentMethod` is an interface that has the `initiatePayment()` function without an implementation.
<br />`PaymentMethod` 是一个接口，它具有 `initiatePayment()` 函数，但没有实现。
* `PaymentType` is an interface that has the `paymentType` property that isn't initialized.
<br />`PaymentType` 是一个接口，它有一个未初始化的 `paymentType` 属性。
* `CreditCardPayment` is a class that implements the `PaymentMethod` and `PaymentType` interfaces.
<br />`CreditCardPayment` 是一个实现了 `PaymentMethod` 和 `PaymentType` 接口的类。
* The `CreditCardPayment` class overrides the inherited `initiatePayment()` function and the `paymentType` property.
<br />`CreditCardPayment` 类重写了继承的 `initiatePayment()` 函数和 `paymentType` 属性。
* `paymentMethod` is an instance of the `CreditCardPayment` class.
<br />`paymentMethod` 是 `CreditCardPayment` 类的一个实例。
* The overridden `initiatePayment()` function is called on the `paymentMethod` instance with a parameter of `100.0`.
<br />重写的 `initiatePayment()` 函数在 `paymentMethod` 实例上被调用，参数为 `100.0`。
* The overridden `paymentType` property is accessed on the `paymentMethod` instance.
<br />在 `paymentMethod` 实例上可以访问重写的 `paymentType` 属性。

For more information about interfaces and interface inheritance, see [Interfaces](interfaces.md).
<br />有关接口和接口继承的更多信息，请参阅[接口](interfaces.md)。

## Delegation
<br /> 委托

Interfaces are useful, but if your interface contains many functions, its child classes can end up with a lot of 
boilerplate code. If you only want to override a small part of a class's behavior, you need to repeat yourself a lot.
<br />接口很有用，但如果接口包含很多函数，其子类最终可能会编写大量样板代码。如果您只想重写类的一小部分行为，则需要重复编写很多代码。

> Boilerplate code is a chunk of code that is reused with little or no alteration in multiple parts of a software project.
> <br />样板代码是指在软件项目的多个部分中几乎无需修改即可重复使用的一段代码。
> 
{style="tip"}

For example, let's say that you have an interface called `DrawingTool` that contains a number of functions and one property
called `color`:
<br />例如，假设你有一个名为 `DrawingTool` 的接口，其中包含许多函数和一个名为 `color` 的属性：

```kotlin
interface DrawingTool {
    val color: String
    fun draw(shape: String)
    fun erase(area: String)
    fun getToolInfo(): String
}
```

You create a class called `PenTool` which implements the `DrawingTool` interface and provides implementations for all of
its members:
<br />您创建一个名为 `PenTool` 的类，该类实现了 `DrawingTool` 接口，并为其所有成员提供了实现：

```kotlin
class PenTool : DrawingTool {
    override val color: String = "black"

    override fun draw(shape: String) {
        println("Drawing $shape using a pen in $color")
    }

    override fun erase(area: String) {
        println("Erasing $area with pen tool")
    }

    override fun getToolInfo(): String {
        return "PenTool(color=$color)"
    }
}
```

You want to create a class like `PenTool` with the same behavior but a different `color` value. 
One approach is to create a new class that expects an object implementing the `DrawingTool` interface as a parameter,
like a `PenTool` class instance. Then, inside the class, you can override the `color` property.
<br />你想创建一个类似 `PenTool` 的类，它的行为与 PenTool 相同，但`color`值不同。一种方法是创建一个新类，
该类接受一个实现了 `DrawingTool` 接口的对象作为参数，例如 `PenTool` 类的实例。然后，在该类内部，你可以重写`color`属性。

But in this scenario, you need to add implementations for each member of the `DrawingTool` interface:
<br />但在这种情况下，您需要为 `DrawingTool` 接口的每个成员添加实现：

```kotlin
interface DrawingTool {
    val color: String
    fun draw(shape: String)
    fun erase(area: String)
    fun getToolInfo(): String
}

class PenTool : DrawingTool {
    override val color: String = "black"

    override fun draw(shape: String) {
        println("Drawing $shape using a pen in $color")
    }

    override fun erase(area: String) {
        println("Erasing $area with pen tool")
    }

    override fun getToolInfo(): String {
        return "PenTool(color=$color)"
    }
}
//sampleStart
class CanvasSession(val tool: DrawingTool) : DrawingTool {
    override val color: String = "blue"

    override fun draw(shape: String) {
        tool.draw(shape)
    }

    override fun erase(area: String) {
        tool.erase(area)
    }

    override fun getToolInfo(): String {
        return tool.getToolInfo()
    }
}
//sampleEnd
fun main() {
    val pen = PenTool()
    val session = CanvasSession(pen)

    println("Pen color: ${pen.color}")
    // Pen color: black

    println("Session color: ${session.color}")
    // Session color: blue

    session.draw("circle")
    // Drawing circle with pen in black

    session.erase("top-left corner")
    // Erasing top-left corner with pen tool

    println(session.getToolInfo())
    // PenTool(color=black)
}
```
{kotlin-runnable="true" id="kotlin-tour-interface-non-delegation"}

You can see that if you have a large number of member functions in the `DrawingTool` interface, the amount of boilerplate
code in the `CanvasSession` class can be large. However, there is an alternative.
<br />您可以看到，如果 `DrawingTool` 接口中有很多成员函数，`CanvasSession` 类中的样板代码量也会很大。不过，还有一种替代方案。

In Kotlin, you can delegate the interface implementation to a class instance using the `by` keyword. For example:
<br />在 Kotlin 中，可以使用 `by` 关键字将接口实现委托给类实例。例如：

```kotlin
class CanvasSession(val tool: DrawingTool) : DrawingTool by tool
```

Here, `tool` is the name of the `PenTool` class instance where the implementations of member functions are delegated to.
<br />这里，`tool` 是 `PenTool` 类实例的名称，成员函数的实现都委托给该实例。

Now you don't have to add implementations for the member functions in the `CanvasSession` class. The compiler does
this for you automatically from the `PenTool` class. This saves you from having to write a lot of boilerplate code. Instead,
you add code only for the behavior you want to change for your child class. 
<br />现在，您无需再为 `CanvasSession` 类中的成员函数添加实现。编译器会自动从 `PenTool` 类中完成这些操作。
这省去了您编写大量样板代码的麻烦。您只需为子类中需要更改的行为添加代码即可。

For example, if you want to change the value of the `color` property:
<br />例如，如果您想更改`color`属性的值：

```kotlin
interface DrawingTool {
    val color: String
    fun draw(shape: String)
    fun erase(area: String)
    fun getToolInfo(): String
}

class PenTool : DrawingTool {
    override val color: String = "black"

    override fun draw(shape: String) {
        println("Drawing $shape using a pen in $color")
    }

    override fun erase(area: String) {
        println("Erasing $area with pen tool")
    }

    override fun getToolInfo(): String {
        return "PenTool(color=$color)"
    }
}

//sampleStart
class CanvasSession(val tool: DrawingTool) : DrawingTool by tool {
    // No boilerplate code!
    override val color: String = "blue"
}
//sampleEnd
fun main() {
    val pen = PenTool()
    val session = CanvasSession(pen)

    println("Pen color: ${pen.color}")
    // Pen color: black

    println("Session color: ${session.color}")
    // Session color: blue

    session.draw("circle")
    // Drawing circle with pen in black

    session.erase("top-left corner")
    // Erasing top-left corner with pen tool

    println(session.getToolInfo())
    // PenTool(color=black)
}
```
{kotlin-runnable="true" id="kotlin-tour-interface-delegation"}

If you want to, you can also override the behavior of an inherited member function in the `CanvasSession` class, but now
you don't have to add new lines of code for every inherited member function.
<br />如果你愿意，你也可以重写 `CanvasSession` 类中继承的成员函数的行为，但现在你不必为每个继承的成员函数添加新的代码行了。

For more information, see [Delegation](delegation.md).
<br />更多信息请参见[委托](delegation.md)部分。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="classes-interfaces-exercise-1"}

Imagine you're working on a smart home system. A smart home typically has different types of devices that all have some
basic features but also unique behaviors. In the code sample below, complete the `abstract` class called `SmartDevice` 
so that the child class `SmartLight` can compile successfully.
<br />假设你正在开发一个智能家居系统。智能家居通常包含不同类型的设备，这些设备都具有一些基本功能，但也各有其独特的行为。
在下面的代码示例中，请完善名为 `SmartDevice` 的抽象类，以便子类 `SmartLight` 能够成功编译。

Then, create another child class called `SmartThermostat` that inherits from the `SmartDevice` class and implements 
`turnOn()` and `turnOff()` functions that return print statements describing which thermostat is heating or turned off.
Finally, add another function called `adjustTemperature()` that accepts a temperature measurement as an input and prints:
`$name thermostat set to $temperature°C.`
<br />然后，创建一个名为 `SmartThermostat` 的子类，该类继承自 `SmartDevice` 类，并实现 `turnOn()` 和 `turnOff()` 函数，
这两个函数会返回打印语句，描述哪个恒温器正在加热或关闭。
最后，添加一个名为 `adjustTemperature()` 的函数，该函数接受一个温度测量值作为输入，并打印：
`$name thermostat set to $temperature°C。`

<deflist collapsible="true">
    <def title="Hint">
        In the <code>SmartDevice</code> class, add the <code>turnOn()</code> and <code>turnOff()</code> functions so that 
        you can override their behavior later in the <code>SmartThermostat</code> class.
        <br />在 <code>SmartDevice</code> 类中，添加 <code>turnOn()</code> 和 <code>turnOff()</code> 函数，以便
        稍后可以在 <code>SmartThermostat</code> 类中重写它们的行为。
    </def>
</deflist>

|--|--|

```kotlin
abstract class // Write your code here

class SmartLight(name: String) : SmartDevice(name) {
    override fun turnOn() {
        println("$name is now ON.")
    }

    override fun turnOff() {
        println("$name is now OFF.")
    }

   fun adjustBrightness(level: Int) {
        println("Adjusting $name brightness to $level%.")
    }
}

class SmartThermostat // Write your code here

fun main() {
    val livingRoomLight = SmartLight("Living Room Light")
    val bedroomThermostat = SmartThermostat("Bedroom Thermostat")
    
    livingRoomLight.turnOn()
    // Living Room Light is now ON.
    livingRoomLight.adjustBrightness(10)
    // Adjusting Living Room Light brightness to 10%.
    livingRoomLight.turnOff()
    // Living Room Light is now OFF.

    bedroomThermostat.turnOn()
    // Bedroom Thermostat thermostat is now heating.
    bedroomThermostat.adjustTemperature(5)
    // Bedroom Thermostat thermostat set to 5°C.
    bedroomThermostat.turnOff()
    // Bedroom Thermostat thermostat is now off.
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-interfaces-exercise-1"}

|---|---|
```kotlin
abstract class SmartDevice(val name: String) {
    abstract fun turnOn()
    abstract fun turnOff()
}

class SmartLight(name: String) : SmartDevice(name) {
    override fun turnOn() {
        println("$name is now ON.")
    }

    override fun turnOff() {
        println("$name is now OFF.")
    }

   fun adjustBrightness(level: Int) {
        println("Adjusting $name brightness to $level%.")
    }
}

class SmartThermostat(name: String) : SmartDevice(name) {
    override fun turnOn() {
        println("$name thermostat is now heating.")
    }

    override fun turnOff() {
        println("$name thermostat is now off.")
    }

   fun adjustTemperature(temperature: Int) {
        println("$name thermostat set to $temperature°C.")
    }
}


fun main() {
    val livingRoomLight = SmartLight("Living Room Light")
    val bedroomThermostat = SmartThermostat("Bedroom Thermostat")
    
    livingRoomLight.turnOn()
    // Living Room Light is now ON.
    livingRoomLight.adjustBrightness(10)
    // Adjusting Living Room Light brightness to 10%.
    livingRoomLight.turnOff()
    // Living Room Light is now OFF.

    bedroomThermostat.turnOn()
    // Bedroom Thermostat thermostat is now heating.
    bedroomThermostat.adjustTemperature(5)
    // Bedroom Thermostat thermostat set to 5°C.
    bedroomThermostat.turnOff()
    // Bedroom Thermostat thermostat is now off.
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-interfaces-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="classes-interfaces-exercise-2"}

Create an interface called `Media` that you can use to implement specific media classes like `Audio`, `Video`, or 
`Podcast`. Your interface must include:
<br />创建一个名为 `Media` 的接口，用于实现特定的媒体类，例如 `Audio`、`Video` 或 `Podcast`。该接口必须包含以下内容：

* A property called `title` to represent the title of the media.
<br />一个名为 `title` 的属性，用于表示媒体的标题。
* A function called `play()` to play the media.
<br />一个名为 `play()` 的函数，用于播放媒体。

Then, create a class called `Audio` that implements the `Media` interface. The `Audio` class must use the `title` property
in its constructor as well as have an additional property called `composer` that has `String` type. In the class, implement
the `play()` function to print the following: `"Playing audio: $title, composed by $composer"`.
<br />然后，创建一个名为 `Audio` 的类，并实现 `Media` 接口。`Audio` 类必须在其构造函数中使用 `title` 属性，
并且还需要一个名为 `composer` 的附加属性，该属性的类型为 `String`。在该类中，实现 `play()` 函数，
以打印以下内容：`"Playing audio: $title, composed by $composer"`。

<deflist collapsible="true">
    <def title="Hint">
        You can use the <code>override</code> keyword in class headers to implement a property from an interface in the constructor.
        <br />您可以在类头中使用 <code>override</code> 关键字，在构造函数中实现接口中的属性。
    </def>
</deflist>

|---|---|
```kotlin
interface // Write your code here

class // Write your code here

fun main() {
    val audio = Audio("Symphony No. 5", "Beethoven")
    audio.play()
   // Playing audio: Symphony No. 5, composed by Beethoven
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-interfaces-exercise-2"}

|---|---|
```kotlin
interface Media {
    val title: String
    fun play()
}

class Audio(override val title: String, val composer: String) : Media {
    override fun play() {
        println("Playing audio: $title, composed by $composer")
    }
}

fun main() {
    val audio = Audio("Symphony No. 5", "Beethoven")
    audio.play()
   // Playing audio: Symphony No. 5, composed by Beethoven
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-interfaces-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="classes-interfaces-exercise-3"}

You're building a payment processing system for an e-commerce application. Each payment method needs to be able to 
authorize a payment and process a transaction. Some payments also need to be able to process refunds.
<br />您正在为一款电子商务应用构建支付处理系统。每种支付方式都需要能够授权付款并处理交易。部分支付方式还需要能够处理退款。

1. In the `Refundable` interface, add a function called `refund()` to process refunds.
<br />在 `Refundable` 接口中，添加一个名为 `refund()` 的函数来处理退款。

2. In the `PaymentMethod` abstract class:
<br />在 `PaymentMethod` 抽象类中：
   * Add a function called `authorize()` that takes an amount and prints a message containing the amount.
   <br />添加一个名为 `authorize()` 的函数，该函数接受一个金额参数并打印包含该金额的消息。
   * Add an abstract function called `processPayment()` that also takes an amount.
   <br />添加一个名为 `processPayment()` 的抽象函数，该函数也接受一个金额参数。

3. Create a class called `CreditCard` that implements the `Refundable` interface and `PaymentMethod` abstract class.
In this class, add implementations for the `refund()` and `processPayment()` functions so that they print the following 
statements:
<br />创建一个名为 `CreditCard` 的类，该类实现 `Refundable` 接口和 `PaymentMethod` 抽象类。
在该类中，添加 `refund()` 和 `processPayment()` 函数的实现，使其打印以下语句：
   * `"Refunding $amount to the credit card."`
   * `"Processing credit card payment of $amount."`

|---|---|
```kotlin
interface Refundable {
    // Write your code here
}

abstract class PaymentMethod(val name: String) {
    // Write your code here
}

class CreditCard // Write your code here

fun main() {
    val visa = CreditCard("Visa")
    
    visa.authorize(100.0)
    // Authorizing payment of $100.0.
    visa.processPayment(100.0)
    // Processing credit card payment of $100.0.
    visa.refund(50.0)
    // Refunding $50.0 to the credit card.
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-interfaces-exercise-3"}

|---|---|
```kotlin
interface Refundable {
    fun refund(amount: Double)
}

abstract class PaymentMethod(val name: String) {
    fun authorize(amount: Double) {
        println("Authorizing payment of $$amount.")
    }

    abstract fun processPayment(amount: Double)
}

class CreditCard(name: String) : PaymentMethod(name), Refundable {
    override fun processPayment(amount: Double) {
        println("Processing credit card payment of $$amount.")
    }

    override fun refund(amount: Double) {
        println("Refunding $$amount to the credit card.")
    }
}

fun main() {
    val visa = CreditCard("Visa")
    
    visa.authorize(100.0)
    // Authorizing payment of $100.0.
    visa.processPayment(100.0)
    // Processing credit card payment of $100.0.
    visa.refund(50.0)
    // Refunding $50.0 to the credit card.
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-interfaces-solution-3"}

### Exercise 4 {initial-collapse-state="collapsed" collapsible="true" id="classes-interfaces-exercise-4"}

You have a simple messaging app that has some basic functionality, but you want to add some functionality for 
_smart_ messages without significantly duplicating your code.
<br />你有一个功能简单的即时通讯应用，具备一些基本功能，但你想添加一些智能消息功能，同时又不想大幅重复编写代码。

In the code below, define a class called `SmartMessenger` that inherits from the `Messenger` interface but delegates 
the implementation to an instance of the `BasicMessenger` class. 
<br />在下面的代码中，定义一个名为 `SmartMessenger` 的类，该类继承自 `Messenger` 接口，但将实现委托给 `BasicMessenger` 类的一个实例。

In the `SmartMessenger` class, override the `sendMessage()` function to send smart messages. The function must accept
a `message` as an input and return a printed statement: `"Sending a smart message: $message"`. In addition, call the 
`sendMessage()` function from the `BasicMessenger` class and prefix the message with `[smart]`.
<br />在 `SmartMessenger` 类中，重写 `sendMessage()` 函数以发送智能消息。该函数必须接受一个 `message` 作为输入，
并返回一条打印语句：`"Sending a smart message: $message"`。此外，还要从 `BasicMessenger` 类中调用 `sendMessage()` 函数，
并在消息前加上 `[smart]` 前缀。

> You don't need to rewrite the `receiveMessage()` function in the `SmartMessenger` class.
> <br />你不需要重写 `SmartMessenger` 类中的 `receiveMessage()` 函数。
> 
{style="note"}

|--|--|

```kotlin
interface Messenger {
    fun sendMessage(message: String)
    fun receiveMessage(): String
}

class BasicMessenger : Messenger {
    override fun sendMessage(message: String) {
        println("Sending message: $message")
    }

    override fun receiveMessage(): String {
        return "You've got a new message!"
    }
}

class SmartMessenger // Write your code here

fun main() {
    val basicMessenger = BasicMessenger()
    val smartMessenger = SmartMessenger(basicMessenger)
    
    basicMessenger.sendMessage("Hello!")
    // Sending message: Hello!
    println(smartMessenger.receiveMessage())
    // You've got a new message!
    smartMessenger.sendMessage("Hello from SmartMessenger!")
    // Sending a smart message: Hello from SmartMessenger!
    // Sending message: [smart] Hello from SmartMessenger!
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-classes-interfaces-exercise-4"}

|---|---|
```kotlin
interface Messenger {
    fun sendMessage(message: String)
    fun receiveMessage(): String
}

class BasicMessenger : Messenger {
    override fun sendMessage(message: String) {
        println("Sending message: $message")
    }

    override fun receiveMessage(): String {
        return "You've got a new message!"
    }
}

class SmartMessenger(val basicMessenger: BasicMessenger) : Messenger by basicMessenger {
    override fun sendMessage(message: String) {
        println("Sending a smart message: $message")
        basicMessenger.sendMessage("[smart] $message")
    }
}

fun main() {
    val basicMessenger = BasicMessenger()
    val smartMessenger = SmartMessenger(basicMessenger)
    
    basicMessenger.sendMessage("Hello!")
    // Sending message: Hello!
    println(smartMessenger.receiveMessage())
    // You've got a new message!
    smartMessenger.sendMessage("Hello from SmartMessenger!")
    // Sending a smart message: Hello from SmartMessenger!
    // Sending message: [smart] Hello from SmartMessenger!
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-classes-interfaces-solution-4"}

## Next step
接下来

[Intermediate: Objects](kotlin-tour-intermediate-objects.md)
<br />[中级：对象](kotlin-tour-intermediate-objects.md)
