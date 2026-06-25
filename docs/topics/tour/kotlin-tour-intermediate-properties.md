[//]: # (title: Intermediate: Properties)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-intermediate-extension-functions.md">Extension functions</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-intermediate-scope-functions.md">Scope functions</a><br />
        <img src="icon-3-done.svg" width="20" alt="Third step" /> <a href="kotlin-tour-intermediate-lambdas-receiver.md">Lambda expressions with receiver</a><br />
        <img src="icon-4-done.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-intermediate-classes-interfaces.md">Classes and interfaces</a><br />
        <img src="icon-5-done.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-intermediate-objects.md">Objects</a><br />
        <img src="icon-6-done.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-intermediate-open-special-classes.md">Open and special classes</a><br />
        <img src="icon-7.svg" width="20" alt="Seventh step" /> <strong>Properties</strong><br />
        <img src="icon-8-todo.svg" width="20" alt="Eighth step" /> <a href="kotlin-tour-intermediate-null-safety.md">Null safety</a><br />
        <img src="icon-9-todo.svg" width="20" alt="Ninth step" /> <a href="kotlin-tour-intermediate-libraries-and-apis.md">Libraries and APIs</a></p>
</tldr>

In the beginner tour, you learned how properties are used to declare characteristics of class instances and how to access
them. This chapter digs deeper into how properties work in Kotlin and explores other ways that you can use them in your code.
<br />在入门教程中，您学习了如何使用属性来声明类实例的特征以及如何访问它们。本章将深入探讨 Kotlin 中属性的工作原理，并探索在代码中使用属性的其他方法。

## Backing fields
后备字段

In Kotlin, properties have default `get()` and `set()` functions, known as property accessors, which handle retrieving 
and modifying their values. While these default functions are not explicitly visible in the code, the compiler automatically
generates them to manage property access behind the scenes. These accessors use a **backing field** to store 
the actual property value.
<br />在 Kotlin 中，属性有默认的 `get()` 和 `set()` 函数，称为属性访问器，用于获取和修改属性值。
虽然这些默认函数在代码中不可见，但编译器会自动生成它们，以在后台管理属性访问。这些访问器使用一个**支持字段**来存储实际的属性值。

Backing fields exist if either of the following is true:
<br />如果满足以下任一条件，则存在后备字段：

* You use the default `get()` or `set()` functions for the property.
  <br />您可以使用属性的默认 `get()` 或 `set()` 函数。
* You try to access the property value in code by using the `field` keyword.
  <br />您尝试在代码中使用 `field` 关键字访问属性值。

> `get()` and `set()` functions are also called getters and setters.
> <br />`get()` 和 `set()` 函数也称为 getter 和 setter。
>
{style="tip"}

For example, this code has the `category` property that has no custom `get()` or `set()` functions and therefore uses the
default implementations:
<br />例如，这段代码的 `category` 属性没有自定义的 `get()` 或 `set()` 函数，因此使用了以下默认实现：

```kotlin
class Contact(val id: Int, var email: String) {
    var category: String = ""
}
```

Under the hood, this is equivalent to this pseudocode:
<br />从本质上讲，这等价于以下伪代码：

```kotlin
class Contact(val id: Int, var email: String) {
    var category: String = ""
        get() = field
        set(value) {
            field = value
        }
}
```
{validate="false"}

In this example:
<br />在这个例子中：

* The `get()` function retrieves the property value from the field: `""`.
  <br />`get()` 函数从字段中检索属性值：`""`。
* The `set()` function accepts `value` as a parameter and assigns it to the field, where `value` is `""`. 
  <br />`set()` 函数接受 `value` 作为参数并将其赋值给字段，其中 `value` 为 `""`。

Access to the backing field is useful when you want to add extra logic in your `get()` or `set()` functions 
without causing an infinite loop. For example, you have a `Person` class with a `name` property:
<br />当您想在 `get()` 或 `set()` 函数中添加额外逻辑，而又不想陷入无限循环时，访问后备字段就非常有用。
例如，您有一个带有 `name` 属性的 `Person` 类：

```kotlin
class Person {
    var name: String = ""
}
```

You want to ensure that the first letter of the `name` property is capitalized, so you create a custom `set()` function
that uses the [`.replaceFirstChar()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/replace-first-char.html) 
and [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase-char.html) extension functions. 
However, if you refer to the property directly in your `set()` function, you create an infinite loop and see a `StackOverflowError`
at runtime:
<br />您希望确保 `name` 属性的首字母大写，因此您创建了一个自定义的 `set()` 函数，
该函数使用了 [`.replaceFirstChar()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/replace-first-char.html) 和 [`.uppercase()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase-char.html) 扩展函数。
但是，如果您在 `set()` 函数中直接引用该属性，则会创建一个无限循环，并在运行时看到 `StackOverflowError` 错误。

```kotlin
class Person {
    var name: String = ""
        set(value) {
            // This causes a runtime error
            name = value.replaceFirstChar { firstChar -> firstChar.uppercase() }
        }
}

fun main() {
    val person = Person()
    person.name = "kodee"
    println(person.name)
    // Exception in thread "main" java.lang.StackOverflowError
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-stackoverflow"}

To fix this, you can use the backing field in your `set()` function instead by referencing it with the `field` keyword:
<br />为了解决这个问题，你可以在 `set()` 函数中使用 `field` 关键字引用支持字段：

```kotlin
class Person {
    var name: String = ""
        set(value) {
            field = value.replaceFirstChar { firstChar -> firstChar.uppercase() }
        }
}

fun main() {
    val person = Person()
    person.name = "kodee"
    println(person.name)
    // Kodee
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-backingfield"}

Backing fields are also useful when you want to add logging, send notifications when a property value changes,
or use additional logic that compares the old and new property values.
<br />当您需要添加日志记录、在属性值更改时发送通知，或使用比较新旧属性值的其他逻辑时，支持字段也很有用。

For more information, see [Backing fields](properties.md#backing-fields).
<br />有关更多信息，请参阅[后备字段](properties.md#backing-fields)。

## Extension properties
扩展属性

Just like extension functions, there are also extension properties. Extension properties allow you to add new properties
to existing classes without modifying their source code. However, extension properties in Kotlin do **not** have backing
fields. This means that you need to write the `get()` and `set()` functions yourself. Additionally, the lack of a backing
field means that they can't hold any state.
<br />与扩展函数类似，Kotlin 中也有扩展属性。扩展属性允许你向现有类添加新属性， 而无需修改其源代码。
但是，Kotlin 中的扩展属性**没有**支持字段。 这意味着你需要自己编写 `get()` 和 `set()` 函数。
此外，由于缺少支持字段，它们无法保存任何状态。

To declare an extension property, write the name of the class that you want to extend followed by a `.` and the name of
your property. Just like with normal class properties, you need to declare a type for your property. 
For example:
<br />要声明扩展属性，请写出要扩展的类名，后跟一个点号 `.`，再写出属性名。
与普通类属性一样，您需要为属性声明类型。
例如：

```kotlin
val String.lastChar: Char
```
{validate="false"}

Extension properties are most useful when you want a property to contain a computed value without using inheritance.
You can think of extension properties working like a function with only one parameter: the receiver.
<br />当您希望属性包含计算值但又不想使用继承时，扩展属性最为有用。
您可以将扩展属性视为一个只有一个参数（即接收者）的函数。

For example, let's say that you have a data class called `Person` with two properties: `firstName` and `lastName`.
<br />例如，假设你有一个名为`Person`的数据类，它有两个属性：`firstName`和`lastName`。

```kotlin
data class Person(val firstName: String, val lastName: String)
```

You want to be able to access the person's full name without modifying the `Person` data class or inheriting from it.
You can do this by creating an extension property with a custom `get()` function:
<br />您希望能够在不修改 `Person` 数据类或继承它的情况下访问人员的全名。
您可以通过创建一个带有自定义 `get()` 函数的扩展属性来实现这一点：

```kotlin
data class Person(val firstName: String, val lastName: String)

// Extension property to get the full name
val Person.fullName: String
    get() = "$firstName $lastName"

fun main() {
    val person = Person(firstName = "John", lastName = "Doe")

    // Use the extension property
    println(person.fullName)
    // John Doe
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-extension"}

> Extension properties can't override existing properties of a class.
> <br />扩展属性不能覆盖类的现有属性。
> 
{style="note"}

Just like with extension functions, the Kotlin standard library uses extension properties widely. For example,
see the [`lastIndex` property](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/last-index.html) for a `CharSequence`.
<br />与扩展函数类似，Kotlin 标准库广泛使用了扩展属性。例如，
请参阅 `CharSequence` 的 [`lastIndex` 属性](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/last-index.html)。

## Delegated properties
委托属性

You already learned about delegation in the [Classes and interfaces](kotlin-tour-intermediate-classes-interfaces.md#delegation) chapter. You can
also use delegation with properties to delegate their property accessors to another object. This is useful
when you have more complex requirements for storing properties that a simple backing field can't handle, such as storing
values in a database table, browser session, or map. Using delegated properties also reduces boilerplate code because the
logic for getting and setting your properties is contained only in the object that you delegate to.
<br />你已经在[类和接口](kotlin-tour-intermediate-classes-interfaces.md#delegation)章节中学习了委托。
你还可以使用属性委托，将属性访问器委托给另一个对象。这在以下情况下非常有用：
当你需要存储更复杂的属性，而简单的支持字段无法处理时，例如将值存储在数据库表、浏览器会话或映射中。
使用委托属性还可以减少样板代码，因为获取和设置属性的逻辑仅包含在你委托的对象中。

The syntax is similar to using delegation with classes but operates on a different level. Declare your property, followed by
the `by` keyword and the object you want to delegate to. For example:
<br />语法类似于在类中使用委托，但操作层级不同。声明属性，后跟`by` 关键字和要委托的对象。例如：

```kotlin
val displayName: String by Delegate
```

Here, the delegated property `displayName` refers to the `Delegate` object for its property accessors.
<br />这里，委托属性 `displayName` 引用了 `Delegate` 对象作为其属性访问器。

Every object you delegate to **must** have a `getValue()` operator function, which Kotlin uses to retrieve the value of 
the delegated property. If the property is mutable, it must also have a `setValue()` operator function for Kotlin to set its value.
<br />你委托的每个对象**必须**拥有一个 `getValue()` 操作符函数，Kotlin 使用该函数来获取被委托属性的值。
如果该属性是可变的，它还必须拥有一个 `setValue()` 操作符函数，以便 Kotlin 设置其值。

By default, the `getValue()` and `setValue()` functions have the following construction:
<br />默认情况下，`getValue()` 和 `setValue()` 函数具有以下结构：

```kotlin
operator fun getValue(thisRef: Any?, property: KProperty<*>): String {}

operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {}
```
{validate="false"}

In these functions:
<br />在这些函数中：

* The `operator` keyword marks these functions as operator functions, enabling them to overload the `get()` and `set()` functions.
  <br />`operator` 关键字将这些函数标记为运算符函数，使它们能够重载 `get()` 和 `set()` 函数。
* The `thisRef` parameter refers to the object **containing** the delegated property. By default, the type is set to `Any?`, but you may need to declare a more specific type.
  <br />`thisRef` 参数指的是**包含**委托属性的对象。默认情况下，类型设置为 `Any?`，但您可能需要声明更具体的类型。
* The `property` parameter refers to the property whose value is accessed or changed. You can use this parameter to access information
like the property's name or type. By default, the type is set to `KProperty<*>` but you can also use `Any?`. You don't need to worry about changing this in your code.
  <br />`property` 参数指的是要访问或更改其值的属性。您可以使用此参数访问属性名称或类型等信息。
默认情况下，类型设置为 `KProperty<*>`，但您也可以使用 `Any?`。您无需在代码中更改此设置。

The `getValue()` function has a return type of `String` by default, but you can adjust this if you want.
<br />`getValue()` 函数默认返回类型为 `String`，但您可以根据需要进行调整。

The `setValue()` function has an additional parameter `value`, which is used to hold the new value that's assigned to the
property.
<br />`setValue()` 函数有一个额外的参数 `value`，用于保存分配给属性的新值。

So, how does this look in practice? Suppose you want to have a computed property, like a user's display name, that is calculated
only once because the operation is expensive and your application is performance-sensitive. You can use a delegated property
to cache the display name so that it is only computed once but can be accessed anytime without performance impact.
<br />那么，这在实践中是如何实现的呢？假设你想要一个计算属性，例如用户的显示名称，并且由于计算成本高昂且你的应用程序对性能要求很高，因此该属性只需计算一次。
你可以使用委托属性来缓存显示名称，这样它只需计算一次，但可以随时访问而不会影响性能。

First, you need to create the object to delegate to. In this case, the object will be an instance of the `CachedStringDelegate` class:
<br />首先，你需要创建要委托的对象。在本例中，该对象将是 `CachedStringDelegate` 类的一个实例：

```kotlin
class CachedStringDelegate {
    var cachedValue: String? = null
}
```

The `cachedValue` property contains the cached value. Within the `CachedStringDelegate` class, add the behavior that you
want from the `get()` function of the delegated property to the `getValue()` operator function body:
<br />`cachedValue` 属性包含缓存值。在 `CachedStringDelegate` 类中，将您希望从委托属性的 `get()` 函数中实现的行为添加到 `getValue()` 运算符函数体中：

```kotlin
class CachedStringDelegate {
    var cachedValue: String? = null

    operator fun getValue(thisRef: Any?, property: Any?): String {
        if (cachedValue == null) {
            cachedValue = "Default Value"
            println("Computed and cached: $cachedValue")
        } else {
            println("Accessed from cache: $cachedValue")
        }
        return cachedValue ?: "Unknown"
    }
}
```

The `getValue()` function checks whether the `cachedValue` property is `null`. If it is, the function assigns the
`"Default value"` and prints a string for logging purposes. If the `cachedValue` property has already been computed, the
property isn't `null`. In this case, another string is printed for logging purposes. Finally, the function uses the Elvis
operator to return the cached value or `"Unknown"` if the value is `null`.
<br />`getValue()` 函数检查 `cachedValue` 属性是否为 `null`。如果为 `null`，则该函数会赋值给 `"Default value"`，并打印一个字符串用于日志记录。
如果 `cachedValue` 属性已被计算，则该属性不为 `null`。在这种情况下，会打印另一个字符串用于日志记录。
最后，该函数使用 Elvis 运算符返回缓存值，如果值为 `null`，则返回 `"Unknown"`。

Now you can delegate the property that you want to cache (`val displayName`) to an instance of the `CachedStringDelegate` class:
<br />现在，您可以将要缓存的属性 (`val displayName`) 委托给 `CachedStringDelegate` 类的实例：

```kotlin
class CachedStringDelegate {
    var cachedValue: String? = null

    operator fun getValue(thisRef: User, property: Any?): String {
        if (cachedValue == null) {
            cachedValue = "${thisRef.firstName} ${thisRef.lastName}"
            println("Computed and cached: $cachedValue")
        } else {
            println("Accessed from cache: $cachedValue")
        }
        return cachedValue ?: "Unknown"
    }
}

class User(val firstName: String, val lastName: String) {
    val displayName: String by CachedStringDelegate()
}

fun main() {
    val user = User("John", "Doe")

    // First access computes and caches the value
    println(user.displayName)
    // Computed and cached: John Doe
    // John Doe

    // Subsequent accesses retrieve the value from cache
    println(user.displayName)
    // Accessed from cache: John Doe
    // John Doe
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-delegated"}

This example:
<br />这个例子：

* Creates a `User` class that has two properties in the header, `firstName`, and `lastName`, and one property in the
class body, `displayName`.
  <br />创建一个名为 `User` 的类，该类在头部有两个属性：`firstName` 和 `lastName`，在类体中有一个属性：`displayName`。
* Delegates the `displayName` property to an instance of the `CachedStringDelegate` class.
  <br />将 `displayName` 属性委托给 `CachedStringDelegate` 类的一个实例。
* Creates an instance of the `User` class called `user`.
  <br />创建一个名为 `user` 的 `User` 类的实例。
* Prints the result of accessing the `displayName` property on the `user` instance.
  <br />打印访问 `user` 实例上的 `displayName` 属性的结果。

Note that in the `getValue()` function, the type for the `thisRef` parameter is narrowed from `Any?` type to the object
type: `User`. This is so that the compiler can access the `firstName` and `lastName` properties of the `User` class.
<br />请注意，在 `getValue()` 函数中，`thisRef` 参数的类型从 `Any?` 类型缩小到对象类型：`User`。
这是为了方便编译器访问 `User` 类的 `firstName` 和 `lastName` 属性。

### Standard delegates
标准代表

The Kotlin standard library provides some useful delegates for you so you don't have to always create yours from scratch.
If you use one of these delegates, you don't need to define `getValue()` and `setValue()` functions because the standard
library automatically provides them.
<br />Kotlin 标准库提供了一些实用的委托，因此您不必总是从头开始创建。
如果您使用这些委托，则无需定义 `getValue()` 和 `setValue()` 函数，因为标准库会自动提供它们。

#### Lazy properties
延迟属性

To initialize a property only when it's first accessed, use a lazy property. The standard library provides the `Lazy`
interface for delegation. 
<br />要仅在首次访问属性时才对其进行初始化，请使用延迟属性。标准库提供了用于委托的 `Lazy` 接口。

To create an instance of the `Lazy` interface, use the `lazy()` function by providing it
with a lambda expression to execute when the `get()` function is called for the first time. Any further calls of the `get()`
function return the same result that was provided on the first call. Lazy properties use the [trailing lambda](kotlin-tour-functions.md#trailing-lambdas) syntax
to pass the lambda expression.
<br />要创建 `Lazy` 接口的实例，请使用 `lazy()` 函数，并为其提供一个 lambda 表达式，以便在首次调用 `get()` 函数时执行。
之后对 `get()` 函数的任何调用都将返回与首次调用相同的结果。Lazy 属性使用 [尾随 lambda](kotlin-tour-functions.md#trailing-lambdas) 语法来传递 lambda 表达式。

For example:
<br />例如：

```kotlin
class Database {
    fun connect() {
        println("Connecting to the database...")
    }

    fun query(sql: String): List<String> {
        return listOf("Data1", "Data2", "Data3")
    }
}

val databaseConnection: Database by lazy {
    val db = Database()
    db.connect()
    db
}

fun fetchData() {
    val data = databaseConnection.query("SELECT * FROM data")
    println("Data: $data")
}

fun main() {
    // First time accessing databaseConnection
    fetchData()
    // Connecting to the database...
    // Data: [Data1, Data2, Data3]

    // Subsequent access uses the existing connection
    fetchData()
    // Data: [Data1, Data2, Data3]
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-lazy"}

In this example:
<br />在这个例子中：

* There is a `Database` class with `connect()` and `query()` member functions. 
  <br />有一个 `Database` 类，其中包含 `connect()` 和 `query()` 成员函数。
* The `connect()` function prints a string to the console, and the `query()` function accepts an SQL query and returns a list.
  <br />`connect()` 函数会将一个字符串打印到控制台，而 `query()` 函数接受一个 SQL 查询并返回一个列表。
* There is a `databaseConnection` property that is a lazy property.
  <br />数据库连接属性是一个延迟加载属性。
* The lambda expression provided to the `lazy()` function:
  <br />传递给 `lazy()` 函数的 lambda 表达式：
  * Creates an instance of the `Database` class.
    <br />创建 `Database` 类的一个实例。
  * Calls the `connect()` member function on this instance (`db`).
    <br />调用此实例（`db`）上的`connect()`成员函数。
  * Returns the instance.
    <br />返回实例。
* There is a `fetchData()` function that:
  <br />有一个 `fetchData()` 函数，它：
  * Creates an SQL query by calling the `query()` function on the `databaseConnection` property.
    <br />通过调用 `databaseConnection` 属性上的 `query()` 函数来创建 SQL 查询。
  * Assigns the SQL query to the `data` variable.
    <br />将 SQL 查询语句赋值给 `data` 变量。
  * Prints the `data` variable to the console.
    <br />将 `data` 变量的值打印到控制台。
* The `main()` function calls the `fetchData()` function. The first time it is called, the lazy property is initialized.
The second time, the same result is returned as the first call.
  <br />`main()` 函数调用 `fetchData()` 函数。第一次调用时，会初始化 lazy 属性。 第二次调用时，返回与第一次相同的结果。

Lazy properties are useful not only when initialization is resource-intensive but also when a property might not be used
in your code. Additionally, lazy properties are thread-safe by default, which is particularly beneficial if you are working
in a concurrent environment.
<br />延迟属性不仅在初始化资源密集型的情况下有用，而且在代码中可能不会使用某个属性时也很有用。
此外，延迟属性默认是线程安全的，这在并发环境中尤其有利。

For more information, see [Lazy properties](delegated-properties.md#lazy-properties).
<br />有关更多信息，请参阅[延迟属性](delegated-properties.md#lazy-properties)。

#### Observable properties
<br />可观察属性

To monitor whether the value of a property changes, use an observable property. An observable property is useful when
you want to detect a change in the property value and use this knowledge to trigger a reaction. The standard library provides
the `Delegates` object for delegation.
<br />要监控属性值是否发生变化，请使用可观察属性。当您想要检测属性值的变化并利用此信息触发相应操作时，可观察属性非常有用。标准库提供了用于委托的 `Delegates` 对象。

To create an observable property, you must first import `kotlin.properties.Delegates.observable`. Then, use the `observable()` function
and provide it with a lambda expression to execute whenever the property changes. Just like with lazy properties, observable
properties use the [trailing lambda](kotlin-tour-functions.md#trailing-lambdas) syntax to pass the lambda expression.
<br />要创建可观察属性，首先必须导入 `kotlin.properties.Delegates.observable`。然后，使用 `observable()` 函数，
并为其提供一个 lambda 表达式，以便在属性更改时执行。与惰性属性类似，可观察属性也使用 [尾随 lambda](kotlin-tour-functions.md#trailing-lambdas) 语法来传递 lambda 表达式。

For example:
<br />例如：

```kotlin
import kotlin.properties.Delegates.observable

class Thermostat {
    var temperature: Double by observable(20.0) { _, old, new ->
        if (new > 25) {
            println("Warning: Temperature is too high! ($old°C -> $new°C)")
        } else {
            println("Temperature updated: $old°C -> $new°C")
        }
    }
}

fun main() {
    val thermostat = Thermostat()
    thermostat.temperature = 22.5
    // Temperature updated: 20.0°C -> 22.5°C

    thermostat.temperature = 27.0
    // Warning: Temperature is too high! (22.5°C -> 27.0°C)
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-observable"}

In this example:
<br />在这个例子中：

* There is a `Thermostat` class that contains an observable property: `temperature`.
  <br />有一个名为 `Thermostat` 的类，其中包含一个可观察的属性：`temperature`。
* The `observable()` function accepts `20.0` as a parameter and uses it to initialize the property.
  <br />`observable()` 函数接受 `20.0` 作为参数，并使用该参数初始化属性。
* The lambda expression provided to the `observable()` function:
  <br />传递给 `observable()` 函数的 lambda 表达式：
  * Has three parameters:
    <br />它包含三个参数：
    * `_`, which refers to the property itself.
      <br />`_`，指的是属性本身。
    * `old`, which is the old value of the property.
      <br />`old`，这是该属性的旧值。
    * `new`, which is the new value of the property.
      <br />`new`，即该属性的新值。
  * Checks if the `new` parameter is greater than `25` and, depending on the result, prints a string to console.
    <br />检查 `new` 参数是否大于 `25`，并根据结果向控制台打印字符串。
* The `main()` function:
  <br />`main()` 函数：
  * Creates an instance of the `Thermostat` class called `thermostat`.
    <br />创建一个名为 `thermostat` 的 `Thermostat` 类的实例。
  * Updates the value of the `temperature` property of the instance to `22.5`, which triggers a print statement with a temperature update.
    <br />将实例的 `temperature` 属性值更新为 `22.5`，这将触发一个打印语句，更新温度。
  * Updates the value of the `temperature` property of the instance to `27.0`, which triggers a print statement with a warning.
    <br />将实例的 `temperature` 属性值更新为 `27.0`，这将触发一个带有警告的打印语句。

Observable properties are useful not only for logging and debugging purposes. You can also use them for use cases like
updating a UI or to perform additional checks, like verifying the validity of data.
<br />可观察属性不仅可用于日志记录和调试，还可以用于其他用例，例如： 更新用户界面或执行其他检查，例如验证数据的有效性。

For more information, see [Observable properties](delegated-properties.md#observable-properties).
<br />有关更多信息，请参阅[可观察属性](delegated-properties.md#observable-properties)。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true" id="properties-exercise-1"}

You manage an inventory system at a bookstore. The inventory is stored in a list where each item represents the quantity
of a specific book. For example, `listOf(3, 0, 7, 12)` means the store has 3 copies of the first book, 0 of the second,
7 of the third, and 12 of the fourth.
<br />你负责一家书店的库存系统。库存信息存储在一个列表中，其中每个项目代表特定书籍的数量。
例如，`listOf(3, 0, 7, 12)` 表示书店有 3 本第一本书，0 本第二本书，7 本第三本书，12 本第四本书。

Write a function called `findOutOfStockBooks()` that returns a list of indices for all the books that are out of stock.
<br />编写一个名为 `findOutOfStockBooks()` 的函数，该函数返回一个包含所有缺货书籍索引的列表。

<deflist collapsible="true">
    <def title="Hint 1">
        Use the <a href="https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/indices.html"><code>indices</code></a> extension property from the standard library.
        <br />使用标准库中的<a href="https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/indices.html"><code>indices</code></a>扩展属性。
    </def>
</deflist>

<deflist collapsible="true">
    <def title="Hint 2">
        You can use the <a href="https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/build-list.html"><code>buildList()</code></a> function to create and manage a list instead of manually creating and returning a mutable list. The <code>buildList()</code> function uses a lambda with a receiver, which you learned about in earlier chapters.
        <br />您可以使用 <a href="https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/build-list.html"><code>buildList()</code></a> 函数来创建和管理列表，而无需手动创建和返回可变列表。<code>buildList()</code> 函数使用带有接收器的 lambda 表达式，您在前面的章节中已经学习过相关内容。
    </def>
</deflist>

|--|--|

```kotlin
fun findOutOfStockBooks(inventory: List<Int>): List<Int> {
    // Write your code here
}

fun main() {
    val inventory = listOf(3, 0, 7, 0, 5)
    println(findOutOfStockBooks(inventory))
    // [1, 3]
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-exercise-1"}

|---|---|
```kotlin
fun findOutOfStockBooks(inventory: List<Int>): List<Int> {
    val outOfStockIndices = mutableListOf<Int>()
    for (index in inventory.indices) {
        if (inventory[index] == 0) {
            outOfStockIndices.add(index)
        }
    }
    return outOfStockIndices
}

fun main() {
    val inventory = listOf(3, 0, 7, 0, 5)
    println(findOutOfStockBooks(inventory))
    // [1, 3]
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 1" id="kotlin-tour-properties-solution-1-1"}

|---|---|
```kotlin
fun findOutOfStockBooks(inventory: List<Int>): List<Int> = buildList {
    for (index in inventory.indices) {
        if (inventory[index] == 0) {
            add(index)
        }
    }
}

fun main() {
    val inventory = listOf(3, 0, 7, 0, 5)
    println(findOutOfStockBooks(inventory))
    // [1, 3]
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution 2" id="kotlin-tour-properties-solution-1-2"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true" id="properties-exercise-2"}

You have a travel app that needs to display distances in both kilometers and miles. Create an extension property for the
`Double` type called `asMiles` to convert a distance in kilometers to miles:
<br />你有一个旅行应用，需要同时显示公里和英里两种单位的距离。请为 `Double` 类型创建一个名为 `asMiles` 的扩展属性，用于将公里数转换为英里数：

> The formula to convert kilometers to miles is `miles = kilometers * 0.621371`.
> <br />公里转换为英里的公式是 `英里 = 公里 * 0.621371`。
>
{style="note"}

<deflist collapsible="true">
    <def title="Hint">
        Remember that extension properties need a custom <code>get()</code> function.
        <br />请记住，扩展属性需要自定义的 <code>get()</code> 函数。
    </def>
</deflist>

|---|---|

```kotlin
val // Write your code here

fun main() {
    val distanceKm = 5.0
    println("$distanceKm km is ${distanceKm.asMiles} miles")
    // 5.0 km is 3.106855 miles

    val marathonDistance = 42.195
    println("$marathonDistance km is ${marathonDistance.asMiles} miles")
    // 42.195 km is 26.218757 miles
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-exercise-2"}

|---|---|
```kotlin
val Double.asMiles: Double
    get() = this * 0.621371

fun main() {
    val distanceKm = 5.0
    println("$distanceKm km is ${distanceKm.asMiles} miles")
    // 5.0 km is 3.106855 miles

    val marathonDistance = 42.195
    println("$marathonDistance km is ${marathonDistance.asMiles} miles")
    // 42.195 km is 26.218757 miles
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-properties-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true" id="properties-exercise-3"}

You have a system health checker that can determine the state of a cloud system. However, the two functions it can run 
to perform a health check are performance intensive. Use lazy properties to initialize the checks so that the expensive
functions are only run when needed:
<br />您有一个系统健康检查器，可以确定云系统的状态。但是，它用于执行健康检查的两个函数性能消耗很大。
请使用延迟加载属性来初始化这些检查，以便仅在需要时才运行这些耗时的函数：

|---|---|

```kotlin
fun checkAppServer(): Boolean {
    println("Performing application server health check...")
    return true
}

fun checkDatabase(): Boolean {
    println("Performing database health check...")
    return false
}

fun main() {
    // Write your code here

    when {
        isAppServerHealthy -> println("Application server is online and healthy")
        isDatabaseHealthy -> println("Database is healthy")
        else -> println("System is offline")
    }
    // Performing application server health check...
    // Application server is online and healthy
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-exercise-3"}

|---|---|
```kotlin
fun checkAppServer(): Boolean {
    println("Performing application server health check...")
    return true
}

fun checkDatabase(): Boolean {
    println("Performing database health check...")
    return false
}

fun main() {
    val isAppServerHealthy by lazy { checkAppServer() }
    val isDatabaseHealthy by lazy { checkDatabase() }

    when {
        isAppServerHealthy -> println("Application server is online and healthy")
        isDatabaseHealthy -> println("Database is healthy")
        else -> println("System is offline")
    }
   // Performing application server health check...
   // Application server is online and healthy
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-properties-solution-3"}

### Exercise 4 {initial-collapse-state="collapsed" collapsible="true" id="properties-exercise-4"}

You're building a simple budget tracker app. The app needs to observe changes to the user's remaining budget and notify
them whenever it goes below a certain threshold. You have a `Budget` class that is initialized with a `totalBudget` property
that contains the initial budget amount. Within the class, create an observable property called `remainingBudget` that prints:
<br />您正在构建一个简单的预算跟踪应用程序。应用需要观察用户剩余预算的变化并通知
每当它低于某个阈值时。您有一个使用`totalBudget`属性初始化的`Budget`类
其中包含初始预算金额。在类中，创建一个名为`remainingBudget`的可观察属性，该属性打印：

* A warning when the value is lower than 20% of the initial budget.
  <br />当实际值低于初始预算的 20% 时发出警告。
* An encouraging message when the budget is increased from the previous value.
  <br />预算较之前有所增加，这是一个令人鼓舞的消息。

|---|---|

```kotlin
import kotlin.properties.Delegates.observable

class Budget(val totalBudget: Int) {
    var remainingBudget: Int // Write your code here
}

fun main() {
    val myBudget = Budget(totalBudget = 1000)
    myBudget.remainingBudget = 800
    myBudget.remainingBudget = 150
    // Warning: Your remaining budget (150) is below 20% of your total budget.
    myBudget.remainingBudget = 50
    // Warning: Your remaining budget (50) is below 20% of your total budget.
    myBudget.remainingBudget = 300
    // Good news: Your remaining budget increased to 300.
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-properties-exercise-4"}

|---|---|
```kotlin
import kotlin.properties.Delegates.observable

class Budget(val totalBudget: Int) {
    var remainingBudget: Int by observable(totalBudget) { _, oldValue, newValue ->
        if (newValue < totalBudget * 0.2) {
            println("Warning: Your remaining budget ($newValue) is below 20% of your total budget.")
        } else if (newValue > oldValue) {
            println("Good news: Your remaining budget increased to $newValue.")
        }
    }
}

fun main() {
    val myBudget = Budget(totalBudget = 1000)
    myBudget.remainingBudget = 800
    myBudget.remainingBudget = 150
    // Warning: Your remaining budget (150) is below 20% of your total budget.
    myBudget.remainingBudget = 50
    // Warning: Your remaining budget (50) is below 20% of your total budget.
    myBudget.remainingBudget = 300
    // Good news: Your remaining budget increased to 300.
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-properties-solution-4"}

## Next step
下一步

[Intermediate: Null safety](kotlin-tour-intermediate-null-safety.md)
<br />[中级：空安全](kotlin-tour-intermediate-null-safety.md)
