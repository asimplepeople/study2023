# 概述

Docker 是一个用于开发、交付和运行应用程序的开放平台。 Docker 使您能够将应用程序与基础架构分开，以便 您可以快速交付软件。使用 Docker，您可以管理您的基础架构 以与管理应用程序相同的方式。通过利用 Docker 的 快速传送、测试和部署代码的方法，您可以 显著减少编写代码和在生产环境中运行代码之间的延迟。

## 我们可以使用Docker做什么？

快速、一致地交付到您的应用程序

Docker 通过允许开发人员在 使用本地容器的标准化环境，为您的应用程序提供 和服务。容器非常适合持续集成和连续 交付 （CI/CD） 工作流。

请参考一下示例方案：
-   您的开发人员在本地编写代码并与同事共享他们的工作 使用 Docker 容器。
-   他们使用 Docker 将他们的应用程序推送到测试环境中并执行 自动和手动测试。
-   当开发人员发现错误时，他们可以在开发环境中修复它们 并将它们重新部署到测试环境进行测试和验证。
-   测试完成后，向客户提供修复程序非常简单： 将更新的映像推送到生产环境。
**响应式部署和扩展**

Docker 基于容器的平台允许高度可移植的工作负载。码头工人 容器可以在开发人员的本地笔记本电脑上运行，无论是物理的还是虚拟的 数据中心、云提供商或混合环境中的计算机。

Docker 的便携性和轻量级特性也使其易于动态传输 管理工作负载，扩展或缩减应用程序和服务 业务需求近乎实时地决定。

**在同一硬件上运行更多工作负载**

Docker是轻量级和快速的。它提供了一种可行、具有成本效益的替代方案 到基于虚拟机监控程序的虚拟机，以便您可以使用更多服务器 实现业务目标的能力。Docker非常适合高密度 环境以及需要执行更多操作的中小型部署 更少的资源。

## Docker的架构

Docker 使用客户端-服务器架构。Docker _客户端_与 Docker _守护进程_，它负责构建、运行和 分发您的 Docker 容器。Docker 客户端和守护程序_可以_在同一系统上运行，也可以将 Docker 客户端连接到远程 Docker 守护 进程。Docker 客户端和守护程序使用 REST API 通过 UNIX 进行通信 套接字或网络接口。另一个 Docker 客户端是 Docker Compose， 这使您可以处理由一组容器组成的应用程序。

![[Pasted image 20230411195705.png]]

### Docker 守护程序

Docker 守护程序 （）侦听 Docker API 请求并管理 Docker 对象，例如映像、容器、网络和卷。守护进程也可以 与其他守护程序通信以管理 Docker 服务。`dockerd`

### Docker 客户端

Docker 客户端 （） 是许多 Docker 用户交互的主要方式 与 Docker 一起。当您使用诸如 的命令时，客户端会发送这些命令 命令到 ，执行这些命令。该命令使用 Docker API。Docker 客户端可以与多个守护程序通信。`docker``docker run``dockerd``docker`

### Docker 桌面

