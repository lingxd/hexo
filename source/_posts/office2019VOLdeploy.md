---
title: office2019VOL版本安装教程
date: 2019-01-22 15:11:14
categories: 教程
tags: [office2019,win10]

---

本文转载自CSDN[**芸芸众生无名之辈**](https://blog.csdn.net/hatmen2/article/details/84677751)

> **众所周知，Office VOL版本可以连接KMS服务器激活，但是office2019没有镜像可以下载，所以只能依靠Office Deployment Tool来进行操作。注：Office2019 Retail零售版有官方镜像可以下载。**

### 下载旧版本的Office

> [office官方卸载工具](https://support.office.com/zh-cn/article/%E4%BB%8E-pc-%E5%8D%B8%E8%BD%BD-office-9dd49b83-264a-477a-8fcc-2fdf5dbf61d8?ui=zh-CN&rs=zh-CN&ad=CN)

### 下载工具

>1. 去官网下载[Office Deployment Tool (ODT)](https://www.microsoft.com/en-us/download/details.aspx?id=49117
)
>![](/images/blog/office/deploy.png)
>2. 下载完成之后运行，执行后，会输出4个文件
>3. 输出的文件，只要是 *.xml 文件都可以删除，留下**setup.exe**文件

### 创建配置文件

> 在线配置xml文件[点这里](https://config.office.com/),根据提示操作，完成后点击【导出】，最终会下载一个XML文件。每个人的选择不同，得出来的**configuration.xml**，以下是笔者的configuration.xml，以供参考。  


```xml
<Configuration ID="87024f0a-c3dc-4626-b8d5-32cbdef9375d">
  <Add OfficeClientEdition="64" Channel="PerpetualVL2019" ForceUpgrade="TRUE">
    <Product ID="ProPlus2019Volume">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
    </Product>
    <Product ID="VisioPro2019Volume">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
    </Product>
    <Product ID="ProjectPro2019Volume">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
    </Product>
  </Add>
  <Property Name="SharedComputerLicensing" Value="0" />
  <Property Name="PinIconsToTaskbar" Value="TRUE" />
  <Property Name="SCLCacheOverride" Value="0" />
  <Updates Enabled="TRUE" />
  <RemoveMSI All="TRUE" />
</Configuration>
```

### 下载&安装
>将**setup.exe**文件和**configuration.xml**两个文件放在同一个文件夹下
>>**打开命令提示符，用管理员权限运行**

```bat
  setup /download configuration.xml
```

>默默等待下载完成，完成之后命令行就继续输入

```bat
  setup /configure configuration.xml
```

### 激活

随便打开一个 Office 输入 Gvlk ，[详情参考](https://docs.microsoft.com)
**随后使用管理员身份执行命令提示符**

```bat
  cd /d "%ProgramFiles%\Microsoft Office\Office16" #进入office的安装目录，目录可通过开始菜单或桌面的快捷方式，右键，“打开文件所在位置”，找到。
  cscript ospp.vbs /sethst:kms.address.org #设置KMS服务器
  cscript ospp.vbs /act #立即激活
  cscript ospp.vbs /dstatus #查看激活状态
```

***kms.address.org*** 替换成真正的KMS服务器即可

### 微软官方安装视频教程

> [微软帮助与支持：Office 2019 部署安装指南](https://v.youku.com/v_show/id_XMzk0MTkwMzQzMg==.html)
