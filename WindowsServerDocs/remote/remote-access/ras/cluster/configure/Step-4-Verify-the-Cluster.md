---
title: 步骤4验证群集
description: 本主题是在 Windows Server 2016 的群集中部署远程访问指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 16a31e5435bcb3f0fa3a332bba0e767beb913c08
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308209"
---
# <a name="step-4-verify-the-cluster"></a>步骤4验证群集

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何验证是否已正确配置 DirectAccess 群集部署。  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>验证通过群集访问内部资源的步骤  
  
1.  将 DirectAccess 客户端计算机连接到公司网络并获取组策略。  
  
2.  将客户端计算机连接到外部网络并尝试访问内部资源。  
  
    你应能够访问所有公司资源。  
  
3.  通过在群集中的每个服务器上关闭，或从外部网络断开连接，而不是其中一个群集服务器来测试连接。 在客户端计算机上，尝试访问公司资源。 在其他群集服务器上重复此测试。  
  
    应该能够通过每个群集服务器访问所有公司资源。  
  


