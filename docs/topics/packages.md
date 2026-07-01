[//]: # (title: Packages and imports)

A source file may start with a package declaration:
<br />源文件可以以包声明开头：

```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*...*/ }

// ...
```

All the contents, such as classes and functions, of the source file are included in this package.
So, in the example above, the full name of `printMessage()` is `org.example.printMessage`,
and the full name of `Message` is `org.example.Message`. 
<br />源文件的所有内容，例如类和函数，都包含在此包中。
因此，在上面的示例中，`printMessage()` 的完整名称是 `org.example.printMessage`，
而 `Message` 的完整名称是 `org.example.Message`。

If the package is not specified, the contents of such a file belong to the _default_ package with no name.
<br />如果未指定包，则此类文件的内容属于没有名称的 _default_ 包。

## Default imports
默认导入

A number of packages are imported into every Kotlin file by default:
<br />默认情况下，每个 Kotlin 文件都会导入一些包：

- [kotlin.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/index.html)
- [kotlin.annotation.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/index.html)
- [kotlin.collections.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/index.html)
- [kotlin.comparisons.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.comparisons/index.html)
- [kotlin.io.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/index.html)
- [kotlin.ranges.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.ranges/index.html)
- [kotlin.sequences.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/index.html)
- [kotlin.text.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/index.html)

Additional packages are imported depending on the target platform:
<br />根据目标平台的不同，还会导入其他软件包：

- JVM:
  - java.lang.*
  - [kotlin.jvm.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.jvm/index.html)

- JS:    
  - [kotlin.js.*](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.js/index.html)

## Imports
导入

Apart from the default imports, each file may contain its own `import` directives.
<br />除了默认导入之外，每个文件还可以包含自己的 `import` 指令。

You can import either a single name:
<br />您可以导入单个名称：

```kotlin
import org.example.Message // Message is now accessible without qualification
```

or all the accessible contents of a scope: package, class, object, and so on:
<br />或者作用域内所有可访问的内容：包、类、对象等等：

```kotlin
import org.example.* // everything in 'org.example' becomes accessible
```

If there is a name clash, you can disambiguate by using `as` keyword to locally rename the clashing entity:
<br />如果出现名称冲突，可以使用 `as` 关键字在本地重命名冲突的实体来消除歧义：

```kotlin
import org.example.Message // Message is accessible
import org.test.Message as TestMessage // TestMessage stands for 'org.test.Message'
```

The `import` keyword is not restricted to importing classes; you can also use it to import other declarations:
<br />`import` 关键字不仅限于导入类；您还可以使用它来导入其他声明：

  * top-level functions and properties
    <br />顶级函数和属性
  * functions and properties declared in [object declarations](object-declarations.md#object-declarations-overview)
    <br />在[对象声明](object-declarations.md#object-declarations-overview)中声明的函数和属性
  * [enum constants](enum-classes.md)
    <br />[枚举常量](enum-classes.md)

## Visibility of top-level declarations
顶级声明的可见性

If a top-level declaration is marked `private`, it is private to the file it's declared in (see [Visibility modifiers](visibility-modifiers.md)).
<br />如果顶级声明标记为“private”，则它对声明它的文件是私有的（参见[可见性修饰符](visibility-modifiers.md)）。
