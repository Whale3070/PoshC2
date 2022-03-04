![PoshC2 Logo](https://raw.githubusercontent.com/nettitude/PoshC2/master/resources/images/PoshC2Logo.png)

![Docker Image CI](https://github.com/nettitude/PoshC2/workflows/Docker%20Image%20CI/badge.svg?branch=master)

PoshC2 是一个代理感知 C2 框架，用于帮助渗透测试人员进行红队、后期利用和横向移动。 

PoshC2 主要用 Python3 编写，遵循模块化格式，使用户能够添加自己的模块和工具，从而实现可扩展且灵活的 C2 框架。 开箱即用的 PoshC2 带有 PowerShell/C# 和 Python2/Python3 植入程序，其中包含用 PowerShell v2 和 v4、C++ 和 C# 源代码编写的有效负载、各种可执行文件、DLL 和原始 shellcode，以及 Python2/Python3 有效负载。 这些在各种设备和操作系统上启用 C2 功能，包括 Windows、*nix 和 OSX。 

PoshC2 的其他显着特点包括： 

* 使用 Docker 提供一致的跨平台支持。 
* 高度可配置的有效负载，包括默认信标时间、抖动、终止日期、用户代理等。
* 大量开箱即用的有效负载经常更新。 
* Shellcode 包含内置 AMSI 绕过和 ETW 修补程序，可实现高成功率和隐蔽性。 
* 自动生成的 Apache Rewrite 规则用于 C2 代理，保护您的 C2 基础架构并保持良好的操作安全性。 
* 一种模块化和可扩展的格式，允许用户创建或编辑可以由 Implants 在内存中运行的 C#、PowerShell 或 Python3 模块。 
* 通过 Pushover 或 Slack 接收成功植入的通知。 
* 全面且维护的上下文帮助和带有上下文自动完成、历史记录和建议的智能提示。 
* 完全加密的通信，即使在通过 HTTP 通信时也能保护 C2 流量的机密性和完整性。 
* 客户端/服务器格式允许多个团队成员使用单个 C2 服务器。 
* 广泛的日志记录。 每个动作和响应都带有时间戳，并与所有相关信息（例如用户、主机、植入物编号等）一起存储在数据库中。除此之外，C2 服务器输出直接记录到单独的文件中。 
* 使用 C# 或 Python2/Python3 不使用 System.Management.Automation.dll 的无 PowerShell 植入程序。 
* 使用 [SharpSocks](https://github.com/nettitude/SharpSocks) 的免费开源 SOCKS 代理 。
* 使用HTTP(S) and SMB 命名管道，用于访问无法访问互联网的内网。

## Documentation

我们维护 PoshC2 文档 在 https://poshc2.readthedocs.io/en/latest/

Find us on #Slack - [poshc2.slack.com](poshc2.slack.com) (to request an invite send an email to labs@nettitude.com)

## Install

您可以直接安装 PoshC2 或使用 Docker 映像，两者的说明如下。 

### Direct install on Kali hosts

An install script is provided for installing PoshC2:

```
*** PoshC2 Install script ***
Usage:
./Install.sh -b <git branch> -p <Directory to clone PoshC2 to>

Defaults are master branch to /opt/PoshC2
```

Elevated privileges are required as the install script performs `apt` updates and installations.

```bash
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash
```

Alternatively the repository can be cloned down and the install script manually run.

```
sudo ./Install.sh
```

You can manually set the PoshC2 installation directory by passing it to the Install.sh script as the `-p` argument. The default is **/opt/PoshC2**:

```
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install.sh | sudo bash -s -- -p /root/PoshC2
```

### Cutting Edge Features

We want to keep the `master` branch stable to ensure that users are able to rely on it when required and for this reason changes can often be feature-complete but not yet present on `master` as they have not been tested completely and signed-off yet.

If you want to look at upcoming features in PoshC2 you can check out the `dev` branch, or any individual feature branches branched off of `dev`.

As features **are** tested before they are merged into `dev` this branch should still be fairly stable and operators can opt in to using this branch or a particular feature branch for their engagement.
This does trade stablity for new features however so do it at your own discretion.

To use `dev` or a feature branch pass the branch name to the Install.sh script as the `-b` argument:

```bash
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/dev/Install.sh | sudo bash -s -- -b dev
```

Note the URL includes the branch name also (here `dev` instead of `master`).

## Installing for Docker

You can also run PoshC2 using Docker, this allows more stable and running and enables PoshC2 to easily run on other operating systems.

The Docker install does not clone PoshC2 as the PoshC2 images on Docker Hub are used, so only a minimal install of some dependencies and scripts are performed.

To start with, install Docker on the host and then add the PoshC2 projects directory to Docker as a shared directory if required for your OS. By default this is **/var/poshc2** on *nix and **/private/var/poshc2** on Mac.

### Kali based hosts

Install script:

```
*** PoshC2 Install script for Docker ***
Usage:
./Install-for-Docker.sh -b <git branch>

Default is the master branch
```

Elevated privileges are required as the install script performs script installations.

```bash
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/master/Install-for-Docker.sh | sudo bash
```

To use the `dev` or feature branches with Docker curl down the `Install-for-Docker.sh` on the appropriate branch and pass the branch name as an argument:

```bash
curl -sSL https://raw.githubusercontent.com/nettitude/PoshC2/BRANCHNAME/Install-for-Docker.sh | sudo bash -s -- -b BRANCHNAME
```

### Windows

On Windows, import the PoshC2.psm1 PowerShell module.

```powershell
Import-Module -DisableNameChecking C:\PoshC2\resources\scripts\PoshC2.psm1
posh-project -PoshC2Dir "C:\PoshC2" -LocalPoshC2ProjectDir "C:\PoshC2_Project" -Arg1 "-n" -Arg2 "newproject"
posh-config -PoshC2Dir "C:\PoshC2" -LocalPoshC2ProjectDir "C:\PoshC2_Project"
posh-server -PoshC2Dir "C:\PoshC2" -LocalPoshC2ProjectDir "C:\PoshC2_Project"
posh -PoshC2Dir "C:\PoshC2" -LocalPoshC2ProjectDir "C:\PoshC2_Project" username
```

## Running PoshC2

Create a new project:

```bash
posh-project -n <project-name>
```

Projects can be switched to or listed using this script:

```bash
[*] Usage: posh-project -n <new-project-name>
[*] Usage: posh-project -s <project-to-switch-to>
[*] Usage: posh-project -l (lists projects)
[*] Usage: posh-project -d <project-to-delete>
[*] Usage: posh-project -c (shows current project)

```

Edit the configuration for your project:

```bash
posh-config
```

Launch the PoshC2 server:

```bash
posh-server
```

Alternatively start it as a service:

```bash
posh-service
```

Separately, run the ImplantHandler for interacting with implants:

```bash
posh -u <username>
```

See https://poshc2.readthedocs.io/en/latest/ for full documentation on PoshC2.

### Specifying a Docker tag

If you are using Docker you can specify the Docker image tag to run with the `-t` option to `posh-server` and `posh`.

E.g.

```bash
posh-server -t latest

```

## Updating PoshC2 Installations

**It is not recommended to update PoshC2 during an engagement. Incoming changes may be incompatible with an existing project and can result in erratic behaviour.**

When using a git cloned version of PoshC2 you can update your PoshC2 installation using the following command:

```
*** PoshC2 Update Script ***
Usage:
posh-update -b <git branch>

Default is the master branch
```

## Using older versions

You can use an older version of PoshC2 by referencing the appropriate tag. Note this only works if you have cloned down the repository.
You can list the tags for the repository by issuing:

```bash
git tag --list
```

If you have a local clone of PoshC2 you can change the version that is in use while offline by just checking out the version you want to use:

```bash
git reset --hard <tag name>
```

For example:

```bash
git reset --hard v4.8
```

However note that this will overwrite any local changes to files, such as changes to the configuration files, and you may have to re-run the install script for that version or re-setup the environment appropriately.

## License / Terms of Use

This software should only be used for **authorised** testing activity and not for malicious use.

By downloading this software you are accepting the terms of use and the licensing agreement.
