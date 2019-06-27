---
title: 管理 bde 状态
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d81808b57b1833ca30b95dc9d4b6aa0b0a4bdbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836558"
---
# <a name="manage-bde-status"></a>管理 bde： 状态



在计算机; 提供有关所有驱动器的以下信息它们是否是受 BitLocker 保护：
-   大小
-   BitLocker 版本
-   转换状态
-   加密的百分比
-   加密方法
-   保护状态
-   锁定状态
-   标识字段
-   密钥保护程序

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive>|表示驱动器号后, 接一个冒号。|
|-protectionaserrorlevel|导致 manage-bde 命令行工具，该卷未受保护; 时发送的返回代码为 0 时受保护卷和 1最常用于的批处理脚本确定驱动器是否受 BitLocker 保护。 此外可以使用 **-p**作为此命令的简化版本。|
|-computername|指定将使用 bde.exe 来修改在其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-状态**命令以显示状态的驱动器 c。
```
manage-bde –status C:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)