[//]: # (title: Basic types)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2.svg" width="20" alt="Second step" /> <strong>Basic types</strong><br />
        <img src="icon-3-todo.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

Every variable and data structure in Kotlin has a type. Types are important because they tell the compiler what you are allowed to 
do with that variable or data structure. In other words, what functions and properties it has.
<br />Kotlin 中的每个变量和数据结构都有一个类型。类型非常重要，因为它们告诉编译器你可以对该变量或数据结构执行哪些操作。换句话说，它们定义了该变量或数据结构有哪些函数和属性。

In the last chapter, Kotlin was able to tell in the previous example that `customers` has type [`Int`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int/).
Kotlin's ability to **infer** the type is called **type inference**. `customers` is assigned an integer
value. From this, Kotlin infers that `customers` has a numerical type `Int`. As a result, the compiler knows that you
can perform arithmetic operations with `customers`:
<br />在上一章中，Kotlin 在前面的示例中能够判断出 `customers` 的类型为 [`Int`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int/)。
Kotlin 的这种类型**推断**能力被称为**类型推断**。`customers` 被赋予了一个整数值。
由此，Kotlin 推断出 `customers` 的类型为数值类型 `Int`。因此，编译器知道你可以对 `customers` 执行算术运算：

```kotlin
fun main() {
//sampleStart
    var customers = 10

    // Some customers leave the queue
    customers = 8

    customers = customers + 3 // Example of addition: 11
    customers += 7            // Example of addition: 18
    customers -= 3            // Example of subtraction: 15
    customers *= 2            // Example of multiplication: 30
    customers /= 3            // Example of division: 10

    println(customers) // 10
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-basic-types-arithmetic"}

> `+=`, `-=`, `*=`, `/=`, and `%=` are augmented assignment operators. For more information, see [Augmented assignments](operator-overloading.md#augmented-assignments).
> <br />`+=`、`-=`、`*=`、`/=` 和 `%=` 是增强赋值运算符。有关更多信息，请参阅[增强赋值](operator-overloading.md#augmented-assignments)。
> 
{style="tip"}

In total, Kotlin has the following basic types:
<br />Kotlin 共有以下几种基本类型：

| **Category**           | **Basic types**                    | **Example code**                                              |
|------------------------|------------------------------------|---------------------------------------------------------------|
| Integers               | `Byte`, `Short`, `Int`, `Long`     | `val year: Int = 2020`                                        |
| Unsigned integers      | `UByte`, `UShort`, `UInt`, `ULong` | `val score: UInt = 100u`                                      |
| Floating-point numbers | `Float`, `Double`                  | `val currentTemp: Float = 24.5f`, `val price: Double = 19.99` |
| Booleans               | `Boolean`                          | `val isEnabled: Boolean = true`                               |
| Characters             | `Char`                             | `val separator: Char = ','`                                   |
| Strings                | `String`                           | `val message: String = "Hello, world!"`                       |

| **类别**                 | **基本类型**                           | **示例代码**                                                      |
|------------------------|------------------------------------|---------------------------------------------------------------|
| Integers               | `Byte`, `Short`, `Int`, `Long`     | `val year: Int = 2020`                                        |
| Unsigned integers      | `UByte`, `UShort`, `UInt`, `ULong` | `val score: UInt = 100u`                                      |
| Floating-point numbers | `Float`, `Double`                  | `val currentTemp: Float = 24.5f`, `val price: Double = 19.99` |
| Booleans               | `Boolean`                          | `val isEnabled: Boolean = true`                               |
| Characters             | `Char`                             | `val separator: Char = ','`                                   |
| Strings                | `String`                           | `val message: String = "Hello, world!"`                       |

For more information on basic types and their properties, see [Types overview](types-overview.md).
<br />有关基本类型及其属性的更多信息，请参阅[类型概述](types-overview.md)。

With this knowledge, you can declare variables and initialize them later. Kotlin can manage this as long as variables
are initialized before the first read.
<br />有了这些知识，你就可以声明变量并在之后初始化它们。只要变量在首次读取之前初始化，Kotlin 就能处理这种情况。

To declare a variable without initializing it, specify its type with `:`. For example:
<br />要声明一个变量而不对其进行初始化，请使用 `:` 指定其类型。例如：

```kotlin
fun main() {
//sampleStart
    // Variable declared without initialization
    val d: Int
    // Variable initialized
    d = 3

    // Variable explicitly typed and initialized
    val e: String = "hello"

    // Variables can be read because they have been initialized
    println(d) // 3
    println(e) // hello
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-basic-types-initialization"}

If you don't initialize a variable before it is read, you see an error:
<br />如果在读取变量之前没有对其进行初始化，则会看到错误：

```kotlin
fun main() {
//sampleStart
    // Variable declared without initialization
    val d: Int
    
    // Triggers an error
    println(d)
    // Variable 'd' must be initialized
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-basic-types-no-initialization" validate="false"}

Now that you know how to declare basic types, it's time to learn about [collections](kotlin-tour-collections.md).
<br />既然你已经知道如何声明基本类型，现在是时候学习[集合](kotlin-tour-collections.md)了。

## Practice
实践

### Exercise {initial-collapse-state="collapsed" collapsible="true"}

Explicitly declare the correct type for each variable:
<br />明确声明每个变量的正确类型：

|---|---|
```kotlin
fun main() {
    val a: Int = 1000 
    val b = "log message"
    val c = 3.14
    val d = 100_000_000_000_000
    val e = false
    val f = '\n'
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-basic-types-exercise"}

|---|---|
```kotlin
fun main() {
    val a: Int = 1000
    val b: String = "log message"
    val c: Double = 3.14
    val d: Long = 100_000_000_000_000
    val e: Boolean = false
    val f: Char = '\n'
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-basic-types-solution"}

## Next step
接下来

[Collections](kotlin-tour-collections.md)
<br />[集合](kotlin-tour-collections.md)

