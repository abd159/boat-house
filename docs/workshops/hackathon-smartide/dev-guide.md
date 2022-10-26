# IDCF Boat House 快速开发指南（SmartIDE）
## IDCF Boat House 项目架构：
![boathouse-arch](images/devguide-boathouse-arch.png)
## 指南概要
如上Boat House项目架构图所示:
* 基于 Spring Boot 框架开发的 **Product Service** 将为Boat House提供 REST API 数据支持；
* 基于Node Js + Vue 框架开发的后端管理平台 **Management Web** 将为Boat House提供后台数据管理功能。

本指南将以 Product Service 和 Boat House 后端管理平台 Management Web 为例，在接下来的三个章节中介绍如何快速上手进行 管理网站 和 后台服务 的开发，以及跨技术栈/IDE情况下如何进行前后端到端的调试。

## Product Service 快速上手指南

### 简介

Boat House Product Service 是 ：
* Boat House 的产品后台数据服务
* 基于Spring Boot 框架开发的 RESTFUL API
* 向前对 Boat House 各网站提供产品数据接口，向后使用MySql数据库进行数据存储

![](images/devguide-product-service-01.png)

### 开发环境
使用SmartIDE容器化开发环境的方式，通过任何浏览器均可进行开发调试，环境中配置好对应的SDK以及docker等工具，不需要在本地安装SDK，开发工具等，所有的这些开发依赖都已经帮您在容器中配置好，你只需要一键启动开发环境，就可以开始你的开发调试。
### 快速开始
1. 访问SmartIDE网站，完成登录
![](images/devguide-smartide-login.png)

2. 配置工作区策略

通过配置工作区策略，设置工作区的git config、ssh key、工作区密码、工作区用户密码。

进入工作区策略配置：

![](images/devguide-smartide-policy-01.png)

开始工作区策略配置：

点击新增创建工作区策略：

![](images/devguide-smartide-policy-02.png)

- git-config：该配置为工作区git提交时的用户信息设置，系统会根据登录用户信息自动设置

![](images/devguide-smartide-policy-03.png)

- ssh-key：该配置为工作区中生成的ssh秘钥信息，其中的公钥信息可配置到任意仓库ssh公钥中，从而实现通过ssh方式拉取代码创建工作区.系统会自动生成秘钥对，用户也可以将本地秘钥进行粘贴修改。

![](images/devguide-smartide-policy-04.png)

- credentials：密码将作为访问工作区的密码或工作区smartide用户密码

![](images/devguide-smartide-policy-05.png)

下面，我们复制刚刚配置好的ssh-key中的rsa-pub信息到源代码仓库服务中，

![](images/devguide-smartide-policy-06.png)

完成以上的基础配置后，下面我们来创建开发环境。

3. 创建开发工作区

下面，我们来创建实验的开发工作区环境，点击左侧[ 工作区管理 ]，然后点击 [ 新增工作区 ]

![](images/devguide-smartide-workspace-01.png)

进入新增工作区页面后，选择资源以及添加代码库地址：

![](images/devguide-smartide-workspace-02.png)

说明：其中资源为已为大家分配好的K8S集群，代码库地址为fork的代码库地址

工作区创建以后，进入到工作区详情页面，待启动完毕后，进入到工作区详情页面中：

![](images/devguide-smartide-workspace-03.png)

4. 打开工作区进行调试开发

点击工作区详情页中的VSCode图标，打开WebIDE：

![](images/devguide-smartide-vscode-01.png)

WebIDE启动后，工具将自动触发以下动作：
- 执行后端代码的mvn package构建
- 安装需要的java扩展包以及docker扩展
- 执行前端代码的npm install以及build
- 启动项目依赖的mysql数据库，并完成初始化。并且，建立网页版的mysql客户端工具mysqladmin

初始化动作执行完毕后，会识别出JAVA PROJECTS以及MAVEN项目情况：

![](images/devguide-smartide-vscode-02.png)

点击[ JAVA PROJECTS ]中的[重新构建工作空间]按钮，完成工作空间的构建
![](images/devguide-smartide-vscode-03.png)


