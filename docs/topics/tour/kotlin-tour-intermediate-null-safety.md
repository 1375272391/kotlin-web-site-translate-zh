[//]: # (title: Intermediate: Null safety)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-done.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-done.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7-done.svg" width="20" alt="Seventh step" /> <a href="kotlin-tour-intermediate-properties.md">Properties</a><br />
        <img src="icon-8.svg" width="20" alt="Eighth step" /> <strong>Null safety</strong><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In the beginner tour, you learned how to handle `null` values in your code. This chapter covers common use cases for null
safety features and how to make the most of them.
<br />在入门教程中，您学习了如何在代码中处理 `null` 值。本章将介绍空值安全特性的常见用例以及如何充分利用它们。

## Smart casts and safe casts
智能转换和安全转换

Kotlin can sometimes infer the type without explicit declaration. When you tell Kotlin to treat a variable or object as if it belongs to a
specific type, this process is called **casting**. When a type is automatically cast, like when it's inferred, it's called
**smart casting**.
<br />Kotlin 有时无需显式声明即可推断类型。当您指示 Kotlin 将变量或对象视为属于特定类型时，此过程称为**类型转换**。当类型自动转换时（例如类型推断），称为**智能转换**。

### is and !is operators
is 和 !is 运算符

Before we explore how casting works, let's see how you can check if an object has a certain type. For this, you can use the
`is` and `!is` operators with `when` or `if` conditional expressions:
<br />在探讨类型转换的工作原理之前，我们先来看看如何检查对象是否具有特定类型。为此，您可以使用 `is` 和 `!is` 运算符以及 `when` 或 `if` 条件表达式：

* `is` checks if the object has the type and returns a boolean value.
  <br />`is` 检查对象是否具有指定类型，并返回布尔值。
* `!is` checks if the object **doesn't** have the type and returns a boolean value.
  <br />`!is` 检查对象是否**不**具有该类型，并返回一个布尔值。

For example:
<br />例如：

```kotlin
fun printObjectType(obj: Any) {
    when (obj) {
        is Int -> println("It's an Integer with value $obj")
        !is Double -> println("It's NOT a Double")
        else -> println("Unknown type")
    }
}

fun main() {
    val myInt = 42
    val myDouble = 3.14
    val myList = listOf(1, 2, 3)
  
    // The type is Int
    printObjectType(myInt)
    // It's an Integer with value 42

    // The type is List, so it's NOT a Double.
    printObjectType(myList)
    // It's NOT a Double

    // The type is Double, so the else branch is triggered.
    printObjectType(myDouble)
    // Unknown type
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-casts"}

> You've already seen an example of how to use a `when` conditional expression with the `is` and `!is` operators in the [Open and other special classes](kotlin-tour-intermediate-open-special-classes.md#sealed-classes) chapter.
> <br />在[Open 和其他特殊类](kotlin-tour-intermediate-open-special-classes.md#sealed-classes)章节中，您已经看到了如何使用`when`条件表达式以及`is`和`!is`运算符的示例。
{style="tip"}

### as and as? operators
as 和 as? 运算符

To explicitly _cast_ an object to any other type, use the `as` operator. This includes casting from a nullable 
type to its non-nullable counterpart. If the cast isn't possible, the program crashes **at runtime**. This is why it's 
called the **unsafe** cast operator.
<br />要显式地将对象强制*转换*为任何其他类型，请使用 `as` 运算符。这包括将可空类型强制转换为其不可空对应类型。
如果强制转换不可行，程序将**在运行时**崩溃。这就是为什么它被称为**不安全**强制转换运算符的原因。

```kotlin
fun main() {
//sampleStart
    val a: String? = null
    val b = a as String

    // Triggers an error at runtime
    print(b)
//sampleEnd
}
```
{kotlin-runnable="true" validate="false" id="kotlin-tour-null-safety-as-operator"}

To explicitly cast an object to a non-nullable type, but return `null` instead of throwing an error on failure, use the `as?`
operator. Since the `as?` operator doesn't trigger an error on failure, it is called the **safe** operator.
<br />要将对象显式强制转换为非空类型，但在转换失败时返回 `null` 而不是抛出错误，请使用 `as?` 运算符。
由于 `as?` 运算符在转换失败时不会触发错误，因此它被称为**安全**运算符。

```kotlin
fun main() {
//sampleStart
    val a: String? = null
    val b = a as? String

    // Returns null value
    print(b)
    // null
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-safe-operator"}

You can combine the `as?` operator with the Elvis operator `?:` to reduce several lines of code down to one. For example,
the following `calculateTotalStringLength()` function calculates the total length of all strings provided in a mixed list:
<br />您可以将 `as?` 运算符与 Elvis 运算符 `?:` 结合使用，从而将多行代码简化为一行。例如，
以下 `calculateTotalStringLength()` 函数计算混合列表中所有字符串的总长度：

```kotlin
fun calculateTotalStringLength(items: List<Any>): Int {
    var totalLength = 0

    for (item in items) {
        totalLength += if (item is String) {
            item.length
        } else {
            0  // Add 0 for non-String items
        }
    }

    return totalLength
}
```

The example:
<br />例如：

* Uses the `totalLength` variable as a counter.
  <br />使用 `totalLength` 变量作为计数器。
* Uses a `for` loop to loop through every item in the list.
  <br />使用“for”循环遍历列表中的每个项目。
* Uses an `if` and the `is` operator to check if the current item is a string:
  <br />使用 `if` 和 `is` 运算符检查当前项是否为字符串：
  * If it is, the string's length is added to the counter.
    <br />如果是，则将字符串的长度添加到计数器中。
  * If it is not, the counter isn't incremented.
    <br />如果不是，则计数器不会递增。
* Returns the final value of the `totalLength` variable.
  <br />返回 `totalLength` 变量的最终值。

This code can be reduced to:
<br />这段代码可以简化为：

```kotlin
fun calculateTotalStringLength(items: List<Any>): Int {
    return items.sumOf { (it as? String)?.length ?: 0 }
}
```

The example uses the [`.sumOf()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/sum-of.html) extension function and provides a lambda expression that:
<br />该示例使用了扩展函数 [`.sumOf()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/sum-of.html)，并提供了一个 lambda 表达式，该表达式：

* For each item in the list, performs a safe cast to `String` using `as?`.
  <br />对于列表中的每个项目，使用 `as?` 安全地将其转换为 `String`。
* Uses a safe call `?.` to access the `length` property if the call doesn't return a `null` value.
  <br />如果调用不返回 `null` 值，则使用安全调用 `?.` 来访问 `length` 属性。
* Uses the Elvis operator `?:` to return `0` if the safe call returns a `null` value.
  <br />使用 Elvis 运算符 `?:`，如果安全调用返回 `null` 值，则返回 `0`。

## Null values and collections
空值和空集合

In Kotlin, working with collections often involves handling `null` values and filtering out unnecessary elements. Kotlin
has useful functions that you can use to write clean, efficient, and null-safe code when working with lists, sets, maps,
and other types of collections.
<br />在 Kotlin 中，处理集合通常涉及处理 `null` 值和过滤掉不必要的元素。
Kotlin 提供了一些实用的函数，可用于编写简洁、高效且空值安全的代码，尤其是在处理列表、集合、映射 以及其他类型的集合时。

To filter `null` values from a list, use the [`filterNotNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/filter-not-null.html) function:
<br />要从列表中过滤掉 `null` 值，请使用 [`filterNotNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/filter-not-null.html) 函数：

```kotlin
fun main() {
//sampleStart
    val emails: List<String?> = listOf("alice@example.com", null, "bob@example.com", null, "carol@example.com")

    val validEmails = emails.filterNotNull()

    println(validEmails)
    // [alice@example.com, bob@example.com, carol@example.com]
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-filternotnull"}

If you want to perform filtering of `null` values directly when creating a list, use the [`listOfNotNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/list-of-not-null.html) function:
<br />如果要在创建列表时直接过滤 `null` 值，请使用 [`listOfNotNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/list-of-not-null.html) 函数：

```kotlin
fun main() {
//sampleStart
    val serverConfig = mapOf(
        "appConfig.json" to "App Configuration",
        "dbConfig.json" to "Database Configuration"
    )

    val requestedFile = "appConfig.json"
    val configFiles = listOfNotNull(serverConfig[requestedFile])

    println(configFiles)
    // [App Configuration]
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-listofnotnull"}

In both of these examples, if all items are `null` values, an empty list is returned.
<br />在这两个例子中，如果所有项均为 `null` 值，则返回一个空列表。

Kotlin also provides functions that you can use to find values in collections. If a value isn't found, they return `null`
values instead of triggering an error:
<br />Kotlin 还提供了用于在集合中查找值的函数。如果找不到值，这些函数会返回 `null` 值，而不是触发错误。

* [`maxOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/max-or-null.html) finds the highest value. If one doesn't exist, returns a `null` value.
  <br />[`maxOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/max-or-null.html) 函数用于查找最大值。如果不存在最大值，则返回 `null` 值。
* [`minOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/min-or-null.html) finds the lowest value. If one doesn't exist, returns a `null` value.
  <br />[`minOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/min-or-null.html) 函数用于查找最小值。如果不存在最小值，则返回 `null` 值。

For example:
<br />例如：

```kotlin
fun main() {
//sampleStart
    // Temperatures recorded over a week
    val temperatures = listOf(15, 18, 21, 21, 19, 17, 16)
  
    // Find the highest temperature of the week
    val maxTemperature = temperatures.maxOrNull()
    println("Highest temperature recorded: ${maxTemperature ?: "No data"}")
    // Highest temperature recorded: 21

    // Find the lowest temperature of the week
    val minTemperature = temperatures.minOrNull()
    println("Lowest temperature recorded: ${minTemperature ?: "No data"}")
    // Lowest temperature recorded: 15
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-collections"}

This example uses the Elvis operator `?:` to return a printed statement if the functions return a `null` value.
<br />本示例使用 Elvis 运算符 `?:`，如果函数返回 `null` 值，则返回打印语句。

> The `maxOrNull()`, and `minOrNull()` functions are designed to be used with collections that **don't**
> contain `null` values. Otherwise, you can't tell whether the function couldn't find the desired value or whether it
> found a `null` value.
> <br />`maxOrNull()` 和 `minOrNull()` 函数设计用于不包含 `null` 值的集合。
> 否则，您将无法判断函数是找不到所需值还是找到了 `null` 值。
>
{style="note"}

You can use the [`singleOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/single-or-null.html) function with a lambda expression to find a single item that matches a condition.
If one doesn't exist or there are multiple items that match, the function returns a `null` value:
<br />您可以使用带有 lambda 表达式的 `singleOrNull()` 函数来查找符合条件的单个元素。
如果不存在符合条件的元素，或者存在多个符合条件的元素，则该函数返回 `null` 值。

```kotlin
fun main() {
//sampleStart
    // Temperatures recorded over a week
    val temperatures = listOf(15, 18, 21, 21, 19, 17, 16)

    // Check if there was exactly one day with 30 degrees
    val singleHotDay = temperatures.singleOrNull{ it == 30 }
    println("Single hot day with 30 degrees: ${singleHotDay ?: "None"}")
    // Single hot day with 30 degrees: None
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-singleornull"}

> The `singleOrNull()` function is designed to be used with collections that **don't** contain `null` values.
> <br />`singleOrNull()` 函数旨在与不包含 `null` 值的集合一起使用。
>
{style="note"}

Some functions use a lambda expression to transform a collection and return `null` values if they can't
fulfill their purpose.
<br />有些函数使用 lambda 表达式来转换集合，如果无法实现其目的，则返回 `null` 值。

To transform a collection with a lambda expression and return the first value that isn't `null`, use the 
[`firstNotNullOfOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/first-not-null-of-or-null.html) function. If no such value exists, the function returns a `null` value:
<br />要使用 lambda 表达式转换集合并返回第一个非 `null` 值，请使用 [`firstNotNullOfOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/first-not-null-of-or-null.html) 函数。如果不存在这样的值，该函数将返回 `null` 值。

```kotlin
fun main() {
//sampleStart
    data class User(val name: String?, val age: Int?)

    val users = listOf(
        User(null, 25),
        User("Alice", null),
        User("Bob", 30)
    )

    val firstNonNullName = users.firstNotNullOfOrNull { it.name }
    println(firstNonNullName)
    // Alice
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-firstnotnullofornull"}

To use a lambda expression to process each collection item sequentially and create an accumulated value (or return a 
`null` value if the collection is empty) use the [`reduceOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/reduce-or-null.html) function:
<br />要使用 lambda 表达式按顺序处理集合中的每个项并创建累积值（如果集合为空，则返回 `null` 值），请使用 [`reduceOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/reduce-or-null.html) 函数：

```kotlin
fun main() {
//sampleStart
    // Prices of items in a shopping cart
    val itemPrices = listOf(20, 35, 15, 40, 10)

    // Calculate the total price using the reduceOrNull() function
    val totalPrice = itemPrices.reduceOrNull { runningTotal, price -> runningTotal + price }
    println("Total price of items in the cart: ${totalPrice ?: "No items"}")
    // Total price of items in the cart: 120

    val emptyCart = listOf<Int>()
    val emptyTotalPrice = emptyCart.reduceOrNull { runningTotal, price -> runningTotal + price }
    println("Total price of items in the empty cart: ${emptyTotalPrice ?: "No items"}")
    // Total price of items in the empty cart: No items
//sampleEnd
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-reduceornull"}

This example also uses the Elvis operator `?:` to return a printed statement if the function returns a `null` value.
<br />此示例还使用了 Elvis 运算符 `?:`，以便在函数返回 `null` 值时返回打印语句。

> The `reduceOrNull()` function is designed to be used with collections that **don't** contain `null` values.
> <br />`reduceOrNull()` 函数旨在用于不包含 `null` 值的集合。
>
{style="note"}

Explore Kotlin's [standard library](https://kotlinlang.org/api/core/kotlin-stdlib/) to find more functions that you can 
use to make your code safer.
<br />探索 Kotlin 的[标准库](https://kotlinlang.org/api/core/kotlin-stdlib/)，找到更多可以用来提高代码安全性的函数。

## Early returns and the Elvis operator
<br />提前返回和Elvis运算符

In the beginner tour, you learned how to use [early returns](kotlin-tour-functions.md#early-returns-in-functions) to stop
your function from being processed further than a certain point. You can use the Elvis operator `?:` with an early return
to check preconditions in a function. This approach is a great way to keep your code concise because you don't need to use
nested checks. The reduced complexity of your code also makes it easier to maintain. For example:
<br />在入门教程中，你学习了如何使用[提前返回](kotlin-tour-functions.md#early-returns-in-functions)来阻止函数执行到某个特定点之后。
你可以使用Elvis运算符`?:`配合提前返回来检查函数中的前提条件。这种方法可以很好地保持代码简洁，因为你不需要使用嵌套检查。
代码复杂度的降低也使其更易于维护。例如：

```kotlin
data class User(
    val id: Int,
    val name: String,
    // List of friend user IDs
    val friends: List<Int>
)

// Function to get the number of friends for a user
fun getNumberOfFriends(users: Map<Int, User>, userId: Int): Int {
    // Retrieves the user or return -1 if not found
    val user = users[userId] ?: return -1
    // Returns the number of friends
    return user.friends.size
}

fun main() {
    // Creates some sample users
    val user1 = User(1, "Alice", listOf(2, 3))
    val user2 = User(2, "Bob", listOf(1))
    val user3 = User(3, "Charlie", listOf(1))

    // Creates a map of users
    val users = mapOf(1 to user1, 2 to user2, 3 to user3)

    println(getNumberOfFriends(users, 1))
    // 2
    println(getNumberOfFriends(users, 2))
    // 1
    println(getNumberOfFriends(users, 4))
    // -1
}
```
{kotlin-runnable="true" id="kotlin-tour-null-safety-early-return"}

In this example:
<br />在这个例子中：

* There is a `User` data class that has properties for the user's `id`, `name` and list of friends.
  <br />有一个名为 `User` 的数据类，它具有用户的 `id`、`name` 和好友列表等属性。
* The `getNumberOfFriends()` function:
  <br />`getNumberOfFriends()` 函数：
  * Accepts a map of `User` instances and a user ID as an integer.
    <br />接受一个 `User` 实例的映射和一个用户 ID（整数）。
  * Accesses the value of the map of `User` instances with the provided user ID.
    <br />使用提供的用户 ID 访问 `User` 实例映射的值。
  * Uses an Elvis operator to return the function early with the value of `-1` if the map value is a `null` value.
    <br />使用 Elvis 运算符，如果映射值为 `null` 值，则提前返回函数，返回值为 `-1`。
  * Assigns the value found from the map to the `user` variable.
    <br />将映射中找到的值赋给 `user` 变量。
  * Returns the number of friends in the user's friends list by using the `size` property.
    <br />使用 `size` 属性返回用户好友列表中的好友数量。
* The `main()` function:
  <br />`main()` 函数：
  * Creates three `User` instances. 
    <br />创建三个 `User` 实例。
  * Creates a map of these `User` instances and assigns them to the `users` variable. 
    <br />创建这些 `User` 实例的映射，并将它们分配给 `users` 变量。
  * Calls the `getNumberOfFriends()` function on the `users` variable with values `1` and `2` that returns two friends for `"Alice"` and one friend for `"Bob"`.
    <br />调用 `users` 变量的 `getNumberOfFriends()` 函数，传入值 `1` 和 `2`，返回 `"Alice"` 的两个朋友和 `"Bob"` 的一个朋友。
  * Calls the `getNumberOfFriends()` function on the `users` variable with value `4`, which triggers an early return with a value of `-1`.
    <br />调用 `users` 变量的 `getNumberOfFriends()` 函数，值为 `4`，这将触发提前返回，返回值为 `-1`。

You may notice that the code could be more concise without an early return. However, this approach needs multiple safe 
calls because the `users[userId]` might return a `null` value, making the code slightly harder to read:
<br />您可能会注意到，如果没有提前返回，代码会更简洁。但是，这种方法需要多次安全调用，因为 `users[userId]` 可能会返回 `null` 值，这会使代码的可读性略有下降：

```kotlin
fun getNumberOfFriends(users: Map<Int, User>, userId: Int): Int {
    // Retrieve the user or return -1 if not found
    return users[userId]?.friends?.size ?: -1
}
```
{validate="false"}

Although this example checks only one condition with the Elvis operator, you can add multiple checks to cover any critical
error paths. Early returns with the Elvis operator prevent your program from doing unnecessary work and make your code 
safer by stopping as soon as a `null` value or invalid case is detected.
<br />虽然此示例仅使用 Elvis 运算符检查一个条件，但您可以添加多个检查以覆盖任何关键错误路径。
Elvis 运算符的提前返回功能可防止程序执行不必要的操作，并在检测到 `null` 值或无效情况时立即停止，从而使代码更安全。

For more information about how you can use `return` in your code, see [Returns and jumps](returns.md).
<br />有关如何在代码中使用 `return` 的更多信息，请参阅[返回和跳转](returns.md)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="null-safety-exercise-1"}

You are developing a notification system for an app where users can enable or disable different types of notifications.
Complete the `getNotificationPreferences()` function so that:
<br />您正在为一款应用开发通知系统，用户可以启用或禁用不同类型的通知。
请完善 `getNotificationPreferences()` 函数，使其能够：

1. The `validUser` variable uses the `as?` operator to check if `user` is an instance of the `User` class. If it isn't, return an empty list.
   <br />`validUser` 变量使用 `as?` 运算符检查 `user` 是否为 `User` 类的实例。如果不是，则返回一个空列表。
2. The `userName` variable uses the Elvis `?:` operator to ensure that the user's name defaults to `"Guest"` if it is `null`.
   <br />`userName` 变量使用 Elvis `?:` 运算符来确保如果用户名为 `null`，则用户名默认为 `"Guest"`。
3. The final return statement uses the `.takeIf()` function to include email and SMS notification preferences only if they are enabled.
   <br />最后返回语句使用 `.takeIf()` 函数，仅在启用电子邮件和短信通知偏好设置时才包含这些偏好设置。
4. The `main()` function runs successfully and prints the expected output.
   <br />`main()` 函数运行成功，并打印出预期的输出。

> The [`takeIf()` function](scope-functions.md#takeif-and-takeunless) returns the original value if the given condition is true,
> otherwise it returns `null`. For example:
> <br />[`takeIf()` 函数](scope-functions.md#takeif-and-takeunless)会在给定条件为真时返回原始值，否则返回 `null`。例如：
>
> ```kotlin
> fun main() {
>     // The user is logged in
>     val userIsLoggedIn = true
>     // The user has an active session
>     val hasSession = true
> 
>     // Gives access to the dashboard if the user is logged in
>     // and has an active session
>     val canAccessDashboard = userIsLoggedIn.takeIf { hasSession }
> 
>     println(canAccessDashboard ?: "Access denied")
>     // true
> }
> ```
>
{style = "tip"}

|--|--|

```kotlin
data class User(val name: String?)

fun getNotificationPreferences(user: Any, emailEnabled: Boolean, smsEnabled: Boolean): List<String> {
    val validUser = // Write your code here
    val userName = // Write your code here

    return listOfNotNull( /* Write your code here */)
}

fun main() {
    val user1 = User("Alice")
    val user2 = User(null)
    val invalidUser = "NotAUser"

    println(getNotificationPreferences(user1, emailEnabled = true, smsEnabled = false))
    // [Email Notifications enabled for Alice]
    println(getNotificationPreferences(user2, emailEnabled = false, smsEnabled = true))
    // [SMS Notifications enabled for Guest]
    println(getNotificationPreferences(invalidUser, emailEnabled = true, smsEnabled = true))
    // []
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-null-safety-exercise-1"}

|--|--|

```kotlin
data class User(val name: String?)

fun getNotificationPreferences(user: Any, emailEnabled: Boolean, smsEnabled: Boolean): List<String> {
    val validUser = user as? User ?: return emptyList()
    val userName = validUser.name ?: "Guest"

    return listOfNotNull(
        "Email Notifications enabled for $userName".takeIf { emailEnabled },
        "SMS Notifications enabled for $userName".takeIf { smsEnabled }
    )
}

fun main() {
    val user1 = User("Alice")
    val user2 = User(null)
    val invalidUser = "NotAUser"

    println(getNotificationPreferences(user1, emailEnabled = true, smsEnabled = false))
    // [Email Notifications enabled for Alice]
    println(getNotificationPreferences(user2, emailEnabled = false, smsEnabled = true))
    // [SMS Notifications enabled for Guest]
    println(getNotificationPreferences(invalidUser, emailEnabled = true, smsEnabled = true))
    // []
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-null-safety-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="null-safety-exercise-2"}

You are working on a subscription-based streaming service where users can have multiple subscriptions, but **only one 
can be active at a time**. Complete the `getActiveSubscription()` function so that it uses the `singleOrNull()` function
with a predicate to return a `null` value if there is more than one active subscription:
<br />你正在开发一个基于订阅的流媒体服务，用户可以拥有多个订阅，但**同一时间只能有一个订阅处于激活状态**。
请完善 `getActiveSubscription()` 函数，使其使用 `singleOrNull()` 函数，并添加一个谓词，以便在存在多个激活订阅时返回 `null` 值：

|--|--|

```kotlin
data class Subscription(val name: String, val isActive: Boolean)

fun getActiveSubscription(subscriptions: List<Subscription>): Subscription? // Write your code here

fun main() {
    val userWithPremiumPlan = listOf(
        Subscription("Basic Plan", false),
        Subscription("Premium Plan", true)
    )

    val userWithConflictingPlans = listOf(
        Subscription("Basic Plan", true),
        Subscription("Premium Plan", true)
    )

    println(getActiveSubscription(userWithPremiumPlan))
    // Subscription(name=Premium Plan, isActive=true)

    println(getActiveSubscription(userWithConflictingPlans))
    // null
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-null-safety-exercise-2"}

|--|--|

```kotlin
data class Subscription(val name: String, val isActive: Boolean)

fun getActiveSubscription(subscriptions: List<Subscription>): Subscription? {
    return subscriptions.singleOrNull { subscription -> subscription.isActive }
}

fun main() {
    val userWithPremiumPlan = listOf(
        Subscription("Basic Plan", false),
        Subscription("Premium Plan", true)
    )

    val userWithConflictingPlans = listOf(
        Subscription("Basic Plan", true),
        Subscription("Premium Plan", true)
    )

    println(getActiveSubscription(userWithPremiumPlan))
    // Subscription(name=Premium Plan, isActive=true)

    println(getActiveSubscription(userWithConflictingPlans))
    // null
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 1" id="kotlin-tour-null-safety-solution-2-1"}

|--|--|

```kotlin
data class Subscription(val name: String, val isActive: Boolean)

fun getActiveSubscription(subscriptions: List<Subscription>): Subscription? =
    subscriptions.singleOrNull { it.isActive }

fun main() {
    val userWithPremiumPlan = listOf(
        Subscription("Basic Plan", false),
        Subscription("Premium Plan", true)
    )

    val userWithConflictingPlans = listOf(
        Subscription("Basic Plan", true),
        Subscription("Premium Plan", true)
    )

    println(getActiveSubscription(userWithPremiumPlan))
    // Subscription(name=Premium Plan, isActive=true)

    println(getActiveSubscription(userWithConflictingPlans))
    // null
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 2" id="kotlin-tour-null-safety-solution-2-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="null-safety-exercise-3"}

You are working on a social media platform where users have usernames and account statuses. You want to see the list of 
currently active usernames. Complete the `getActiveUsernames()` function so that the [`mapNotNull()` function](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/map-not-null.html)
has a predicate that returns the username if it is active or a `null` value if it isn't:
<br />你正在开发一个社交媒体平台，用户拥有用户名和账户状态。你想查看当前活跃用户名的列表。
请完善 `getActiveUsernames()` 函数，使其能够与 `mapNotNull()` 函数配合使用。
该函数会添加一个谓词，如果用户名处于活跃状态则返回该用户名，否则返回 `null` 值：

|--|--|

```kotlin
data class User(val username: String, val isActive: Boolean)

fun getActiveUsernames(users: List<User>): List<String> {
    return users.mapNotNull { /* Write your code here */ }
}

fun main() {
    val allUsers = listOf(
        User("alice123", true),
        User("bob_the_builder", false),
        User("charlie99", true)
    )

    println(getActiveUsernames(allUsers))
    // [alice123, charlie99]
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-null-safety-exercise-3"}

|--|--|

> Just like in Exercise 1, you can use the [`takeIf()` function](scope-functions.md#takeif-and-takeunless) when you check
> if the user is active.
> <br />就像练习 1 中一样，当检查用户是否处于活动状态时，可以使用 [`takeIf()` 函数](scope-functions.md#takeif-and-takeunless)。
>
{ style = "tip" }

|--|--|

```kotlin
data class User(val username: String, val isActive: Boolean)

fun getActiveUsernames(users: List<User>): List<String> {
    return users.mapNotNull { user ->
        if (user.isActive) user.username else null
    }
}

fun main() {
    val allUsers = listOf(
        User("alice123", true),
        User("bob_the_builder", false),
        User("charlie99", true)
    )

    println(getActiveUsernames(allUsers))
    // [alice123, charlie99]
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 1" id="kotlin-tour-null-safety-solution-3-1"}

|--|--|

```kotlin
data class User(val username: String, val isActive: Boolean)

fun getActiveUsernames(users: List<User>): List<String> =
    users.mapNotNull { user -> user.username.takeIf { user.isActive } }

fun main() {
    val allUsers = listOf(
        User("alice123", true),
        User("bob_the_builder", false),
        User("charlie99", true)
    )

    println(getActiveUsernames(allUsers))
    // [alice123, charlie99]
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 2" id="kotlin-tour-null-safety-solution-3-2"}

### Exercise 4 {initial-collapse-state="collapsed" collapsible="true" id="null-safety-exercise-4"}

You are working on an inventory management system for an e-commerce platform. Before processing a sale, you need to check
if the requested quantity of a product is valid based on the available stock.
<br />你正在为一个电商平台开发库存管理系统。在处理一笔销售之前，你需要检查：
所请求的产品数量是否符合现有库存。

Complete the `validateStock()` function so that it uses early returns and the Elvis operator (where applicable) to check if:
<br />完善 `validateStock()` 函数，使其使用早期回报和 Elvis 运算符（如果适用）来检查：

* The `requested` variable is `null`.
  <br />请求的变量为 `null`。
* The `available` variable is `null`.
  <br />`available` 变量为 `null`。
* The `requested` variable is a negative value.
  <br />`requested` 变量为负值。
* The amount in the `requested` variable is higher than in the `available` variable.
  <br />`requested` 变量中的数量高于 `available` 变量中的数量。

In all of the above cases, the function must return early with the value of `-1`.
<br />在上述所有情况下，该函数必须提前返回，返回值为 `-1`。

|--|--|

```kotlin
fun validateStock(requested: Int?, available: Int?): Int {
    // Write your code here
}

fun main() {
    println(validateStock(5,10))
    // 5
    println(validateStock(null,10))
    // -1
    println(validateStock(-2,10))
    // -1
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-null-safety-exercise-4"}

|--|--|

```kotlin
fun validateStock(requested: Int?, available: Int?): Int {
    val validRequested = requested ?: return -1
    val validAvailable = available ?: return -1

    if (validRequested < 0) return -1
    if (validRequested > validAvailable) return -1

    return validRequested
}

fun main() {
    println(validateStock(5,10))
    // 5
    println(validateStock(null,10))
    // -1
    println(validateStock(-2,10))
    // -1
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-null-safety-solution-4"}

## Next step
接下来

[Intermediate: Libraries and APIs](kotlin-tour-intermediate-libraries-and-apis.md)
<br />[中级：库和 API](kotlin-tour-intermediate-libraries-and-apis.md)
