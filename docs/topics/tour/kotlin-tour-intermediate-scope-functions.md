[//]: # (title: Intermediate: Scope functions)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2.svg" width="20" alt="Second step" /> <strong>Scope functions</strong><br />
        <img src="icon-3-todo.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In this chapter, you'll build on your understanding of extension functions to learn how to use scope functions to 
write more idiomatic code.
<br />在本章中，你将在对扩展函数的理解基础上，学习如何使用作用域函数来编写更符合惯用语法的代码。

## Scope functions
作用域函数

In programming, a scope is the area in which your variable or object is recognized. The most commonly referred to scopes
are the global scope and the local scope:
<br >在编程中，范围是识别变量或对象的区域。最常提到的范围分别是全局作用域和局部作用域：

* **Global scope** – a variable or object that is accessible from anywhere in the program.
<br />**全局作用域** – 可以从程序中的任何位置访问的变量或对象。
* **Local scope** – a variable or object that is only accessible within the block or function where it is defined.
<br />**局部作用域** – 仅在定义它的代码块或函数内可访问的变量或对象。

In Kotlin, there are also scope functions that allow you to create a temporary scope around an object and execute some code.
<br />在 Kotlin 中，还有作用域函数，允许你围绕一个对象创建一个临时作用域并执行一些代码。

Scope functions make your code more concise because you don't have to refer to the name of your object within the temporary
scope. Depending on the scope function, you can access the object either by referencing it via the keyword `this` or using it as an
argument via the keyword `it`.
<br />作用域函数使你的代码更简洁，因为你无需在临时作用域内引用对象的名称。根据作用域函数的不同，你可以通过关键字 `this` 引用对象，也可以通过关键字 `it` 
将其作为参数传递来访问对象。

Kotlin has five scope functions in total: `let`, `apply`, `run`, `also`, and `with`.
<br />Kotlin 共有五个作用域函数：`let`、`apply`、`run`、`also` 和 `with`。

Each scope function takes a lambda expression and returns either the object or the result of the lambda expression. In 
this tour, we explain each scope function and how to use it.
<br />每个作用域函数都接受一个 lambda 表达式，并返回该 lambda 表达式的对象或结果。在本教程中，我们将解释每个作用域函数及其用法。

> You can also watch the [Back to the Stdlib: Making the Most of Kotlin's Standard Library](https://youtu.be/DdvgvSHrN9g?feature=shared&t=1511)
> talk on scope functions by Sebastian Aigner, Kotlin developer advocate.
> <br />您还可以观看 Kotlin 开发者倡导者 Sebastian Aigner 的演讲
[回到标准库：充分利用 Kotlin 标准库中的作用域函数](https://youtu.be/DdvgvSHrN9g?feature=shared&t=1511)。
> 
{style="tip"}

### Let

Use the `let` scope function when you want to perform null checks in your code and later perform further actions
with the returned object.
<br />当你想在代码中执行空值检查，并在之后对返回的对象执行进一步操作时，请使用 `let` 作用域函数。

Consider the example:
<br />请看以下示例：

```kotlin
fun sendNotification(recipientAddress: String): String {
    println("Yo $recipientAddress!")
    return "Notification sent!"
}

fun getNextAddress(): String {
    return "sebastian@jetbrains.com"
}

fun main() {
    val address: String? = getNextAddress()
    sendNotification(address)
}
```
{validate = "false"}

The example has two functions:
<br />该示例包含两个函数：
* `sendNotification()`, which has a function parameter `recipientAddress` and returns a string.
<br />`sendNotification()`，它有一个函数参数 `recipientAddress`，并返回一个字符串。
* `getNextAddress()`, which has no function parameters and returns a string.
<br />`getNextAddress()`，该函数没有参数，返回一个字符串。

The example creates a variable `address` that has a nullable `String` type. But this becomes a problem when you call
the `sendNotification()` function because this function doesn't expect that `address` could be a `null` value.
The compiler reports an error as a result: 
<br />示例创建了一个名为 `address` 的变量，其类型为可为空的 `String`。
但是，当您调用 `sendNotification()` 函数时，就会出现问题，因为该函数不接受 `address` 为 `null` 值。 因此，编译器会报告一个错误：
```text
Argument type mismatch: actual type is 'String?', but 'String' was expected.
参数类型不匹配：实际类型为'String?'，但预期类型为'String'。
```

From the beginner tour, you already know that you can perform a null check with an if condition or use the [Elvis operator `?:`](kotlin-tour-null-safety.md#use-elvis-operator). 
But what if you want to use the returned object later in your code? You could achieve this with an if condition **and** an 
else branch:
<br />从入门教程中，您已经知道可以使用 if 条件进行空值检查，或者使用 [Elvis 运算符 `?:`](kotlin-tour-null-safety.md#use-elvis-operator)。
但是，如果您想在代码的后续部分使用返回的对象呢？您可以使用 if 条件**以及** else 分支来实现这一点：

```kotlin
fun sendNotification(recipientAddress: String): String {
    println("Yo $recipientAddress!")
    return "Notification sent!"
}

fun getNextAddress(): String {
    return "sebastian@jetbrains.com"
}

fun main() { 
    //sampleStart
    val address: String? = getNextAddress()
    val confirm = if(address != null) {
        sendNotification(address)
    } else { null }
    //sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-let-non-null-if"}

However, a more concise approach is to use the `let` scope function:
<br />然而，更简洁的方法是使用 `let` 作用域函数：

```kotlin
fun sendNotification(recipientAddress: String): String {
    println("Yo $recipientAddress!")
    return "Notification sent!"
}

fun getNextAddress(): String {
    return "sebastian@jetbrains.com"
}

fun main() {
    //sampleStart
    val address: String? = getNextAddress()
    val confirm = address?.let {
        sendNotification(it)
    }
    //sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-let-non-null"}

The example:
例如：
* Creates variables called `address` and `confirm`.
  <br />创建名为 `address` 和 `confirm` 的变量。
* Uses a safe call for the `let` scope function on the `address` variable.
  <br />对 `address` 变量使用 `let` 作用域函数的安全调用。
* Creates a temporary scope within the `let` scope function.
  <br />在 `let` 作用域函数内创建一个临时作用域。
* Passes the `sendNotification()` function as a lambda expression into the `let` scope function.
  <br />将 `sendNotification()` 函数作为 lambda 表达式传递给 `let` 作用域函数。
* Refers to the `address` variable via `it`, using the temporary scope.
  <br />通过 `it` 引用 `address` 变量，使用临时作用域。
* Assigns the result to the `confirm` variable.
  <br />将结果赋值给 `confirm` 变量。

With this approach, your code can handle the `address` variable potentially being a `null` value, and you can use the 
`confirm` variable later in your code.
<br />通过这种方法，您的代码可以处理 `address` 变量可能为 `null` 值的情况，并且您可以在代码的后续部分使用 `confirm` 变量。

### Apply

Use the `apply` scope function to initialize objects, like a class instance, at the time of creation rather than later
on in your code. This approach makes your code easier to read and manage.
<br />使用 `apply` 作用域函数在创建对象（例如类实例）时立即初始化，而不是在代码的后续步骤中初始化。这种方法可以使代码更易于阅读和维护。

Consider the example:
<br />请看以下示例：

```kotlin
class Client() {
    var token: String? = null
    fun connect() = println("connected!")
    fun authenticate() = println("authenticated!")
    fun getData() : String {
        println("getting data!")
        return "Mock data"
    }
}

val client = Client()

fun main() {
    client.token = "asdf"
    client.connect()
    // connected!
    client.authenticate()
    // authenticated!
    client.getData()
    // getting data!
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-apply-before"}

The example has a `Client` class that contains one property called `token` and three member functions: `connect()`,
`authenticate()`, and `getData()`.
<br />该示例包含一个名为 `Client` 的类，其中包含一个名为 `token` 的属性和三个成员函数：`connect()`、`authenticate()` 和 `getData()`。

The example creates `client` as an instance of the `Client` class before initializing its `token` property and calling its
member functions in the `main()` function.
<br />该示例在初始化其 `token` 属性之前，创建了 `client` 作为 `Client` 类的一个实例，并在 `main()` 函数中调用了其成员函数。

Although this example is compact, in the real world, it can be a while before you can configure and use the class instance
(and its member functions) after you've created it. However, if you use the `apply` scope function you can create, configure and
use member functions on your class instance all in the same place in your code:
<br />虽然这个例子很简洁，但在实际应用中，创建类实例（及其成员函数）后，可能需要一段时间才能配置和使用它。
但是，如果使用 `apply` 作用域函数，就可以在代码的同一位置创建、配置和使用类实例的成员函数：

```kotlin
class Client() {
    var token: String? = null
    fun connect() = println("connected!")
    fun authenticate() = println("authenticated!")
    fun getData() : String {
        println("getting data!")
        return "Mock data"
    }
}
//sampleStart
val client = Client().apply {
    token = "asdf"
    connect()
    // connected!
    authenticate()
    // authenticated!
}

fun main() {
    client.getData()
    // getting data!
}
//sampleEnd
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-apply-after"}

The example:
<br />例如：

* Creates `client` as an instance of the `Client` class.
  <br />创建 `client` 作为 `Client` 类的实例。
* Uses the `apply` scope function on the `client` instance.
  <br />对 `client` 实例使用 `apply` 作用域函数。
* Creates a temporary scope within the `apply` scope function so that you don't have to explicitly refer to the `client` instance when accessing its properties or functions.
  <br />在 `apply` 作用域函数内创建一个临时作用域，这样在访问其属性或函数时，就不必显式地引用 `client` 实例。
* Passes a lambda expression to the `apply` scope function that updates the `token` property and calls the `connect()` and `authenticate()` functions.
  <br />将 lambda 表达式传递给 `apply` 作用域函数，该函数更新 `token` 属性并调用 `connect()` 和 `authenticate()` 函数。
* Calls the `getData()` member function on the `client` instance in the `main()` function.
  <br />在 `main()` 函数中调用 `client` 实例的 `getData()` 成员函数。

As you can see, this strategy is convenient when you are working with large pieces of code.
<br />如您所见，这种策略在处理大型代码时非常方便。

### Run

Similar to `apply`, you can use the `run` scope function to initialize an object, but it's better to use `run` 
to initialize an object at a specific moment in your code **and** immediately compute a result.
<br />与 `apply` 类似，您可以使用 `run` 作用域函数来初始化对象，但最好使用 `run` 在代码中的特定时刻初始化对象，**并**立即计算结果。

Let's continue the previous example for the `apply` function, but this time, you want the `connect()` and
`authenticate()` functions to be grouped so that they are called on every request.
<br />让我们继续之前关于 `apply` 函数的例子，但这一次，您希望将 `connect()` 和
`authenticate()` 函数组合在一起，以便在每次请求时都调用它们。

For example:
<br />例如：

```kotlin
class Client() {
    var token: String? = null
    fun connect() = println("connected!")
    fun authenticate() = println("authenticated!")
    fun getData() : String {
        println("getting data!")
        return "Mock data"
    }
}

//sampleStart
val client: Client = Client().apply {
    token = "asdf"
}

fun main() {
    val result: String = client.run {
        connect()
        // connected!
        authenticate()
        // authenticated!
        getData()
        // getting data!
    }
}
//sampleEnd
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-run"}

The example:
<br />例如：

* Creates `client` as an instance of the `Client` class.
  <br />创建 `client` 作为 `Client` 类的实例。
* Uses the `apply` scope function on the `client` instance.
  <br />对 `client` 实例使用 `apply` 作用域函数。
* Creates a temporary scope within the `apply` scope function so that you don't have to explicitly refer to the `client` instance when accessing its properties or functions.
  <br />在 `apply` 作用域函数内创建一个临时作用域，这样在访问其属性或函数时，就不必显式地引用 `client` 实例。
* Passes a lambda expression to the `apply` scope function that updates the `token` property.
  <br />将 lambda 表达式传递给更新 `token` 属性的 `apply` 作用域函数。

The `main()` function:
<br />`main()` 函数：

* Creates a `result` variable with type `String`.
  <br />创建一个名为 `result` 的 `String` 类型变量。
* Uses the `run` scope function on the `client` instance.
  <br />在 `client` 实例上使用 `run` 作用域函数。
* Creates a temporary scope within the `run` scope function so that you don't have to explicitly refer to the `client` instance when accessing its properties or functions.
  <br />在 `run` 作用域函数内创建一个临时作用域，这样在访问其属性或函数时，就不必显式地引用 `client` 实例。
* Passes a lambda expression to the `run` scope function that calls the `connect()`, `authenticate()`, and `getData()` functions.
  <br />将 lambda 表达式传递给 `run` 作用域函数，该函数调用 `connect()`、`authenticate()` 和 `getData()` 函数。
* Assigns the result to the `result` variable.
  <br />将结果赋值给 `result` 变量。

Now you can use the returned result further in your code.
<br />现在你可以在代码中进一步使用返回的结果了。

### Also

Use the `also` scope function to complete an additional action with an object and then return the object to continue 
using it in your code, like writing a log.
<br />使用 `also` 作用域函数可以对对象执行额外的操作，然后返回该对象以便在代码中继续使用它，例如写入日志。

Consider the example:
<br />请看以下示例：

```kotlin
fun main() {
    val medals: List<String> = listOf("Gold", "Silver", "Bronze")
    val reversedLongUppercaseMedals: List<String> =
        medals
            .map { it.uppercase() }
            .filter { it.length > 4 }
            .reversed()
    println(reversedLongUppercaseMedals)
    // [BRONZE, SILVER]
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-also-before"}

The example:
<br />例如：

* Creates the `medals` variable that contains a list of strings.
  <br />创建包含字符串列表的 `medals` 变量。
* Creates the `reversedLongUpperCaseMedals` variable that has the `List<String>` type.
        <br />创建类型为 `List<String>` 的 `reversedLongUpperCaseMedals` 变量。
* Uses the `.map()` extension function on the `medals` variable.
  <br />对 `medals` 变量使用 `.map()` 扩展函数。
* Passes a lambda expression to the `.map()` function that refers to `medals` via the `it` keyword and calls the `.uppercase()` extension function on it.
  <br />将 lambda 表达式传递给 `.map()` 函数，该表达式通过 `it` 关键字引用 `medals`，并对其调用 `.uppercase()` 扩展函数。
* Uses the `.filter()` extension function on the `medals` variable.
  <br />对 `medals` 变量使用 `.filter()` 扩展函数。
* Passes a lambda expression as a predicate to the `.filter()` function that refers to `medals` via the `it` keyword and checks if the item in the list has more than 4 characters.
  <br />将 lambda 表达式作为谓词传递给 `.filter()` 函数，该函数通过 `it` 关键字引用 `medals`，并检查列表中的项目是否超过 4 个字符。
* Uses the `.reversed()` extension function on the `medals` variable.
  <br />对 `medals` 变量使用 `.reversed()` 扩展函数。
* Assigns the result to the `reversedLongUpperCaseMedals` variable.
  <br />将结果赋值给 `reversedLongUpperCaseMedals` 变量。
* Prints the list contained in the `reversedLongUpperCaseMedals` variable.
  <br />打印 `reversedLongUpperCaseMedals` 变量中包含的列表。

It would be useful to add some logging in between the function calls to see what is happening to the `medals` variable.
The `also` function helps with that:
<br />在函数调用之间添加一些日志记录会很有帮助，这样可以查看 `medals` 变量的变化情况。`also` 函数可以实现这一点：

```kotlin
fun main() {
    val medals: List<String> = listOf("Gold", "Silver", "Bronze")
    val reversedLongUppercaseMedals: List<String> =
        medals
            .map { it.uppercase() }
            .also { println(it) }
            // [GOLD, SILVER, BRONZE]
            .filter { it.length > 4 }
            .also { println(it) }
            // [SILVER, BRONZE]
            .reversed()
    println(reversedLongUppercaseMedals)
    // [BRONZE, SILVER]
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-also-after"}

Now the example:
<br />现在举个例子：

* Uses the `also` scope function on the `medals` variable.
  <br />对 `medals` 变量使用 `also` 作用域函数。
* Creates a temporary scope within the `also` scope function so that you don't have to explicitly refer to the `medals` variable when using it as a function parameter.
  <br />在 `also` 作用域函数内创建一个临时作用域，这样在使用 `medals` 变量作为函数参数时，就不必显式地引用它了。
* Passes a lambda expression to the `also` scope function that calls the `println()` function using the `medals` variable as a function parameter via the `it` keyword.
  <br />将 lambda 表达式传递给 `also` 作用域函数，该函数通过 `it` 关键字使用 `medals` 变量作为函数参数调用 `println()` 函数。

Since the `also` function returns the object, it is useful for not only logging but debugging, chaining
multiple operations, and performing other side-effect operations that don't affect the main flow of your code.
<br />由于 `also` 函数会返回对象，因此它不仅可以用于记录日志，还可以用于调试、链接多个操作，以及执行其他不会影响代码主流程的副作用操作。

### With

Unlike the other scope functions, `with` is not an extension function, so the syntax is different. You pass the receiver
object to `with` as an argument. 
<br />与其他作用域函数不同，`with` 不是扩展函数，因此语法有所不同。你需要将接收者对象作为参数传递给 `with`。

Use the `with` scope function when you want to call multiple functions on an object.
<br />当想要对一个对象调用多个函数时，请使用 `with` 作用域函数。

Consider this example:
<br />请看这个例子：

```kotlin
class Canvas {
    fun rect(x: Int, y: Int, w: Int, h: Int): Unit = println("$x, $y, $w, $h")
    fun circ(x: Int, y: Int, rad: Int): Unit = println("$x, $y, $rad")
    fun text(x: Int, y: Int, str: String): Unit = println("$x, $y, $str")
}

fun main() {
    val mainMonitorPrimaryBufferBackedCanvas = Canvas()

    mainMonitorPrimaryBufferBackedCanvas.text(10, 10, "Foo")
    mainMonitorPrimaryBufferBackedCanvas.rect(20, 30, 100, 50)
    mainMonitorPrimaryBufferBackedCanvas.circ(40, 60, 25)
    mainMonitorPrimaryBufferBackedCanvas.text(15, 45, "Hello")
    mainMonitorPrimaryBufferBackedCanvas.rect(70, 80, 150, 100)
    mainMonitorPrimaryBufferBackedCanvas.circ(90, 110, 40)
    mainMonitorPrimaryBufferBackedCanvas.text(35, 55, "World")
    mainMonitorPrimaryBufferBackedCanvas.rect(120, 140, 200, 75)
    mainMonitorPrimaryBufferBackedCanvas.circ(160, 180, 55)
    mainMonitorPrimaryBufferBackedCanvas.text(50, 70, "Kotlin")
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-with-before"}

The example creates a `Canvas` class that has three member functions: `rect()`, `circ()`, and `text()`. Each of these member
functions prints a statement constructed from the function parameters that you provide.
<br />该示例创建了一个名为 `Canvas` 的类，该类包含三个成员函数：`rect()`、`circ()` 和 `text()`。每个成员函数都会打印一条由您提供的函数参数构造的语句。

The example creates `mainMonitorPrimaryBufferBackedCanvas` as an instance of the `Canvas` class before calling a sequence
of member functions on the instance with different function parameters.
<br />该示例首先创建 `mainMonitorPrimaryBufferBackedCanvas` 作为 `Canvas` 类的实例，然后对该实例调用一系列具有不同函数参数的成员函数。

You can see that this code is hard to read. If you use the `with` function, the code is streamlined:
<br />你可以看到这段代码难以阅读。如果使用 `with` 函数，代码就会变得简洁明了：

```kotlin
class Canvas {
    fun rect(x: Int, y: Int, w: Int, h: Int): Unit = println("$x, $y, $w, $h")
    fun circ(x: Int, y: Int, rad: Int): Unit = println("$x, $y, $rad")
    fun text(x: Int, y: Int, str: String): Unit = println("$x, $y, $str")
}

fun main() {
    //sampleStart
    val mainMonitorSecondaryBufferBackedCanvas = Canvas()
    with(mainMonitorSecondaryBufferBackedCanvas) {
        text(10, 10, "Foo")
        rect(20, 30, 100, 50)
        circ(40, 60, 25)
        text(15, 45, "Hello")
        rect(70, 80, 150, 100)
        circ(90, 110, 40)
        text(35, 55, "World")
        rect(120, 140, 200, 75)
        circ(160, 180, 55)
        text(50, 70, "Kotlin")
    }
    //sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-scope-function-with-after"}

This example:
<br />这个例子：
* Uses the `with` scope function with the `mainMonitorSecondaryBufferBackedCanvas` instance as the receiver.
  <br />使用 `with` 作用域函数，并将 `mainMonitorSecondaryBufferBackedCanvas` 实例作为接收器。
* Creates a temporary scope within the `with` scope function so that you don't have to explicitly refer to the `mainMonitorSecondaryBufferBackedCanvas` instance when calling its member functions.
  <br />在 `with` 作用域函数内创建一个临时作用域，这样在调用其成员函数时，就不必显式地引用 `mainMonitorSecondaryBufferBackedCanvas` 实例。
* Passes a lambda expression to the `with` scope function that calls a sequence of member functions with different function parameters.
  <br />将 lambda 表达式传递给 `with` 作用域函数，该函数调用一系列具有不同函数参数的成员函数。

Now that this code is much easier to read, you are less likely to make mistakes.
<br />现在这段代码更容易阅读，你出错的可能性也更小了。

## Use case overview
用例概述

This section has covered the different scope functions available in Kotlin and their main use cases for making your code
more idiomatic. You can use this table as a quick reference. It's important to note that you don't need a complete understanding
of how these functions work in order to use them in your code.
<br />本节介绍了 Kotlin 中可用的各种作用域函数及其主要用途，这些用途可以帮助你编写更符合 Kotlin 惯用语法的代码。
你可以使用此表作为快速参考。需要注意的是，你无需完全理解这些函数的工作原理即可在代码中使用它们。

| Function | Access to `x` via | Return value  | Use case                                                                                     |
|----------|-------------------|---------------|----------------------------------------------------------------------------------------------|
| `let`    | `it`              | Lambda result | Perform null checks in your code and later perform further actions with the returned object. |
| `apply`  | `this`            | `x`           | Initialize objects at the time of creation.                                                  |
| `run`    | `this`            | Lambda result | Initialize objects at the time of creation **AND** compute a result.                         |
| `also`   | `it`              | `x`           | Complete additional actions before returning the object.                                     |
| `with`   | `this`            | Lambda result | Call multiple functions on an object.                                                        |

| 函数      | 通过以下方式访问 `x` | 返回值  　    | 用例                          |
|---------|--------------|-----------|-----------------------------|
| `let`   | `it`         | Lambda 结果 | 在代码中执行空值检查，然后对返回的对象执行进一步操作。 |
| `apply` | `this`       | `x`       | 在创建对象时对其进行初始化。              |
| `run`   | `this`       | Lambda 结果 | 在创建对象时初始化对象**并且**计算结果。      |
| `also`  | `it`         | `x`       | 在返回对象之前，请完成其他操作。            |
| `with`  | `this`       | Lambda 结果 | 对对象执行所有多个函数。                |

For more information about scope functions, see [Scope functions](scope-functions.md).
<br />有关作用域函数的更多信息，请参阅[作用域函数](scope-functions.md)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="scope-functions-exercise-1"}

Rewrite the `.getPriceInEuros()` function as a single-expression function that uses safe call operators `?.` and the `let` scope function.
<br />将 `.getPriceInEuros()` 函数重写为使用安全调用运算符 `?.` 和 `let` 作用域函数的单表达式函数。

<deflist collapsible="true">
    <def title="Hint">
        Use safe call operators <code>?.</code> to safely access the <code>priceInDollars</code> property from the <code>getProductInfo()</code>
        function. Then, use the <code>let</code> scope function to convert the value of <code>priceInDollars</code> into euros.
        <br />使用安全调用运算符 `?.` 可以安全地从 `getProductInfo()` 函数访问 `priceInDollars` 属性。
        然后，使用 `let` 作用域函数将 `priceInDollars` 的值转换为欧元。
    </def>
</deflist>

|---|---|
```kotlin
data class ProductInfo(val priceInDollars: Double?)

class Product {
    fun getProductInfo(): ProductInfo? {
        return ProductInfo(100.0)
    }
}

// Rewrite this function
fun Product.getPriceInEuros(): Double? {
    val info = getProductInfo()
    if (info == null) return null
    val price = info.priceInDollars
    if (price == null) return null
    return convertToEuros(price)
}

fun convertToEuros(dollars: Double): Double {
    return dollars * 0.85
}

fun main() {
    val product = Product()
    val priceInEuros = product.getPriceInEuros()

    if (priceInEuros != null) {
        println("Price in Euros: €$priceInEuros")
        // Price in Euros: €85.0
    } else {
        println("Price information is not available.")
    }
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-scope-functions-exercise-1"}

|---|---|
```kotlin
data class ProductInfo(val priceInDollars: Double?)

class Product {
    fun getProductInfo(): ProductInfo? {
        return ProductInfo(100.0)
    }
}

fun Product.getPriceInEuros() = getProductInfo()?.priceInDollars?.let { convertToEuros(it) }

fun convertToEuros(dollars: Double): Double {
    return dollars * 0.85
}

fun main() {
    val product = Product()
    val priceInEuros = product.getPriceInEuros()

    if (priceInEuros != null) {
        println("Price in Euros: €$priceInEuros")
        // Price in Euros: €85.0
    } else {
        println("Price information is not available.")
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-scope-functions-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="scope-functions-exercise-2"}

You have an `updateEmail()` function that updates the email address of a user. Use the `apply` scope function
to update the email address and then the `also` scope function to print a log message: `Updating email for user with ID: ${it.id}`.
    <br />你有一个 `updateEmail()` 函数，用于更新用户的电子邮件地址。使用 `apply` 作用域函数来更新电子邮件地址，
    然后使用 `also` 作用域函数打印一条日志消息：`正在更新 ID 为 ${it.id} 的用户的电子邮件`。

|---|---|
```kotlin
data class User(val id: Int, var email: String)

fun updateEmail(user: User, newEmail: String): User = // Write your code here

fun main() {
    val user = User(1, "old_email@example.com")
    val updatedUser = updateEmail(user, "new_email@example.com")
    // Updating email for user with ID: 1

    println("Updated User: $updatedUser")
    // Updated User: User(id=1, email=new_email@example.com)
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-scope-functions-exercise-2"}

|---|---|
```kotlin
data class User(val id: Int, var email: String)

fun updateEmail(user: User, newEmail: String): User = user.apply {
    this.email = newEmail
}.also { println("Updating email for user with ID: ${it.id}") }

fun main() {
    val user = User(1, "old_email@example.com")
    val updatedUser = updateEmail(user, "new_email@example.com")
    // Updating email for user with ID: 1

    println("Updated User: $updatedUser")
    // Updated User: User(id=1, email=new_email@example.com)
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-scope-functions-solution-2"}

## Next step
下一步

[Intermediate: Lambda expressions with receiver](kotlin-tour-intermediate-lambdas-receiver.md)
    <br />[中级：带接收器的 Lambda 表达式](kotlin-tour-intermediate-lambdas-receiver.md)
