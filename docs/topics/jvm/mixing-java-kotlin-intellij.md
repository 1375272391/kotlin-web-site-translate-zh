[//]: # (title: Adding Kotlin to a Java project – tutorial)

<web-summary>Integrate Kotlin into an existing Java project − configure Maven or Gradle build files, organize source files, and convert Java code to Kotlin in IntelliJ IDEA.</web-summary>

Kotlin is fully interoperable with Java, so you can gradually introduce it into your existing Java projects without
having to rewrite everything.
<br />Kotlin 与 Java 完全兼容，因此您可以逐步将其引入到现有的 Java 项目中，而无需重写所有内容。

In this tutorial, you'll learn how to:
<br />在本教程中，您将学习如何：

* Set up Maven or Gradle build tools to compile both Java and Kotlin code.
  <br />设置 Maven 或 Gradle 构建工具，以编译 Java 和 Kotlin 代码。
* Organize Java and Kotlin source files in your project directories.
  <br />将Java和Kotlin源文件整理到项目目录中。
* Convert Java files to Kotlin using IntelliJ IDEA.
  <br />使用 IntelliJ IDEA 将 Java 文件转换为 Kotlin 文件。

> You can use any existing Java project for this tutorial or clone our public [sample project](https://github.com/kotlin-hands-on/kotlin-junit-sample/tree/main/complete)
> with both Maven and Gradle build files already set up.
> <br />你可以使用任何现有的 Java 项目来完成本教程，或者克隆我们的公共[示例项目](https://github.com/kotlin-hands-on/kotlin-junit-sample/tree/main/complete)，
> 该项目已预先配置好 Maven 和 Gradle 构建文件。
> 
> You can also hand over the conversion to your AI agent of choice using our [prepared skill](https://github.com/Kotlin/kotlin-agent-skills/blob/main/skills/kotlin-tooling-java-to-kotlin/SKILL.md).
> Keep in mind that AI processing results are not entirely predictable.
> <br />您还可以使用我们[预先准备好的技能](https://github.com/Kotlin/kotlin-agent-skills/blob/main/skills/kotlin-tooling-java-to-kotlin/SKILL.md)
> 将转换工作交给您选择的 AI 代理。请注意，AI 的处理结果并非完全可预测。
>
{style="tip"}

## Project configuration
项目配置

To add Kotlin to a Java project, you need to configure the project to use both Kotlin and Java,
depending on the build tool you use.
<br />要在 Java 项目中添加 Kotlin，您需要将项目配置为同时使用 Kotlin 和 Java，具体取决于您使用的构建工具。

The project configuration ensures that both Kotlin and Java code are compiled properly and can reference each other
seamlessly.
<br />项目配置确保 Kotlin 和 Java 代码都能正确编译，并且可以相互引用，无缝衔接。

### Maven

> Starting with **IntelliJ IDEA 2025.3**, when you add your first Kotlin file to a Maven-based Java project,  
> the IDE automatically updates your `pom.xml` file to include the Kotlin Maven plugin and standard dependencies.  
> You can still configure it manually if you want to customize versions or build phases.
> <br />从 **IntelliJ IDEA 2025.3** 版本开始，当您将第一个 Kotlin 文件添加到基于 Maven 的 Java 项目中时，
> IDE 会自动更新 `pom.xml` 文件，以包含 Kotlin Maven 插件和标准依赖项。
> 如果您需要自定义版本或构建阶段，仍然可以手动配置。
>
{style="note"}

To use Kotlin and Java together in a Maven project, apply the Kotlin Maven plugin and add Kotlin dependencies in your
`pom.xml` file:
<br />要在 Maven 项目中同时使用 Kotlin 和 Java，请应用 Kotlin Maven 插件，并在您的`pom.xml` 文件中添加 Kotlin 依赖项：

1. In the `<properties>` section, add the Kotlin version property:
        <br />在 `<properties>` 部分，添加 Kotlin 版本属性：

    ```xml
    ```
   {src="jvm-test-tutorial/pom.xml" ignore-vars="false" include-lines="13,17,18"}

2. In the `<dependencies>` section, add the required dependencies to the `<plugins>` section:
        <br />在 `<dependencies>` 部分，添加所需的依赖项到 `<plugins>` 部分：

    ```xml
    ```
   {src="jvm-test-tutorial/pom.xml" include-lines="32,38-43,45-49,55"}

3. In the `<build><plugins>` section, add the Kotlin plugin:
        <br />在 `<build><plugins>` 部分中，添加 Kotlin 插件：

    ```xml
    ```
   {src="jvm-test-tutorial/pom.xml" include-lines="57-58,95-96,99-107"}

   Enabling `<extensions>true</extensions>` in the Kotlin Maven plugin helps to:
        <br />在 Kotlin Maven 插件中启用 `<extensions>true</extensions>` 有助于：

   * Automatically add the `kotlin-stdlib` dependency to the project.
            <br />自动将 `kotlin-stdlib` 依赖项添加到项目中。
   * Configure execution phases to compile Kotlin first, then Java.
            <br />配置执行阶段，先编译 Kotlin，再编译 Java。
   * Reference Kotlin code in Java code and vice versa.
            <br />在 Java 代码中引用 Kotlin 代码，反之亦然。
   * Automatically align the JVM target version with the Java compiler version.
            <br />自动使 JVM 目标版本与 Java 编译器版本保持一致。

   You don't need a separate `maven-compiler-plugin` in the `<build><pluginManagement>` section when using the Kotlin
   Maven plugin with extensions.
        <br />使用 Kotlin Maven 插件及其扩展时，无需在 ` <build><pluginManagement>` 部分单独添加 `maven-compiler-plugin`。

4. Reload the Maven project in your IDE.
        <br />在 IDE 中重新加载 Maven 项目。
5. Run tests to verify the configuration:
        <br />运行测试以验证配置：

    ```bash
    ./mvnw clean test
    ```

### Gradle

To use Kotlin and Java together in a Gradle project, apply the Kotlin JVM plugin and add Kotlin dependencies in your
`build.gradle.kts` file:
<br />要在 Gradle 项目中同时使用 Kotlin 和 Java，请应用 Kotlin JVM 插件，并在以下文件中添加 Kotlin 依赖项：
`build.gradle.kts` 文件：

1. In the `plugins {}` block, add the Kotlin JVM plugin:
   <br />在 `plugins {}` 代码块中，添加 Kotlin JVM 插件：

    ```kotlin
    plugins {
        // Other plugins
        kotlin("jvm") version "%kotlinVersion%"
    }
    ```

2. Set the JVM toolchain version to match your Java version:
   <br />将 JVM 工具链版本设置为与您的 Java 版本匹配：

    ```kotlin
    kotlin {
        jvmToolchain(17)
    }
    ```

   This ensures Kotlin uses the same JDK version as your Java code.
   <br />这样可以确保 Kotlin 使用与 Java 代码相同的 JDK 版本。

3. In the `dependencies {}` block, add the `kotlin("test")` library that provides Kotlin test utilities and integrates
   with JUnit:
   <br />在 `dependencies {}` 代码块中，添加提供 Kotlin 测试工具并与 JUnit 集成的 `kotlin("test")` 库：

    ```kotlin
    dependencies {
        // Other dependencies
    
        testImplementation(kotlin("test"))
        // Other test dependencies
    }
    ```

4. Reload the Gradle project in your IDE.
   <br />在 IDE 中重新加载 Gradle 项目。
5. Run your tests to verify the configuration:
   <br />运行测试以验证配置：

    ```bash
    ./gradlew clean test
    ```

## Project structure
项目结构

With this configuration, you can mix Java and Kotlin files in the same source directories:
<br />通过这种配置，您可以在同一源代码目录中混合使用 Java 和 Kotlin 文件：

```none
src/
  ├── main/
  │    ├── java/          # Java and Kotlin production code
  │    └── kotlin/        # Additional Kotlin production code (optional)
  └── test/
       ├── java/          # Java and Kotlin test code
       └── kotlin/        # Additional Kotlin test code (optional)
```

You can create these directories manually or let IntelliJ IDEA create them when you add your first Kotlin file.
<br />您可以手动创建这些目录，也可以让 IntelliJ IDEA 在您添加第一个 Kotlin 文件时创建它们。

The Kotlin plugin automatically recognizes both `src/main/java` and `src/test/java` directories,
so you can keep `.kt` and `.java` files in the same directories.
<br />Kotlin插件会自动识别`src/main/java`和`src/test/java`目录，因此您可以将`.kt`和`.java`文件放在同一目录中。

## Convert Java files to Kotlin
将 Java 文件转换为 Kotlin

The Kotlin plugin also bundles a Java to Kotlin converter (_J2K_) that automatically converts Java files to Kotlin.
To use J2K on a file, click **Convert Java File to Kotlin File** in its context menu or in the **Code** menu of IntelliJ IDEA.
<br />Kotlin 插件还捆绑了一个 Java 到 Kotlin 转换器 (_J2K_)，可以自动将 Java 文件转换为 Kotlin 文件。
要对文件使用 J2K，请在 IntelliJ IDEA 的上下文菜单或 **代码** 菜单中单击 **Convert Java File to Kotlin File**。

![Convert Java to Kotlin](convert-java-to-kotlin.png){width=500}

While the converter is not fool-proof, it does a pretty decent job of converting most boilerplate code from Java to Kotlin.
However, some manual tweaking is sometimes required.
<br />虽然转换器并非万无一失，但它在将大多数 Java 样板代码转换为 Kotlin 方面做得相当不错。不过，有时需要进行一些手动调整。

## Explore compiler plugins {initial-collapse-state="collapsed" collapsible="true"}
探索编译器插件

If you have a more complex project that uses [Spring](https://spring.io/) or Java Persistence API (JPA), you can use Kotlin compiler
plugins that automatically adapt Kotlin's language features to framework expectations, reducing boilerplate:
<br />如果您有一个使用 Spring 或 Java 持久化 API (JPA) 的更复杂的项目，您可以使用 Kotlin 编译器插件。
这些插件可以自动将 Kotlin 的语言特性适配到框架的预期，从而减少样板代码：

* The **[`all-open`](all-open-plugin.md)** plugin automatically makes classes and their members `open` when used with specific
  annotations. This is particularly useful for frameworks like Spring that require classes to be non-final.
  <br />当与特定注解一起使用时，**[`all-open`](all-open-plugin.md)** 插件会自动将类及其成员设置为 `open`。这对于像 Spring 这样要求类为非 final 的框架尤其有用。

  For Spring, you can use a dedicated [`kotlin-spring`](all-open-plugin.md#spring-support) plugin,
  which is a wrapper on top of `all-open`. It specifies Spring annotations automatically.
  <br />对于 Spring，您可以使用专门的 [`kotlin-spring`](all-open-plugin.md#spring-support) 插件，它是 `all-open` 的封装，可以自动指定 Spring 注解。
* The **[`no-arg`](no-arg-plugin.md)** plugin generates an additional zero-argument constructor for classes with specific annotations.
  This allows JPA to instantiate classes that otherwise wouldn't have a default constructor.
  <br />**[`no-arg`](no-arg-plugin.md)** 插件会为带有特定注解的类生成一个额外的无参构造函数。这使得 JPA 能够实例化那些原本没有默认构造函数的类。

  You can also use the [`kotlin-jpa`](no-arg-plugin.md#jpa-support) plugin, which is a wrapper on top of `no-arg`.
  It specifies no-arg annotations automatically.
  <br />您还可以使用 [`kotlin-jpa`](no-arg-plugin.md#jpa-support) 插件，它是 `no-arg` 的封装。它会自动指定 no-arg 注解。
* The **[`power-assert`](power-assert.md)** plugin improves the debugging experience by providing detailed failure messages with
  contextual information for assertions. It shows intermediate values and helps you understand why a test failed.
  <br />**[`power-assert`](power-assert.md)** 插件通过提供包含断言上下文信息的详细失败消息来改善调试体验。它会显示断言的中间值，并帮助您了解测试失败的原因。

## Next step
实践

The easiest way to start using Kotlin in a Java project is by adding Kotlin tests first:
<br />在 Java 项目中使用 Kotlin 的最简单方法是先添加 Kotlin 测试：

[Add your first Kotlin test to your Java project](jvm-test-using-junit.md)
<br />[向你的 Java 项目添加你的第一个 Kotlin 测试](jvm-test-using-junit.md)

### See also
参见

* [Kotlin and Java interoperability details](java-to-kotlin-interop.md)
  <br />[Kotlin 和 Java 互操作性详情](java-to-kotlin-interop.md)
* [Maven build configuration reference](maven.md)
  <br />[Maven 构建配置参考](maven.md)