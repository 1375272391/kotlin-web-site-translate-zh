[//]: # (title: Create a console app – tutorial)

<web-summary>Create a Kotlin console app in IntelliJ IDEA and run it using the Kotlin compiler.</web-summary>

This tutorial demonstrates how to use IntelliJ IDEA for creating a console application.
<br />本教程演示如何使用 IntelliJ IDEA 创建控制台应用程序。

To get started, first download and install the latest version of [IntelliJ IDEA](https://www.jetbrains.com/idea/download/).
<br />首先，下载并安装最新版本的[IntelliJ IDEA](https://www.jetbrains.com/idea/download/)。

## Create a project
创建项目

1. In IntelliJ IDEA, select **File** | **New** | **Project**.
   <br />在 IntelliJ IDEA 中，选择 **文件** | **新建** | **项目**。
2. In the list on the left, select **Kotlin**.
   <br />在左侧列表中，选择**Kotlin**。
3. Name the new project and change its location if necessary.
   <br />给新项目命名，如有必要，请更改其位置。

   > Select the **Create Git repository** checkbox to place the new project under version control. You will be able to do
   > it later at any time.
   > <br />选中**Create Git repository**复选框，即可将新项目置于版本控制之下。您以后随时都可以执行此操作。
   >
   {style="tip"}
   
   ![Create a console application](jvm-new-project.png){width=700}

4. Select the **IntelliJ** build system. It's a native builder that doesn't require downloading additional artifacts.
   <br />选择 **IntelliJ** 构建系统。它是一个原生构建器，无需下载额外的组件。

   If you want to create a more complex project that needs further configuration, select Maven or Gradle. For Gradle,
   choose a language for the build script: Kotlin or Groovy.
   <br />如果您想创建一个需要更多配置的更复杂的项目，请选择 Maven 或 Gradle。对于 Gradle，请选择构建脚本的语言：Kotlin 或 Groovy。
5. From the **JDK** list, select the [JDK](https://www.oracle.com/java/technologies/downloads/) that you want to use in
   your project.
   <br />从**JDK**列表中，选择你想在项目中使用的[JDK](https://www.oracle.com/java/technologies/downloads/)。
   * If the JDK is installed on your computer, but not defined in the IDE, select **Add JDK** and specify the path to the
   JDK home directory. 
     <br />如果您的计算机上已安装 JDK，但 IDE 中未定义，请选择 **添加 JDK** 并指定 JDK 主目录的路径。
   * If you don't have the necessary JDK on your computer, select **Download JDK**.
     <br />如果您的计算机上没有必要的 JDK，请选择 **下载 JDK**。

6. Enable the **Add sample code** option to create a file with a sample `"Hello World!"` application.
   <br />启用 **Add sample code** 选项，创建一个包含示例 `"Hello World!"` 应用程序的文件。

    > You can also enable the **Generate code with onboarding tips** option to add some additional useful comments to your
    > sample code.
    > <br />您还可以启用 **Generate code with onboarding tips** 选项，为示例代码添加一些额外的有用注释。
    >
    {style="tip"}

7. Click **Create**.
   <br />点击 **Create**。

    > If you chose the Gradle build system, you have in your project a build script file: `build.gradle(.kts)`. It includes
    > the `kotlin("jvm")` plugin and dependencies required for your console application. Make sure that you use the latest
    > version of the plugin:
    > <br />如果您选择了 Gradle 构建系统，您的项目中会有一个构建脚本文件：`build.gradle(.kts)`。
    > 它包含了控制台应用程序所需的 `kotlin("jvm")` 插件及其依赖项。请确保您使用的是最新版本的插件：
    > 
    > <tabs group="build-script">
    > <tab title="Kotlin" group-key="kotlin">
    > 
    > ```kotlin
    > plugins {
    >     kotlin("jvm") version "%kotlinVersion%"
    >     application
    > }
    > ```
    > 
    > </tab>
    > <tab title="Groovy" group-key="groovy">
    > 
    > ```groovy
    > plugins {
    >     id 'org.jetbrains.kotlin.jvm' version '%kotlinVersion%'
    >     id 'application'
    > }
    > ```
    > 
    > </tab>
    > </tabs>
    > 
    {style="note"}

## Create an application
创建应用程序

1. Open the `Main.kt` file in `src/main/kotlin`.  
   The `src` directory contains Kotlin source files and resources. The `Main.kt` file contains sample code that will print 
   `Hello, Kotlin!` as well as several lines with values of the cycle iterator.
   <br />打开 `src/main/kotlin` 目录下的 `Main.kt` 文件。
   `src` 目录包含 Kotlin 源文件和资源。`Main.kt` 文件包含示例代码，该代码将打印`Hello, Kotlin!` 以及几行循环迭代器的值。

   ![Main.kt with main fun](jvm-main-kt-initial.png){width=700}

2. Modify the code so that it requests your name and says `Hello` to you:
   <br />修改代码，使其询问你的姓名并向你问好：

   * Create an input prompt and assign to the `name` variable the value returned by the [`readln()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/readln.html) function.
         <br />创建一个输入提示符，并将 [`readln()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.io/readln.html) 函数返回的值赋给 `name` 变量。
   * Let's use a string template instead of concatenation by adding a dollar sign `$` before the variable name directly in the text output like this – `$name`.
         <br />让我们使用字符串模板而不是字符串连接，直接在文本输出中的变量名前添加美元符号`$`，像这样 – `$name`。
   
   ```kotlin
   fun main() {
       println("What's your name?")
       val name = readln()
       println("Hello, $name!")
   
       // ...
   }
   ```

## Run the application
运行应用程序

Now the application is ready to run. The easiest way to do this is to click the green **Run** icon in the gutter and select **Run 'MainKt'**.
<br />现在应用程序已准备就绪。最简单的方法是点击侧边栏中的绿色 **Run** 图标，然后选择 **Run 'MainKt'**。

![Running a console app](jvm-run-app.png){width=350}

You can see the result in the **Run** tool window.
<br />您可以在 **Run** 工具窗口中看到结果。

![Kotlin run output](jvm-output-1.png){width=600}
   
Enter your name and accept the greetings from your application! 
<br />请输入您的姓名并接受来自您应用程序的问候！

![Kotlin run output](jvm-output-2.png){width=600}

Congratulations! You have just run your first Kotlin application.
<br />恭喜！您刚刚运行了您的第一个 Kotlin 应用程序。

## What's next?
接下来是什么？

Once you've created this application, you can start to dive deeper into Kotlin syntax:
<br />创建好这个应用程序之后，你就可以开始深入了解 Kotlin 语法了：

* Take the [Kotlin tour](kotlin-tour-welcome.md) 
  <br />参加 [Kotlin 导览](kotlin-tour-welcome.md)
* Install the [JetBrains Academy plugin](https://plugins.jetbrains.com/plugin/10081-jetbrains-academy) for IDEA and complete 
  exercises from the [Kotlin Koans course](https://plugins.jetbrains.com/plugin/10081-jetbrains-academy/docs/learner-start-guide.html?section=Kotlin%20Koans)
  <br />安装适用于 IDEA 的 [JetBrains Academy 插件](https://plugins.jetbrains.com/plugin/10081-jetbrains-academy) 并完成
  [Kotlin Koans 课程](https://plugins.jetbrains.com/plugin/10081-jetbrains-academy/docs/learner-start-guide.html?section=Kotlin%20Koans) 中的练习
