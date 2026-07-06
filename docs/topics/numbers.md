[//]: # (title: Numbers)
[//]: # (description: Learn how to use numbers in Kotlin, including numeric types, literals, conversions, arithmetic operations, overflow, and JVM-specific behavior.)

The Kotlin number types represent:
<br />Kotlin 数字类型代表：
* Integer values ([Byte](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-byte/),
  [Short](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-short/),
  [Int](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-int/),
  and [Long](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-long/))
  <br />整数值（[Byte](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-byte/),
  [Short](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-short/),
  [Int](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-int/),
  以及[Long](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-long/)）
* Floating-point values ([Float](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-float/)
  and [Double](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-double/))
  <br />浮点值（[Float](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-float/)
  和 [Double](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/-double/)）

Use number types to store and process numeric data, for example, in arithmetic, counters, measurements,
and other calculations.
<br />使用数字类型来存储和处理数值数据，例如，在算术运算、计数、测量以及其他计算中。

## Choose a number type
选择数字类型

In most cases, you can refer to the following rules to determine the
correct number type for your task:
<br />大多数情况下，您可以参考以下规则来确定任务所需的正确数字类型：

* Use `Int` for whole numbers.
  <br />整数请使用 `Int` 类型。
* Use `Long` for whole numbers outside the `Int` range.
  <br />对于超出 `Int` 范围的整数，请使用 `Long`。
* Use `Double` for decimal numbers.
  <br />对于小数，请使用 `Double` 类型。
* Use `Float` when lower precision is acceptable or required.
  <br />当可以接受或需要较低的精度时，请使用 `Float`。
* Use `Byte` and `Short` when an API or data format requires them.
  <br />当 API 或数据格式需要时，请使用 `Byte` 和 `Short`。

> Kotlin also provides [](unsigned-integer-types.md) as a Beta feature. 
> <br />Kotlin 还提供了 [](unsigned-integer-types.md) 作为 Beta 功能。
>
{style="tip"}

## Integer types
整数类型

Kotlin provides four integer types with different sizes and value ranges:
<br />Kotlin 提供了四种不同大小和值范围的整数类型：

| Type	    | Size (bits) | Min value                                    | Max value                                      |
|----------|-------------|----------------------------------------------|------------------------------------------------|
| `Byte`	  | 8           | -128                                         | 127                                            |
| `Short`	 | 16          | -32768                                       | 32767                                          |
| `Int`	   | 32          | -2,147,483,648 (-2<sup>31</sup>)             | 2,147,483,647 (2<sup>31</sup> - 1)             |
| `Long`	  | 64          | -9,223,372,036,854,775,808 (-2<sup>63</sup>) | 9,223,372,036,854,775,807 (2<sup>63</sup> - 1) |

| 类型	      | 大小（位） | 最小价值                                         | 最大值                                            |
|----------|-------|----------------------------------------------|------------------------------------------------|
| `Byte`	  | 8     | -128                                         | 127                                            |
| `Short`	 | 16    | -32768                                       | 32767                                          |
| `Int`	   | 32    | -2,147,483,648 (-2<sup>31</sup>)             | 2,147,483,647 (2<sup>31</sup> - 1)             |
| `Long`	  | 64    | -9,223,372,036,854,775,808 (-2<sup>63</sup>) | 9,223,372,036,854,775,807 (2<sup>63</sup> - 1) |


### Declare integer values
声明整数值

Kotlin supports the following literal forms for integer values:
<br />Kotlin 支持以下整数值的字面量形式：

* Decimals: `123`
  <br />小数：`123`
* Hexadecimals: `0x0F`
  <br />十六进制：`0x0F`
* Binaries: `0b00001011`
  <br />二进制：`0b00001011`

> Kotlin does not support octal literals.
> <br />Kotlin 不支持八进制字面量。
>
{style="note"}

To declare a numeric value, specify the type explicitly: 
<br />要声明数值，请显式指定类型：

```kotlin
val one: Int = 1

// Use underscores to improve readability
val oneBillion: Long = 1_000_000_000
val hexBytes: Int = 0x7F_EC_DE_5E
val bytes: Int = 0b01010010_01101001_10010100_10010010

val oneByte: Byte = 1
val oneShort: Short = 1
```

You can also append the `L` suffix, to declare a `Long` value:
<br />您还可以添加 `L` 后缀，以声明一个 `Long` 值：

```kotlin
val oneLong = 1L
```

When you declare a numeric type explicitly, the compiler checks that the value
fits in the range of that type:
<br />当你显式声明一个数值类型时，编译器会检查该值是否符合该类型的范围：

```kotlin
// Value fits in Byte
// 值符合字节限制
val oneByte: Byte = 1

// Error: the value does not fit in Byte
// 错误：该值不符合字节限制。
val tooBig: Byte = 128
```

When you do not specify a numeric type, Kotlin infers `Int` if the
value fits in the `Int` range. Otherwise, Kotlin infers `Long`:
<br />当您未指定数值类型时，如果值在 `Int` 范围内，Kotlin 会推断为 `Int`。否则，Kotlin 会推断为 `Long`。

```kotlin
val million = 1_000_000 // Int
val threeBillion = 3_000_000_000 // Long
```

If a value can be absent, use nullable types:
<br />如果某个值可以为空，请使用可空类型：

```kotlin
val maybeAbsent: Int? = null
```

## Floating-point types
浮点类型

For numbers with a fractional part, Kotlin provides `Float` and `Double`.
<br />对于带有小数部分的数字，Kotlin 提供了 `Float` 和 `Double` 类型。

Floating-point types follow
the [IEEE 754 standard](https://en.wikipedia.org/wiki/IEEE_754).
`Float` reflects the _single precision_. `Double` reflects the _double precision_.
<br />浮点类型遵循[IEEE 754 标准](https://en.wikipedia.org/wiki/IEEE_754)。
`Float` 表示*单精度*浮点数。`Double` 表示双精度浮点数。

Floating-point types differ in size and precision:
<br />浮点数据类型在大小和精度上有所不同：

| Type	    | Size (bits) | Significant bits | Exponent bits | Decimal digits |
|----------|-------------|------------------|---------------|----------------|
| `Float`	 | 32          | 24               | 8             | 6-7            |
| `Double` | 64          | 53               | 11            | 15-16          |    

| 类型	      | 大小（位） | 有效位 | 指数位 | 十进制数字 |
|----------|-------|-----|-----|-------|
| `Float`	 | 32    | 24  | 8   | 6-7   |
| `Double` | 64    | 53  | 11  | 15-16 | 

### Declare floating-point values
声明浮点值

To declare a floating-point literal, include a decimal point (`.`) or use exponent notation:
<br />要声明浮点字面量，请包含小数点（`.`）或使用指数表示法：

```kotlin
val pi = 3.14
val avogadro = 6.02214076e23
```

By default, Kotlin infers floating-point literals as `Double`. 
To declare a `Float`, add the `f` or `F` suffix:
<br />默认情况下，Kotlin 将浮点字面量推断为 `Double`。
要声明为 `Float`，请添加 `f` 或 `F` 后缀：

```kotlin
val pi = 3.14 // Double
val eFloat = 2.7182817f // Float
```

> Kotlin rounds a `Float` literal that contains more precision than `Float` can store.
> <br />Kotlin 会对精度超过 `Float` 字面量存储精度的 `Float` 字面量进行四舍五入。
>
{style="note"}

If a value can be absent, use nullable types:
<br />如果某个值可以为空，请使用可空类型：

```kotlin
val maybeAbsent: Double? = null
```

## Arithmetic operations
算术运算

Kotlin supports the standard arithmetic operations on numbers: `+`, `-`, `*`, `/`, and `%`.
<br />Kotlin 支持对数字进行标准算术运算：`+`、`-`、`*`、`/` 和 `%`。

Use these operators to perform common calculations:
<br />使用以下运算符可执行常用计算：

```kotlin
fun main() {
//sampleStart
    println(1 + 2) // 3
    println(2_500_000_000L - 1L) // 2499999999
    println(3.14 * 2.71) // 8.5094
    println(10.0 / 3) // 3.3333333333333335
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

The result type depends on the types of the operands. Learn more in [](#mixed-numeric-expressions).
<br />结果类型取决于操作数的类型。了解更多信息，请参阅 [](#mixed-numeric-expressions)。

> You can override these operators in custom number classes.
> For more information, see [Operator overloading](operator-overloading.md).
> <br />您可以在自定义数字类中重写这些运算符。
> 更多信息，请参阅[运算符重载](operator-overloading.md)。
>
{style="tip"}

### Integer division
整数除法

Division between integer values always returns an integer result. The compiler discards the fractional part:
<br />整数除法总是返回整数结果。编译器会丢弃小数部分：

```kotlin
fun main() {
//sampleStart
    val intValue = 5 / 2
    println(intValue) // 2
    
    val longValue = 5L / 2
    println(longValue) // 2
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

To return a floating-point result, make at least one operand a `Float` or `Double`:
<br />要返回浮点结果，至少要将一个操作数设为 `Float` 或 `Double` 类型：

```kotlin
fun main() {
//sampleStart
    val a = 5 / 2.0
    println(a) // 2.5
    
    val b = 5 / 2.toDouble()
    println(b) // 2.5
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

## Type conversion
类型转换

Numeric types are not subtypes of one another. Kotlin requires explicit
conversions to avoid silent data loss and unexpected behavior.
<br />数值类型之间并非互为子类型。Kotlin 要求显式进行类型转换，以避免数据丢失和意外行为。

For example, a function that expects `Double` cannot accept an `Int` or a `Float` value without conversion:
<br />例如，一个期望接收 `Double` 类型值的函数不能接受 `Int` 或 `Float` 类型的值，必须进行转换：

```kotlin
fun main() {
//sampleStart
    fun printDouble(x: Double) { 
        print(x) 
    }

    val x = 1.0
    val xInt = 1
    val xFloat = 1.0f
    val one: Double = 1 // Error: initializer type mismatch

    printDouble(x) // OK
    printDouble(xInt) // Error: argument type mismatch
    printDouble(xFloat) // Error: argument type mismatch
//sampleEnd
}
```
{kotlin-runnable="true" validate="false"}

All number types support conversions to other number types.
To convert a number to another type, use an explicit conversion function:
<br />所有数字类型都支持与其他数字类型的转换。
要将数字转换为另一种类型，请使用显式转换函数：

* `toByte()`
* `toShort()`
* `toInt()`
* `toLong()`
* `toFloat()`
* `toDouble()`

For example, the following code converts an `Int` value to `Double`:
<br />例如，以下代码将 `Int` 值转换为 `Double` 值：

```kotlin
fun main() {
//sampleStart
    val intValue: Int = 1
    val doubleValue = intValue.toDouble()
    
    println(doubleValue) // 1.0
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

When you convert a floating-point value to an integer type, the compiler discards the fractional part:
<br />将浮点值转换为整数类型时，编译器会丢弃小数部分：

```kotlin
fun main() {
//sampleStart
    val d: Double = 1.5
    val l: Long = d.toLong()
    
    println(l) // 1
//sampleEnd    
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

### Mixed numeric expressions
混合数值表达式

Kotlin does not support implicit conversion for assignments or function arguments. 
However, you can combine different numeric types in arithmetic expressions. In such cases, 
Kotlin determines a result type based on the operand types, 
and arithmetic operators handle the conversion automatically:
<br />Kotlin 不支持赋值或函数参数的隐式转换。
但是，您可以在算术表达式中组合不同的数值类型。在这种情况下，Kotlin 会根据操作数类型确定结果类型，并且算术运算符会自动处理类型转换：

```kotlin
val intNumber: Int = 1
val longNumber: Long = 1000
val result = intNumber + longNumber // 1001, Long
```

If you try to assign the result to a smaller type, the compiler reports an error:
<br />如果尝试将结果赋值给一个较小的类型，编译器会报错：

```kotlin
val intNumber: Int = 1
val longNumber: Long = 1000
val result: Int = intNumber + longNumber 
// Error: Initializer type mismatch
// 错误：初始化器类型不匹配
```

### Integer literal types
整数字面量类型

During type inference, Kotlin treats unsuffixed integer literals as a special [Integer Literal Type (ILT)](https://kotlinlang.org/spec/type-system.html#integer-literal-types)
until the surrounding context determines a specific type:
<br />在类型推断过程中，Kotlin 会将不带后缀的整数字面量视为特殊的[整数字面量类型 (ILT)](https://kotlinlang.org/spec/type-system.html#integer-literal-types)
直到上下文确定了具体类型为止：

```kotlin
//sampleStart
fun List<Any>.log() {
    println(joinToString(" | ") { it::class.simpleName ?: "Unknown" })
}

fun main() {
    listOf(1, 2).log()
    // Int | Int
    
    listOf(1L, 2L).log()
    // Long | Long
    
    // Compiler interprets 1 as an ILT and resolves it to Long
    listOf(1, 2L).log()
    // Long | Long
    
    // .toInt() converts the literal to Int
    listOf(1.toInt(), 2L).log()
    // Int | Long
}
//sampleEnd
```
{kotlin-runnable="true"}

It's especially easy to miss with the `Int` and `Long` values because they have the same string representation
at runtime. To avoid this, specify the expected type or convert values explicitly:
<br />`Int` 和 `Long` 值尤其容易出错，因为它们在运行时具有相同的字符串表示形式。
为避免这种情况，请指定预期类型或显式转换值：

```kotlin
//sampleStart
fun List<Any>.log() {
    println(joinToString(" | ") { it::class.simpleName ?: "Unknown" })
}

fun main() {
    val longValues: List<Long> = listOf(1, 2L)
    longValues.log()
    // Long | Long

    val numberValues: List<Number> = listOf(1.toInt(), 2L)
    numberValues.log()
    // Int | Long
}
//sampleEnd
```
{kotlin-runnable="true"}

You can also use an explicit type to catch unintended type inference:
<br />您还可以使用显式类型来捕获意外的类型推断：

```kotlin
fun main() {
//sampleStart
    val intValues: List<Int> = listOf(1, 2L)
    // Error: initializer type mismatch
//sampleEnd
}
```
{kotlin-runnable="true" validate="false"}

> Learn more about [Integer literal types](https://kotlinlang.org/spec/type-system.html#integer-literal-types).
> <br />了解更多关于[整数字面量类型](https://kotlinlang.org/spec/type-system.html#integer-literal-types)。
> 
{style="tip"}

## Data overflow
数据溢出

Numeric types can represent only values within their defined ranges.
<br />数值类型只能表示其定义范围内的值。

If the result of an operation falls outside that range, overflow occurs. 
If you convert a value to a smaller numeric type, the converted value may not preserve 
the original numeric value.
<br />如果运算结果超出该范围，则会发生溢出。
如果将值转换为较小的数值类型，转换后的值可能无法保留原始数值。

This behavior can affect the result of your code even when the compiler accepts it.
<br />即使编译器接受这种行为，它也可能影响代码的运行结果。

### Overflow in operations
操作溢出

Each integer type can store only values within its defined range. When the result of an
arithmetic operation exceeds that range, _data overflow_ occurs:
<br />每种整数类型只能存储其定义范围内的值。
当算术运算的结果超出该范围时，就会发生*数据溢出*：

```kotlin
fun main(){
//sampleStart
    val intNumber: Int = 2147483647
    // Max Int value is 2147483647
    println(intNumber + 1) // -2147483648
//sampleEnd    
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

Here, the result wraps around because the value no longer fits in `Int`.
<br />这里，结果会循环，因为该值不再适合 `Int`。

> The compiler does not automatically produce an error when integer overflow occurs.
> <br />当发生整数溢出时，编译器不会自动报错。
>
{style="note"}

### Overflow in negation
否定溢出

Overflow can also occur during negation. 
For example, you cannot represent the positive counterpart of `Int.MIN_VALUE` as an `Int`.
<br />取反运算过程中也可能发生溢出。
例如，您不能将 `Int.MIN_VALUE` 的正值表示为 `Int` 类型。

```kotlin
fun main(){
//sampleStart
    val min = Int.MIN_VALUE
    println(-min) // -2147483648
//sampleEnd    
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

### Narrowing conversions
缩小转换范围

When you convert a value to a smaller integer type, 
the result may not preserve the original numeric value:
<br />当您将一个值转换为较小的整数类型时，
结果可能无法保留原始数值：

```kotlin
fun main() {
//sampleStart
    val large: Int = 130
    val narrowed: Byte = large.toByte()

    println(narrowed) // -126
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

However, since floating-point types follow the
[IEEE 754 Standard](https://en.wikipedia.org/wiki/IEEE_754), very large results can become `Infinity`:
<br />然而，由于浮点类型遵循
[IEEE 754 标准](https://en.wikipedia.org/wiki/IEEE_754)，非常大的结果可能会变成“无穷大”：

```kotlin
fun main() {
//sampleStart
    println(Double.MAX_VALUE * 2) // Infinity
//sampleEnd    
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

## Bitwise operations
位运算

Kotlin provides _bitwise operations_ for `Int` and `Long`. These operations are represented by
a set of [infix functions](functions.md#infix-notation) and `inv()`.
<br />Kotlin 为 `Int` 和 `Long` 提供了位运算。这些运算由以下函数表示：
一组[中缀函数](functions.md#infix-notation)和`inv()`。

```kotlin
fun main() {
//sampleStart
    val x = 1
    
    println(x shl 2) // 4
    println(x and 0x000FF000) // 0
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

Bitwise operations include:
<br />位运算包括：

* `shl()` – signed shift left
  <br />`shl()` – 有符号左移
* `shr()` – signed shift right
  <br />`shr()` – 右移符号
* `ushr()` – unsigned shift right
  <br />`ushr()` – 无符号右移
* `and()` – bitwise AND
  <br />`and()` – 按位与
* `or()` – bitwise OR
  <br />`or()` – 按位或
* `xor()` – bitwise XOR
  <br />`xor()` – 按位异或
* `inv()` – bitwise inversion
  <br />`inv()` – 按位反转

## Floating-point number comparison
浮点数比较

In Kotlin, floating-point comparison depends on the static type of the operands.
<br />在 Kotlin 中，浮点数比较取决于操作数的静态类型。

When the operands are statically known to be `Float` or `Double`,
operations on the numbers and the range that they form
follow the [IEEE 754 Standard for Floating-Point Arithmetic](https://en.wikipedia.org/wiki/IEEE_754).
<br />当操作数被静态地确定为“浮点数”或“双精度浮点数”时，
对这些数字及其构成范围的运算遵循[IEEE 754 浮点运算标准](https://en.wikipedia.org/wiki/IEEE_754)。

However, in generic use cases (such as `Any`, `Comparable<...>`, or `Collection<T>`), behavior differs for
operands that are not statically typed as floating-point numbers. In these cases, Kotlin
uses the `equals()` and `compareTo()` implementations for `Float` and `Double`. 
<br />然而，在通用用例（例如 `Any`、`Comparable<...>` 或 `Collection<T>`）中，对于非静态类型为浮点数的操作数，其行为有所不同。
在这种情况下，Kotlin 使用 `Float` 和 `Double` 的 `equals()` 和 `compareTo()` 实现。

As a result:
<br />因此：

* `NaN` is considered equal to itself
  <br />`NaN` 被视为与其自身相等。
* `NaN` is considered greater than any other element including `POSITIVE_INFINITY`
  <br />`NaN` 被认为大于任何其他元素，包括 `POSITIVE_INFINITY`。
* `-0.0` is considered less than `0.0`
  <br />`-0.0` 被认为小于 `0.0`

The following example shows the difference between operands statically typed as floating-point numbers
and operands used through generic types:
<br />以下示例展示了静态类型为浮点数的操作数与通过泛型类型使用的操作数之间的区别：

```kotlin
//sampleStart  
fun generalizedEquals(a: Any, b: Any): Boolean {
    return a == b
}

fun main() {
    // Operands statically typed as floating-point numbers
    println(Double.NaN == Double.NaN) // false
    println(0.0 == -0.0) // true

    // Operands used through a non-floating-point static type
    println(generalizedEquals(Double.NaN, Double.NaN)) // true
    println(generalizedEquals(0.0, -0.0)) // false
}
//sampleEnd  
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-numbers-floating-comp"}

## Boxing and caching numbers on the JVM
JVM上的装箱和缓存数字

On the JVM, non-nullable numeric values are usually stored using primitive types, such as `int`, `long`, or `double`.
However, when you use [generic types](generics.md) or nullable numeric types like `Int?`, the value is boxed and
represented as an object.
<br />在 JVM 中，非空数值通常使用基本类型（例如 `int`、`long` 或 `double`）存储。
但是，当您使用[泛型类型](generics.md)或可空数值类型（例如 `Int?`）时，该值会被装箱并表示为一个对象。

The JVM applies a [memory optimization technique](https://docs.oracle.com/javase/specs/jls/se22/html/jls-5.html#jls-5.1.7)
to small numbers by caching their boxed representations. As a result,
boxed numbers with the same value can be [referentially equal](equality.md#referential-equality).
<br />JVM 对小数字应用了一种[内存优化技术](https://docs.oracle.com/javase/specs/jls/se22/html/jls-5.html#jls-5.1.7)，
通过缓存它们的装箱表示来实现。
因此，具有相同值的装箱数字可以[引用相等](equality.md#referential-equality)。

For example, the JVM caches boxed `Integer` values in the range `-128` to `127`. Therefore, the following
code returns `true`:
<br />例如，JVM 会缓存范围在 -128 到 127 之间的装箱 `Integer` 值。因此，以下代码会返回 `true`：

```kotlin
fun main() {
//sampleStart
    val score: Int = 100
    val savedScore: Int? = score
    val displayedScore: Int? = score
    
    println(savedScore === displayedScore) // true
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" validate="false"}

For values outside the cached range, boxed values are separate objects. In that case,
they are not referentially equal, even if their values are [structurally equal](equality.md#structural-equality).
For this reason, use `==` to compare numeric values:
<br />对于缓存范围之外的值，带框的值是独立的对象。在这种情况下，
即使它们的值在[结构上相等](equality.md#structural-equality)，它们在引用上也不相等。
因此，请使用 `==` 来比较数值：

```kotlin
fun main() {
//sampleStart
    val score: Int = 10000
    val savedScore: Int? = score
    val displayedScore: Int? = score

    println(savedScore === displayedScore) // false
    println(savedScore == displayedScore) // true
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" validate="false"}