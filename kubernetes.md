# Kubernetes手册



## 第4章 常用的 kubectl 命令

Kubectl命令行实用程序是一款功能十分强大的工具，在接下来的几章中，我们将使用 kubectl 来创建 Kubenetes 对象， 并与 Kubenetes API 交互。但在此之前，我们先来熟悉一下如何利用基本的 kubectl 命令来操作 Kubernetes 对象。

### 4.1 命名空间

Kubernetes 使用命名空间来组织集群中的对象。你可以把每个命名空间想象成一个文件，其中包含了一组对象。默认情况下，kubectl命令行工具交互的默认命名空间是default。如果你想使用其他命名空间，则可以在kubectl中指定--namespace标志。例如，kubectl --namespace=mystuff 可以操作mystuff命名空间中的对象。如果你想操作所有命名空间（例如，列出集群中的所有Pod），则可以指定 --all-namespaces 标志。


### 4.2 上下文

如果想永久地改变默认命名空间，则可以使用上下文。上下文记录在kubectl配置文件中，一般位于 $HOME$/.kube/config 中。该配置文件还保存了集群所在的位置以及身份验证所需的信息。例如，你可以通过一下命令，创建一个拥有不同默认命名空间的上下文，供 kubectl 命令使用：

```sh
$ kubectl config set-context my-context --namespace=mystuff
```

该命令会创建一个新的上下文，但并不会实际投入使用。如果想使用这个新创建的上下文，则需运行如下命令：

```sh
$ kubectl config use-cotext my-context
```

此外，我们还可以利用上下文来管理多个集群或针对多个用户进行身份认证，方法是在 set-context 命令中指定 --user 或 --cluasters 标志。

### 4.3 查看 Kubernetes API 对象

Kubernetes 中包含的一切都是有 RESTful 资源表示。在本书中，我们称这些资源未 Kubernetes 对象。每个 Kubernetes 对象都拥有唯一的 HTTP 路径。例如，https://your-k8s.com/apo/v1/namespaces/default/pods/my-pod 代表了默认命名空间中一个名叫 my-pod 的 Pod。kubectl 命令可以向这些 URL 发送 HTTP 请求，访问这些路径表示的 Kubernetes 对象。

通过 kubectl 查看 Kubernetes 对象的最基本命令是 get。如果运行 kubectl get <资源名称>，则可以获得当前命名空间中所有资源的清单。如果想获取特定资源，则可以使用kubectl get <资源名称> <对象名称>。

默认情况下，kubectl 会输出方便人类阅读的结果，供我们阅览API服务器的相应，但未了在终端的一行中显示一个对象，这种输出格式会删除对象的许多细节。获取更多信息的一种方法是加入 -o wide b标志，这样会显示更多信息，但会增加一行的长度。如果想看完整的对象，则可以通过-o json 或 -o yaml 标志分别输出 JSON 或 YAML 格式的对象。

一种常见的调整kubectl输出的做法是删除表头，常用于kubectl 与 Unix 管道结合使用的情况
todo

#### 14.1.3 Kubernetes 中的角色和角色绑定
在 Kubernetes 中， 代表角色和角色绑定的资源有两种: 一种仅适用于命名空间 (Role 和 RoleBinding); 另一对使用于整个集群（ClusterRole 和 ClusterRoleBinding）。
下面，我们先来看一看 Role 和 RoleBingding。Role 字段带有命名空间，表示这个命名空间中的能力。你不能将命名空间的角色赋予非命名空间的资源（例如自定义资源定义 CustomResourceDefinition 等），而且将RoleBinding绑定给某个角色只能提供同时包含了这个Role 和RoleDefinition的命名空间内的授权。

