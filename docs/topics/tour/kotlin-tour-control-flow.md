[//]: # (title: Control flow)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-collections.md">Collections</a><br />
        <img src="icon-4.svg" width="20" alt="Fourth step" /> <strong>Control flow</strong><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

Like other programming languages, Kotlin is capable of making decisions based on whether a piece of code is evaluated to
be true. Such pieces of code are called **conditional expressions**. Kotlin is also able to create and iterate
through loops.
<br />与其他编程语言一样，Kotlin 能够根据代码的执行结果（是否为真）来做出决策。这样的代码片段被称为**条件表达式**。Kotlin 也能够创建和遍历循环。

## Conditional expressions
条件表达式

Kotlin provides `if` and `when` for checking conditional expressions. 
<br />Kotlin 提供了 `if` 和 `when` 来检查条件表达式。

> If you have to choose between `if` and `when`, we recommend using `when` because it:
> <br />如果必须在 `if` 和 `when` 之间选择，我们建议使用 `when`，因为：
> 
> * Makes your code easier to read.
> <br />让你的代码更易于阅读。
> * Makes it easier to add another branch.
> <br />使添加另一个分支更容易。
> * Leads to fewer mistakes in your code.
> <br />从而减少代码中的错误。
> 
{style="note"}

### If

To use `if`, add the conditional expression within parentheses `()` and the action to take if the result is true within 
curly braces `{}`:
<br />要使用 `if` 语句，请将条件表达式放在圆括号 `()` 内，并将结果为真时要执行的操作放在花括号 `{}` 内：

```kotlin
fun main() {
//sampleStart
    val d: Int
    val check = true

    if (check) {
        d = 1
    } else {
        d = 2
    }

    println(d)
    // 1
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-if"}

There is no ternary operator `condition ? then : else` in Kotlin. Instead, `if` can be used as an expression. If there is
only one line of code per action, the curly braces `{}` are optional:
<br />Kotlin 中没有三元运算符 `condition ? then : else`。取而代之的是，可以使用 `if` 表达式。如果每个操作只有一行代码，则花括号 `{}` 是可选的。

```kotlin
fun main() { 
//sampleStart
    val a = 1
    val b = 2

    println(if (a > b) a else b) // Returns a value: 2
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-if-expression"}

### When

Use `when` when you have a conditional expression with multiple branches.
<br />当条件表达式包含多个分支时，请使用 `when`。

To use `when`:
<br />使用 `when` 时：

* Place the value you want to evaluate within parentheses `()`.
  <br />将要评估的值放在括号 `()` 内。
* Place the branches within curly braces `{}`.
  <br />将分支放在花括号 `{}` 内。
* Use `->` in each branch to separate each check from the action to take if the check is successful.
  <br />在每个分支中使用 `->` 将每个检查与检查成功后要执行的操作分隔开。

`when` can be used either as a statement or as an expression. A **statement** doesn't return anything but performs actions
instead.
<br />`when` 既可以用作语句，也可以用作表达式。**语句** 不返回任何值，而是执行操作。

Here is an example of using `when` as a statement:
<br />以下是使用 `when` 作为语句的示例：

```kotlin
fun main() {
//sampleStart
    val obj = "Hello"

    when (obj) {
        // Checks whether obj equals to "1"
        "1" -> println("One")
        // Checks whether obj equals to "Hello"
        "Hello" -> println("Greeting")
        // Default statement
        else -> println("Unknown")     
    }
    // Greeting
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-when-statement"}

> Note that all branch conditions are checked sequentially until one of them is satisfied. So only the first suitable 
> branch is executed.
> <br />请注意，所有分支条件都会按顺序检查，直到满足其中一个条件为止。因此，只会执行第一个合适的分支。
>
{style="note"}

An **expression** returns a value that can be used later in your code.
<br />**表达式**会返回一个值，该值可以在代码的后续部分中使用。

Here is an example of using `when` as an expression. The `when` expression is assigned immediately to a variable which is
later used with the `println()` function:
<br />以下是使用 `when` 作为表达式的示例。`when` 表达式会立即赋值给一个变量，该变量稍后将与 `println()` 函数一起使用：

```kotlin
fun main() {
//sampleStart    
    val obj = "Hello"    
    
    val result = when (obj) {
        // If obj equals "1", sets result to "one"
        "1" -> "One"
        // If obj equals "Hello", sets result to "Greeting"
        "Hello" -> "Greeting"
        // Sets result to "Unknown" if no previous condition is satisfied
        else -> "Unknown"
    }
    println(result)
    // Greeting
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-when-expression"}

The examples of `when` that you've seen so far both had a subject: `obj`. But `when` can also be used without a subject.
<br />你目前看到的`when`例句都带有主语`obj`。但`when`也可以不带主语。

This example uses a `when` expression **without** a subject to check a chain of Boolean expressions:
<br />此示例使用**不带**主语的 `when` 表达式来检查一系列布尔表达式：

```kotlin
fun main() {
    val trafficLightState = "Red" // This can be "Green", "Yellow", or "Red"

    val trafficAction = when {
        trafficLightState == "Green" -> "Go"
        trafficLightState == "Yellow" -> "Slow down"
        trafficLightState == "Red" -> "Stop"
        else -> "Malfunction"
    }

    println(trafficAction)
    // Stop
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-when-expression-boolean"}

However, you can have the same code but with `trafficLightState` as the subject:
<br />但是，您可以使用相同的代码，但将主题改为 `trafficLightState`：

```kotlin
fun main() {
    val trafficLightState = "Red" // This can be "Green", "Yellow", or "Red"

    val trafficAction = when (trafficLightState) {
        "Green" -> "Go"
        "Yellow" -> "Slow down"
        "Red" -> "Stop"
        else -> "Malfunction"
    }

    println(trafficAction)  
    // Stop
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-when-expression-boolean-subject"}

Using `when` with a subject makes your code easier to read and maintain. When you use a subject with a `when` expression, 
it also helps Kotlin check that all possible cases are covered. Otherwise, if you don't use a subject with a 
`when` expression, you need to provide an else branch.
<br />在 `when` 语句中使用 subject 可以提高代码的可读性和可维护性。
此外，当 subject 与 `when` 表达式一起使用时，Kotlin 还能帮助检查所有可能的情况是否都被覆盖。
否则，如果不在 `when` 表达式中使用 subject，则需要提供一个 else 分支。

## Conditional expressions practice
条件表达式练习

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="conditional-expressions-exercise-1"}

Create a simple game where you win if throwing two dice results in the same number. Use `if` to print `You win :)`
if the dice match or `You lose :(` otherwise.
<br />创建一个简单的游戏，掷两个骰子点数相同则获胜。使用 `if` 语句，如果骰子点数相同则输出 `You win :)`，否则输出 `You lose :(`。

> In this exercise, you import a package so that you can use the `Random.nextInt()` function to give you a random `Int`.
> For more information about importing packages, see [Packages and imports](packages.md).
> <br />在本练习中，您将导入一个包，以便可以使用 `Random.nextInt()` 函数生成一个随机的 `Int` 值。有关导入包的更多信息，请参阅[包和导入](packages.md)。
>
{style="tip"}

<deflist collapsible="true">
    <def title="Hint">
        Use the <a href="operator-overloading.md#equality-and-inequality-operators">equality operator</a> (<code>==</code>) to compare the dice results. 
        <br />使用 <a href="operator-overloading.md#equality-and-inequality-operators">相等运算符</a> (<code>==</code>) 比较骰子结果。
    </def>
</deflist>

|---|---|
```kotlin
import kotlin.random.Random

fun main() {
    val firstResult = Random.nextInt(6)
    val secondResult = Random.nextInt(6)
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-control-flow-conditional-exercise-1"}

|---|---|
```kotlin
import kotlin.random.Random

fun main() {
    val firstResult = Random.nextInt(6)
    val secondResult = Random.nextInt(6)
    if (firstResult == secondResult)
        println("You win :)")
    else
        println("You lose :(")
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-control-flow-conditional-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="conditional-expressions-exercise-2"}

Using a `when` expression, update the following program so that it prints the corresponding actions when you input the 
names of game console buttons.
<br />使用 `when` 表达式，更新以下程序，使其在输入游戏机按钮名称时打印相应的操作。

| **Button** | **Action**             |
|------------|------------------------|
| A          | Yes                    |
| B          | No                     |
| X          | Menu                   |
| Y          | Nothing                |
| Other      | There is no such button |

| **按钮** | **操作**  |
|--------|---------|
| A      | Yes     |
| B      | No      |
| X      | 菜单      |
| Y      | 无       |
| 其他     | 没有这样的按钮 |

|---|---|
```kotlin
fun main() {
    val button = "A"

    println(
        // Write your code here
    )
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-control-flow-conditional-exercise-2"}

|---|---|
```kotlin
fun main() {
    val button = "A"
    
    println(
        when (button) {
            "A" -> "Yes"
            "B" -> "No"
            "X" -> "Menu"
            "Y" -> "Nothing"
            else -> "There is no such button"
        }
    )
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-control-flow-conditional-solution-2"}

## Ranges
范围

Before talking about loops, it's useful to know how to construct ranges for loops to iterate over.
<br />在讨论循环之前，了解如何构建循环要迭代的范围是很有用的。

The most common way to create a range in Kotlin is to use the `..` operator. For example, `1..4` is equivalent to `1, 2, 3, 4`.
<br />在 Kotlin 中，创建范围最常用的方法是使用 `..` 运算符。例如，`1..4` 等价于 `1, 2, 3, 4`。

To declare a range that doesn't include the end value, use the `..<` operator. For example, `1..<4` is equivalent to `1, 2, 3`.
<br />要声明一个不包含结束值的范围，请使用 `..<` 运算符。例如，`1..<4` 等价于 `1, 2, 3`。

To declare a range in reverse order, use [`downTo`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/down-to.html). For example, `4 downTo 1` is equivalent to `4, 3, 2, 1`.
<br />要声明一个反向顺序的范围，请使用 [`downTo`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/down-to.html) 函数。例如，`4 downTo 1` 等价于 `4, 3, 2, 1`。

To declare a range that increments in a step that isn't 1, use [`step`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/step.html) and your desired increment value.
For example, `1..5 step 2` is equivalent to `1, 3, 5`.
<br />要声明一个步长不为 1 的增量范围，请使用 [`step`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/step.html)函数并指定所需的增量值。
例如，`1..5 step 2` 等价于 `1, 3, 5`。

You can also do the same with `Char` ranges:
您也可以对 `Char` 范围执行相同的操作：

* `'a'..'d'` is equivalent to `'a', 'b', 'c', 'd'`
  <br />`'a'..'d'` 等价于 `'a', 'b', 'c', 'd'`
* `'z' downTo 's' step 2` is equivalent to `'z', 'x', 'v', 't'`
  <br />`'z' downTo 's' step 2` 等价于 `'z', 'x', 'v', 't'`

## Loops
循环

The two most common loop structures in programming are `for` and `while`. Use `for` to iterate over a range of 
values and perform an action. Use `while` to continue an action until a particular condition is satisfied.
<br />编程中最常见的两种循环结构是 `for` 和 `while`。使用 `for` 循环遍历一系列值并执行操作。使用 `while` 循环持续执行操作，直到满足特定条件为止。

### For

Using your new knowledge of ranges, you can create a `for` loop that iterates over numbers 1 to 5 and prints the number 
each time.
<br />利用你新掌握的范围知识，你可以创建一个 `for` 循环，遍历数字 1 到 5，并每次打印出对应的数字。

Place the iterator and range within parentheses `()` with keyword `in`. Add the action you want to complete within curly
braces `{}`:
<br />将迭代器和范围放在括号 `()` 内，并使用关键字 `in`。将要执行的操作放在花括号 `{}` 内：

```kotlin
fun main() {
//sampleStart
    for (number in 1..5) { 
        // number is the iterator and 1..5 is the range
        print(number)
    }
    // 12345
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-for-loop"}

Collections can also be iterated over by loops:
<br />也可以使用循环遍历集合：

```kotlin
fun main() { 
//sampleStart
    val cakes = listOf("carrot", "cheese", "chocolate")

    for (cake in cakes) {
        println("Yummy, it's a $cake cake!")
    }
    // Yummy, it's a carrot cake!
    // Yummy, it's a cheese cake!
    // Yummy, it's a chocolate cake!
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-for-collection-loop"}

### While

`while` can be used in two ways:
<br />`while` 语句有两种用法：

  * To execute a code block while a conditional expression is true. (`while`)
    <br />当条件表达式为真时执行代码块。（`while`）
  * To execute the code block first and then check the conditional expression. (`do-while`)
    <br />先执行代码块，然后再检查条件表达式（`do-while`）。

In the first use case (`while`):
<br />在第一个使用场景（`while`）中：

* Declare the conditional expression for your while loop to continue within parentheses `()`. 
  <br />在括号 `()` 内声明 while 循环继续的条件表达式。
* Add the action you want to complete within curly braces `{}`.
  <br />在花括号 `{}` 内添加要执行的操作。

> The following examples use the [increment operator](operator-overloading.md#increments-and-decrements) `++` to
> increment the value of the `cakesEaten` variable.
> <br />以下示例使用 [递增运算符](operator-overloading.md#increments-and-decrements) `++` 来递增 `cakesEaten` 变量的值。
>
{style="tip"}

```kotlin
fun main() {
//sampleStart
    var cakesEaten = 0
    while (cakesEaten < 3) {
        println("Eat a cake")
        cakesEaten++
    }
    // Eat a cake
    // Eat a cake
    // Eat a cake
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-while-loop"}

In the second use case (`do-while`):
<br />在第二个用例（`do-while`）中：

* Declare the conditional expression for your while loop to continue within parentheses `()`.
  <br />在括号 `()` 内声明 while 循环继续的条件表达式。
* Define the action you want to complete within curly braces `{}` with the keyword `do`.
  <br />用关键字 `do` 在花括号 `{}` 内定义要完成的操作。

```kotlin
fun main() {
//sampleStart
    var cakesEaten = 0
    var cakesBaked = 0
    while (cakesEaten < 3) {
        println("Eat a cake")
        cakesEaten++
    }
    do {
        println("Bake a cake")
        cakesBaked++
    } while (cakesBaked < cakesEaten)
    // Eat a cake
    // Eat a cake
    // Eat a cake
    // Bake a cake
    // Bake a cake
    // Bake a cake
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-while-do-loop"}

For more information and examples of conditional expressions and loops, see [Conditions and loops](control-flow.md).
<br />有关条件表达式和循环的更多信息和示例，请参阅[条件和循环](control-flow.md)。

Now that you know the fundamentals of Kotlin control flow, it's time to learn how to write your own [functions](kotlin-tour-functions.md).
<br />现在你已经了解了 Kotlin 控制流的基础知识，是时候学习如何编写自己的[函数](kotlin-tour-functions.md)。

## Loops practice
循环练习

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="loops-exercise-1"}

You have a program that counts pizza slices until there's a whole pizza with 8 slices. Refactor this program in two ways:
<br />你有一个程序，它会数披萨片，直到数完一个完整的披萨（共 8 片）。请用两种方式重构这个程序：

* Use a `while` loop.
  <br />使用`while`循环。
* Use a `do-while` loop.
  <br />使用 `do-while` 循环。

|---|---|
```kotlin
fun main() {
    var pizzaSlices = 0
    // Start refactoring here
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    println("There's only $pizzaSlices slice/s of pizza :(")
    pizzaSlices++
    // End refactoring here
    println("There are $pizzaSlices slices of pizza. Hooray! We have a whole pizza! :D")
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-control-flow-loops-exercise-1"}

|---|---|
```kotlin
fun main() {
    var pizzaSlices = 0
    while ( pizzaSlices < 7 ) {
        pizzaSlices++
        println("There's only $pizzaSlices slice/s of pizza :(")
    }
    pizzaSlices++
    println("There are $pizzaSlices slices of pizza. Hooray! We have a whole pizza! :D")
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 1" id="kotlin-tour-control-flow-loops-exercise-1-solution-1"}

|---|---|
```kotlin
fun main() {
    var pizzaSlices = 0
    pizzaSlices++
    do {
        println("There's only $pizzaSlices slice/s of pizza :(")
        pizzaSlices++
    } while ( pizzaSlices < 8 )
    println("There are $pizzaSlices slices of pizza. Hooray! We have a whole pizza! :D")
}

```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 2" id="kotlin-tour-control-flow-loops-exercise-1-solution-2"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="loops-exercise-2"}

Write a program that simulates the [Fizz buzz](https://en.wikipedia.org/wiki/Fizz_buzz) game. Your task is to print 
numbers from 1 to 100 incrementally, replacing any number divisible by three with the word "fizz", and any number 
divisible by five with the word "buzz". Any number divisible by both 3 and 5 must be replaced with the word "fizzbuzz".
<br />编写一个程序来模拟 [Fizz Buzz](https://en.wikipedia.org/wiki/Fizz_buzz) 游戏。
你的任务是按顺序打印从 1 到 100 的数字，并将任何能被 3 整除的数字替换为单词“fizz”，将任何能被 5 整除的数字替换为单词“buzz”。
任何既能被 3 又能被 5 整除的数字都必须替换为单词“fizzbuzz”。

<deflist collapsible="true">
    <def title="Hint 1">
        Use a <code>for</code> loop to count numbers and a <code>when</code> expression to decide what to print at each
        step. 
        <br />使用 <code>for</code> 循环来统计数字，并使用 <code>when</code> 表达式来决定每一步要打印的内容。
    </def>
</deflist>

<deflist collapsible="true">
    <def title="Hint 2">
        Use the modulo operator (<code>%</code>) to return the remainder of a number being divided. Use the <a href="operator-overloading.md#equality-and-inequality-operators">equality operator</a> 
        (<code>==</code>) to check if the remainder equals zero.
        <br />使用取模运算符 (<code>%</code>) 返回被除数的余数。
        <br />使用 <a href="operator-overloading.md#equality-and-inequality-operators">相等运算符</a>(<code>==</code>) 
        检查余数是否等于零。
    </def>
</deflist>

|---|---|
```kotlin
fun main() {
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-control-flow-loops-exercise-2"}

|---|---|
```kotlin
fun main() {
    for (number in 1..100) {
        println(
            when {
                number % 15 == 0 -> "fizzbuzz"
                number % 3 == 0 -> "fizz"
                number % 5 == 0 -> "buzz"
                else -> "$number"
            }
        )
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-control-flow-loops-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="loops-exercise-3"}

You have a list of words. Use `for` and `if` to print only the words that start with the letter `l`.
<br />你有一系列单词。使用 `for` 和 `if` 语句，只打印出以字母 `l` 开头的单词。

<deflist collapsible="true">
    <def title="Hint">
        Use the <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/starts-with.html"> <code>.startsWith()</code>
        </a> function for <code>String</code> type. 
        <br />对 <code>String</code> 类型使用 <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/starts-with.html"> <code>.startsWith()</code>
        </a> 函数。
    </def>
</deflist>

|---|---|
```kotlin
fun main() {
    val words = listOf("dinosaur", "limousine", "magazine", "language")
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-control-flow-loops-exercise-3"}

|---|---|
```kotlin
fun main() {
    val words = listOf("dinosaur", "limousine", "magazine", "language")
    for (w in words) {
        if (w.startsWith("l"))
            println(w)
    }
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-control-flow-loops-solution-3"}

## Next step
接下来

[Functions](kotlin-tour-functions.md)
<br />[函数](kotlin-tour-functions.md)
