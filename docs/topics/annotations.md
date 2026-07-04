[//]: # (title: Annotations)

Annotations are tags that you can use to attach metadata to elements in your code. Tools and frameworks process this 
metadata during compilation and runtime, and perform different actions based on it.
<br />注解是用于将元数据附加到代码元素上的标签。工具和框架会在编译和运行时处理这些元数据，并根据元数据执行不同的操作。

You can annotate your code to simplify and automate common tasks, such as generating boilerplate code, enforcing coding standards or writing documentation.
<br />您可以对代码进行注释，以简化和自动化常见任务，例如生成样板代码、强制执行编码标准或编写文档。

> If you want to develop your own annotation processors, you can use the [Kotlin Symbol Processing (KSP)](ksp-overview.md) API.
> <br />如果您想开发自己的注释处理器，可以使用 [Kotlin 符号处理 (KSP)](ksp-overview.md) API。
>
{style="tip"}

## Declaration
声明

Annotations are a special type of class. To declare an annotation, use the `annotation` keyword before the class declaration:
<br />注解是一种特殊的类。要声明注解，请在类声明前使用 `annotation` 关键字：

```kotlin
annotation class Fancy
```

Additional attributes of the annotation can be specified by annotating the annotation class with meta-annotations:
<br />可以通过使用元注解注解注解类来指定注解的其他属性：

  * [`@Target`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-target/index.html) specifies the possible kinds of
    elements which can be annotated with the annotation (such as classes, functions, properties, and expressions);
    <br />[`@Target`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-target/index.html) 指定了可以使用注解进行注解的元素类型（例如类、函数、属性和表达式）；
  * [`@Retention`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-retention/index.html) specifies whether the
    annotation is stored in the compiled class files and whether it's visible through reflection at runtime
    (by default, both are true);
    <br />[`@Retention`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-retention/index.html) 指定注解是否 存储在已编译的类文件中，以及是否在运行时可通过反射访问
    （默认情况下，两者都为 true）；
  * [`@Repeatable`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-repeatable/index.html) allows using the same annotation
    on a single element multiple times;
    <br />[`@Repeatable`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-repeatable/index.html) 允许在同一个元素上多次使用相同的注解；
  * [`@MustBeDocumented`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-must-be-documented/index.html) specifies that the
    annotation is part of the public API and should be included in the class or method signature shown in the
    generated API documentation.
    <br />[`@MustBeDocumented`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-must-be-documented/index.html) 指定该注解是公共 API 的一部分，应该包含在生成的 API 文档中显示的类或方法签名中。

```kotlin
@Target(AnnotationTarget.CLASS, AnnotationTarget.FUNCTION,
        AnnotationTarget.TYPE_PARAMETER, AnnotationTarget.VALUE_PARAMETER,
        AnnotationTarget.EXPRESSION)
@Retention(AnnotationRetention.SOURCE)
@MustBeDocumented
annotation class Fancy
```

## Usage
用法

```kotlin
@Fancy class Foo {
    @Fancy fun baz(@Fancy foo: Int): Int {
        return (@Fancy 1)
    }
}
```

If you need to annotate the primary constructor of a class, you need to add the `constructor` keyword
to the constructor declaration, and add the annotations before it:
<br />如果需要为类的主构造函数添加注解，则需要在构造函数声明中添加 `constructor` 关键字，并在其前面添加注解：

```kotlin
class Foo @Inject constructor(dependency: MyDependency) { ... }
```

You can also annotate property accessors:
<br />您还可以为属性访问器添加注解：

```kotlin
class Foo {
    var x: MyDependency? = null
        @Inject set
}
```

## Constructors
构造函数

Annotations can have constructors that take parameters.
<br />注解可以有带参数的构造函数。

```kotlin
annotation class Special(val why: String)

@Special("example") class Foo {}
```

Allowed parameter types are:
<br />允许的参数类型有：

 * Types that correspond to Java primitive types (Int, Long etc.)
   <br />与 Java 基本类型（Int、Long 等）对应的类型
 * Strings
   <br />字符串
 * Classes (`Foo::class`)
   <br />类（`Foo::class`）
 * Enums
   <br />枚举
 * Other annotations
   <br />其他注解
 * Arrays of the types listed above
   <br />上述类型的数组

Annotation parameters cannot have nullable types, because the JVM does not support storing `null` as a value
of an annotation attribute.
<br />注解参数不能是可空类型，因为 JVM 不支持将 `null` 存储为注解属性的值。

If an annotation is used as a parameter of another annotation, its name is not prefixed with the `@` character:
<br />如果一个注解被用作另一个注解的参数，则其名称不以 `@` 字符为前缀：

```kotlin
annotation class ReplaceWith(val expression: String)

annotation class Deprecated(
        val message: String,
        val replaceWith: ReplaceWith = ReplaceWith(""))

@Deprecated("This function is deprecated, use === instead", ReplaceWith("this === other"))
```

If you need to specify a class as an argument of an annotation, use a Kotlin class
([KClass](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect/-k-class/index.html)). The Kotlin compiler will
automatically convert it to a Java class, so that the Java code can access the annotations and arguments
normally.
<br />如果需要将类指定为注解的参数，请使用 Kotlin 类（[KClass](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect/-k-class/index.html)）。
Kotlin 编译器会自动将其转换为 Java 类，以便 Java 代码可以正常访问注解和参数。

```kotlin

import kotlin.reflect.KClass

annotation class Ann(val arg1: KClass<*>, val arg2: KClass<out Any>)

@Ann(String::class, Int::class) class MyClass
```

## Instantiation
实例化

In Java, an annotation type is a form of an interface, so you can implement it and use an instance.
As an alternative to this mechanism, Kotlin lets you call a constructor of an annotation class in arbitrary code
and similarly use the resulting instance.
<br />在 Java 中，注解类型是一种接口，因此您可以实现它并使用其实例。
作为这种机制的替代方案，Kotlin 允许您在任意代码中调用注解类的构造函数，并类似地使用生成的实例。

```kotlin
annotation class InfoMarker(val info: String)

fun processInfo(marker: InfoMarker): Unit = TODO()

fun main(args: Array<String>) {
    if (args.isNotEmpty())
        processInfo(getAnnotationReflective(args))
    else
        processInfo(InfoMarker("default"))
}
```

Learn more about instantiation of annotation classes in [this KEEP](https://github.com/Kotlin/KEEP/blob/master/proposals/annotation-instantiation.md).
<br />有关注解类实例化的更多信息，请参阅[此 KEEP](https://github.com/Kotlin/KEEP/blob/master/proposals/annotation-instantiation.md)。

## Lambdas
Lambda 函数

Annotations can also be used on lambdas. They will be applied to the `invoke()` method into which the body
of the lambda is generated. This is useful for frameworks like [Quasar](https://docs.paralleluniverse.co/quasar/),
which uses annotations for concurrency control.
<br />注解也可以用于 lambda 表达式。它们会被应用到 `invoke()` 方法中，lambda 表达式的主体就是通过这个方法生成的。
这对于像 [Quasar](https://docs.paralleluniverse.co/quasar/) 这样的框架非常有用，Quasar 使用注解来实现并发控制。

```kotlin
annotation class Suspendable

val f = @Suspendable { Fiber.sleep(10) }
```

## Annotation use-site targets
~~注解使用站点目标~~

When you're annotating a property or a primary constructor parameter, there are multiple Java elements that are
generated from the corresponding Kotlin element, and therefore multiple possible locations for the annotation in
the generated Java bytecode. To specify how exactly the annotation should be generated, use the following syntax:
<br />当您为属性或主构造函数参数添加注解时，系统会从相应的 Kotlin 元素生成多个 Java 元素，因此在生成的 Java 字节码中，注解可能位于多个位置。
要指定注解的具体生成方式，请使用以下语法：

```kotlin
class Example(@field:Ann val foo,    // annotate only the Java field
              @get:Ann val bar,      // annotate only the Java getter
              @param:Ann val quux)   // annotate only the Java constructor parameter
```

The same syntax can be used to annotate the entire file. To do this, put an annotation with the target `file` at
the top level of a file, before the package directive or before all imports if the file is in the default package:
<br />可以使用相同的语法来注释整个文件。为此，请在以下位置添加一个以 `file` 为目标的注释：
文件的顶层，位于包指令之前；或者，如果文件位于默认包中，则位于所有导入语句之前：

```kotlin
@file:JvmName("Foo")

package org.jetbrains.demo
```

If you have multiple annotations with the same target, you can avoid repeating the target by adding brackets after the
target and putting all the annotations inside the brackets (except for the `all` meta-target):
<br />如果多个注解的目标相同，可以通过在目标后添加方括号并将所有注解放在方括号内（`all` 元目标除外）来避免重复指定目标：

```kotlin
class Example {
     @set:[Inject VisibleForTesting]
     var collaborator: Collaborator
}
```

The full list of supported use-site targets is:
<br />支持的用途站点目标完整列表如下：

  * `file`
  * `field`
  * `property` (annotations with this target are not visible to Java)
    <br />(带有此目标的注解对 Java 不可见。)
  * `get` (property getter)
    <br />(属性获取器)
  * `set` (property setter)
    <br />(属性获取器)
  * `all` (a meta-target for properties, see the [`all` meta-target](#all-meta-target) section for more information)
    <br />(属性的元目标，更多信息请参见[`all` 元目标](#all-meta-target)部分)
  * `receiver` (receiver parameter of an extension function or property)
    <br />扩展函数或属性的接收器参数

    To annotate the receiver parameter of an extension function, use the following syntax:
    <br />要为扩展函数的接收器参数添加注释，请使用以下语法：

    ```kotlin
    fun @receiver:Fancy String.myExtension() { ... }
    ```

  * `param` (constructor parameter)
    <br />(构造函数参数)
  * `setparam` (property setter parameter)
    <br />(属性设置参数)
  * `delegate` (the field storing the delegate instance for a delegated property)
    <br />(存储委托属性的委托实例的字段)

### Defaults when no use-site targets are specified
~~未指定使用站点目标时的默认值~~

If you don't specify a use-site target, the compiler chooses the target according to the `@Target` annotation of the annotation
you use. If there are multiple applicable targets, the compiler chooses one or more of them in the following order:
<br />如果您未指定使用目标，编译器将根据您使用的注解的 `@Target` 注解选择目标。
如果存在多个适用目标，编译器将按以下顺序选择一个或多个：

* The constructor parameter target (`param`).
  <br />构造函数参数目标（`param`）。
* The property target (`property`).
  <br />属性目标（`property`）。
* The field target (`field`), if it's applicable and the property target (`property`) isn't.
  <br />如果适用，则指定字段目标（`field`），而不指定属性目标（`property`）。

If none of `param`, `property`, or `field` are applicable, the annotation is invalid, and you need to specify a use-site target explicitly.
<br />如果 `param`、`property` 或 `field` 都不适用，则注解无效，需要显式指定 use-site 目标。

Let's use the [`@Email` annotation from Jakarta Bean Validation](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/email):
<br />让我们使用 [Jakarta Bean Validation 中的 `@Email` 注解](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/email)：

```java
@Target(value={METHOD,FIELD,ANNOTATION_TYPE,CONSTRUCTOR,PARAMETER,TYPE_USE})
public @interface Email { }
```

With this annotation, consider the following example:
<br />结合此注释，请考虑以下示例：

```kotlin
data class User(val username: String,
                // @Email is now equivalent to @param:Email @field:Email
                @Email val email: String) {
    // @Email is still equivalent to @field:Email
    @Email val secondaryEmail: String? = null
}
```

In this example, the `@Email` annotation applies to both the constructor parameter and the field targets for the `email` property because the property:
<br />在这个例子中，`@Email` 注解同时应用于构造函数参数和 `email` 属性的字段目标，因为该属性：

* Is declared in the primary constructor.
  <br />在主构造函数中声明。
* Has no custom getter or setter, so the compiler generates a backing field.
  <br />没有自定义 getter 或 setter，因此编译器会生成一个支持字段。

The `@Email` annotation only applies to the field target for the `secondaryEmail` property because the property:
<br />`@Email` 注解仅适用于 `secondaryEmail` 属性的目标字段，因为该属性：

* Isn't declared in the primary constructor.
  <br />未在主构造函数中声明。
* Has no custom getter or setter, so the compiler generates a backing field.
  <br />没有自定义 getter 或 setter，因此编译器会生成一个支持字段。

### `all` meta-target
`all` 元目标

The `all` target makes it easier to apply the same annotation not only to the parameter and the property or field, but also to the corresponding getter and setter.
<br />`all` 目标使得不仅可以将相同的注解应用于参数和属性或字段，还可以应用于相应的 getter 和 setter，从而更容易实现相同的注解。

Specifically, the annotation marked with `all` is propagated, if applicable:
<br />具体来说，如果适用，则会传播标记为`all`的注解：

* To the constructor parameter (`param`) if the property is defined in the primary constructor.
  <br />如果属性在主构造函数中定义，则将其传递给构造函数参数（`param`）。
* To the property itself (`property`).
  <br />到该属性本身（`property`）。
* To the backing field (`field`) if the property has one.
  <br />如果属性有支持字段（`field`），则将其赋值给该支持字段。
* To the getter (`get`).
  <br />到 getter（`get`）。
* To the setter parameter (`setparam`) if the property is defined as `var`.
  <br />如果属性定义为 `var`，则传递给 setter 参数（`setparam`）。
* To the Java-only target `RECORD_COMPONENT` if the class has the `@JvmRecord` annotation.
  <br />如果类具有 `@JvmRecord` 注解，则指向仅限 Java 的目标 `RECORD_COMPONENT`。

Let's use the [`@Email` annotation from Jakarta Bean Validation](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/email),
which is defined as follows:
<br />让我们使用 [Jakarta Bean Validation 中的 `@Email` 注解](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/email)，
其定义如下：

```java
@Target(value={METHOD,FIELD,ANNOTATION_TYPE,CONSTRUCTOR,PARAMETER,TYPE_USE})
public @interface Email { }
```

In the example below, this `@Email` annotation is applied to all relevant targets:
<br />在下面的示例中，此 `@Email` 注解应用于所有相关目标：

```kotlin
data class User(
    val username: String,
    // Applies @Email to param, field, and get
    @all:Email val email: String,
    // Applies @Email to param, field, get, and setparam
    @all:Email var name: String,
) {
    // Applies @Email to field and getter (not param since it's not in the constructor)
    @all:Email val secondaryEmail: String? = null
}
```

You can use the `all` meta-target with any property, both inside and outside the primary constructor.
<br />您可以将 `all` 元目标与任何属性一起使用，无论是在主构造函数内部还是外部。

#### Limitations
限制

The `all` target comes with some limitations:
<br />`all` 目标有一些限制：

* It does not propagate an annotation to types, potential extension receivers, or context receivers or parameters.
  <br />它不会将注解传播到类型、潜在的扩展接收器、上下文接收器或参数。
* It cannot be used with multiple annotations:
  <br />它不能与多个注释一起使用：
    ```kotlin
    @all:[A B] // forbidden, use @all:A @all:B
    val x: Int = 5
    ```
* It cannot be used with [delegated properties](delegated-properties.md).
  <br />它不能与[委托属性](delegated-properties.md)一起使用。

## Java annotations
Java 注解

Java annotations are 100% compatible with Kotlin:
<br />Java 注解与 Kotlin 完全兼容：

```kotlin
import org.junit.Test
import org.junit.Assert.*
import org.junit.Rule
import org.junit.rules.*

class Tests {
    // apply @Rule annotation to property getter
    @get:Rule val tempFolder = TemporaryFolder()

    @Test fun simple() {
        val f = tempFolder.newFile()
        assertEquals(42, getTheAnswer())
    }
}
```

Since the order of parameters for an annotation written in Java is not defined, you can't use a regular function
call syntax for passing the arguments. Instead, you need to use the named argument syntax:
<br />由于 Java 注解的参数顺序未定义，因此不能使用常规函数调用语法来传递参数。
相反，需要使用命名参数语法：

``` java
// Java
public @interface Ann {
    int intValue();
    String stringValue();
}
```

```kotlin
// Kotlin
@Ann(intValue = 1, stringValue = "abc") class C
```

Just like in Java, a special case is the `value` parameter; its value can be specified without an explicit name:
<br />与 Java 类似，`value` 参数是一个特例；它的值可以不用显式名称来指定：

``` java
// Java
public @interface AnnWithValue {
    String value();
}
```

```kotlin
// Kotlin
@AnnWithValue("abc") class C
```

### Arrays as annotation parameters
将数组作为注解参数

If the `value` argument in Java has an array type, it becomes a `vararg` parameter in Kotlin:
<br />如果 Java 中的 `value` 参数是数组类型，那么在 Kotlin 中它就变成了 `vararg` 参数：

``` java
// Java
public @interface AnnWithArrayValue {
    String[] value();
}
```

```kotlin
// Kotlin
@AnnWithArrayValue("abc", "foo", "bar") class C
```

For other arguments that have an array type, you need to use the array literal syntax or
`arrayOf(...)`:
<br />对于其他具有数组类型的参数，您需要使用数组字面量语法或
`arrayOf(...)`：

``` java
// Java
public @interface AnnWithArrayMethod {
    String[] names();
}
```

```kotlin
@AnnWithArrayMethod(names = ["abc", "foo", "bar"])
class C
```

### Accessing properties of an annotation instance
访问注解实例的属性

Values of an annotation instance are exposed as properties to Kotlin code:
<br />注解实例的值会作为属性暴露给 Kotlin 代码：

``` java
// Java
public @interface Ann {
    int value();
}
```

```kotlin
// Kotlin
fun foo(ann: Ann) {
    val i = ann.value
}
```

### Ability to not generate JVM 1.8+ annotation targets
能够不生成 JVM 1.8+ 注解目标

If a Kotlin annotation has `TYPE` among its Kotlin targets, the annotation maps to `java.lang.annotation.ElementType.TYPE_USE`
in its list of Java annotation targets. This is just like how the `TYPE_PARAMETER` Kotlin target maps to
the `java.lang.annotation.ElementType.TYPE_PARAMETER` Java target. This is an issue for Android clients with API levels
less than 26, which don't have these targets in the API.
<br />如果 Kotlin 注解的目标列表中包含 `TYPE`，则该注解会映射到 Java 注解目标列表中的 `java.lang.annotation.ElementType.TYPE_USE`。
这与 Kotlin 目标 `TYPE_PARAMETER` 映射到 Java 目标 `java.lang.annotation.ElementType.TYPE_PARAMETER` 的方式类似。
对于 API 级别低于 26 的 Android 客户端来说，这是一个问题，因为这些客户端的 API 中没有这些目标。

To avoid generating the `TYPE_USE` and `TYPE_PARAMETER` annotation targets, use the new compiler argument `-Xno-new-java-annotation-targets`.
<br />为避免生成 `TYPE_USE` 和 `TYPE_PARAMETER` 注解目标，请使用新的编译器参数 `-Xno-new-java-annotation-targets`。

## Repeatable annotations
可重复注解

Just like [in Java](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html), Kotlin has repeatable annotations,
which can be applied to a single code element multiple times. To make your annotation repeatable, mark its declaration
with the [`@kotlin.annotation.Repeatable`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-repeatable/)
meta-annotation. This will make it repeatable both in Kotlin and Java. Java repeatable annotations are also supported
from the Kotlin side.
<br />就像 [Java](https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html) 一样，Kotlin 也支持可重复注解，
它可以多次应用于同一个代码元素。要使注解可重复，请在其声明中添加 [`@kotlin.annotation.Repeatable`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.annotation/-repeatable/) 元注解。
这样，该注解在 Kotlin 和 Java 中都可重复使用。Kotlin 也支持 Java 的可重复注解。

The main difference with the scheme used in Java is the absence of a _containing annotation_, which the Kotlin compiler
generates automatically with a predefined name. For an annotation in the example below, it will generate the containing
annotation `@Tag.Container`:
<br />与 Java 中使用的方案的主要区别在于，Kotlin 编译器不会自动生成包含注解，而是使用预定义的名称。例如，对于下面的注解，它将生成包含注解 `@Tag.Container`：

```kotlin
@Repeatable
annotation class Tag(val name: String)

// The compiler generates the @Tag.Container containing annotation
// 编译器会生成包含注解的 @Tag.Container。
```

You can set a custom name for a containing annotation by applying the
[`@kotlin.jvm.JvmRepeatable`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.jvm/-jvm-repeatable/) meta-annotation
and passing an explicitly declared containing annotation class as an argument:
<br />您可以通过应用以下元注解来为包含注解设置自定义名称：
[`@kotlin.jvm.JvmRepeatable`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.jvm/-jvm-repeatable/)
并将显式声明的包含注解类作为参数传递：

```kotlin
@JvmRepeatable(Tags::class)
annotation class Tag(val name: String)

annotation class Tags(val value: Array<Tag>)
```

To extract Kotlin or Java repeatable annotations via reflection, use the [`KAnnotatedElement.findAnnotations()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect.full/find-annotations.html)
function.
<br />要通过反射提取 Kotlin 或 Java 的可重复注解，请使用 [`KAnnotatedElement.findAnnotations()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect.full/find-annotations.html)
函数。

Learn more about Kotlin repeatable annotations in [this KEEP](https://github.com/Kotlin/KEEP/blob/master/proposals/repeatable-annotations.md).
<br />了解更多关于 Kotlin 可重复注解的信息，请参阅[此 KEEP](https://github.com/Kotlin/KEEP/blob/master/proposals/repeatable-annotations.md)。
