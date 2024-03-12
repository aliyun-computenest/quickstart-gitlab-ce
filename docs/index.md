# Gitlab 社区版计算巢快速部署

>**免责声明：**本服务由第三方提供，我们尽力确保其安全性、准确性和可靠性，但无法保证其完全免于故障、中断、错误或攻击。因此，本公司在此声明：对于本服务的内容、准确性、完整性、可靠性、适用性以及及时性不作任何陈述、保证或承诺，不对您使用本服务所产生的任何直接或间接的损失或损害承担任何责任；对于您通过本服务访问的第三方网站、应用程序、产品和服务，不对其内容、准确性、完整性、可靠性、适用性以及及时性承担任何责任，您应自行承担使用后果产生的风险和责任；对于因您使用本服务而产生的任何损失、损害，包括但不限于直接损失、间接损失、利润损失、商誉损失、数据损失或其他经济损失，不承担任何责任，即使本公司事先已被告知可能存在此类损失或损害的可能性；我们保留不时修改本声明的权利，因此请您在使用本服务前定期检查本声明。如果您对本声明或本服务存在任何问题或疑问，请联系我们。

## 概述

Gitlab是一个开源的Git代码仓库系统，可以实现自托管的，即用于构建私有的代码托管平台和项目管理系统。系统基于Ruby on Rails开发，速度快、安全稳定。它拥有与Github类似的功能，能够浏览源代码，管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。团队成员可以利用内置的简单聊天程序(Wall)进行交流。它还提供一个代码片段收集功能可以轻松实现代码复用，便于日后有需要的时候进行查找。

## 计费说明

Gitlab 社区版上的费用主要涉及：

- 所选vCPU与内存规格
- 系统盘类型及容量
- 公网带宽

## 部署架构

<img src="1.png" width="1500" height="700" align="bottom"/>

## 参数说明

| 参数组        | 参数项         | 说明                                                                     |
|------------|-------------|------------------------------------------------------------------------|
| 服务实例       | 服务实例名称      | 长度不超过64个字符，必须以英文字母开头，可包含数字、英文字母、短划线（-）和下划线（_）                          |
|            | 地域          | 服务实例部署的地域                                                              |
|            | 付费类型        | 资源的计费类型：按量付费和包年包月                                                      |
| ECS实例配置    | 实例类型        | 可用区下可以使用的实例规格                                                          |
|            | 实例密码        | 长度8-30，必须包含三项（大写字母、小写字母、数字、 ()`~!@#$%^&*-+=&#124;{}[]:;'<>,.?/ 中的特殊符号） |
| 网络配置       | 可用区         | ECS实例所在可用区                                                             |
|            | VPC ID      | 资源所在VPC                                                                |
|            | 交换机ID       | 资源所在交换机                                                                |
| gitlab账号配置 | gitlab管理员密码 | gitlab管理员密码配置                                                          |

## RAM账号所需权限

部署Gitlab 社区版，需要对部分阿里云资源进行访问和创建操作。因此您的账号需要包含如下资源的权限。
  **说明**：当您的账号是RAM账号时，才需要添加此权限。

| 权限策略名称                          | 备注                                 |
|---------------------------------|------------------------------------|
| AliyunECSFullAccess             | 管理云服务器服务（ECS）的权限                   |
| AliyunVPCFullAccess             | 管理专有网络（VPC）的权限                     |
| AliyunROSFullAccess             | 管理资源编排服务（ROS）的权限                   |
| AliyunComputeNestUserFullAccess | 管理计算巢服务（ComputeNest）的用户侧权限         |

## 部署流程

1.访问Gitlab 社区版服务[部署链接](https://computenest.console.aliyun.com/service/instance/create/cn-hangzhou?type=user&ServiceId=service-c6be5c5106944ed2b738&ServiceVersion=6)
，按提示填写部署参数：

![image.png](2.png)

2.参数填写完成后可以看到对应询价明细，确认参数后点击**下一步：确认订单**。

![image.png](3.png)

3.确认订单完成后同意服务协议并点击**立即创建**进入部署阶段。

![image.png](4.png)

4.等待部署完成后进入服务实例管理。

5.在控制台找到GitLab服务链接并访问。

![image.png](5.png)

6.输入配置的管理员邮箱和密码即可登录gitlab后台管理页面。

![image.png](6.png)
![image.png](7.png)

## Git安装并配置

1.安装Git工具。

> sudo yum install git -y

2.生成密钥对文件id_rsa。

> ssh-keygen

生成密钥对的过程中，系统会提示输入密钥对存放目录（默认为当前用户目录下的.ssh/id_rsa，例如/root/.ssh/id_rsa）和密钥对密码，您可以手动输入，也可以按Enter保持默认。 回显信息类似如下所示。

![image.png](9.png)

3.查看并复制公钥文件id_rsa.pub中的内容。

> cat .ssh/id_rsa.pub

![image.png](10.png)

4.在gitlab页面中，单击Add SSH key，将公钥文件id_rsa.pub中的内容粘贴到Key所在的文本框中。

![image.png](11.png)
![image.png](12.png)

5.单击Add key，SSH Key添加完成后，如图所示。

![image.png](13.png)

## 创建项目

1.在GitLab的主页中，单击Create a project。

![image.png](14.png)

2.单击Create blank project，设置Project name和Project URL，然后单击Create project。本文以mywork项目为例进行说明。

![image.png](15.png)

## 使用gitlab

1.配置使用Git仓库的人员姓名和邮箱。

> git config --global user.name "testname" 
> 
> git config --global user.email "abc@example.com" 

2.克隆已创建的项目到本地。

![image.png](16.png)

![image.png](17.png)

3.进入到项目目录。

> cd mywork/ 

4.创建test.sh文件并增加内容‘gitlab-ce_test’。

> vim test.sh

5.将test.sh文件加入到索引中。

> git add test.sh

6.将test.sh提交到本地仓库。

> git commit -m "test.sh"

7.将文件同步到GitLab服务器上。

> git push -u origin main

![image.png](18.png)

8.在网页中查看上传的test.sh文件已经同步到GitLab服务器中。

![image.png](19.png)

## 参考文档

https://help.aliyun.com/zh/ecs/use-cases/deploy-and-use-gitlab#2578472088odq