[//]: # (title: What's new in Kotlin %kotlinEapVersion%)

<primary-label ref="eap"/>

<web-summary>Read the Kotlin Early Access Preview release notes and try the latest experimental Kotlin features before they are officially released.</web-summary>

_[Released: %kotlinEapReleaseDate%](eap.md#build-details)_

> This document doesn't cover all of the features of the Early Access Preview (EAP) release,
> but it highlights some major improvements.
> <br />本文档并未涵盖抢先体验预览版 (EAP) 的所有功能，但重点介绍了一些重大改进。
>
> See the full list of changes in the [GitHub changelog](https://github.com/JetBrains/kotlin/releases/tag/v%kotlinEapVersion%).
> <br />请参阅[GitHub 变更日志](https://github.com/JetBrains/kotlin/releases/tag/v%kotlinEapVersion%)中的完整变更列表。
>
{style="note"}

The Kotlin %kotlinEapVersion% release is out! Here are some details of this EAP release:
<br />Kotlin %kotlinEapVersion% 版本已发布！以下是此 EAP 版本的一些详细信息：

* **Standard library:** [Support for coroutine stack trace recovery](#standard-library-support-for-coroutine-stack-trace-recovery)
  <br />**标准库：** [支持协程堆栈跟踪恢复](#standard-library-support-for-coroutine-stack-trace-recovery)
* **Kotlin/Native:** [Default incremental compilation for `klib` artifacts](#kotlin-native-incremental-compilation-enabled-by-default)
  <br />**Kotlin/Native:** [klib 工件的默认增量编译](#kotlin-native-incremental-compilation-enabled-by-default)
* **Kotlin/Wasm:** [Changes to top-level `require()` calls and improved companion object initialization order](#kotlin-wasm)
  <br />**Kotlin/Wasm：[对顶层 `require()` 调用进行了更改，并改进了伴生对象初始化顺序](#kotlin-wasm)
* **Kotlin/JS:** [New DSL for browser-testing](#kotlin-js-new-dsl-for-browser-testing)
  <br />**Kotlin/JS：** [用于浏览器测试的新DSL](#kotlin-js-new-dsl-for-browser-testing)
* **Build tools API:** [Support for new targets: Kotlin/JS, Kotlin/Wasm, and Kotlin metadata](#build-tools-api-support-for-kotlin-js-kotlin-wasm-and-kotlin-metadata)
  <br />**构建工具 API：** [支持新目标：Kotlin/JS、Kotlin/Wasm 和 Kotlin 元数据](#build-tools-api-support-for-kotlin-js-kotlin-wasm-and-kotlin-metadata)

> For information about the Kotlin release cycle, see [Kotlin release process](releases.md).
> <br />有关 Kotlin 发布周期的信息，请参阅[Kotlin 发布流程](releases.md)。
>
{style="tip"}

## Update to Kotlin %kotlinEapVersion%
更新至 Kotlin %kotlinEapVersion%

The latest version of Kotlin is included in the latest versions of [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
and [Android Studio](https://developer.android.com/studio).
<br />最新版本的 Kotlin 已包含在最新版本的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
和[Android Studio](https://developer.android.com/studio) 中。

To update to the new Kotlin version, make sure your IDE is updated to the latest version and [change the Kotlin version](releases.md#update-to-a-new-kotlin-version)
to %kotlinEapVersion% in your build scripts.
<br />要更新到新的 Kotlin 版本，请确保您的 IDE 已更新到最新版本，并在构建脚本中将 Kotlin 版本更改为 %kotlinEapVersion%。

## Standard library: Support for coroutine stack trace recovery
<primary-label ref="experimental-opt-in"/>
标准库：支持协程堆栈跟踪恢复

Kotlin %kotlinEapVersion% adds the `StackTraceRecoverable` interface to the standard library.
This improves integration with the `kotlinx.coroutines` library because it lets you define how to create new exception
instances for stack trace recovery without adding a dependency on `kotlinx.coroutines`.
<br />Kotlin %kotlinEapVersion% 将 `StackTraceRecoverable` 接口添加到标准库中。
这改进了与 `kotlinx.coroutines` 库的集成，因为它允许您定义如何创建新的异常实例以进行堆栈跟踪恢复，而无需依赖 `kotlinx.coroutines`。

Stack trace recovery helps with debugging when one coroutine throws an exception, and another rethrows it.
It lets you see where the exception originates and where another coroutine rethrows it.
<br />当一个协程抛出异常，而另一个协程又重新抛出该异常时，堆栈跟踪恢复有助于调试。
它可以让您看到异常的源头以及另一个协程重新抛出异常的位置。

The `kotlinx.coroutines` library performs stack trace recovery by creating a new exception instance with additional
coroutine stack trace information. This happens automatically for exceptions with constructors that take only an
exception message, a cause, both, or no arguments.
<br />`kotlinx.coroutines` 库通过创建一个包含额外协程堆栈跟踪信息的新异常实例来执行堆栈跟踪恢复。
对于构造函数仅接受异常消息、原因、两者都接受或不接受任何参数的异常，此过程会自动执行。

If an exception constructor has additional required arguments, such as a line number or an error code, implement the
`StackTraceRecoverable` interface to define how the `kotlinx.coroutines` library creates a new instance of that exception.
<br />如果异常构造函数有额外的必需参数，例如行号或错误代码，则实现`StackTraceRecoverable` 接口，
以定义 `kotlinx.coroutines` 库如何创建该异常的新实例。

To do so, override the `copyForStackTraceRecovery()` function. This function returns a new exception instance for stack
trace recovery, or `null` if you don't want the `kotlinx.coroutines` library to copy the exception.
<br />为此，请重写 `copyForStackTraceRecovery()` 函数。
此函数返回一个新的异常实例以进行堆栈跟踪恢复，如果您不希望 `kotlinx.coroutines` 库复制异常，则返回 `null`。

> The `StackTraceRecoverable` interface is available on all targets, but the `kotlinx.coroutines`
> library uses it for stack trace recovery only on the JVM.
> <br />`StackTraceRecoverable` 接口在所有目标平台上都可用，但 `kotlinx.coroutines` 库仅在 JVM 上使用它进行堆栈跟踪恢复。
>
{style="note"}

These APIs are [Experimental](components-stability.md#stability-levels-explained) and require opt-in with the
`@OptIn(ExperimentalStdlibCoroutineSupportApi::class)` annotation.

Here's an example of a custom exception that preserves a `line` property when it creates a new instance for stack trace
recovery:
<br />以下是一个自定义异常的示例，该异常在创建新实例以进行堆栈跟踪恢复时会保留 `line` 属性：

```kotlin
import kotlin.coroutines.ExperimentalStdlibCoroutineSupportApi
import kotlin.coroutines.StackTraceRecoverable

@OptIn(ExperimentalStdlibCoroutineSupportApi::class)
class FileEditException
// The implementation requires a private constructor
// to pass the cause to the IllegalStateException constructor
private constructor(
    val line: Int,
    private val detail: String,
    cause: Throwable?,
) : IllegalStateException("When editing line $line: $detail", cause),
    // Implements StackTraceRecoverable for stack trace recovery
    StackTraceRecoverable<FileEditException> {

    constructor(line: Int, detail: String) : this(line, detail, null)

    // Copies the line number and message details
    override fun copyForStackTraceRecovery(): FileEditException =
        FileEditException(line, detail, this)
}

@OptIn(ExperimentalStdlibCoroutineSupportApi::class) 
fun main() {
    val original = FileEditException(15, "Unexpected token")
    val copy = original.copyForStackTraceRecovery()

    println(copy.message)
    // When editing line 15: Unexpected token

    println(copy.cause == original)
    // true
}
```

For more information, see the feature's [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/stdlib/KEEP-0461-stacktrace-recoverable.md).
We would appreciate your feedback in [YouTrack](https://youtrack.jetbrains.com/issue/KT-86595).
<br />更多信息请参阅该功能的 [KEEP](https://github.com/Kotlin/KEEP/blob/main/proposals/stdlib/KEEP-0461-stacktrace-recoverable.md)。
欢迎您在 [YouTrack](https://youtrack.jetbrains.com/issue/KT-86595) 中提供反馈。

## Kotlin/Native: Incremental compilation enabled by default
Kotlin/Native：默认启用增量编译

Starting with %kotlinEapVersion%, incremental compilation of `klib` artifacts is enabled by default.
<br />从 %kotlinEapVersion% 开始，默认情况下启用 `klib` 工件的增量编译。

With incremental compilation, if only a part of the `klib` artifact produced by the project module changes, only the affected part
of the `klib` is further recompiled into a binary.
<br />使用增量编译时，如果项目模块生成的 `klib` 工件只有一部分发生更改，则只有受影响的 `klib` 部分会被进一步重新编译成二进制文件。

This optimization was first introduced in [Kotlin 1.9.20](whatsnew1920.md#incremental-compilation-of-klib-artifacts)
and has proven to drastically reduce compilation time for debug builds.
<br />这项优化最初是在 Kotlin 1.9.20 版本中引入的，
并且已被证明可以大幅减少调试版本的编译时间。

Note that in some cases, this optimization may come with a performance cost for clean builds.
<br />请注意，在某些情况下，这种优化可能会降低全新构建的性能。

If you face unexpected problems with this feature, you can disable it manually. To do that, set the following option
in your `gradle.properties` file:
<br />如果此功能出现意外问题，您可以手动禁用它。为此，请在 `gradle.properties` 文件中设置以下选项：

```none
kotlin.incremental.native=false
```

Please report any problems to our issue tracker [YouTrack](https://kotl.in/issue). For more tips on improving compilation
time, see our [documentation](native-improving-compilation-time.md).
<br />请将任何问题报告至我们的问题跟踪系统 [YouTrack](https://kotl.in/issue)。
如需更多关于优化编译时间的技巧，请参阅我们的 [文档](native-improving-compilation-time.md)。

## Kotlin/Wasm

Kotlin %kotlinEapVersion% changes how Kotlin/Wasm handles top-level `require()` calls in `@JsFun` declarations and
aligns companion object initialization order with JVM behavior.
<br />Kotlin %kotlinEapVersion% 改变了 Kotlin/Wasm 处理 `@JsFun` 声明中顶级 `require()` 调用的方式，并且使伴生对象的初始化顺序与 JVM 行为保持一致。

### Changes to top-level `require()` calls in `@JsFun` declarations
对 `@JsFun` 声明中的顶级 `require()` 调用进行更改

Kotlin/Wasm now reports an error when a `@JsFun` declaration uses the top-level `require()` function.
<br />现在，当 `@JsFun` 声明使用顶层 `require()` 函数时，Kotlin/Wasm 会报告错误。

Previously, the compiler generated a `require` variable in the `import-object.mjs` file, allowing `@JsFun` declarations
to call `require()`.
<br />之前，编译器会在 `import-object.mjs` 文件中生成一个 `require` 变量，
允许 `@JsFun` 声明调用 `require()`。

This behavior unintentionally exposed a compiler implementation detail. To support migration away from it, Kotlin/Wasm
removes this generated `require` declaration, and the compiler now reports errors for such calls. For example:
<br />这种行为无意中暴露了编译器实现的一个细节。为了支持迁移，Kotlin/Wasm
移除了生成的 `require` 声明，编译器现在会对这类调用报告错误。例如：

```kotlin
// Reports an error
@JsFun("(mod) => require(mod)")
external fun loadModule(mod: String): JsAny
```

To prepare for this change, replace top-level `require()` calls in `@JsFun` declarations with the `@JsModule` annotation:
<br />为了应对这一变化，请将 `@JsFun` 声明中的顶级 `require()` 调用替换为 `@JsModule` 注解：

```kotlin
@JsModule("module")
external val module: Module

external interface Module {
    // Defines the expected module members
}
```

For dynamic module loading, use the `import()` expression instead.
Add the `/* webpackIgnore: true */` magic comment to prevent webpack from parsing the dynamic import:
<br />对于动态模块加载，请改用 `import()` 表达式。
添加 `/* webpackIgnore: true */` 魔法注释，以防止 webpack 解析动态导入：

```kotlin
@JsFun(
    """
    ((module) => () => module)(
        await import(/* webpackIgnore: true */ "module")
    )
"""
)
private external fun loadModuleDynamically(): JsAny?
```

You can also use the `import()` expression conditionally. For example, you can load a module only when running in Node.js:
<br />您还可以有条件地使用 `import()` 表达式。例如，您可以仅在 Node.js 环境中运行时加载某个模块：

```kotlin
@JsFun(
    """
    ((module) => () => module)(
        ((typeof process !== "undefined") && (process.release.name === "node"))
            ? await import(/* webpackIgnore: true */ "module")
            : null
    )
"""
)
private external fun loadNodeModule(): JsAny?
```

If your project relies on dependencies that require a top-level `require()` function, add it as a property of
`globalThis` as a workaround:
<br />如果你的项目依赖于需要顶层 `require()` 函数的依赖项，可以将其添加为 `globalThis` 的属性作为一种变通方法：

```kotlin
@JsFun(
    """
    ((module) => {
        globalThis.require = module.default.createRequire(import.meta.url)
        return () => {}
    })(await import("node:module"))
"""
)
external fun defineRequire()
```

If you run into any issues, share your feedback in our [issue tracker](https://youtrack.jetbrains.com/projects/KT/issues/KT-86192).
<br />如果您遇到任何问题，请在我们的[问题跟踪器](https://youtrack.jetbrains.com/projects/KT/issues/KT-86192)中分享您的反馈。

### Improved companion object initialization order
改进的伴随对象初始化顺序

Kotlin/Wasm now initializes superclass companion objects before subclass companion objects, matching the JVM behavior.
Previously, the initialization could be reversed, leading to inconsistent behavior across platforms.
<br />Kotlin/Wasm 现在会先初始化超类伴生对象，然后再初始化子类伴生对象，这与 JVM 的行为一致。
此前，初始化顺序可能会颠倒，导致不同平台上的行为不一致。

The update improves cross-platform consistency and reduces platform-specific differences in class initialization behavior.
It also enables correct handling of companion object initialization in deeper inheritance hierarchies, including cases
where intermediate classes don’t declare companion objects.
<br />此次更新提升了跨平台一致性，并减少了不同平台在类初始化行为上的差异。
它还支持在更深层次的继承结构中正确处理伴生对象的初始化，包括中间类未声明伴生对象的情况。

## Kotlin/JS: new DSL for browser-testing
<primary-label ref="experimental-opt-in"/>

Kotlin %kotlinEapVersion% introduces a new experimental DSL for running Kotlin/JS tests in a browser environment.
<br />Kotlin %kotlinEapVersion% 引入了一种新的实验性 DSL，用于在浏览器环境中运行 Kotlin/JS 测试。

Currently, the Kotlin Gradle plugin uses [Karma](https://github.com/karma-runner/karma) as a browser launcher to run
JavaScript tests across different browsers. The Karma project has been deprecated for 2 years now, which made us explore
alternative ways to support browser testing.
<br />目前，Kotlin Gradle 插件使用 [Karma](https://github.com/karma-runner/karma) 作为浏览器启动器，用于在不同浏览器上运行JavaScript 测试。
Karma 项目已经弃用两年了，这促使我们探索 其他支持浏览器测试的方法。

The new DSL is intended to replace Karma as a manager of different tools under the hood and includes:
新的DSL旨在取代Karma，作为底层不同工具的管理器，其包含：

* [Mocha](https://mochajs.org/) as a test runner.
  <br />使用 [Mocha](https://mochajs.org/) 作为测试运行器。
* [Webpack](https://webpack.js.org/) as a bundler (will be replaced with [Vite](https://vite.dev/)
  in [future releases](https://youtrack.jetbrains.com/issue/KT-48308/)).
  <br />[Webpack](https://webpack.js.org/) 作为打包工具（将在[未来版本](https://youtrack.jetbrains.com/issue/KT-48308/)中被[Vite](https://vite.dev/)取代）。
* [Playwright](https://playwright.dev/) as a browser driver and a distribution manager that supports Chromium, Firefox,
  and WebKit (Safari) browser engines.
  <br />[Playwright](https://playwright.dev/) 是一款浏览器驱动程序和分发管理器，支持 Chromium、Firefox 和 WebKit (Safari) 浏览器引擎。

To try out the new testing DSL, add the opt-in `test{}` block inside `browser{}` for your Kotlin/JS target:
<br />要试用新的测试 DSL，请在 Kotlin/JS 目标的 `browser{}` 内添加可选的 `test{}` 代码块：

```kotlin
kotlin {
    js {
        browser {
            @OptIn(ExperimentalJsTestDsl::class)
            // Add and configure the new test{} block
            test {
                // Set up options common for all browsers
                browserDefaults {
                    timeout = Duration.ofSeconds(2)
                    headless = true
                }
                // Enable Chromium test runner
                chromium {
                    // Override the common timeout option
                    timeout = Duration.ofSeconds(5)
                    launchArgs.add("--no-sandbox")
                }
                // Enable Firefox test runner
                firefox()
                // Enable WebKit test runner
                webkit { }
                // Enable and configure an additional WebKit test runner
                webkit("noheadless") {
                    // Set up custom options
                    headless = false
                }
            }
        }
    }
}
```

The new DSL is in active development. We would appreciate your feedback in [YouTrack](https://youtrack.jetbrains.com/issue/KT-66897).
<br />新的DSL正在积极开发中。我们非常欢迎您在[YouTrack](https://youtrack.jetbrains.com/issue/KT-66897)中提供反馈意见。

## Build tools API: Support for Kotlin/JS, Kotlin/Wasm, and Kotlin metadata
<primary-label ref="experimental-general"/>
构建工具 API：支持 Kotlin/JS、Kotlin/Wasm 和 Kotlin 元数据

In [Kotlin 2.2.0](whatsnew22.md#new-experimental-build-tools-api), the build tools API (BTA) became available for
Kotlin/JVM. Kotlin 2.4.20-Beta1 takes the next step toward BTA stabilization by adding support for new targets:
Kotlin/JS, Kotlin/Wasm, and Kotlin metadata.
<br />在 [Kotlin 2.2.0](whatsnew22.md#new-experimental-build-tools-api) 版本中，构建工具 API (BTA) 已可用于
Kotlin/JVM。Kotlin 2.4.20-Beta1 通过新增对以下目标的支持，进一步推进了 BTA 的稳定化：
Kotlin/JS、Kotlin/Wasm 和 Kotlin 元数据。

This makes the Kotlin Gradle plugin interact with the compiler more consistently. In some cases, you can also benefit
from faster, more stable compilation.
<br />这使得 Kotlin Gradle 插件与编译器之间的交互更加一致。在某些情况下，您还可以受益于更快、更稳定的编译。

The BTA is a universal API that acts as an abstraction layer between build systems and the Kotlin compiler ecosystem.
It helps support Kotlin features and compatibility with the Kotlin compiler in available build tools.
<br />BTA 是一个通用 API，它充当构建系统和 Kotlin 编译器生态系统之间的抽象层。
它有助于在现有构建工具中支持 Kotlin 特性并保持与 Kotlin 编译器的兼容性。

We plan to roll out the BTA support for the new targets in the Kotlin Gradle plugin gradually:
我们计划逐步在 Kotlin Gradle 插件中推出对新目标的 BTA 支持：

* In Kotlin 2.4.20-Beta1, BTA is enabled in Kotlin/JS, Kotlin/Wasm, and Kotlin metadata by default to gather feedback.
  No additional changes in projects are required.
  <br />在 Kotlin 2.4.20-Beta1 中，默认情况下已在 Kotlin/JS、Kotlin/Wasm 和 Kotlin 元数据中启用 BTA，以收集反馈。
  无需对项目进行任何其他更改。
* Between Kotlin 2.4.20-Beta2 and the final Kotlin 2.4.20 release, BTA in the new targets will be available as an opt-in.
  To try it out, add the corresponding properties to your `gradle.properties` file:
  <br />在 Kotlin 2.4.20-Beta2 和最终版 Kotlin 2.4.20 之间，新目标中的 BTA 将作为可选功能提供。
  要试用，请将相应的属性添加到您的 `gradle.properties` 文件中：

  ```kotlin
  kotlin.wasm.runViaBuildToolsApi = true
  kotlin.js.runViaBuildToolsApi = true
  kotlin.metadata.runViaBuildToolsApi = true
  ```

* Starting with Kotlin 2.5.0, BTA will be enabled in Kotlin/JS, Kotlin/Wasm, and Kotlin metadata by default again.
  <br />从 Kotlin 2.5.0 开始，BTA 将再次默认在 Kotlin/JS、Kotlin/Wasm 和 Kotlin 元数据中启用。

If you're curious about the BTA proposal or want to share your feedback, see this [KEEP](https://github.com/Kotlin/KEEP/blob/build-tools-api/proposals/extensions/build-tools-api.md).
<br />如果您对 BTA 提案感兴趣或想分享您的反馈，请参阅此 [KEEP](https://github.com/Kotlin/KEEP/blob/build-tools-api/proposals/extensions/build-tools-api.md)。
