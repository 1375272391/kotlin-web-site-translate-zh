[//]: # (title: Idioms)

A collection of random and frequently used idioms in Kotlin. If you have a favorite idiom, contribute it by sending a pull request.
<br />这里收集了一些 Kotlin 中常用的随机惯用法。如果你有喜欢的惯用法，欢迎提交 pull request 来贡献出来。

## Create DTOs (POJOs/POCOs)
创建 DTO（POJO/POCO）

```kotlin
data class Customer(val name: String, val email: String)
```

provides a `Customer` class with the following functionality:
<br />提供一个具有以下功能的 `Customer` 类：

* getters (and setters in case of `var`s) for all properties
  <br />所有属性的 getter（以及 `var` 类型的 setter）
* `equals()`
* `hashCode()`
* `toString()`
* `copy()`
* `component1()`, `component2()`, ..., for all properties (see [Data classes](data-classes.md))
  <br />`component1()`、`component2()`、...，用于所有属性（参见[数据类](data-classes.md)）

## Default values for function parameters
函数参数的默认值

```kotlin
fun foo(a: Int = 0, b: String = "") { ... }
```

## Filter a list
筛选列表

```kotlin
val positives = list.filter { x -> x > 0 }
```

Or alternatively, even shorter:
<br />或者，更简洁一些：

```kotlin
val positives = list.filter { it > 0 }
```

Learn the difference between [Java and Kotlin filtering](java-to-kotlin-collections-guide.md#filter-elements).
<br />了解 [Java 和 Kotlin 过滤](java-to-kotlin-collections-guide.md#filter-elements) 之间的区别。

## Check the presence of an element in a collection
检查集合中是否存在某个元素

```kotlin
if ("john@example.com" in emailsList) { ... }

if ("jane@example.com" !in emailsList) { ... }
```

## String interpolation
字符串插值

```kotlin
println("Name $name")
```

Learn the difference between [Java and Kotlin string concatenation](java-to-kotlin-idioms-strings.md#concatenate-strings).
<br />了解 [Java 和 Kotlin 字符串连接](java-to-kotlin-idioms-strings.md#concatenate-strings)之间的区别。

## Read standard input safely
安全读取标准输入

```kotlin
// Reads a string and returns null if the input can't be converted into an integer. For example: Hi there!
val wrongInt = readln().toIntOrNull()
println(wrongInt)
// null

// Reads a string that can be converted into an integer and returns an integer. For example: 13
val correctInt = readln().toIntOrNull()
println(correctInt)
// 13
```

For more information, see [Read standard input.](read-standard-input.md)
<br />更多信息，请参阅[读取标准输入](read-standard-input.md)。

## Instance checks
实例检查

```kotlin
when (x) {
    is Foo -> ...
    is Bar -> ...
    else   -> ...
}
```

## Read-only list
只读列表

```kotlin
val list = listOf("a", "b", "c")
```
## Read-only map
只读 map

```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

## Access a map entry
访问 map 条目

```kotlin
println(map["key"])
map["key"] = value
```

## Traverse a map or a list of pairs
遍历 map 或配对列表

```kotlin
for ((k, v) in map) {
    println("$k -> $v")
}
```

`k` and `v` can be any convenient names, such as `name` and `age`.
<br />`k` 和 `v` 可以是任何方便的名称，例如 `name` 和 `age`。

## Iterate over a range
遍历一个范围

```kotlin
for (i in 1..100) { ... }  // closed-ended range: includes 100
for (i in 1..<100) { ... } // open-ended range: does not include 100
for (x in 2..10 step 2) { ... }
for (x in 10 downTo 1) { ... }
(1..10).forEach { ... }
```

## Lazy property
延迟属性

```kotlin
val p: String by lazy { // the value is computed only on first access
    // compute the string
}
```

## Extension functions
扩展功能

```kotlin
fun String.spaceToCamelCase() { ... }

"Convert this to camelcase".spaceToCamelCase()
```

## Create a singleton
创建单例

```kotlin
object Resource {
    val name = "Name"
}
```

## Use inline value classes for type-safe values
使用内联值类来实现类型安全的值

```kotlin
@JvmInline
value class EmployeeId(private val id: String)

@JvmInline
value class CustomerId(private val id: String)
```

If you accidentally mix up `EmployeeId` and `CustomerId`, a compilation error is triggered.
<br />如果误将 `EmployeeId` 和 `CustomerId` 混淆，则会触发编译错误。

> The `@JvmInline` annotation is only needed for JVM backends.
> <br />`@JvmInline` 注解仅适用于 JVM 后端。
>
{style="note"}

## Instantiate an abstract class
实例化一个抽象类

```kotlin
abstract class MyAbstractClass {
    abstract fun doSomething()
    abstract fun sleep()
}

fun main() {
    val myObject = object : MyAbstractClass() {
        override fun doSomething() {
            // ...
        }

        override fun sleep() { // ...
        }
    }
    myObject.doSomething()
}
```

## If-not-null shorthand
If-not-null 简写

```kotlin
val files = File("Test").listFiles()

println(files?.size) // size is printed if files is not null
```

## If-not-null-else shorthand
If-not-null-else 简写

```kotlin
val files = File("Test").listFiles()

// For simple fallback values:
println(files?.size ?: "empty") // if files is null, this prints "empty"

// To calculate a more complicated fallback value in a code block, use `run`
val filesSize = files?.size ?: run { 
    val someSize = getSomeSize()
    someSize * 2
}
println(filesSize)
```

## Execute an expression if null
如果为空，则执行表达式

```kotlin
val values = ...
val email = values["email"] ?: throw IllegalStateException("Email is missing!")
```

## Get first item of a possibly empty collection
获取可能为空的集合中的第一个元素

```kotlin
val emails = ... // might be empty
val mainEmail = emails.firstOrNull() ?: ""
```

Learn the difference between [Java and Kotlin first item getting](java-to-kotlin-collections-guide.md#get-the-first-and-the-last-items-of-a-possibly-empty-collection).
<br />了解 [Java 和 Kotlin 获取第一个项目](java-to-kotlin-collections-guide.md#get-the-first-and-the-last-items-of-a-possibly-empty-collection) 之间的区别。

## Execute if not null
如果非空则执行

```kotlin
val value = ...

value?.let {
    ... // execute this block if not null
}
```

## Map nullable value if not null
如果非空，则映射可为空的值

```kotlin
val value = ...

val mapped = value?.let { transformValue(it) } ?: defaultValue 
// defaultValue is returned if the value or the transform result is null.
```

## Return on when statement
返回条件语句

```kotlin
fun transform(color: String): Int {
    return when (color) {
        "Red" -> 0
        "Green" -> 1
        "Blue" -> 2
        else -> throw IllegalArgumentException("Invalid color param value")
    }
}
```

## try-catch expression
try-catch 表达式

```kotlin
fun test() {
    val result = try {
        count()
    } catch (e: ArithmeticException) {
        throw IllegalStateException(e)
    }

    // Working with result
}
```

## if expression
if 表达式

```kotlin
val y = if (x == 1) {
    "one"
} else if (x == 2) {
    "two"
} else {
    "other"
}
```

## Builder-style usage of methods that return Unit
Builder 式使用返回 Unit 的方法

```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

## Single-expression functions
单表达式函数

```kotlin
fun theAnswer() = 42
```

This is equivalent to
<br />这相当于

```kotlin
fun theAnswer(): Int {
    return 42
}
```

This can be effectively combined with other idioms, leading to shorter code. For example, with the `when` expression:
<br />它可以与其他惯用法有效结合，从而编写更简洁的代码。例如，与 `when` 表达式结合使用：

```kotlin
fun transform(color: String): Int = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}
```

## Call multiple methods on an object instance (with)
对对象实例调用多个方法 (with)

```kotlin
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { //draw a 100 pix square
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```

## Configure properties of an object (apply)
配置对象的属性(apply)

```kotlin
val myRectangle = Rectangle().apply {
    length = 4
    breadth = 5
    color = 0xFAFAFA
}
```

This is useful for configuring properties that aren't present in the object constructor.
<br />这对于配置对象构造函数中不存在的属性非常有用。

## Java 7's try-with-resources
Java 7 的 try-with-resources

```kotlin
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}
```

## Generic function that requires the generic type information
需要泛型类型的泛型函数

```kotlin
//  public final class Gson {
//     ...
//     public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
//     ...

inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T = this.fromJson(json, T::class.java)
```

## Swap two variables
交换两个变量

```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

## Mark code as incomplete (TODO)
将代码标记为不完整 (TODO)
 
Kotlin's standard library has a `TODO()` function that will always throw a `NotImplementedError`.
Its return type is `Nothing` so it can be used regardless of expected type.
There's also an overload that accepts a reason parameter:
<br />Kotlin 标准库中有一个 `TODO()` 函数，它总是会抛出一个 `NotImplementedError` 异常。
它的返回类型是 `Nothing`，因此无论预期类型如何，都可以使用它。
此外，它还有一个重载版本，接受一个 reason 参数：

```kotlin
fun calcTaxes(): BigDecimal = TODO("Waiting for feedback from accounting")
```

IntelliJ IDEA's kotlin plugin understands the semantics of `TODO()` and automatically adds a code pointer in the TODO tool window. 
<br />IntelliJ IDEA 的 kotlin 插件能够理解 `TODO()` 的语义，并自动在 TODO 工具窗口中添加代码指针。

## What's next?
接下来

* Solve [Advent of Code puzzles](advent-of-code.md) using the idiomatic Kotlin style.
  <br />使用地道的 Kotlin 风格解决 [Advent of Code 谜题](advent-of-code.md)。
* Learn how to perform [typical tasks with strings in Java and Kotlin](java-to-kotlin-idioms-strings.md).
  <br />学习如何在 Java 和 Kotlin 中执行 [使用字符串的典型任务](java-to-kotlin-idioms-strings.md)。
* Learn how to perform [typical tasks with collections in Java and Kotlin](java-to-kotlin-collections-guide.md).
  <br />学习如何在 [Java 和 Kotlin 中使用集合执行典型任务](java-to-kotlin-collections-guide.md)。
* Learn how to [handle nullability in Java and Kotlin](java-to-kotlin-nullability-guide.md).
  <br />了解如何[在 Java 和 Kotlin 中处理空值](java-to-kotlin-nullability-guide.md)。
