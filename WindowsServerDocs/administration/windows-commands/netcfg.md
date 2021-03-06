---
title: netcfg
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfbe8cd757f78bfa3e808a9126af7d1698579885
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79320001"
---
# <a name="netcfg"></a>netcfg

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安装 Windows 预安装环境（WinPE），这是用于部署工作站的 Windows 轻型版本。
## <a name="syntax"></a>语法
```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```
### <a name="parameters"></a>参数
|参数|说明|
|-------|--------|
|/v|**在详细（详细）** 模式下运行|
|/e|在安装和卸载过程中使用服务**环境**变量|
|/winpe|为 Windows 预安装环境（WinPE）安装 TCP/IP、NetBIOS 和 Microsoft 客户端|
|/l|提供 INF**位置**|
|/c|提供要安装的组件的**类**;协议、服务或客户端|
|/i|提供组件**ID**|
|/s|提供要**显示**的组件的类型。<br /><br />\ta = 适配器，n = net 组件|
|/b|显示**绑定路径**，后跟包含路径名称的字符串。|
|/?|在命令提示符下显示**帮助**。|

## <a name="BKMK_Examples"></a>示例

使用 c:\oemdir\example.inf 安装协议*示例*：
```
netcfg /l c:\oemdir\example.inf /c p /i example
```
若要安装*MS_Server*服务：
```
netcfg /c s /i MS_Server
```
为 Windows 预安装环境安装 TCP/IP、NetBIOS 和 Microsoft 客户端
```
netcfg /v /winpe
```
若要显示是否安装了组件*MS_IPX* ：
```
netcfg /q MS_IPX
```
卸载组件*MS_IPX*：
```
netcfg /u MS_IPX
```
显示所有已安装的 net 组件：
```
netcfg /s n
```
显示包含*MS_TCPIP*的绑定路径：
```
netcfg /b ms_tcpip
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
