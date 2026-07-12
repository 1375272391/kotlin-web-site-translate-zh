[//]: # (title: Types overview)

In Kotlin, everything is an object in the sense that you can call member functions and properties on any variable.
While certain types have an optimized internal representation as primitive values at runtime (such as numbers, characters, and booleans),
they appear and behave like regular classes to you.
<br />在 Kotlin 中，一切皆对象，这意味着你可以对任何变量调用成员函数和属性。
虽然某些类型在运行时会以原始值（例如数字、字符和布尔值）进行优化的内部表示，
但对你而言，它们看起来和行为都与普通类无异。

This section describes the basic types used in Kotlin:
<br />本节介绍 Kotlin 中使用的基本类型：

* [Numbers](numbers.md) and their [unsigned counterparts](unsigned-integer-types.md)
  <br />[数字](numbers.md)及其[无符号对应类型](unsigned-integer-types.md)
* [Booleans](booleans.md)
  <br />[布尔值](booleans.md)
* [Characters](characters.md)
  <br />[角色](characters.md)
* [Strings](strings.md)
  <br />[字符串](strings.md)
* [Arrays](arrays.md)
  <br />[数组](arrays.md)

To learn about other Kotlin types, such as `Nothing`, `Any`, and `Unit`, look through the Kotlin API reference:
<br />要了解其他 Kotlin 类型，例如 `Nothing`、`Any` 和 `Unit`，请查阅 Kotlin API 参考文档：

* [`Any`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-any/)
* [`Nothing`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-nothing.html)
* [`Unit`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-unit/)

Kotlin also has non-denotable types. They are the types that you can't write directly in the Kotlin code. Instead, the
compiler uses them internally, for example, for interoperability with other languages. Kotlin creates non-denotable
types to represent type information that is more precise than what Kotlin source syntax allows.

Even though you can't declare non-denotable types yourself, you may encounter them in compiler diagnostics, IDE
tooltips, or inferred type displays. Learn more about non-denotable types in:

* [Platform types](java-interop.md#null-safety-and-platform-types)
* [](typecasts.md#intersection-types)
* [](numbers.md#integer-literal-types)
* [](generics.md#captured-types)
* [Kotlin language specification: Type system](https://kotlinlang.org/spec/type-system.html)

> [Learn how to perform type checks and casts in Kotlin](typecasts.md).
> <br />[学习如何在 Kotlin 中执行类型检查和强制转换](typecasts.md)。
>
{style="tip"}