Docker Desktop 是一款易于安装的应用程序，适用于您的 Mac、Windows 或 Linux 环境，使您能够构建和共享容器化应用程序和微服务。Docker Desktop 包括 Docker 守护程序 （）、Docker 客户端 （）、Docker Compose、Docker Content Trust、Kubernetes 和 Credential Helper。有关更多信息，请参阅 [Docker 桌面](https://docs.docker.com/desktop/)。`dockerd``docker`

### Docker 注册表

Docker _注册表_存储 Docker 映像。Docker Hub 是公共的 任何人都可以使用的注册表，并且 Docker 配置为在 默认情况下为 Docker Hub。您甚至可以运行自己的私有注册表。

使用 or 命令时，所需的图像是 从配置的注册表中提取。使用该命令时， 映像将推送到配置的注册表。`docker pull``docker run``docker push`

### Docker 对象

当您使用 Docker 时，您正在创建和使用映像、容器、网络、 卷、插件和其他对象。本节简要概述了一些 这些对象。

**图像**

**图像**是只读模板，其中包含有关创建 Docker 的说明 容器。通常，一个图像_基于_另一个图像，还有一些额外的 定制。例如，您可以构建一个基于该映像的映像，但会安装 Apache Web 服务器和您的应用程序，以及 使应用程序运行所需的配置详细信息。`ubuntu`

您可以创建自己的图像，也可以仅使用其他人创建的图像 并在注册表中发布。若要生成自己的映像，请使用简单的语法创建一个 _Dockerfile_，用于定义创建映像和运行所需的步骤 它。Dockerfile 中的每个指令都会在映像中创建一个图层。当你 更改 Dockerfile 并重建映像，仅更改那些具有 更改后重建。这是使图像如此轻巧，小巧， 与其他虚拟化技术相比，速度更快。

**容器**

容器是映像的可运行实例。您可以创建、启动、停止、 使用 Docker API 或 CLI 移动或删除容器。您可以连接一个 容器到一个或多个网络，将存储附加到其中，甚至创建新的 基于其当前状态的图像。

默认情况下，容器与其他容器的隔离相对较好，并且 它的主机。您可以控制容器的网络、存储、 或其他底层子系统来自其他容器或主机 机器。

容器由其映像以及任何配置选项定义 在创建或启动它时提供它。删除容器后，对 未存储在持久存储中的状态将消失。

示例命令 `docker run`
以下命令运行一个容器，以交互方式附加到您的 本地命令行会话，并运行 。`ubuntu``/bin/bash`

```
$ docker run -it ubuntu /bin/bash
```

运行此命令时，将发生以下情况（假设您正在使用 默认注册表配置）：

1.  如果本地没有映像，Docker 会将其从 配置注册表，就像您手动运行一样。`ubuntu``docker pull ubuntu`
2.  Docker 会创建一个新容器，就像您手动运行命令一样。`docker container create`
3.  Docker为容器分配一个读写文件系统，作为其最终 层。这允许正在运行的容器创建或修改文件和 其本地文件系统中的目录。
4.  Docker 创建一个网络接口，将容器连接到默认容器 网络，因为您没有指定任何网络选项。这包括 为容器分配 IP 地址。默认情况下，容器可以 **使用主机的网络连接连接到外部网络。**
5.  Docker 启动容器并执行 .因为容器 以交互方式运行并连接到终端（由于 和 标志），您可以在输出记录时使用键盘提供输入 您的终端。`/bin/bash`  `-i`  `-t`
6.  键入以终止命令时，容器 停止但不删除。您可以重新启动或删除它。`exit` `/bin/bash`

## 底层技术

Docker是用[Go编程语言](https://golang.org/)编写的，需要 利用 Linux 内核的几个特性来提供其功能。 Docker使用一种称为提供隔离工作区的技术 称为_容器_。运行容器时，Docker 会为该容器创建一组_命名空间_。`namespaces`

这些命名空间提供一层隔离。容器的每个方面都运行 在单独的命名空间中，其访问权限仅限于该命名空间。

# 开始使用
## 第一部分：概述

欢迎！我们很高兴您想学习 Docker。

本指南包含有关如何开始使用 Docker 的分步说明。您将在本指南中学习和执行的一些操作包括：

-   将镜像生成并作为容器运行。
-   使用 Docker Hub 共享映像。
-   使用具有数据库的多个容器部署 Docker 应用程序。
-   使用 Docker Compose 运行应用程序。

在掌握本指南的部分内容之前，您应该了解容器和图像。

### 什么是容器？

简而言之，容器是计算机上的沙盒进程，与主机上的所有其他进程隔离。这种隔离利用[内核命名空间和 cgroups](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504)， 在 Linux 中已经存在很长时间的功能。Docker一直致力于使这些功能变得平易近人且易于使用。总而言之，容器：
-   是图像的可运行实例。您可以使用 DockerAPI 或 CLI 创建、启动、停止、移动或删除容器。
-   可以在本地计算机、虚拟机上运行或部署到云中。
-   是可移植的（可以在任何操作系统上运行）。
-   与其他容器隔离，并运行自己的软件、二进制文件和配置。

### 什么是容器镜像？

运行容器时，它使用隔离的文件系统。此自定义文件系统由容器映像提供。由于映像包含容器的文件系统，因此它必须包含运行应用程序所需的一切 - 所有依赖项、配置、脚本、二进制文件等。该映像还包含容器的其他配置，例如环境变量、要运行的默认命令和其他元数据。

在本指南的后面部分，您将更深入地了解图像，涵盖分层、最佳做法等主题。

如果您熟悉，可以将容器看作。文件系统只是来自映像。但是，容器添加了额外的隔离，而这些隔离在简单地使用 chroot.chrootchroot 时是不可用的

## 第二部分： 容器化应用程序

在本指南的其余部分，您将使用一个运行在 Node.js 上的简单 todo 列表管理器。如果您不熟悉 Node.js，不要担心。本指南不需要任何以前的 JavaScript 经验。

要完成本指南，您需要以下各项：

-   在本地运行的 Docker。按照说明下载[并安装 Docker](https://docs.docker.com/get-docker/)。
-   一个 [Git 客户端](https://git-scm.com/downloads)。
-   用于编辑文件的 IDE 或文本编辑器。Docker建议使用[Visual Studio Code](https://code.visualstudio.com/)。
-   对[容器和图像](https://docs.docker.com/get-started/overview/#docker-objects)的概念性理解。

### 获取应用

1. 使用以下命令克隆[入门存储库](https://github.com/docker/getting-started/tree/master)：

```
$ git clone https://github.com/docker/getting-started.git
```
2. 查看克隆的存储库的内容。在目录中，您应该看到两个子目录（ 和 ）。`getting-started/app` `package.json` `src` `spec`

### 生成应用的容器镜像

若要生成[容器映像](https://docs.docker.com/get-started/overview/#docker-objects)，需要使用 .Dockerfile 只是一个基于文本的文件，没有包含指令脚本的文件扩展名。Docker 使用此脚本构建容器映像。`Dockerfile`

1. 在与文件相同的目录中，创建一个名为 的文件。您可以使用以下命令根据您的操作系统创建 Dockerfile。`app` `package.json` `Dockerfile`

在 Windows 命令提示符下，运行下面列出的以下命令。

将目录更改为目录。替换为目录的路径。`app` `\path\to\app` `getting-started\app`
```
$ cd \path\to\app
```

创建一个名为 的空文件 。`Dockerfile`

```
 $ type nul > Dockerfile
```

2.  使用文本编辑器或代码编辑器，将以下内容添加到 Dockerfile：
    
    ```
    # syntax=docker/dockerfile:1
       
    FROM node:18-alpine
    WORKDIR /app
    COPY . .
    RUN yarn install --production
    CMD ["node", "src/index.js"]
    EXPOSE 3000
    ```
**注意**
在 Dockerfile 示例中选择一个指令以了解有关该指令的更多信息。

3. 使用以下命令生成容器映像：
在终端中，将目录更改为目录。替换为目录的路径。`getting-started/app``/path/to/app``getting-started/app`

```sh
$ cd /path/to/app
```

生成容器镜像。
```sh
$ docker build -t getting-started .
```

该命令使用 Dockerfile 生成新的容器映像。您可能已经注意到Docker下载了很多“层”。这是因为您指示构建器要从映像开始。但是，由于您的机器上没有它，Docker 需要下载映像。`docker build``node:18-alpine`

Docker 下载映像后，Dockerfile 中的指令会复制到应用程序中，并用于安装应用程序的依赖项。该指令指定从此映像启动容器时要运行的默认命令。`yarn` `CMD`

最后，标志标记您的图像。简单地将其视为最终图像的人类可读名称。由于您命名了映像，因此可以在运行容器时引用该映像。`-t` `getting-started`

命令末尾的 告诉 Docker 它应该在当前目录中查找 。`.` `docker build` `Dockerfile`

### 启动应用容器

现在你已经拥有了镜像，你可以运行应用在一个容器中， 为此，你将使用该命令`docker run`

1. 使用命令启动容器，并指定刚创建的映像的名称：`docker run`
```sh
$ docker run -dp 3000:3000 getting-started
```

你使用了`-d`标志在”分离“模式（在后台）运行了一个新的容器。你也使用了`-p` 标志映射了本地端口3000和容器端口3000。没有这个端口映射，你将无法访问应用。

2. 几秒后，打开浏览器并访问，http://localhost:3000/ 你可以看到你的应用。 ![[Pasted image 20230411204141.png]]

此时，您应该有一个正在运行的待办事项列表管理器，其中包含一些项目，全部由您构建。

如果快速查看容器，应会看到至少一个正在使用映像且位于端口上的容器正在运行。要查看您的容器，您可以使用 CLI 或 Docker Desktop 的图形界面。`getting-started``3000`

```
$ docker ps
```
## 第三部分：更新应用程序

在第 [2 部分中](https://docs.docker.com/get-started/02_our_app/)，您容器化了待办事项应用程序。在本部分中，你将更新应用程序和容器映像。您还将学习如何停止和移除容器。

### 更新源代码

在以下步骤中，当您没有任何待办事项列表项时，您将“空文本”更改为“您还没有待办事项！在上面添加一个！
1. 使用新的空文本更新文件中的第56行。`src/static/js/app.js`
```
...
 -                <p className="text-center">No items yet! Add one above!</p>
 +                <p className="text-center">You have no todo items yet! Add one above!</p>
 ...
```

2. 构建你更新之后的镜像，使用你在第二章的一样的命令 `docker build
```
$ docker build -t getting-started .
```

3. 使用更新后的代码启动一个新容器。.

```
 $ docker run -dp 3000:3000 getting-started
```
你可能会看到这样的错误(id会不同):
```
docker: Error response from daemon: driver failed programming external connectivity on endpoint laughing_burnell 
(bb242b2ca4d67eba76e79474fb36bb5125708ebdabd7f45c8eaf16caaabde9dd): Bind for 0.0.0.0:3000 failed: port is already allocated.
```
### 移除旧容器

要移除容器，首先需要停止它。一旦它停止了，你就可以移除它。您可以使用CLI或Docker Desktop的图形界面删除旧容器。选择你觉得最舒服的选项。

#### 使用命令移除容器
1. 使用以下命令获取容器的ID.`docker ps
```
docker ps
```
2. 使用一下命令停止容器.使用id 替换 < the-container-id >`docker stop` `docker ps`.

```
docker stop  <the-container-id>
```

3. 一旦容器停止了， 你就可以使用以下命令进行删除
```
docker rm <the-container-id>
```
通过向命令中添加标志，可以在单个命令中停止和删除容器docker rm -f < the-container-id >

### 启动更新的应用容器

1. 现在，使用命令启动更新的应用程序。`docker run`
```
docker run -dp 3000:3000 getting-started
```
2. 在 [http://localhost:3000](http://localhost:3000/) 刷新浏览器，您应该会看到更新的帮助文本。

## 第四部分：共享应用程序

构建映像后，可以共享它。要共享 Docker 镜像，您必须使用 Docker 注册表。默认注册表是 Docker 中心，是你使用的所有映像的来源。

**码头工人编号**
> Docker ID 允许您访问 Docker Hub，这是世界上最大的容器映像库和社区。如果您没有 [Docker ID](https://hub.docker.com/signup)，请免费创建一个。

### 创建存储库
要推送镜像，您首先需要在 Docker Hub 上创建一个存储库。
1.  [注册](https://www.docker.com/pricing?utm_source=docker&utm_medium=webreferral&utm_campaign=docs_driven_upgrade)或登录到 [Docker Hub](https://hub.docker.com/)。
    
2.  选择“**创建存储库**”按钮。
    
3.  对于存储库名称，请使用 。确保可见性为 。`getting-started` `Public`
    
    > **私有仓库**
    > 
    > 您是否知道 Docker 提供私有存储库，允许您将内容限制为特定用户或团队？查看 [Docker 定价](https://www.docker.com/pricing?utm_source=docker&utm_medium=webreferral&utm_campaign=docs_driven_upgrade)页面上的详细信息。
    
4.  选择“**创建**”按钮。
如果您查看下图，可以看到一个示例 Docker 命令。此命令将推送到此存储库。
![[Pasted image 20230411213550.png]]
### 推送镜像
1. 在命令行中，尝试运行在 Docker Hub 上看到的推送命令。请注意，您的命令 将使用您的命名空间，而不是“docker”。
```
docker push docker/getting-started
 The push refers to repository [docker.io/docker/getting-started]
 An image does not exist locally with the tag: docker/getting-started
```
为什么会失败？push 命令正在寻找名为 docker/getting-start 的映像，但是 没有找到。如果你运行，你也不会看到一个。`docker image ls`

若要解决此问题，需要“标记”已构建的现有映像，以为其指定另一个名称。
2. 使用命令登录到 Docker Hub 。`docker login -u YOUR-USER-NAME`
3. 使用该命令为映像指定新名称。请务必换成您的 Docker ID。`docker tag``getting-started``YOUR-USER-NAME`

```sh
 docker tag getting-started YOUR-USER-NAME/getting-started
```

若要了解有关该命令的详细信息，请参阅 [docker 标记](https://docs.docker.com/engine/reference/commandline/tag/)。`docker tag`
4. 现在再次尝试您的推送命令。如果要从 Docker Hub 复制值，则可以删除该部分，因为您没有向映像名称添加标记。如果未指定标记，则 Docker 将使用名为 的标记。`tagname` `latest`

```
docker push YOUR-USER-NAME/getting-started
```

### 在新实例上运行映像
现在，映像已生成并推送到注册表中，请尝试在品牌上运行应用 从未见过此容器映像的新实例。为此，您将使用 Play with Docker。

Play with Docker 使用 amd64 平台。如果您使用的是带有 Apple 芯片的基于 ARM 的 Mac，则需要重新构建映像以与 Play with Docker 兼容，并将新映像推送到存储库。

要为 amd64 平台构建映像，请使用标志。`--platform`

```
docker build --platform linux/amd64 -t YOUR-USER-NAME/getting-started .
```
Docker buildx 还支持构建多平台镜像。若要了解详细信息，请参阅 [Mult-platform 映像](https://docs.docker.com/build/building/multi-platform/)。
1.  打开浏览器以[玩 Docker](https://labs.play-with-docker.com/)。
    
2.  选择**“登录**”，然后从下拉列表中选择**“docker**”。
    
3.  与您的 Docker Hub 帐户连接。
    
4.  登录后，选择左侧栏上的**“添加新实例**”选项。如果您没有看到它，请使您的浏览器更宽一点。几秒钟后，将在浏览器中打开一个终端窗口。
![[Pasted image 20230411214906.png]]
1.  在终端中，启动新推送的应用程序。
    
    ```
     $ docker run -dp 3000:3000 YOUR-USER-NAME/getting-started
    ```
    
    您应该看到映像被拉下并最终启动。
    
2.  当出现 3000 徽章时，选择 3000 徽章，您应该会看到带有您修改的应用程序。 如果未显示 3000 徽章，则可以选择“**打开端口**”按钮并键入 <>。
## 第五部分： 保留数据库
如果您没有注意到，我们的待办事项列表每次都会被擦除干净 我们启动容器。这是为什么呢？让我们深入了解容器的工作原理。
### 容器的文件系统
当容器运行时，它会将映像中的各种层用于其文件系统。 每个容器都有自己的“暂存空间”来创建/更新/删除文件。任何 更改不会在另一个容器中看到，_即使_它们使用相同的映像也是如此。
#### 在实践中看这一点
为了看到这一点，我们将启动两个容器并在每个容器中创建一个文件。 您将看到在一个容器中创建的文件在另一个容器中不可用。

1.启动一个容器，该容器将创建一个以随机数命名的文件 介于 1 和 10000 之间。`ubuntu``/data.txt`

```
 $ docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
```
如果您对该命令感到好奇，我们将启动一个 bash shell 并调用两个 命令（为什么我们有 ）。第一部分选取一个随机数并写入 它到 .第二个命令只是监视文件以保持容器运行。`&&``/data.txt`
2. 验证是否可以通过访问容器中的终端来查看输出。为此，请转到 Docker Desktop 中的容器，将鼠标悬停在运行 **ubuntu** 映像的容器上，然后选择“**显示容器操作**”菜单。从下拉菜单中，选择在**终端中打开**。
 您将看到一个在 Ubuntu 容器中运行 shell 的终端。运行以下命令以查看文件的内容。之后再次关闭此终端。`/data.txt`

```
 $ cat /data.txt
```
如果您更喜欢命令行，则可以使用该命令来执行相同的操作。你需要得到 容器的 ID（用于获取它）并使用以下命令获取内容。`docker exec``docker ps`

```
 $ docker exec <container-id> cat /data.txt
```
你应该看到一个随机数！
3. 现在，让我们启动另一个容器（相同的图像），我们将看到我们没有相同的容器 文件。`ubuntu`

```
 $ docker run -it ubuntu ls /
```
在这种情况下，该命令将列出容器根目录中的文件。 看，那里没有文件！那是因为它被写入了暂存空间 只有第一个容器。`data.txt`
4. 继续并使用命令删除第一个容器。`docker rm -f <container-id>` 
### 容器的容积
在前面的实验中，我们看到每个容器每次启动时都从映像定义开始。 虽然容器可以创建、更新和删除文件，但当容器被删除时，这些更改会丢失 并且所有更改都隔离到该容器。有了卷，我们可以改变这一切。

[卷](https://docs.docker.com/storage/volumes/)提供了连接特定文件系统路径的能力 容器返回到主机。如果挂载了容器中的目录，则会更改该目录 目录也会显示在主机上。如果我们在容器重启后挂载相同的目录，我们会看到 相同的文件。

卷有两种主要类型。我们最终将同时使用两者，但我们将从卷装载开始。

### 保留代办事项数据
默认情况下，待办事项应用将其数据存储在容器文件系统中的 SQLite 数据库中。如果您不熟悉SQLite，请不要担心！它只是一个关系数据库 所有数据存储在单个文件中。虽然这对于大规模应用程序来说不是最好的， 它适用于小型演示。稍后我们将讨论如何将其切换到其他数据库引擎。`/etc/todos/todo.db`
由于数据库是单个文件，如果我们可以将该文件保留在主机上并使其可用于 下一个容器，它应该能够从最后一个容器停止的地方继续。通过创建卷并附加 （通常称为“挂载”）它到存储数据的目录中，我们可以持久化数据。作为我们的容器 写入文件时，它将保存到卷中的主机。`todo.db`
如前所述，我们将使用卷装载。将卷装载视为不透明的数据桶。 Docker 完全管理卷，包括卷在磁盘上的存储位置。你只需要记住 卷的名称。

1. 使用该命令创建卷。`docker volume create`

```
 $docker volume create todo-db
```

2.  在仪表板（或 ）中再次停止并删除待办事项应用程序容器，因为它仍在运行，而不使用持久卷。`docker rm -f <id>`
3. 启动待办事项应用容器，但添加用于指定卷装载的选项。我们将为卷命名，并挂载 它到容器中，这将捕获在路径上创建的所有文件。`--mount` `/etc/todos`

```
 $ docker run -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
```
4. 容器启动后，打开应用并将一些项目添加到待办事项列表中。
![[Pasted image 20230412115636.png]]
5.  停止并删除待办事项应用的容器。使用仪表板或获取 ID，然后将其删除。`docker ps``docker rm -f <id>`
    
6.  使用上面的相同命令启动一个新容器。
    
7.  打开应用。您应该会看到您的项目仍在您的列表中！
    
8.  继续并在完成签出列表后删除容器。
万岁！现在，您已经学会了如何持久化数据！

### 深入了解卷
很多人经常问“当我使用卷时，Docker 将我的数据存储在哪里？如果你想知道， 您可以使用该命令。`docker volume inspect`

```
$ docker volume inspect todo-db
[
    {
        "CreatedAt": "2019-09-26T02:18:36Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/todo-db/_data",
        "Name": "todo-db",
        "Options": {},
        "Scope": "local"
    }
]
```
这是磁盘上存储数据的实际位置。请注意，在大多数计算机上，您将 需要具有根访问权限才能从主机访问此目录。但是，这就是它所在的地方！`Mountpoint`

> 在 Docker Desktop 中运行时，Docker 命令实际上在计算机上的小型 VM 中运行。 如果要查看挂载点目录的实际内容，则需要查看 该虚拟机。

## 第六部分： 使用绑定装载

### 快速卷类型比较

在第 [5 部分中](https://docs.docker.com/get-started/05_persisting_data/)，我们讨论并使用卷装载来持久化 我们数据库中的数据。当您在某个地方需要时，卷装载是一个不错的选择 持久用于存储应用程序数据。

绑定挂载是另一种类型的挂载，它允许您从 主机的文件系统放入容器中。处理应用程序时，您可以 使用绑定装载将源代码装载到容器中。容器看到 保存文件后立即对代码进行更改。这意味着 您可以在容器中运行监视文件系统更改的进程 并回应他们。

在本章中，我们将了解如何使用绑定挂载和一个名为[nodemon](https://npmjs.com/package/nodemon)的工具来监视文件更改，然后重新启动应用程序 自然而然。大多数其他语言都有等效的工具，并且 框架。
#### 快速卷类型的比较
下表概述了卷装入和绑定装入之间的主要区别。
|                                              | Named volumes                                    | Bind mounts                                          | 
|:--------------------------------------------:| ------------------------------------------------ | ---------------------------------------------------- | 
|                   主机位置                   | Docker chooses                                   | You decide                                           |  
|      挂在方式 (using `--mount`)        | type=volume,src=my-volume,target=/usr/local/data | `type=bind,src=/path/to/data,target=/usr/local/data` |  
| 用容器内容填充新卷 | yes                                              | no                                                 |  
|          支持卷驱动程序            | yes                                              | no                                                  | 

> 用volume创建的卷会把容器内的文件带到卷里面。
> 如果多个容器挂载同一个volume，已经存在的文件不会被覆盖， 并且如果是同一个镜像的话，不会对文件进行追加。不同镜像使用同一个volume会对文件进行追加。

### 测试绑定卷

在了解如何使用绑定挂载来开发应用程序之前， 让我们运行一个快速实验，以实际了解绑定如何挂载 工作。

如果你在Windows上按照这些步骤操作，请确保使用PowerShell而不是 命令提示符 。`cmd`

1. 打开终端并确保当前工作目录位于入门存储库的目录中。`app`
2. 运行以下命令以在具有 绑定挂载。`bash``ubuntu`
```
docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
```
该选项告诉 Docker 创建绑定挂载，其中 主机上的当前工作目录 （），并且是该目录应出现在容器内的位置 （）。`--mount` `src` `getting-started/app` `target` `/src`
3. 运行命令后，Docker 在 容器文件系统的根目录。`bash`

```
root@ac1237fad8db:/# pwd
/
root@ac1237fad8db:/# ls
bin   dev  home  media  opt   root  sbin  srv  tmp  var
boot  etc  lib   mnt    proc  run   src   sys  usr
```
4. 现在，更改目录中的目录。`src`
这是启动容器时装载的目录。清单 此目录的内容显示的文件与主机上目录中的文件相同。`getting-started/app`

```
root@ac1237fad8db:/# cd src
root@ac1237fad8db:/src# ls
Dockerfile  node_modules  package.json  spec  src  yarn.lock
```
5. 创建一个名为 的新文件 。`myfile.txt`

```
root@ac1237fad8db:/src# touch myfile.txt
root@ac1237fad8db:/src# ls
Dockerfile  myfile.txt  node_modules  package.json  spec  src  yarn.lock
```
6. 现在，如果您在主机上打开此目录，您将看到该文件 已在目录中创建。`myfile.txt`
![[Pasted image 20230412123337.png]]

7.  从主机中删除该文件。`myfile.txt`
8.  在容器中，再次列出目录的内容。你会 看到文件现在消失了。`app` 
```
root@ac1237fad8db:/src# ls
Dockerfile  node_modules  package.json  spec  src  yarn.lock
```
9. 使用 + 停止交互式容器会话。`Ctrl` `D`
以上就是对绑定挂载的简要介绍。此过程 演示了如何在主机和容器之间共享文件，以及如何 变化立即反映在双方。现在让我们看看如何使用 绑定挂载以开发软件。

### 在开发容器中运行应用
以下步骤介绍如何使用绑定运行开发容器 执行以下操作的装载：

-   将我们的源代码挂载到容器中
-   安装所有依赖项
-   开始监视文件系统更改`nodemon`
所以，让我们开始吧！

1.  确保当前没有任何容器在运行。`getting-started`
    
2.  从目录运行以下命令。`getting-started/app`
    
    如果您使用的是 Mac 或 Linux 设备，请使用以下命令。
```
docker run -dp 3000:3000 \
    -w /app --mount type=bind,src="$(pwd)",target=/app \
    node:18-alpine \
    sh -c "yarn install && yarn run dev"
```
如果您使用的是 Windows，请在 PowerShell 中使用以下命令。
```
docker run -dp 3000:3000 `
    -w /app --mount type=bind,src="$(pwd)",target=/app `
    node:18-alpine `
    sh -c "yarn install && yarn run dev"
```
-   `-dp 3000:3000`- 和以前一样。在分离（后台）模式下运行，并且 创建端口映射
-   `-w /app`- 设置“工作目录”或当前目录 命令将从
-   `--mount type=bind,src="$(pwd)",target=/app`- 绑定安装电流 目录从主机进入容器中的目录`/app`
-   `node:18-alpine`- 要使用的图像。请注意，这是 我们的应用程序来自 Dockerfile
-   `sh -c "yarn install && yarn run dev"`- 命令。我们正在开始一个 贝壳使用（阿尔卑斯山没有）并跑到 安装软件包，然后运行以开始开发 服务器。如果我们查看 ，我们将看到脚本 开始。`sh``bash``yarn install``yarn run dev``package.json``dev``nodemon`
3. 您可以使用 查看日志。你会知道你已经准备好了 当你看到这个：`docker logs <container-id>`

```
docker logs -f <container-id>
nodemon src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching dir(s): *.*
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000
```
观看完日志后，按 + 退出。`Ctrl``C`
4. 现在，对应用程序进行更改。在文件中，在线 109、将“添加项目”按钮改为简单地说“添加”：`src/static/js/app.js`
```
- {submitting ? 'Adding...' : 'Add Item'}
+ {submitting ? 'Adding...' : 'Add'}
```
保存文件。
5. 在 Web 浏览器中刷新页面，您应该会看到反映的更改 几乎立即。节点服务器可能需要几秒钟才能 重新启动。如果收到错误，请尝试在几秒钟后刷新。
![[Pasted image 20230412235926.png]]
6. 随意进行您想要进行的任何其他更改。每次你做一个 更改并保存文件，该过程将重新启动应用程序内部 容器自动。完成后，停止容器并构建您的 使用的新图像：`nodemon`

```
docker build -t getting-started .
```
在本地开发设置中，使用绑定装载很常见。优点是 开发计算机不需要拥有所有构建工具和 已安装的环境。使用单个命令、依赖项和 工具已拉动并准备就绪。我们将来会讨论 Docker Compose 步骤，因为这将有助于简化我们的命令（我们已经得到很多 标志）。`docker run`
除了卷挂载和绑定挂载之外，Docker 还支持其他挂载 类型和存储驱动程序，用于处理更复杂和更专业的用例。 要了解有关高级存储概念的更多信息，请参阅[在 Docker 中管理数据](https://docs.docker.com/storage/)。
## 第七部分： 多容器应用

到目前为止，你一直在使用单个容器应用。但是，现在您将 MySQL 添加到 应用程序堆栈。经常出现以下问题 - “MySQL 将在哪里运行？将其安装在相同的 容器还是单独运行？一般来说，每个容器应该做一件事，并且做得好。以下是单独运行容器的几个原因：

-   您很有可能必须以不同于数据库的方式扩展 API 和前端。
-   单独的容器允许您单独进行版本控制和更新。
-   虽然可以在本地使用数据库容器，但可能需要使用托管服务 用于生产中的数据库。那时，您不希望将数据库引擎与应用一起交付。
-   运行多个进程将需要进程管理器（容器仅启动一个进程），这增加了容器启动/关闭的复杂性。

还有更多原因。因此，如下图所示，最好在多个容器中运行应用。

![Todo App connected to MySQL container](https://docs.docker.com/get-started/images/multi-app-architecture.png)
### 容器网络
请记住，默认情况下，容器是隔离运行的，并且对其他进程一无所知 或同一台计算机上的容器。那么，如何允许一个容器与另一个容器通信呢？答案是 联网。如果将两个容器放在同一个网络上，它们可以相互通信。
### 启动我的数据库
有两种方法可以将容器放在网络上：

-   启动容器时分配网络。
-   将已在运行的容器连接到网络。

在以下步骤中，你将首先创建网络，然后在启动时附加 MySQL 容器。

1.  创建网络。
    
    ```
    $ docker network create todo-app
    ```
    
2.  启动 MySQL 容器并将其附加到网络。您还将定义一些环境变量，这些变量 数据库将用于初始化数据库。要了解有关 MySQL 环境变量的更多信息，请参阅 [MySQL Docker Hub 列表中的](https://hub.docker.com/_/mysql/)“环境变量”部分。
    
    -   Mac / Linux
    -   窗户
    
    ```
     $ docker run -d \
         --network todo-app --network-alias mysql \
         -v todo-mysql-data:/var/lib/mysql \
         -e MYSQL_ROOT_PASSWORD=secret \
         -e MYSQL_DATABASE=todos \
         mysql:8.0
    ```
    
    ---
    
    在上面的命令中，您将看到标志。在后面的部分中，您将了解有关此标志的更多信息。`--network-alias`
    
    > **提示**
    > 
    > 您会注意到上面命令中命名的卷挂载在 ，这是 MySQL 存储其数据的位置。但是，您从未运行过命令。Docker 识别出您想要使用命名卷，并自动为您创建一个。`todo-mysql-data``/var/lib/mysql``docker volume create`
    
3.  若要确认数据库已启动并正在运行，请连接到数据库并验证它是否已连接。
    
    ```
     $ docker exec -it <mysql-container-id> mysql -u root -p
    ```
    
    当密码提示出现时，键入 。在 MySQL shell 中，列出数据库并验证 您会看到数据库。`secret``todos`
    
    ```
     mysql> SHOW DATABASES;
    ```
    
    您应该看到如下所示的输出：
    
    ```
     +--------------------+
     | Database           |
     +--------------------+
     | information_schema |
     | mysql              |
     | performance_schema |
     | sys                |
     | todos              |
     +--------------------+
     5 rows in set (0.00 sec)
    ```
    
4.  退出 MySQL 外壳以返回到计算机上的外壳。
    
    ```
    mysql> exit
    ```
    
    您现在有一个数据库，可供您使用。`todos`
### 链接到mysq
现在您知道MySQL已启动并运行，您可以使用它。但是，你如何使用它？如果您运行 同一网络上的另一个容器，如何找到该容器？请记住，每个容器都有自己的 IP 地址。

为了回答上述问题并更好地了解容器网络，您将使用 [nicolaka/netshoot](https://github.com/nicolaka/netshoot) 容器， 它附带了许多可用于故障排除或调试网络问题的工具。

1.  使用 nicolaka/netshoot 映像启动一个新容器。确保将其连接到同一网络。
    
    ```
     $ docker run -it --network todo-app nicolaka/netshoot
    ```
    
2.  在容器内部，您将使用该命令，这是一个有用的 DNS 工具。你要抬头 主机名的 IP 地址。`dig``mysql`
    
    ```
     $ dig mysql
    ```
    
    您应该获得如下所示的输出。
    
    ```
     ; <<>> DiG 9.18.8 <<>> mysql
     ;; global options: +cmd
     ;; Got answer:
     ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32162
     ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
    
     ;; QUESTION SECTION:
     ;mysql.				IN	A
    
     ;; ANSWER SECTION:
     mysql.			600	IN	A	172.23.0.2
    
     ;; Query time: 0 msec
     ;; SERVER: 127.0.0.11#53(127.0.0.11)
     ;; WHEN: Tue Oct 01 23:47:24 UTC 2019
     ;; MSG SIZE  rcvd: 44
    ```
    
    在“答案部分”中，您将看到解析为的记录（您的IP地址很可能具有不同的值）。虽然通常不是有效的主机名， Docker 能够将其解析为具有该网络别名的容器的 IP 地址。请记住，您使用了较早的。`A``mysql``172.23.0.2``mysql``--network-alias`
    
    这意味着您的应用程序只需要连接到名为的主机，它将与 数据库。`mysql`l
### 使用Mysql运行应用

待办事项应用程序支持设置一些环境变量来指定MySQL连接设置。它们是：

-   `MYSQL_HOST`- 正在运行的MySQL服务器的主机名
-   `MYSQL_USER`- 用于连接的用户名
-   `MYSQL_PASSWORD`- 用于连接的密码
-   `MYSQL_DB`- 连接后要使用的数据库

> **注意**
> 
> 虽然使用 env vars 来设置连接设置通常被接受用于开发，但强烈建议不要这样做 在生产中运行应用程序时。Docker的前安全主管Diogo Monica[写了一篇精彩的博客文章](https://diogomonica.com/2017/03/27/why-you-shouldnt-use-env-variables-for-secret-data/)来解释原因。
> 
> 更安全的机制是使用容器业务流程框架提供的机密支持。在大多数情况下， 这些机密作为文件装载到正在运行的容器中。您将看到许多应用程序（包括MySQL映像和待办事项应用程序） 还支持带有后缀的 env var，以指向包含变量的文件。`_FILE`
> 
> 例如，设置 var 将导致应用使用引用文件的内容 作为连接密码。Docker没有做任何事情来支持这些env vars。你的应用需要知道才能查找 变量并获取文件内容。`MYSQL_PASSWORD_FILE`

现在可以启动开发就绪容器。

1.  指定上面的每个环境变量，并将容器连接到应用网络。确保运行此命令时位于目录中。`getting-started/app`
    
    -   Mac / Linux
    -   窗户
    
    ```
     $ docker run -dp 3000:3000 \
       -w /app -v "$(pwd):/app" \
       --network todo-app \
       -e MYSQL_HOST=mysql \
       -e MYSQL_USER=root \
       -e MYSQL_PASSWORD=secret \
       -e MYSQL_DB=todos \
       node:18-alpine \
       sh -c "yarn install && yarn run dev"
    ```
    
    ---
    
2.  如果查看容器 （） 的日志，应会看到类似于以下内容的消息，指示它是 使用 MySQL 数据库。`docker logs -f <container-id>`
    
    ```
     $ nodemon src/index.js
     [nodemon] 2.0.20
     [nodemon] to restart at any time, enter `rs`
     [nodemon] watching dir(s): *.*
     [nodemon] starting `node src/index.js`
     Connected to mysql db at host mysql
     Listening on port 3000
    ```
    
3.  在浏览器中打开应用程序，然后将一些项目添加到待办事项列表中。
    
4.  连接到 mysql 数据库并证明项目正在写入数据库。记住，密码 是。`secret`
    
    ```
     $ docker exec -it <mysql-container-id> mysql -p todos
    ```
    
    在 mysql shell 中，运行以下命令：
    
    ```
     mysql> select * from todo_items;
     +--------------------------------------+--------------------+-----------+
     | id                                   | name               | completed |
     +--------------------------------------+--------------------+-----------+
     | c906ff08-60e6-44e6-8f49-ed56a0853e85 | Do amazing things! |         0 |
     | 2912a79e-8486-4bc3-a4c5-460793a575ab | Be awesome!        |         0 |
     +--------------------------------------+--------------------+-----------+
    ```
    
    您的表格看起来会有所不同，因为它包含您的项目。但是，您应该看到它们存储在那里。
## 第八部分： 使用Docker compose

[Docker Compose](https://docs.docker.com/compose/) 是一个工具，旨在帮助定义和 共享多容器应用程序。使用 Compose，我们可以创建一个 YAML 文件来定义服务 只需一个命令，就可以旋转或拆除所有内容。

使用 Compose _的最大优点是_您可以在文件中定义应用程序堆栈，并将其保留在 您的项目存储库（现在受版本控制），并轻松使其他人能够参与您的项目。 有人只需要克隆你的存储库并启动撰写应用程序。事实上，你可能会看到很多项目 在GitHub / GitLab上，现在正是这样做的。

那么，我们如何开始呢？

### 安装Docker compose

如果您安装了适用于 Windows 或 Mac 的 Docker Desktop/Toolbox，那么您已经拥有 Docker Compose！ Play-with-Docker 实例也已经安装了 Docker Compose。如果您在 一台 Linux 机器，您将需要[安装 Docker Compose](https://docs.docker.com/compose/install/)。

安装后，您应该能够运行以下命令并查看版本信息。

```
$ docker compose version
```

### 创建docker-compose文件
1.  在文件夹的根目录中，创建一个名为 的文件。`/getting-started/app` `docker-compose.yml`

2.  在撰写文件中，我们将首先定义要作为应用程序的一部分运行的服务（或容器）列表。
```
services:
```


现在，我们将开始一次将服务迁移到撰写文件中。

### 定义应用服务
请记住，这是我们用来定义应用容器的命令。

```
$ docker run -dp 3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```

1.  首先，让我们定义容器的服务条目和映像。我们可以为服务选择任何名称。 该名称将自动成为网络别名，这在定义我们的 MySQL 服务时很有用。
    
    ```
    
    services:
      app:
        image: node:18-alpine
    ```
    
2.  通常，您会看到接近定义的，尽管对排序没有要求。 因此，让我们继续将其移动到我们的文件中。`command``image`
    
    ```
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
    ```
    
3.  让我们通过为服务定义 来迁移命令的一部分。我们将在这里使用[短](https://docs.docker.com/compose/compose-file/compose-file-v3/#short-syntax-1)语法，但也有一个更详细的[长语法](https://docs.docker.com/compose/compose-file/compose-file-v3/#long-syntax-1)可用。`-p 3000:3000``ports`
    
    ```
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 3000:3000
    ```
    
4.  接下来，我们将使用 和定义。卷也有[短](https://docs.docker.com/compose/compose-file/compose-file-v3/#short-syntax-3)语法和[长](https://docs.docker.com/compose/compose-file/compose-file-v3/#long-syntax-3)语法。`-w /app``-v "$(pwd):/app"``working_dir``volumes`
    
    Docker Compose 卷定义的一个优点是，我们可以使用当前目录中的相对路径。
    
    ```
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 3000:3000
        working_dir: /app
        volumes:
          - ./:/app
    ```
    
5.  最后，我们需要使用 key 迁移环境变量定义。`environment`
    
    ```
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 3000:3000
        working_dir: /app
        volumes:
          - ./:/app
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: root
          MYSQL_PASSWORD: secret
          MYSQL_DB: todos
    ```

### 定义mysql服务

现在，是时候定义MySQL服务了。我们用于该容器的命令如下：

```
$ docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

1.  我们将首先定义新服务并命名它，以便它自动获取网络别名。我们会 继续并指定要使用的图像。`mysql`
    
    ```
    
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
    ```
    
2.  接下来，我们将定义卷映射。当我们使用 运行容器时，创建了命名卷 自然而然。但是，使用 Compose 运行时不会发生这种情况。我们需要在顶级部分中定义卷，然后在服务配置中指定挂载点。只需提供卷名， 使用默认选项。不过还有[更多可用的选项](https://docs.docker.com/compose/compose-file/compose-file-v3/#volume-configuration-reference)。`docker run``volumes:`
    
    ```
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql
    
    volumes:
      todo-mysql-data:
    ```
    
3.  最后，我们只需要指定环境变量。
    
    ```
    services:
      app:
        # The app service definition
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: secret
          MYSQL_DATABASE: todos
    
    volumes:
      todo-mysql-data:
    ```
    

此时，我们的完整应该如下所示：`docker-compose.yml`

```
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```
### 运行应用程序堆栈

现在我们有了文件，我们可以启动它了！`docker-compose.yml`

1.  确保没有应用程序/数据库的其他副本首先运行（ 和 ）。`docker ps``docker rm -f <ids>`
    
2.  使用该命令启动应用程序堆栈。我们将添加标志以运行 背景。`docker compose up``-d`
    
    ```
     $ docker compose up -d
    ```
    
    当我们运行这个时，我们应该看到这样的输出：
    
    ```
     Creating network "app_default" with the default driver
     Creating volume "app_todo-mysql-data" with default driver
     Creating app_app_1   ... done
     Creating app_mysql_1 ... done
    ```
    
    您会注意到卷和网络都已创建！默认情况下，Docker Compose 会自动为应用程序堆栈创建一个网络（这就是我们没有在撰写文件中定义网络的原因）。
    
3.  让我们使用该命令查看日志。你将看到来自每个服务的日志交错 到单个流中。当您想要监视与时间相关的问题时，这非常有用。标志“遵循”的 日志，因此会在生成时为您提供实时输出。`docker compose logs -f``-f`
    
    如果已运行该命令，则会看到如下所示的输出：
    
    ```
     mysql_1  | 2019-10-03T03:07:16.083639Z 0 [Note] mysqld: ready for connections.
     mysql_1  | Version: '8.0.31'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
     app_1    | Connected to mysql db at host mysql
     app_1    | Listening on port 3000
    ```
    
    服务名称显示在行的开头（通常为彩色），以帮助区分消息。如果你愿意 查看特定服务的日志时，可以将服务名称添加到 logs 命令的末尾（例如，）。`docker compose logs -f app`
    
    > **提示：在启动应用程序之前等待数据库**
    > 
    > 当应用程序启动时，它实际上坐下来等待MySQL启动。 并在尝试连接到它之前做好准备。Docker没有任何内置的 支持等待另一个容器完全启动、运行和准备就绪 在启动另一个容器之前。对于基于节点的项目，您可以使用 [等待端口](https://github.com/dwmkerr/wait-port)依赖项。其他语言/框架也存在类似的项目。
    
4.  此时，您应该能够打开您的应用程序并看到它正在运行。嘿！我们只剩下一个命令！

### 查看Docker仪表盘中的程序对战
如果我们查看 Docker 仪表板，我们会看到有一个名为 **app** 的组。这是来自Docker的“项目名称” 组合并用于将容器组合在一起。默认情况下，项目名称只是 所在的目录的名称。`docker-compose.yml`

![带有应用项目的 Docker 仪表板](https://docs.docker.com/get-started/images/dashboard-app-project-collapsed.png)

如果单击**应用**旁边的披露箭头，您将看到我们在撰写文件中定义的两个容器。名字也有点 更具描述性，因为它们遵循 的模式。所以，很容易 快速查看哪个容器是我们的应用，哪个容器是 MySQL 数据库。`<service-name>-<replica-number>`

![扩展了应用项目的 Docker 仪表板](https://docs.docker.com/get-started/images/dashboard-app-project-expanded.png)
### 删除全部
当您准备好将其全部拆除时，只需在 Docker 仪表板上运行或点击垃圾桶即可 对于整个应用程序。容器将停止，网络将被移除。`docker compose down`

> **警告**
> 
> 删除卷
> 
> 默认情况下，运行 时不会删除撰写文件中的命名卷。如果你愿意 删除卷，您将需要添加标志。`docker compose down``--volumes`
> 
> 删除应用程序堆栈时，Docker 仪表板_不会_删除卷。

拆除后，您可以切换到另一个项目，运行并准备好为该项目做出贡献！真的 没有比这更简单的了！`docker compose up`

## 第九部分： 镜像构建最佳实践

### 图像分层
您知道可以查看构成图像的内容吗？使用该命令，您可以看到用于在图像中创建每个图层的命令。`docker image history`

1.  使用该命令查看图像中的图层 在本教程前面部分创建。`docker image history``getting-started`
    
    ```
     $ docker image history getting-started
    ```
    
    您应该获得如下所示的输出（日期/ID 可能不同）。
    
    ```
     IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
     a78a40cbf866        18 seconds ago      /bin/sh -c #(nop)  CMD ["node" "src/index.j…    0B                  
     f1d1808565d6        19 seconds ago      /bin/sh -c yarn install --production            85.4MB              
     a2c054d14948        36 seconds ago      /bin/sh -c #(nop) COPY dir:5dc710ad87c789593…   198kB               
     9577ae713121        37 seconds ago      /bin/sh -c #(nop) WORKDIR /app                  0B                  
     b95baba1cfdb        13 days ago         /bin/sh -c #(nop)  CMD ["node"]                 0B                  
     <missing>           13 days ago         /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B                  
     <missing>           13 days ago         /bin/sh -c #(nop) COPY file:238737301d473041…   116B                
     <missing>           13 days ago         /bin/sh -c apk add --no-cache --virtual .bui…   5.35MB              
     <missing>           13 days ago         /bin/sh -c #(nop)  ENV YARN_VERSION=1.21.1      0B                  
     <missing>           13 days ago         /bin/sh -c addgroup -g 1000 node     && addu…   74.3MB              
     <missing>           13 days ago         /bin/sh -c #(nop)  ENV NODE_VERSION=12.14.1     0B                  
     <missing>           13 days ago         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B                  
     <missing>           13 days ago         /bin/sh -c #(nop) ADD file:e69d441d729412d24…   5.59MB   
    ```
    
    每条线表示图像中的一个图层。此处的显示屏显示了底部的底座 顶部的最新图层。使用它，您还可以快速查看每个图层的大小，帮助 诊断大图像。
    
2.  您会注意到其中几行被截断。如果添加标志，您将获得 全输出（是的...有趣的是你如何使用截断的标志来获得未截断的输出，是吧？`--no-trunc`
    
    ```
     $ docker image history --no-trunc getting-started
    ```

### 图像缓存
现在，您已经了解了分层的实际应用，需要学习一个重要的课程来帮助减少构建 容器映像的时间。

> 一旦层发生变化，还必须重新创建所有下游层

让我们再看一次我们曾经使用的 Dockerfile...

```
# syntax=docker/dockerfile:1
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

回到映像历史记录输出，我们看到 Dockerfile 中的每个命令都成为映像中的新层。 您可能还记得，当我们对映像进行更改时，必须重新安装纱线依赖项。有没有 如何解决这个问题？每次构建时都围绕相同的依赖项发布没有多大意义，对吧？

为了解决这个问题，我们需要重构我们的 Dockerfile 以帮助支持依赖项的缓存。对于基于节点 应用程序，这些依赖项在文件中定义。那么，如果我们先只复制该文件怎么办， 安装依赖项，_然后_复制其他所有内容？然后，我们只在有 对 .有意义？`package.json``package.json`

1.  首先更新要复制的 Dockerfile，安装依赖项，然后将其他所有内容复制到其中。`package.json`
    
    ```
     # syntax=docker/dockerfile:1
     FROM node:18-alpine
     WORKDIR /app
     COPY package.json yarn.lock ./
     RUN yarn install --production
     COPY . .
     CMD ["node", "src/index.js"]
    ```
    
2.  创建一个与 Dockerfile 位于同一文件夹中的文件，其中包含以下内容。`.dockerignore`
    
    ```ignore
     node_modules
    ```
    
    `.dockerignore`文件是有选择地仅复制图像相关文件的简单方法。 您可以[在此处](https://docs.docker.com/engine/reference/builder/#dockerignore-file)阅读有关此内容的更多信息。 在这种情况下，应在第二步中省略该文件夹，否则， 它可能会覆盖由步骤中的命令创建的文件。 有关为什么建议对 Node.js 应用程序和其他最佳实践执行此操作的更多详细信息， 看看他们关于[Dockerizing a Node.js Web应用程序的](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)指南。`node_modules``COPY``RUN`
    
3.  使用 构建新映像。`docker build`
    
    ```
     $ docker build -t getting-started .
    ```
    
    您应该看到这样的输出...
    
    ```
     [+] Building 16.1s (10/10) FINISHED
     => [internal] load build definition from Dockerfile
     => => transferring dockerfile: 175B
     => [internal] load .dockerignore
     => => transferring context: 2B
     => [internal] load metadata for docker.io/library/node:18-alpine
     => [internal] load build context
     => => transferring context: 53.37MB
     => [1/5] FROM docker.io/library/node:18-alpine
     => CACHED [2/5] WORKDIR /app
     => [3/5] COPY package.json yarn.lock ./
     => [4/5] RUN yarn install --production
     => [5/5] COPY . .
     => exporting to image
     => => exporting layers
     => => writing image     sha256:d6f819013566c54c50124ed94d5e66c452325327217f4f04399b45f94e37d25
     => => naming to docker.io/library/getting-started
    ```
    
    您将看到所有图层均已重新构建。非常好，因为我们对 Dockerfile 进行了相当多的更改。
    
4.  现在，对文件进行更改（例如更改为“真棒待办事项应用程序”）。`src/static/index.html``<title>`
    
5.  现在再次使用 构建 Docker 映像。这一次，您的输出应该看起来有点不同。`docker build -t getting-started .`
    
    ```
     [+] Building 1.2s (10/10) FINISHED
     => [internal] load build definition from Dockerfile
     => => transferring dockerfile: 37B
     => [internal] load .dockerignore
     => => transferring context: 2B
     => [internal] load metadata for docker.io/library/node:18-alpine
     => [internal] load build context
     => => transferring context: 450.43kB
     => [1/5] FROM docker.io/library/node:18-alpine
     => CACHED [2/5] WORKDIR /app
     => CACHED [3/5] COPY package.json yarn.lock ./
     => CACHED [4/5] RUN yarn install --production
     => [5/5] COPY . .
     => exporting to image
     => => exporting layers
     => => writing image     sha256:91790c87bcb096a83c2bd4eb512bc8b134c757cda0bdee4038187f98148e2eda
     => => naming to docker.io/library/getting-started
    ```
    
    首先，您应该注意到构建速度要快得多！而且，您将看到有几个步骤正在使用先前缓存的图层。所以，万岁！我们正在使用构建缓存。推送和拉取此映像并对其进行更新 也会快得多。万岁！

### 多阶段构建
虽然我们不会在本教程中深入探讨，但多阶段构建是一个非常强大的功能。 工具，以帮助使用多个阶段创建映像。它们有几个优点：

-   将构建时依赖项与运行时依赖项分开
-   通过_仅_提供应用运行所需的内容来减小整体图像大小

#### Maven/Tomcat示例

在构建基于 Java 的应用程序时，需要使用 JDK 将源代码编译为 Java 字节码。然而 生产中不需要 JDK。此外，您可能正在使用 Maven 或 Gradle 等工具来帮助构建应用程序。 在我们的最终图像中也不需要这些。多阶段构建会有所帮助。

```
# syntax=docker/dockerfile:1
FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package

FROM tomcat
COPY --from=build /app/target/file.war /usr/local/tomcat/webapps 
```

在此示例中，我们使用一个阶段（称为）使用 Maven 执行实际的 Java 构建。在第二个 阶段（从 开始），我们从阶段复制文件。最终图像只是最后阶段 正在创建（可以使用标志覆盖）。`build``FROM tomcat``build``--target`

#### 反应示例

在构建 React 应用程序时，我们需要一个 Node 环境来编译 JS 代码（通常是 JSX）、SASS 样式表、 以及更多静态 HTML、JS 和 CSS。如果我们不做服务器端渲染，我们甚至不需要 Node 环境 用于我们的生产版本。为什么不在静态nginx容器中交付静态资源？

```
# syntax=docker/dockerfile:1
FROM node:18 AS build
WORKDIR /app
COPY package* yarn.lock ./
RUN yarn install
COPY public ./public
COPY src ./src
RUN yarn run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

在这里，我们使用图像来执行构建（最大化层缓存），然后复制输出 放入 nginx 容器中。很酷吧？`node:18`

## 第十部分： 接下来怎么办？

虽然我们已经完成了入门指南，但还有很多关于容器的知识需要学习！ 我们不会在这里深入探讨，但接下来还有其他一些领域要看！

### 容器编排

在生产中运行容器非常困难。您不想登录到计算机并简单地运行 or .为什么不呢？那么，如果容器死了怎么办？你怎么做 跨多台计算机进行扩展？容器编排解决了这个问题。像Kubernetes这样的工具， Swarm、Nomad 和 ECS 都有助于解决这个问题，但方式略有不同。`docker run``docker-compose up`

一般的想法是，你有接收**预期状态**的“经理”。此状态可能是 “我想运行我的 Web 应用的两个实例并公开端口 80。”然后，经理们查看所有 群集中的计算机，并将工作委派给“辅助角色”节点。经理监视变化（例如 容器退出），然后努力使实际状态反映预期**状态**。

### 云原生计算基金会项目

CNCF是各种开源项目的供应商中立之家，包括Kubernetes，Prometheus， 特使，林克德，NATS，等等！您可以在此处查看[毕业和孵化的项目](https://www.cncf.io/projects/)，并在此处查看整个[CNCF景观](https://landscape.cncf.io/)。有很多项目需要帮助 解决有关监视、日志记录、安全性、映像注册表、消息传递等方面的问题！

因此，如果您不熟悉容器环境和云原生应用程序开发，欢迎您！请 与社区联系，提出问题，并继续学习！我们很高兴有你！

###  入门视频研讨会

我们推荐 DockerCon 2022 的视频研讨会。观看下面的视频或使用链接在特定部分打开视频。

-   [码头工人概述和安装](https://youtu.be/gAGEar5HQoU)
-   [拉取、运行和探索容器](https://youtu.be/gAGEar5HQoU?t=1400)
-   [构建容器镜像](https://youtu.be/gAGEar5HQoU?t=3185)
-   [容器化应用](https://youtu.be/gAGEar5HQoU?t=4683)
-   [连接数据库并设置绑定挂载](https://youtu.be/gAGEar5HQoU?t=6305)
-   [将容器部署到云](https://youtu.be/gAGEar5HQoU?t=8280)

