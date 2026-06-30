[//]: # (title: Get started with Kotlin)

<tldr>
<p>Latest Kotlin release:<b> <a href="%kotlinLatestWhatsnew%">%kotlinVersion%</a></b></p>
</tldr>

Kotlin is a modern language that's concise, multiplatform, and interoperable with Java and other languages.
<br />Kotlin 是一种现代语言，它简洁、跨平台，并且可以与 Java 和其他语言互操作。

New to Kotlin? Take our tour to learn the fundamentals directly in your browser.
<br />Kotlin新手？参加我们的入门教程，直接在浏览器中学习基础知识。

<a href="kotlin-tour-welcome.md"><img src="start-kotlin-tour.svg" width="700" alt="Start the Kotlin tour" style="block"/></a>

## Install Kotlin
安装 Kotlin

Kotlin is included in each [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) and [Android Studio](https://developer.android.com/studio) release.
Download and install one of these IDEs to start using Kotlin.
<br />Kotlin 已包含在每个 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) 和 [Android Studio](https://developer.android.com/studio) 版本中。
下载并安装其中一个 IDE 即可开始使用 Kotlin。

## Choose your Kotlin use case
选择你的 Kotlin 用例
 
<tabs>

<tab id="console" title="Console">

Here you'll learn how to develop a console application and create unit tests with Kotlin.
<br />在这里，你将学习如何使用 Kotlin 开发控制台应用程序并创建单元测试。

1. **[Create a basic JVM application with the IntelliJ IDEA project wizard](jvm-get-started.md).**
    <br />**[使用 IntelliJ IDEA 项目向导创建一个基本的 JVM 应用程序](jvm-get-started.md)**

2. **[Write your first unit test](jvm-test-using-junit.md).**
    <br />**[编写你的第一个单元测试](jvm-test-using-junit.md)**

</tab>

<tab id="backend" title="Backend">

Here you'll learn how to develop a backend application with Kotlin server-side.
<br />在这里，你将学习如何使用 Kotlin 服务器端开发后端应用程序。

* **Introduce Kotlin to your Java project:**
<br />将 Kotlin 引入到你的 Java 项目中：

  * [Configure a Java project to work with Kotlin](mixing-java-kotlin-intellij.md)
    <br />[配置 Java 项目以与 Kotlin 协同工作](mixing-java-kotlin-intellij.md)
  * [Add Kotlin tests to your Java Maven project](jvm-test-using-junit.md)
    <br />[将 Kotlin 测试添加到您的 Java Maven 项目中](jvm-test-using-junit.md)

* **Create a backend app from scratch with Kotlin:**
<br /> 使用 Kotlin 从零开始创建一个后端应用程序

  * [Create a RESTful web service with Spring Boot](jvm-get-started-spring-boot.md)
<br />[使用 Spring Boot 创建 RESTful Web 服务](jvm-get-started-spring-boot.md)
  * [Create HTTP APIs with Ktor](https://ktor.io/docs/creating-http-apis.html)
<br />[使用 Ktor 创建 HTTP API](https://ktor.io/docs/creating-http-apis.html)

</tab>

<tab id="cross-platform-mobile" title="Cross-platform">

Here you'll learn how to develop a cross-platform application using [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform/get-started.html).
<br />在这里，您将学习如何使用[Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform/get-started.html)开发跨平台应用程序。

1. **[Set up your environment for cross-platform development](https://kotlinlang.org/docs/multiplatform/quickstart.html).**
    <br />[为跨平台开发搭建环境](https://kotlinlang.org/docs/multiplatform/quickstart.html)
2. **Create your first application for iOS and Android:**
    <br />创建您的第一个 iOS 和 Android 应用：

   * Create a cross-platform application from scratch and:
         <br />从零开始创建一个跨平台应用程序
     * [Share business logic while keeping the UI native](https://kotlinlang.org/docs/multiplatform/multiplatform-create-first-app.html)
       <br />[在保持原生用户界面的同时，共享业务逻辑](https://kotlinlang.org/docs/multiplatform/multiplatform-create-first-app.html)
     * [Share business logic and UI](https://kotlinlang.org/docs/multiplatform/compose-multiplatform-create-first-app.html)
     <br />[共享业务逻辑和用户界面](https://kotlinlang.org/docs/multiplatform/compose-multiplatform-create-first-app.html)
   * [Make your existing Android application work on iOS](https://kotlinlang.org/docs/multiplatform/multiplatform-integrate-in-existing-app.html)
     <br />[让您现有的 Android 应用在 iOS 上运行](https://kotlinlang.org/docs/multiplatform/multiplatform-integrate-in-existing-app.html)
   * [Create a cross-platform application using Ktor and SQLdelight](https://kotlinlang.org/docs/multiplatform/multiplatform-ktor-sqldelight.html)
     <br />[使用 Ktor 和 SQLdelight 创建一个跨平台应用程序](https://kotlinlang.org/docs/multiplatform/multiplatform-ktor-sqldelight.html)

3. **Explore [sample projects](https://kotlinlang.org/docs/multiplatform/multiplatform-samples.html)**.
<br />**探索[示例项目](https://kotlinlang.org/docs/multiplatform/multiplatform-samples.html)**.

</tab>

<tab id="android" title="Android">

To start using Kotlin for Android development, read [Google's recommendation for getting started with Kotlin on Android](https://developer.android.com/kotlin/get-started).
<br />要开始使用 Kotlin 进行 Android 开发，请阅读[Google 关于在 Android 上使用 Kotlin 入门的建议](https://developer.android.com/kotlin/get-started).

</tab>

<tab id="data-analysis" title="Data analysis">

From building data pipelines to productionizing machine learning models, Kotlin is a great choice for working with data and getting the most out of it.
<br />从构建数据管道到将机器学习模型投入生产，Kotlin 是处理数据并充分利用数据的绝佳选择。

1. **Explore and experiment with your data:**

   * [DataFrame](https://kotlin.github.io/dataframe/overview.html) – a library for data analysis and manipulation.
   * [Kandy](https://kotlin.github.io/kandy/welcome.html) – a plotting tool for data visualization.

2. **Follow Kotlin for Data Analysis on Twitter:** [KotlinForData](http://twitter.com/KotlinForData).

</tab>

</tabs>

## Get support
获取支持

If you encounter any difficulties or problems, ask for help in ![Slack](slack.svg){width=25}{type="joined"} Slack: [get an invite](https://surveys.jetbrains.com/s3/kotlin-slack-sign-up) or report an issue in our [issue tracker](https://youtrack.jetbrains.com/issues/KT).
<br />如果您遇到任何困难或问题，请在 ![Slack](slack.svg){width=25}{type="joined"} Slack 中寻求帮助：[获取邀请](https://surveys.jetbrains.com/s3/kotlin-slack-sign-up) 或在我们的[问题跟踪器](https://youtrack.jetbrains.com/issues/KT) 中报告问题。

If anything is missing or seems confusing on this page, please [share your feedback](https://surveys.hotjar.com/d82e82b0-00d9-44a7-b793-0611bf6189df).
<br />如果此页面上有任何遗漏或令人困惑的内容，请[分享您的反馈](https://surveys.hotjar.com/d82e82b0-00d9-44a7-b793-0611bf6189df)。