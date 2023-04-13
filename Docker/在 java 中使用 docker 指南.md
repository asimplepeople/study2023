## 概述
Java 入门指南教您如何使用 Docker 创建容器化的 Spring Boot 应用程序。在本模块中，您将学习如何：

-   使用 Maven 克隆并运行 Spring Boot 应用程序
-   创建一个新的 Dockerfile，其中包含构建 Java 映像所需的指令
-   将新生成的映像作为容器运行
-   设置本地开发环境以将数据库连接到容器
-   使用 Docker Compose 运行 Spring Boot 应用程序
-   **使用 GitHub 操作为应用程序配置 CI/CD 管道** （重点）
-   将应用程序部署到云

完成 Java 入门模块后，您应该能够根据本指南中提供的示例和说明容器化您自己的 Java 应用程序。

## 构建你自己的java镜像

###  先决条件

-   您了解基本的 [Docker 概念](https://docs.docker.com/get-started/overview/)。
-   您熟悉 [Docker 文件格式](https://docs.docker.com/build/building/packaging/#dockerfile)。
-   您已在计算机上[启用构建工具包](https://docs.docker.com/build/buildkit/#getting-started)。

### 概述

现在我们已经很好地了解了容器和 Docker 平台，让我们来看看如何构建我们的第一个映像。映像包括运行应用程序所需的所有内容 - 代码或二进制文件、运行时、依赖项以及所需的任何其他文件系统对象。

若要完成本教程，需要以下各项：

-   在本地运行的 Docker。按照说明下载[并安装 Docker](https://docs.docker.com/get-docker/)
-   一个 Git 客户端
-   用于编辑文件的 IDE 或文本编辑器。我们建议使用 [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)。

### 示例应用程序

让我们将本模块中使用的示例应用程序克隆到本地开发计算机。在终端中运行以下命令以克隆存储库。

```
cd /path/to/working/directory
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

### 在没有 Docker 的情况下测试应用程序（可选）

在此步骤中，我们将在没有 Docker 的情况下在本地测试应用程序，然后再 继续使用 Docker 构建和运行应用程序。本节 要求您在计算机上安装 Java OpenJDK 版本 15 或更高版本。[下载并安装 Java](https://jdk.java.net/)

如果您不想在计算机上安装 Java，则可以跳过此步骤，并且 继续下一节，我们将在其中解释如何构建和运行 Docker 中的应用程序，它不需要您安装 Java 您的机器。

让我们启动我们的应用程序并确保它正常运行。Maven将管理所有项目流程（编译，测试，打包等）。我们之前克隆的**Spring Pets Clinic**项目包含一个嵌入式版本的Maven。因此，我们不需要在本地计算机上单独安装 Maven。

打开终端并导航到我们创建的工作目录并运行以下命令：

```
$ ./mvnw spring-boot:run
```

这将下载依赖项、生成项目并启动它。

要测试应用程序是否正常工作，请打开新浏览器并导航到 。`http://localhost:8080`

切换回运行我们服务器的终端，您应该会在服务器日志中看到以下请求。计算机上的数据将有所不同。

```
o.s.s.petclinic.PetClinicApplication     : Started
PetClinicApplication in 11.743 seconds (JVM running for 12.364)
```

伟大！我们验证了该应用程序是否有效。在此阶段，您已完成 在本地测试服务器脚本。

在运行服务器的终端会话中按以停止它。`CTRL-c`

现在，我们将继续在 Docker 中构建和运行应用程序。

### 创建适用于 Java 的 Dockerfile

接下来，我们需要在 Dockerfile 中添加一行，告诉 Docker 什么基础映像 我们想用于我们的应用程序。

```
# syntax=docker/dockerfile:1

FROM eclipse-temurin:17-jdk-jammy
```

Docker 镜像可以从其他镜像继承。在本指南中，我们使用Eclipse Termurin，这是最受欢迎的官方图像之一，具有值得构建的JDK。

为了使运行其余命令时更容易，让我们设置图像的 工作目录。这将指示 Docker 使用此路径作为默认位置 对于所有后续命令。通过这样做，我们不必输入完整的文件 路径，但可以使用基于工作目录的相对路径。

```
WORKDIR /app
```

通常，一旦你下载了一个编写的项目，你做的第一件事就是 使用Maven进行项目管理的Java是安装依赖项。

在我们运行之前，我们需要将 Maven 包装器和我们的文件放入我们的镜像中。我们将使用该命令来执行此操作。该命令采用两个参数。第一个参数告诉 Docker 什么 要复制到映像中的文件。第二个参数告诉 Docker 要将该文件复制到的位置。我们将复制所有这些文件，然后 目录到我们的工作目录中 - 。`mvnw dependency` `pom.xml` `COPY` `COPY` `/app`

```
COPY .mvn/ .mvn
COPY mvnw pom.xml ./
```


一旦我们将文件放在图像中，我们就可以使用命令 以执行命令 。这工作原理完全相同 就像我们在机器上本地运行（或）依赖项一样， 但这次依赖项将安装到映像中。`pom.xml` `RUN` `mvnw dependency:resolve` `mvnw` `mvn`
```
RUN ./mvnw dependency:resolve
```

此时，我们有一个基于 OpenJDK 版本 17 的 Eclipse Termurin 映像，并且我们还安装了依赖项。接下来我们需要做的是将我们的源代码添加到图像中。我们将像处理上面的文件一样使用该命令。`COPY` `pom.xml`

```
COPY src ./src
```

此命令获取当前目录中的所有文件，并将它们复制到映像中。现在，我们所要做的就是告诉 Docker 当我们的镜像在容器内执行时我们要运行什么命令。我们使用命令执行此操作。`COPY` `CMD`

```
CMD ["./mvnw", "spring-boot:run"]
```

此命令获取当前目录中的所有文件，并将它们复制到映像中。现在，我们所要做的就是告诉 Docker 当我们的镜像在容器内执行时我们要运行什么命令。我们使用命令执行此操作。`COPY``CMD`

```
CMD ["./mvnw", "spring-boot:run"]
```

这是完整的 Dockerfile。
```
# syntax=docker/dockerfile:1

FROM eclipse-temurin:17-jdk-jammy

WORKDIR /app

COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN ./mvnw dependency:resolve

COPY src ./src

CMD ["./mvnw", "spring-boot:run"]
```

#### 创建文件

为了提高构建的性能，并作为一般最佳实践，我们建议您在与 Dockerfile 相同的目录中创建一个文件。对于本教程，您的文件应仅包含一行： `.dockerignore`  `.dockerignore`

```
target
```

此行不包括目录，该目录包含来自 Maven 的输出， 从 Docker [构建上下文](https://docs.docker.com/build/building/context/)。有很多 仔细构建文件的充分理由，但是这一行 文件现在足够好了。`target` `.dockerignore`

### 构建镜像

现在我们已经创建了 Dockerfile，让我们构建我们的映像。为此，我们使用命令。该命令从 Dockerfile 和“上下文”构建 Docker 映像。生成的上下文是位于指定 PATH 或 URL 中的文件集。Docker 生成过程可以访问位于此上下文中的任何文件。`docker build``docker build`

构建命令可以选择采用标志。该标记用于设置图像的名称和格式为 .我们暂时省略可选选项，以帮助简化操作。如果我们不传递标签，Docker 使用“latest”作为其默认标签。您可以在生成输出的最后一行中看到这一点。`--tag``name:tag``tag`

让我们构建我们的第一个 Docker 镜像。

```
$ docker build --tag java-docker .
```

```
Sending build context to Docker daemon  5.632kB
Step 1/7 : FROM eclipse-temurin:17-jdk-jammy
Step 2/7 : WORKDIR /app
...
Successfully built a0bb458aabd0
Successfully tagged java-docker:latest
```

### 查看本地镜像

要查看本地计算机上的图像列表，我们有两个选项。一种是使用 CLI，另一种是使用 [Docker Desktop](https://docs.docker.com/desktop/use-desktop/images/)。由于我们目前正在终端中工作，让我们看一下使用 CLI 列出图像。

要列出图像，只需运行该命令。`docker images`

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED          SIZE
java-docker         latest              b1b5f29f74f0        47 minutes ago   567MB
```

您应该至少看到我们刚刚构建的映像。`java-docker:latest`

### 标记镜像

映像名称由斜杠分隔的名称组件组成。名称组件可以包含小写字母、数字和分隔符。分隔符定义为句点、一个或两个下划线或一个或多个短划线。名称组件不能以分隔符开头或结尾。

映像由清单和图层列表组成。此时不要太担心清单和层，除了“标记”指向这些工件的组合。一个图像可以有多个标签。让我们为我们构建的映像创建第二个标记，并查看其图层。

若要为上面生成的映像创建新标记，请运行以下命令：

```
$ docker tag java-docker:latest java-docker:v1.0.0
```

该命令为映像创建新标记。它不会创建新映像。标记指向同一图像，只是引用图像的另一种方式。`docker tag`

现在，运行命令以查看本地映像的列表。`docker images`

```
$ docker images
REPOSITORY    TAG      IMAGE ID		  CREATED		  SIZE
java-docker   latest   b1b5f29f74f0	  59 minutes ago	567MB
java-docker   v1.0.0   b1b5f29f74f0	  59 minutes ago	567MB
```

您可以看到我们有两个以 .我们知道它们是相同的图像，因为如果您查看该列，您可以看到两个图像的值相同。`java-docker``IMAGE ID`

让我们删除刚刚创建的标记。为此，我们将使用该命令。该命令代表“删除图像”。`rmi``rmi`

```
$ docker rmi java-docker:v1.0.0
Untagged: java-docker:v1.0.0
```

请注意，来自 Docker 的响应告诉我们该映像尚未被删除，而只是“未标记”。您可以通过运行命令来检查这一点。`docker images`

```
$ docker images
REPOSITORY      TAG     IMAGE ID        CREATED              SIZE
java-docker    	latest	b1b5f29f74f0	59 minutes ago	     567MB
```

我们标记的映像已被移除，但我们计算机上仍有该标记可用。`:v1.0.0` `java-docker:latest`

## 运行容器

### 先决条件

完成[构建 Java 映像](https://docs.docker.com/language/java/build-images/)中构建 Java 映像的步骤。

