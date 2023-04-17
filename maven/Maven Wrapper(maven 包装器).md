# 概述

Maven 包装器是确保 Maven 构建的用户拥有的简单方法 运行 Maven 构建所需的一切。

为什么有必要这样做？_Maven迄今为止对用户来说一直非常稳定，是 在大多数系统上可用或易于采购：但与许多最近的 Maven中的更改将使用户更容易拥有完全封装的构建 项目提供的设置。使用Maven包装器，这很容易做到 这是一个从 Gradle 借来的好主意和初始实现。

为您的项目设置 Maven 包装器的最简单方法是使用 [Maven 包装器插件](https://maven.apache.org/wrapper/maven-wrapper-plugin)及其提供的目标。添加或更新所有 项目所需的 Maven 包装器文件执行以下命令：`wrapper`

# Apache Maven Wrapper 插件

## 用法

Wapper 脚本可以在你的项目中添加一些文件，如果你想要构建maven项目，你不需要再去安装 Maven 了。取而代之的是，你可以执行Maven 包装器的脚本 （像 `mvnw` / `mvnw.`）,它能下载并解压maven在 `${user.home}/.m2/wrapper/dists` /  `%homepath%/.m2/wrapper/dists` 文件夹中，并且提供了一个简单的方法让任何人通过同一版本的maven构建项目。

Apache Maven Wrapper 插件，能很轻松的把包装器脚本加到你的项目中。

脚本的工作方式如下：
- 下载 Maven-wrapper jar，如果尚不可用，
-  maven-wrapper.jar包含下载，安装和运行Apache Maven的代码


## Apache Maven Wrapper 分发类型

有四种类型:
- only-scipt: mvnw / mvnw lite的实现。cmd脚本将maven直接通过下载wget或卷曲在* nix,或在Windows PowerShell,然后执行/调用原始的mvn / mvn。下载maven分布cmd脚本。这种不使用maven-wrapper。jar和MavenWrapperDownloader。java,只需要包装器脚本。

## 使用Maven存储库管理器

当使用内部Maven存储库管理器时，您有两个选择:

1. 设置正确的URL包装器jar projectmaven-wrapper.properties和Maven发行版