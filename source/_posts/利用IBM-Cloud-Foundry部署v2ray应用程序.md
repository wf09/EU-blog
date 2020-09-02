---
title: 利用IBM Cloud Foundry部署v2ray应用程序
date: 2020/9/1 10:56:00
tags:
  - IBM Cloud 
  - 代理
abbrlink: h489d762
cover: https://ftp.fly97.cn/image/ibm-cloud.png
---

Cloud Foundry提供了云、开发者框架和应用服务的选择，可以更快、更容易的构建、测试、发布和大规模部署应用程序。它是一个开源项目，可通过各种私有云发行版和公有云实例获得。

通过注册IBM Cloud Lite账户，无需信用卡，可免费使用IBM提供的Cloud Foundry容器服务。

### 准备工作

1. IBM Cloud Lite账户
2. Windows电脑或者Linux服务器一台

### 安装依赖

#### Windows：

下载 https://github.com/cloudfoundry/cli/releases/tag/v6.52.0 到任意目录下，打开cmd终端，在该目录下执行

```powershell
cf -v
```

返回信息如图所示

![](https://ftp.fly97.cn/image/20200831215017.png)

#### Linux：

使用root权限执行以下命令

```bash
wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add -
echo "deb https://packages.cloudfoundry.org/debian stable main" | sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list
sudo apt-get update
sudo apt-get install cf-cli
```

安装完毕后在任意目录下执行以下命令

```bash
cf -v
```

返回信息如图所示

![](https://ftp.fly97.cn/image/20200831215456.png)

### 文件准备

#### clone以下仓库

```
git clone https://github.com/wf09/v2ray-cloudfoundry.git
```

#### 修改配置文件

注意name命名有可能重复，尽量选择独一无二的名称。

**./manifest.yml**

```yaml
applications:
- path: .
  name: <APP_NAME>
  random-route: false
  buildpack: go_buildpack
  memory: 256M
```

**./v2ray/config.json**

根据自己喜好修改传输方式和加密方式，注意IP需要监听在**0.0.0.0**，端口需要监听在**8080**端口。

### 部署到ibm

#### 登录到ibm

命令行中输入以下命令

```sh
cf login -a https://api.us-south.cf.cloud.ibm.com
```

依次输入账号密码，最后敲回车

![](https://ftp.fly97.cn/image/20200901102745.png)

#### 开始部署

输入以下命令

```bash
cf push
```

开始部署

![](https://ftp.fly97.cn/image/20200901103010.png)

等待部署完毕

![](https://ftp.fly97.cn/image/20200901103054.png)

若部署过程中出现错误可以输入以下命令查看运行日志

```
cf logs <APP_NAME> --recent
```

![](https://ftp.fly97.cn/image/20200901103521.png)

### 定时重启

Lite计划中，若十天不更新代码，则容器会自动关机。可以利用Github Actions定时重启容器。具体请参考以下以下项目。

https://github.com/wf09/IBMWorkflow

全文完。

