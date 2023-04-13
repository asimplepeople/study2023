# 概述
使用本节中的资源通过 Docker Desktop 了解 Docker 概念。
# 什么是容器？
容器是代码的独立环境。这意味着容器不了解您的操作系统或文件。它在 Docker Desktop 提供给您的环境中运行。这就是为什么容器通常拥有代码运行所需的一切，直到基本操作系统。您可以使用 Docker Desktop 来管理和浏览您的容器。

继续执行以下步骤，在 Docker Desktop 中查看和浏览实际容器。
### 第一步 设置指南
您需要的第一件事是一个正在运行的容器。对于本指南，请使用预制容器。若要开始，请启动 Docker 桌面，然后在本地终端中运行以下命令：`welcome-to-docker`

```
$ docker run -p 8088:80 -d --name welcome-to-docker docker/welcome-to-docker
```
### 第二步 在Docker桌面查看容器
你刚刚运行了一个容器！打开 Docker Desktop 查看。此容器运行显示 HTML 文本的简单 Web 服务器。处理更复杂的项目时，您将在不同的容器中运行不同的部分。例如，前端、后端和数据库的不同容器。在本指南中，您只有一个简单的前端容器。

![运行入门容器的 Docker 桌面](https://docs.docker.com/get-started/images/getting-started-container.png)
### 第三步 查看前端
![[Pasted image 20230413180107.png]]
### 第四步 浏览容器
![[Pasted image 20230413180658.png]]
### 第五步 停止容器

容器将继续运行，直到您停止它。若要在 Docker Desktop 中停止容器，请转到“容器”选项卡，然后选择**容器**的“**操作**”列中的**“停止**”图标。`welcome-to-docker`

![[Pasted image 20230413174717.png]]
### 深入了解

#### 分解命令 `docker run`

设置容器时，使用了该命令。以下是该命令的不同部分的作用：`docker run`

-   `docker run`：用于运行容器。它至少需要一个参数，该参数是您要运行的图像。在本例中，它是 .`docker/welcome-to-docker`
-   `-p 8088:80`：这让 Docker 知道容器中的端口 80 需要从本地主机上的端口 8088 访问。
-   `-d`：这将在分离或后台运行容器。
-   `—-name welcome-to-docker`：这将设置容器的名称。如果您不这样做，Docker 会为您选择一个随机名称。
## 如何运行容器？

## 运行Docker中心镜像
## 发布您的镜像
### Step1 获取image

在发布映像之前，需要要发布的映像。对于本指南，请使用图像。`welcome-to-docker`

若要获取映像，请使用 Docker Desktop 搜索映像，然后选择“**拉取**”。`welcome-to-docker`

![[Pasted image 20230413201403.png]]

### Step2 登录Docker
若要在 Docker Hub 上公开发布映像，首先需要一个帐户。选择 Docker Desktop 右上角的“登录”以**登录**或在 Docker Hub 上创建新帐户。
![[Pasted image 20230413201341.png]]
### 第三步 重命名镜像

在将映像发布到 Docker Hub 之前，需要重命名它，以便 Docker Hub 知道该映像是你的。在终端中运行以下命令以重命名映像。替换为您的 Docker ID。`docker tag` `YOUR-USERNAME`

```
docker tag docker/welcome-to-docker xiezb/welcome-to-docker
```

第四步 推送镜像到docker hub

在 Docker Desktop 中，转到“映像”选项卡并找到您的**映像**。在“操作”列中，选择“**显示图像操作**”图标，然后选择**“推送到 Hub**”。你的映像将上传到 Docker 中心，并公开供任何人使用。

![[Pasted image 20230413201635.png]]