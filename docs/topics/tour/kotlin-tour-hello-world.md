[//]: # (title: Hello world)

<no-index/>

<tldr>
    <p><img src="icon-1.svg" width="20" alt="First step" /> <strong>Hello world</strong><br />
        <img src="icon-2-todo.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3-todo.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

Here is a simple program that prints "Hello, world!":
<br />这是一个简单的程序，可以打印"Hello, world!"：

```kotlin
fun main() {
    println("Hello, world!")
    // Hello, world!
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="hello-world-kotlin"}

In Kotlin:
<br />在 Kotlin 中：

* `fun` is used to declare a function
  <br />`fun` 用于声明函数
* The `main()` function is where your program starts from
  <br />`main()` 函数是程序开始的地方。
* The body of a function is written within curly braces `{}`
  <br />函数体写在花括号 `{}` 内。
* [`println()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/println.html) and [`print()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/print.html) functions print their arguments to standard output
  <br />[`println()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/println.html) 和 [`print()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/print.html) 函数会将它们的参数打印到标准输出。

A function is a set of instructions that performs a specific task. Once you create a function, you can use it whenever 
you need to perform that task, without having to write the instructions all over again. Functions are discussed in more
detail in a couple of chapters. Until then, all examples use the `main()` function.
<br />函数是一组执行特定任务的指令。创建函数后，您可以在需要执行该任务时随时使用它，而无需重新编写所有指令。
函数将在后续章节中进行更详细的讨论。在此之前，所有示例均使用 `main()` 函数。

## Variables
变量

All programs need to be able to store data, and variables help you to do just that. In Kotlin, you can declare:
<br />所有程序都需要能够存储数据，而变量正是用来帮助你实现这一点的。在 Kotlin 中，你可以声明：

* Read-only variables with `val`
  <br />只读变量，带有 `val`
* Mutable variables with `var`
  <br />使用 `var` 表示可变变量

> You can't change a read-only variable once you have given it a value.
> <br />一旦给只读变量赋值，就不能更改它的值。
>
{style="note"}

To assign a value, use the assignment operator `=`.
<br />要赋值，请使用赋值运算符 `=`。

For example:
<br />例如：

```kotlin
fun main() { 
//sampleStart
    val popcorn = 5    // There are 5 boxes of popcorn
    val hotdog = 7     // There are 7 hotdogs
    var customers = 10 // There are 10 customers in the queue
    
    // Some customers leave the queue
    customers = 8
    println(customers)
    // 8
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-variables"}

> Variables can be declared outside the `main()` function at the beginning of your program. Variables declared in this way
> are said to be declared at **top level**.
> <br />变量可以在程序开头的 `main()` 函数之外声明。以这种方式声明的变量被称为在**顶层**声明。
> 
{style="tip"}

As `customers` is a mutable variable, its value can be reassigned after declaration.
<br />由于 `customers` 是一个可变变量，因此在声明之后可以重新赋值。

> We recommend declaring all variables as read-only (`val`) by default. Only use mutable variables (`var`) if you really
> need to. That way, you're less likely to accidentally change something that wasn't meant to change.
> <br />我们建议默认情况下将所有变量声明为只读变量（`val`）。只有在真正需要时才使用可变变量（`var`）。这样可以降低意外更改不应更改的内容的风险。
> 
{style="note"}

## String templates
字符串模板

It's useful to know how to print the contents of variables to standard output. You can do this with **string templates**. 
You can use template expressions to access data stored in variables and other objects, and convert them into strings.
A string value is a sequence of characters in double quotes `"`. Template expressions always start with a dollar sign `$`.
<br />了解如何将变量的内容打印到标准输出非常有用。您可以使用**字符串模板**来实现这一点。
您可以使用模板表达式访问存储在变量和其他对象中的数据，并将其转换为字符串。
字符串值是由双引号 `"` 括起来的字符序列。模板表达式始终以美元符号 `$` 开头。

To evaluate a piece of code in a template expression, place the code within curly braces `{}` after the dollar sign `$`.
<br />要评估模板表达式中的一段代码，请将代码放在美元符号 `$` 后面的花括号 `{}` 内。

For example:
<br />例如：

```kotlin
fun main() { 
//sampleStart
    val customers = 10
    println("There are $customers customers")
    // There are 10 customers
    
    println("There are ${customers + 1} customers")
    // There are 11 customers
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-string-templates"}

For more information, see [String templates](strings.md#string-templates).
<br />有关更多信息，请参阅[字符串模板](strings.md#string-templates)。

You will notice that there aren't any types declared for variables. Kotlin has inferred the type itself: `Int`. This tour
explains the different Kotlin basic types and how to declare them in the [next chapter](kotlin-tour-basic-types.md).
<br />你会注意到变量没有声明类型。Kotlin 已经推断出了类型：`Int`。
本教程将在[下一章](kotlin-tour-basic-types.md)中解释不同的 Kotlin 基本类型以及如何声明它们。

## Practice
实践

### Exercise {initial-collapse-state="collapsed" collapsible="true"}

Complete the code to make the program print `"Mary is 20 years old"` to standard output:
<br />完成以下代码，使程序能够将“Mary is 20 years old”打印到标准输出：

|---|---|
```kotlin
fun main() {
    val name = "Mary"
    val age = 20
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-hello-world-exercise"}

|---|---|
```kotlin
fun main() {
    val name = "Mary"
    val age = 20
    println("$name is $age years old")
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-hello-world-solution"}

## Next step
接下来

[Basic types](kotlin-tour-basic-types.md)
<br />[基本类型](kotlin-tour-basic-types.md)
