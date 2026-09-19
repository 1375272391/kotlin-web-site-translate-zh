[//]: # (title: What's new in Kotlin 2.4.0)

<show-structure depth="1"/>

<web-summary>Read the Kotlin 2.4.0 release notes covering new language features, updates to Kotlin Multiplatform, JVM, Native, JS, and Wasm, and build tool support for Gradle and Maven.</web-summary>

_[Released: July 14, 2026](releases.md#release-history)_

<tldr>
    <p> For details about bug fix release 2.4.10, see the <a href="https://github.com/JetBrains/kotlin/releases/tag/v2.4.10">changelog</a></p>
    <p> 有关错误修复版本 2.4.10 的详细信息，请参阅。 <a href="https://github.com/JetBrains/kotlin/releases/tag/v2.4.10">更新日志</a></p>
</tldr>

The Kotlin 2.4.0 release is out! Here are the main highlights:
<br />Kotlin 2.4.0 版本正式发布！以下是主要亮点：

* **Language:** [Stable context parameters, explicit backing fields, and multiple features for annotation use-site targets](#stable-features)
  <br />**语言：** [稳定的上下文参数、显式支持字段以及用于注释使用站点目标的多个特征](#stable-features)
* **Standard library:** [Stabilized support for the UUID API](#stable-uuid-api-in-the-common-kotlin-standard-library) and [support for checking sorted order](#support-for-checking-sorted-order)
  <br />**标准库：** [稳定支持 UUID API](#stable-uuid-api-in-the-common-kotlin-standard-library) 和 [支持检查排序顺序](#support-for-checking-sorted-order)
* **Kotlin/JVM:** [Support for Java 26](#support-for-java-26) and [annotations in metadata enabled by default](#annotations-in-metadata-enabled-by-default)
  <br />**Kotlin/JVM：** [支持 Java 26](#support-for-java-26) 和 [默认启用元数据注解](#annotations-in-metadata-enabled-by-default)
* **Kotlin/Native:** [Support for Swift packages as dependencies, updates on Swift export, and the CMS GC enabled by default](#kotlin-native)
  <br />**Kotlin/Native：** [支持将 Swift 包作为依赖项，更新 Swift 导出，并默认启用 CMS GC](#kotlin-native)
* **Kotlin/Wasm:** [Incremental compilation enabled by default and support for WebAssembly Component Model](#kotlin-wasm)
  <br />**Kotlin/Wasm：** [默认启用增量编译，并支持 WebAssembly 组件模型](#kotlin-wasm)
* **Kotlin/JS**: [Support for value class export and ES2015 features in JS code inlining](#kotlin-js)
  <br />**Kotlin/JS**：[支持 JS 代码内联中的值类导出和 ES2015 特性](#kotlin-js)
* **Gradle:** [Compatibility with Gradle 9.5.0](#gradle)
  <br />**Gradle：** [兼容 Gradle 9.5.0](#gradle)
* **Maven:** [Automatic alignment between Java and JVM target versions](#maven)
  <br />**Maven：** [Java 和 JVM 目标版本之间的自动对齐](#maven)
* **Kotlin compiler:** [More consistent inline function behavior during `.klib` compilation](#consistent-intra-module-function-inlining-during-klib-compilation)
  <br />**Kotlin 编译器：** [在 `.klib` 编译期间实现更一致的内联函数行为](#consistent-intra-module-function-inlining-during-klib-compilation)

You can also find an overview of the updates in this video:
<br />您也可以通过这段视频了解更新内容的概述：

<video src="https://www.youtube.com/v/RI4J0C2_FR8" title="What's New in Kotlin 2.4"/>

> For information about the Kotlin release cycle, see the [Kotlin release process](releases.md).
> <br />有关 Kotlin 发布周期的信息，请参阅 [Kotlin 发布流程](releases.md)。
>
{style="tip"}

## Update to Kotlin 2.4.0
更新至 Kotlin 2.4.0

The latest version of Kotlin is included in the latest versions of [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
and [Android Studio](https://developer.android.com/studio).
<br />最新版本的 Kotlin 已包含在最新版本的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
和 [Android Studio](https://developer.android.com/studio) 中。

To update to the new Kotlin version, make sure your IDE is updated to the latest version and [change the Kotlin version](releases.md#update-to-a-new-kotlin-version)
to 2.4.0 in your build scripts.
<br />要更新到新的 Kotlin 版本，请确保您的 IDE 已更新到最新版本，并在构建脚本中将 Kotlin 版本更改为 2.4.0。

## New features {id=new-stable-features}
<primary-label ref="stable"/>
新功能

In previous Kotlin releases, several new features were introduced as Experimental.
The following features have now graduated to [Stable](components-stability.md#stability-levels-explained) in Kotlin 2.4.0, so you no longer need to opt in to use them:
<br />在之前的 Kotlin 版本中，一些新功能以实验性（Experimental）的形式引入。
以下功能现已在 Kotlin 2.4.0 中升级为稳定版（Stable），因此您不再需要选择启用它们：

* [Context parameters](context-parameters.md), except for [context arguments](#explicit-context-arguments-for-context-parameters) and [callable references](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md#callable-references)
  <br />[上下文参数](context-parameters.md)，但[上下文参数](#explicit-context-arguments-for-context-parameters)和[可调用引用](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md#callable-references)除外
* [`@all` meta-target for properties](annotations.md#all-meta-target)
  <br />[`@all` 属性的元目标](annotations.md#all-meta-target)
* [New defaulting rules for use-site annotation targets](annotations.md#defaults-when-no-use-site-targets-are-specified)
  <br />[用于 use-site 注解目标的新默认规则](annotations.md#defaults-when-no-use-site-targets-are-specified)
* [Explicit backing fields](properties.md#explicit-backing-fields)
  <br />[显式支持字段](properties.md#explicit-backing-fields)
* [Stable UUID API in the common Kotlin standard library](#stable-uuid-api-in-the-common-kotlin-standard-library)
  <br />[通用 Kotlin 标准库中的稳定 UUID API](#stable-uuid-api-in-the-common-kotlin-standard-library)
* [New API for converting unsigned integers to `BigInteger` on the JVM](#new-api-for-converting-unsigned-integers-to-biginteger-on-the-jvm)
  <br />[JVM 上用于将无符号整数转换为 `BigInteger` 的新 API](#new-api-for-converting-unsigned-integers-to-biginteger-on-the-jvm)
* [Support for checking sorted order](#support-for-checking-sorted-order)
  <br />[支持检查排序顺序](#support-for-checking-sorted-order)
* [Support for value class export to JavaScript/TypeScript](#support-for-value-class-export-to-javascript-typescript)
  <br />[支持将值类导出到 JavaScript/TypeScript](#support-for-value-class-export-to-javascript-typescript)
* [Support for ES2015 features when inlining JS code](#support-for-es2015-features-when-inlining-js-code)
  <br />[支持内联 JS 代码时的 ES2015 特性](#support-for-es2015-features-when-inlining-js-code)
* [Maven: Automatic alignment between Java and JVM target versions](#automatic-alignment-between-java-and-jvm-target-versions)
  <br />[Maven：Java 和 JVM 目标版本之间的自动对齐](#automatic-alignment-between-java-and-jvm-target-versions)
* [Support for Maven Toolchains](#support-for-maven-toolchains)
  <br />[对 Maven 工具链的支持](#support-for-maven-toolchains)

## New features {id=new-experimental-features}
<primary-label ref="experimental-exp"/>
新功能

* [Explicit context arguments for context parameters](#explicit-context-arguments-for-context-parameters)
  <br />[上下文参数的显式上下文参数](#explicit-context-arguments-for-context-parameters)
* [Support for collection literals](#support-for-collection-literals)
  <br />[支持集合字面量](#support-for-collection-literals)
* [Improved compile-time constants](#improved-compile-time-constants)
  <br />[改进的编译时常量](#improved-compile-time-constants)
* [Improved unused result checks for higher-order functions](#improved-unused-result-checks-for-higher-order-functions) 
  <br />[改进了高阶函数的未使用结果检查](#improved-unused-result-checks-for-higher-order-functions)
* [New `@IntroducedAt` annotation to generate version-based overloads for optional parameters](#new-introducedat-annotation-to-generate-version-based-overloads-for-optional-parameters)
  <br />[新增 `@IntroducedAt` 注解，用于为可选参数生成基于版本的重载](#new-introducedat-annotation-to-generate-version-based-overloads-for-optional-parameters)
* [New map fallback functions to distinguish `null` values and missing keys](#new-map-fallback-functions-to-distinguish-null-values-and-missing-keys)
  <br />[新增映射回退函数，用于区分 `null` 值和缺失的键](#new-map-fallback-functions-to-distinguish-null-values-and-missing-keys)
* [Swift package import](#swift-package-import)
  <br />[Swift 包导入](#swift-package-import)
* [Swift export goes Alpha with improved concurrency support](#swift-export-goes-alpha-with-improved-concurrency-support)
  <br />[Swift export 进入 Alpha 测试阶段，改进了并发支持](#swift-export-goes-alpha-with-improved-concurrency-support)
* [Support for the WebAssembly Component Model](#support-for-the-webassembly-component-model)
  <br />[对 WebAssembly 组件模型的支持](#support-for-the-webassembly-component-model)

## Language
语言

Kotlin 2.4.0 promotes context parameters, explicit backing fields, and annotation use-site targets features to [Stable](components-stability.md#stability-levels-explained).
This release also introduces [explicit context arguments for context parameters](#explicit-context-arguments-for-context-parameters).
<br />Kotlin 2.4.0 将上下文参数、显式支持字段和注解使用点目标特性提升至[稳定版](components-stability.md#stability-levels-explained)。
此版本还引入了[上下文参数的显式上下文参数](#explicit-context-arguments-for-context-parameters)。

### Stable features
<secondary-label ref="language"/>
稳定功能

Kotlin 2.2.0 and 2.3.0 introduced a few language features as [Experimental](components-stability.md#stability-levels-explained). We're happy to announce that the following language features are now [Stable](components-stability.md#stability-levels-explained) in this release:
<br />Kotlin 2.2.0 和 2.3.0 引入了一些语言特性，这些特性当时处于[实验性](components-stability.md#stability-levels-explained)状态。我们很高兴地宣布，以下语言特性在此版本中已[稳定](components-stability.md#stability-levels-explained)状态：

* [Context parameters](whatsnew22.md#preview-of-context-parameters), except for [context arguments](#explicit-context-arguments-for-context-parameters) and [callable references](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md#callable-references)
  <br />[上下文参数](whatsnew22.md#preview-of-context-parameters)，但[上下文参数](#explicit-context-arguments-for-context-parameters)和[可调用引用](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md#callable-references)除外。
* [`@all` meta-target for properties](annotations.md#all-meta-target)
  <br />[`@all` 属性的元目标](annotations.md#all-meta-target)
* [New defaulting rules for use-site annotation targets](annotations.md#defaults-when-no-use-site-targets-are-specified)
  <br />[用于 use-site 注解目标的新默认规则](annotations.md#defaults-when-no-use-site-targets-are-specified)
* [Explicit backing fields](properties.md#explicit-backing-fields)
  <br />[显式支持字段](properties.md#explicit-backing-fields)

[See the full list of Kotlin language design features and proposals](kotlin-language-features-and-proposals.md).
<br />[查看 Kotlin 语言设计特性和提案的完整列表](kotlin-language-features-and-proposals.md)。

### No more deprecation warnings on the last segments of imports
<secondary-label ref="language"/>
导入语句的最后几段不再出现弃用警告

In previous Kotlin versions, when a deprecated class was imported, the deprecation error was reported at the call site
as well as at the import directive itself. As there's no way to suppress deprecation errors on imports, you may have
worked around this by suppressing deprecation reports for the entire file or by using star imports.
<br />在之前的 Kotlin 版本中，当导入已弃用的类时，会在调用点以及导入指令本身报告弃用错误。
由于没有办法抑制导入时的弃用错误，您可能通过抑制整个文件的弃用报告或使用星号导入来解决这个问题。

Since reporting the deprecation on the import of a called symbol isn't useful in most cases, Kotlin 2.4.0 doesn't issue
a warning when the deprecated symbol is referenced in the last segment of the import directive.
<br />由于在大多数情况下，报告被调用符号的导入已弃用并没有什么用处，因此 Kotlin 2.4.0 不会在导入指令的最后一个部分引用已弃用的符号时发出警告。

For more information, see [KT-30155](https://youtrack.jetbrains.com/issue/KT-30155).
<br />有关更多信息，请参阅[KT-30155](https://youtrack.jetbrains.com/issue/KT-30155)。

### Explicit context arguments for context parameters
<primary-label ref="experimental-opt-in"/>
上下文参数的显式上下文参数

<secondary-label ref="language"/>

Kotlin 2.4.0 introduces explicit context arguments for [context parameters](context-parameters.md).
<br />Kotlin 2.4.0 为 [上下文参数](context-parameters.md) 引入了显式上下文参数。

Kotlin 2.3.20 [changed the overload resolution for context parameters](whatsnew2320.md#changes-to-overload-resolution-for-context-parameters).
As a result, calls to overloads that differ only by context parameters can become ambiguous.
<br />Kotlin 2.3.20 [更改了上下文参数的重载解析](whatsnew2320.md#changes-to-overload-resolution-for-context-parameters)。
因此，仅上下文参数不同的重载调用可能会产生歧义。

You can now resolve this ambiguity by passing an explicit context argument at the call site.
<br />现在可以通过在调用点传递显式上下文参数来解决这种歧义。

Here's an example:
<br />举个例子：

```kotlin
class EmailSender
class SmsSender

context(emailSender: EmailSender)
fun sendNotification() {
    println("Sent email notification")
}

context(smsSender: SmsSender)
fun sendNotification() {
    println("Sent SMS notification")
}

context(defaultEmailSender: EmailSender, defaultSmsSender: SmsSender)
fun notifyUser() {
    
    // Selects the overload with the EmailSender context parameter
    sendNotification(emailSender = defaultEmailSender)

    // Selects the overload with the SmsSender context parameter
    sendNotification(smsSender = defaultSmsSender)
}
```

You can also use explicit context arguments instead of the `context()` function to reduce nesting and make some calls easier to read.
If you need to use the same context arguments in multiple calls, use the `context()` function instead.
<br />您也可以使用显式的上下文参数，而不是使用 `context()` 函数，这样可以减少嵌套，使某些调用更易于阅读。
如果需要在多个调用中使用相同的上下文参数，请改用 `context()` 函数。

This feature is [Experimental](components-stability.md#stability-levels-explained). To opt in, add the following compiler
option to your build file:
<br />此功能为[实验性功能](components-stability.md#stability-levels-explained)。要启用此功能，请将以下编译器
选项添加到您的构建文件中：

<tabs group="build-system">
<tab title="Gradle" group-key="gradle">

```kotlin
kotlin {
    compilerOptions {
        freeCompilerArgs.add("-Xexplicit-context-arguments")
    }
}
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <configuration>
                <args>
                    <arg>-Xexplicit-context-arguments</arg>
                </args>
            </configuration>
        </plugin>
    </plugins>
</build>
```

</tab>
</tabs>

For more information, see the feature's [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0448-explicit-context-arguments.md).
<br />有关更多信息，请参阅该功能的 [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0448-explicit-context-arguments.md)。

### Support for collection literals
<primary-label ref="experimental-opt-in"/>
支持集合字面量

<secondary-label ref="language"/>

Kotlin 2.4.0 introduces experimental support for collection literals. You can now create collections in a
simpler and more concise way using brackets `[]`.
<br />Kotlin 2.4.0 引入了对集合字面量的实验性支持。
现在，您可以使用方括号 `[]` 以更简单、更简洁的方式创建集合。

For example:
<br />例如：

```kotlin
fun main() {
    // Mutable list with explicit type declaration
    // val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")

    // Mutable list with brackets syntax
    val shapes: MutableList<String> = ["triangle", "square", "circle"]
    println(shapes)
    // [triangle, square, circle]
}
```
{validate="false"}

> Currently, collection literals can't be used to construct collections defined in Java. For more information, see [KT-80494](https://youtrack.jetbrains.com/issue/KT-80494).
> <br />目前，集合字面量不能用于构造在 Java 中定义的集合。有关更多信息，请参阅 [KT-80494](https://youtrack.jetbrains.com/issue/KT-80494)。
>
{style="note"}

If the compiler doesn't have enough information to infer the collection type, it defaults to the `List` type:
<br />如果编译器没有足够的信息来推断集合类型，则默认使用 `List` 类型：

```kotlin
fun main() {
    val fruit = ["apple", "banana", "cherry"]
    
    println(fruit)
    // [apple, banana, cherry]
}
```
{validate="false"}

You can also declare custom `operator fun of` functions to use bracket syntax with your own types. For example, if you
have the following `DoubleMatrix` class:
<br />您还可以声明自定义的 `operator fun of` 函数，以便使用方括号语法处理您自己的类型。
例如，如果您有以下 `DoubleMatrix` 类：

```kotlin
class DoubleMatrix(vararg val rows: Row) {
    companion object {
        operator fun of(vararg rows: Row) = DoubleMatrix(*rows)
    }
    class Row(vararg val elements: Double) {
        companion object {
            operator fun of(vararg elements: Double) = Row(*elements)
        }
    }
}
```
{validate="false"}

You can create an `identityMatrix` class instance like this:
<br />你可以像这样创建 `identityMatrix` 类实例：

```kotlin
fun main() {
    val identityMatrix: DoubleMatrix = [
        [1.0, 0.0, 0.0],
        [0.0, 1.0, 0.0],
        [0.0, 0.0, 1.0],
    ]
}
```
{validate="false"}

In this example, the compiler translates the nested collection literals into calls to the corresponding `operator fun of`
functions. The compiler resolves these calls recursively and uses the expected types to choose the correct overloads.
<br />在此示例中，编译器将嵌套集合文字转换为对相应 `operator fun of` 的调用功能。
编译器递归地解析这些调用，并使用预期的类型来选择正确的重载。

This feature is [Experimental](components-stability.md#stability-levels-explained). To opt in, add the following compiler
option to your build file:
<br />此功能为[实验性功能](components-stability.md#stability-levels-explained)。
要启用此功能，请将以下编译器选项添加到您的构建文件中：

<tabs group="build-system">
<tab title="Gradle" group-key="gradle">

```kotlin
kotlin {
    compilerOptions {
        freeCompilerArgs.add("-Xcollection-literals")
    }
}
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <configuration>
                <args>
                    <arg>-Xcollection-literals</arg>
                </args>
            </configuration>
        </plugin>
    </plugins>
</build>
```

</tab>
</tabs>

For more information, see the feature's [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0416-collection-literals.md).
<br />有关更多信息，请参阅该功能的 [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0416-collection-literals.md)。

### Improved compile-time constants
<primary-label ref="experimental-opt-in"/>
改进的编译时常量

<secondary-label ref="language"/>

Kotlin 2.4.0 brings experimental improvements to [compile-time constants](properties.md#compile-time-constants),
making support for numeric and string types more consistent and easier to use. These improvements include support for:
<br />Kotlin 2.4.0 对[编译时常量](properties.md#compile-time-constants)进行了实验性改进，
使对数值和字符串类型的支持更加一致且易于使用。这些改进包括对以下类型的支持：

* Unsigned type operations.
  <br />无符号类型操作。
* Standard library functions for strings, like `.lowercase()`, `.uppercase()`, and `.trim()` functions.
  <br />标准库中用于字符串的函数，例如 `.lowercase()`、`.uppercase()` 和 `.trim()` 函数。
* Evaluation of the `.name` property of [enum constants](enum-classes.md#working-with-enum-constants) and the [`KCallable` interface](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.reflect/-k-callable/).
  <br />对枚举常量（enum-classes.md#working-with-enum-constants）和 [`KCallable` 接口](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.reflect/-k-callable/) 的 `.name` 属性进行评估。

To make it clear which functions are evaluated at compile time, Kotlin 2.4.0 introduces the `IntrinsicConstEvaluation` annotation.
Some functions are evaluated at compile-time but don't have the annotation yet. Later releases will add the annotation
to the remaining functions. For a list of supported functions, see the KEEP [appendix](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0444-improve-compile-time-constants.md#appendix).
<br />为了明确哪些函数在编译时求值，Kotlin 2.4.0 引入了 `IntrinsicConstEvaluation` 注解。
有些函数在编译时求值，但目前还没有这个注解。后续版本会将此注解添加到剩余的函数中。
有关支持的函数列表，请参阅 KEEP 的[附录](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0444-improve-compile-time-constants.md#appendix)。

This feature is [Experimental](components-stability.md#stability-levels-explained). To opt in, add the following compiler option to your build file:
<br />此功能为[实验性功能](components-stability.md#stability-levels-explained)。 要启用此功能，请将以下编译器选项添加到您的构建文件中：

<tabs group="build-system">
<tab title="Gradle" group-key="gradle">

```kotlin
kotlin {
    compilerOptions {
        freeCompilerArgs.add("-Xintrinsic-const-evaluation")
    }
}
```

</tab>
<tab title="Maven" group-key="maven">

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <configuration>
                <args>
                    <arg>-Xintrinsic-const-evaluation</arg>
                </args>
            </configuration>
        </plugin>
    </plugins>
</build>
```

</tab>
</tabs>

For more information, see the feature's [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0444-improve-compile-time-constants.md).
<br />有关更多信息，请参阅该功能的 [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0444-improve-compile-time-constants.md)。

### Improved unused result checks for higher-order functions
<primary-label ref="experimental-opt-in"/>
改进了高阶函数的未使用结果检查

<secondary-label ref="language"/>

Kotlin 2.4.0 introduces a new Experimental `returnsResultOf()` contract to improve the [unused return value checker](unused-return-value-checker.md).
<br />Kotlin 2.4.0 引入了一个新的实验性 `returnsResultOf()` 合约，以改进 [未使用的返回值检查器](unused-return-value-checker.md)。

This contract enables the checker to distinguish between unused results that can be ignored and meaningful unused results
from higher-order functions that return the result of a lambda, such as the `let` scope function.
<br />该合约使检查器能够区分可以忽略的未使用结果和有意义的未使用结果
来自返回 lambda 结果的高阶函数，例如 `let` 作用域函数。

> Kotlin contracts are [Experimental](components-stability.md#stability-levels-explained). To opt in, add the 
> `@OptIn(ExperimentalContracts::class)` annotation when declaring a function with a contract.
> <br />Kotlin 合约目前处于[实验性](components-stability.md#stability-levels-explained)。
> 要启用合约，请在声明带有合约的函数时添加 `@OptIn(ExperimentalContracts::class)` 注解。
>
{style="warning"}

To use this feature, add `returnsResultOf()` to the function's contract:
<br />要使用此功能，请将 `returnsResultOf()` 添加到函数的契约中：

```kotlin
import kotlin.contracts.ExperimentalContracts
import kotlin.contracts.contract

@OptIn(ExperimentalContracts::class)
inline fun <T, R> T.customLet(block: (T) -> R): R {
    contract {
        returnsResultOf(block)
    }
    return block(this)
}
```

Here's an example that uses a custom `.customLet()` function with a nullable value:
<br />以下示例使用了带有可空值的自定义 `.customLet()` 函数：

```kotlin
fun handleNullablePackageName(packageName: String?, builder: StringBuilder) {
    // The checker doesn't report a warning
    // because the return value of the append() function can be ignored
    packageName?.customLet { builder.append(it) }

    // The checker reports a warning because the returned string is unused
    packageName?.customLet { "kotlin.$it" }
}
```

The unused return value checker is [Experimental](components-stability.md#stability-levels-explained) and must be enabled
to report unused return values.
For more information about enabling and configuring the checker, see [Unused return value checker](unused-return-value-checker.md#configure-the-unused-return-value-checker).
<br />未使用的返回值检查器目前处于[实验性](components-stability.md#stability-levels-explained)状态，必须启用才能报告未使用的返回值。
有关启用和配置此检查器的更多信息，请参阅[未使用的返回值检查器](unused-return-value-checker.md#configure-the-unused-return-value-checker)。

#### How to enable {id=how-to-enable-unused-return-value-checker}
如何启用

The `returnsResultOf()` contract is [Experimental](components-stability.md#stability-levels-explained). Be aware that using
it produces pre-release binaries that earlier Kotlin compiler versions can't read. To opt in, add the following compiler
option to your build file:
<br />`returnsResultOf()` 合约目前处于[实验性](components-stability.md#stability-levels-explained) 阶段。
请注意，使用它生成的预发布二进制文件可能无法被早期版本的 Kotlin 编译器读取。要启用此功能，请将以下编译器选项添加到您的构建文件中：

<tabs group="build-system">
<tab title="Gradle" group-key="gradle">

```kotlin
// build.gradle(.kts)
kotlin {
    compilerOptions {
        freeCompilerArgs.add("-Xallow-returns-result-of")
    }
}
```

</tab> <tab title="Maven" group-key="maven">

```xml
<!-- pom.xml -->
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <configuration>
                <args>
                    <arg>-Xallow-returns-result-of</arg>
                </args>
            </configuration>
        </plugin>
    </plugins>
</build>
```
</tab> 
</tabs>

### New `@IntroducedAt` annotation to generate version-based overloads for optional parameters
<primary-label ref="experimental-opt-in"/>
新增 `@IntroducedAt` 注解，用于为可选参数生成基于版本的重载。

<secondary-label ref="language"/>

Kotlin 2.4.0 introduces the `@IntroducedAt` annotation for preserving binary compatibility when adding new optional parameters to published APIs.
<br />Kotlin 2.4.0 引入了 `@IntroducedAt` 注解，以便在向已发布的 API 添加新的可选参数时保持二进制兼容性。

Previously, adding optional parameters to a function often required using `@JvmOverloads`, which can generate more overloads than needed.
Alternatively, preserving binary compatibility required you to keep older signatures as hidden deprecated overloads.
<br />以前，向函数添加可选参数通常需要使用 `@JvmOverloads` 注解，这可能会生成过多的重载。
或者，为了保持二进制兼容性，需要将旧的函数签名保留为隐藏的已弃用重载。

With the `@IntroducedAt` annotation, you can annotate newly added optional parameters with the version in which they were introduced.
The compiler uses this information to automatically generate the corresponding hidden overloads.
<br />使用 `@IntroducedAt` 注解，您可以为新添加的可选参数添加标记，标明它们引入的版本号。
编译器会利用此信息自动生成相应的隐藏重载。

This annotation is [Experimental](components-stability.md#stability-levels-explained). To opt in, use the `@OptIn(ExperimentalVersionOverloading::class)` annotation.
<br />此注解为[实验性](components-stability.md#stability-levels-explained)。要启用此注解，请使用 `@OptIn(ExperimentalVersionOverloading::class)` 注解。

Here's an example:
<br />举个例子：

```kotlin
@OptIn(ExperimentalVersionOverloading::class)
fun Button(
    label: String = "",
    color: Color = DefaultColor,
    @IntroducedAt("1.1") borderColor: Color = DefaultBorderColor,
    @IntroducedAt("1.2") borderStyle: Style = DefaultBorderStyle,
    @IntroducedAt("1.2") borderWidth: Int = 1,
    onClick: () -> Unit
) {
    // Function body
}
```

In this example, the compiler generates hidden overloads for the older versions of the `Button()` function.
<br />在这个例子中，编译器会为旧版本的 `Button()` 函数生成隐藏的重载。

Since both `@IntroducedAt` and `@JvmOverloads` generate overloads, using them together can cause conflicting overloads.
If you use both annotations, the compiler reports a warning. If you suppress the warning, the compiler prioritizes overloads
generated from the `@IntroducedAt` annotation.
<br />由于`@IntroducedAt`和 `@JvmOverloads` 都会生成重载，因此将它们一起使用可能会导致重载冲突。
如果同时使用这两个注释，编译器会报告警告。如果您抑制警告，编译器会优先考虑重载
从`@IntroducedAt` 注释生成。

## Standard library
标准库

Kotlin 2.4.0 stabilizes support for UUIDs in the common Kotlin standard library. It also adds new extension
functions for converting unsigned integers to `BigInteger` on the JVM and support for checking sorted order.
<br />Kotlin 2.4.0 稳定了 Kotlin 标准库中对 UUID 的支持。它还新增了扩展函数，用于在 JVM 上将无符号整数转换为 `BigInteger`，并支持检查排序顺序。

### Stable UUID API in the common Kotlin standard library
<secondary-label ref="standard-library"/>
Kotlin 标准库中稳定的 UUID API

Kotlin 2.0.20 introduced a [class for generating UUIDs](whatsnew2020.md#support-for-uuids-in-the-common-kotlin-standard-library)
(universally unique identifiers) and added support for converting between Kotlin and Java UUIDs. Later releases gradually
improved this experimental feature by adding support for:
<br />Kotlin 2.0.20 引入了一个用于生成 UUID 的类（通用唯一标识符），并增加了对 Kotlin 和 Java UUID 之间转换的支持。
后续版本逐步改进了这项实验性功能，增加了对以下功能的支持：

* [Comparing UUIDs with `<` and `>` operators](whatsnew2120.md#changes-in-uuid-parsing-formatting-and-comparability)
  <br />[使用 `<` 和 `>` 运算符比较 UUID](whatsnew2120.md#changes-in-uuid-parsing-formatting-and-comparability)
* [Parsing UUIDs from hex-and-dash and plain text formats](uuids.md#parse-uuids)
  <br />[解析十六进制和纯文本格式的 UUID](uuids.md#parse-uuids)
* [Returning `null` when parsing invalid UUIDs](whatsnew23.md#support-for-returning-null-when-parsing-invalid-uuids).
  <br />[解析无效 UUID 时返回 `null`](whatsnew23.md#support-for-returning-null-when-parsing-invalid-uuids)。

In Kotlin 2.4.0, [the `kotlin.uuid.Uuid` API](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.uuid/-uuid/) becomes [Stable](components-stability.md#stability-levels-explained).
The only exceptions are [the functions for generating V4 and V7 UUIDs](whatsnew23.md#support-for-generating-v7-uuids-for-specific-timestamps), which remain [Experimental](components-stability.md#stability-levels-explained) and still require opt-in.
<br />在 Kotlin 2.4.0 中，[`kotlin.uuid.Uuid` API](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.uuid/-uuid/) 已变为[稳定版](components-stability.md#stability-levels-explained)。
唯一的例外是[用于生成 V4 和 V7 UUID 的函数](whatsnew23.md#support-for-generating-v7-uuids-for-specific-timestamps)，这些函数仍处于[实验性](components-stability.md#stability-levels-explained)状态，需要用户选择启用。

For more information about how to work with UUIDs, see [UUIDs](uuids.md).
<br />有关如何使用 UUID 的更多信息，请参阅 [UUIDs](uuids.md)。

### Support for checking sorted order
<secondary-label ref="standard-library"/>
支持检查排序顺序

Kotlin 2.4.0 adds new extension functions for checking sorted order in iterables, arrays, and sequences.
<br />Kotlin 2.4.0 新增了用于检查可迭代对象、数组和序列中排序顺序的扩展函数。

This includes the following extension functions:
<br />这包括以下扩展功能：

* `.isSorted()`
* `.isSortedDescending()`
* `.isSortedWith(comparator)`
* `.isSortedBy(selector)`
* `.isSortedByDescending(selector)`

You can use these extension functions to check whether elements are already sorted, without sorting them again or creating your own helper functions.
They return `true` if the elements are in the specified order, or if there are fewer than two elements, and `false` otherwise.
These functions stop as soon as they encounter an out-of-order pair, which makes them efficient for large inputs.
<br />你可以使用这些扩展函数来检查元素是否已排序，而无需再次排序或创建自己的辅助函数。
如果元素顺序正确，或者元素数量少于两个，则返回 `true`；否则返回 `false`。
这些函数一旦遇到乱序元素对就会停止，因此对于大型输入数据非常高效。

Here's an example of checking sorted order with `.isSorted()` and `.isSortedBy()` functions:
<br />以下是使用 `.isSorted()` 和 `.isSortedBy()` 函数检查排序顺序的示例：

```kotlin
data class User(val name: String, val age: Int)

fun main() {
    val numbers = listOf(1, 2, 3, 4)
    println(numbers.isSorted())
    // true

    val users = listOf(
        User("Alice", 24),
        User("Bob", 31),
        User("Charlie", 29),
    )
    println(users.isSortedBy(User::age))
    // false
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="2.4.0-Beta2" id="kotlin-2-4-0-check-sorted-order"}

### New API for converting unsigned integers to `BigInteger` on the JVM
<secondary-label ref="standard-library"/>
用于在 JVM 上将无符号整数转换为“BigInteger”的新 API

Kotlin 2.4.0 introduces the `UInt.toBigInteger()` and `ULong.toBigInteger()` extension functions on the JVM.
<br />Kotlin 2.4.0 在 JVM 上引入了 `UInt.toBigInteger()` 和 `ULong.toBigInteger()` 扩展函数。

Previously, converting `UInt` and `ULong` values to `BigInteger` required string-based workarounds or custom conversion logic.
Starting with Kotlin 2.4.0, you can now use `.toBigInteger()` to convert unsigned integer values directly to `BigInteger`.
<br />以前，将 `UInt` 和 `ULong` 值转换为 `BigInteger` 需要使用基于字符串的变通方法或自定义转换逻辑。
从 Kotlin 2.4.0 开始，您现在可以使用 `.toBigInteger()` 直接将无符号整数值转换为 `BigInteger`。

Here's an example:
<br />举个例子：

```kotlin
fun main() {
    //sampleStart
    val unsignedLong = Long.MAX_VALUE.toULong() + 1uL
    val unsignedInt = UInt.MAX_VALUE

    println(unsignedLong.toBigInteger())
    // 9223372036854775808

    println(unsignedInt.toBigInteger())
    // 4294967295
   //sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="2.4.0-Beta2" id="kotlin-2-4-0-convert-unsigned-int"}

### New map fallback functions to distinguish `null` values and missing keys
<primary-label ref="experimental-opt-in"/>
新增映射回退函数，用于区分 `null` 值和缺失的键

<secondary-label ref="standard-library"/>

Kotlin 2.4.0 adds new variants of the existing [`.getOrElse()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/get-or-else.html)
and [`.getOrPut()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/get-or-put.html) [map extension functions](map-operations.md)
for maps with nullable values. These functions retrieve a value for a key or use a default value as a fallback. 
For maps with nullable values, the new variants let you choose whether a stored `null` value behaves like a missing key
or an existing value, and they make that choice clear in their function names.
<br />Kotlin 2.4.0 为现有的 `.getOrElse()` 和 `.getOrPut()` 函数添加了新的变体，用于处理可空值的映射。
这些函数可以获取键的值，或者使用默认值作为回退。
对于可空值的映射，新的变体允许您选择存储的 `null` 值是作为缺失的键还是作为已存在的值，并且函数名称会明确地表明这种选择。

The new extension functions include the following:
<br />新增的扩展功能包括以下几项：

* `.getOrElseIfNull(key, defaultValue)` and `.getOrPutIfNull(key, defaultValue)`, which return the default value if the key is missing or has a `null` value, similar to the existing `.getOrElse()` and `.getOrPut()` functions.
  <br />`.getOrElseIfNull(key, defaultValue)` 和 `.getOrPutIfNull(key, defaultValue)`，如果键缺失或值为 `null`，则返回默认值，类似于现有的 `.getOrElse()` 和 `.getOrPut()` 函数。
* `.getOrElseIfMissing(key, defaultValue)` and `.getOrPutIfMissing(key, defaultValue)`, which return the default value only when the map doesn't contain the specified key.
  <br />`.getOrElseIfMissing(key, defaultValue)` 和 `.getOrPutIfMissing(key, defaultValue)` 仅在映射中不包含指定的键时才返回默认值。

These APIs are [Experimental](components-stability.md#stability-levels-explained) and require opt-in with the `@OptIn(ExperimentalStdlibApi::class)` annotation.
<br />这些 API 是 [实验性的](components-stability.md#stability-levels-explained)，需要使用 `@OptIn(ExperimentalStdlibApi::class)` 注解进行选择加入。

Here's an example that demonstrates the difference between `.getOrPutIfNull()` and `.getOrPutIfMissing()` when the key exists with a `null` value:
<br />以下示例演示了当键存在但值为 `null` 时，`.getOrPutIfNull()` 和 `.getOrPutIfMissing()` 之间的区别：

```kotlin
@OptIn(ExperimentalStdlibApi::class)
fun main() {
    val mapForNull = mutableMapOf<String, String?>("user" to null)
    val mapForMissing = mutableMapOf<String, String?>("user" to null)

    // Replaces the value if "user" has a null value
    mapForNull.getOrPutIfNull("user") { "default_user" }

    println(mapForNull)
    // {user=default_user}

    // Keeps the null value because "user" exists in the map
    mapForMissing.getOrPutIfMissing("user") { "default_user" }

    println(mapForMissing)
    // {user=null}
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="2.4.0" id="kotlin-2-4-0-getorput-diff"}

You can also use the `.getOrElseIfMissing()` and `.getOrPutIfMissing()` functions for caches that store nullable values.
If `defaultValue` returns `null`, the map stores it and doesn't call `defaultValue` again for the same key.
<br />对于存储可为空值的缓存，您还可以使用 `.getOrElseIfMissing()` 和 `.getOrPutIfMissing()` 函数。
如果 `defaultValue` 返回 `null`，则映射会存储该值，并且不会对同一个键再次调用 `defaultValue`。

Here's an example:
<br />举个例子：

```kotlin
data class Response(val body: String)

class Service {
    var queryCount = 0

    fun query(key: String): Response? {
        queryCount += 1
        return null
    }
}

//sampleStart
@OptIn(ExperimentalStdlibApi::class)
fun main() {
    val service = Service()
    val cache = mutableMapOf<String, Response?>()

    fun getCachedResponseOrQuery(key: String): Response? =
        cache.getOrPutIfMissing(key) { service.query(key) }

    // Stores null because the cache doesn't contain "user"
    getCachedResponseOrQuery("user")

    println(cache)
    // {user=null}

    // Uses the cached null and doesn't query the service again
    getCachedResponseOrQuery("user")

    println(service.queryCount)
    // 1
}
//sampleEnd
```
{kotlin-runnable="true" kotlin-min-compiler-version="2.4.0" id="kotlin-2-4-0-getorif-missing"}

We would appreciate your feedback in [YouTrack](https://youtrack.jetbrains.com/issue/KT-67337).
<br />我们非常感谢您通过 [YouTrack](https://youtrack.jetbrains.com/issue/KT-67337) 提供反馈。

## Kotlin/JVM

Kotlin 2.4.0 supports a new Java version and enables annotations in metadata by default.
<br />Kotlin 2.4.0 支持新的 Java 版本，并默认启用元数据中的注解。

### Support for Java 26
<secondary-label ref="jvm"/>
支持 Java 26

Starting with Kotlin 2.4.0, the compiler can generate classes containing Java 26 bytecode.
<br />从 Kotlin 2.4.0 开始，编译器可以生成包含 Java 26 字节码的类。

### Annotations in metadata enabled by default
<secondary-label ref="jvm"/>
元数据中的注释默认启用

The Kotlin Metadata JVM library in Kotlin 2.2.0 [introduced support for reading annotations stored in Kotlin metadata](whatsnew22.md#support-for-reading-and-writing-annotations-in-kotlin-metadata). With this support, the Kotlin compiler writes annotations into metadata alongside the JVM bytecode, making them accessible to the Kotlin Metadata JVM library. As a result, annotation processors and other tools can understand and manipulate these annotations at the metadata level without using reflection or modifying source code.
<br />Kotlin 2.2.0 中的 Kotlin Metadata JVM 库引入了对读取存储在 Kotlin 元数据中的注解的支持。有了这项支持，Kotlin 编译器会将注解与 JVM 字节码一起写入元数据，从而使 Kotlin Metadata JVM 库能够访问这些注解。因此，注解处理器和其他工具无需使用反射或修改源代码，即可在元数据级别理解和操作这些注解。

In Kotlin 2.4.0, this support is enabled by default.
<br />在 Kotlin 2.4.0 中，此功能默认启用。

## Kotlin/Native

Starting with Kotlin 2.4.0, [Swift export is promoted to Alpha](#swift-export-goes-alpha-with-improved-concurrency-support).
This release also brings support for [Swift package import](#swift-package-import), Xcode 26.4, improvements for memory consumption, and garbage collection.
<br />从 Kotlin 2.4.0 开始，[Swift export 已升级为 Alpha 版本](#swift-export-goes-alpha-with-improved-concurrency-support)。
此版本还支持 [Swift package import](#swift-package-import)、Xcode 26.4，并改进了内存消耗和垃圾回收机制。

### Default concurrent marking in garbage collector
<secondary-label ref="native"/>
垃圾回收器中的默认并发标记

In Kotlin 2.0.20, the Kotlin team [introduced experimental support](whatsnew2020.md#concurrent-marking-in-garbage-collector)
for the concurrent mark and sweep garbage collector (CMS GC). After processing user feedback and fixing regressions,
we are now ready to enable CMS by default, starting with Kotlin 2.4.0.
<br />在 Kotlin 2.0.20 中，Kotlin 团队[引入了对并发标记清除垃圾回收器 (CMS GC) 的实验性支持](whatsnew2020.md#concurrent-marking-in-garbage-collector)。
在处理用户反馈并修复回归问题后，我们现在准备从 Kotlin 2.4.0 开始默认启用 CMS。

The previous default parallel mark concurrent sweep (PMCS) setup in the garbage collector had to pause application
threads while the GC marked objects in the heap. In contrast, CMS allows the marking phase to run concurrently with application threads.
<br />之前垃圾回收器中默认的并行标记并发清除（PMCS）设置必须暂停应用程序线程，同时垃圾回收器标记堆中的对象。
相比之下，CMS 允许标记阶段与应用程序线程并发运行。

This significantly improves GC pause duration and app responsiveness, which is important for the performance of
latency-critical applications. CMS has already demonstrated its effectiveness in benchmarks for UI applications built with [Compose Multiplatform](https://blog.jetbrains.com/kotlin/2024/10/compose-multiplatform-1-7-0-released/#performance-improvements-on-ios).
<br />这显著改善了 GC 暂停时间和应用程序响应速度，这对延迟敏感型应用程序的性能至关重要。
CMS 已在基于 [Compose Multiplatform](https://blog.jetbrains.com/kotlin/2024/10/compose-multiplatform-1-7-0-released/#performance-improvements-on-ios) 构建的 UI 应用程序的基准测试中证明了其有效性。

If you face problems, you can switch back to PMCS. To do that, set the following [binary option](native-binary-options.md)
in your `gradle.properties` file:
<br />如果遇到问题，您可以切换回 PMCS。为此，请在 `gradle.properties` 文件中设置以下[二进制选项](native-binary-options.md)：

```none
kotlin.native.binary.gc=pmcs
```

For more information on the Kotlin/Native garbage collector, see our [documentation](native-memory-manager.md#garbage-collector).
<br />有关 Kotlin/Native 垃圾回收器的更多信息，请参阅我们的[文档](native-memory-manager.md#garbage-collector)。

### Reduced memory consumption during devirtualization analysis
<secondary-label ref="native"/>
降低去虚拟化分析期间的内存消耗

Previously, devirtualization analysis was one of the most memory-consuming phases in the Kotlin/Native compiler. Namely, the link release task consumed too much memory, especially in large projects.
<br />此前，去虚拟化分析是 Kotlin/Native 编译器中最消耗内存的阶段之一。具体来说，链接释放任务消耗了过多的内存，尤其是在大型项目中。

Kotlin 2.4.0 introduces improvements that help reduce peak memory consumption during link release tasks.
<br />Kotlin 2.4.0 引入了一些改进，有助于减少链接释放任务期间的峰值内存消耗。

According to benchmarks from one of our EAP users, the improved devirtualization analysis reduced memory consumption by link release tasks by half, saving at least 13 GB.
<br />根据我们一位 EAP 用户的基准测试，改进的去虚拟化分析将链路释放任务的内存消耗减少了一半，至少节省了 13 GB。

### Support for Xcode 26.4
<secondary-label ref="native"/>
支持 Xcode 26.4

Starting with Kotlin 2.4.0, the Kotlin/Native compiler supports Xcode 26.4 – one of the latest stable versions of Xcode.
<br />从 Kotlin 2.4.0 开始，Kotlin/Native 编译器支持 Xcode 26.4——这是 Xcode 的最新稳定版本之一。

You can now update your Xcode and get access to the latest APIs to continue working on your Kotlin projects for Apple operating systems.
<br />现在您可以更新 Xcode 并访问最新的 API，以便继续开发适用于 Apple 操作系统的 Kotlin 项目。

### LLVM update to version 21
<secondary-label ref="native"/>
LLVM 更新至版本 21

In Kotlin 2.4.0, we updated LLVM from version 19 to 21. The new version includes performance improvements and helps keep the Kotlin/Native compiler up to date.
<br />在 Kotlin 2.4.0 中，我们将 LLVM 从版本 19 更新到了版本 21。新版本包含性能改进，并有助于保持 Kotlin/Native 编译器的最新状态。

This update shouldn't affect your code, but if you encounter any issues, please report them to our [issue tracker](http://kotl.in/issue).
<br />本次更新不应影响您的代码，但如果您遇到任何问题，请向我们的[问题跟踪器](http://kotl.in/issue)报告。

### Changes to Apple target support
<secondary-label ref="native"/>
苹果目标支持变更

Kotlin 2.4.0 raises the default minimum supported versions of Apple targets:
<br />Kotlin 2.4.0 提高了对 Apple 目标平台的默认最低支持版本要求：

* For iOS and tvOS, from 14.0 to 15.0.
* For macOS, from 11.0 to 12.0.
* For watchOS, from 7.0 to 8.0.

If you need to support a lower version in your project than the default one, use the `freeCompilerArgs` option in your build file:
<br />如果你的项目需要支持比默认版本更低的编译器版本，请在构建文件中使用 `freeCompilerArgs` 选项：

```kotlin
kotlin {
    targets.withType<org.jetbrains.kotlin.gradle.plugin.mpp.KotlinNativeTarget>().configureEach {
        binaries.configureEach {
            freeCompilerArgs += "-Xoverride-konan-properties=minVersion.ios=14.0"
            freeCompilerArgs += "-Xoverride-konan-properties=minVersion.macos=11.0"
            freeCompilerArgs += "-Xoverride-konan-properties=minVersion.tvos=14.0"
            freeCompilerArgs += "-Xoverride-konan-properties=minVersion.watchos=7.0"
        }
    }
}
```

### Swift export goes Alpha with improved concurrency support
<primary-label ref="alpha"/>
Swift 导出功能进入 Alpha 测试阶段，改进了并发支持

<secondary-label ref="native"/>

Starting with Kotlin 2.4.0, Kotlin's interoperability with Swift through Swift export is officially in Alpha!
This release brings major improvements to concurrency support, adding native and direct structured concurrency to Swift
export and the ability to export `kotlinx.coroutines` flows to Swift.
<br />从 Kotlin 2.4.0 开始，Kotlin 通过 Swift 导出与 Swift 的互操作性正式进入 Alpha 阶段！
此版本对并发支持进行了重大改进，为 Swift 导出添加了原生和直接结构化并发支持，并支持将 `kotlinx.coroutines` 流程导出到 Swift。

#### Support for structured concurrency
支持结构化并发
You can now seamlessly call suspending Kotlin code from Swift. Kotlin [`suspend` functions](composing-suspending-functions.md)
and suspend functional types are exported as Swift's idiomatic `async` counterparts:
<br />现在，您可以从 Swift 中无缝调用 Kotlin 的挂起代码。
Kotlin 的 [`suspend` 函数](composing-suspending-functions.md)和挂起函数类型已导出为 Swift 惯用的 `async` 对应函数：

```kotlin
// Kotlin
suspend fun hello(): String {
    delay(1000)
    return "Hello Swift! This is Kotlin."
}
```

```swift
// Swift
let msg = try await hello()
```
#### Export of flow types to Swift
将流程类型导出到 Swift

This update also adds support for exporting `kotlinx.coroutines` flows to Swift. Flows in `kotlinx.coroutines` represent
an asynchronous stream of data that can be emitted and consumed concurrently. They are commonly used for reactive programming
patterns, such as listening for database updates, network requests, or UI events.
<br />此次更新还增加了将 `kotlinx.coroutines` 流程导出到 Swift 的支持。
`kotlinx.coroutines` 中的流程表示可以并发发出和消费的异步数据流。
它们通常用于响应式编程模式，例如监听数据库更新、网络请求或 UI 事件。

Previously, the only way to expose the `Flow` interface from [`kotlinx.coroutines.flow`](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-flow/)
to Swift was through third-party solutions. Now you can export flows out of the box into Swift's idiomatic counterpart: [`AsyncSequence`](https://developer.apple.com/documentation/Swift/AsyncSequence).
<br />以前，将 `kotlinx.coroutines.flow` 接口暴露给 Swift 的唯一方法是通过第三方解决方案。
现在，您可以直接将流程导出为 Swift 的惯用接口：`AsyncSequence`。

The feature is enabled by default. You can export any public API with the `Flow` type to Swift while preserving type information.
For example:
<br />此功能默认启用。您可以将任何 `Flow` 类型的公共 API 导出到 Swift，同时保留类型信息。
例如：

```kotlin
// Kotlin
// Type String is preserved when exporting Flow
fun flowOfStrings(): Flow<String> = flowOf("hello", "any", "world")
```

```Swift
// Swift
var actual: [String] = []

// Type String is correctly inferred from Kotlin
for try await element in flowOfStrings().asAsyncSequence() {
    actual.append(element)
}
```

For more information about Swift export, see our [documentation](native-swift-export.md).
<br />有关 Swift 导出的更多信息，请参阅我们的[文档](native-swift-export.md)。

### Swift package import
<primary-label ref="experimental-general"/>
Swift 包导入

<secondary-label ref="native"/>

Kotlin Multiplatform projects now can declare [Swift packages](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/) as dependencies for an iOS app in their Gradle configuration:
<br />Kotlin 多平台项目现在可以在 Gradle 配置中将 [Swift 包](https://docs.swift.org/swiftpm/documentation/packagemanagerdocs/) 声明为 iOS 应用的依赖项：

```kotlin
// build.gradle.kts
kotlin {
    swiftPMDependencies {
        swiftPackage(
            url = url("https://github.com/firebase/firebase-ios-sdk.git"),
            version = from("12.11.0"),
            products = listOf(
                product("FirebaseAI"),
                product("FirebaseAnalytics"),
                ...
}
```
{validate="false"}

For working samples and more detailed information, see [SwiftPM import](https://kotlinlang.org/docs/multiplatform/multiplatform-spm-import.html).
<br />有关工作示例和更详细的信息，请参阅[SwiftPM 导入](https://kotlinlang.org/docs/multiplatform/multiplatform-spm-import.html)。

If your project relies on CocoaPods dependencies, you can migrate the current setup to use Swift packages. The KMP tooling
accounts for this use case and helps you reconfigure the project automatically. For details, see our [CocoaPods migration guide](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration.html).
<br />如果您的项目依赖于 CocoaPods 依赖项，您可以将当前配置迁移到使用 Swift 包。
KMP 工具考虑到了这种情况，可以帮助您自动重新配置项目。详情请参阅我们的[CocoaPods 迁移指南](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration.html)。

## Kotlin/Wasm

Kotlin 2.4.0 enables incremental compilation for Kotlin/Wasm by default and introduces support for the WebAssembly Component Model.
<br />Kotlin 2.4.0 默认启用 Kotlin/Wasm 的增量编译，并引入了对 WebAssembly 组件模型的支持。

### Incremental compilation enabled by default
<secondary-label ref="wasm"/>
默认启用增量编译

Kotlin/Wasm introduced incremental compilation in Kotlin 2.1.0. Starting with Kotlin 2.4.0, it is [Stable](components-stability.md#stability-levels-explained) and enabled by default.
With this feature, the compiler rebuilds only the files affected by recent changes, which significantly reduces build time.
<br />Kotlin/Wasm 在 Kotlin 2.1.0 中引入了增量编译。
从 Kotlin 2.4.0 开始，增量编译已[稳定](components-stability.md#stability-levels-explained)，并且默认启用。
借助此功能，编译器仅重新构建受最近更改影响的文件，从而显著缩短构建时间。

To disable incremental compilation, add the following line to your project's `local.properties` or `gradle.properties` file:
<br />要禁用增量编译，请将以下行添加到项目的 `local.properties` 或 `gradle.properties` 文件中：

```none
# gradle.properties
kotlin.incremental.wasm=false
```

If you run into any issues, report them in [YouTrack](https://kotl.in/issue)
<br />如果您遇到任何问题，请在 [YouTrack](https://kotl.in/issue) 中报告。

### Improved display of internal variables in Chrome DevTools
<secondary-label ref="wasm"/>
改进了Chrome开发者工具中内部变量的显示

Kotlin 2.4.0 improves the debugging experience for Kotlin/Wasm in Chrome DevTools by making temporary, synthetic,
and internal variables easier to distinguish from user-defined variables.
<br />Kotlin 2.4.0 改进了 Chrome DevTools 中 Kotlin/Wasm 的调试体验，使临时变量、合成变量和内部变量更容易与用户定义的变量区分开来。

The Kotlin compiler and compiler plugins, such as Compose, can generate these variables. They now use the `~` prefix
by default, so they are grouped together and moved to the end of the variable list, which Chrome DevTools sorts by name.
<br />Kotlin 编译器和编译器插件（例如 Compose）可以生成这些变量。
它们现在默认使用 `~` 前缀，因此会被分组并移动到变量列表的末尾，而 Chrome 开发者工具会按名称对变量列表进行排序。

### Support for the WebAssembly Component Model
<primary-label ref="experimental-general"/>
支持 WebAssembly 组件模型

<secondary-label ref="wasm"/>

Kotlin/Wasm goes a step further in Kotlin 2.4.0 by introducing experimental support for the [WebAssembly Component Model](https://component-model.bytecodealliance.org/).
The proposal defines a way to build components from Wasm modules through standardized interfaces and types. This approach
helps Wasm evolve from a low-level binary instruction format into a system for composing reusable, language-agnostic components.
It enables Kotlin/Wasm to go beyond the browser. For example, Kotlin and WebAssembly are well suited for Function-as-a-Service,
also known as FaaS or serverless, applications.
<br />Kotlin/Wasm 在 Kotlin 2.4.0 中更进一步，引入了对 [WebAssembly 组件模型](https://component-model.bytecodealliance.org/) 的实验性支持。
该提案定义了一种通过标准化接口和类型从 Wasm 模块构建组件的方法。
这种方法有助于 Wasm 从底层二进制指令格式演进为一个用于组合可重用、与语言无关的组件的系统。
它使 Kotlin/Wasm 的应用范围超越了浏览器。
例如，Kotlin 和 WebAssembly 非常适合函数即服务 (FaaS)，也称为 FaaS 或无服务器应用程序。

To try this feature, check out [a simple server built with `wasi:http`](https://github.com/Kotlin/sample-wasi-http-kotlin/).
<br />要尝试此功能，请查看[使用 `wasi:http` 构建的简单服务器](https://github.com/Kotlin/sample-wasi-http-kotlin/)。

<img src="kotlin-wasm-wasi-http.gif" alt="Kotlin/Wasm with WebAssembly Component Model" width="600"/>

Share your feedback in [YouTrack](https://youtrack.jetbrains.com/issue/KT-64569/Kotlin-Wasm-Support-Component-Model).
<br />请在 [YouTrack](https://youtrack.jetbrains.com/issue/KT-64569/Kotlin-Wasm-Support-Component-Model) 中分享您的反馈。

## Kotlin/JS

Kotlin 2.4.0 further improves export to JavaScript/TypeScript, including support for exporting value classes, interfaces,
and type variance, as well as ES2015 features when inlining JS code.
<br />Kotlin 2.4.0 进一步改进了导出到 JavaScript/TypeScript 的功能，包括支持导出值类、接口和类型变体，以及在内联 JS 代码时支持 ES2015 特性。

### Support for value class export to JavaScript/TypeScript
<secondary-label ref="js"/>
支持将值类导出到 JavaScript/TypeScript

Previously, only regular Kotlin classes could be exported to JavaScript/TypeScript.
Kotlin 2.4.0 lifts that limitation. You can now export Kotlin's [inline value classes](inline-classes.md) as regular TypeScript classes.
<br />此前，只有常规的 Kotlin 类才能导出为 JavaScript/TypeScript。
Kotlin 2.4.0 解除了这一限制。现在，您可以将 Kotlin 的[内联值类](inline-classes.md)导出为常规的 TypeScript 类。

To export a value class, mark it with the `@JsExport` annotation on the Kotlin side:
<br />要导出值类，请在 Kotlin 端使用 `@JsExport` 注解对其进行标记：

```Kotlin
// Kotlin
@JsExport
@JvmInline
value class Email(val address: String) {
    init { require(address.contains("@")) { "Invalid email" } }
}

@JsExport
class AuthService {
    suspend fun login(email: Email): String = ...
}
```

From the TypeScript side, it looks like a regular class:
<br />从 TypeScript 的角度来看，它看起来像一个普通的类：

```TypeScript
// TypeScript
import { AuthService, Email } from "..."
const auth = new AuthService();

console.log(await auth.login(new Email("jane@example.com"))); 
// "Welcome, jane@example.com!"
console.log(await auth.login(new Email("not-an-email"))); 
// "Invalid email"
```

For more information, see [`@JsExport` annotation](js-to-kotlin-interop.md#jsexport-annotation).
<br />有关更多信息，请参阅 [`@JsExport` 注解](js-to-kotlin-interop.md#jsexport-annotation)。

### Support for ES2015 features when inlining JS code
<secondary-label ref="js"/>
内联 JS 代码时支持 ES2015 特性

Starting with Kotlin 2.4.0, JavaScript code inlining has full support for [ES2015 features](js-project-setup.md#support-for-es2015-features).
<br />从 Kotlin 2.4.0 开始，JavaScript 代码内联完全支持 [ES2015 特性](js-project-setup.md#support-for-es2015-features)。

It's useful for interoperability with third-party libraries, as well as for direct control over automatic application code generation.
<br />它有助于与第三方库进行互操作，并可直接控制自动应用程序代码生成。

Now you can use modern JS features inside [`js()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.js/js.html) calls, including:
<br />现在您可以在 [`js()`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.js/js.html) 调用中使用现代 JS 特性，包括：

* `const` and `let` variable declarations
  <br />`const` 和 `let` 变量声明
* ES classes
  <br />ES 类
* Generators
* Lambdas ([arrow functions](whatsnew21.md#support-for-generating-es2015-arrow-functions))
  <br />Lambda 表达式（[箭头函数](whatsnew21.md#support-for-generating-es2015-arrow-functions)）
* Spread and rest operators
* Template strings
  <br />模板字符串

Remember that the parameter of the `js()` function should be a string constant because it's parsed at compile time and translated to JavaScript code "as-is".
For example, to inline the spread operator, use:
<br />请记住，`js()` 函数的参数应该是字符串常量，因为它会在编译时被解析并 "原样" 转换为 JavaScript 代码。
例如，要内联扩展运算符，请使用：

```kotlin
fun spreadExample(): dynamic = js("""
    const add = (a, b, c) => a + b + c;

    const nums = [1, 2, 3];
    const sum = add(...nums);

    const a = [1, 2, 3];
    const b = [...a, 4, 5, 6];

    return { sum, b: b };
""")
```

For more information on inlining JavaScript code, see [our documentation](js-interop.md#inline-javascript).
<br />有关内联 JavaScript 代码的更多信息，请参阅[我们的文档](js-interop.md#inline-javascript)。

### Preserve type variance when exporting to TypeScript
<secondary-label ref="js"/>
导出为 TypeScript 时保留类型差异

Previously, Kotlin [variance](generics.md#variance) information in generic positions was lost when exporting types to TypeScript.
<br />以前，当将类型导出到 TypeScript 时，泛型位置中的 Kotlin [variance](generics.md#variance) 信息会丢失。

With Kotlin 2.4.0, variance annotation is now saved during export and mapped to TypeScript's [variance annotations](https://www.typescriptlang.org/docs/handbook/2/generics.html#variance-annotations).
<br />从 Kotlin 2.4.0 开始，variance 注解现在会在导出期间保存，并映射到 TypeScript 的 [variance 注解](https://www.typescriptlang.org/docs/handbook/2/generics.html#variance-annotations)。

In your Kotlin code, define the variance of your generic type parameters:
<br />在你的 Kotlin 代码中，定义泛型类型参数的方差：

```Kotlin
// Kotlin
// 'out' signals covariance (the interface only produces T)
interface Producer<out T> {
    fun produce(): T
}

// 'in' signals contravariance (the interface only consumes T)
interface Consumer<in T> {
    fun consume(item: T)
}
```

With Kotlin 2.4.0, the `in` and `out` keywords are preserved in the generated TypeScript output:
<br />在 Kotlin 2.4.0 中，生成的 TypeScript 输出会保留 `in` 和 `out` 关键字：

```TypeScript
// Generated .d.ts
export interface Producer<out T> {
    produce(): T;
}

export interface Consumer<in T> {
    consume(item: T): void;
}
```

### Improved interface export to JavaScript/TypeScript
<secondary-label ref="js"/>
改进了导出到 JavaScript/TypeScript 的接口

Kotlin 2.4.0 makes it more convenient to export Kotlin interfaces to JavaScript/TypeScript.
<br />Kotlin 2.4.0 使得将 Kotlin 接口导出到 JavaScript/TypeScript 更加方便。

The new `@JsNoRuntime` annotation removes the previously required metadata for implementing Kotlin interfaces, allowing
the direct mapping to regular TypeScript interfaces, similar to how external interfaces already behave by default.
<br />新的 `@JsNoRuntime` 注解移除了之前实现 Kotlin 接口所需的元数据，从而允许直接映射到常规 TypeScript 接口，类似于外部接口的默认行为。

To export a Kotlin interface, for example in your Kotlin Multiplatform project, annotate it with `@JsNoRuntime` in the common code:
<br />例如，要在 Kotlin 多平台项目中导出 Kotlin 接口，请在公共代码中使用 `@JsNoRuntime` 注解它：

```kotlin
// commonMain
import kotlin.js.JsNoRuntime

@JsNoRuntime
expect interface DataProcessor {
    fun process(data: String): Int 
}
```

Then provide the actual implementation in your JS-specific source code:
<br />然后，请在您的 JS 源代码中提供实际的实现：

```kotlin
// jsMain
@JsNoRuntime
actual interface DataProcessor {
    actual fun process(data: String)
} 
```

Because the required metadata for implementing Kotlin interfaces is removed, the interface is mapped to a regular TypeScript interface:
<br />由于实现 Kotlin 接口所需的元数据已被移除，因此该接口被映射到常规的 TypeScript 接口：

```TypeScript
// Generated .d.ts
export interface DataProcessor {
    process(data: string): void;
}
```

The `@JsNoRuntime` annotation is only allowed on standard interfaces, so that TypeScript can treat Kotlin interfaces as
regular TypeScript interfaces. Therefore, the following operations are prohibited:
<br />`@JsNoRuntime` 注解仅允许用于标准接口，以便 TypeScript 可以将 Kotlin 接口视为常规的 TypeScript 接口。因此，禁止执行以下操作：

* `is` and `as` type checks.
  <br />`is` 和 `as` 类型检查。
* Class references with the [`::class` syntax](js-reflection.md).
  <br />使用 [`::class` syntax](js-reflection.md) 进行类引用。
* Passing an interface as a reified type argument.
  <br />将接口作为具体化的类型参数传递。

> Avoid annotating external interfaces with `@JsNoRuntime`, as this results in a compiler warning.
> <br />避免使用 `@JsNoRuntime` 注解外部接口，因为这会导致编译器警告。
>
{type="note"}

### Lifting restrictions on exporting interfaces
<primary-label ref="experimental-general"/>
解除对接口出口的限制

<secondary-label ref="js"/>

Kotlin 2.4.0 makes another step toward the stabilization of `@JsExport`, improving how Kotlin interfaces are exported.
<br />Kotlin 2.4.0 在稳定 `@JsExport` 方面又迈出了一步，改进了 Kotlin 接口的导出方式。

Now you can export Kotlin interfaces with nested classes and named companion objects:
<br />现在您可以导出包含嵌套类和命名伴生对象的 Kotlin 接口：

```kotlin
@JsExport
interface Identity {
    class Metadata(val tag: String)

    companion object Registry {
        val defaultTag = "GUEST"
    }
}
```

For more information, see [`@JsExport` annotation](js-to-kotlin-interop.md#jsexport-annotation).
<br />有关更多信息，请参阅 [`@JsExport` 注解](js-to-kotlin-interop.md#jsexport-annotation)。

## Gradle

Kotlin 2.4.0 is fully compatible with Gradle 7.6.3 through 9.5.0. You can also use Gradle versions up to
the latest Gradle release. However, be aware that doing so may result in deprecation warnings, and some new Gradle features might not work.
Kotlin 2.4.0 also brings improvements like consistent default module names across platforms and compiler messages written
to the Problems API for the Kotlin/JVM.
<br />Kotlin 2.4.0 完全兼容 Gradle 7.6.3 至 9.5.0 版本。您也可以使用 Gradle 的最新版本。
但是，请注意，这样做可能会导致弃用警告，并且某些新的 Gradle 功能可能无法正常工作。
Kotlin 2.4.0 还带来了一些改进，例如跨平台一致的默认模块名称，以及将编译器消息写入 Kotlin/JVM 的 Problems API。

### Minimum supported AGP version bumped to 8.5.2
<secondary-label ref="gradle"/>
支持的最低 AGP 版本升至 8.5.2

Starting with Kotlin 2.4.0, the minimum supported Android Gradle plugin version is 8.5.2.
<br />从 Kotlin 2.4.0 开始，支持的最低 Android Gradle 插件版本为 8.5.2。

### Consistent module names across platforms
<secondary-label ref="gradle"/>
跨平台保持一致的模块名称

Prior to Kotlin 2.4.0, default module names differed across platforms. This inconsistency could cause naming conflicts 
and resolution issues. Kotlin 2.4.0 standardizes the default names to `{group}:{project_name}` across all platforms.
<br />在 Kotlin 2.4.0 之前，不同平台上的默认模块名称各不相同。
这种不一致性可能会导致命名冲突和解析问题。Kotlin 2.4.0 将所有平台上的默认名称统一为 `{group}:{project_name}`。

If you need to revert the JVM module name to its previous version, add the following to your `build.gradle.kts` file for a Kotlin/JVM project:
<br />如果需要将 JVM 模块名称还原到之前的版本，请将以下内容添加到 Kotlin/JVM 项目的 `build.gradle.kts` 文件中：

```kotlin
kotlin {
    compilerOptions.moduleName(project.name)
}
```

For a multiplatform project:
<br />对于跨平台项目：

```kotlin
kotlin {
    jvm {
        compilerOptions.moduleName(project.name)
    }
}
```

### Compiler messages written to Problems API for Kotlin/JVM
<secondary-label ref="gradle"/>
编译器消息已写入 Kotlin/JVM 的问题 API

In Kotlin 2.2.0, the Kotlin Gradle plugin (KGP) started reporting diagnostics to [Gradle's Problems API](https://docs.gradle.org/current/userguide/reporting_problems.html)
to provide a consistent experience both in Gradle's CLI and in IntelliJ IDEA.
<br />在 Kotlin 2.2.0 中，Kotlin Gradle 插件 (KGP) 开始向 [Gradle 的问题 API](https://docs.gradle.org/current/userguide/reporting_problems.html) 报告诊断信息，
以便在 Gradle 的 CLI 和 IntelliJ IDEA 中提供一致的体验。

In Kotlin 2.4.0, the plugin also writes compiler messages to the Problems API for Kotlin/JVM, bringing the API closer to
becoming a single source for all logs and messages.
<br />在 Kotlin 2.4.0 中，该插件还会将编译器消息写入 Kotlin/JVM 的问题 API，使该 API 更接近于成为所有日志和消息的单一来源。

## Maven

Kotlin 2.4.0 makes project configuration even easier with support for Maven Toolchains and automatic alignment between Java and JVM target versions.
<br />Kotlin 2.4.0 支持 Maven 工具链，并实现了 Java 和 JVM 目标版本之间的自动对齐，使项目配置更加容易。

### Automatic alignment between Java and JVM target versions
<secondary-label ref="maven"/>
Java 和 JVM 目标版本之间的自动对齐

To simplify project configuration and prevent compatibility issues, the Kotlin Maven plugin now automatically aligns the
JVM target version with the Java compiler version configured in the project.
<br />为了简化项目配置并防止兼容性问题，Kotlin Maven 插件现在会自动将 JVM 目标版本与项目中配置的 Java 编译器版本保持一致。

This ensures that the Kotlin and Maven compilers target the same bytecode version, avoiding issues where Kotlin-generated
bytecode is incompatible with the rest of the project or the intended deployment environment.
<br />这样可以确保 Kotlin 和 Maven 编译器针对相同的字节码版本，避免 Kotlin 生成的字节码与项目的其他部分或预期的部署环境不兼容的问题。

With the `<extensions>` option enabled, you don't need to set the `kotlin.compiler.jvmTarget` or `kotlin.compiler.jdkRelease` options.
If neither of them is defined, the Kotlin Maven plugin automatically resolves the JVM target version in the following order:
<br />启用 `<extensions>` 选项后，无需设置 `kotlin.compiler.jvmTarget` 或 `kotlin.compiler.jdkRelease` 选项。
如果这两个选项均未定义，Kotlin Maven 插件将按以下顺序自动解析 JVM 目标版本：

1. As the `maven.compiler.release` version defined either as a project property or within the `maven-compiler-plugin` configuration.
   <br />作为项目属性或在 `maven-compiler-plugin` 配置中定义的 `maven.compiler.release` 版本。

   In this case, both `jvmTarget` and `jdkRelease` compiler options are set for the Kotlin compiler, limiting the API to a specific JDK version.
   <br />在这种情况下，Kotlin 编译器同时设置了 `jvmTarget` 和 `jdkRelease` 编译器选项，将 API 限制为特定的 JDK 版本。

2. As the `maven.compiler.target` version in case the Maven release version is not set. The compiler target can be defined either as a project property or within the `maven-compiler-plugin` configuration.
   <br />如果未设置 Maven 发布版本，则使用 `maven.compiler.target` 版本。编译器目标可以定义为项目属性，也可以在 `maven-compiler-plugin` 配置中定义。

   In this case, only Kotlin's `jvmTarget` is set, and the API is not limited to a specific JDK version.
   <br />在这种情况下，仅设置了 Kotlin 的 `jvmTarget`，并且 API 不限于特定的 JDK 版本。

This greatly simplifies your Kotlin project configuration, so your `pom.xml` file can look like this:
<br />这大大简化了您的 Kotlin 项目配置，因此您的 `pom.xml` 文件可以如下所示：

```xml
<properties>
    <maven.compiler.release>17</maven.compiler.release>
    <kotlin.version>%kotlinVersion%</kotlin.version>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <version>${kotlin.version}</version>
            <extensions>true</extensions>
        </plugin>
    </plugins>
</build>
```

During the build, the plugin outputs a similar message:
<br />构建过程中，插件会输出类似的消息：

```none
[INFO] Using jvmTarget=17 (derived from maven.compiler.release=17)
```

> The `<extensions>` option only checks project-level properties and the global `maven-compiler-plugin` configuration.
> It doesn't check the configurations defined in the plugin's `<executions>` section.
> <br />`<extensions>` 选项仅检查项目级别的属性和全局 `maven-compiler-plugin` 配置。 
> 它不会检查插件的 `<executions>` 部分中定义的配置。
>
{style="note"}

For more information about automatic project configuration, see [our documentation](maven-configure-project.md#jvm-target-version).
<br />有关自动项目配置的更多信息，请参阅[我们的文档](maven-configure-project.md#jvm-target-version)。

### Support for Maven Toolchains
<secondary-label ref="maven"/>
支持 Maven 工具链

Kotlin 2.4.0 introduces support for [Maven Toolchains](https://maven.apache.org/guides/mini/guide-using-toolchains.html) to the Kotlin Maven plugin.
<br />Kotlin 2.4.0 为 Kotlin Maven 插件引入了对 [Maven 工具链](https://maven.apache.org/guides/mini/guide-using-toolchains.html) 的支持。

The feature helps manage the JDK version in your build. With Maven Toolchains, you can specify the JDK version used for 
Kotlin compilation, independent of the JVM version running Maven (set in `JAVA_HOME`). When the `maven-toolchains-plugin`
is configured in the build, the Kotlin Maven plugin automatically picks up the selected JDK toolchain, in the same way
the Maven compiler plugin and other Maven plugins do. This allows you to configure a single toolchain to control the JDK
used across all plugins in the build, including Kotlin compilation:
<br />此功能有助于管理构建中的 JDK 版本。借助 Maven 工具链，您可以指定用于 Kotlin 编译的 JDK 版本，而无需考虑运行 Maven 的 JVM 版本（在 `JAVA_HOME` 中设置）。
当在构建中配置了 `maven-toolchains-plugin` 插件后，Kotlin Maven 插件会自动选择 JDK 工具链，就像 Maven 编译器插件和其他 Maven 插件一样。
这样，您只需配置一个工具链即可控制构建中所有插件（包括 Kotlin 编译）使用的 JDK。

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-toolchains-plugin</artifactId>
    <version>3.2.0</version>
    <executions>
        <execution>
            <goals>
                <goal>toolchain</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <toolchains>
            <jdk>
                <version>21</version>
            </jdk>
        </toolchains>
    </configuration>
</plugin>
```
Keep in mind the priority of different ways to set up the JDK version:
<br />请注意设置 JDK 版本不同方法的优先级：

1. `jdkHome` in the `kotlin-maven-plugin` configuration. An explicitly set `jdkHome` option always takes precedence over the toolchain version. 
   <br />`kotlin-maven-plugin` 配置中的 `jdkHome` 选项。显式设置的 `jdkHome` 选项始终优先于工具链版本。
2. JDK version in `maven-toolchains-plugin`. The JDK version set through Maven Toolchains overrides the JDK version set in the `JAVA_HOME` path.
   <br />`maven-toolchains-plugin` 中的 JDK 版本。通过 Maven 工具链设置的 JDK 版本会覆盖在 `JAVA_HOME` 路径中设置的 JDK 版本。
3. The `JAVA_HOME` path.
   <br />`JAVA_HOME` 路径。

You can also use a plugin-specific `<jdkToolchain>` option to directly set the JDK version in the toolchain of `kotlin-maven-plugin`.
Compared to using `maven-toolchains-plugin`, this parameter only affects Kotlin compilation and has no impact on other plugins in the build.
<br />您还可以使用特定于插件的 `<jdkToolchain>` 选项直接在 `kotlin-maven-plugin` 工具链中设置 JDK 版本。
与使用 `maven-toolchains-plugin` 相比，该参数仅影响 Kotlin 编译，对构建中的其他插件没有影响。

> Currently, setting `maven-toolchains-plugin` to use a specific JDK version does not affect the `kapt` and `test-kapt` 
> goals of `kotlin-maven-plugin`. To work around this, set the necessary version in the `JAVA_HOME` path. For more details,
> see [KT-79897](https://youtrack.jetbrains.com/issue/KT-79897).
> <br />目前，将 `maven-toolchains-plugin` 设置为使用特定的 JDK 版本不会影响 `kotlin-maven-plugin` 的 `kapt` 和 `test-kapt` 目标。
> 要解决此问题，请在 `JAVA_HOME` 路径中设置所需的版本。更多详情，请参阅 [KT-79897](https://youtrack.jetbrains.com/issue/KT-79897)。
>
{style="note"}

For more information on configuring Kotlin Maven projects, see our [documentation](maven-configure-project.md).
<br />有关配置 Kotlin Maven 项目的更多信息，请参阅我们的[文档](maven-configure-project.md)。

## Build tools API
<secondary-label ref="bta"/>
构建工具 API

Kotlin 2.4.0 brings a number of improvements to the build tools API (BTA). The BTA:
<br />Kotlin 2.4.0 对构建工具 API (BTA) 进行了多项改进。BTA：

* Introduces new type-safe abstractions for most JVM and common compiler options. The BTA now handles their format instead of the client, reducing the risk of errors and providing an additional layer of assistance. This change is backwards-compatible at runtime, but it may break source compatibility.
  <br />为大多数 JVM 和常用编译器选项引入了新的类型安全抽象。BTA 现在负责处理它们的格式，而不是由客户端处理，从而降低了出错风险并提供额外的辅助层。此更改在运行时向后兼容，但可能会破坏源代码兼容性。
* Can now track non-source changes in incremental compilation, such as configuring a different Kotlin version or changing compiler options. Build systems can control this behavior through the `BaseIncrementalCompilationConfiguration.TRACK_CONFIGURATION_INPUTS` option.
  <br />现在可以跟踪增量编译中的非源代码更改，例如配置不同的 Kotlin 版本或更改编译器选项。构建系统可以通过 `BaseIncrementalCompilationConfiguration.TRACK_CONFIGURATION_INPUTS` 选项控制此行为。
* Supports [binary compatibility validation](gradle-binary-compatibility-validation.md) through the `AbiValidationToolchain`, making it easier for other build systems to add this functionality.
  <br />通过 `AbiValidationToolchain` 支持 [二进制兼容性验证](gradle-binary-compatibility-validation.md)，使其他构建系统更容易添加此功能。
* Introduces a new feature so that build systems can customize how compiler messages are displayed through the [`CompilerMessageRenderer`](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/CompilerMessageRenderer.kt) interface and the [`JvmCompilationOperation` builder](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/jvm/operations/JvmCompilationOperation.kt#L59).
  <br />引入了一项新功能，使构建系统能够通过 [`CompilerMessageRenderer`](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/CompilerMessageRenderer.kt) 接口和 [`JvmCompilationOperation` 构建器](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/jvm/operations/JvmCompilationOperation.kt#L59) 自定义编译器消息的显示方式。
* Introduces new options for configuring [Kotlin daemon](kotlin-daemon.md) logging:
  <br />引入了配置 Kotlin 守护进程日志记录的新选项：
  * `LOGS_PATH` — the directory for daemon log files.
    <br />`LOGS_PATH` — 守护进程日志文件的目录。
  * `LOGS_FILE_SIZE_LIMIT` — the maximum log file size in bytes.
    <br />`LOGS_FILE_SIZE_LIMIT` — 日志文件的最大大小（以字节为单位）。
  * `LOGS_FILE_COUNT_LIMIT` —  the maximum number of retained log files.
    <br />LOGS_FILE_COUNT_LIMIT` — 保留的日志文件的最大数量。

  By default, limits are set to a value specific to the Kotlin compiler version. To have no limit, build tools must set the option to `null`.
  <br />默认情况下，限制值取决于 Kotlin 编译器版本。要取消限制，构建工具必须将此选项设置为 `null`。

  Build systems can set the option when configuring the [execution policy](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/ExecutionPolicy.kt):
  <br />构建系统可以在配置[执行策略](https://github.com/JetBrains/kotlin/blob/2.4.0/compiler/build-tools/kotlin-build-tools-api/src/main/kotlin/org/jetbrains/kotlin/buildtools/api/ExecutionPolicy.kt)时设置该选项：

  ```kotlin
  val executionPolicy = kotlinToolchains.daemonExecutionPolicy {
      set(ExecutionPolicy.WithDaemon.LOGS_PATH, Paths("/var/log/kotlin-daemon"))
      set(ExecutionPolicy.WithDaemon.LOGS_FILE_SIZE_LIMIT, 10_485_760L)
      set(ExecutionPolicy.WithDaemon.LOGS_FILE_COUNT_LIMIT, 10)
  }
  ```

## Kotlin compiler
Kotlin 编译器

Kotlin 2.4.0 includes more consistent behavior for inline functions declared in the same module during `.klib` compilation.
<br />Kotlin 2.4.0 为在 `.klib` 编译期间在同一模块中声明的内联函数引入了更一致的行为。

### Consistent intra-module function inlining during klib compilation
<secondary-label ref="compiler"/>
klib编译期间一致的模块内函数内联

Previously, [function inlining](inline-functions.md) behaved inconsistently on different Kotlin platforms. The JetBrains
team is working to unify it across all supported platforms to ensure the same compatibility guarantees.
<br />此前，[函数内联](inline-functions.md)在不同的 Kotlin 平台上表现不一致。JetBrains 团队正在努力统一所有受支持平台上的函数内联行为，以确保相同的兼容性。

On the Kotlin/JVM, function inlining happens at compile time. So, when Kotlin sources are compiled with the Kotlin/JVM
compiler, the resulting class files have no inline function calls in the bytecode because the bodies of inline functions
are inlined into their call sites, so their behavior is fixed during compilation.
<br />在 Kotlin/JVM 中，函数内联发生在编译时。因此，当使用 Kotlin/JVM 编译器编译 Kotlin 源代码时，
生成的类文件中的字节码中不会包含内联函数调用，因为内联函数的函数体
已被内联到其调用位置，因此它们的行为在编译期间就被固定下来了。

On the contrary, on Kotlin/Native, Kotlin/JS, and Kotlin/Wasm, function inlining did not happen during source-to-klib
compilation, only during binary generation. As a result, the behavior of inline functions wasn't fixed during `.klib` compilation,
and `.klib` libraries didn't provide the same compatibility guarantees for inline functions as Kotlin/JVM does.
<br />相反，在 Kotlin/Native、Kotlin/JS 和 Kotlin/Wasm 中，函数内联并非在源文件到 klib 的编译过程中进行，而仅在二进制文件生成过程中进行。
因此，内联函数的行为在 `.klib` 编译过程中并未得到固定，
并且 `.klib` 库无法像 Kotlin/JVM 那样为内联函数提供相同的兼容性保证。

Kotlin 2.4.0 takes the first step in unifying the behavior of inline functions by enabling intra-module
inlining when generating `.klib` artifacts:
<br />Kotlin 2.4.0 通过启用模块内内联，迈出了统一内联函数行为的第一步。
在生成 `.klib` 工件时启用内联：

```kotlin
// Existing logging.klib library
inline fun logDebug(message: String) {
    println("[DEBUG] $message")
}
```

```kotlin
// Currently compiled App module
inline fun greetUser(name: String) {
    println("Hello, $name!")
}

fun main() {
    logDebug("App started") // Not inlined: declared in another module
    greetUser("Alice")      // Inlined: declared in the same module
}
```

When compiled to a `.klib`, the code looks something like:
<br />编译成 `.klib` 文件后，代码大致如下：

```kotlin
// Pseudocode
fun main() {
    logDebug("App started")  // Not inlined, declared in another module
    val tmp0 = "Alice"
    println("Hello, $tmp0!") // Inlined from greetUser()
}
```

This means only inline functions declared in the same module are inlined during `.klib` compilation. Other functions,
in this case, are inlined during the generation of platform-specific binaries.
<br />这意味着只有在同一模块中声明的内联函数才会在 `.klib` 编译期间被内联。其他函数，则会在生成平台特定的二进制文件期间被内联。

#### How to enable {id=how-to-enable-intra-module-inlining}
如何启用

Starting with 2.4.0, the intra-module inlining is enabled by default for Kotlin/Native, Kotlin/JS, and
Kotlin/Wasm.
<br />从 2.4.0 版本开始，Kotlin/Native、Kotlin/JS 和 Kotlin/Wasm 默认启用模块内联功能。

If you face unexpected problems with this feature, you can disable it using the following compiler option in the command
line:
<br />如果此功能遇到意外问题，您可以使用以下命令行编译器选项禁用它：

```bash
-Xklib-ir-inliner=disabled
```

The next step is to enable cross-module inlining to ensure all inline functions in the project are consistently inlined.
This change is planned for future Kotlin releases, but you can already try it out using the following compiler option
in the command line:
<br />下一步是启用跨模块内联，以确保项目中的所有内联函数都保持一致的内联方式。
此更改计划在未来的 Kotlin 版本中实现，但您现在就可以使用以下编译器选项在命令行中尝试启用它：

```bash
-Xklib-ir-inliner=full
```

Please share your feedback and report any problems in [YouTrack](https://kotl.in/issue).
<br />请分享您的反馈意见，并在 [YouTrack](https://kotl.in/issue) 中报告任何问题。

### Consistent partial library linkage across Kotlin compilers
<secondary-label ref="compiler"/>
Kotlin 编译器之间一致的部分库链接

In Kotlin 1.9.0, partial library linkage was enabled by default for both the Kotlin/Native and Kotlin/JS compilers, with
Kotlin/Wasm following in Kotlin 2.0.0. This feature effectively makes compilers treat linkage issues in Kotlin libraries
consistently with Kotlin/JVM.
<br />在 Kotlin 1.9.0 中，Kotlin/Native 和 Kotlin/JS 编译器默认启用部分库链接，
Kotlin/Wasm 编译器在 Kotlin 2.0.0 中也采用了此功能。该特性有效地使编译器处理 Kotlin 库中的链接问题时，与 Kotlin/JVM 保持一致。

Since then, we haven't received negative feedback and haven't noticed users disabling the partial linkage in their projects.
That's why starting with Kotlin 2.4.0, the partial linkage is always enabled, and the `-Xpartial-linkage` compiler option is now deprecated.
<br />自那以后，我们没有收到任何负面反馈，也没有发现用户在其项目中禁用部分链接。
因此，从 Kotlin 2.4.0 开始，部分链接始终启用，并且 `-Xpartial-linkage` 编译器选项已被弃用。

The default log level for all Kotlin compilers is `SILENT`. Linkage issues are not reported during compilation. To change
this behavior in your projects, set the `-Xpartial-linkage-loglevel` compiler option in your build file:
<br />所有 Kotlin 编译器的默认日志级别为 `SILENT`。编译期间不会报告链接问题。
要更改项目中的此行为，请在构建文件中设置 `-Xpartial-linkage-loglevel` 编译器选项：

```kotlin
// build.gradle.kts
kotlin {
    macosX64("native") {
        binaries.executable()
        
        compilations.configureEach {
            compilerOptions.configure {
                // To report linkage issues with the “info” log level:
                freeCompilerArgs.add("-Xpartial-linkage-loglevel=INFO")

                // To report issues as errors:
                freeCompilerArgs.add("-Xpartial-linkage-loglevel=ERROR")
            }
        }
    }
}
```
{validate="false"}

* `INFO` reports linkage issues with the "info" log level.
  <br />`INFO` 报告链接问题，日志级别为 "info"。
* `WARNING` reports warnings at compile time and records them in compilation logs.
  <br />`WARNING` 会在编译时报告警告，并将其记录在编译日志中。
* `ERROR` allows compilation to fail in case of linkage issues and reports errors in compilation logs. Use this option to examine the linkage issues more closely.
  <br />`ERROR` 选项允许在出现链接问题时编译失败，并将错误报告到编译日志中。使用此选项可以更详细地检查链接问题。

If you encounter issues with this feature, please report them in [our issue tracker](https://kotl.in/issue).
<br />如果您在使用此功能时遇到问题，请在[我们的问题跟踪器](https://kotl.in/issue)中报告。

## Kotlin compiler plugins
Kotlin编译器插件

In Kotlin 2.4.0, Kotlin's compiler plugins received notable updates, too. The kapt plugin can now exclude unnecessary 
annotation processors from the compile classpath, and the Power-assert plugin offers simplified configuration through the new runtime library.
<br />在 Kotlin 2.4.0 中，Kotlin 的编译器插件也获得了显着更新。 kapt 插件现在可以排除不必要的
来自编译类路径的注释处理器，Power-assert 插件通过新的运行时库提供简化的配置。

### kapt: Exclude annotation processors from compile classpath
kapt：从编译类路径中排除注解处理器

Kotlin 2.4.0 adds support for the `includeCompileClasspath` configuration option for annotation processor discovery, 
similar to the Kotlin Gradle plugin. The new option allows you to exclude unnecessary annotation processors from the compile classpath.
<br />Kotlin 2.4.0 新增了对注解处理器发现的 `includeCompileClasspath` 配置选项的支持，类似于 Kotlin Gradle 插件。该新选项允许您从编译类路径中排除不必要的注解处理器。

To configure this in your build file, set the `includeCompileClasspath` option to `false` in the `<execution>` section of the kapt plugin:
<br />要在构建文件中配置此项，请在 kapt 插件的 `<execution>` 部分中将 `includeCompileClasspath` 选项设置为 `false`：

```xml
<execution>
    <id>kapt</id>
        <goals><goal>kapt</goal></goals>
        <configuration>
            <!-- Add new option -->
            <includeCompileClasspath>false</includeCompileClasspath> 
            <sourceDirs>...</sourceDirs>
            <annotationProcessorPaths>...</annotationProcessorPaths>
        </configuration>
</execution>
```

Alternatively, you can do the same with the `kapt.include.compile.classpath` in the `<properties>` section:
<br />或者，您也可以在 `<properties>` 部分中使用 `kapt.include.compile.classpath` 来实现相同的功能：

```xml
<properties>
    <kapt.include.compile.classpath>false</kapt.include.compile.classpath>
</properties>
```

With the option set to `false`, annotation processors not included in the `<annotationProcessorPaths>` section of the
kapt configuration are excluded from the kapt processing.
<br />如果将此选项设置为“false”，则未包含在 kapt 配置的 `<annotationProcessorPaths>` 部分中的注释处理器将从 kapt 处理中排除。

If `includeCompileClasspath` is not set and kapt detects an annotation processor on the compile classpath that is not
explicitly defined in the `<annotationProcessorPaths>` section, you'll see the following deprecation warning:
<br />如果未设置 `includeCompileClasspath`，并且 kapt 检测到编译类路径中存在未在 `<annotationProcessorPaths>` 部分中显式定义的注解处理器，则会看到以下弃用警告：

```text
[WARNING] Annotation processors discovery from compile classpath is deprecated. Set 'kapt.include.compile.classpath=false' to disable discovery.
```

For more information on kapt configuration, see our [documentation](kapt.md).
<br />有关 kapt 配置的更多信息，请参阅我们的[文档](kapt.md)。

### Power-assert: New runtime library
Power-assert：新的运行时库

Kotlin 2.4.0 makes Power-assert capable functions more discoverable and easier to configure with the new runtime library.
<br />Kotlin 2.4.0 的新运行时库使得支持 Power-assert 的函数更容易被发现和配置。

Previously, adopting Power-assert required complex build configurations and function parameter conventions. Starting with
this release, Power-assert capable functions can use the new runtime library to integrate directly with the compiler plugin transformations.
<br />此前，采用 Power-assert 需要复杂的构建配置和函数参数约定。从本版本开始，支持 Power-assert 的函数可以使用新的运行时库直接与编译器插件转换集成。

This brings major improvements for both plugin users and library authors:
<br />这为插件用户和库作者都带来了重大改进：

* The new `CallExplanation` data structure provides detailed information about the call site. This enables more dynamic diagram rendering for assertion failures and better integration with external tools.
  <br />新的 `CallExplanation` 数据结构提供了关于调用点的详细信息。这使得断言失败的图表渲染更加动态，并能更好地与外部工具集成。
* The new `@PowerAssert` annotation makes assertion functions instantly discoverable by the compiler plugin. That way, you can now add out-of-the-box support for Power-assert into your libraries.
  <br />新的 `@PowerAssert` 注解使得编译器插件能够立即发现断言函数。这样，​​您现在可以为您的库添加开箱即用的 Power-assert 支持。

> Use our [example collection](https://github.com/bnorm/power-assert-examples#power-assert-examples) as a playground for experimenting with the new features.
> <br />使用我们的[示例集合](https://github.com/bnorm/power-assert-examples#power-assert-examples)作为试验场来体验新功能。
>
{style="tip"}

For more information, see our [documentation](power-assert.md#use-the-power-assert-plugin).
<br />有关更多信息，请参阅我们的[文档](power-assert.md#use-the-power-assert-plugin)。

## Compose compiler
Compose 编译器

With Kotlin 2.4.0, the Compose compiler offers more consistent incremental compilation and advances the deprecation cycle of several feature flags.
<br />Kotlin 2.4.0 中的 Compose 编译器提供了更一致的增量编译，并加快了几个特性标志的弃用周期。

### Consistent incremental compilation for internal declarations
<secondary-label ref="compose-compiler"/>
内部声明的一致性增量编译

Starting from Kotlin 2.4.0, the Compose compiler offers more consistent incremental compilation. Stability of internal 
types across different files is now inferred during runtime. This allows Compose to update inferred stability values even
when class usages are not recompiled.
<br />从 Kotlin 2.4.0 开始，Compose 编译器提供了更一致的增量编译。现在，内部类型在不同文件中的稳定性会在运行时进行推断。
这使得 Compose 即使在类使用未被重新编译的情况下也能更新推断出的稳定性值。

As a side effect, the size of your artifacts may increase whenever a `@Composable` function uses an `internal` class from
a different file as a parameter. This is caused by the compiler encoding the execution paths for both stable and unstable
cases, since stability has to be decided during runtime. This overhead of runtime stability is removed by minifiers that
perform full-app optimizations (such as R8) as they are able to infer the unnecessary execution path and eliminate it.
<br />副作用是，当 `@Composable` 函数使用来自不同文件的 `internal` 类作为参数时，编译产物的大小可能会增加。
这是因为编译器会同时对稳定和不稳定两种情况下的执行路径进行编码，
因为稳定性需要在运行时确定。执行全应用优化的压缩器（例如 R8）可以消除这种运行时稳定性开销，因为它们能够推断出不必要的执行路径并将其消除。

This update does not change the final stability value, so the behavior of `@Composable` functions remains unchanged.
<br />此次更新不会改变最终稳定性值，因此 `@Composable` 函数的行为保持不变。

### Feature flag deprecations
<secondary-label ref="compose-compiler"/>
功能标志弃用

Kotlin 2.4.0 advances the deprecation cycle of experimental feature flags that graduated to stable and are now enabled by default:
<br />Kotlin 2.4.0 加快了实验性功能标志的弃用周期，这些标志已升级为稳定版，现在默认启用：

* `StrongSkipping`, `IntrinsicRemember`, and associated DSL properties are advanced to `DeprecationLevel.ERROR`. They will be removed in Kotlin 2.5.0.
  <br />`StrongSkipping`、`IntrinsicRemember` 及其相关的 DSL 属性已提升至 `DeprecationLevel.ERROR` 级别，将在 Kotlin 2.5.0 中移除。
* `OptimizeNonSkippingGroups` and `PausableComposition` are now deprecated. They are scheduled to be removed in Kotlin 2.6.0.
  <br />`OptimizeNonSkippingGroups` 和 `PausableComposition` 现已弃用，计划在 Kotlin 2.6.0 中移除。

## Breaking changes and deprecations
重大变更和弃用

This section highlights important breaking changes and deprecations. For a complete overview, see our [Compatibility guide](compatibility-guide-24.md).
<br />本节重点介绍重要的重大变更和弃用项。如需完整概述，请参阅我们的[兼容性指南](compatibility-guide-24.md)。

* Starting with Kotlin 2.4.0, the compiler no longer supports `-language-version=1.9`. As a result, the K1 compiler is no longer supported.
  <br />从 Kotlin 2.4.0 开始，编译器不再支持 `-language-version=1.9`。因此，K1 编译器不再受支持。
* Kotlin 2.4.0 streamlines the DSL for binary compatibility validation in the Kotlin Gradle plugin and deprecates some parts. For the latest DSL, see [Binary compatibility validation in the Kotlin Gradle plugin](gradle-binary-compatibility-validation.md).
  <br />Kotlin 2.4.0 简化了 Kotlin Gradle 插件中二进制兼容性验证的 DSL，并弃用了其中的一些部分。有关最新的 DSL，请参阅[Kotlin Gradle 插件中的二进制兼容性验证](gradle-binary-compatibility-validation.md)。
* [Support for Kotlin script execution through the `KotlinScriptMojo` Maven plugin has been removed](compatibility-guide-22.md#deprecations-to-kotlin-scripting).
  <br />[已移除通过 `KotlinScriptMojo` Maven 插件执行 Kotlin 脚本的支持](compatibility-guide-22.md#deprecations-to-kotlin-scripting)。

## Documentation updates
文档更新
We made the following documentation changes in the Kotlin ecosystem:
<br />我们对 Kotlin 生态系统中的文档进行了以下更改：

* [Liquid Glass in a Compose Multiplatform app](https://kotlinlang.org/docs/multiplatform/ios-liquid-glass.html) – Migrate an iOS app from fully Compose-driven navigation to native SwiftUI navigation with iOS 26 Liquid Glass styling.
  <br />[Compose 多平台应用中的 Liquid Glass](https://kotlinlang.org/docs/multiplatform/ios-liquid-glass.html) – 将 iOS 应用从完全由 Compose 驱动的导航迁移到具有 iOS 26 Liquid Glass 样式的原生 SwiftUI 导航。
* [Adding Swift packages as dependencies to KMP modules](https://kotlinlang.org/docs/multiplatform/multiplatform-spm-import.html) – Learn how to set up a SwiftPM dependency in your KMP project.
  <br />[将 Swift 包作为依赖项添加到 KMP 模块](https://kotlinlang.org/docs/multiplatform/multiplatform-spm-import.html) – 了解如何在 KMP 项目中设置 SwiftPM 依赖项。
* [Switch Kotlin Multiplatform project from CocoaPods to SwiftPM dependencies](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration.html) manually or [with Junie](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration-ai.html) – Learn how you can use Junie and Kotlin AI skills to make migration easier.
  <br />[将 Kotlin 多平台项目从 CocoaPods 依赖项切换到 SwiftPM 依赖项](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration.html)手动或使用 [Junie](https://kotlinlang.org/docs/multiplatform/multiplatform-cocoapods-spm-migration-ai.html)——了解如何使用 Junie 和 Kotlin AI 技能使迁移更轻松。
* [Configure TeamCity for a KMP app](https://kotlinlang.org/docs/multiplatform/configure-teamcity-for-kmp.html) – Use TeamCity to build, test, and deploy your KMP applications.
  <br />[为 KMP 应用配置 TeamCity](https://kotlinlang.org/docs/multiplatform/configure-teamcity-for-kmp.html) – 使用 TeamCity 构建、测试和部署您的 KMP 应用程序。
* [Recommended serialization approaches for Navigation 3](https://kotlinlang.org/docs/multiplatform/compose-navigation-3.html#recommended-serialization-approaches) – Find the best way to use serialization with Navigation 3 in your CMP application.
  <br />[Navigation 3 推荐的序列化方法](https://kotlinlang.org/docs/multiplatform/compose-navigation-3.html#recommended-serialization-approaches) – 在您的 CMP 应用程序中找到使用 Navigation 3 进行序列化的最佳方法。
* [Multiplatform ViewModel](https://kotlinlang.org/docs/multiplatform/compose-viewmodel.html) – Learn how to set up and work with ViewModels in a multiplatform project.
  <br />[多平台 ViewModel](https://kotlinlang.org/docs/multiplatform/compose-viewmodel.html) – 学习如何在多平台项目中设置和使用 ViewModel。
* [Backend development with Kotlin](server-overview.md) – Explore the different frameworks you can use for backend development.
  <br />[使用 Kotlin 进行后端开发](server-overview.md) – 探索可用于后端开发的不同框架。
* [Create a task manager app with Spring Boot and Claude](spring-boot-claude.md) – Learn how Claude can help you create an app with Spring Boot from scratch.
  <br />[使用 Spring Boot 和 Claude 创建任务管理器应用程序](spring-boot-claude.md) – 了解 Claude 如何帮助您从头开始使用 Spring Boot 创建应用程序。
* [Configure a Maven project](maven-configure-project.md) – Set up Kotlin compilation in your existing Java Maven project or in a new Kotlin Maven project.
  <br />[配置 Maven 项目](maven-configure-project.md) – 在现有的 Java Maven 项目或新的 Kotlin Maven 项目中设置 Kotlin 编译。
* [Test Kotlin projects with Maven](jvm-test-maven.md) – Learn how to create tests with JUnit and use Maven plugins to run unit and integration tests.
  <br />[使用 Maven 测试 Kotlin 项目](jvm-test-maven.md) – 学习如何使用 JUnit 创建测试，并使用 Maven 插件运行单元测试和集成测试。
* [Use annotation processors in Kotlin projects](jvm-annotation-processors.md) – Choose between kapt and KSP to process annotations in your backend project.
  <br />[在 Kotlin 项目中使用注解处理器](jvm-annotation-processors.md) – 在 kapt 和 KSP 之间进行选择，以处理后端项目中的注解。
* [Kotlin AI skills](kotlin-ai-skills.md) – Use agent skills to help you perform Kotlin-specific tasks.
  <br />[Kotlin AI 技能](kotlin-ai-skills.md) – 使用代理技能来帮助您执行 Kotlin 特有的任务。
* [Kotlin Language Server](kotlin-lsp.md) – Read about JetBrains' official implementation of the Language Server Protocol (LSP) for Kotlin.
  <br />[Kotlin 语言服务器](kotlin-lsp.md) – 阅读有关 JetBrains 官方实现的 Kotlin 语言服务器协议 (LSP) 的信息。
* [Numbers](numbers.md) – Explore Kotlin's number types and how to work with them.
  <br />[数字](numbers.md) – 探索 Kotlin 的数字类型以及如何使用它们。
* [Getting started with KSP](ksp-quickstart.md) – Learn how to add a KSP-based processor to your project or create your own.
  <br />[KSP 入门指南](ksp-quickstart.md) – 学习如何将基于 KSP 的处理器添加到您的项目中或创建您自己的处理器。
* [Migrate from kapt to KSP](ksp-kapt-migration.md) – Migrate your annotation processors to get the best out of Kotlin's features.
  <br />[从 kapt 迁移到 KSP](ksp-kapt-migration.md) – 迁移您的注解处理器，以充分利用 Kotlin 的功能。
* [Lincheck overview](lincheck-guide.md) – Understand how Lincheck works behind the scenes to test concurrent code on the JVM.
  <br />[Lincheck 概述](lincheck-guide.md) – 了解 Lincheck 在 JVM 上测试并发代码的底层工作原理。
* [Getting started with Lincheck](lincheck-getting-started.md) – Create a project and run tests with Lincheck.
  <br />[Lincheck 入门指南](lincheck-getting-started.md) – 使用 Lincheck 创建项目并运行测试。
* [Testing arbitrary code with Lincheck](lincheck-testing-arbitrary-code.md) – Learn how to test concurrent code with Lincheck.
  <br />[使用 Lincheck 测试任意代码](lincheck-testing-arbitrary-code.md) – 学习如何使用 Lincheck 测试并发代码。
* [How to test data structures with Lincheck](lincheck-how-to-test-data-structures.md) – Dive into Lincheck's data structure testing process.
  <br />[如何使用 Lincheck 测试数据结构](lincheck-how-to-test-data-structures.md) – 深入了解 Lincheck 的数据结构测试过程。
* [Testing strategies with Lincheck](lincheck-testing-strategies.md) – Learn about Lincheck's testing strategies: model checking and stress testing.
  <br />[使用 Lincheck 进行测试策略](lincheck-testing-strategies.md) – 了解 Lincheck 的测试策略：模型检查和压力测试。
* [Configuring a testing strategy with Lincheck](lincheck-testing-strategies-options.md) – Explore the different options for Lincheck's testing strategies.
  <br />[使用 Lincheck 配置测试策略](lincheck-testing-strategies-options.md) – 探索 Lincheck 测试策略的不同选项。
* [Deploy a Ktor application with Dokku](https://ktor.io/docs/dokku.html) – Learn about the deployment workflow with Dokku.
  <br />[使用 Dokku 部署 Ktor 应用程序](https://ktor.io/docs/dokku.html) – 了解使用 Dokku 的部署工作流程。