举一个具体的例子，下面是一个简单的角色，能够赋予某个身份创建和修改Pod以及服务的能力：
```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-and-services
rules:
- apiGroups: [""]
  resources: ["pods", "services"]
  verbs: ["create", "delete", "get", "list", "patch", "update", "watch"]
```
为理论将这个Role绑定给用户alice，我们需要创建一个RoleBinding,如下所示。这个RoleBinding还会将组mydevs绑定到这个Role：
```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-and-services
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: alice
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: mydevs
roleRef:
- apiGroup: rbac.authorization.k8s.io
  kind: rule
  name: pod-and-services
```
当然，有时你需要创建一个适用于整个集群的角色，或者想要限制对集群级资源的访问。此时，我们可以使用ClusterRole和ClusterRoleBinding资源。这两者与前面命名空间的角色和角色绑定基本相同，只不过作用泛微更大。

**Kubernetes 角色的动词**

角色的定义包含两个方面，即资源（比如Pod等）以及描述可针对改资源执行哪些动作的动词。表14-1列举了Kubernetes RBAC中常用的动词。
表14-1: Kubernetes RBAC中常用的动词
| 动词   | HTTP方法 | 描述                            |
| ------ | -------- | ------------------------------- |
| create | POST     | 创建一个新资源                  |
| delete | DELETE   | 是你出一个已有资源              |
| get    | GET      | 获取一个资源                    |
| list   | GET      | 列出一个资源的集合              |
| patch  | PATCH    | 修改已有资源的某一部分          |
| update | PUT      | 修改已有资源的整个对象          |
| watch  | GET      | 持续监视资源的更新              |
| proxy  | GET      | 通过流式WebSocket代理链接到资源 |

**使用内置的角色**

当然，设计自己的角色可能非常复杂且很耗时。另外，Kubernetes拥有大量众所周知的系统身份（例如调度器等），这些身份拥有一组已知的能力。因此，Kubernetes拥有大量内置的集群角色。你可以通过一下命令查看这些角色：
```sh
$ kubectl get clusterroles
```

尽管这些内置的角色大多数面向的是系统实用程序，但其中有四个是针对一般用户设计的：

- cluster-admin角色：提供整个集群完整的访问权限
- admin角色：提供整个命名空间完整的访问权限
- edit角色：允许用户修改命名空间中的一切
- view角色：提供命名空间中的只读权限

大多数集群都已经设置好了很多ClusterRole绑定，你可以通过kubectl get clusterrolebindings 查看这些绑定。



**内置角色的自动协调**

Kubernetes API 服务器启动时，会自动安装许多默认的ClusterRole,这些角色都定义在API服务器本身的代码中。这意味着，针对集群内置角色做出的修改只是暂时的，一旦API服务器重启（例如升级等），你的修改就会被覆盖。



为了防止这种情况的发生，在实际修改内置角色之前，你需要将 rbac.authorization.kubernetes.io/autoupdate 注释添加到 ClusterRole 资源，并将值设置为false。如果这个注释为false，则API服务器不会覆盖已修改的ClusterRole资源。

🎶在默认情况下，Kubernetes API服务器会安装一个集群角色，允许 system:unauthenticated用户访问API服务器的API发现断点。对于任何可能暴露到恶意环境（例如公共互联网）的集群来说，这都是一个坏主意，这些暴露存在至少一个严重的安全漏洞。因此，如果你在公共互联网或其他恶意环境中运行Kubernetes服务，则应该确保API服务器设置了--anonymous-auth=false标志.

### 14.2 RBAC的管理技巧

管理集群的RBAC即复杂又令人沮丧。更令人担心的是，配置错误的RBAC可能导致安全问题。幸运的是，我们有集中工具和技术可以降低管理RBAC的难度。



#### 14.2.1 使用can-i测试权限

第一个非常使用的工具是kubectl的auth can-i命令。该工具可以测试某个特定用户是否可以执行某个特定操作。你可以使用can-i验证配置设定是否符合集群的配置，也可以在用户提交错误报告时，要求他们使用这个工具验证他们是否有权限访问。



最简单的用法是，通过can-i命令验证一个动词和一个资源。



例如，如下命令可以验证当前kubectl 用户是否被授权创建Pod：

```
$ kubectl auth can-i create pods
```

你还可以使用--subresource 命令行标志来测试日志或端口转发之类的子资源：

```
$ kubectl auth can-i get pods --subresources=logs
```



## 第15章 存储解决方案与Kubernetes的继承




