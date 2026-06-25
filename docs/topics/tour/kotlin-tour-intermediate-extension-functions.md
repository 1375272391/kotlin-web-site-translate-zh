[//]: # (title: Intermediate: Extension functions)

<no-index/>

<tldr>
    <p><img src="icon-1.svg" width="20" alt="First step" /> <strong>Extension functions</strong><br />
        <img src="icon-2-todo.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-todo.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In this chapter, you'll explore special Kotlin functions that make your code more concise and readable. Learn how they
can help you use efficient design patterns to take your projects to the next level.
<br />本章将探索一些特殊的 Kotlin 函数，它们能让你的代码更简洁、更易读。你将学习到它们如何 帮助你运用高效的设计模式，将你的项目提升到一个新的水平。

## Extension functions
扩展函数

In software development, you often need to modify a program's behavior without changing the original source code. 
For example, you might want to add extra functionality to a class from a third-party library.
<br />在软件开发中，您经常需要在不更改原始源代码的情况下修改程序的行为。
例如，您可能希望向来自第三方库的类添加额外的功能。

You can do this by adding _extension functions_ to extend a class. You call extension functions the same way 
you call member functions of a class, using a period `.`.
<br />你可以通过添加扩展函数来扩展类。调用扩展函数的方式与调用类的成员函数相同，使用句点 `.`。

Before introducing the complete syntax for extension functions, you need to understand what a **receiver** is.
The receiver is what the function is called on. In other words, the receiver is where or with whom the information is shared.
<br />在介绍扩展函数的完整语法之前，你需要了解什么是**接收器**。
接收器是函数被调用的对象。换句话说，接收器是信息共享的对象。

![An example of sender and receiver](receiver-highlight.png){width="500"}

In this example, the `main()` function calls the [`.first()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/first.html) function to return the first element in a list.
The `.first()` function is called **on** the `readOnlyShapes` variable, so the `readOnlyShapes` variable is the receiver.
<br />在这个例子中，`main()` 函数调用 `.first()` 函数返回列表中的第一个元素。
`.first()` 函数是**针对** `readOnlyShapes` 变量调用的，因此 `readOnlyShapes` 变量是接收者。

To create an extension function, write the name of the class that you want to extend followed by a `.` and the name of
your function. Continue with the rest of the function declaration, including its parameters and return type.
<br />要创建扩展函数，请先写出要扩展的类名，后跟一个点号`.`，再写出函数名。 然后继续填写函数声明的其余部分，包括参数和返回类型。

For example:
<br />例如：

```kotlin
fun String.bold(): String = "<b>$this</b>"

fun main() {
    // "hello" is the receiver
    println("hello".bold())
    // <b>hello</b>
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-extension-function"}

In this example:
<br />在这个例子中：

* `String` is the extended class.
  <br />`String` 是扩展类。
* `bold` is the name of the extension function. 
  <br />`bold` 是扩展函数的名称。
* The `.bold()` extension function's return type is `String`.
  <br />`.bold()` 扩展函数的返回类型为 `String`。
* `"hello"`, an instance of `String`, as the receiver.
  <br />`"hello"`，一个 `String` 实例，作为接收者。
* The receiver is accessed inside the body by the [keyword](keyword-reference.md): `this`.
  <br />接收器在主体内部通过 [关键字](keyword-reference.md): `this` 访问。
* A string template (`$`) is used to access the value of `this`.
  <br />使用字符串模板（`$`）来访问`this`的值。
* The `.bold()` extension function takes a string and returns it in a `<b>` HTML element for bold text.
  <br />`.bold()` 扩展函数接受一个字符串，并将其以 `<b>` HTML 元素的形式返回，用于加粗文本。

## Extension-oriented design
<br />面向扩展的设计

You can define extension functions anywhere, which enables you to create extension-oriented designs. These designs separate 
core functionality from useful but non-essential features, making your code easier to read and maintain.
<br />您可以在任何位置定义扩展函数，从而创建面向扩展的设计。这些设计将核心功能与有用但非必要的功能分离，使您的代码更易于阅读和维护。

A good example is the [`HttpClient`](https://api.ktor.io/ktor-client-core/io.ktor.client/-http-client/index.html) class from the Ktor library, which helps perform network requests. The core of
its functionality is a single function `request()`, which takes all the information needed for an HTTP request:
<br />一个很好的例子是 Ktor 库中的 [`HttpClient`](https://api.ktor.io/ktor-client-core/io.ktor.client/-http-client/index.html) 类，
它有助于执行网络请求。其核心功能是一个简单的函数 `request()`，该函数接收 HTTP 请求所需的所有信息：

```kotlin
class HttpClient {
    fun request(method: String, url: String, headers: Map<String, String>): HttpResponse {
        // Network code
    }
}
```
{validate="false"}

In practice, the most popular HTTP requests are GET or POST requests. It makes sense for the library to provide shorter
names for these common use cases. However, these don't require writing new network code, only a specific request call.
In other words, they are perfect candidates to be defined as separate `.get()` and `.post()` extension functions:
<br />实际上，最常见的 HTTP 请求是 GET 或 POST 请求。因此，库为这些常见用例提供更简洁的名称是合理的。
然而，这些名称不需要编写新的网络代码，只需要特定的请求调用即可。
换句话说，它们非常适合定义为单独的 `.get()` 和 `.post()` 扩展函数：

```kotlin
fun HttpClient.get(url: String): HttpResponse = request("GET", url, emptyMap())
fun HttpClient.post(url: String): HttpResponse = request("POST", url, emptyMap())
```
{validate="false"}

These `.get()` and `.post()` functions extend the `HttpClient` class. They can directly use the `request()` function from the `HttpClient` class
because they're called on an instance of the `HttpClient` class as the receiver. You can use these extension functions to
call the `request()` function with the appropriate HTTP method, which simplifies your code and makes it easier to understand:
<br />这些 `.get()` 和 `.post()` 函数继承自 `HttpClient` 类。它们可以直接使用 `HttpClient` 类中的 `request()` 函数，
因为它们是在 `HttpClient` 类的实例作为接收方调用的。您可以使用这些扩展函数来
使用适当的 HTTP 方法调用 `request()` 函数，这可以简化您的代码并使其更易于理解：

```kotlin
class HttpClient {
    fun request(method: String, url: String, headers: Map<String, String>): HttpResponse {
        println("Requesting $method to $url with headers: $headers")
        return HttpResponse("Response from $url")
    }
}

fun HttpClient.get(url: String): HttpResponse = request("GET", url, emptyMap())

fun main() {
    val client = HttpClient()

    // Making a GET request using request() directly
    val getResponseWithMember = client.request("GET", "https://example.com", emptyMap())

    // Making a GET request using the get() extension function
    // The client instance is the receiver
    val getResponseWithExtension = client.get("https://example.com")
}
```
{validate="false"}

This extension-oriented approach is widely used in Kotlin's [standard library](https://kotlinlang.org/api/latest/jvm/stdlib/)
and other libraries. For example, the `String` class has many [extension functions](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/#extension-functions)
to help you work with strings.
<br />这种面向扩展的方法在 Kotlin 的[标准库](https://kotlinlang.org/api/latest/jvm/stdlib/)
以及其他库中得到广泛应用。例如，`String` 类拥有许多[扩展函数](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/#extension-functions)
来帮助你处理字符串。

For more information about extension functions, see [Extensions](extensions.md).
<br />有关扩展功能的更多信息，请参阅[扩展](extensions.md)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="extension-functions-exercise-1"}

Write an extension function called `isPositive` that takes an integer and checks whether it is positive.
<br />编写一个名为 `isPositive` 的扩展函数，该函数接受一个整数并检查它是否为正数。

|---|---|
```kotlin
fun Int.// Write your code here

fun main() {
    println(1.isPositive())
    // true
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-extension-functions-exercise-1"}

|---|---|
```kotlin
fun Int.isPositive(): Boolean = this > 0

fun main() {
    println(1.isPositive())
    // true
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-extension-functions-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="extension-functions-exercise-2"}

Write an extension function called `toLowercaseString` that takes a string and returns a lowercase version.
<br />编写一个名为 `isPositive` 的扩展函数，该函数接受一个整数并检查它是否为正数。

<deflist collapsible="true">
    <def title="Hint">
        Use the <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/lowercase.html"> <code>.lowercase()</code>
        </a> function for the <code>String</code> type. 
    </def>
</deflist>

|---|---|
```kotlin
fun // Write your code here

fun main() {
    println("Hello World!".toLowercaseString())
    // hello world!
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-extension-functions-exercise-2"}

|---|---|
```kotlin
fun String.toLowercaseString(): String = this.lowercase()

fun main() {
    println("Hello World!".toLowercaseString())
    // hello world!
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-extension-functions-solution-2"}

## Next step
下一步

[Intermediate: Scope functions](kotlin-tour-intermediate-scope-functions.md)
    <br />[中级：作用域函数](kotlin-tour-intermediate-scope-functions.md)
