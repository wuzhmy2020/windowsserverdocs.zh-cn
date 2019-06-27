---
title: 使用 get DriverGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f82969e03b3474cf39afd2ae5c3ef2f9d4f8b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847148"
---
# <a name="using-the-get-drivergroup-command"></a>使用 get DriverGroup 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在服务器上将显示有关驱动程序组的信息。
## <a name="syntax"></a>语法
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定的驱动程序组的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。  如果未指定服务器名称，则使用本地服务器。|
|[/ 显示: {PackageMetaData&#124;筛选器&#124;所有}]|显示指定的组中的所有驱动程序包的元数据。 **PackageMetaData**显示有关驱动程序组的所有筛选器的信息。 **筛选器**显示所有驱动程序包的元数据和组筛选器。|
## <a name="BKMK_examples"></a>示例
若要查看有关驱动程序文件的信息，请键入：
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 get AllDriverGroups 命令](using-the-get-alldrivergroups-command.md)