[//]: # (title: Compatibility guide for Kotlin 2.4.x)

_[Keeping the Language Modern](kotlin-evolution-principles.md)_ and _[Comfortable Updates](kotlin-evolution-principles.md)_ are among the fundamental principles in
Kotlin Language Design. The former says that constructs which obstruct language evolution should be removed, and the
latter says that this removal should be well-communicated beforehand to make code migration as smooth as possible.
_[保持语言现代化](kotlin-evolution-principles.md)_ 和 _[易于更新](kotlin-evolution-principles.md)_是 Kotlin 语言设计的基本原则之一。
前者指出，应该移除阻碍语言演进的结构；后者指出，移除操作应事先充分沟通，以确保代码迁移尽可能顺畅。

While most of the language changes were already announced through other channels, like update changelogs or compiler
warnings, this document summarizes them all, providing a complete reference for migration from Kotlin 2.3 to Kotlin 2.4.
This document also includes information about tool-related changes.
虽然大部分语言变更已通过其他渠道（例如更新日志或编译器警告）发布，但本文档汇总了所有变更，为从 Kotlin 2.3 迁移到 Kotlin 2.4 提供了完整的参考。
本文档还包含有关工具相关变更的信息。

## Basic terms
基本术语

In this document, we introduce several kinds of compatibility:
本文将介绍几种兼容性：

- _source_: source-incompatible change stops code that used to compile fine (without errors or warnings) from compiling
  anymore
  源不兼容的更改导致原本可以正常编译（无错误或警告）的代码无法再编译
- _binary_: two binary artifacts are said to be binary-compatible if interchanging them doesn't lead to loading or
  linkage errors
  如果交换两个二进制文件不会导致加载或链接错误，则称这两个二进制文件是二进制兼容的。
- _behavioral_: a change is said to be behavioral-incompatible if the same program demonstrates different behavior
  before and after applying the change
  如果同一程序在应用变更前后表现出不同的行为，则称该变更与行为不兼容。

Remember that those definitions are given only for pure Kotlin. Compatibility of Kotlin code from the other languages
perspective (for example, from Java) is out of the scope of this document.
请记住，这些定义仅适用于纯 Kotlin 代码。从其他语言（例如 Java）的角度来看，Kotlin 代码的兼容性不在本文档的讨论范围之内。

## Language
语言

### Drop support for `-language-version=1.9` and the K1 compiler
放弃对 `-language-version=1.9` 和 K1 编译器的支持

> **Issue**: [KT-80590](https://youtrack.jetbrains.com/issue/KT-80590)
>
> **Component**: Compiler
>
> **Incompatible change type**: source
>
> **Short summary**: Starting with Kotlin 2.4, the compiler no longer supports [`-language-version=1.9`](compiler-reference.md#language-version-version).
> As a result, the K1 compiler is no longer supported.
> <br />**简要说明**：从 Kotlin 2.4 开始，编译器不再支持 [`-language-version=1.9`](compiler-reference.md#language-version-version)。
> 因此，K1 编译器不再受支持。
>
> **Deprecation cycle**:
>
> - 2.2.0: report a warning when using `-language-version` with version 1.9
> - 2.4.0: raise the warning to an error

### Prohibit flexible explicit nullable type arguments for Java types
禁止 Java 类型使用灵活的显式可空类型参数

> **Issue**: [KTLC-284](https://youtrack.jetbrains.com/issue/KTLC-284)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Previously, when calling Java APIs from Kotlin, the compiler could treat explicitly specified nullable type arguments as flexible type arguments.
> Kotlin 2.4.0 no longer applies this behavior for nullable type arguments, so the compiler now reports errors for code that could break type safety or fail at runtime.
> <br />**简要概述**：以前，从 Kotlin 调用 Java API 时，编译器可以将显式指定的可空类型参数视为灵活类型参数。
> Kotlin 2.4.0 不再对可空类型参数应用此行为，因此编译器现在会对可能破坏类型安全或在运行时失败的代码报告错误。
>
> **Deprecation cycle**:
>
> - 2.2.0: report a warning for explicitly specified nullable type arguments that are treated as flexible types
> - 2.4.0: raise the warning to an error

### Prohibit always-false `is` checks for definitely incompatible types
禁止对绝对不兼容的类型进行始终为假的 `is` 检查

> **Issue**: [KTLC-365](https://youtrack.jetbrains.com/issue/KTLC-365)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: The compiler now prevents meaningless `is` checks that are always false because the checked types are definitely incompatible.
> This keeps the behavior consistent with other operations involving incompatible types.
> <br />**简要概述**：编译器现在会阻止那些毫无意义的 `is` 检查，因为这些检查的结果始终为假，因为被检查的类型显然不兼容。
> 这样可以确保行为与其他涉及不兼容类型的操作保持一致。
>
> **Deprecation cycle**:
>
> - 2.0.0: report a warning for `is` checks with definitely incompatible types
> - 2.4.0: raise the warning to an error

### Prohibit exposing types and declarations with lower visibility in inline functions
禁止在内联函数中暴露可见性较低的类型和声明

> **Issue**: [KTLC-283](https://youtrack.jetbrains.com/issue/KTLC-283)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: The compiler now prevents inline functions from exposing types and declarations that have lower visibility than the inline function itself.
> <br />**简要总结**：编译器现在会阻止内联函数暴露可见性低于内联函数本身的类型和声明。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning for exposing types and declarations with lower visibility in inline functions
> - 2.4.0: raise the warning to an error

### Change default use-site target selection for annotations
更改注解的默认使用站点目标选择

> **Issue**: [KTLC-391](https://youtrack.jetbrains.com/issue/KTLC-391)
>
> **Component**: Core language
>
> **Incompatible change type**: binary
>
> **Short summary**: Kotlin 2.4.0 updates the defaulting rules for propagating annotations to parameters, properties, and fields.
> This can affect annotation processing, reflection, and binary metadata after recompilation.
> When you don't specify a use-site target, the compiler now uses `param` and `property` if they apply, and uses `field` only if `property` doesn't apply.
> <br />**简要概述**：Kotlin 2.4.0 更新了注解传播到参数、属性和字段的默认规则。
> 这可能会影响注解处理、反射以及重新编译后的二进制元数据。
> 如果您未指定使用目标，编译器现在会在 `param` 和 `property` 适用时使用它们，并且仅当 `property` 不适用时才使用 `field`。
>
> You can specify a use-site target explicitly, such as `@param:Annotation` instead of `@Annotation`.
> To use the previous defaulting rule for your whole project, add `-Xannotation-default-target=first-only` to your build file.
> <br />您可以显式指定使用站点目标，例如使用 `@param:Annotation` 而不是 `@Annotation`。
> 要将之前的默认规则应用于整个项目，请将 `-Xannotation-default-target=first-only` 添加到构建文件中。
>  
> **Deprecation cycle**:
>
> - 2.2.0: report a warning when the new defaulting rule changes the chosen use-site targets
> - 2.4.0: enable the new defaulting rule

### Forbid implicit references to inaccessible types
禁止对不可访问类型的隐式引用

> **Issue**: [KTLC-384](https://youtrack.jetbrains.com/issue/KTLC-384)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Using declarations that implicitly reference inaccessible types from indirect dependencies now results in an error.
> <br />**简要总结**：现在，使用隐式引用间接依赖项中不可访问类型的声明会导致错误。
> 
> To migrate, add an explicit dependency on the module that declares the inaccessible type, or update the intermediate API so it doesn't expose that type.
> <br />要进行迁移，请添加对声明不可访问类型的模块的显式依赖，或者更新中间 API，使其不公开该类型。
> 
> **Deprecation cycle**:
>
> - 2.3.0: report a warning for implicit references to inaccessible types
> - 2.4.0: raise the warning to an error

### Enforce Jakarta nullability annotations
强制执行 Jakarta 空值注释

> **Issue**: [KTLC-285](https://youtrack.jetbrains.com/issue/KTLC-285)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: The compiler now enforces declared nullability in Kotlin for Java declarations that use [`jakarta.annotation.Nullable`](https://jakarta.ee/specifications/annotations/2.1/apidocs/jakarta.annotation/jakarta/annotation/nullable) or [`jakarta.annotation.Nonnull`](https://jakarta.ee/specifications/annotations/2.1/apidocs/jakarta.annotation/jakarta/annotation/nonnull).
> If you assign a Java declaration marked as nullable by these annotations to a non-null Kotlin type, the compiler reports an error.
> <br />**简要概述**：编译器现在强制 Kotlin 对使用 [`jakarta.annotation.Nullable`](https://jakarta.ee/specifications/annotations/2.1/apidocs/jakarta.annotation/jakarta/annotation/nullable) 或 [`jakarta.annotation.Nonnull`](https://jakarta.ee/specifications/annotations/2.1/apidocs/jakarta.annotation/jakarta/annotation/nonnull) 的 Java 声明执行可空性声明。
> 如果您将这些注解标记为可空的 Java 声明赋值给非空的 Kotlin 类型，编译器将报错。
>
> **Deprecation cycle**:
>
> - 2.2.0: report a warning for nullability mismatches in Java declarations annotated with Jakarta nullability annotations
> - 2.4.0: raise the warning to an error

### Report misplaced type arguments in callable reference qualifiers
报告可调用引用限定符中类型参数位置错误的问题

> **Issue**: [KTLC-388](https://youtrack.jetbrains.com/issue/KTLC-388)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: The compiler now checks the left-hand side of callable references and reports a warning if an inner class contains type arguments in the wrong part of the qualifier.
> <br />**简要总结**：编译器现在会检查可调用引用的左侧，如果内部类在限定符的错误部分包含类型参数，则会发出警告。
> 
> To migrate, update the reference so that each type argument belongs to the class that declares it.
> For example, write the full type `Outer<Int>.Inner<String>::toString` instead of `Inner<String, Int>::toString`.
> <br />要进行迁移，请更新引用，使每个类型参数都属于声明它的类。
> 例如，写出完整的类型 `Outer<Int>.Inner<String>::toString`，而不是 `Inner<String, Int>::toString`。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning when type arguments in the left-hand side of a callable reference belong to another part of the qualifier

### Report errors for class literals from reified type parameters with nullable upper bounds
报告来自具有可空上限的具体化类型参数的类字面量的错误

> **Issue**: [KTLC-370](https://youtrack.jetbrains.com/issue/KTLC-370)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: The compiler now reports an error when you use `::class` on an expression whose type comes from a reified type parameter with a nullable upper bound.
> If you use `::class` on such an expression, make the value non-null first with an explicit null check or the `!!` operator.
> <br />**简要说明**：现在，当您对类型来自具有可空上限的具体化类型参数的表达式使用 `::class` 时，编译器会报告错误。
> 如果您对此类表达式使用 `::class`，请先通过显式空值检查或 `!!` 运算符将值设为非空。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning when `::class` is used on an expression whose type comes from a reified type parameter with a nullable upper bound
> - 2.4.0: raise the warning to an error

### Prohibit initialization before declarations in anonymous objects
禁止在匿名对象声明之前进行初始化。

> **Issue**: [KTLC-290](https://youtrack.jetbrains.com/issue/KTLC-290)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now reports an error when you initialize a property in an `init` block of an anonymous object before declaring that property.
> <br />**简要总结**：Kotlin 现在会在匿名对象的 `init` 块中初始化属性，且未声明该属性时报告错误。
> 
> **Deprecation cycle**:
>
> - 2.2.20: report a warning when an `init` block in an anonymous object initializes a property before the property declaration
> - 2.4.0: raise the warning to an error

### Enforce exhaustiveness for `when` expressions with non-abstract Java sealed classes
对非抽象 Java 密封类的 `when` 表达式强制执行穷举性

> **Issue**: [KTLC-366](https://youtrack.jetbrains.com/issue/KTLC-366)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now checks exhaustiveness more strictly and requires an `else` branch or a branch that matches the sealed class itself when you use a `when` expression with a non-abstract Java sealed class.
> Previously, Kotlin could treat such `when` expressions as exhaustive even though the Java sealed class itself could be instantiated directly.
> <br />**简要概述**：Kotlin 现在对穷尽性检查更加严格，当使用 `when` 表达式指定非抽象的 Java 密封类时，必须存在 `else` 分支或与密封类本身匹配的分支。
> 此前，即使可以直接实例化 Java 密封类，Kotlin 也可能将此类 `when` 表达式视为穷尽性语句。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning for non-exhaustive `when` expressions with non-abstract Java sealed classes
> - 2.4.0: raise the warning to an error

### Prohibit `operator` modifier on `getValue()` and `setValue()` functions with too many parameters
禁止在参数过多的 `getValue()` 和 `setValue()` 函数上使用 `operator` 修饰符

> **Issue**: [KTLC-289](https://youtrack.jetbrains.com/issue/KTLC-289)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: When you mark the [`getValue()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.properties/-read-only-property/get-value.html) or [`setValue()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.properties/-read-write-property/set-value.html) functions with the `operator` modifier, the compiler now checks that they have the required number of value parameters.
> The `getValue()` function must have exactly two value parameters, and the `setValue()` function must have exactly three.
> To migrate, remove the `operator` modifier or change the function signature.
> <br />**简要概述**：当您使用 `operator` 修饰符标记 `getValue()` 或 `setValue()` 函数时，编译器现在会检查它们是否具有所需数量的值参数。
> `getValue()` 函数必须恰好有两个值参数，`setValue()` 函数必须恰好有三个值参数。
> 要进行迁移，请移除 `operator` 修饰符或更改函数签名。
>
> **Deprecation cycle**:
>
> - 2.2.20: report a warning for `operator` `getValue()` and `setValue()` functions with too many value parameters
> - 2.4.0: raise the warning to an error

### Prohibit inconsistent type arguments in generic calls
禁止在泛型调用中使用不一致的类型参数

> **Issue**: [KTLC-373](https://youtrack.jetbrains.com/issue/KTLC-373)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: When you specify type arguments in a generic call, the compiler now reports an error if one type argument violates an upper-bound constraint that depends on another type argument.
> If type parameters depend on each other, use type arguments that match those constraints, for example `Container<Alpha, AlphaKey>()` instead of `Container<Alpha, BetaKey>()`.
> <br />**简要说明**：在泛型调用中指定类型参数时，如果某个类型参数违反了依赖于另一个类型参数的上限约束，编译器现在会报告错误。
> 如果类型参数相互依赖，请使用符合这些约束的类型参数，例如使用 `Container<Alpha, AlphaKey>()` 而不是 `Container<Alpha, BetaKey>()`。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning when explicit type arguments in a generic call violate upper-bound constraints between type parameters
> - 2.4.0: raise the warning to an error

### Deprecate references to the `javaClass` property
弃用对 `javaClass` 属性的引用

> **Issue**: [KTLC-375](https://youtrack.jetbrains.com/issue/KTLC-375)
>
> **Component**: Kotlin/JVM
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin 2.4.0 deprecates property references to the [`javaClass`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.jvm/java-class.html) property to reduce confusion with `::class.java`.
> Use `.javaClass` to get the runtime Java class of an object, or `::class.java` to get a Java class reference.
> <br />**简要说明**：Kotlin 2.4.0 弃用了对 `javaClass` 属性的引用，以避免与 `::class.java` 混淆。
> 使用 `.javaClass` 获取对象的运行时 Java 类，或使用 `::class.java` 获取 Java 类引用。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning for property references to the `javaClass` property

### Report errors for implicit enum constructor calls that require opt-in
报告需要选择加入的隐式枚举构造函数调用的错误

> **Issue**: [KTLC-359](https://youtrack.jetbrains.com/issue/KTLC-359)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now reports an error when an enum entry implicitly calls an enum primary constructor that requires opt-in.
> To migrate, add `@OptIn` to the enum class or to each enum entry that calls the constructor.
> <br />**简要说明**：Kotlin 现在会在枚举条目隐式调用需要选择加入（opt-in）的枚举主构造函数时报错。
> 要进行迁移，请将 `@OptIn` 注解添加到枚举类或每个调用该构造函数的枚举条目。
>
> **Deprecation cycle**:
>
> - 2.2.20: report a warning when an enum entry implicitly calls an enum primary constructor that requires opt-in
> - 2.4.0: raise the warning to an error

### Forbid `inline` modifier on enum entries
禁止在枚举条目上使用 `inline` 修饰符

> **Issue**: [KTLC-361](https://youtrack.jetbrains.com/issue/KTLC-361)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now reports an error when you use the `inline` modifier on an enum entry.
> <br />**简要总结**：现在，当您在枚举条目上使用 `inline` 修饰符时，Kotlin 会报告错误。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning when the `inline` modifier is used on an enum entry
> - 2.4.0: raise the warning to an error

### Prohibit array literals outside annotation calls and parameter defaults
禁止在注解调用和参数默认值之外使用数组字面量

> **Issue**: [KTLC-369](https://youtrack.jetbrains.com/issue/KTLC-369)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Using array literals outside annotation calls and default values for annotation parameters now results in an error.
> To migrate, use `arrayOf(...)`, for example `Roles(arrayOf("admin", "user"))` instead of `Roles(["admin", "user"])`.
> <br />**简要说明**：现在，在注解调用之外使用数组字面量以及使用注解参数的默认值会导致错误。
> 要进行迁移，请使用 `arrayOf(...)`，例如 `Roles(arrayOf("admin", "user"))` 而不是 `Roles(["admin", "user"])`。
> 
> **Deprecation cycle**:
>
> - 2.3.0: report a warning for array literals outside annotation calls and default values for annotation parameters
> - 2.4.0: raise the warning to an error

### Prohibit `_root_ide_package_` in CLI compiler mode
在 CLI 编译器模式下禁止使用 `_root_ide_package_`

> **Issue**: [KTLC-378](https://youtrack.jetbrains.com/issue/KTLC-378)
>
> **Component**: Compiler
>
> **Incompatible change type**: source
>
> **Short summary**: Using the IDE-only `_root_ide_package_` qualifier in CLI compiler mode now results in an error.
> <br />**简要总结**：在 CLI 编译器模式下使用仅限 IDE 使用的 `_root_ide_package_` 限定符现在会导致错误。
>
> **Deprecation cycle**:
>
> - 2.3.20: report a warning for `_root_ide_package_` references in CLI compiler mode
> - 2.4.0: raise the warning to an error

### Correct equality for function references with vararg conversions
正确处理带有可变参数转换的函数引用相等性

> **Issue**: [KTLC-385](https://youtrack.jetbrains.com/issue/KTLC-385)
>
> **Component**: Kotlin/JVM
>
> **Incompatible change type**: behavioral
>
> **Short summary**: Kotlin/JVM now treats function references with different conversions as unequal.
> Previously, Kotlin/JVM ignored vararg conversion in equality checks when the same function reference also used another conversion, so `getDefault(::foo) == getDefaultAndVararg(::foo)` could return `true` even though only one side used vararg conversion.
> <br />**简短摘要**：Kotlin/JVM 现在将具有不同转换的函数引用视为不相等。
> 以前，当同一函数引用还使用另一种转换时，Kotlin/JVM 在相等性检查中忽略可变参数转换，因此 `getDefault(::foo) == getDefaultAndVararg(::foo)` 可能返回 `true`，即使只有一侧使用可变参数转换。
>
> **Deprecation cycle**:
>
> - 2.4.0: introduce the new behavior

### Enforce opt-in for companion object access
强制要求选择加入以访问伴随对象

> **Issue**: [KTLC-386](https://youtrack.jetbrains.com/issue/KTLC-386)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now reports an opt-in error when a class name reference resolves to a companion object that requires opt-in.
> For example, `val p = C` requires opt-in if `C` resolves to a companion object marked with an opt-in annotation.
> <br />**简要说明**：Kotlin 现在会在类名引用解析为需要 opt-in 注解的伴生对象时报告 opt-in 错误。
> 例如，如果 `C` 解析为带有 opt-in 注解的伴生对象，则 `val p = C` 需要 opt-in 注解。
>
> **Deprecation cycle**:
>
> - 2.3.20: report a warning when companion object access requires opt-in
> - 2.4.0: raise the warning to an error for `ERROR`-level opt-in requirements

### Report type mismatches from supertypes with nested generic arguments
报告具有嵌套泛型参数的超类型类型不匹配

> **Issue**: [KTLC-372](https://youtrack.jetbrains.com/issue/KTLC-372)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin now reports an error when the compiler detects a type mismatch involving a supertype with nested generic arguments.
> Previously, the compiler could miss this mismatch, which later failed with a `ClassCastException`.
> To migrate, use a type argument that matches the receiver's generic type, or remove the explicit type argument so the compiler can infer it.
> <br />**简要概述**：Kotlin 现在会在编译器检测到包含嵌套泛型参数的超类型类型不匹配时报告错误。
> 之前，编译器可能会漏掉这种类型不匹配，导致后续抛出 `ClassCastException` 异常。
> 要进行迁移，请使用与接收者泛型类型匹配的类型参数，或者移除显式类型参数，以便编译器可以进行类型推断。
>
> **Deprecation cycle**:
>
> - 2.4.0: report an error for type mismatches involving supertypes with nested generic arguments

### Prohibit inferred types with inaccessible declarations
禁止使用不可访问声明的推断类型

> **Issue**: [KTLC-363](https://youtrack.jetbrains.com/issue/KTLC-363)
>
> **Component**: Core language
>
> **Incompatible change type**: source
>
> **Short summary**: Using an inferred type that contains a declaration inaccessible in the current scope now results in an error.
> <br />**简要总结**：使用包含当前作用域中无法访问的声明的推断类型现在会导致错误。
>
> **Deprecation cycle**:
>
> - 2.3.0: report a warning when an inferred type contains a declaration that isn't accessible in the current scope
> - 2.4.0: raise the warning to an error

## Standard library
标准库

### Deprecate `kotlin.io.readLine()` function
弃用 `kotlin.io.readLine()` 函数

> **Issue**: [KTLC-394](https://youtrack.jetbrains.com/issue/KTLC-394)
>
> **Component**: kotlin-stdlib
>
> **Incompatible change type**: source
>
> **Short summary**: The [`kotlin.io.readLine()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.io/read-line.html) function is deprecated.
> Use the [`readln()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.io/readln.html) function instead of `readLine()!!`, and the [`readlnOrNull()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.io/readln-or-null.html) function instead of `readLine()`.
> <br />**简要说明**：`kotlin.io.readLine()` 函数已被弃用。
> 请使用 `readln()` 函数代替 `readLine()`，并使用 `readlnOrNull()` 函数代替 `readLine()`。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning when using `kotlin.io.readLine()`

### Deprecate `AbstractCoroutineContextKey` and related APIs
弃用 `AbstractCoroutineContextKey` 及相关 API'

> **Issue**: [KT-84970](https://youtrack.jetbrains.com/issue/KT-84970)
>
> **Component**: kotlin-stdlib
>
> **Incompatible change type**: source
>
> **Short summary**: The [`AbstractCoroutineContextKey`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.coroutines/-abstract-coroutine-context-key/) class and its related APIs were experimental since Kotlin 1.3 and proved to be error-prone.
> For this reason, this class and the related [`getPolymorphicElement()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.coroutines/get-polymorphic-element.html) and [`minusPolymorphicKey()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.coroutines/minus-polymorphic-key.html) functions are deprecated.
> <br />**简要说明**：自 Kotlin 1.3 起，`AbstractCoroutineContextKey`` 类及其相关 API 一直处于实验阶段，且已被证明容易出错。
> 因此，该类及其相关的 `getPolymorphicElement()` 和 `minusPolymorphicKey()` 函数已被弃用。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning when using the deprecated APIs

### Change `Random.nextDouble()` contract for infinite bounds
修改 `Random.nextDouble()` 合约以适应无限边界

> **Issue**: [KT-84368](https://youtrack.jetbrains.com/issue/KT-84368)
>
> **Component**: kotlin-stdlib
>
> **Incompatible change type**: behavioral
>
> **Short summary**: The documented contract for [`Random.nextDouble(until)`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.random/-random/next-double.html) now requires the `until` bound to be finite.
> Use a finite bound instead.
> <br />**简要说明**：[`Random.nextDouble(until)`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.random/-random/next-double.html) 的文档化约定现在要求 `until` 的取值范围必须是有限的。
> 请改用有限的取值范围。
>
> **Deprecation cycle**:
>
> - 2.4.0: enable the new behavior

## Tools
工具

### Deprecate legacy Kotlin/JS compiler type selection APIs
弃用旧版 Kotlin/JS 编译器类型选择 API

> **Issue**: [KT-64275](https://youtrack.jetbrains.com/issue/KT-64275), [KT-84753](https://youtrack.jetbrains.com/issue/KT-84753)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin 2.4.0 removes deprecated Gradle APIs related to selecting the legacy Kotlin/JS compiler type.
> <br />**简要总结**：Kotlin 2.4.0 移除了与选择旧版 Kotlin/JS 编译器类型相关的已弃用的 Gradle API。
> 
> Additionally, the `KotlinJsCompilerType` enum and the `KotlinProjectExtension.js()` overloads with a compiler type parameter are deprecated.
> To migrate, remove the compiler type argument from the `js()` target declaration and use the `js {}` block instead.
> <br />此外，`KotlinJsCompilerType` 枚举和带有编译器类型参数的 `KotlinProjectExtension.js()` 重载已被弃用。
> 要进行迁移，请从 `js()` 目标声明中移除编译器类型参数，并改用 `js {}` 代码块。
>
> **Deprecation cycle**:
>
> - 1.8.0: deprecate legacy Kotlin/JS compiler type constants
> - 2.4.0: remove the deprecated legacy compiler type APIs and report a warning when using `KotlinJsCompilerType` or `KotlinProjectExtension.js()` overloads with a compiler type parameter

### Deprecate `sourceSets` in the Kotlin Android extension
弃用 Kotlin Android 扩展中的 `sourceSets`

> **Issue**: [KT-74451](https://youtrack.jetbrains.com/issue/KT-74451)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: The `sourceSets` property in `KotlinAndroidProjectExtension` is deprecated.
> To migrate, configure source sets through the Android Gradle plugin's `android { sourceSets { ... } }` block instead.
> <br />**简要说明**：`KotlinAndroidProjectExtension` 中的 `sourceSets` 属性已弃用。
> 要迁移，请改用 Android Gradle 插件的 `android { sourceSets { ... } }` 代码块来配置源集。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning when accessing `sourceSets` from `KotlinAndroidProjectExtension`

### Remove consumable configurations for Kotlin/Native Apple frameworks
移除 Kotlin/原生 Apple 框架的消耗性配置

> **Issue**: [KT-74503](https://youtrack.jetbrains.com/issue/KT-74503), [KT-82230](https://youtrack.jetbrains.com/issue/KT-82230)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin 2.4.0 removes generated consumable Gradle configurations that expose Kotlin/Native Apple frameworks as outgoing artifacts.
> <br />**简要总结**：Kotlin 2.4.0 移除了生成的、可用于生成输出的 Gradle 配置，这些配置会将 Kotlin/Native Apple 框架作为输出工件公开。
>
> **Deprecation cycle**:
>
> - 2.4.0: remove consumable configurations for Kotlin/Native Apple frameworks

### Remove deprecated task, compilation, and DSL APIs from the Kotlin Gradle plugin
从 Kotlin Gradle 插件中移除已弃用的任务、编译和 DSL API

> **Issue**: [KT-85509](https://youtrack.jetbrains.com/issue/KT-85509)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin 2.4.0 removes the following deprecated Kotlin Gradle plugin APIs:
> <br />**简要概述**：Kotlin 2.4.0 移除了以下已弃用的 Kotlin Gradle 插件 API：
>
> Compile task configuration APIs:
> <br />编译任务配置 API：
>   * `KotlinJvmCompile.parentKotlinOptions`
>   * `KotlinJvmCompile.moduleName`
>   * `KotlinJvmFactory.createKotlinJvmOptions()`
>   * `BaseKotlinCompile.moduleName` from `KotlinCompile` and `Kotlin2JsCompile` tasks
> 
> Kotlin Multiplatform hierarchy and target APIs:
> <br />Kotlin 跨平台层次结构和目标 API：
>   * `DeprecatedKotlinTargetHierarchyDsl`
>   * `KotlinMultiplatformExtension.targetHierarchy`
>   * `KotlinTargetComponent.sourcesArtifacts`
>   * `KotlinTarget.sourceSets`
>   * `KotlinHierarchyBuilder.withoutCompilations()`
>   * `KotlinHierarchyBuilder.filterCompilations()`
>   * `KotlinHierarchyBuilder.withWasm()`
>   * `KotlinCompilation.defaultSourceSetName`
> 
> Kotlin compilation task APIs:
> <br />Kotlin 编译任务 API：
>   * `KotlinCompilation.compileKotlinTaskProvider`
>   * `KotlinCompilation.compileKotlinTask`
>
> Kotlin dependency handler APIs:
> <br />Kotlin依赖处理程序API：
>   * `KotlinDependencyHandler.enforcedPlatform()`
>   * `KotlinDependencyHandler.platform()`
> Other deprecated task and extension APIs:
> <br />其他已弃用的任务和扩展 API：
>   * `KaptExtension.processors`
>   * `KotlinTest.excludes`
>   * `KotlinTest.fileResolver`
>   * `KotlinTest.execHandleFactory`
>   * `IncrementalSyncTask.destinationDir`
>
> To migrate, remove usages of these APIs and use the replacements suggested by the deprecation diagnostics.
> <br />要进行迁移，请移除对这些 API 的使用，并使用弃用诊断建议的替代方案。
>
> **Deprecation cycle**:
>
> - 2.4.0: remove the deprecated APIs

### Deprecate explicit shrunk classpath snapshot configuration
弃用显式缩小类路径快照配置

> **Issue**: [KT-75837](https://youtrack.jetbrains.com/issue/KT-75837)
>
> **Component**: Build tools API 
>
> **Incompatible change type**: source
>
> **Short summary**: The `shrunkClasspathSnapshot` configuration parameter in `ClasspathSnapshotBasedIncrementalCompilationApproachParameters` is deprecated.
> The shrunk classpath snapshot is an internal incremental compilation cache, so the compiler now creates and manages it automatically under the incremental compiler metadata `workingDirectory`.
> To migrate, use the automatically managed snapshot file, instead of passing a value to `shrunkClasspathSnapshot`.
> <br />**简要说明**：`ClasspathSnapshotBasedIncrementalCompilationApproachParameters` 中的 `shrunkClasspathSnapshot` 配置参数已弃用。
> 缩减类路径快照是一个内部增量编译缓存，因此编译器现在会在增量编译器元数据 `workingDirectory` 下自动创建和管理它。
> 要进行迁移，请使用自动管理的快照文件，而不是向 `shrunkClasspathSnapshot` 传递值。
>
> **Deprecation cycle**:
>
> - 2.4.0: report a warning when using `shrunkClasspathSnapshot`

### Remove redundant ABI validation Gradle DSL elements
移除冗余的 ABI 验证 Gradle DSL 元素

> **Issue**: [KT-80685](https://youtrack.jetbrains.com/issue/KT-80685)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: Kotlin 2.4.0 simplifies the [ABI validation](gradle-binary-compatibility-validation.md) Gradle DSL and removes redundant configuration entries.
> To migrate, configure report settings directly in `abiValidation {}` instead of `abiValidation { legacyDump { ... } }`, remove `abiValidation { klib { enabled = ... } }`, and use `keepLocallyUnsupportedTargets` instead of `klib.keepUnsupportedTargets`.
> <br />**简要概述**：Kotlin 2.4.0 简化了 Gradle DSL 的 [ABI 验证](gradle-binary-compatibility-validation.md)，并移除了冗余的配置项。
> 要进行迁移，请直接在 `abiValidation {}` 中配置报告设置，而不是 `abiValidation { legacyDump { ... } }`；移除 `abiValidation { klib { enabled = ... } }`；并使用 `keepLo​​callyUnsupportedTargets` 代替 `klib.keepUnsupportedTargets`。
>
> **Deprecation cycle**:
>
> - 2.4.0: remove redundant ABI validation DSL elements

### Deprecate obsolete Compose compiler Gradle plugin options
弃用过时的 Compose 编译器 Gradle 插件选项

> **Issue**: [KT-85343](https://youtrack.jetbrains.com/issue/KT-85343)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: In Kotlin 2.4.0, the following deprecated Compose compiler Gradle plugin options now report an error when used:
> <br />**简要总结**：在 Kotlin 2.4.0 中，以下已弃用的 Compose 编译器 Gradle 插件选项在使用时会报错：
>
> * `generateFunctionKeyMetaClasses`
> * `enableIntrinsicRemember`
> * `enableNonSkippingGroupOptimization`
> * `enableStrongSkippingMode`
> * `stabilityConfigurationFile`
> * `ComposeFeatureFlag.StrongSkipping`
> * `ComposeFeatureFlag.IntrinsicRemember`
>
> Use `featureFlags` instead of the deprecated feature options, and `stabilityConfigurationFiles` instead of `stabilityConfigurationFile`.
> <br />请使用 `featureFlags` 代替已弃用的功能选项，并使用 `stabilityConfigurationFiles` 代替 `stabilityConfigurationFile`。
>
> **Deprecation cycle**:
>
> - 2.0.20: report warnings for `enableIntrinsicRemember`, `enableNonSkippingGroupOptimization`, and `enableStrongSkippingMode`
> - 2.1.0: report a warning for `stabilityConfigurationFile`
> - 2.4.0: raise the warnings to errors

### Report errors for obsolete Kotlin/Native Gradle task APIs
报告已过时的 Kotlin/Native Gradle 任务 API 的错误

> **Issue**: [KT-85510](https://youtrack.jetbrains.com/issue/KT-85510)
>
> **Component**: Gradle
>
> **Incompatible change type**: source
>
> **Short summary**: The following deprecated Kotlin/Native Gradle task APIs now report an error when used:
> <br />**简要说明**：以下已弃用的 Kotlin/Native Gradle 任务 API 现在在使用时会报告错误：
>
> `AbstractKotlinNativeCompile` properties:
> <br />`AbstractKotlinNativeCompile` 属性：
>
> * `additionalCompilerOptions`
> * `languageSettings`
> * `progressiveMode`
>
> `KotlinNativeCompile` properties:
> <br />`KotlinNativeCompile` 属性：
>
> * `moduleName`
> * `konanDataDir`
> * `konanHome`
> * `languageVersion`
> * `apiVersion`
> * `enabledLanguageFeatures`
> * `optInAnnotationsInUse`
> * `additionalCompilerOptions`
>
> `CInteropProcess` properties:
> <br />`CInteropProcess` 属性：
>
> * `outputFile`
> * `konanDataDir`
> * `konanHome`
> * `defFile`
>
> `KotlinNativeLink` properties:
> <br />`KotlinNativeLink` 属性：
>
> * `languageSettings`
> * `additionalCompilerOptions`
> * `konanDataDir`
> * `konanHome`
>
> Additionally, the `KotlinNativeLink.compilation` property is removed.
> <br />此外，还移除了 `KotlinNativeLink.compilation` 属性。
>
> **Deprecation cycle**:
>
> - 2.4.0: report an error for the deprecated Kotlin/Native Gradle task APIs, remove the `KotlinNativeLink.compilation` property