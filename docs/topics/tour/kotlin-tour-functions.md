[//]: # (title: Functions)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5.svg" width="20" alt="Fifth step" /> <strong>Functions</strong><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

You can declare your own functions in Kotlin using the `fun` keyword.
<br />在 Kotlin 中，您可以使用 `fun` 关键字声明自己的函数。

```kotlin
fun hello() {
    return println("Hello, world!")
}

fun main() {
    hello()
    // Hello, world!
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-function-demo"}

In Kotlin:
<br />在 Kotlin 中：

* Function parameters are written within parentheses `()`.
  <br />函数参数写在括号 `()` 内。
* Each parameter must have a type, and multiple parameters must be separated by commas `,`.
  <br />每个参数都必须有类型，多个参数之间必须用逗号“,”分隔。
* The return type is written after the function's parentheses `()`, separated by a colon `:`.
  <br />返回类型写在函数括号 `()` 之后，用冒号 `:` 分隔。
* The body of a function is written within curly braces `{}`.
  <br />函数体写在花括号 `{}` 内。
* The `return` keyword is used to exit or return something from a function.
  <br />`return` 关键字用于退出函数或从函数中返回某些内容。

> If a function doesn't return anything useful, the return type and `return` keyword can be omitted. Learn more about
> this in [Functions without return](#functions-without-return).
> <br />如果一个函数不返回任何有用的值，则可以省略返回类型和 `return` 关键字。了解更多信息，请参阅[无返回值的函数](#functions-without-return)。
>
{style="note"}

In the following example:
<br />在以下示例中：

* `x` and `y` are function parameters.
  <br />`x` 和 `y` 是函数参数。
* `x` and `y` have type `Int`.
  <br />`x` 和 `y` 的类型为 `Int`。
* The function's return type is `Int`.
  <br />该函数的返回类型为`Int`。
* The function returns a sum of `x` and `y` when called.
  <br />调用该函数时，返回 `x` 和 `y` 之和。

```kotlin
fun sum(x: Int, y: Int): Int {
    return x + y
}

fun main() {
    println(sum(1, 2))
    // 3
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-simple-function"}

> We recommend in our [coding conventions](coding-conventions.md#function-names) that you name functions starting with 
> a lowercase letter and use camel case with no underscores.
> <br />我们在[编码规范](coding-conventions.md#function-names)中建议，函数名称应以小写字母开头，并使用驼峰命名法，且不使用下划线。
> 
{style="note"}

## Named arguments
命名参数

For concise code, when calling your function, you don't have to include parameter names. However, including parameter names
does make your code easier to read. This is called using **named arguments**. If you do include parameter names, then 
you can write the parameters in any order.
<br />为了编写简洁的代码，调用函数时，您不必包含参数名。但是，包含参数名确实能使代码更易于阅读。这称为使用**命名参数**。如果您包含参数名，则可以按任意顺序编写参数。

> In the following example, [string templates](strings.md#string-templates) (`$`) are used to access
> the parameter values, convert them to `String` type, and then concatenate them into a string for printing.
> <br />在以下示例中，[字符串模板](strings.md#string-templates) (`$`) 用于访问参数值，将其转换为 `String` 类型，然后将其连接成一个字符串进行打印。
> 
{style="tip"}

```kotlin
fun printMessageWithPrefix(message: String, prefix: String) {
    println("[$prefix] $message")
}

fun main() {
    // Uses named arguments with swapped parameter order
    printMessageWithPrefix(prefix = "Log", message = "Hello")
    // [Log] Hello
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-named-arguments-function"}

## Default parameter values
默认参数值

You can define default values for your function parameters. Any parameter with a default value can be omitted when
calling your function. To declare a default value, use the assignment operator `=` after the type:
<br />您可以为函数参数定义默认值。任何具有默认值的参数在调用函数时都可以省略。
要声明默认值，请在类型后使用赋值运算符 `=`：

```kotlin
fun printMessageWithPrefix(message: String, prefix: String = "Info") {
    println("[$prefix] $message")
}

fun main() {
    // Function called with both parameters
    printMessageWithPrefix("Hello", "Log") 
    // [Log] Hello
    
    // Function called only with message parameter
    printMessageWithPrefix("Hello")        
    // [Info] Hello
    
    printMessageWithPrefix(prefix = "Log", message = "Hello")
    // [Log] Hello
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-default-param-function"}

> You can skip specific parameters with default values, rather than omitting them all. However, after the 
> first skipped parameter, you must name all subsequent parameters.
> <br />您可以跳过某些参数并使用默认值，而不是全部省略。但是，在跳过第一个参数之后，您必须为所有后续参数命名。
>
{style="note"}

## Functions without return
没有返回值的函数

If your function doesn't return a useful value then its return type is `Unit`. `Unit` is a type with only one value – 
`Unit`. You don't have to declare that `Unit` is returned explicitly in your function body. This means that you don't 
have to use the `return` keyword or declare a return type:
<br />如果你的函数没有返回任何有用的值，那么它的返回类型就是 `Unit`。`Unit` 是一种只有一个值的类型——`Unit`。
你不需要在函数体中显式声明返回的是 `Unit`。这意味着你不需要使用 `return` 关键字或声明返回类型。

```kotlin
fun printMessage(message: String) {
    println(message)
    // `return Unit` or `return` is optional
}

fun main() {
    printMessage("Hello")
    // Hello
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-unit-function"}

## Single-expression functions
单表达式函数

To make your code more concise, you can use single-expression functions. For example, the `sum()` function can be shortened:
<br />为了使代码更简洁，可以使用单表达式函数。例如，`sum()` 函数可以简化为：

```kotlin
fun sum(x: Int, y: Int): Int {
    return x + y
}

fun main() {
    println(sum(1, 2))
    // 3
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-simple-function-before"}

You can remove the curly braces `{}` and declare the function body using the assignment operator `=`. When you use the 
assignment operator `=`, Kotlin uses type inference, so you can also omit the return type. The `sum()` function then becomes one line:
<br />您可以移除花括号 `{}`，并使用赋值运算符 `=` 声明函数体。
使用赋值运算符 `=` 时，Kotlin 会进行类型推断，因此您也可以省略返回类型。这样，`sum()` 函数就简化为一行代码：

```kotlin
fun sum(x: Int, y: Int) = x + y

fun main() {
    println(sum(1, 2))
    // 3
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-simple-function-after"}

However, if you want your code to be quickly understood by other developers, it's a good idea to explicitly define the 
return type even when using the assignment operator `=`.
<br />但是，如果您希望其他开发人员能够快速理解您的代码，即使使用赋值运算符 `=`，最好也显式定义返回类型。

> If you use `{}` curly braces to declare your function body, you must declare the return type unless it is the `Unit` type.
> <br />如果使用 `{}` 花括号声明函数体，则必须声明返回类型，除非它是 `Unit` 类型。
> 
{style="note"}

## Early returns in functions
函数的提前返回

To stop the code in your function from being processed further than a certain point, use the `return` keyword. This example
uses `if` to return from a function early if the conditional expression is found to be true:
<br />要阻止函数中的代码执行到某个特定位置之后，请使用 `return` 关键字。以下示例使用 `if` 语句，如果条件表达式为真，则提前从函数返回：

```kotlin
// A list of registered usernames
val registeredUsernames = mutableListOf("john_doe", "jane_smith")

// A list of registered emails
val registeredEmails = mutableListOf("john@example.com", "jane@example.com")

fun registerUser(username: String, email: String): String {
    // Early return if the username is already taken
    if (username in registeredUsernames) {
        return "Username already taken. Please choose a different username."
    }

    // Early return if the email is already registered
    if (email in registeredEmails) {
        return "Email already registered. Please use a different email."
    }

    // Proceed with the registration if the username and email are not taken
    registeredUsernames.add(username)
    registeredEmails.add(email)

    return "User registered successfully: $username"
}

fun main() {
    println(registerUser("john_doe", "newjohn@example.com"))
    // Username already taken. Please choose a different username.
    println(registerUser("new_user", "newuser@example.com"))
    // User registered successfully: new_user
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-function-early-return"}

## Functions practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="functions-exercise-1"}

Write a function called `circleArea` that takes the radius of a circle in integer format as a parameter and outputs the
area of that circle.
<br />编写一个名为 `circleArea` 的函数，该函数接受一个整数格式的圆半径作为参数，并输出该圆的面积。

> In this exercise, you import a package so that you can access the value of <math>π</math> via `PI`. For more information about
> importing packages, see [Packages and imports](packages.md).
> <br />在本练习中，您需要导入一个包，以便可以通过 `PI` 访问 <math>π</math> 的值。有关导入包的更多信息，请参阅[包和导入](packages.md)。
>
{style="tip"}

<deflist collapsible="true" id="kotlin-tour-functions-exercise-1-hint">
    <def title="Hint">
        The formula for calculating the area of a circle is <math>πr^2</math>, where <math>r</math> is the radius.
        <br />计算圆面积的公式是 <math>πr^2</math> ，其中 <math>r</math> 是半径。
    </def>
</deflist>

|---|---|
```kotlin
import kotlin.math.PI

// Write your code here

fun main() {
    println(circleArea(2))
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-functions-exercise-1"}

|---|---|
```kotlin
import kotlin.math.PI

fun circleArea(radius: Int): Double {
    return PI * radius * radius
}

fun main() {
    println(circleArea(2)) // 12.566370614359172
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-functions-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="functions-exercise-2"}

Rewrite the `circleArea` function from the previous exercise as a single-expression function.
<br />将上一个练习中的 `circleArea` 函数重写为单表达式函数。

|---|---|
```kotlin
import kotlin.math.PI

// Write your code here

fun main() {
    println(circleArea(2))
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-functions-exercise-2"}

|---|---|
```kotlin
import kotlin.math.PI

fun circleArea(radius: Int): Double = PI * radius * radius

fun main() {
    println(circleArea(2)) // 12.566370614359172
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-functions-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="functions-exercise-3"}

You have a function that translates a time interval given in hours, minutes, and seconds into seconds. In most cases,
you need to pass only one or two function parameters while the rest are equal to 0. Improve the function and the code that
calls it by using default parameter values and named arguments so that the code is easier to read.
<br />你有一个函数，可以将以小时、分钟和秒为单位的时间间隔转换为秒。大多数情况下，
你只需要传递一个或两个函数参数，其余参数都为 0。
通过使用默认参数值和命名参数来改进该函数及其调用代码，使代码更易于阅读。

|---|---|
```kotlin
fun intervalInSeconds(hours: Int, minutes: Int, seconds: Int) =
    ((hours * 60) + minutes) * 60 + seconds

fun main() {
    println(intervalInSeconds(1, 20, 15))
    println(intervalInSeconds(0, 1, 25))
    println(intervalInSeconds(2, 0, 0))
    println(intervalInSeconds(0, 10, 0))
    println(intervalInSeconds(1, 0, 1))
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-functions-exercise-3"}

|---|---|
```kotlin
fun intervalInSeconds(hours: Int = 0, minutes: Int = 0, seconds: Int = 0) =
    ((hours * 60) + minutes) * 60 + seconds

fun main() {
    println(intervalInSeconds(1, 20, 15))
    println(intervalInSeconds(minutes = 1, seconds = 25))
    println(intervalInSeconds(hours = 2))
    println(intervalInSeconds(minutes = 10))
    println(intervalInSeconds(hours = 1, seconds = 1))
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-functions-solution-3"}

## Lambda expressions
Lambda表达式

Kotlin allows you to write even more concise code for functions by using lambda expressions.
<br />Kotlin 允许你使用 lambda 表达式编写更简洁的函数代码。

For example, the following `uppercaseString()` function:
<br />例如，以下 `uppercaseString()` 函数：

```kotlin
fun uppercaseString(text: String): String {
    return text.uppercase()
}
fun main() {
    println(uppercaseString("hello"))
    // HELLO
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-function-before"}

Can also be written as a lambda expression:
<br />也可以写成 lambda 表达式：

```kotlin
fun main() {
    val upperCaseString = { text: String -> text.uppercase() }
    println(upperCaseString("hello"))
    // HELLO
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-variable"}

Lambda expressions can be hard to understand at first glance so let's break it down. Lambda expressions are written 
within curly braces `{}`.
<br />Lambda 表达式乍一看可能难以理解，所以让我们来详细讲解一下。Lambda 表达式写在花括号 `{}` 内。

Within the lambda expression, you write:
<br />在 lambda 表达式中，您可以这样写：

* The parameters followed by an `->`.
  <br />参数后跟一个 `->`。
* The function body after the `->`.
  <br />`->` 后面的函数体。

In the previous example:
<br />在前一个例子中：

* `text` is a function parameter.
  <br />`text` 是一个函数参数。
* `text` has type `String`.
  <br />`text` 的类型为 `String`。
* The function returns the result of the [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html)
  <br />该函数返回 [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html) 的结果。
function called on `text`.
  <br />对 `text` 调用函数。
* The entire lambda expression is assigned to the `upperCaseString` variable with the assignment operator `=`.
  <br />整个 lambda 表达式通过赋值运算符 `=` 赋值给 `upperCaseString` 变量。
* The lambda expression is called by using the variable `upperCaseString` like a function and the string `"hello"` as a parameter.
  <br />lambda 表达式是通过将变量 `upperCaseString` 当作函数来调用，并将字符串 `"hello"` 作为参数来调用的。
* The `println()` function prints the result.
  <br />`println()` 函数会打印结果。

> If you declare a lambda without parameters, then there is no need to use `->`. For example:
> <br />如果声明的 lambda 表达式没有参数，则无需使用 `->`。例如：
> ```kotlin
> { println("Log message") }
> ```
>
{style="note"}

Lambda expressions can be used in a number of ways. You can:
<br />Lambda 表达式有多种用途。您可以：

* [Pass a lambda expression as a parameter to another function](#pass-to-another-function)
  <br />[将 lambda 表达式作为参数传递给另一个函数。](#pass-to-another-function)
* [Return a lambda expression from a function](#return-from-a-function)
  <br />[从函数返回 lambda 表达式](#return-from-a-function)
* [Invoke a lambda expression on its own](#invoke-separately)
  <br />[单独调用 lambda 表达式](#invoke-separately)

### Pass to another function
传递给另一个函数

A great example of when it is useful to pass a lambda expression to a function, is using the [`.filter()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html)
function on collections:
<br />向函数传递 lambda 表达式的一个绝佳示例是使用集合上的 [`.filter()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/filter.html) 函数：

```kotlin
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
    
    val positives = numbers.filter ({ x -> x > 0 })
    
    val isNegative = { x: Int -> x < 0 }
    val negatives = numbers.filter(isNegative)
    
    println(positives)
    // [1, 3, 5]
    println(negatives)
    // [-2, -4, -6]
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-filter"}

The `.filter()` function accepts a lambda expression as a predicate and applies it to each element of the list. The function keeps an element only if the predicate returns `true`:
<br />`.filter()` 函数接受一个 lambda 表达式作为谓词，并将其应用于列表中的每个元素。该函数仅当谓词返回 `true` 时才保留元素：

* `{ x -> x > 0 }` returns `true` if the element is positive.
  <br />如果元素为正数，则 `{ x -> x > 0 }` 返回 `true`。
* `{ x -> x < 0 }` returns `true` if the element is negative.
  <br />如果元素为负数，则 `{ x -> x < 0 }` 返回 `true`。

This example demonstrates two ways of passing a lambda expression to a function:
<br />此示例演示了将 lambda 表达式传递给函数的两种方法：

* For positive numbers, the example adds the lambda expression directly in the `.filter()` function.
  <br />对于正数，该示例直接在 `.filter()` 函数中添加 lambda 表达式。
* For negative numbers, the example assigns the lambda expression to the `isNegative` variable. Then
the `isNegative` variable is used as a function parameter in the `.filter()` function. In this case, you have to specify
the type of function parameters (`x`) in the lambda expression.
  <br />对于负数，示例中将 lambda 表达式赋值给 `isNegative` 变量。
然后，`isNegative` 变量被用作 `.filter()` 函数的函数参数。在这种情况下，您必须在 lambda 表达式中指定函数参数 (`x`) 的类型。

> If a lambda expression is the only function parameter, you can drop the function parentheses `()`:
> <br />如果 lambda 表达式是唯一的函数参数，则可以省略函数括号 `()`：
> 
> ```kotlin
> val positives = numbers.filter { x -> x > 0 }
> ```
> 
> This is an example of a [trailing lambda](#trailing-lambdas), which is discussed in more detail at the end of this
> chapter.
> <br />这是一个[尾随 lambda](#trailing-lambdas)的例子，本章末尾将对此进行更详细的讨论。
>
{style="note"}

Another good example, is using the [`.map()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html) 
function to transform items in a collection:
<br />另一个很好的例子是使用 [`.map()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map.html) 函数来转换集合中的元素：

```kotlin
fun main() {
    //sampleStart
    val numbers = listOf(1, -2, 3, -4, 5, -6)
    val doubled = numbers.map { x -> x * 2 }
    
    val isTripled = { x: Int -> x * 3 }
    val tripled = numbers.map(isTripled)
    
    println(doubled)
    // [2, -4, 6, -8, 10, -12]
    println(tripled)
    // [3, -6, 9, -12, 15, -18]
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-map"}

The `.map()` function accepts a lambda expression as a transform function:
<br />`.map()` 函数接受一个 lambda 表达式作为转换函数：

* `{ x -> x * 2 }` takes each element of the list and returns that element multiplied by 2.
  <br />`{ x -> x * 2 }` 取列表中的每个元素，并返回该元素乘以 2 的结果。
* `{ x -> x * 3 }` takes each element of the list and returns that element multiplied by 3.
  <br />`{ x -> x * 3 }` 取列表中的每个元素，并返回该元素乘以 3 的结果。

### Function types
函数类型

Before you can return a lambda expression from a function, you first need to understand **function
types**.
<br />在从函数返回 lambda 表达式之前，首先需要了解**函数 类型**。

You have already learned about basic types but functions themselves also have a type. Kotlin's type inference 
can infer a function's type from the parameter type. But there may be times when you need to explicitly
specify the function type. The compiler needs the function type so that it knows what is and isn't 
allowed for that function.
<br />你已经学习了基本类型，但函数本身也有类型。Kotlin 的类型推断可以根据参数类型推断函数类型。
但有时你可能需要显式地指定函数类型。编译器需要函数类型，以便知道该函数允许什么，不允许什么。

The syntax for a function type has:
<br />函数类型的语法如下：

* Each parameter's type written within parentheses `()` and separated by commas `,`.
  <br />每个参数的类型写在括号 `()` 内，并用逗号 `,` 分隔。
* The return type written after `->`.
  <br />`->` 后面写的返回类型。

For example: `(String) -> String` or `(Int, Int) -> Int`.
<br />例如：`(String) -> String` 或 `(Int, Int) -> Int`。

This is what a lambda expression looks like if a function type for `upperCaseString()` is defined:
<br />如果定义了 `upperCaseString()` 函数类型，则 lambda 表达式如下所示：

```kotlin
val upperCaseString: (String) -> String = { text -> text.uppercase() }

fun main() {
    println(upperCaseString("hello"))
    // HELLO
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-function-type"}

If your lambda expression has no parameters then the parentheses `()` are left empty. For example: `() -> Unit`
<br />如果你的 lambda 表达式没有参数，那么括号 `()` 将留空。例如：`() -> Unit`

> You must declare parameter and return types either in the lambda expression or as a function type. Otherwise, the
> compiler won't be able to know what type your lambda expression is.
> <br />你必须在 lambda 表达式中或作为函数类型声明参数和返回值类型。否则，编译器将无法确定 lambda 表达式的类型。
> 
> For example, the following won't work:
> <br />例如，以下操作将无法生效：
> 
> `val upperCaseString = { str -> str.uppercase() }`
>
{style="note"}

### Return from a function
函数返回

Lambda expressions can be returned from a function. So that the compiler understands what type the lambda
expression returned is, you must declare a function type.
<br />Lambda 表达式可以从函数返回。为了让编译器理解返回的 Lambda 表达式的类型，您必须声明函数类型。

In the following example, the `toSeconds()` function has function type `(Int) -> Int` because it always returns a lambda
expression that takes a parameter of type `Int` and returns an `Int` value.
<br />在以下示例中，`toSeconds()` 函数的函数类型为 `(Int) -> Int`，因为它总是返回一个 lambda 表达式，该表达式接受一个类型为 `Int` 的参数并返回一个 `Int` 值。

This example uses a `when` expression to determine which lambda expression is returned when `toSeconds()` is called:
<br />此示例使用 `when` 表达式来确定在调用 `toSeconds()` 时返回哪个 lambda 表达式：

```kotlin
fun toSeconds(time: String): (Int) -> Int = when (time) {
    "hour" -> { value -> value * 60 * 60 }
    "minute" -> { value -> value * 60 }
    "second" -> { value -> value }
    else -> { value -> value }
}

fun main() {
    val timesInMinutes = listOf(2, 10, 15, 1)
    val min2sec = toSeconds("minute")
    val totalTimeInSeconds = timesInMinutes.map(min2sec).sum()
    println("Total time is $totalTimeInSeconds secs")
    // Total time is 1680 secs
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-return-from-function"}

### Invoke separately
分别调用

Lambda expressions can be invoked on their own by adding parentheses `()` after the curly braces `{}` and including
any parameters within the parentheses:
<br />Lambda 表达式可以通过在花括号 `{}` 后添加圆括号 `()` 并包含以下参数来单独调用：
任何参数都包含在圆括号内：

```kotlin
fun main() {
    //sampleStart
    println({ text: String -> text.uppercase() }("hello"))
    // HELLO
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambda-standalone"}

### Trailing lambdas
尾部 lambda

As you have already seen, if a lambda expression is the only function parameter, you can drop the function parentheses `()`.
If a lambda expression is passed as the last parameter of a function, then the expression can be written outside the
function parentheses `()`. In both cases, this syntax is called a **trailing lambda**.
<br />如您所见，如果 lambda 表达式是函数的唯一参数，则可以省略函数括号 `()`。
如果 lambda 表达式作为函数的最后一个参数传递，则可以将其写在函数括号 `()` 之外。这两种情况下，这种语法都称为**尾随 lambda**。

For example, the [`.fold()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/fold.html) function accepts an 
initial value and an operation:
<br />例如，[`.fold()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/fold.html) 函数接受一个初始值和一个操作：

```kotlin
fun main() {
    //sampleStart
    // The initial value is zero. 
    // The operation sums the initial value with every item in the list cumulatively.
    println(listOf(1, 2, 3).fold(0, { x, item -> x + item })) // 6

    // Alternatively, in the form of a trailing lambda
    println(listOf(1, 2, 3).fold(0) { x, item -> x + item })  // 6
    //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-trailing-lambda"}

For more information on lambda expressions, see [Lambda expressions and anonymous functions](lambdas.md#lambda-expressions-and-anonymous-functions).
<br />有关 lambda 表达式的更多信息，请参阅[Lambda 表达式和匿名函数](lambdas.md#lambda-expressions-and-anonymous-functions)。

The next step in our tour is to learn about [classes](kotlin-tour-classes.md) in Kotlin.
<br />接下来，我们将学习 Kotlin 中的 [classes](kotlin-tour-classes.md)。

## Lambda expressions practice
Lambda 表达式练习

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="lambdas-exercise-1"}

You have a list of actions supported by a web service, a common prefix for all requests, and an ID of a particular resource.
To request an action `title` over the resource with ID: 5, you need to create the following URL: `https://example.com/book-info/5/title`.
Use a lambda expression to create a list of URLs from the list of actions.
<br />您拥有一个 Web 服务支持的操作列表、所有请求的通用前缀以及特定资源的 ID。
要对 ID 为 5 的资源请求 `title` 操作，您需要创建以下 URL：`https://example.com/book-info/5/title`。
使用 lambda 表达式从操作列表中创建 URL 列表。

|---|---|
```kotlin
fun main() {
    val actions = listOf("title", "year", "author")
    val prefix = "https://example.com/book-info"
    val id = 5
    val urls = // Write your code here
    println(urls)
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambdas-exercise-1"}

|---|---|
```kotlin
fun main() {
    val actions = listOf("title", "year", "author")
    val prefix = "https://example.com/book-info"
    val id = 5
    val urls = actions.map { action -> "$prefix/$id/$action" }
    println(urls)
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-lambdas-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="lambdas-exercise-2"}

Write a function that takes an `Int` value and an action (a function with type `() -> Unit`) which then repeats the 
action the given number of times. Then use this function to print “Hello” 5 times.
<br />编写一个函数，该函数接受一个 `Int` 值和一个动作（一个类型为 `() -> Unit` 的函数），然后重复执行该动作指定的次数。最后，使用此函数打印 5 次“Hello”。

|---|---|
```kotlin
fun repeatN(n: Int, action: () -> Unit) {
    // Write your code here
}

fun main() {
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lambdas-exercise-2"}

|---|---|
```kotlin
fun repeatN(n: Int, action: () -> Unit) {
    for (i in 1..n) {
        action()
    }
}

fun main() {
    repeatN(5) {
        println("Hello")
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-lambdas-solution-2"}

## Next step
接下来

[Classes](kotlin-tour-classes.md)
<br />[类](kotlin-tour-classes.md)
