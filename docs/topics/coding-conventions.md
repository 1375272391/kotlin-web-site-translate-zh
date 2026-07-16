[//]: # (title: Coding conventions)

Commonly known and easy-to-follow coding conventions are vital for any programming language.
Here we provide guidelines on the code style and code organization for projects that use Kotlin.
<br />对于任何编程语言而言，遵循通用且易于理解的编码规范都至关重要。
本文将为使用 Kotlin 的项目提供代码风格和代码组织方面的指导原则。

## Configure style in IDE
在 IDE 中配置样式

Two most popular IDEs for Kotlin - [IntelliJ IDEA](https://www.jetbrains.com/idea/) and [Android Studio](https://developer.android.com/studio/)
provide powerful support for code styling. You can configure them to automatically format your code in consistence with
the given code style. 
<br />两款最流行的 Kotlin IDE——[IntelliJ IDEA](https://www.jetbrains.com/idea/) 和 [Android Studio](https://developer.android.com/studio/)
都提供了强大的代码风格支持。您可以配置它们，使其自动按照
指定的代码风格格式化代码。
 
### Apply the style guide
应用风格指南

1. Go to **Settings/Preferences | Editor | Code Style | Kotlin**.
   <br />前往**设置/首选项 | 编辑器 | 代码样式 | Kotlin**。
2. Click **Set from...**.
   <br />点击**从…设置**。
3. Select **Kotlin style guide** .
   <br />选择**Kotlin 风格指南**。

### Verify that your code follows the style guide
请检查您的代码是否符合风格指南。

1. Go to **Settings/Preferences | Editor | Inspections | General**.
   <br />前往**设置/首选项 | 编辑器 | 检查 | 常规**。
2. Switch on **Incorrect formatting** inspection.
Additional inspections that verify other issues described in the style guide (such as naming conventions) are enabled by default.
   <br />启用“格式错误”检查。默认情况下，还会启用其他检查，以验证样式指南中描述的其他问题（例如命名规则）。

<!-- Replace with an external link when the guide is moved -->

For more information, see the [Migrate to Kotlin code style with IntelliJ IDEA](code-style-migration-guide.md) guide.
<br />有关更多信息，请参阅 [使用 IntelliJ IDEA 迁移到 Kotlin 代码风格](code-style-migration-guide.md) 指南。

## Source code organization
源代码组织

### Directory structure
目录结构

In pure Kotlin projects, the recommended directory structure follows the package structure with
the common root package omitted. For example, if all the code in the project is in the `org.example.kotlin` package and its
subpackages, files with the `org.example.kotlin` package should be placed directly under the source root, and
files in `org.example.kotlin.network.socket` should be in the `network/socket` subdirectory of the source root.
<br />在纯 Kotlin 项目中，推荐的目录结构遵循包结构，但省略了公共根包。
例如，如果项目中的所有代码都在 `org.example.kotlin` 包及其子包中，那么 `org.example.kotlin` 包中的文件应该直接放在源代码根目录下，
而 `org.example.kotlin.network.socket` 包中的文件应该放在源代码根目录下的 `network/socket` 子目录中。

>On JVM: In projects where Kotlin is used together with Java, Kotlin source files should reside in the same 
>source root as the Java source files, and follow the same directory structure: each file should be stored in the 
>directory corresponding to each package statement.
> <br />在 JVM 上：在 Kotlin 与 Java 一起使用的项目中，Kotlin 源文件应与 Java 源文件位于同一源代码根目录，并遵循相同的目录结构：
> 每个文件应存储在与每个包语句对应的目录中。
>
{style="note"}

### Source file names
源文件名

If a Kotlin file contains a single class or interface (potentially with related top-level declarations), its name should be the same
as the name of the class, with the `.kt` extension appended. It applies to all types of classes and interfaces.
If a file contains multiple classes, or only top-level declarations, choose a name describing what the file contains, and name the file accordingly.
Use [upper camel case](https://en.wikipedia.org/wiki/Camel_case), where the first letter of each word is capitalized.
For example, `ProcessDeclarations.kt`.
<br />如果一个 Kotlin 文件只包含一个类或接口（可能包含相关的顶层声明），其文件名应与类名相同，并附加 `.kt` 扩展名。这适用于所有类型的类和接口。
如果一个文件包含多个类，或者只包含顶层声明，请选择一个能够描述文件内容的名称，并据此命名文件。
使用[大驼峰式命名法](https://en.wikipedia.org/wiki/Camel_case)，其中每个单词的首字母大写。
例如，`ProcessDeclarations.kt`。

The name of the file should describe what the code in the file does. Therefore, you should avoid using meaningless
words such as `Util` in file names.
<br />文件名应该描述文件中代码的功能。因此，应避免在文件名中使用诸如 `Util` 之类的无意义词语。

#### Multiplatform projects
多平台项目

In multiplatform projects, files with top-level declarations in platform-specific source sets should have a suffix
associated with the name of the source set. For example:
<br />在多平台项目中，包含平台特定源集中的顶级声明的文件应带有与源集名称关联的后缀。
例如：

* **jvm**Main/kotlin/Platform.**jvm**.kt
* **android**Main/kotlin/Platform.**android**.kt
* **ios**Main/kotlin/Platform.**ios**.kt

As for the common source set, files with top-level declarations should not have a suffix. For example, `commonMain/kotlin/Platform.kt`.
<br />至于公共源文件集，包含顶级声明的文件不应有后缀。例如，`commonMain/kotlin/Platform.kt`。

##### Technical details {initial-collapse-state="collapsed" collapsible="true"}
技术细节

We recommend following this file naming scheme in multiplatform projects due to JVM limitations: it doesn't allow
top-level members (functions, properties).
<br />由于 JVM 的限制，我们建议在多平台项目中遵循以下文件命名方案：它不允许使用顶级成员（函数、属性）。

To work around this, the Kotlin JVM compiler creates wrapper classes (so-called "file facades") that contain top-level
member declarations. File facades have an internal name derived from the file name.
<br />为了解决这个问题，Kotlin JVM 编译器会创建包装类（所谓的“文件外观”），其中包含顶层成员声明。
文件外观的内部名称源自文件名。

In turn, JVM doesn't allow several classes with the same fully qualified name (FQN). This might lead to situations when
a Kotlin project cannot be compiled to JVM:
<br />反过来，JVM 不允许存在多个具有相同完全限定名 (FQN) 的类。这可能会导致以下情况：
Kotlin 项目无法编译到 JVM：

```none
root
|- commonMain/kotlin/myPackage/Platform.kt // contains 'fun count() { }'
|- jvmMain/kotlin/myPackage/Platform.kt // contains 'fun multiply() { }'
```

Here both `Platform.kt` files are in the same package, so the Kotlin JVM compiler produces two file facades, both of which
have FQN `myPackage.PlatformKt`. This produces the "Duplicate JVM classes" error.
<br />这里两个 `Platform.kt` 文件位于同一个包中，因此 Kotlin JVM 编译器会生成两个文件外观，这两个外观都具有完全限定名 `myPackage.PlatformKt`。
这会导致 "Duplicate JVM classes" 错误。

The simplest way to avoid that is renaming one of the files according to the guideline above. This naming scheme helps
avoid clashes while retaining code readability.
<br />避免这种情况最简单的方法是根据上述准则重命名其中一个文件。
这种命名方案有助于避免冲突，同时保持代码的可读性。

> There are two scenarios where these recommendations may seem redundant, but we still advise to follow them:
> <br />在两种情况下，这些建议可能看起来多余，但我们仍然建议您遵循这些建议：
> 
> * Non-JVM platforms don't have issues with duplicating file facades. However, this naming scheme can help you keep
> file naming consistent.
> <br />非 JVM 平台不存在文件外观重复的问题。但是，这种命名方案可以帮助您保持文件命名的一致性。
> * On JVM, if source files don't have top-level declarations, the file facades aren't generated, and you won't face
> naming clashes.
> <br />在 JVM 中，如果源文件没有顶级声明，则不会生成文件外观，也不会遇到命名冲突。
> 
>   However, this naming scheme can help you avoid situations when a simple refactoring
> or an addition could include a top-level function and result in the same "Duplicate JVM classes" error.
> <br />但是，这种命名方案可以帮助您避免简单的重构或添加操作导致包含顶级函数，从而引发同样的 "Duplicate JVM classes" 错误的情况。
> 
{style="tip"}

### Source file organization
源文件组织

Placing multiple declarations (classes, top-level functions or properties) in the same Kotlin source file is encouraged
as long as these declarations are closely related to each other semantically, and the file size remains reasonable
(not exceeding a few hundred lines).
<br />鼓励在同一个 Kotlin 源文件中放置多个声明（类、顶级函数或属性），
只要这些声明在语义上紧密相关，并且文件大小保持在合理范围内（不超过几百行）。

In particular, when defining extension functions for a class which are relevant for all clients of this class,
put them in the same file with the class itself. When defining extension functions that make sense 
only for a specific client, put them next to the code of that client. Avoid creating files just to hold 
all extensions of some class.
<br />尤其需要注意的是，当定义适用于该类所有客户端的扩展函数时，
请将它们与类本身放在同一个文件中。当定义仅对特定客户端有意义的扩展函数时，
请将它们放在该客户端代码旁边。避免为了存放某个类的所有扩展而创建文件。

### Class layout
类布局

The contents of a class should go in the following order:
<br />类内容应按以下顺序排列：

1. Property declarations and initializer blocks
   <br />属性声明和初始化块
2. Secondary constructors
   <br />辅助构造器
3. Method declarations
   <br />方法声明
4. Companion object
   <br />伴生对象

Do not sort the method declarations alphabetically or by visibility, and do not separate regular methods
from extension methods. Instead, put related stuff together, so that someone reading the class from top to bottom can 
follow the logic of what's happening. Choose an order (either higher-level stuff first, or vice versa) and stick to it.
<br />不要按字母顺序或可见性对方法声明进行排序，也不要将常规方法与扩展方法分开。
而是将相关的内容放在一起，以便从上到下阅读类的人能够理解其逻辑。选择一种顺序（先写高级内容，或先写高级内容），并坚持下去。

Put nested classes next to the code that uses those classes. If the classes are intended to be used externally and aren't
referenced inside the class, put them in the end, after the companion object.
<br />将嵌套类放在使用这些类的代码旁边。如果这些类打算在外部使用，并且不在类内部引用，则将它们放在最后，在伴随对象之后。

### Interface implementation layout
接口实现布局

When implementing an interface, keep the implementing members in the same order as members of the interface (if necessary,
interspersed with additional private methods used for the implementation).
<br />实现接口时，实现成员的顺序应与接口成员的顺序保持一致（如有必要，可穿插使用实现过程中添加的私有方法）。

### Overload layout
重载布局

Always put overloads next to each other in a class.
<br />类中超载函数始终放在相邻的位置。

## Naming rules
命名规则

Package and class naming rules in Kotlin are quite simple:
<br />Kotlin 中的包和类命名规则非常简单：

* Names of packages are always lowercase and do not use underscores (`org.example.project`). Using multi-word
names is generally discouraged, but if you do need to use multiple words, you can either just concatenate them together
or use camel case (`org.example.myProject`).
  <br />软件包名称始终为小写，且不使用下划线（`org.example.project`）。
  通常不建议使用多词名称，但如果确实需要使用多个单词，您可以将它们连接起来，或者使用驼峰命名法（`org.example.myProject`）。

* Names of classes and objects use upper camel case:
  <br />类名和对象名使用大驼峰命名法：

```kotlin
open class DeclarationProcessor { /*...*/ }

object EmptyDeclarationProcessor : DeclarationProcessor() { /*...*/ }
```

### Function names
函数名
 
Names of functions, properties, and local variables start with a lowercase letter and use camel case without underscores:
<br />函数、属性和局部变量的名称以小写字母开头，并使用驼峰命名法，不带下划线：

```kotlin
fun processDeclarations() { /*...*/ }
var declarationCount = 1
```

### Names for class-like functions
类函数的名称

There are two exceptions where function names should follow class-naming convention instead.
Functions of this kind are usually defined at the top level.
<br />有两种例外情况，函数名应该遵循类命名约定。
这类函数通常定义在顶层。

* Factory functions that create class instances can have the same name as the abstract return type:
  <br />创建类实例的工厂函数可以与抽象返回类型同名：

   ```kotlin
   interface Foo { /*...*/ }

   class FooImpl : Foo { /*...*/ }

   fun Foo(): Foo { return FooImpl() }
   ```

* `@Composable` functions that return `Unit`:
<br />返回 `Unit` 的 `@Composable` 函数：

   ```kotlin
   @Composable fun TabHeader { /*...*/ }
   ```

### Names for test methods
测试方法的名称

In tests (and **only** in tests), you can use method names with spaces enclosed in backticks.
Note that such method names are only supported by Android runtime from API level 30. Underscores
in method names are also allowed in test code.
<br />在测试中（且**仅限**测试中），您可以使用反引号括起来的带空格的方法名。
请注意，此类方法名仅在 Android API 级别 30 及更高版本中受支持。
测试代码中也允许在方法名中使用下划线。

```kotlin
class MyTestCase {
    @Test fun `ensure everything works`() { /*...*/ }

    @Test fun ensureEverythingWorks_onAndroid() { /*...*/ }
}
```

### Property names
属性名称

Names of constants (properties marked with `const`, or top-level or object `val` properties with no custom `get` function
that hold deeply immutable data) should use all uppercase, underscore-separated names following the [screaming snake case](https://en.wikipedia.org/wiki/Snake_case)
convention:
<br />常量名称（标记为 `const` 的属性，或没有自定义 `get` 函数的顶级或对象 `val` 属性，
且这些属性保存着深度不可变的数据）应使用全大写字母，并以下划线分隔，遵循 [尖叫蛇命名法](https://en.wikipedia.org/wiki/Snake_case) 约定：

```kotlin
const val MAX_COUNT = 8
val USER_NAME_FIELD = "UserName"
```

Names of top-level or object properties which hold objects with behavior or mutable data should use camel case names:
<br />包含具有行为或可变数据的对象的最高级属性或对象属性的名称应使用驼峰式命名法：

```kotlin
val mutableCollection: MutableSet<String> = HashSet()
```

Names of properties holding references to singleton objects can use the same naming style as `object` declarations:
<br />持有单例对象引用的属性名称可以使用与 `object` 声明相同的命名风格：

```kotlin
val PersonComparator: Comparator<Person> = /*...*/
```

For enum constants, it's OK to use either all uppercase, underscore-separated ([screaming snake case](https://en.wikipedia.org/wiki/Snake_case)) names
(`enum class Color { RED, GREEN }`) or upper camel case names, depending on the usage. 
<br />对于枚举常量，可以使用全大写、下划线分隔的（蛇形命名法）名称（例如 `enum class Color { RED, GREEN }`），也可以使用大驼峰命名法，具体取决于用法。
   
### Names for backing properties
支持属性的名称

If a class has two properties which are conceptually the same but one is part of a public API and another is an implementation
detail, use an underscore as the prefix for the name of the private property:
<br />如果一个类有两个概念上相同的属性，但一个属于公共 API，另一个属于实现细节，
请使用下划线作为私有属性名称的前缀：

```kotlin
class C {
    private val _elementList = mutableListOf<Element>()

    val elementList: List<Element>
        get() = _elementList
}
```

### Choose good names
选择好名字

The name of a class is usually a noun or a noun phrase explaining what the class _is_: `List`, `PersonReader`.
<br />类的名称通常是名词或名词短语，用来解释该类是什么：`List`、`PersonReader`。

The name of a method is usually a verb or a verb phrase saying what the method _does_: `close`, `readPersons`.
The name should also suggest if the method is mutating the object or returning a new one. For instance `sort` is
sorting a collection in place, while `sorted` is returning a sorted copy of the collection.
<br />方法名通常是一个动词或动词短语，用来描述方法的功能，例如 `close`、`readPersons`。
方法名还应该表明方法是修改对象还是返回一个新对象。例如，`sort` 表示对集合进行原地排序，而 `sorted` 表示返回集合的排序版本。

The names should make it clear what the purpose of the entity is, so it's best to avoid using meaningless words
(`Manager`, `Wrapper`) in names.
<br />名称应该清楚地表明实体的用途，因此最好避免在名称中使用无意义的词语（例如`Manager`、`Wrapper`）。

When using an acronym as part of a declaration name, follow these rules:
<br />在声明名称中使用首字母缩写词时，请遵循以下规则：

* For two-letter acronyms, use uppercase for both letters. For example, `IOStream`.
  <br />对于双字母缩写词，两个字母都使用大写。例如，`IOStream`。
* For acronyms longer than two letters, capitalize only the first letter. For example, `XmlFormatter` or `HttpInputStream`.
  <br />对于超过两个字母的缩写，只需将首字母大写。例如，`XmlFormatter` 或 `HttpInputStream`。

## Formatting
格式化

### Indentation
缩进

Use four spaces for indentation. Do not use tabs.
<br />缩进请使用四个空格，不要使用制表符。

For curly braces, put the opening brace at the end of the line where the construct begins, and the closing brace
on a separate line aligned horizontally with the opening construct.
<br />对于花括号，将左花括号放在结构开始的行尾，右花括号放在单独的一行，并与左花括号水平对齐。

```kotlin
if (elements != null) {
    for (element in elements) {
        // ...
    }
}
```

>In Kotlin, semicolons are optional, and therefore line breaks are significant. The language design assumes 
>Java-style braces, and you may encounter surprising behavior if you try to use a different formatting style.
> <br />在 Kotlin 中，分号是可选的，因此换行符非常重要。该语言的设计默认使用 Java 风格的大括号，如果您尝试使用不同的格式，可能会遇到意想不到的行为。
>
{style="note"}

### Horizontal whitespace
水平留白

* Put spaces around binary operators (`a + b`). Exception: don't put spaces around the "range to" operator (`0..i`).
  <br />二元运算符（`a + b`）前后要加空格。例外：范围运算符（`0..i`）前后不要加空格。
* Do not put spaces around unary operators (`a++`).
  <br />不要在一元运算符（`a++`）周围加空格。
* Put spaces between control flow keywords (`if`, `when`, `for`, and `while`) and the corresponding opening parenthesis.
  <br />在控制流关键字（`if`、`when`、`for` 和 `while`）和相应的左括号之间添加空格。
* Do not put a space before an opening parenthesis in a primary constructor declaration, method declaration or method call.
  <br />在主构造函数声明、方法声明或方法调用中，不要在左括号前加空格。

```kotlin
class A(val x: Int)

fun foo(x: Int) { ... }

fun bar() {
    foo(1)
}
```

* Never put a space after `(`, `[`, or before `]`, `)`.
  <br />切勿在`(`, `[`, 之后或`]`, `)`之前添加空格。
* Never put a space around `.` or `?.`: `foo.bar().filter { it > 2 }.joinToString()`, `foo?.bar()`.
  <br />永远不要在 `.` 或 `?.` 周围加空格：`foo.bar().filter { it > 2 }.joinToString()`、`foo?.bar()`。
* Put a space after `//`: `// This is a comment`.
  <br />在 `//` 后面加一个空格：`// 这是一条注释`。
* Do not put spaces around angle brackets used to specify type parameters: `class Map<K, V> { ... }`.
  <br />不要在用于指定类型参数的尖括号周围添加空格：`class Map<K, V> { ... }`。
* Do not put spaces around `::`: `Foo::class`, `String::length`.
  <br />不要在 `::` 周围添加空格：`Foo::class`, `String::length`。
* Do not put a space before `?` used to mark a nullable type: `String?`.
  <br />不要在用于标记可空类型的 `?` 前加空格：`String?`。

As a general rule, avoid horizontal alignment of any kind. Renaming an identifier to a name with a different length
should not affect the formatting of either the declaration or any of the usages.
<br />一般而言，应避免任何形式的水平对齐。将标识符重命名为不同长度的名称不应影响声明或任何用法的格式。

### Colon
冒号

Put a space before `:` in the following scenarios:
<br />在以下情况下，请在冒号 `:` 前添加一个空格：

* When it's used to separate a type and a supertype.
  <br />当用于区分类型和超类型时。
* When delegating to a superclass constructor or a different constructor of the same class.
  <br />当委托给超类构造函数或同一类的不同构造函数时。
* After the `object` keyword.
  <br />在 `object` 关键字之后。
    
Don't put a space before `:` when it separates a declaration and its type.
<br />当冒号 (:) 用于分隔声明及其类型时，冒号前不要加空格。
 
Always put a space after `:`.
<br />`:` 后面一定要加一个空格。

```kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}

class FooImpl : Foo() {
    constructor(x: String) : this(x) { /*...*/ }

    val x = object : IFoo { /*...*/ } 
}
```

### Class headers
类头

Classes with a few primary constructor parameters can be written in a single line:
<br />具有少量主要构造函数参数的类可以用一行代码编写：

```kotlin
class Person(id: Int, name: String)
```

Classes with longer headers should be formatted so that each primary constructor parameter is in a separate line with indentation.
Also, the closing parenthesis should be on a new line. If you use inheritance, the superclass constructor call, or 
the list of implemented interfaces should be located on the same line as the parenthesis:
<br />类头较长时，应将每个主要构造函数参数放在单独的行上，并进行缩进。
此外，右括号也应另起一行。如果使用继承，则超类构造函数调用或已实现接口列表应与右括号位于同一行。

```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { /*...*/ }
```

For multiple interfaces, the superclass constructor call should be located first and then each interface should
be located in a different line:
<br />对于多个接口，应该首先调用超类构造函数，然后每个接口应该位于不同的行：

```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker { /*...*/ }
```

For classes with a long supertype list, put a line break after the colon and align all supertype names horizontally:
<br />对于具有较长超类型列表的类，请在冒号后添加换行符，并将所有超类型名称水平对齐：

```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne {

    fun foo() { /*...*/ }
}
```

To clearly separate the class header and body when the class header is long, either put a blank line
following the class header (as in the example above), or put the opening curly brace on a separate line:
<br />当类头过长时，为了清晰地分隔类头和类体，可以在类头后添加一个空行（如上例所示），或者将左大括号放在单独的一行：

```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne 
{
    fun foo() { /*...*/ }
}
```

Use regular indent (four spaces) for constructor parameters. This ensures that properties declared in the primary constructor have the same indentation as properties
declared in the body of a class.
<br />构造函数参数使用常规缩进（四个空格）。这样可以确保在主构造函数中声明的属性与在类主体中声明的属性具有相同的缩进。

### Modifiers order
修饰符顺序

If a declaration has multiple modifiers, always put them in the following order:
<br />如果一个声明包含多个修饰符，请始终按照以下顺序排列：

```kotlin
public / protected / private / internal
expect / actual
final / open / abstract / sealed / const
external
override
lateinit
tailrec
vararg
suspend
inner
enum / annotation / fun // as a modifier in `fun interface` 
companion
inline / value
infix
operator
data
```

Place all annotations before modifiers:
<br />所有注释都应放在修饰符之前：

```kotlin
@Named("Foo")
private val foo: Foo
```

Unless you're working on a library, omit redundant modifiers (for example, `public`).
<br />除非你在开发库，否则请省略冗余修饰符（例如，`public`）。

### Annotations
注释

Place annotations on separate lines before the declaration to which they are attached, and with the same indentation:
<br />将注释放在其所附加的声明之前的单独一行，并保持与声明相同的缩进：

```kotlin
@Target(AnnotationTarget.PROPERTY)
annotation class JsonExclude
```

Annotations without arguments may be placed on the same line:
<br />没有参数的注解可以放在同一行：

```kotlin
@JsonExclude @JvmField
var x: String
```

A single annotation without arguments may be placed on the same line as the corresponding declaration:
<br />单个不带参数的注解可以与相应的声明放在同一行：

```kotlin
@Test fun foo() { /*...*/ }
```

### File annotations
文件注释

File annotations are placed after the file comment (if any), before the `package` statement, 
and are separated from `package` with a blank line (to emphasize the fact that they target the file and not the package).
<br />文件注释位于文件注释（如果有）之后、`package` 语句之前，并且与`package`之间用空行隔开（以强调它们针对的是文件而不是包）。

```kotlin
/** License, copyright and whatever */
@file:JvmName("FooBar")

package foo.bar
```

### Functions
函数

If the function signature doesn't fit on a single line, use the following syntax:
<br />如果函数签名无法在一行内写完，请使用以下语法：

```kotlin
fun longMethodName(
    argument: ArgumentType = defaultValue,
    argument2: AnotherArgumentType,
): ReturnType {
    // body
}
```

Use regular indent (four spaces) for function parameters. It helps ensure consistency with constructor parameters.
<br />函数参数应使用常规缩进（四个空格）。这有助于确保与构造函数参数保持一致。

Prefer using an expression body for functions with the body consisting of a single expression.
<br />对于主体仅包含单个表达式的函数，建议使用表达式主体。

```kotlin
fun foo(): Int {     // bad
    return 1 
}

fun foo() = 1        // good
```

### Expression bodies
表达式

If the function has an expression body whose first line doesn't fit on the same line as the declaration, put the `=` sign on the first line
and indent the expression body by four spaces.
<br />如果函数体表达式的第一行无法与函数声明放在同一行，则在第一行添加等号 `=` ，并将表达式体缩进四个空格。

```kotlin
fun f(x: String, y: String, z: String) =
    veryLongFunctionCallWithManyWords(andLongParametersToo(), x, y, z)
```

### Properties
属性

For very simple read-only properties, consider one-line formatting:
<br />对于非常简单的只读属性，可以考虑使用单行格式：

```kotlin
val isEmpty: Boolean get() = size == 0
```

For more complex properties, always put `get` and `set` keywords on separate lines:
<br />对于更复杂的属性，请始终将 `get` 和 `set` 关键字放在不同的行上：

```kotlin
val foo: String
    get() { /*...*/ }
```

For properties with an initializer, if the initializer is long, add a line break after the `=` sign
and indent the initializer by four spaces:
<br />对于带有初始值设定项的属性，如果初始值设定项过长，请在等号 `=` 后添加换行符；
并将初始值设定项缩进四个空格：

```kotlin
private val defaultCharset: Charset? =
    EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
```

### Control flow statements
控制流语句

If the condition of an `if` or `when` statement is multiline, always use curly braces around the body of the statement.
Indent each subsequent line of the condition by four spaces relative to the statement start. 
Put the closing parentheses of the condition together with the opening curly brace on a separate line:
<br />如果 `if` 或 `when` 语句的条件是多行的，则始终使用花括号将语句主体括起来。
条件语句的每一行相对于语句开头缩进四个空格。
将条件语句的右括号和左花括号放在单独的一行上：

```kotlin
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
) {
    return createKotlinNotConfiguredPanel(module)
}
```

This helps align the condition and statement bodies. 
<br />这有助于使条件语句和语句主体保持一致。

Put the `else`, `catch`, `finally` keywords, as well as the `while` keyword of a `do-while` loop, on the same line as the 
preceding curly brace:
<br />将 `else`、`catch`、`finally` 关键字以及 `do-while` 循环中的 `while` 关键字放在与前面的大括号相同的行上：

```kotlin
if (condition) {
    // body
} else {
    // else part
}

try {
    // body
} finally {
    // cleanup
}
```

In a `when` statement, if a branch is more than a single line, consider separating it from adjacent case blocks with a blank line:
<br />在 `when` 语句中，如果一个分支超过一行，请考虑用空行将其与相邻的 case 代码块分隔开：

```kotlin
private fun parsePropertyValue(propName: String, token: Token) {
    when (token) {
        is Token.ValueToken ->
            callback.visitValue(propName, token.value)

        Token.LBRACE -> { // ...
        }
    }
}
```

Put short branches on the same line as the condition, without braces.
<br />将短分支放在与条件相同的行上，无需加括号。

```kotlin
when (foo) {
    true -> bar() // good
    false -> { baz() } // bad
}
```

### Method calls
方法调用

In long argument lists, put a line break after the opening parenthesis. Indent arguments by four spaces. 
Group multiple closely related arguments on the same line.
<br />在较长的论证列表中，在左括号后换行。论证之间缩进四个空格。
将多个密切相关的论证放在同一行。

```kotlin
drawSquare(
    x = 10, y = 10,
    width = 100, height = 100,
    fill = true
)
```

Put spaces around the `=` sign separating the argument name and value.
<br />在等号 `=` 前后加空格，分隔参数名和值。

### Wrap chained calls
包装链式调用

When wrapping chained calls, put the `.` character or the `?.` operator on the next line, with a single indent:
<br />当需要包装链式调用时，请将 `.` 字符或 `?.` 运算符放在下一行，并缩进一次：

```kotlin
val anchor = owner
    ?.firstChild!!
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```

The first call in the chain should usually have a line break before it, but it's OK to omit it if the code makes more sense that way.
<br />链中的第一个调用通常应该在它前面有一个换行符，但如果省略换行符使代码更易于理解，也可以省略。

### Lambdas
Lambda 函数

In lambda expressions, spaces should be used around the curly braces, as well as around the arrow which separates the parameters
from the body. If a call takes a single lambda, pass it outside parentheses whenever possible.
<br />在 lambda 表达式中，花括号周围以及分隔参数和表达式主体的箭头周围都应该使用空格。
如果一个调用只接受一个 lambda 表达式，请尽可能将其放在括号之外。

```kotlin
list.filter { it > 10 }
```

If assigning a label for a lambda, do not put a space between the label and the opening curly brace:
<br />如果要为 lambda 表达式指定标签，请勿在标签和左大括号之间添加空格：

```kotlin
fun foo() {
    ints.forEach lit@{
        // ...
    }
}
```

When declaring parameter names in a multiline lambda, put the names on the first line, followed by the arrow and the newline:
<br />在多行 lambda 表达式中声明参数名称时，请将名称放在第一行，后跟箭头和换行符：

```kotlin
appendCommaSeparated(properties) { prop ->
    val propertyValue = prop.get(obj)  // ...
}
```

If the parameter list is too long to fit on a line, put the arrow on a separate line:
<br />如果参数列表太长，一行写不下，请将箭头放在单独的一行上：

```kotlin
foo {
    context: Context,
    environment: Env
    ->
    context.configureEnv(environment)
}
```

### Trailing commas
尾随逗号

A trailing comma is a comma symbol after the last item in a series of elements:
<br />尾随逗号是指在一系列元素中的最后一个元素之后添加的逗号符号：

```kotlin
class Person(
    val firstName: String,
    val lastName: String,
    val age: Int, // trailing comma
)
```

Using trailing commas has several benefits:
<br />在句末使用逗号有几个好处：

* It makes version-control diffs cleaner – as all the focus is on the changed value.
  <br />它使版本控制差异更清晰——因为所有注意力都集中在更改的值上。
* It makes it easy to add and reorder elements – there is no need to add or delete the comma if you manipulate elements.
  <br />这样可以轻松添加和重新排列元素——在操作元素时无需添加或删除逗号。
* It simplifies code generation, for example, for object initializers. The last element can also have a comma.
  <br />它简化了代码生成，例如对象初始化器。最后一个元素也可以包含逗号。

Trailing commas are entirely optional – your code will still work without them. The Kotlin style guide encourages the use of trailing commas at the declaration site and leaves it at your discretion for the call site.
<br />尾随逗号完全是可选的——即使没有它们，你的代码仍然可以正常运行。Kotlin 风格指南鼓励在声明处使用尾随逗号，而对于调用处是否使用则由你自行决定。

To enable trailing commas in the IntelliJ IDEA formatter, go to **Settings/Preferences | Editor | Code Style | Kotlin**, 
open the **Other** tab and select the **Use trailing comma** option.
<br />要在 IntelliJ IDEA 格式化程序中启用尾随逗号，请转到**设置/首选项 | 编辑器 | 代码样式 | Kotlin**，
打开**其他**选项卡，然后选择**使用尾随逗号**选项。

#### Enumerations {initial-collapse-state="collapsed" collapsible="true"}
枚举

```kotlin
enum class Direction {
    NORTH,
    SOUTH,
    WEST,
    EAST, // trailing comma
}
```

#### Value arguments {initial-collapse-state="collapsed" collapsible="true"}
值参数

```kotlin
fun shift(x: Int, y: Int) { /*...*/ }
shift(
    25,
    20, // trailing comma
)
val colors = listOf(
    "red",
    "green",
    "blue", // trailing comma
)
```

#### Class properties and parameters {initial-collapse-state="collapsed" collapsible="true"}
类属性和参数

```kotlin
class Customer(
    val name: String,
    val lastName: String, // trailing comma
)
class Customer(
    val name: String,
    lastName: String, // trailing comma
)
```

#### Function value parameters {initial-collapse-state="collapsed" collapsible="true"}
函数值参数

```kotlin
fun powerOf(
    number: Int, 
    exponent: Int, // trailing comma
) { /*...*/ }
constructor(
    x: Comparable<Number>,
    y: Iterable<Number>, // trailing comma
) {}
fun print(
    vararg quantity: Int,
    description: String, // trailing comma
) {}
```

#### Parameters with optional type (including setters) {initial-collapse-state="collapsed" collapsible="true"}
带可选类型的参数（包括设置器）

```kotlin
val sum: (Int, Int, Int) -> Int = fun(
    x,
    y,
    z, // trailing comma
): Int {
    return x + y + x
}
println(sum(8, 8, 8))
```

#### Indexing suffix {initial-collapse-state="collapsed" collapsible="true"}
索引后缀

```kotlin
class Surface {
    operator fun get(x: Int, y: Int) = 2 * x + 4 * y - 10
}
fun getZValue(mySurface: Surface, xValue: Int, yValue: Int) =
    mySurface[
        xValue,
        yValue, // trailing comma
    ]
```

#### Parameters in lambdas {initial-collapse-state="collapsed" collapsible="true"}
lambda 表达式中的参数

```kotlin
fun main() {
    val x = {
            x: Comparable<Number>,
            y: Iterable<Number>, // trailing comma
        ->
        println("1")
    }
    println(x)
}
```

#### when entry {initial-collapse-state="collapsed" collapsible="true"}

```kotlin
fun isReferenceApplicable(myReference: KClass<*>) = when (myReference) {
    Comparable::class,
    Iterable::class,
    String::class, // trailing comma
        -> true
    else -> false
}
```

#### Collection literals (in annotations) {initial-collapse-state="collapsed" collapsible="true"}
集合字面量（在注解中）

```kotlin
annotation class ApplicableFor(val services: Array<String>)
@ApplicableFor([
    "serializer",
    "balancer",
    "database",
    "inMemoryCache", // trailing comma
])
fun run() {}
```

#### Type arguments {initial-collapse-state="collapsed" collapsible="true"}
类型参数

```kotlin
fun <T1, T2> foo() {}
fun main() {
    foo<
            Comparable<Number>,
            Iterable<Number>, // trailing comma
            >()
}
```

#### Type parameters {initial-collapse-state="collapsed" collapsible="true"}
类型参数

```kotlin
class MyMap<
        MyKey,
        MyValue, // trailing comma
        > {}
```

#### Destructuring declarations {initial-collapse-state="collapsed" collapsible="true"}
解构宣言

```kotlin
data class Car(val manufacturer: String, val model: String, val year: Int)
val myCar = Car("Tesla", "Y", 2019)
val (
    manufacturer,
    model,
    year, // trailing comma
) = myCar
val cars = listOf<Car>()
fun printMeanValue() {
    var meanValue: Int = 0
    for ((
        _,
        _,
        year, // trailing comma
    ) in cars) {
        meanValue += year
    }
    println(meanValue/cars.size)
}
printMeanValue()
```

## Documentation comments
文档注释

For longer documentation comments, place the opening `/**` on a separate line and begin each subsequent line
with an asterisk:
<br />对于较长的文档注释，请将开头的 `/**` 放在单独的一行，并在每行之后添加一个星号：

```kotlin
/**
 * This is a documentation comment
 * on multiple lines.
 */
```

Short comments can be placed on a single line:
<br />简短的评论可以写在一行里：

```kotlin
/** This is a short documentation comment. */
```

Generally, avoid using `@param` and `@return` tags. Instead, incorporate the description of parameters and return values
directly into the documentation comment, and add links to parameters wherever they are mentioned. Use `@param` and
`@return` only when a lengthy description is required which doesn't fit into the flow of the main text.
<br />通常情况下，应避免使用 `@param` 和 `@return` 标签。相反，应将参数和返回值的描述直接融入文档注释中，并在提及参数时添加指向它们的链接。
仅当需要较长的描述且无法融入正文时才使用 `@param` 和 `@return`。

```kotlin
// Avoid doing this:

/**
 * Returns the absolute value of the given number.
 * @param number The number to return the absolute value for.
 * @return The absolute value.
 */
fun abs(number: Int): Int { /*...*/ }

// Do this instead:

/**
 * Returns the absolute value of the given [number].
 */
fun abs(number: Int): Int { /*...*/ }
```

## Avoid redundant constructs
避免冗余结构

In general, if a certain syntactic construction in Kotlin is optional and highlighted by the IDE
as redundant, you should omit it in your code. Do not leave unnecessary syntactic elements in code
just "for clarity".
<br />一般来说，如果 Kotlin 中的某个语法结构是可选的，并且被 IDE 高亮显示为冗余，
那么你应该在代码中省略它。不要仅仅为了“清晰”而在代码中保留不必要的语法元素。

### Unit return type
单位返回类型

If a function returns Unit, the return type should be omitted:
<br />如果函数返回 Unit 类型，则应省略返回类型：

```kotlin
fun foo() { // ": Unit" is omitted here

}
```

### Semicolons
分号

Omit semicolons whenever possible.
<br />尽可能省略分号。

### String templates
字符串模板

Don't use curly braces when inserting a simple variable into a string template. Use curly braces only for longer expressions:
<br />在字符串模板中插入简单变量时，不要使用花括号。只有当表达式较长时才使用花括号：

```kotlin
println("$name has ${children.size} children")
```

Use [multi-dollar string interpolation](strings.md#multi-dollar-string-interpolation)
to treat the dollar sign chars `$` as string literals:
<br />使用[多美元符号字符串插值](strings.md#multi-dollar-string-interpolation)将美元符号字符 `$` 视为字符串字面量：

```kotlin
val KClass<*>.jsonSchema : String
    get() = $$"""
        {
            "$schema": "https://json-schema.org/draft/2020-12/schema",
            "$id": "https://example.com/product.schema.json",
            "$dynamicAnchor": "meta",
            "title": "$${simpleName ?: qualifiedName ?: "unknown"}",
            "type": "object"
        }
        """
```

## Idiomatic use of language features
语言特征的习语化运用

### Immutability
不变性

Prefer using immutable data to mutable. Always declare local variables and properties as `val` rather than `var` if
they are not modified after initialization.
<br />尽量使用不可变数据而非可变数据。如果局部变量和属性在初始化后不会被修改，则始终将其声明为 `val` 而不是 `var`。

Always use immutable collection interfaces (`Collection`, `List`, `Set`, `Map`) to declare collections which are not
mutated. When using factory functions to create collection instances, always use functions that return immutable
collection types when possible:
<br />始终使用不可变集合接口（`Collection`、`List`、`Set`、`Map`）来声明不会发生改变的集合。当使用工厂函数创建集合实例时，尽可能使用返回不可变集合类型的函数。

```kotlin
// Bad: use of a mutable collection type for value which will not be mutated
// 错误做法：将可变集合类型用于不会改变的值。
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ... }

// Good: immutable collection type used instead
// 优点：改用不可变集合类型
fun validateValue(actualValue: String, allowedValues: Set<String>) { ... }

// Bad: arrayListOf() returns ArrayList<T>, which is a mutable collection type
// 错误：arrayListOf() 返回 ArrayList<T>，这是一个可变集合类型。
val allowedValues = arrayListOf("a", "b", "c")

// Good: listOf() returns List<T>
// 好：list Of() 返回 List<T>
val allowedValues = listOf("a", "b", "c")
```

### Default parameter values
默认参数值

Prefer declaring functions with default parameter values to declaring overloaded functions.
<br />优先声明带有默认参数值的函数，而不是声明重载函数。

```kotlin
// Bad
fun foo() = foo("a")
fun foo(a: String) { /*...*/ }

// Good
fun foo(a: String = "a") { /*...*/ }
```

### Type aliases
类型别名

If you have a functional type or a type with type parameters which is used multiple times in a codebase, prefer defining
a type alias for it:
<br />如果代码库中多次使用函数式类型或带有类型参数的类型，建议为其定义类型别名：

```kotlin
typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person>
```
If you use a private or internal type alias for avoiding name collision, prefer the `import ... as ...` mentioned in 
[Packages and Imports](packages.md).
<br />如果使用私有或内部类型别名来避免名称冲突，请优先使用[包和导入](packages.md)中提到的`import ... as ...`。

### Lambda parameters
Lambda 参数

In lambdas which are short and not nested, it's recommended to use the `it` convention instead of declaring the parameter
explicitly. In nested lambdas with parameters, always declare parameters explicitly.
<br />对于简短且非嵌套的 lambda 表达式，建议使用 `it` 约定，而不是显式声明参数。
对于带有参数的嵌套 lambda 表达式，务必显式声明参数。

### Returns in a lambda
在 lambda 表达式中返回

Avoid using multiple labeled returns in a lambda. Consider restructuring the lambda so that it will have a single exit point.
If that's not possible or not clear enough, consider converting the lambda into an anonymous function.
<br />避免在 lambda 表达式中使用多个带标签的返回值。考虑重构 lambda 表达式，使其只有一个退出点。
如果无法重构或重构后仍然不够清晰，请考虑将 lambda 表达式转换为匿名函数。

Do not use a labeled return for the last statement in a lambda.
<br />不要在 lambda 表达式的最后一个语句中使用带标签的 return 语句。

### Named arguments
命名参数

Use the named argument syntax when a method takes multiple parameters of the same primitive type, or for parameters of `Boolean` type,
unless the meaning of all parameters is absolutely clear from context.
<br />当方法接受多个相同原始类型的参数，或者参数类型为 `Boolean` 时，请使用命名参数语法，
除非所有参数的含义都能从上下文中完全明确地表达出来。

```kotlin
drawSquare(x = 10, y = 10, width = 100, height = 100, fill = true)
```

### Conditional statements
条件语句

Prefer using the expression form of `try`, `if`, and `when`.
<br />建议使用 `try`、`if` 和 `when` 的表达式形式。

```kotlin
return if (x) foo() else bar()
```

```kotlin
return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
```

The above is preferable to:
<br />上述内容优于：

```kotlin
if (x)
    return foo()
else
    return bar()
```

```kotlin
when(x) {
    0 -> return "zero"
    else -> return "nonzero"
}
```

### if versus when

Prefer using `if` for binary conditions instead of `when`. 
For example, use this syntax with `if`:
<br />对于二元条件语句，建议使用 `if` 而不是 `when`。例如，可以使用以下 `if` 语法：

```kotlin
if (x == null) ... else ...
```

Instead of this one with `when`:
<br />而不是使用 `when`：

```kotlin
when (x) {
    null -> // ...
    else -> // ...
}
```

Prefer using `when` if there are three or more options.
<br />如果有三个或更多选项，请优先使用 `when`。

### Guard conditions in when expression
守卫条件在表达式中

Use parentheses when combining multiple boolean expressions in `when` expressions or statements with [guard conditions](control-flow.md#guard-conditions-in-when-expressions):
<br />在 `when` 表达式或带有 [守卫条件](control-flow.md#guard-conditions-in-when-expressions) 的语句中组合多个布尔表达式时，请使用括号：

```kotlin
when (status) {
    is Status.Ok if (status.info.isEmpty() || status.info.id == null) -> "no information"
}
```

Instead of:
<br />而不是：

```kotlin
when (status) {
    is Status.Ok if status.info.isEmpty() || status.info.id == null -> "no information"
}
```

### Nullable Boolean values in conditions
条件中的空布尔值

If you need to use a nullable `Boolean` in a conditional statement, use `if (value == true)` or `if (value == false)` checks.
<br />如果需要在条件语句中使用可为空的 `Boolean`，请使用 `if (value == true)` 或 `if (value == false)` 检查。

### Loops
循环

Prefer using higher-order functions (`filter`, `map` etc.) to loops. Exception: `forEach` (prefer using a regular `for` loop instead,
unless the receiver of `forEach` is nullable or `forEach` is used as part of a longer call chain).
<br />尽量使用高阶函数（例如 `filter`、`map` 等）而不是循环。例外情况：`forEach`（最好使用常规的 `for` 循环，
除非 `forEach` 的接收者可为空，或者 `forEach` 是更长调用链的一部分）。

When making a choice between a complex expression using multiple higher-order functions and a loop, understand the cost
of the operations being performed in each case and keep performance considerations in mind. 
<br />在选择使用多个高阶函数的复杂表达式和循环时，要了解
每种情况下执行的操作的成本，并牢记性能方面的考虑因素。

### Loops on ranges
循环遍历范围

Use the `..<` operator to loop over an open-ended range:
<br />使用 `..<` 运算符可以遍历一个开放式范围：

```kotlin
for (i in 0..n - 1) { /*...*/ }  // bad
for (i in 0..<n) { /*...*/ }  // good
```

### Strings
字符串

Prefer string templates to string concatenation.
<br />优先使用字符串模板，而不是字符串拼接。

Prefer multiline strings to embedding `\n` escape sequences into regular string literals.
<br />优先使用多行字符串，而不是在常规字符串字面量中嵌入 `\n` 转义序列。

To maintain indentation in multiline strings, use `trimIndent` when the resulting string does not require any internal
indentation, or `trimMargin` when internal indentation is required:
<br />要保持多行字符串的缩进，当生成的字符串不需要任何内部缩进时，请使用 `trimIndent`；当需要内部缩进时，请使用 `trimMargin`。

```kotlin
fun main() {
//sampleStart
    println("""
     Not
     trimmed
     text
     """
    )

    println("""
     Trimmed
     text
     """.trimIndent()
    )

    println()

    val a = """Trimmed to margin text:
            |if(a > 1) {
            |    return a
            |}""".trimMargin()

   println(a)
//sampleEnd
}
```
{kotlin-runnable="true"}

Learn the difference between [Java and Kotlin multiline strings](java-to-kotlin-idioms-strings.md#use-multiline-strings).
<br />了解 [Java 和 Kotlin 多行字符串](java-to-kotlin-idioms-strings.md#use-multiline-strings) 之间的区别。

### Functions vs properties
函数和属性

In some scenarios, functions with no arguments might be interchangeable with read-only properties. 
Although the semantics are similar, there are some stylistic conventions on when to prefer one to another.
<br />在某些情况下，不带参数的函数可以与只读属性互换使用。
虽然语义相似，但在何时优先使用哪种方式方面，存在一些风格上的约定。

Prefer a property over a function when the underlying algorithm:
<br />当底层算法满足以下条件时，优先选择属性而非函数：

* Does not throw.
  <br />不会抛出。
* Is cheap to calculate (or cached on the first run).
  <br />计算成本低（或在首次运行时缓存）。
* Returns the same result over invocations if the object state hasn't changed.
  <br />如果对象状态没有改变，则每次调用返回相同的结果。

### Extension functions
扩展功能

Use extension functions liberally. Every time you have a function that works primarily on an object, consider making it
an extension function accepting that object as a receiver. To minimize API pollution, restrict the visibility of
extension functions as much as it makes sense. As necessary, use local extension functions, member extension functions,
or top-level extension functions with private visibility.
<br />尽可能多地使用扩展函数。每当您有一个主要作用于对象的函数时，请考虑将其
改为一个接受该对象作为接收者的扩展函数。为了最大限度地减少 API 污染，请尽可能限制扩展函数的可见性。
必要时，请使用局部扩展函数、成员扩展函数，
或具有私有可见性的顶级扩展函数。

### Infix functions
中缀函数

Declare a function as `infix` only when it works on two objects which play a similar role. Good examples: `and`, `to`, `zip`.
Bad example: `add`.
<br />只有当函数作用于两个功能相似的对象时，才应将其声明为 `infix` 函数。例如：`and`、`to`、`zip`。
反例：`add`。

Do not declare a method as `infix` if it mutates the receiver object.
<br />如果方法会改变接收者对象，则不要将该方法声明为 `infix` 。

### Factory functions
工厂功能

If you declare a factory function for a class, avoid giving it the same name as the class itself. Prefer using a distinct name,
making it clear why the behavior of the factory function is special. Only if there is really no special semantics,
you can use the same name as the class.
<br />如果要为类声明工厂函数，请避免使用与类本身相同的名称。最好使用一个独特的名称，
并清楚地说明工厂函数的特殊行为。只有当确实没有特殊语义时，
才可以使用与类相同的名称。

```kotlin
class Point(val x: Double, val y: Double) {
    companion object {
        fun fromPolar(angle: Double, radius: Double) = Point(...)
    }
}
```

If you have an object with multiple overloaded constructors that don't call different superclass constructors and
can't be reduced to a single constructor including parameters with default values, prefer to replace the overloaded constructors with
factory functions.
<br />如果一个对象有多个重载构造函数，这些构造函数不调用不同的超类构造函数，并且
无法简化为一个包含带默认值参数的单个构造函数，那么最好用
工厂函数替换重载构造函数。

### Platform types
平台类型

A public function/method returning an expression of a platform type must declare its Kotlin type explicitly:
<br />返回平台类型表达式的公共函数/方法必须显式声明其 Kotlin 类型：

```kotlin
fun apiCall(): String = MyJavaApi.getProperty("name")
```

Any property (package-level or class-level) initialized with an expression of a platform type must declare its Kotlin type explicitly:
<br />任何使用平台类型表达式初始化的属性（包级别或类级别）都必须显式声明其 Kotlin 类型：

```kotlin
class Person {
    val name: String = MyJavaApi.getProperty("name")
}
```

A local value initialized with an expression of a platform type may or may not have a type declaration:
<br />使用平台类型表达式初始化的局部值可能具有类型声明，也可能不具有类型声明：

```kotlin
fun main() {
    val name = MyJavaApi.getProperty("name")
    println(name)
}
```

### Scope functions apply/with/run/also/let
作用域函数 apply/with/run/also/let

Kotlin provides a set of functions to execute a block of code in the context of a given object: `let`, `run`, `with`, `apply`, and `also`.
For the guidance on choosing the right scope function for your case, refer to [Scope Functions](scope-functions.md).
<br />Kotlin 提供了一组函数，用于在给定对象的上下文中执行一段代码块：`let`、`run`、`with`、`apply` 和 `also`。
有关如何选择适合您情况的作用域函数的指导，请参阅[作用域函数](scope-functions.md)。

## Coding conventions for libraries
库的编码规范

When writing libraries, it's recommended to follow an additional set of rules to ensure API stability:
<br />编写库时，建议遵循一套额外的规则以确保 API 的稳定性：

 * Always explicitly specify member visibility (to avoid accidentally exposing declarations as public API).
   <br />务必明确指定成员可见性（以避免意外地将声明公开为公共 API）。
 * Always explicitly specify function return types and property types (to avoid accidentally changing the return type
   when the implementation changes).
   <br />务必明确指定函数返回类型和属性类型（以避免在实现更改时意外更改返回类型）。
 * Provide [KDoc](kotlin-doc.md) comments for all public members, except for overrides that do not require any new documentation
   (to support generating documentation for the library).
   <br />为所有公共成员提供 [KDoc](kotlin-doc.md) 注释，但不需要任何新文档的覆盖成员除外（以支持为库生成文档）。

Learn more about best practices and ideas to consider when writing an API for your library in the [Library authors' guidelines](api-guidelines-introduction.md).
<br />了解更多有关为您的库编写 API 时的最佳实践和注意事项，请参阅[库作者指南](api-guidelines-introduction.md)。
