[//]: # (title: Visibility modifiers)

Classes, objects, interfaces, constructors, and functions, as well as properties and their setters, can have *visibility modifiers*.
Getters always have the same visibility as their properties.
<br />类、对象、接口、构造函数、函数以及属性及其 setter 方法都可以拥有*可见性修饰符*。
getter 方法始终与其对应的属性具有相同的可见性。

There are four visibility modifiers in Kotlin: `private`, `protected`, `internal`, and `public`.
The default visibility is `public`.
<br />Kotlin 中有四种可见性修饰符：`private`、`protected`、`internal` 和 `public`。
默认可见性为 `public`。

On this page, you'll learn how the modifiers apply to different types of declaring scopes.
<br />在本页中，您将学习修饰符如何应用于不同类型的声明作用域。

## Packages
包

Functions, properties, classes, objects, and interfaces can be declared at the "top-level" directly inside a package:
<br />函数、属性、类、对象和接口可以直接在包的“顶层”声明：

```kotlin
// file name: example.kt
package foo

fun baz() { ... }
class Bar { ... }
```

* If you don't use a visibility modifier, `public` is used by default, which means that your declarations will be
  visible everywhere.
  <br />如果您不使用可见性修饰符，则默认使用 `public`，这意味着您的声明将在任何地方都可见。
* If you mark a declaration as `private`, it will only be visible inside the file that contains the declaration.
  <br />如果将声明标记为 `private` ，则该声明仅在包含该声明的文件内可见。
* If you mark it as `internal`, it will be visible everywhere in the same [module](#modules).
  <br />如果将其标记为 `internal`，则在同一个[模块](#modules)中的任何地方都可见。
* The `protected` modifier is not available for top-level declarations.
  <br />`protected` 修饰符不可用于顶级声明。

>To use a visible top-level declaration from another package, you should [import](packages.md#imports) it.
> <br />要使用来自另一个包的可见顶级声明，您应该[导入](packages.md#imports)它。
>
{style="note"}

Examples:
<br />例如：

```kotlin
// file name: example.kt
package foo

private fun foo() { ... } // visible inside example.kt

public var bar: Int = 5 // property is visible everywhere
    private set         // setter is visible only in example.kt
    
internal val baz = 6    // visible inside the same module
```

## Class members
类成员

For members declared inside a class:
<br />对于在类中声明的成员：

* `private` means that the member is visible inside this class only (including all its members).
  <br />`private` 表示该成员仅在该类内部可见（包括该类的所有成员）。
* `protected` means that the member has the same visibility as one marked as `private`, but that it is also visible in subclasses.
  <br />`protected` 表示该成员与标记为 `private` 的成员具有相同的可见性，但它在子类中也可见。
* `internal` means that any client *inside this module* who sees the declaring class sees its `internal` members.
  <br />`internal`意味着任何*在此模块内部*的客户端，只要看到声明该类，就能看到它的 `internal` 成员。
* `public` means that any client who sees the declaring class sees its `public` members.
  <br />`public` 表示任何看到声明类的客户端都可以看到其 `public` 成员。

> In Kotlin, an outer class does not see private members of its inner classes.
> <br />在 Kotlin 中，外部类看不到其内部类的私有成员。
>
{style="note"}

If you override a `protected` or an `internal` member and do not specify the visibility explicitly, the overriding member
will also have the same visibility as the original.
<br />如果您覆盖了受保护的成员或内部成员，并且没有显式指定可见性，则覆盖的成员也将具有与原始成员相同的可见性。

Examples:
<br />例如：

```kotlin
open class Outer {
    private val a = 1
    protected open val b = 2
    internal open val c = 3
    val d = 4  // public by default
    
    protected class Nested {
        public val e: Int = 5
    }
}

class Subclass : Outer() {
    // a is not visible
    // b, c and d are visible
    // Nested and e are visible

    override val b = 5   // 'b' is protected
    override val c = 7   // 'c' is internal
}

class Unrelated(o: Outer) {
    // o.a, o.b are not visible
    // o.c and o.d are visible (same module)
    // Outer.Nested is not visible, and Nested::e is not visible either 
}
```

### Constructors
构造函数

Use the following syntax to specify the visibility of the primary constructor of a class:
<br />使用以下语法指定类的主构造函数的可见性：

> You need to add an explicit `constructor` keyword.
> <br />你需要添加一个显式的 `constructor` 关键字。
>
{style="note"}

```kotlin
class C private constructor(a: Int) { ... }
```

Here the constructor is `private`. By default, all constructors are `public`, which effectively
amounts to them being visible everywhere the class is visible (this means that a constructor of an `internal` class is only
visible within the same module).
<br />这里的构造函数是 `private` 的。默认情况下，所有构造函数都是 `public` 的，这实际上
意味着它们在类可见的任何地方都可见（这意味着 `internal` 类的构造函数仅在同一个模块内可见）。

For sealed classes, constructors are `protected` by default. For more information, see [Sealed classes](sealed-classes.md#constructors).
<br />对于密封类，构造函数默认是受保护的。更多信息，请参阅[密封类](sealed-classes.md#constructors)。

### Local declarations
局部声明

Local variables, functions, and classes can't have visibility modifiers.
<br />局部变量、函数和类不能有可见性修饰符。

## Modules
模块

The `internal` visibility modifier means that the member is visible within the same module. More specifically,
a module is a set of Kotlin files compiled together, for example:
<br />`internal` 可见性修饰符表示该成员在同一个模块内可见。更具体地说，模块是一组编译在一起的 Kotlin 文件，例如：

* An IntelliJ IDEA module.
  <br />IntelliJ IDEA 模块。
* A Maven project.
  <br />一个 Maven 项目。
* A Gradle source set (with the exception that the `test` source set can access the internal declarations of `main`).
  <br />Gradle 源集（但 `test` 源集可以访问 `main` 的内部声明）。
