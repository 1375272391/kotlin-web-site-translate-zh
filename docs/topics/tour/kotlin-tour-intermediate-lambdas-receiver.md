[//]: # (title: Intermediate: Lambda expressions with receiver)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3.svg" width="20" alt="Third step" /> <strong>Lambda expressions with receiver</strong><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In this chapter, you'll learn how to use receivers with another type of function, lambda expressions, and how they
can help you create a domain-specific language.
<br />在本章中，你将学习如何将接收器与另一种类型的函数——lambda表达式——一起使用，以及它们如何 帮助你创建特定领域的语言。

## Lambda expressions with receiver
带有接收器的 Lambda 表达式

In the beginner tour, you learned how to use [lambda expressions](kotlin-tour-functions.md#lambda-expressions). Lambda expressions can also have a receiver. 
In this case, lambda expressions can access any member functions or properties of the receiver without having
to explicitly specify the receiver each time. Without these additional references, your code is easier to read and maintain.
    <br />在入门教程中，您学习了如何使用[lambda表达式](kotlin-tour-functions.md#lambda-expressions)。lambda表达式也可以拥有接收者。
在这种情况下，lambda表达式可以访问接收者的任何成员函数或属性，而无需
每次都显式指定接收者。省去了这些额外的引用，您的代码更易于阅读和维护。

> Lambda expressions with receiver are also known as function literals with receiver.
> <br />带有接收器的 Lambda 表达式也称为带有接收器的函数字面量。
>
{style="tip"}

The syntax for a lambda expression with receiver is different when you define the function type. First, write the receiver
that you want to extend. Next, put a `.` and then complete the rest of your function type definition. For example:
<br />定义函数类型时，带接收器的 lambda 表达式的语法有所不同。首先，编写要扩展的接收器。
然后，添加一个点号`.`，并完成函数类型定义的其余部分。例如：

```kotlin
MutableList<Int>.() -> Unit
```

This function type has:
<br />此函数类型具有：

* `MutableList<Int>` as the receiver.
        <br />`MutableList<Int>` 作为接收器。
* No function parameters within the parentheses `()`.
        <br />括号 `()` 内没有函数参数。
* No return value: `Unit`.
        <br />无返回值：`Unit`。

Consider this example that draws shapes on a canvas:
    <br />请看下面这个在画布上绘制形状的例子：

```kotlin
class Canvas {
    fun drawCircle() = println("🟠 Drawing a circle")
    fun drawSquare() = println("🟥 Drawing a square")
}

// Lambda expression with receiver definition
fun render(block: Canvas.() -> Unit): Canvas {
    val canvas = Canvas()
    // Use the lambda expression with receiver
    canvas.block()
    return canvas
}

fun main() {
    render {
        drawCircle()
        // 🟠 Drawing a circle
        drawSquare()
        // 🟥 Drawing a square
    }
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-intermediate-tour-lambda-expression-with-receiver"}

In this example:
    <br />在这个例子中：

* The `Canvas` class has two functions that simulate drawing a circle or a square.
        <br />`Canvas` 类有两个函数，可以模拟绘制圆形或正方形。
* The `render()` function takes a `block` parameter and returns an instance of the `Canvas` class.
        <br />`render()` 函数接受一个 `block` 参数，并返回一个 `Canvas` 类的实例。
* The `block` parameter is a lambda expression with receiver, where the `Canvas` class is the receiver.
        <br />`block` 参数是一个带有接收器的 lambda 表达式，其中 `Canvas` 类是接收器。
* The `render()` function creates an instance of the `Canvas` class and calls the `block()` lambda expression on the `canvas` instance, using it as the receiver.
        <br />`render()` 函数创建 `Canvas` 类的实例，并调用 `canvas` 实例上的 `block()` lambda 表达式，将其用作接收器。
* The `main()` function calls the `render()` function with a lambda expression, which is passed to the `block` parameter.
        <br />`main()` 函数使用 lambda 表达式调用 `render()` 函数，并将 lambda 表达式传递给 `block` 参数。
* Inside the lambda passed to the `render()` function, the program calls the `drawCircle()` and `drawSquare()` functions on an instance of the `Canvas` class.
        <br />在传递给 `render()` 函数的 lambda 表达式中，程序在 `Canvas` 类的实例上调用 `drawCircle()` 和 `drawSquare()` 函数。

  Because the `drawCircle()` and `drawSquare()` functions are called in the lambda expression with receiver, they can be called
  directly as if they are inside the `Canvas` class.
      <br />因为 `drawCircle()` 和 `drawSquare()` 函数是在带有接收器的 lambda 表达式中调用的，所以它们可以
      直接调用，就像它们在 `Canvas` 类中一样。

Lambda expressions with receiver are helpful when you want to create a domain-specific language (DSL). Since you have
access to the receiver's member functions and properties without explicitly referencing the receiver, your code 
becomes leaner.
    <br />当您想要创建领域特定语言 (DSL) 时，带有接收器的 Lambda 表达式非常有用。因为您无需显式引用接收器即可访问其成员函数和属性，所以您的代码会更加简洁。

To demonstrate this, consider an example that configures items in a menu. Let's begin with a `MenuItem` class and a 
`Menu` class that contains a function to add items to the menu called `item()`, as well as a list of all menu items `items`:
    <br />为了演示这一点，我们来看一个配置菜单项的示例。首先，我们定义一个 `MenuItem` 类和一个 `Menu` 类。
    `Menu` 类包含一个用于向菜单添加项的函数 `item()`，以及一个包含所有菜单项的列表 `items`：

```kotlin
class MenuItem(val name: String)

class Menu(val name: String) {
    val items = mutableListOf<MenuItem>()

    fun item(name: String) {
        items.add(MenuItem(name))
    }
}
```

Let's use a lambda expression with receiver passed as a function parameter (`init`) to the `menu()` function that builds 
a menu as a starting point:
    <br />让我们使用 lambda 表达式，并将接收器作为函数参数（`init`）传递给构建菜单的 `menu()` 函数作为起点：

```kotlin
fun menu(name: String, init: Menu.() -> Unit): Menu {
    // Creates an instance of the Menu class
    val menu = Menu(name)
    // Calls the lambda expression with receiver init() on the class instance
    menu.init()
    return menu
}
```

Now you can use the DSL to configure a menu and create a `printMenu()` function to print the menu structure to the console:
    <br />现在您可以使用 DSL 配置菜单，并创建一个 `printMenu()` 函数将菜单结构打印到控制台：

```kotlin
class MenuItem(val name: String)

class Menu(val name: String) {
    val items = mutableListOf<MenuItem>()

    fun item(name: String) {
        items.add(MenuItem(name))
    }
}

fun menu(name: String, init: Menu.() -> Unit): Menu {
    val menu = Menu(name)
    menu.init()
    return menu
}

//sampleStart
fun printMenu(menu: Menu) {
    println("Menu: ${menu.name}")
    menu.items.forEach { println("  Item: ${it.name}") }
}

// Use the DSL
fun main() {
    // Create the menu
    val mainMenu = menu("Main Menu") {
        // Add items to the menu
        item("Home")
        item("Settings")
        item("Exit")
    }

    // Print the menu
    printMenu(mainMenu)
    // Menu: Main Menu
    //   Item: Home
    //   Item: Settings
    //   Item: Exit
}
//sampleEnd
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-intermediate-tour-lambda-expression-with-receiver-dsl"}

As you can see, using a lambda expression with receiver greatly simplifies the code needed to create your menu. Lambda 
expressions are not only useful for setup and creation but also for configuration. They are commonly used in building 
DSLs for APIs, UI frameworks, and configuration builders to produce streamlined code, allowing you to focus more easily 
on the underlying code structure and logic.
    <br />如您所见，使用带有接收器的 lambda 表达式可以极大地简化创建菜单所需的代码。Lambda 表达式不仅对设置和创建有用，而且对配置也很有用。
它们常用于构建 API、UI 框架和配置构建器的 DSL，以生成精简的代码，使您能够更轻松地专注于底层代码结构和逻辑。

Kotlin's ecosystem has many examples of this design pattern, such as in the [`buildList()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-list.html)
and [`buildString()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/build-string.html) functions from the 
standard library.
    <br />Kotlin 的生态系统中有很多这种设计模式的例子，例如标准库中的 [`buildList()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/build-list.html) 函数
    以及 [`buildString()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/build-string.html) 函数。

> Lambda expressions with receivers can be combined with **type-safe builders** in Kotlin to make DSLs that detect any problems
> with types at compile time rather than at runtime. To learn more, see [Type-safe builders](type-safe-builders.md).
>     <br />在 Kotlin 中，可以将带有接收器的 Lambda 表达式与**类型安全构建器**结合使用，从而创建 DSL，以便在编译时而不是运行时检测类型问题。
>     要了解更多信息，请参阅[类型安全构建器](type-safe-builders.md)。
>
{style="tip"}

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="lambda-receivers-exercise-1"}

You have a `fetchData()` function that accepts a lambda expression with receiver. Update the lambda expression to use 
the `append()` function so that the output of your code is: `Data received - Processed`.
    <br />你有一个 `fetchData()` 函数，它接受一个带有接收器的 lambda 表达式。
    请更新该 lambda 表达式，使其使用 `append()` 函数，以便代码的输出为：`Data received - Processed`。

|---|---|
```kotlin
fun fetchData(callback: StringBuilder.() -> Unit) {
    val builder = StringBuilder("Data received")
    builder.callback()
}

fun main() {
    fetchData {
        // Write your code here
        // Data received - Processed
    }
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-receivers-exercise-1"}

|---|---|
```kotlin
fun fetchData(callback: StringBuilder.() -> Unit) {
    val builder = StringBuilder("Data received")
    builder.callback()
}

fun main() {
    fetchData {
        append(" - Processed")
        println(this.toString())
        // Data received - Processed
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-lambda-receivers-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="lambda-receivers-exercise-2"}

You have a `Button` class and `ButtonEvent` and `Position` data classes. Write some code that triggers the `onEvent()`
member function of the `Button` class to trigger a double-click event. Your code should print `"Double click!"`.
    <br />你有一个 `Button` 类以及 `ButtonEvent` 和 `Position` 数据类。编写一些代码来触发 `Button` 类的 `onEvent()` 成员函数，以触发双击事件。
    你的代码应该打印出 `"Double click!"`。

```kotlin
class Button {
    fun onEvent(action: ButtonEvent.() -> Unit) {
        // Simulate a double-click event (not a right-click)
        val event = ButtonEvent(isRightClick = false, amount = 2, position = Position(100, 200))
        event.action() // Trigger the event callback
    }
}

data class ButtonEvent(
    val isRightClick: Boolean,
    val amount: Int,
    val position: Position
)

data class Position(
    val x: Int,
    val y: Int
)

fun main() {
    val button = Button()

    button.onEvent {
        // Write your code here
        // Double click!
    }
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-receivers-exercise-2"}

|---|---|
```kotlin
class Button {
    fun onEvent(action: ButtonEvent.() -> Unit) {
        // Simulate a double-click event (not a right-click)
        val event = ButtonEvent(isRightClick = false, amount = 2, position = Position(100, 200))
        event.action() // Trigger the event callback
    }
}

data class ButtonEvent(
    val isRightClick: Boolean,
    val amount: Int,
    val position: Position
)

data class Position(
    val x: Int,
    val y: Int
)

fun main() {
    val button = Button()
    
    button.onEvent {
        if (!isRightClick && amount == 2) {
            println("Double click!")
            // Double click!
        }
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-lambda-receivers-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="lambda-receivers-exercise-3"}

Write a function that creates a copy of a list of integers where every element is incremented by 1. Use the provided 
function skeleton that extends `List<Int>` with an `incremented` function.
    <br />编写一个函数，创建一个整数列表的副本，并将列表中的每个元素递增 1. 使用提供的函数框架，该框架扩展了 `List<Int>` 并添加了一个 `incremented` 函数。

```kotlin
fun List<Int>.incremented(): List<Int> {
    val originalList = this
    return buildList {
        // Write your code here
    }
}

fun main() {
    val originalList = listOf(1, 2, 3)
    val newList = originalList.incremented()
    println(newList)
    // [2, 3, 4]
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-receivers-exercise-3"}

|---|---|
```kotlin
fun List<Int>.incremented(): List<Int> {
    val originalList = this
    return buildList {
        for (n in originalList) add(n + 1)
    }
}

fun main() {
    val originalList = listOf(1, 2, 3)
    val newList = originalList.incremented()
    println(newList)
    // [2, 3, 4]
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-lambda-receivers-solution-3"}

## Next step
接下来

[Intermediate: Classes and interfaces](kotlin-tour-intermediate-classes-interfaces.md)
    <br />[中级：类和接口](kotlin-tour-intermediate-classes-interfaces.md)
