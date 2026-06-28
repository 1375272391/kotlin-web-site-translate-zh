[//]: # (title: Null safety)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5-done.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6-done.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7.svg" width="20" alt="Final step" /> <strong>Null safety</strong><br /></p>
</tldr>

In Kotlin, it's possible to have a `null` value. Kotlin uses `null` values when something is missing or not yet set.
You've already seen an example of Kotlin returning a `null` value in the [Collections](kotlin-tour-collections.md#kotlin-tour-map-no-key)
chapter when you tried to access a key-value pair with a key that doesn't exist in the map. Although it's useful to use
`null` values in this way, you might run into problems if your code isn't prepared to handle them. 
<br />在 Kotlin 中，可以使用 `null` 值。当缺少某些内容或尚未设置时，Kotlin 会使用 `null` 值。
在 [Collections](kotlin-tour-collections.md#kotlin-tour-map-no-key) 章节中，您已经看到过 Kotlin 返回 `null` 值的示例：当您尝试访问一个键值对，而该键在映射中不存在时。
虽然以这种方式使用 `null` 值很有用，但如果您的代码没有做好处理准备，则可能会遇到问题。

To help prevent issues with `null` values in your programs, Kotlin has null safety in place. Null safety detects
potential problems with `null` values at compile time, rather than at run time.
<br />为了帮助避免程序中出现 `null` 值的问题，Kotlin 引入了空值安全机制。空值安全机制会在编译时而非运行时检测出 `null` 值可能存在的问题。

Null safety is a combination of features that allow you to:
<br />空安全是一系列功能的组合，允许您：

* Explicitly declare when `null` values are allowed in your program.
  <br />在程序中明确声明何时允许使用 `null` 值。
* Check for `null` values.
  <br />检查是否存在空值。
* Use safe calls to properties or functions that may contain `null` values.
  <br />对可能包含“null”值的属性或函数使用安全调用。
* Declare actions to take if `null` values are detected.
  <br />声明检测到 `null` 值时要采取的操作。

## Nullable types
可空类型

Kotlin supports nullable types which allows the possibility for the declared type to have `null` values. By default, a type
is **not** allowed to accept `null` values. Nullable types are declared by explicitly adding `?` after the type declaration.
<br />Kotlin 支持可空类型，允许声明的类型包含 `null` 值。默认情况下，类型
**不允许**接受 `null` 值。可空类型通过在类型声明后显式添加 `?` 来声明。

For example:
<br />例如：

```kotlin
fun main() {
    // neverNull has String type
    var neverNull: String = "This can't be null"

    // Throws a compiler error
    neverNull = null

    // nullable has nullable String type
    var nullable: String? = "You can keep a null here"

    // This is OK
    nullable = null

    // By default, null values aren't accepted
    var inferredNonNull = "The compiler assumes non-nullable"

    // Throws a compiler error
    inferredNonNull = null

    // notNull doesn't accept null values
    fun strLength(notNull: String): Int {                 
        return notNull.length
    }

    println(strLength(neverNull)) // 18
    println(strLength(nullable))  // Throws a compiler error
}
```
{kotlin-runnable="true" validate="false" kotlin-min-compiler-version="1.3" id="kotlin-tour-nullable-type"}

> `length` is a property of the [String](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/) class that 
> contains the number of characters within a string.
> <br />`length` 是 [String](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-string/) 类的一个属性，它包含字符串中的字符数。
>
{style="tip"}

## Check for null values
检查空值

You can check for the presence of `null` values within conditional expressions. In the following example, the `describeString()`
function has an `if` statement that checks whether `maybeString` is **not** `null` and if its `length` is greater than zero:
<br />您可以在条件表达式中检查是否存在 `null` 值。
在以下示例中，`describeString()` 函数包含一个 `if` 语句，用于检查 `maybeString` 是否**不**为 `null` 以及其 `length` 是否大于零：

```kotlin
fun describeString(maybeString: String?): String {
    if (maybeString != null && maybeString.length > 0) {
        return "String of length ${maybeString.length}"
    } else {
        return "Empty or null string"
    }
}

fun main() {
    val nullString: String? = null
    println(describeString(nullString))
    // Empty or null string
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-check-nulls"}

## Use safe calls
使用安全调用

To safely access properties of an object that might contain a `null` value, use the safe call operator `?.`. The safe call
operator returns `null` if either the object or one of its accessed properties is `null`. This is useful if you want to avoid the presence of `null`
values triggering errors in your code.
<br />要安全地访问可能包含 `null` 值的对象的属性，请使用安全调用运算符 `?.`。
如果对象或其访问的属性为 `null`，则安全调用运算符将返回 `null`。如果您希望避免因 `null` 值而导致代码出错，这将非常有用。

In the following example, the `lengthString()` function uses a safe call to return either the length of the string or `null`:
<br />在以下示例中，`lengthString()` 函数使用安全调用来返回字符串的长度或 `null`：

```kotlin
fun lengthString(maybeString: String?): Int? = maybeString?.length

fun main() { 
    val nullString: String? = null
    println(lengthString(nullString))
    // null
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-safe-call-property"}

> Safe calls can be chained so that if any property of an object contains a `null` value, then `null` is returned without 
> an error being thrown. For example:
> <br />可以链式调用安全函数，这样，如果对象的任何属性包含 `null` 值，则返回 `null` 而不会抛出错误。
> 例如：
> 
> ```kotlin
>   person.company?.address?.country
> ```
>
{style="tip"}

The safe call operator can also be used to safely call an extension or member function. In this case, a null check is 
performed before the function is called. If the check detects a `null` value, then the call is skipped and `null` is returned.
<br />安全调用运算符也可用于安全地调用扩展函数或成员函数。在这种情况下，函数调用之前会执行空值检查。如果检查检测到 `null` 值，则跳过调用并返回 `null`。

In the following example, `nullString` is `null` so the invocation of [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html)
is skipped and `null` is returned:
<br />在以下示例中，`nullString` 为 `null`，因此会跳过对 [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html) 的调用，
并返回 `null`：

```kotlin
fun main() {
    val nullString: String? = null
    println(nullString?.uppercase())
    // null
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-safe-call-function"}

## Use Elvis operator
使用 Elvis 操作

You can provide a default value to return if a `null` value is detected by using the **Elvis operator** `?:`.
<br />如果检测到 `null` 值，可以使用 **Elvis 运算符** `?:` 提供要返回的默认值。

Write on the left-hand side of the Elvis operator what should be checked for a `null` value.
Write on the right-hand side of the Elvis operator what should be returned if a `null` value is detected.
<br />在 Elvis 运算符的左侧写出需要检查的值是否为 `null`。
<br />在 Elvis 运算符的右侧写出如果检测到 `null` 值，应该返回什么。

In the following example, `nullString` is `null` so the safe call to access the `length` property returns a `null` value.
As a result, the Elvis operator returns `0`:
<br />在以下示例中，`nullString` 的值为 `null`，因此安全调用访问 `length` 属性会返回 `null` 值。
结果，Elvis 运算符返回 `0`：

```kotlin
fun main() {
    val nullString: String? = null
    println(nullString?.length ?: 0)
    // 0
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-elvis-operator"}

For more information about null safety in Kotlin, see [Null safety](null-safety.md).
<br />有关 Kotlin 中空安全性的更多信息，请参阅[空安全性](null-safety.md)。

## Practice
实践

### Exercise {initial-collapse-state="collapsed" collapsible="true"}

You have the `employeeById` function that gives you access to a database of employees of a company. Unfortunately, this 
function returns a value of the `Employee?` type, so the result can be `null`. Your goal is to write a function that 
returns the salary of an employee when their `id` is provided, or `0` if the employee is missing from the database.
<br />您可以使用`employeeById`函数来访问公司员工的数据库。
不幸的是，这函数返回`Employee?`类型的值，因此结果可以为`null`。
您的目标是编写一个函数如果提供了`id`，则返回员工的工资；如果数据库中缺少该员工，则返回`0`。

|---|---|
```kotlin
data class Employee (val name: String, var salary: Int)

fun employeeById(id: Int) = when(id) {
    1 -> Employee("Mary", 20)
    2 -> null
    3 -> Employee("John", 21)
    4 -> Employee("Ann", 23)
    else -> null
}

fun salaryById(id: Int) = // Write your code here

fun main() {
    println((1..5).sumOf { id -> salaryById(id) })
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-null-safety-exercise"}

|---|---|
```kotlin
data class Employee (val name: String, var salary: Int)

fun employeeById(id: Int) = when(id) {
    1 -> Employee("Mary", 20)
    2 -> null
    3 -> Employee("John", 21)
    4 -> Employee("Ann", 23)
    else -> null
}

fun salaryById(id: Int) = employeeById(id)?.salary ?: 0

fun main() {
    println((1..5).sumOf { id -> salaryById(id) })
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-null-safety-solution"}

## What's next?
接下来是什么？

Congratulations! Now that you have completed the beginner tour, take your understanding of Kotlin to the next level with
our intermediate tour:
<br />恭喜！您已完成 Kotlin 入门教程，现在可以通过我们的中级教程进一步提升您的 Kotlin 水平：

<a href="kotlin-tour-intermediate-extension-functions.md"><img src="start-intermediate-tour.svg" width="700" alt="Start the intermediate Kotlin tour" style="block"/></a>
