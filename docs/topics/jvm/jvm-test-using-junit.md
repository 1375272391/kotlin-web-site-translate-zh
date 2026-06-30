[//]: # (title: Test Java code using Kotlin and JUnit – tutorial)

<web-summary>Set up a Java project built with Maven or Gradle to integrate JUnit tests written in Kotlin.</web-summary>

Kotlin is fully interoperable with Java, which means you can write tests for Java code using Kotlin and run them together
with your existing Java tests in the same project.
<br />Kotlin 与 Java 完全兼容，这意味着您可以使用 Kotlin 为 Java 代码编写测试，并将它们与同一个项目中现有的 Java 测试一起运行。

In this tutorial, you'll learn how to:
<br />在本教程中，您将学习如何：

* Configure a mixed Java–Kotlin project to run tests using [JUnit](https://junit.org/).
  <br />配置混合 Java–Kotlin 项目以使用 [JUnit](https://junit.org/) 运行测试。
* Add Kotlin tests that verify Java code.
  <br />添加 Kotlin 测试来验证 Java 代码。
* Run tests using Maven or Gradle.
  <br />使用 Maven 或 Gradle 运行测试。

> Before you start, make sure you have:
> <br />开始之前，请确保您已准备好：
>
> * [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) or [VS Code](https://code.visualstudio.com/Download) with the installed [Kotlin extension](https://github.com/Kotlin/kotlin-lsp/tree/main?tab=readme-ov-file#vs-code-quick-start).
    <br />已安装 [Kotlin 扩展程序](https://github.com/Kotlin/kotlin-lsp/tree/main?tab=readme-ov-file#vs-code-quick-start)的 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)或 [VS Code](https://code.visualstudio.com/Download)。
> * Java 17 or later.
    <br />Java 17 或更高版本。
>
{style="note"}

## Configure the project
配置项目

1. In your IDE, clone the sample project from version control:
   <br />在集成开发环境 (IDE) 中，从版本控制系统克隆示例项目：

   ```text
   https://github.com/kotlin-hands-on/kotlin-junit-sample.git
   ```

2. Navigate to the `initial` module and review the project structure:
   <br />导航至“初始”模块并查看项目结构：

    ```text
    kotlin-junit-sample/
    ├── initial/
    │   ├── src/
    │   │   ├── main/java/    # Java source code
    │   │   └── test/java/    # JUnit test in Java
    │   ├── pom.xml           # Maven configuration
    │   └── build.gradle.kts  # Gradle configuration
    ```

   The `initial` module contains a simple Todo application in Java with a single test.
   <br />`initial` 模块包含一个简单的 Java Todo 应用程序，其中包含一个测试。

3. In the same directory, open your build file and update its contents to support Kotlin:
   <br />在同一目录下，打开你的构建文件并更新其内容以支持 Kotlin：

    <tabs group="build-system">
    <tab title="Maven" group-key="maven">

    ```xml
    ```
   {src="jvm-test-tutorial/pom.xml" initial-collapse-state="collapsed" collapsible="true" ignore-vars="false" collapsed-title="pom.xml file"}

    * In the `<properties>` section, set the Kotlin version.
      <br />在 `<properties>` 部分中，设置 Kotlin 版本。
    * In the `<dependencies>` section, add JUnit Jupiter dependencies to run tests.
      <br />在 `<dependencies>` 部分，添加 JUnit Jupiter 依赖项以运行测试。
    * In the `<build><plugins>` section, apply `kotlin-maven-plugin` with `<extensions>` set to `true`. It automatically
      adds corresponding executions and the `kotlin-stdlib` dependency to the build.
      <br />在 `<build><plugins>` 部分，应用 `kotlin-maven-plugin` 插件，并将 `<extensions>` 设置为 `true`。它会自动向构建中添加相应的执行和 `kotlin-stdlib` 依赖项。
    * You don't need to add `maven-compiler-plugin` to the `<build><pluginManagement>` section when using the Kotlin
      Maven plugin with extensions.
      <br />使用 Kotlin Maven 插件及其扩展时，无需将 `maven-compiler-plugin` 添加到 `<build><pluginManagement>` 部分。

    </tab>
    <tab title="Gradle" group-key="gradle">

    ```kotlin
    // build.gradle.kts
    group = "org.jetbrains.kotlin"
    version = "1.0-SNAPSHOT"
    description = "kotlin-junit-complete"
    java.sourceCompatibility = JavaVersion.VERSION_17
    
    plugins {
        application
        kotlin("jvm") version "%kotlinVersion%"
    }

    kotlin {
        jvmToolchain(17)
    }

    application {
        mainClass.set("org.jetbrains.kotlin.junit.App")
    }

    repositories {
        mavenCentral()
    }

    dependencies {
        implementation("com.gitlab.klamonte:jexer:1.6.0")

        testImplementation(kotlin("test"))
        testImplementation(libs.org.junit.jupiter.junit.jupiter.api)
        testImplementation(libs.org.junit.jupiter.junit.jupiter.params)
        testRuntimeOnly(libs.org.junit.jupiter.junit.jupiter.engine)
        testRuntimeOnly(libs.org.junit.platform.junit.platform.launcher)
    }

    tasks.test {
        useJUnitPlatform()
    }
    ```
   {initial-collapse-state="collapsed" collapsible="true" collapsed-title="build.gradle.kts"}

    * In the `plugins {}` block, add the `kotlin("jvm")` plugin.
      <br />在 `plugins {}` 代码块中，添加 `kotlin("jvm")` 插件。
    * Set the JVM toolchain version to match your Java version.
      <br />将 JVM 工具链版本设置为与您的 Java 版本匹配。
    * In the `dependencies {}` block, add the `kotlin.test` library that provides Kotlin's test utilities and integrates with JUnit.
      <br />在 `dependencies {}` 块中，添加 `kotlin.test` 库，该库提供 Kotlin 的测试工具并与 JUnit 集成。
      
    Kotlin/JVM supports the latest stable JUnit version, JUnit 6. You can find it in the `gradle/libs.versions.toml` version catalog.
    <br />Kotlin/JVM 支持最新的稳定版 JUnit，即 JUnit 6。您可以在 `gradle/libs.versions.toml` 版本目录中找到它。
   
    If you generally prefer using the version catalog, you can even add the `kotlin("jvm")` plugin there:
    <br />如果您通常更喜欢使用版本目录，您甚至可以在那里添加 `kotlin("jvm")` 插件：

    ```toml
    # gradle/libs.versions.toml
    [versions]
    kotlin = "%kotlinVersion%"
    junit = "6.0.3"

    [libraries]
    org-junit-jupiter-junit-jupiter-api = { module = "org.junit.jupiter:junit-jupiter-api", version.ref = "junit" }
    org-junit-jupiter-junit-jupiter-params = { module = "org.junit.jupiter:junit-jupiter-params", version.ref = "junit" }
    org-junit-jupiter-junit-jupiter-engine = { module = "org.junit.jupiter:junit-jupiter-engine", version.ref = "junit" }
    org-junit-platform-junit-platform-launcher = { module = "org.junit.platform:junit-platform-launcher" }
      
    [plugins]
    kotlinJvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
    ```
    {initial-collapse-state="collapsed" collapsible="true" collapsed-title="libs.versions.toml"}

    </tab>
    </tabs>

4. Reload the build file in your IDE.
   <br />在 IDE 中重新加载构建文件。

For more detailed instructions on build file setup, see [Project configuration](mixing-java-kotlin-intellij.md#project-configuration).
<br />有关构建文件设置的更详细说明，请参阅[项目配置](mixing-java-kotlin-intellij.md#project-configuration)。

## Add your first Kotlin test
添加你的第一个 Kotlin 测试

The `TodoItemTest.java` test in `initial/src/test/java` already verifies the app basics: item creation, defaults,
unique IDs, and state changes.
<br />`initial/src/test/java` 中的 `TodoItemTest.java` 测试已经验证了应用程序的基本功能：项目创建、默认值、唯一 ID 和状态更改。

You can expand the test coverage by adding a Kotlin test that verifies repository-level behavior:
<br />您可以通过添加一个 Kotlin 测试来扩展测试覆盖范围，该测试可以验证存储库级别的行为：

1. Navigate to the same test source directory, `initial/src/test/java`.
   <br />导航到相同的测试源目录，`initial/src/test/java`。
2. Create a `TodoRepositoryTest.kt` file in the same package as the Java test.
   <br />在与 Java 测试相同的包中创建 `TodoRepositoryTest.kt` 文件。
3. Create the test class with field declarations and setup function:
   <br />创建包含字段声明和设置函数的测试类：

   ```kotlin
   package org.jetbrains.kotlin.junit

   import org.junit.jupiter.api.BeforeEach
   import org.junit.jupiter.api.Assertions
   import org.junit.jupiter.api.Test
   import org.junit.jupiter.api.DisplayName

   internal class TodoRepositoryTest {
       lateinit var repository: TodoRepository
       lateinit var testItem1: TodoItem
       lateinit var testItem2: TodoItem

       @BeforeEach
       fun setUp() {
           repository = TodoRepository()
           testItem1 = TodoItem("Task 1", "Description 1")
           testItem2 = TodoItem("Task 2", "Description 2")
       }
   }
   ```

    * JUnit annotations work the same in Kotlin as in Java.
      <br />JUnit 注解在 Kotlin 中的作用与在 Java 中的作用相同。
    * In Kotlin, the [`lateinit` keyword](properties.md#late-initialized-properties-and-variables) allows declaring
      non-null properties that are initialized later.
      This helps to avoid having to use nullable types (`TodoRepository?`) in your tests.
      <br />在 Kotlin 中，[`lateinit` 关键字](properties.md#late-initialized-properties-and-variables)允许声明非空属性，这些属性会在稍后初始化。这有助于避免在测试中使用可空类型（例如 `TodoRepository?`）。

4. Add a test inside the `TodoRepositoryTest` class to check the initial repository state and its size:
   <br />在 `TodoRepositoryTest` 类中添加一个测试，以检查初始存储库状态及其大小：

   ```kotlin
   @Test
   @DisplayName("Should start with empty repository")
   fun shouldStartEmpty() {
       Assertions.assertEquals(0, repository.size())
       Assertions.assertTrue(repository.all.isEmpty())
   }
   ```

    * Unlike Java static import, Jupiter's `Assertions` is imported as a class and used as a qualifier for assertion functions.
      <br />与 Java 静态导入不同，Jupiter 的 `Assertions` 是以类的形式导入的，并用作断言函数的限定符。
    * Instead of `.getAll()` calls, you can access Java getters as properties in Kotlin with `repository.all`.
      <br />在 Kotlin 中，您可以使用 `repository.all` 来访问 Java getter 作为属性，而无需使用 `.getAll()` 调用。

5. Write another test to verify copy behavior for all items:
   <br />编写另一个测试来验证所有项目的复制行为：

   ```kotlin
   @Test
   @DisplayName("Should return defensive copy of items")
   fun shouldReturnDefensiveCopy() {
       repository.add(testItem1)

       val items1 = repository.all
       val items2 = repository.all

       Assertions.assertNotSame(items1, items2)
       Assertions.assertThrows(
           UnsupportedOperationException::class.java
       ) { items1.clear() }
       Assertions.assertEquals(1, repository.size())
   }
   ```

    * To get a Java class object from a Kotlin class, use `::class.java`.
      <br />要从 Kotlin 类获取 Java 类对象，请使用 `::class.java`。
    * You can split complex assertions across multiple lines without using any special continuation characters.
      <br />你可以将复杂的断言拆分到多行中，而无需使用任何特殊的续行符。

6. Add a test to verify finding items by ID:
   <br />添加一项测试，以验证是否能通过 ID 找到项目：

   ```kotlin
   @Test
   @DisplayName("Should find item by ID")
   fun shouldFindItemById() {
       repository.add(testItem1)
       repository.add(testItem2)

        val found = repository.getById(testItem1.id())

        Assertions.assertTrue(found.isPresent)
        Assertions.assertEquals(testItem1, found.get())
   }
   ```

   Kotlin works smoothly with the Java [`Optional` API](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html).
   It automatically converts getter methods to properties, that's why the `isPresent()` method is accessed here as a property.
   <br />Kotlin 可以与 Java 的 [`Optional` API](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html) 无缝协作。
   它会自动将 getter 方法转换为属性，因此这里 `isPresent()` 方法是以属性的形式访问的。

7. Write a test to verify the item removal mechanism:
   <br />编写测试用例来验证物品移除机制：

   ```kotlin
    @Test
    @DisplayName("Should remove item by ID")
    fun shouldRemoveItemById() {
        repository.add(testItem1)
        repository.add(testItem2)

        val removed = repository.remove(testItem1.id())

        Assertions.assertTrue(removed)
        Assertions.assertEquals(1, repository.size())
        Assertions.assertTrue(repository.getById(testItem1.id()).isEmpty)
        Assertions.assertTrue(repository.getById(testItem2.id()).isPresent)
    }
   
    @Test
    @DisplayName("Should return false when removing non-existent item")
    fun shouldReturnFalseForNonExistentRemoval() {
        repository.add(testItem1)

        val removed = repository.remove("non-existent-id")

        Assertions.assertFalse(removed)
        Assertions.assertEquals(1, repository.size())
    }
   ```

   In Kotlin, you can chain method calls and property access, for example `repository.getById(id).isEmpty`.
   <br />在 Kotlin 中，你可以链式调用方法和属性访问，例如 `repository.getById(id).isEmpty`。

> You can add even more tests to the `TodoRepositoryTest` test class to cover additional functionality.
> See the full source code in the sample project's [`complete`](https://github.com/kotlin-hands-on/kotlin-junit-sample/blob/main/complete/src/test/java/org/jetbrains/kotlin/junit/TodoRepositoryTest.kt) module.
> <br />您可以向 `TodoRepositoryTest` 测试类添加更多测试，以涵盖更多功能。
> 请参阅示例项目的 [`complete`](https://github.com/kotlin-hands-on/kotlin-junit-sample/blob/main/complete/src/test/java/org/jetbrains/kotlin/junit/TodoRepositoryTest.kt) 模块中的完整源代码。
>
{style="tip"}

## Run tests
运行测试

Run both Java and Kotlin tests to verify your project works as expected:
<br />同时运行 Java 和 Kotlin 测试，以验证您的项目是否按预期工作：

1. Run the test using the gutter icon:
   <br />使用边栏图标运行测试：

   ![Run the test](run-test.png)

   You can also run all project tests from the `initial` directory using the command line:
   <br />您还可以使用命令行从 `initial` 目录运行所有项目测试：

    <tabs group="build-system">
    <tab title="Maven" group-key="maven">

    ```bash
    mvn test
    ```

    </tab>
    <tab title="Gradle" group-key="gradle">

    ```bash
    ./gradlew test
    ```

    </tab>
    </tabs>

2. Check that the test works correctly by changing one of the variable values.
   For example, modify the `shouldAddItem` test to expect an incorrect repository size:
   <br />通过更改其中一个变量的值来检查测试是否正常工作。
   例如，修改 `shouldAddItem` 测试，使其预期存储库大小不正确：

   ```kotlin
   @Test
   @DisplayName("Should add item to repository")
   fun shouldAddItem() {
       repository.add(testItem1)

       Assertions.assertEquals(2, repository.size())  // Changed from 1 to 2
       Assertions.assertTrue(repository.all.contains(testItem1))
   }
   ```

3. Run the test again and verify that it fails:
   <br />再次运行测试并验证其是否失败：

   ![Check the test result. The test has failed](test-failed.png)

> You can find the fully configured project with tests in the sample project's
> [`complete`](https://github.com/kotlin-hands-on/kotlin-junit-sample/tree/main/complete) module.
> <br />您可以在示例项目的[`complete`](https://github.com/kotlin-hands-on/kotlin-junit-sample/tree/main/complete) 模块中找到配置完整的项目以及测试。
>
{style="tip"}

## What's next
接下来会发生什么

Learn more about [testing Kotlin projects with Maven](jvm-test-maven.md).
<br />了解更多关于[使用 Maven 测试 Kotlin 项目](jvm-test-maven.md)。

