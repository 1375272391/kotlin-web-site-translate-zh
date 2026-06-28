[//]: # (title: Basic syntax overview)

This is a collection of basic syntax elements with examples. At the end of every section, you'll find a link to
a detailed description of the related topic.
<br />这是一系列基本语法元素及其示例。每节末尾都附有链接，指向 相关主题的详细说明。

You can also learn all the Kotlin essentials with the free [Kotlin Core track](https://hyperskill.org/tracks?category=4&utm_source=jbkotlin_hs&utm_medium=referral&utm_campaign=kotlinlang-docs&utm_content=button_1&utm_term=22.03.23)
by JetBrains Academy.
<br />您还可以通过 JetBrains Academy 提供的免费 [Kotlin Core 课程](https://hyperskill.org/tracks?category=4&utm_source=jbkotlin_hs&utm_medium=referral&utm_campaign=kotlinlang-docs&utm_content=button_1&utm_term=22.03.23)学习所有 Kotlin 基础知识。

## Package definition and imports
包定义和导入

Package specification should be at the top of the source file:
<br />包规范应位于源文件的顶部：

```kotlin
package my.demo

import kotlin.text.*

// ...
```

It is not required to match directories and packages: source files can be placed arbitrarily in the file system.
<br />不需要匹配目录和软件包：源文件可以任意放置在文件系统中。

See [Packages](packages.md).
<br />请参阅[软件包](packages.md)。

## Program entry point
程序入口点

An entry point of a Kotlin application is the `main` function:
<br />Kotlin应用程序的入口点是`main`函数：

```kotlin
fun main() {
    println("Hello world!")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-hello-world"}

Another form of `main` accepts a variable number of `String` arguments: 
<br />`main` 函数的另一种形式接受可变数量的 `String` 参数：

```kotlin
fun main(args: Array<String>) {
    println(args.contentToString())
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

## Print to the standard output
打印到标准输出

`print` prints its argument to the standard output:
<br />`print` 函数将其参数打印到标准输出：

```kotlin
fun main() {
//sampleStart
    print("Hello ")
    print("world!")
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-print"}

`println` prints its arguments and adds a line break, so that the next thing you print appears on the next line:
<br />`println` 函数会打印出它的参数，并在末尾添加一个换行符，以便接下来打印的内容出现在下一行：

```kotlin
fun main() {
//sampleStart
    println("Hello world!")
    println(42)
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-println"}

## Read from the standard input
从标准输入读取

The `readln()` function reads from the standard input. This function reads the entire line the user enters as a string.
<br />`readln()` 函数从标准输入读取内容。该函数会将用户输入的整行内容读取为一个字符串。

You can use the `println()`, `readln()`, and `print()` functions together to print messages requesting 
and showing user input:
<br />您可以结合使用 `println()`、`readln()` 和 `print()` 函数来打印请求和显示用户输入的消息：

```kotlin
// Prints a message to request input
println("Enter any word: ")

// Reads and stores the user input. For example: Happiness
val yourWord = readln()

// Prints a message with the input
print("You entered the word: ")
print(yourWord)
// You entered the word: Happiness
```

For more information, see [Read standard input](read-standard-input.md).
<br />有关更多信息，请参阅[读取标准输入](read-standard-input.md)。

## Functions
函数

A function with two `Int` parameters and `Int` return type:
<br />一个具有两个 `Int` 参数和 `Int` 返回类型的函数：

```kotlin
//sampleStart
fun sum(a: Int, b: Int): Int {
    return a + b
}
//sampleEnd

fun main() {
    print("sum of 3 and 5 is ")
    println(sum(3, 5))
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-return-int"}

A function body can be an expression. Its return type is inferred:
<br />函数体可以是表达式。其返回类型是自动推断的：

```kotlin
//sampleStart
fun sum(a: Int, b: Int) = a + b
//sampleEnd

fun main() {
    println("sum of 19 and 23 is ${sum(19, 23)}")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-function-expression"}

A function that returns no meaningful value:
<br />一个不返回任何有意义值的函数：

```kotlin
//sampleStart
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
//sampleEnd

fun main() {
    printSum(-1, 8)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-return-unit"}

`Unit` return type can be omitted:
<br />可以省略 `Unit` 返回类型：

```kotlin
//sampleStart
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
//sampleEnd

fun main() {
    printSum(-1, 8)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-function-omit-unit"}

See [Functions](functions.md).
<br />请参阅[函数](functions.md)。

## Variables
变量

In Kotlin, you declare a variable starting with a keyword, `val` or `var`, followed by the name of the variable.
<br />在 Kotlin 中，声明变量时，要以关键字 `val` 或 `var` 开头，后跟变量名。

Use the `val` keyword to declare variables that are assigned a value only once. These are immutable, read-only local variables that can't be reassigned a different value
after initialization: 
<br />使用 `val` 关键字声明只赋值一次的变量。这些是不可变的、只读的局部变量，初始化后不能被重新赋值。

```kotlin
fun main() {
//sampleStart
    // Declares the variable x and initializes it with the value of 5
    val x: Int = 5
    // 5
//sampleEnd
    println(x)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-val"}

Use the `var` keyword to declare variables that can be reassigned. These are mutable variables, and you can change their values after initialization:
<br />使用 `var` 关键字声明可以重新赋值的变量。这些是可变变量，您可以在初始化后更改它们的值：

```kotlin
fun main() {
//sampleStart
    // Declares the variable x and initializes it with the value of 5
    var x: Int = 5
    // Reassigns a new value of 6 to the variable x
    x += 1
    // 6
//sampleEnd
    println(x)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-var"}

Kotlin supports type inference and automatically identifies the data type of a declared variable. When declaring a variable, you can omit the type after the variable name:
<br />Kotlin 支持类型推断，能够自动识别已声明变量的数据类型。声明变量时，可以省略变量名后的类型：

```kotlin
fun main() {
//sampleStart
    // Declares the variable x with the value of 5;`Int` type is inferred
    val x = 5
    // 5
//sampleEnd
    println(x)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-inference"}

You can use variables only after initializing them. You can either initialize a variable at the moment of declaration or declare a variable first and initialize it later. 
In the second case, you must specify the data type:
<br />变量只有在初始化之后才能使用。你可以直接在声明变量时进行初始化，也可以先声明变量，稍后再进行初始化。
如果是第二种情况，则必须指定数据类型：

```kotlin
fun main() {
//sampleStart
    // Initializes the variable x at the moment of declaration; type is not required
    val x = 5
    // Declares the variable c without initialization; type is required
    val c: Int
    // Initializes the variable c after declaration 
    c = 3
    // 5 
    // 3
//sampleEnd
    println(x)
    println(c)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-initialize"}

You can declare variables at the top level:
<br />您可以在顶层声明变量：

```kotlin
//sampleStart
val PI = 3.14
var x = 0

fun incrementX() {
    x += 1
}
// x = 0; PI = 3.14
// incrementX()
// x = 1; PI = 3.14
//sampleEnd

fun main() {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-variable-top-level"}

For information about declaring properties, see [Properties](properties.md).
<br />有关声明属性的信息，请参阅[属性](properties.md)。

## Creating classes and instances
创建类和实例

To define a class, use the `class` keyword:
<br />要定义一个类，请使用 `class` 关键字：
```kotlin
class Shape
```

Properties of a class can be listed in its declaration or body: 
<br />类的属性可以列在其声明或类体中：

```kotlin
class Rectangle(val height: Double, val length: Double) {
    val perimeter = (height + length) * 2 
}
```

The default constructor with parameters listed in the class declaration is available automatically:
<br />类声明中列出的带参数的默认构造函数会自动启用：

```kotlin
class Rectangle(val height: Double, val length: Double) {
    val perimeter = (height + length) * 2 
}
fun main() {
    val rectangle = Rectangle(5.0, 2.0)
    println("The perimeter is ${rectangle.perimeter}")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-class-constructor"}

Inheritance between classes is declared by a colon (`:`). Classes are `final` by default; to make a class inheritable, 
mark it as `open`:
<br />类之间的继承关系用冒号（`:`）声明。类默认是`final`的；要使类可继承，请将其标记为`open`：

```kotlin
open class Shape

class Rectangle(val height: Double, val length: Double): Shape() {
    val perimeter = (height + length) * 2 
}
```

For more information about constructors and inheritance, see [Classes](classes.md) and [Objects and instances](object-declarations.md).
<br />有关构造函数和继承的更多信息，请参阅[类](classes.md)和[对象和实例](object-declarations.md)。

## Comments
注释

Just like most modern languages, Kotlin supports single-line (or _end-of-line_) and multi-line (_block_) comments:
<br />与大多数现代语言一样，Kotlin 支持单行（或行尾）注释和多行（块级）注释：

```kotlin
// This is an end-of-line comment

/* This is a block comment
   on multiple lines. */
```

Block comments in Kotlin can be nested:
<br />Kotlin 中的代码块注释可以嵌套：

```kotlin
/* The comment starts here
/* contains a nested comment *​/  
and ends here. */
```

See [Documenting Kotlin Code](kotlin-doc.md) for information on the documentation comment syntax.
<br />有关文档注释语法的信息，请参阅[Kotlin 代码文档](kotlin-doc.md)。

## String templates
字符串模板

```kotlin
fun main() {
//sampleStart
    var a = 1
    // simple name in template:
    val s1 = "a is $a" 
    
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
//sampleEnd
    println(s2)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-string-templates"}

See [String templates](strings.md#string-templates) for details.
<br />有关详细信息，请参阅[字符串模板](strings.md#string-templates)。

## Conditional expressions
条件表达式

```kotlin
//sampleStart
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
//sampleEnd

fun main() {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-conditional-expressions"}

In Kotlin, `if` can also be used as an expression:
<br />在 Kotlin 中，`if` 也可以用作表达式：

```kotlin
//sampleStart
fun maxOf(a: Int, b: Int) = if (a > b) a else b
//sampleEnd

fun main() {
    println("max of 0 and 42 is ${maxOf(0, 42)}")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-if-expression"}

See [`if`-expressions](control-flow.md#if-expression).
<br />请参阅[`if`表达式](control-flow.md#if-expression)。

## for loop
for 循环

```kotlin
fun main() {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-for-loop"}

or:
<br />或者：

```kotlin
fun main() {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-for-loop-indices"}

See [for loop](control-flow.md#for-loops).
<br />请参阅[for循环](control-flow.md#for-loops)。

## while loop
while 循环

```kotlin
fun main() {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
        index++
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-while-loop"}

See [while loop](control-flow.md#while-loops).
<br />请参阅[while循环](control-flow.md#while-loops)。

## when expression
when表达式

```kotlin
//sampleStart
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
//sampleEnd

fun main() {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-when-expression"}

See [when expressions and statements](control-flow.md#when-expressions-and-statements).
<br />请参阅[when表达式和语句](control-flow.md#when-expressions-and-statements)。

## Ranges
范围

Check if a number is within a range using `in` operator:
<br />使用 `in` 运算符检查数字是否在范围内：

```kotlin
fun main() {
//sampleStart
    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-range-in"}

Check if a number is out of range:
<br />检查数字是否超出范围：

```kotlin
fun main() {
//sampleStart
    val list = listOf("a", "b", "c")
    
    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range, too")
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-out-of-range"}

Iterate over a range:
<br />遍历一个范围：

```kotlin
fun main() {
//sampleStart
    for (x in 1..5) {
        print(x)
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-iterate-range"}

Or over a progression:
<br />或者通过某种方式：

```kotlin
fun main() {
//sampleStart
    for (x in 1..10 step 2) {
        print(x)
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x)
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-iterate-progression"}

See [Ranges and progressions](ranges.md).
<br />请参阅[范围和进展](ranges.md)。

## Collections
集合

Iterate over a collection:
<br />遍历集合：

```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
//sampleStart
    for (item in items) {
        println(item)
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-iterate-collection"}

Check if a collection contains an object using `in` operator:
<br />使用 `in` 运算符检查集合是否包含对象：

```kotlin
fun main() {
    val items = setOf("apple", "banana", "kiwifruit")
//sampleStart
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-collection-in"}

Use [lambda expressions](lambdas.md) to filter and map collections:
<br />使用[lambda表达式](lambdas.md)来过滤和映射集合：

```kotlin
fun main() {
//sampleStart
    val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
    fruits
      .filter { it.startsWith("a") }
      .sortedBy { it }
      .map { it.uppercase() }
      .forEach { println(it) }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-collection-filter-map"}

See [Collections overview](collections-overview.md).
<br />请参阅[集合概览](collections-overview.md)。

## Nullable values and null checks
可为空值和空值检查

A reference must be explicitly marked as nullable when a `null` value is possible. Nullable type names have `?` at the end.
For example, `Int?`.
<br />当引用可能为 `null` 值时，必须显式地将其标记为可空类型。可空类型名称以 `?` 结尾。
例如，`Int?`。

Return `null` if `str` does not hold an integer:
<br />如果字符串 `str` 不包含整数，则返回 `null`：

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}
```

Use a function returning nullable value:
<br />使用返回可为空值的函数：

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

//sampleStart
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    }    
}
//sampleEnd

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-function-nullable-value"}

or:
<br />或者：

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)
    
//sampleStart
    // ...
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }

    // x and y are automatically cast to non-nullable after null check
    println(x * y)
//sampleEnd
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("99", "b")
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-function-null-check"}

See [Null-safety](null-safety.md).
<br />请参阅[空安全](null-safety.md)。

## Type checks and automatic casts
类型检查和自动铸造

The `is` operator checks if an expression is an instance of a type.
If an immutable local variable or property is checked for a specific type, there's no need to cast it explicitly:
<br />`is` 运算符用于检查表达式是否为某个类型的实例。
如果要检查不可变局部变量或属性的类型，则无需显式进行类型转换：

```kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` is automatically cast to `String` in this branch
        return obj.length
    }

    // `obj` is still of type `Any` outside of the type-checked branch
    return null
}
//sampleEnd

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-is-operator"}

or:
<br />或者

```kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}
//sampleEnd

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-is-operator-expression"}

or even:
<br />甚至：

```kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    // `obj` is automatically cast to `String` on the right-hand side of `&&`
    if (obj is String && obj.length >= 0) {
        return obj.length
    }

    return null
}
//sampleEnd

fun main() {
    fun printLength(obj: Any) {
        println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Error: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength("")
    printLength(1000)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-basic-syntax-is-operator-logic"}

See [Classes](classes.md) and [Type casts](typecasts.md).
<br />请参阅[类](classes.md)和[类型转换](typecasts.md)。
