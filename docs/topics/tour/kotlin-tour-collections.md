[//]: # (title: Collections)

<no-index/>

<tldr>
    <p><img src="icon-1-done.svg" width="20" alt="First step" /> <a href="kotlin-tour-hello-world.md">Hello world</a><br />
        <img src="icon-2-done.svg" width="20" alt="Second step" /> <a href="kotlin-tour-basic-types.md">Basic types</a><br />
        <img src="icon-3.svg" width="20" alt="Third step" /> <strong>Collections</strong><br />
        <img src="icon-4-todo.svg" width="20" alt="Fourth step" /> <a href="kotlin-tour-control-flow.md">Control flow</a><br />
        <img src="icon-5-todo.svg" width="20" alt="Fifth step" /> <a href="kotlin-tour-functions.md">Functions</a><br />
        <img src="icon-6-todo.svg" width="20" alt="Sixth step" /> <a href="kotlin-tour-classes.md">Classes</a><br />
        <img src="icon-7-todo.svg" width="20" alt="Final step" /> <a href="kotlin-tour-null-safety.md">Null safety</a></p>
</tldr>

When programming, it is useful to be able to group data into structures for later processing. Kotlin provides collections
for exactly this purpose.
<br />在编程中，将数据分组到结构中以便后续处理非常有用。Kotlin 提供了集合（Collection）
正是为此目的而提供的。

Kotlin has the following collections for grouping items:
<br />Kotlin 提供了以下用于对项目进行分组的集合：

| **Collection type** | **Description**                                                         |
|---------------------|-------------------------------------------------------------------------|
| Lists               | Ordered collections of items                                            |
| Sets                | Unique unordered collections of items                                   |
| Maps                | Sets of key-value pairs where keys are unique and map to only one value |

| **集合类型** | **描述**                 |
|----------|------------------------|
| Lists    | 已排序的物品集合               |
| Sets     | 独特的无序物品集合              |
| Maps     | 键值对集合，其中键是唯一的且仅映射到一个值。 |

Each collection type can be mutable or read only.
<br />每种集合类型都可以是可变的，也可以是只读的。

## List

Lists store items in the order that they are added, and allow for duplicate items. 
<br />列表按添加顺序存储项目，并允许重复项目。

To create a read-only list ([`List`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/)), use the 
[`listOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/list-of.html) function.
<br />要创建只读列表（[`List`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/)），请使用
[`listOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/list-of.html) 函数。

To create a mutable list ([`MutableList`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list.html)),
use the [`mutableListOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-list-of.html) function.
<br />要创建可变列表（[`MutableList`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list.html)），
请使用[`mutableListOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-list-of.html)函数。


When creating lists, Kotlin can infer the type of items stored. To declare the type explicitly, add the type
within angled brackets `<>` after the list declaration:
<br />创建列表时，Kotlin 可以推断存储项的类型。要显式声明类型，请在列表声明后，用尖括号 `<>` 添加类型：

```kotlin
fun main() { 
//sampleStart
    // Read only list
    val readOnlyShapes = listOf("triangle", "square", "circle")
    println(readOnlyShapes)
    // [triangle, square, circle]
    
    // Mutable list with explicit type declaration
    val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
    println(shapes)
    // [triangle, square, circle]
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-lists-declaration"}

> To prevent unwanted modifications, you can create a read-only view of a mutable list by assigning it to a `List`:
> <br />为防止意外修改，您可以通过将可变列表赋值给一个 `List` 对象来创建该列表的只读视图：
> 
> ```kotlin
>     val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
>     val shapesLocked: List<String> = shapes
> ```
> This is also called **casting**.
> <br />这也称为**铸造**。
> 
{style="tip"}

Lists are ordered so to access an item in a list, use the [indexed access operator](operator-overloading.md#indexed-access-operator) `[]`:
<br />列表是有序的，因此要访问列表中的项，请使用[索引访问运算符](operator-overloading.md#indexed-access-operator) `[]`：

```kotlin
fun main() { 
//sampleStart
    val readOnlyShapes = listOf("triangle", "square", "circle")
    println("The first item in the list is: ${readOnlyShapes[0]}")
    // The first item in the list is: triangle
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-list-access"}

To get the first or last item in a list, use [`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html)
and [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html) functions respectively:
<br />要获取列表中的第一个或最后一个元素，请分别使用 [`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html) 函数
以及 [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html) 函数：

```kotlin
fun main() { 
//sampleStart
    val readOnlyShapes = listOf("triangle", "square", "circle")
    println("The first item in the list is: ${readOnlyShapes.first()}")
    // The first item in the list is: triangle
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-list-first"}

> [`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html) and [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html)
> functions are examples of **extension** functions. To call an extension function on an object, write the function name 
> after the object appended with a period `.` 
> <br />[`.first()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first.html) 和 [`.last()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/last.html) 函数是**扩展函数**的示例。要调用对象的扩展函数，请在对象名称后添加函数名，并在函数名后加上句点 `.`。
> 
> Extension functions are covered in detail [in the intermediate tour](kotlin-tour-intermediate-extension-functions.md#extension-functions).
> For now, you only need to know how to call them. 
> <br />扩展函数在[中级教程](kotlin-tour-intermediate-extension-functions.md#extension-functions)中有详细介绍。
> 现在，你只需要知道如何调用它们即可。
> 
{style="note"}

To get the number of items in a list, use the [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html)
function:
<br />要获取列表中的元素数量，请使用 [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) 函数：

```kotlin
fun main() { 
//sampleStart
    val readOnlyShapes = listOf("triangle", "square", "circle")
    println("This list has ${readOnlyShapes.count()} items")
    // This list has 3 items
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-list-count"}

To check that an item is in a list, use the [`in` operator](operator-overloading.md#in-operator):
<br />要检查某个项目是否在列表中，请使用 [`in` 运算符](operator-overloading.md#in-operator):

```kotlin
fun main() {
//sampleStart
    val readOnlyShapes = listOf("triangle", "square", "circle")
    println("circle" in readOnlyShapes)
    // true
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-list-in"}

To add or remove items from a mutable list, use [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/add.html)
and [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) functions respectively:
<br />要向可变列表中添加或删除元素，请分别使用 [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/add.html) 函数和 [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) 函数：

```kotlin
fun main() { 
//sampleStart
    val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")
    // Add "pentagon" to the list
    shapes.add("pentagon") 
    println(shapes)  
    // [triangle, square, circle, pentagon]

    // Remove the first "pentagon" from the list
    shapes.remove("pentagon") 
    println(shapes)  
    // [triangle, square, circle]
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-list-add-remove"}

## Set

Whereas lists are ordered and allow duplicate items, sets are **unordered** and only store **unique** items.
<br />列表是有序的，允许重复项，而集合是**无序的**，只存储**唯一**项。

To create a read-only set ([`Set`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-set/)), use the 
[`setOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set-of.html) function.
<br />要创建只读集合（[`Set`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-set/)），请使用
[`setOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/set-of.html) 函数。

To create a mutable set ([`MutableSet`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/)),
use the [`mutableSetOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-set-of.html) function.
<br />要创建一个可变集合（[`MutableSet`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/)），
请使用[`mutableSetOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-set-of.html)函数。

When creating sets, Kotlin can infer the type of items stored. To declare the type explicitly, add the type
within angled brackets `<>` after the set declaration:
<br />创建集合时，Kotlin 可以推断存储项的类型。要显式声明类型，请在集合声明后，用尖括号 `<>` 添加类型：

```kotlin
fun main() {
//sampleStart
    // Read-only set
    val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
    // Mutable set with explicit type declaration
    val fruit: MutableSet<String> = mutableSetOf("apple", "banana", "cherry", "cherry")
    
    println(readOnlyFruit)
    // [apple, banana, cherry]
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-sets-declaration"}

You can see in the previous example that because sets only contain unique elements, the duplicate `"cherry"` item is dropped.
<br />从前面的例子中可以看出，由于集合只包含唯一元素，因此重复的`"cherry"`项会被删除。

> To prevent unwanted modifications, you can create a read-only view of a mutable set by assigning it to a `Set`:
> <br />为防止意外修改，您可以通过将可变集合赋值给一个 `Set` 对象来创建该集合的只读视图：
> 
> ```kotlin
>     val fruit: MutableSet<String> = mutableSetOf("apple", "banana", "cherry", "cherry")
>     val fruitLocked: Set<String> = fruit
> ```
>
{style="tip"}

> As sets are **unordered**, you can't access an item at a particular index.
> <br />由于集合是**无序的**，因此无法通过特定索引访问元素。
> 
{style="note"}

To get the number of items in a set, use the [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html)
function:
<br />要获取集合中元素的数量，请使用 [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) 函数：

```kotlin
fun main() { 
//sampleStart
    val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
    println("This set has ${readOnlyFruit.count()} items")
    // This set has 3 items
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-set-count"}

To check that an item is in a set, use the [`in` operator](operator-overloading.md#in-operator):
<br />要检查某个元素是否在集合中，请使用 [`in` 运算符](operator-overloading.md#in-operator):

```kotlin
fun main() {
//sampleStart
    val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")
    println("banana" in readOnlyFruit)
    // true
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-set-in"}

To add or remove items from a mutable set, use [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/add.html)
and [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) functions respectively:
<br />要向可变集合中添加或删除元素，请分别使用 [`.add()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-set/add.html) 函数
以及 [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) 函数：

```kotlin
fun main() { 
//sampleStart
    val fruit: MutableSet<String> = mutableSetOf("apple", "banana", "cherry", "cherry")
    fruit.add("dragonfruit")    // Add "dragonfruit" to the set
    println(fruit)              // [apple, banana, cherry, dragonfruit]
    
    fruit.remove("dragonfruit") // Remove "dragonfruit" from the set
    println(fruit)              // [apple, banana, cherry]
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-set-add-remove"}

## Map

Maps store items as key-value pairs. You access the value by referencing the key. You can imagine a map like a food menu.
You can find the price (value), by finding the food (key) you want to eat. Maps are useful if you want to look up a value
without using a numbered index, like in a list.
<br />映射表以键值对的形式存储数据。您可以通过引用键来访问值。您可以将映射表想象成一份食物菜单。
您可以通过找到想吃的食物（键）来找到价格（值）。如果您想查找某个值，但又不想使用像列表那样的编号索引，映射表就非常有用。

> * Every key in a map must be unique so that Kotlin can understand which value you want to get. 
> <br />映射中的每个键都必须是唯一的，这样 Kotlin 才能知道你想获取哪个值。
> * You can have duplicate values in a map.
> <br />映射中可以有重复值。
>
{style="note"}

To create a read-only map ([`Map`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/)), use the 
[`mapOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-of.html) function.
<br />要创建只读映射（[`Map`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/)），请使用
[`mapOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-of.html) 函数。

To create a mutable map ([`MutableMap`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-map/)),
use the [`mutableMapOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-map-of.html) function.
<br />要创建可变映射（[`MutableMap`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-map/)），
请使用[`mutableMapOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/mutable-map-of.html)函数。

When creating maps, Kotlin can infer the type of items stored. To declare the type explicitly, add the types
of the keys and values within angled brackets `<>` after the map declaration. For example: `MutableMap<String, Int>`.
The keys have type `String` and the values have type `Int`.
<br />创建映射时，Kotlin 可以推断存储项的类型。要显式声明类型，请在映射声明后用尖括号 `<>` 添加键和值的类型。
例如：`MutableMap<String, Int>`。 键的类型为 `String`，值的类型为 `Int`。

The easiest way to create maps is to use [`to`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/to.html) between each 
key and its related value:
<br />创建映射的最简单方法是在每个键及其对应值之间使用 [`to`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/to.html) 函数：

```kotlin
fun main() {
//sampleStart
    // Read-only map
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println(readOnlyJuiceMenu)
    // {apple=100, kiwi=190, orange=100}

    // Mutable map with explicit type declaration
    val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println(juiceMenu)
    // {apple=100, kiwi=190, orange=100}
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-maps-declaration"}

> To prevent unwanted modifications, you can create a read-only view of a mutable map by assigning it to a `Map`:
> <br />为防止意外修改，您可以通过将可变映射分配给 `Map` 来创建其只读视图：
> 
> ```kotlin
>     val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
>     val juiceMenuLocked: Map<String, Int> = juiceMenu
> ```
>
{style="tip"}

To access a value in a map, use the [indexed access operator](operator-overloading.md#indexed-access-operator) `[]` with
its key:
<br />要访问映射中的值，请使用[索引访问运算符](operator-overloading.md#indexed-access-operator) `[]`，并传入其键：

```kotlin
fun main() {
//sampleStart
    // Read-only map
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println("The value of apple juice is: ${readOnlyJuiceMenu["apple"]}")
    // The value of apple juice is: 100
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-access"}

> If you try to access a key-value pair with a key that doesn't exist in a map, you see a `null` value:
> <br />如果尝试使用映射中不存在的键访问键值对，则会看到 `null` 值：
>
> ```kotlin
> fun main() {
> //sampleStart
>     // Read-only map
>     val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
>     println("The value of pineapple juice is: ${readOnlyJuiceMenu["pineapple"]}")
>     // The value of pineapple juice is: null
> //sampleEnd
> }
> ```
> {kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-no-key" validate="false"}
> 
> This tour explains null values later in the [Null safety](kotlin-tour-null-safety.md) chapter.
> <br />本教程将在[空值安全性](kotlin-tour-null-safety.md)章节中解释空值。
> 
{style="note"}

You can also use the [indexed access operator](operator-overloading.md#indexed-access-operator) `[]` to add items to a mutable map:
<br />您还可以使用[索引访问运算符](operator-overloading.md#indexed-access-operator) `[]` 向可变映射添加项：

```kotlin
fun main() {
//sampleStart
    val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    juiceMenu["coconut"] = 150 // Add key "coconut" with value 150 to the map
    println(juiceMenu)
    // {apple=100, kiwi=190, orange=100, coconut=150}
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-add-item"}

To remove items from a mutable map, use the [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) 
function:
<br />要从可变映射中移除元素，请使用 [`.remove()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/remove.html) 函数：

```kotlin
fun main() {
//sampleStart
    val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    juiceMenu.remove("orange")    // Remove key "orange" from the map
    println(juiceMenu)
    // {apple=100, kiwi=190}
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-put-remove"}

To get the number of items in a map, use the [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html)
function:
<br />要获取映射中的元素数量，请使用 [`.count()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/count.html) 函数：

```kotlin
fun main() {
//sampleStart
    // Read-only map
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println("This map has ${readOnlyJuiceMenu.count()} key-value pairs")
    // This map has 3 key-value pairs
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-count"}

To check if a specific key is already included in a map, use the [`.containsKey()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/contains-key.html)
function:
<br />要检查某个特定键是否已包含在映射中，请使用 [`.containsKey()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/contains-key.html) 函数。

```kotlin
fun main() {
//sampleStart
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println(readOnlyJuiceMenu.containsKey("kiwi"))
    // true
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-contains-keys"}

To obtain a collection of the keys or values of a map, use the [`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html)
and [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html) properties respectively:
<br />要获取映射的键或值的集合，请分别使用 [`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html)
和 [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html) 属性：

```kotlin
fun main() {
//sampleStart
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println(readOnlyJuiceMenu.keys)
    // [apple, kiwi, orange]
    println(readOnlyJuiceMenu.values)
    // [100, 190, 100]
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-keys-values"}

> [`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html) and [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html)
> are examples of **properties** of an object. To access the property of an object, write the property name
> after the object appended with a period `.`
> <br />[`keys`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/keys.html) 和 [`values`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-map/values.html) 
> 是对象的**属性**示例。要访问对象的属性，请在对象名称后添加属性名，并在其后加一个句点 `.`。
> `keys` 和 `values`
>
> Properties are discussed in more detail in the [Classes](kotlin-tour-classes.md) chapter.
> At this point in the tour, you only need to know how to access them.
> <br />属性的详细讨论请参见[类](kotlin-tour-classes.md)章节。
> 在本教程的这一部分，您只需要了解如何访问它们即可。
>
{style="note"}

To check that a key or value is in a map, use the [`in` operator](operator-overloading.md#in-operator):
<br />要检查键或值是否在映射中，请使用 [`in` 运算符](operator-overloading.md#in-operator):

```kotlin
fun main() {
//sampleStart
    val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
    println("orange" in readOnlyJuiceMenu.keys)
    // true
    
    // Alternatively, you don't need to use the keys property
    println("orange" in readOnlyJuiceMenu)
    // true
    
    println(200 in readOnlyJuiceMenu.values)
    // false
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-map-in"}

For more information on what you can do with collections, see [Collections](collections-overview.md).
<br />有关集合的更多信息，请参阅[集合](collections-overview.md)。

Now that you know about basic types and how to manage collections, it's time to explore the [control flow](kotlin-tour-control-flow.md)
that you can use in your programs.
<br />现在你已经了解了基本类型以及如何管理集合，是时候探索你可以在程序中使用的[控制流](kotlin-tour-control-flow.md)了。

## Practice
实践

### Exercise 1 {initial-collapse-state="collapsed" collapsible="true"}

You have a list of “green” numbers and a list of “red” numbers. Complete the code to print how many numbers there
are in total.
<br />你有一份“绿色”数字列表和一份“红色”数字列表。请完成代码，输出总共有多少个数字。

|---|---|
```kotlin
fun main() {
    val greenNumbers = listOf(1, 4, 23)
    val redNumbers = listOf(17, 2)
    // Write your code here
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-collections-exercise-1"}

|---|---|
```kotlin
fun main() {
    val greenNumbers = listOf(1, 4, 23)
    val redNumbers = listOf(17, 2)
    val totalCount = greenNumbers.count() + redNumbers.count()
    println(totalCount)
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-collections-solution-1"}

### Exercise 2 {initial-collapse-state="collapsed" collapsible="true"}

You have a set of protocols supported by your server. A user requests to use a particular protocol. Complete the program
to check whether the requested protocol is supported or not (`isSupported` must be a Boolean value).
<br />您的服务器支持一组协议。用户请求使用某个特定协议。请完成以下程序，以检查所请求的协议是否受支持（`isSupported` 必须为布尔值）。

|---|---|
```kotlin
fun main() {
    val SUPPORTED = setOf("HTTP", "HTTPS", "FTP")
    val requested = "smtp"
    val isSupported = // Write your code here 
    println("Support for $requested: $isSupported")
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-collections-exercise-2"}

<deflist collapsible="true" id="kotlin-tour-collections-exercise-2-hint">
    <def title="Hint">
        Make sure that you check the requested protocol in upper case. You can use the <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html"><code>.uppercase()</code></a>
function to help you with this.
        <br />请确保您检查请求的协议是否全部大写。您可以使用 <a href="https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/uppercase.html"><code>.uppercase()</code></a> 
函数来帮助您完成此操作。
    </def>
</deflist>

|---|---|
```kotlin
fun main() {
    val SUPPORTED = setOf("HTTP", "HTTPS", "FTP")
    val requested = "smtp"
    val isSupported = requested.uppercase() in SUPPORTED
    println("Support for $requested: $isSupported")
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-collections-solution-2"}

### Exercise 3 {initial-collapse-state="collapsed" collapsible="true"}

Define a map that relates integer numbers from 1 to 3 to their corresponding spelling. Use this map to spell the given 
number.
<br />定义一个映射表，将 1 到 3 的整数与其对应的拼写关联起来。使用此映射表拼写给定的数字。

|---|---|
```kotlin
fun main() {
    val number2word = // Write your code here
    val n = 2
    println("$n is spelled as '${<Write your code here >}'")
}
```
{validate="false" kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="kotlin-tour-collections-exercise-3"}

|---|---|
```kotlin
fun main() {
    val number2word = mapOf(1 to "one", 2 to "two", 3 to "three")
    val n = 2
    println("$n is spelt as '${number2word[n]}'")
}
```
{initial-collapse-state="collapsed" collapsible="true" collapsed-title="Example solution" id="kotlin-tour-collections-solution-3"}

## Next step
接下来

[Control flow](kotlin-tour-control-flow.md)
<br />[控制流](kotlin-tour-control-flow.md)