1. 如果本地没有开发环境，请参考以下连接，一步快速启动开发环境：https://gitee.com/idcf-boat-house/boat-house-backend/blob/master/src/product-service/api/.ide/readme.md

### （模式二）传统开发模式

1. 安装 [Itellij IDEA](https://www.jetbrains.com/idea/)
1. 使用 IDEA 打开 Product Service Api 代码
![](images/devguide-product-service-02.png)
1. 点击 pom.xml 查看外部引用项，并点击加载
![](images/devguide-product-service-03.png)
1. Docker启动MySql本地数据库
    ```bash
    docker pull mysql
    docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=[Your Password] -d mysql
    docker ps
    ```
    ![](images/devguide-product-service-04.png)
    ![](images/devguide-product-service-05.png)
    ![](images/devguide-product-service-06.png)
1. 修改 application.properties 文件数据库连接字符串
![](images/devguide-product-service-07.png)
1. 运行 Product Service 站点
![](images/devguide-product-service-08.png)
1. 浏览器打开 Swagger UI 地址（http://localhost:8080/api/v1.0/swagger-ui.html）
![](images/devguide-product-service-09.png)


## Management Web 快速上手指南

### 简介

Boat House Management Web 是：
* Boat House 的后台管理网站
* 基于 Node JS + Express + Vue 框架开发的网站应用
* 向前给 Boat House 管理者提供管理整个餐厅的功能，向后调用 REST API 使用 统计服务/产品服务/账户服务

![](images/devguide-management-web-01.png)

![](images/devguide-debugging-11.png)

### 开发环境

* Windows/Mac OS
* [VS Code](https://code.visualstudio.com/) IDE
* [nodejs](https://nodejs.org/)

### 快速开始
1. 安装 [VS Code](https://www.jetbrains.com/idea/), [nodejs](https://nodejs.org/)
1. 使用 VS Code 打开 Management Web 代码
![](images/devguide-management-web-02.png)
1. 点击 package.json 查看外部引用项，并运行 npm install 加载
![](images/devguide-management-web-03.png)
1. 修改 server.js 文件product service api 地址
![](images/devguide-management-web-04.png)
1. 运行 npm run build 进行应用打包
![](images/devguide-management-web-05.png)
1. 点击左侧 Debug 工具栏，启动 Debug
![](images/devguide-debugging-08.png)
1. 浏览器打开网站地址（http://localhost:4000）
![](images/devguide-management-web-06.png)

## Product Service & Management Web 跨技术栈/IDE连调指南
### 整体架构图：
![](images/devguide-debugging-arch.png)

### 具体步骤
* **Product Service ： IDEA Debugging Mode**
1. IDEA中打开Product Service api 项目目录
![](images/devguide-debugging-01.png)
1. 加载 Pom 文件中所需要的外部引用包
![](images/devguide-debugging-02.png)
1. 点击右上角 Debug 键
![](images/devguide-debugging-03.png)
1. Debug 已启动，端口号（默认8080）如下
![](images/devguide-debugging-04.png)
1. 打开 Swagger UI(http://localhost:8080/), 确认服务已经可以被访问
![](images/devguide-debugging-05.png)
1. 在要调试的方法上打断点
![](images/devguide-debugging-06.png)

* **Management Web 后端：VS Code Debugging Mode**
1. VS Code中选中项目中的 server.js 文件，修改 product service 地址为 http://localhost:8080
![](images/devguide-debugging-07.png)
1. 点击左侧 Debug 工具栏，启动 Debug
![](images/devguide-debugging-08.png)
1. Debug 已启动，端口号（默认4000）如下
![](images/devguide-debugging-09.png)
1. 在 server.js 要调试的后台函数中打断点
![](images/devguide-debugging-10.png)

* **Management Web 前端：用户浏览器（开发者模式）**
1. 打开 Boat House 后台管理网站
![](images/devguide-debugging-11.png)
1. F12开启浏览器开发者模式，点击Source找到要调试的.vue文件，在要调试的 js 函数中打断点
![](images/devguide-debugging-12.png)
1. 开始连调，浏览器页面触发JS方法，监控 Http Request/Response 流转过程
![](images/devguide-debugging-13.png)
![](images/devguide-debugging-14.png)
![](images/devguide-debugging-15.png)