[//]: # (title: Intermediate: Libraries and APIs)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-done.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-done.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-done.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8-done.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9.svg" width="20" alt="Ninth step" /> <strong>Libraries and APIs</strong><br /></p>
</tldr>

To get the most out of Kotlin, use existing libraries and APIs so you can spend more time coding and less time 
reinventing the wheel.
<br />为了充分发挥 Kotlin 的优势，请使用现有的库和 API，这样您就可以将更多的时间花在编写代码上，而不是重复造轮子上。

Libraries distribute reusable code that simplifies common tasks. Within libraries, there are packages and objects that
group related classes, functions, and utilities. Libraries expose APIs (Application Programming Interfaces) as a set of 
functions, classes, or properties that developers can use in their code.
<br />库分发可重用的代码，简化常见任务。库中包含包和对象，它们将相关的类、函数和实用程序分组在一起。
库将 API（应用程序编程接口）公开为一组函数、类或属性，供开发人员在代码中使用。

![Kotlin libraries and APIs](kotlin-library-diagram.svg){width=600}

Let's explore what's possible with Kotlin.
<br />让我们一起探索 Kotlin 的可能性。

## The standard library
标准库

Kotlin has a standard library that provides essential types, functions, collections, and utilities to make your code 
concise and expressive. A large portion of the standard library (everything in the [`kotlin` package](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/)) is readily available
in any Kotlin file without the need to import it explicitly:
Kotlin 拥有一个标准库，其中提供了必要的类型、函数、集合和实用工具，使你的代码简洁而富有表现力。
标准库的大部分内容（[`kotlin` 包](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/) 中的所有内容）都可以直接在任何 Kotlin 文件中使用，无需显式导入。

```kotlin
fun main() {
    val text = "emosewa si niltoK"
    
   // Use the reversed() function from the standard library
    val reversedText = text.reversed()

    // Use the print() function from the standard library
    print(reversedText)
    // Kotlin is awesome
}
```
{kotlin-runnable="true" id="kotlin-tour-libraries-stdlib"}

However, some parts of the standard library require an import before you can use them in your code. 
For example, if you want to use the standard library's time measurement features, you need to import the [`kotlin.time` package](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/).
<br />但是，标准库中的某些部分需要先导入才能在代码中使用。
例如，如果您想使用标准库的时间测量功能，则需要导入 [`kotlin.time` 包](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/)。

At the top of your file, add the `import` keyword followed by the package that you need:
<br />在文件顶部添加 `import` 关键字，后跟所需的包名：

```kotlin
import kotlin.time.*
```

The asterisk `*` is a wildcard import that tells Kotlin to import everything within the package. You can't use the 
asterisk `*` with companion objects. Instead, you need to explicitly declare the members of a companion object that you want to use.
<br />星号 `*` 是一个通配符导入，它告诉 Kotlin 导入包内的所有内容。星号 `*` 不能用于伴生对象。你需要显式声明要使用的伴生对象成员。

For example:
<br />例如：

```kotlin
import kotlin.time.Duration
import kotlin.time.Duration.Companion.hours
import kotlin.time.Duration.Companion.minutes

fun main() {
    val thirtyMinutes: Duration = 30.minutes
    val halfHour: Duration = 0.5.hours
    println(thirtyMinutes == halfHour)
    // true
}
```
{kotlin-runnable="true" id="kotlin-tour-libraries-time"}

This example:
<br />这个例子：

* Imports the `Duration` class and the `hours` and `minutes` extension properties from its companion object.
  <br />从其伴生对象导入 `Duration` 类以及 `hours` 和 `minutes` 扩展属性。
* Uses the `minutes` property to convert `30` into a `Duration` of 30 minutes.
  <br />使用 `minutes` 属性将 `30` 转换为 30 分钟的 `Duration`。
* Uses the `hours` property to convert `0.5` into a `Duration` of 30 minutes.
  <br />使用 `hours` 属性将 `0.5` 转换为 30 分钟的 `Duration`。
* Checks if both durations are equal and prints the result.
  <br />检查两个持续时间是否相等，并打印结果。

### Search before you build
构造前先进行调查

Before you decide to write your own code, check the standard library to see if what you're looking for already exists. 
Here's a list of areas where the standard library already provides a number of classes, functions, and properties for you:
<br />在决定编写自己的代码之前，请先查看标准库，看看你需要的功能是否已经存在。
以下列出了标准库已经为你提供的一些类、函数和属性：

* [Collections](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/)
  <br />[集合](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/)
* [Sequences](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.sequences/)
  <br />[序列](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.sequences/)
* [String manipulation](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.text/)
  <br />[字符串操作](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.text/)
* [Time management](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/)
  <br />[时间管理](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/)

To learn more about what else is in the standard library, explore its [API reference](https://kotlinlang.org/api/core/kotlin-stdlib/).
<br />要了解标准库中还有哪些内容，请浏览其[API 参考](https://kotlinlang.org/api/core/kotlin-stdlib/)。

## Kotlin libraries
Kotlin 库

The standard library covers many common use cases, but there are some that it doesn't address. Fortunately, the Kotlin
team and the rest of the community have developed a wide range of libraries to complement the standard library. For example,
[`kotlinx-datetime`](https://kotlinlang.org/api/kotlinx-datetime/) helps you manage time across different platforms.
<br />标准库涵盖了许多常见用例，但仍有一些用例无法满足。幸运的是，Kotlin 团队和社区其他成员开发了各种各样的库来补充标准库。
例如，[`kotlinx-datetime`](https://kotlinlang.org/api/kotlinx-datetime/) 可以帮助您跨平台管理时间。

You can find useful libraries on our [search platform](https://klibs.io/). To use them, you need to take extra steps,
like adding a dependency or plugin. Each library has a GitHub repository with instructions on how to include
it in your Kotlin projects.
<br />您可以在我们的[搜索平台](https://klibs.io/)上找到有用的库。要使用它们，您需要执行一些额外的步骤，
例如添加依赖项或插件。每个库都有一个 GitHub 仓库，其中包含如何将其包含在您的 Kotlin 项目中的说明。

Once you add the library, you can import any package within it. Here's an example of how to import the `kotlinx-datetime`
package to find the current time in New York: 
<br />添加库之后，您就可以导入其中的任何包。以下示例展示了如何导入 `kotlinx-datetime` 包来获取纽约的当前时间：

```kotlin
import kotlinx.datetime.*

fun main() {
    val now = Clock.System.now() // Get current instant
    println("Current instant: $now")

    val zone = TimeZone.of("America/New_York")
    val localDateTime = now.toLocalDateTime(zone)
    println("Local date-time in NY: $localDateTime")
}
```
{kotlin-runnable="true" id="kotlin-tour-libraries-datetime"}

This example:
<br />这个例子：

* Imports the `kotlinx.datetime` package.
  <br />导入 `kotlinx.datetime` 包。
* Uses the `Clock.System.now()` function to create an instance of the `Instant` class that contains the current time and assigns the result to the `now` variable.
  <br />使用 `Clock.System.now()` 函数创建一个包含当前时间的 `Instant` 类的实例，并将结果赋值给 `now` 变量。
* Prints the current time.
  <br />打印当前时间。
* Uses the `TimeZone.of()` function to find the time zone for New York and assigns the result to the `zone` variable.
  <br />使用 `TimeZone.of()` 函数查找纽约的时区，并将结果赋值给 `zone` 变量。
* Calls the `.toLocalDateTime()` function on the instance containing the current time, with the New York time zone as an argument.
  <br />调用包含当前时间的实例的 `.toLocalDateTime()` 函数，并将纽约时区作为参数。
* Assigns the result to the `localDateTime` variable.
  <br />将结果赋值给 `localDateTime` 变量。
* Prints the time adjusted for the time zone in New York.
  <br />打印出已根据纽约时区调整后的时间。

> To explore the functions and classes that this example uses in more detail, see the [API reference](https://kotlinlang.org/api/kotlinx-datetime/kotlinx-datetime/kotlinx.datetime/).
> <br />要更详细地了解此示例使用的函数和类，请参阅[API 参考](https://kotlinlang.org/api/kotlinx-datetime/kotlinx-datetime/kotlinx.datetime/)。
>
{style="tip"}

## Opt in to APIs
选择启用 API

Library authors may mark certain APIs as requiring opt-in before you can use them in your code. They usually do this when
an API is still in development and may change in the future. If you don't opt in, you see warnings or errors like this:
<br />库作者可能会将某些 API 标记为需要您先启用才能在代码中使用。他们通常会在以下情况下这样做：
API 仍在开发中，将来可能会发生变化。如果您未启用，则会看到类似这样的警告或错误：

```text
This declaration needs opt-in. Its usage should be marked with '@...' or '@OptIn(...)'
```

To opt in, write `@OptIn` followed by parentheses containing the class name that categorizes the API, appended by two colons `::` and `class`.
<br />要选择加入，请写 `@OptIn`，后面跟着包含 API 分类类名的括号，再附加两个冒号 `::` 和 `class`。

For example, the `uintArrayOf()` function from the standard library falls under `@ExperimentalUnsignedTypes`, as shown
[in the API reference](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/to-u-int-array.html):
<br />例如，标准库中的 `uintArrayOf()` 函数属于 `@ExperimentalUnsignedTypes` 注解，如以下 API 参考文档所示：
[在 API 参考文档](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/to-u-int-array.html)：

```kotlin
@ExperimentalUnsignedTypes
inline fun uintArrayOf(vararg elements: UInt): UIntArray
```

In your code, the opt-in looks like:
<br />在你的代码中，选择加入的语句如下所示：

```kotlin
@OptIn(ExperimentalUnsignedTypes::class)
```

Here's an example that opts in to use the `uintArrayOf()` function to create an array of unsigned integers and modifies one of its elements:
<br />以下示例选择使用 `uintArrayOf()` 函数创建一个无符号整数数组，并修改其中一个元素：

```kotlin
@OptIn(ExperimentalUnsignedTypes::class)
fun main() {
    // Create an unsigned integer array
    val unsignedArray: UIntArray = uintArrayOf(1u, 2u, 3u, 4u, 5u)

    // Modify an element
    unsignedArray[2] = 42u
    println("Updated array: ${unsignedArray.joinToString()}")
    // Updated array: 1, 2, 42, 4, 5
}
```
{kotlin-runnable="true" id="kotlin-tour-libraries-apis"}

This is the easiest way to opt in, but there are other ways. To learn more, see [Opt-in requirements](opt-in-requirements.md).
<br />这是最简单的加入方式，但还有其他方式。要了解更多信息，请参阅[加入要求](opt-in-requirements.md)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="libraries-exercise-1"}

You are developing a financial application that helps users calculate the future value of their investments. The formula
to calculate compound interest is:
<br />您正在开发一款金融应用程序，帮助用户计算其投资的未来价值。计算复利的公式是：
<math>A = P \times (1 + \displaystyle\frac{r}{n})^{nt}</math>

Where:
<br />其中：

* `A` is the amount of money accumulated after interest (principal + interest).
  <br />`A` 是扣除利息后积累的金额（本金 + 利息）。
* `P` is the principal amount (the initial investment).
  <br />`P` 是本金（初始投资额）。
* `r` is the annual interest rate (decimal).
  <br />`r` 是年利率（小数）。
* `n` is the number of times interest is compounded per year.
  <br />`n` 表示每年复利的次数。
* `t` is the time the money is invested for (in years).
  <br />`t` 表示资金的投资期限（以年为单位）。

Update the code to:
<br />更新代码为：

1. Import the necessary functions from the [`kotlin.math` package](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.math/).
   <br />从 [`kotlin.math` 包](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.math/) 导入必要的函数。
2. Add a body to the `calculateCompoundInterest()` function that calculates the final amount after applying compound interest.
   <br />在 `calculateCompoundInterest()` 函数中添加一个函数体，用于计算应用复利后的最终金额。

|--|--|

```kotlin
// Write your code here

fun calculateCompoundInterest(P: Double, r: Double, n: Int, t: Int): Double {
    // Write your code here
}

fun main() {
    val principal = 1000.0
    val rate = 0.05
    val timesCompounded = 4
    val years = 5
    val amount = calculateCompoundInterest(principal, rate, timesCompounded, years)
    println("The accumulated amount is: $amount")
    // The accumulated amount is: 1282.0372317085844
}

```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-libraries-exercise-1"}

|---|---|
```kotlin
import kotlin.math.*

fun calculateCompoundInterest(P: Double, r: Double, n: Int, t: Int): Double {
    return P * (1 + r / n).pow(n * t)
}

fun main() {
    val principal = 1000.0
    val rate = 0.05
    val timesCompounded = 4
    val years = 5
    val amount = calculateCompoundInterest(principal, rate, timesCompounded, years)
    println("The accumulated amount is: $amount")
    // The accumulated amount is: 1282.0372317085844
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-libraries-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="libraries-exercise-2"}

You want to measure the time it takes to perform multiple data processing tasks in your program. Update the code
to add the correct import statements and functions from the [`kotlin.time`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/) package:
<br />你想测量程序中执行多个数据处理任务所需的时间。请更新代码，添加来自 [`kotlin.time`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.time/) 包的正确导入语句和函数：

|---|---|

```kotlin
// Write your code here

fun main() {
    val timeTaken = /* Write your code here */ {
        // Simulate some data processing
        val data = List(1000) { it * 2 }
        val filteredData = data.filter { it % 3 == 0 }

        // Simulate processing the filtered data
        val processedData = filteredData.map { it / 2 }
        println("Processed data")
    }

    println("Time taken: $timeTaken") // e.g. 16 ms
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-libraries-exercise-2"}

|---|---|
```kotlin
import kotlin.time.measureTime

fun main() {
    val timeTaken = measureTime {
        // Simulate some data processing
        val data = List(1000) { it * 2 }
        val filteredData = data.filter { it % 3 == 0 }

        // Simulate processing the filtered data
        val processedData = filteredData.map { it / 2 }
        println("Processed data")
    }

    println("Time taken: $timeTaken") // e.g. 16 ms
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-libraries-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="properties-exercise-3"}

There's a new feature in the standard library available in the latest Kotlin release. You want to try it out, but it 
requires opt-in. The feature falls under [`@ExperimentalStdlibApi`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-experimental-stdlib-api/).
What should the opt-in look like in your code?
<br />最新 Kotlin 版本中的标准库新增了一项功能。你想尝试一下，但它需要手动启用。
该功能属于 [`@ExperimentalStdlibApi`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-experimental-stdlib-api/)。你的代码中应该如何实现启用功能？

|---|---|
```kotlin
@OptIn(ExperimentalStdlibApi::class)
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-libraries-solution-3"}

## What's next?
接下来是什么？

Congratulations! You've completed the intermediate tour! Would you like to [share your feedback](https://surveys.hotjar.com/bf4ce865-99ce-4fc1-b107-e9b16bc31592) about your experience? 
<br />恭喜！您已完成中级导览！您是否愿意[分享您的反馈](https://surveys.hotjar.com/bf4ce865-99ce-4fc1-b107-e9b16bc31592)，分享您的体验？

As a next step, check out our tutorials for popular Kotlin applications:
<br />接下来，请查看我们关于热门 Kotlin 应用程序的教程：

* [Create a backend application with Spring Boot and Kotlin](jvm-create-project-with-spring-boot.md)
  <br />[使用 Spring Boot 和 Kotlin 创建后端应用程序](jvm-create-project-with-spring-boot.md)
* Create a cross-platform application for Android and iOS from scratch and:
  <br />从零开始创建一个适用于 Android 和 iOS 的跨平台应用程序，并：
    * [Share business logic while keeping the UI native](https://kotlinlang.org/docs/multiplatform/multiplatform-create-first-app.html)
      <br />[在保持原生用户界面的同时共享业务逻辑](https://kotlinlang.org/docs/multiplatform/multiplatform-create-first-app.html)
    * [Share business logic and UI](https://kotlinlang.org/docs/multiplatform/compose-multiplatform-create-first-app.html)
      <br />[共享业务逻辑和用户界面](https://kotlinlang.org/docs/multiplatform/compose-multiplatform-create-first-app.html)
