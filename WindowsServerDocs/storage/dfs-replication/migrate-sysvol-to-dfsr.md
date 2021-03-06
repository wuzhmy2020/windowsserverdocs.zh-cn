---
title: 将 SYSVOL 复制迁移到 DFS 复制
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: d8d9d47ff8f14ce316d2352729247ab2dcf4acbc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949705"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>将 SYSVOL 复制迁移到 DFS 复制


更新日期：2010 年 8 月 25 日

适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

域控制器使用名为 SYSVOL 的特殊共享文件夹将登录脚本和组策略对象文件复制到其他域控制器。 Windows 2000 Server 和 Windows Server 2003 使用文件复制服务 (FRS) 来复制 SYSVOL，而 Windows Server 2008 在使用 Windows Server 2008 域功能级别的域中时使用较新的 DFS 复制服务，在运行较旧域功能级别的域中使用 FRS。

若要使用 DFS 复制来复制 SYSVOL 文件夹，可以创建使用 Windows Server 2008 域功能级别的新域，也可以使用本文档中讨论的过程升级现有域并将复制迁移到 DFS 复制。

本文档假定你具备 Active Directory 域服务 (ADDS)、FRS 和分布式文件系统复制（DFS 复制）的基本知识。 有关详细信息，请参阅 [Active Directory 域服务概述](https://go.microsoft.com/fwlink/?linkid=147787)、[FRS 概述](https://go.microsoft.com/fwlink/?linkid=121763)或 [DFS 复制概述](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> 若要下载本指南的可打印版本，请转到 <a href="https://go.microsoft.com/fwlink/?linkid=150375">SYSVOL 复制迁移指南：FRS 到 DFS 复制</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>本指南包含的内容

[SYSVOL 迁移概念性信息](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [SYSVOL 迁移状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [SYSVOL 迁移过程概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[SYSVOL 迁移过程](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [迁移到已准备状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [迁移到已重定向状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [迁移到已消除状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[有关 SYSVOL 迁移的疑难解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [有关 SYSVOL 迁移问题的疑难解答](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [将 SYSVOL 迁移回滚到以前的稳定状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[SYSVOL 迁移参考信息](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [支持的 SYSVOL 迁移方案](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [验证 SYSVOL 迁移的状态](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [SYSVOL 迁移工具操作](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>其他参考

[SYSVOL 迁移系列：第 1 部分 - SYSVOL 迁移过程简介](https://go.microsoft.com/fwlink/?linkid=121756)

[SYSVOL 迁移系列：第 2 部分 - Dfsrmig.exe：SYSVOL 迁移工具](https://go.microsoft.com/fwlink/?linkid=121757)

[SYSVOL 迁移系列：第 3 部分 - 迁移到“已准备”状态](https://go.microsoft.com/fwlink/?linkid=121758)

[SYSVOL 迁移系列：第 4 部分 - 迁移到“已重定向”状态](https://go.microsoft.com/fwlink/?linkid=121759)

[SYSVOL 迁移系列：第 5 部分 - 迁移到“已消除”状态](https://go.microsoft.com/fwlink/?linkid=121760)

[Windows Server 2008 中的分布式文件系统分步指南](https://go.microsoft.com/fwlink/?linkid=85231)

[FRS 技术参考](https://go.microsoft.com/fwlink/?linkid=121764)

