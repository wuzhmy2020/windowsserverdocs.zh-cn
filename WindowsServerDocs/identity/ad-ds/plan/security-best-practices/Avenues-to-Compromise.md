---
ms.assetid: d7a4d2e1-217d-4ffc-93f0-817149bd9e7f
title: 危及系统安全的途径
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 916bffd4f83e5c72816035542772342010e1a67c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874478"
---
# <a name="avenues-to-compromise"></a>危及系统安全的途径

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

*法律数字 7:最安全的网络是一个管理良好。* - [十个永恒定律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
遇到灾难性入侵事件的组织，评估通常展现出组织具有有限的可见性的实际状态的其 IT 基础结构，这可能显著不同于其"按记录"状态。 这种变化情况产生公开泄漏，通常以小的发现之前破坏已发展到点的攻击者有效地"拥有"环境的风险的环境的安全漏洞。  
  
详细的评估，这些组织的 AD DS 配置、 公钥基础结构 (Pki)、 服务器、 工作站、 应用程序的访问控制列表 (Acl) 和其他技术显示配置错误和漏洞，如果修正，可能已阻止的初始危害。  
  
IT 文档、 流程和过程的分析识别漏洞引入已利用的攻击者能够最终获得用于完全破坏的 Active Directory 林的权限的管理实践中的缺口。 完全受攻击的林是林中的特权的一个攻击者危及不仅各个系统、 应用程序或用户帐户，但提升其访问权限，可以获取级别，它们可以修改或销毁的所有方面。 当 Active Directory 安装被泄漏到，程度上，攻击者可以进行更改，以便维护整个环境中，甚至更糟糕，若要销毁该目录的系统和它所管理的帐户是否存在。  
  
虽然数通常可利用的漏洞，请按照说明中的不是针对 Active Directory 的攻击，它们使攻击者可用于运行 （也称为特权的权限提升的环境中建立成功提升） 攻击和到最终目标和危及 AD DS。  
  
本文档的此部分重点介绍描述攻击者通常使用可以访问基础结构，以及最终启动特权提升攻击的机制。 另请参阅以下各节：  
  
-   [减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)详细的安全配置的 Active Directory 的建议。  
  
-   [监视 Active Directory 破坏符号](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)建议，以帮助检测泄漏  
  
-   [规划泄露](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)高级方法，帮助准备针对从基础结构的攻击 IT 和业务透视  
  
> [!NOTE]  
> 尽管本文档重点介绍 Active Directory 和 Windows 系统的是 AD DS 域的一部分，攻击者很少仅仅关注 Active Directory 和 Windows。 在环境中混合使用操作系统、 目录、 应用程序和数据存储库，是常见查找非 Windows 系统也被破坏。 这是尤其如此，如果系统提供"之间的桥梁"Windows 和非 Windows 环境，如 Windows 和 UNIX 或 Linux 客户端，提供身份验证服务添加到多个操作系统的目录访问的文件服务器或将数据同步跨不同的目录的元目录。  
>   
> 针对 AD DS 是因为它提供到 Windows 系统，但其他客户端的集中式访问权限和配置管理功能。 任何其他目录或提供身份验证和配置管理服务的应用程序可以，并将确定攻击者的目标。 尽管本文档侧重于保护措施，可以减少 Active Directory 安装，包括非 Windows 计算机，每个组织的安全威胁的可能性为还应准备目录、 应用程序或数据存储库对这些系统的攻击。  
  
## <a name="initial-breach-targets"></a>初始违反目标  
没有人有意生成公开要破坏的组织的 IT 基础结构。 第一次构造的 Active Directory 林，通常是原始的和当前。 在年传递和获取新的操作系统和应用程序时，它们添加到该林。 识别的 Active Directory 提供了可管理性优势，因为越来越多的内容添加到目录、 更多的人将其计算机或应用程序集成与 AD DS 和域升级为支持提供的最新功能当前版本的 Windows 操作系统。 但是，什么也随着时间推移，恰好是，即使在添加新的基础结构，可能不好它们最初是维护基础结构的其他部分、 系统和应用程序运行正常并因此不会接收注意和组织开始忘记它们不消除了其旧的基础结构。 根据我们在评估遭到入侵的基础结构，显示较旧、 更大、 更复杂环境，更有可能是有大量实例通常可利用的漏洞。  
  
而不考虑攻击者的动机，大多数信息安全漏洞一次启动一个或两个系统的危害。 这些初始事件或在网络中，入口点通常利用漏洞无法得到了修复，但不是。 [2012年数据违反调查报表 (DBIR)](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)，这是生成由 Verizon 风险团队在与许多国家/地区安全机构和其他公司协作中的年度研究，指出"不是 96%的攻击难度高，"，并且"97%的漏洞已能够避免通过简单或中间的控制。" 这些结果可能会遵循通常可利用的漏洞的直接结果。  
  
### <a name="gaps-in-antivirus-and-antimalware-deployments"></a>在防病毒和反恶意软件部署中的缺口  
*法律数字八个：过时的恶意软件扫描程序在所有才略微优于任何扫描程序。* - [十个永恒定律的安全 （版本 2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
分析组织的防病毒和反恶意软件部署经常会发现大多数工作站都配置了防病毒和反恶意软件已启用的软件和当前是在其中的环境。 例外情况通常是不经常连接到的防病毒和反恶意软件的软件可能很难部署、 配置和更新的企业环境或员工设备的工作站。  
  
服务器填充，但是，通常小于一致地在多个受破坏环境中受保护。 中报告[2012年数据漏洞调查](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)、 94%的所有数据的威胁所涉及的服务器，它表示通过上一年的 18%增加和 69%的攻击整合恶意软件。 在服务器填充不常见查找防病毒和反恶意软件的安装是不一致配置、 已过时，配置不正确，或甚至包括禁用的。 在某些情况下，管理人员通过禁用了防病毒和反恶意软件的软件，但在其他情况下，攻击者后禁用该软件影响的服务器通过其他漏洞。 禁用防病毒和反恶意软件软件后，攻击者则计划在服务器上的恶意软件并专注于跨服务器总体传播泄漏。  
  
非常重要，不仅能以确保您的系统受到保护与当前的全面的恶意软件防护，但还监视系统禁用或删除防病毒和反恶意软件的软件和时自动重新启动保护手动禁用。 尽管没有防病毒和反恶意软件软件可以保证预防和检测所有病毒感染，但正确配置，并且已部署的防病毒和反恶意软件实现可以减少感染的可能性。  
  
### <a name="incomplete-patching"></a>不完整修补  
*定律第三步：如果你不关注的安全修补程序，你的网络不是您用于长时间。* - [十个永恒定律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
Microsoft 在虽然在少数情况下会发布安全更新 （这些也称为是"带外"更新） 的每月安全更新之间的每个月的第二个星期二发布安全公告时认为漏洞会引起给客户系统险情。 小型企业配置其 Windows 计算机使用 Windows 更新管理系统和应用程序的修补或大型组织中用于管理软件等 System Center Configuration Manager (SCCM) 部署修补程序根据详细，层次结构的计划，许多客户相对及时修补其 Windows 基础结构。  
  
但是，一些基础结构仅包含 Windows 计算机和 Microsoft 应用程序，并且在受破坏环境中很常见发现组织的修补程序管理策略包含空白。 在这些环境中的 Windows 系统不一致得到修补。 如果根本个别情况下，修补非 Windows 操作系统。 商业现成 (COTS) 应用程序包含的漏洞的修补程序存在，但尚未应用。 通常使用工厂默认凭据配置网络设备，并没有固件更新年后它们的安装。 应用程序，但通常保留由其供应商不再受支持的操作系统运行，即使它们可以不再进行修补的漏洞对这一事实。 这些修补程序的系统的每个攻击者代表另一个潜在的入口点。  
  
消费化 IT 已引入了其他挑战中该员工拥有的设备用于访问公司拥有的数据，和组织可能有小到无法控制修补和员工的个人设备的配置。 企业级硬件通常附带了企业级配置选项和管理功能，但代价是在单独的自定义项和设备选择更少选项。 专注于员工的硬件提供广泛的制造商、 供应商、 硬件的安全功能、 软件安全功能、 管理功能和配置选项，并且许多企业功能，完全可能不存在。  
  
#### <a name="patch-and-vulnerability-management-software"></a>修补程序和漏洞管理软件  
如果对 Windows 系统和 Microsoft 应用程序的有效的修补程序管理系统，创建未修补的漏洞的攻击面的一部分已得到解决。 但是，除非还保留非 Windows 系统、 非 Microsoft 应用程序、 网络基础结构和员工设备保持最新修补程序和其他修补程序，基础结构保持易受攻击。 在某些情况下，应用程序的供应商可能提供了自动更新功能;其他情况下，可能需要设计一个方法，以定期检索和应用修补程序和其他修补程序。
  
### <a name="outdated-applications-and-operating-systems"></a>过期的应用程序和操作系统  
*"不能指望六年历史的操作系统可防止您受到六个月旧式攻击"。* 信息安全专业人员有 10 多年的体验如何保护企业安装  
  
尽管"获取当前，了解最新"听起来像营销短语、 过期的操作系统和应用程序在许多组织中创建风险 IT 基础结构。 2003 年发布的操作系统可能仍由供应商支持和更新提供的地址漏洞，但该操作系统可能不包含在较新版本的操作系统中添加的安全功能。 过时的系统甚至可以要求的特定 AD DS 的安全配置以支持这些计算机的较小功能削弱。  
  
不能进行重组编写可以使用传统的身份验证协议通过供应商通常不再支持应用程序的应用程序以支持更强的身份验证机制。 但是，组织的 Active Directory 域，仍可能配置为存储 LAN Manager 哈希值或可撤消的加密密码，以支持此类应用程序。 较新的操作系统在引入之前编写的应用程序可能无法正常或根本当前在操作系统上，要求组织维护较旧和较旧的系统，并且在某些情况下，完全不受支持的硬件和软件。  
  
甚至在其中组织具有到 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 更新它们的域控制器的情况下，它是典型若要查找的成员的重要部分服务器填充正在运行 Windows Server 2003，Windows2000 server 或 Windows NT Server 4.0 （这些都是完全不受支持）。 组织维护老化系统的时间越长，越多的功能集之间不一致会增长，并越有可能成为生产系统，将不受支持。 此外，再维护 Active Directory 林，越我们观察到旧系统和应用程序会在升级计划中丢失。 这意味着将一台运行单个应用程序可以引入域或林范围漏洞，因为 Active Directory 配置为支持它的旧协议和身份验证机制。  
  
若要消除旧系统和应用程序，应首先集中在标识和目录，然后确定是否要升级或替换应用程序或主机。 虽然它可能很难找到替代项为其既没有支持也没有的升级路径的高度专业化应用程序，您可能能够利用名为"creative 析构"的概念将旧应用程序替换为新的应用程序提供必要的功能。 [规划泄露](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)本文档后面"规划泄露"中的更深入地介绍。  
  
### <a name="misconfiguration"></a>配置错误  
*定律 4 个数字：它不会更令人永远不会首先保护的计算机上安装的安全修补程序。* - [十个永恒定律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
甚至在此处系统通常保存当前和修补的环境中，我们通常确定存在的空白或操作系统中的错误配置计算机和 Active Directory 上运行的应用程序。 某些配置错误公开仅在本地计算机泄漏，但一台计算机"所有后，"攻击者通常专注于进一步传播该泄漏在其他系统之间并最终传送到 Active Directory。 以下是一些常见领域在其中我们识别带来风险的配置。  
  
#### <a name="in-active-directory"></a>在 Active Directory 中  
最常用攻击者的目标中 Active Directory 的帐户是那些最高特权组，例如，域管理员 (DA)、 企业管理员 (EA) 或活动中的内置管理员 (BA) 组的成员的成员目录。 这些组的成员身份应减小为最少数量的可能的帐户，以便限制这些组的受攻击面。 甚至可以消除这些特权组; 中的"永久"成员资格也就是说，可以实现，可暂时其域和林范围的权限在需要时才填充这些组的设置。 当使用高特权的帐户时，应仅在指定的安全系统，例如域控制器或安全管理主机上使用它们。 中提供的详细的信息来帮助实现这些配置的所有[减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)。  
  
当我们评估 Active Directory 中的最高特权组的成员身份时，我们通常在所有这三个最高特权组中找到过多的成员身份。 在某些情况下，组织有几十个甚至数百个数据组中的帐户。 在其他情况下，组织帐户将直接为内置管理员组，认为组"更少特权"比 DAs 组。 不是这样。 我们经常发现在林根域中，尽管 EA 权限很少是临时需要少量的 EA 组永久成员。 在所有三个组中查找 IT 用户的日常管理帐户也很常见，即使这是一个有效地冗余配置。 如中所述[减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)，无论帐户是永久成员的其中一个部分或全部用户，可以使用帐户侵入，并甚至会破坏 AD DS 环境和系统和由它管理的帐户。 中提供的安全配置和使用的 Active Directory 中的特权帐户的建议[减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)。  
  
#### <a name="on-domain-controllers"></a>域控制器上  
我们评估的域控制器，我们经常发现时找到它们配置和管理与成员服务器没有任何差别。 有时，域控制器运行同一应用程序并不是因为在需要域控制器上，但由于应用程序是标准生成过程的一部分，在成员服务器上安装的实用程序。 这些应用程序可能会对域控制器提供最少功能，但会大幅增加其攻击面通过要求打开端口，创建权限较高的服务帐户，或授予访问权限的用户系统的配置设置人员不应连接到域控制器身份验证和组策略应用程序以外的任何目的。 在一些漏洞，攻击者已使用域控制器不仅要获取访问权限的域控制器，但若要修改或损坏的 AD DS 数据库已安装的工具。  
  
当我们提取域控制器上的 Internet 资源管理器配置设置时，我们发现，具有较高的权限在 Active Directory 中的、 已使用帐户访问 Internet 和 intranet 的域帐户以登录用户控制器。 在某些情况下，若要允许的 Internet 下载内容，在域控制器上 Internet Explorer 设置配置帐户并免费软件实用程序已从 Internet 站点下载并安装在域控制器上。 Internet Explorer 增强安全配置为用户和管理员启用默认情况下，但我们通常观察到，它是已为管理员禁用。 当高特权的帐户访问 Internet，并将内容下载到的任何计算机时，该计算机就会处于严重的风险。 如果计算机是域控制器，则整个 AD DS 安装置于危险。  
  
##### <a name="protecting-domain-controllers"></a>保护域控制器  
域控制器应被视为关键基础结构组件、 更严格地保护和严格比文件、 打印和应用程序服务器配置。 域控制器不应运行不正常的域控制器需要或不保护免受攻击的域控制器的任何软件。 域控制器不应允许访问 Internet，并应配置和实施组策略对象 (Gpo) 的安全设置。 中提供了有关安全的安装、 配置和管理域控制器的详细的建议[针对攻击保护域控制器](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)。  
  
#### <a name="within-the-operating-system"></a>在操作系统中  
*定律第二：如果攻击者能够更改您的计算机上的操作系统，它就不再您的计算机了。* - [十个永恒定律的安全 （版本 2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
尽管某些组织中创建不同类型的服务器的基线配置，并允许有限的自定义项的操作系统安装之后，受破坏环境的分析经常会揭露大量的部署中的服务器特定的方式，并手动、 独立地进行配置。 在安全配置服务器，执行相同的函数的两个服务器之间的配置可能完全不同。 相反，可能会一致地强制执行，但还一致地配置不正确; 服务器的配置基线也就是说，在给定类型的所有服务器上创建相同的漏洞的方式配置服务器。 配置错误包括如禁用安全功能的做法，授予过多的权限和对帐户 （尤其是服务帐户） 的权限，跨系统和允许安装使用的完全相同的本地凭据未经授权的应用程序和创建其自己的漏洞的实用程序。  
  
##### <a name="disabling-security-features"></a>禁用安全功能  
组织有时会禁用带相信 WFAS 很难配置或需要占用大量工作的配置的 Windows 防火墙使用高级安全 (WFAS)。 但是，从开始与 Windows Server 2008 中，当服务器上安装任何角色或功能，默认情况下使用最少特权角色或功能正常工作，所需的配置和 Windows 防火墙将自动配置为支持角色或功能。 通过禁用 WFAS （并不在其原位置使用另一个基于主机的防火墙），组织提高整个 Windows 环境的攻击面。 外围防火墙提供了某种保护以抵御攻击的直接目标环境与 Internet，但提供无保护免受攻击，如利用其他攻击媒介[偷渡式下载](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)攻击或从其他被破坏的系统在 intranet 上发起的攻击。  
  
有时在服务器上禁用用户帐户控制 (UAC) 设置，因为管理人员找到提示具有侵入性。 尽管[Microsoft 支持文章 2526083](https://support.microsoft.com/kb/2526083)介绍 UAC 可能禁用 Windows Server 的方案，除非运行服务器核心安装 （其中 UAC 禁用的设计），不应在服务器上禁用 UAC未经仔细考虑和研究。  
  
在其他情况下，服务器设置配置为不安全的值，因为组织将过时的服务器配置设置应用到新操作系统，如将 Windows Server 2003 基线应用于运行 Server 2012 Windows 的计算机Server 2008 R2 或 Windows Server 2008 中，而无需更改以反映在操作系统中的更改的基线。 而不是部署新操作系统时又旧到新操作系统的服务器基线，查看安全更改和配置设置，以确保实现的设置是适用且适用于新的操作系统。  
  
##### <a name="granting-excessive-privilege"></a>授予过多的权限  
在我们评估几乎每个环境中，额外特权授予在 Windows 系统上的本地和基于域的帐户。 向用户授予他们的工作站，运行使用超出所需正常工作，权限配置的服务的成员服务器上的本地管理员权限和跨服务器总体的本地管理员组包含几十个甚至数百个本地和域帐户。 计算机上只有一个特权帐户受到安全威胁，使得攻击者破坏的每个用户和登录到计算机，并传播到其他系统的危害的 harvest 和利用凭据的服务帐户。  
  
但传递的哈希 (PTH) 和其他凭据盗窃攻击无处不今天，这是因为没有使它变得简单的免费工具，并轻松地提取时攻击者其他特权帐户的凭据已获得管理员的或系统级访问的计算机。 即使没有允许的凭据登录会话中搜集的工具，具有特权访问的计算机的攻击者可以轻松地安装捕获击键、 屏幕截图和剪贴板内容的击键记录器。 具有特权访问的计算机的攻击者可以禁用反恶意软件、 安装 rootkit、 修改受保护的文件，或自动执行攻击，或打开到服务器的计算机上安装恶意软件[偷渡式下载](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)主机。  
  
用于扩展超出一台计算机受到破坏的策略不同，但传播泄漏的关键是获取对其他系统的高特权访问权限。 通过减少任何系统具有特权访问权限的帐户数，您可以降低受攻击面不仅该计算机，但收集有价值的凭据，从计算机的攻击的可能性。  
  
##### <a name="standardizing-local-administrator-credentials"></a>标准化本地管理员凭据  
是否是有价值的 Windows 计算机上的本地管理员帐户重命名，很长时间以来在作为到安全专家之间的辩论。 有关本地管理员帐户实际上重要的是是否它们跨多台计算机配置具有相同的用户名和密码。  
  
如果本地管理员帐户名为为相同的值在服务器之间，分配给该帐户的密码也被配置为相同的值，攻击者可以提取上一台计算机上的管理员或系统级的访问权限的帐户的凭据已获取。 攻击者不需要最初破坏的管理员帐户;他们需要仅要入侵的用户是本地管理员组，或配置为以 localsystem 身份或使用管理员特权运行的服务帐户的成员的帐户。 然后，攻击者可提取管理员帐户的凭据，重播网络登录到网络上其他计算机中的这些凭据。  
  
只要另一台计算机具有本地帐户使用相同的用户名和密码 （或密码哈希） 作为要显示的帐户凭据，登录尝试成功，并且攻击者获得对目标计算机特许访问权限。 在当前版本的 Windows 中，内置管理员帐户是[默认情况下禁用](https://technet.microsoft.com/library/cc753450.aspx)，但在旧版操作系统，默认情况下启用了帐户。  
  
> [!NOTE]  
> 某些组织已有意配置中的理念基础，这提供了"防故障"在所有其他特权的帐户被锁定在系统之外的情况下启用的本地管理员帐户。 但是，即使禁用本地管理员帐户，没有其他帐户可用，可让登录到系统管理员权限的帐户，系统可以引导到安全模式和内置本地管理员可以重新启用帐户，如中所述[Microsoft 支持文章 814777](https://support.microsoft.com/kb/814777)。 此外，如果系统仍然会成功应用 Gpo，可以修改 GPO （临时） 重新启用管理员帐户，或受限制的组可以配置为将基于域的帐户添加到本地管理员组。 可以执行修复，可以再次禁用管理员帐户。 若要有效地防止使用内置本地管理员帐户凭据的横向危害，唯一的用户名和密码必须配置为本地管理员帐户。 若要部署通过 GPO 的本地管理员帐户的唯一密码，请参阅[用于通过 GPO 的内置管理员帐户的密码管理解决方案](https://technet.microsoft.com/mt227395.aspx)technet 上。  
  
##### <a name="permitting-installation-of-unauthorized-applications"></a>允许未经授权的应用程序的安装  
*定律第一部分：如果攻击者能够设法使您在计算机上运行他的程序，它就不再只是您的计算机了。* - [十个永恒定律的安全 （版本 2.0）](https://technet.microsoft.com/security/hh278941.aspx)  
  
组织在服务器之间部署一致的基准设置，是否不应允许的应用程序不是服务器的定义的角色的一部分安装。 通过允许安装软件的不是服务器的指定功能的一部分，服务器公开到无意或恶意的软件的增加服务器的攻击面，引入了应用程序漏洞，或导致的安装系统不稳定。  
  
#### <a name="applications"></a>应用程序  
如前文所述，应用程序通常安装并配置为使用被授予更多的权限比应用程序实际需要的帐户。 在某些情况下，应用程序的文档指定服务帐户必须是服务器的本地管理员组的成员，或必须配置为在本地系统上下文中运行。 这通常是不是因为该应用程序需要这些权限，但应用程序的服务帐户需要因为确定哪些权限和权限要求投入更多时间和精力。 如果应用程序不会安装与所需的应用程序和其配置的功能才能正常的最小特权，系统将公开利用应用程序权限，而无需对操作系统本身的任何攻击的攻击。  
  
### <a name="lack-of-secure-application-development-practices"></a>缺少的安全应用程序开发做法  
存在基础结构以支持业务工作负荷。 这些工作负荷是在自定义应用程序中实现时，务必确保应用程序开发使用安全最佳实践。 企业级事件的根本原因分析经常会发现初始泄漏会受影响通过自定义应用程序尤其是那些是 Internet 面向。 大多数这些妥协的通过的良好已知攻击，例如 SQL 注入 (注 SQLi) 受到安全威胁和跨站点脚本 (XSS) 攻击。  
  
SQL 注入是允许用户输入，若要修改执行传递到数据库的 SQL 语句的应用程序漏洞。 可以通过应用程序中，参数 （如在查询字符串或 cookie） 或其他方法中的字段提供此输入。 此注入的结果是提供给数据库的 SQL 语句是从根本上不同于哪些开发人员的预期。 例如，需要使用用户名/密码组合的评估中的常用查询：  
  
`SELECT userID FROM users WHERE username = 'sUserName' AND password = 'sPassword'`  
  
当收到时数据库服务器时，它指示要在用户表中查找，并返回用户 Id 的任何记录其中的用户名和密码匹配所提供的用户 （可能通过某种类型的一个登录窗体） 的服务器。 自然，开发人员的意图在这种情况下是可以由用户提供正确的用户名和密码仅返回有效记录。 如果任一项不正确，则数据库服务器将不能查找匹配的记录并返回一个空的结果。  
  
当攻击者执行一些如提供其自己代替有效的数据的 SQL 异常时，会出现此问题。 由于 SQL 是解释型上实时数据库服务器，像开发人员必须将其放在自己会处理插入的代码。 例如，如果攻击者输入**管理员**针对的用户 ID 和**xyz**或者**1 = 1**作为密码，是由数据库处理所得到的语句：  
  
`SELECT userID FROM users WHERE username = 'administrator' AND password = 'xyz' OR 1=1`  
  
当数据库服务器通过处理此查询时，表中的所有行将都返回在查询中因为 1 = 1 将计算结果始终为 True，因此如果已知或提供正确的用户名和密码并不重要。 在大多数情况下的最终结果是将用户的数据库; 中的第一个用户以登录用户在大多数情况下，这将是管理用户。  
  
例如，这可用于添加，只需日志记录格式不正确的 SQL 语句上，除了删除或更改数据，或甚至从数据库中删除 (delete) 的整个表。 在其中使用过多的权限组合注 SQLi 最极端的情况下，可以运行操作系统命令可用于创建新用户，若要下载攻击工具，或执行攻击者选择的任何其他操作。  
  
在跨站点脚本、 应用程序的输出中引入了此漏洞。 以开始攻击，攻击者提供给应用程序，格式不正确数据，但这种格式不正确的数据是在受害者的浏览器将运行的脚本代码 （如 JavaScript) 的窗体中。 XSS 漏洞攻击可以允许攻击者的用户的上下文中运行目标应用程序的任何功能启动浏览器。 XSS 攻击通常是通过鼓励用户单击一个链接，连接到该应用程序并运行的攻击代码的网页仿冒电子邮件启动的。  
  
XSS 通常利用了在网上银行和电子商务方案其中攻击者可以进行购买或传输中，当被利用的用户的上下文中。 对于自定义基于 web 的标识管理应用程序上有针对性的攻击，它可以允许攻击者可以创建自己的标识、 修改权限和权限，并会导致系统泄漏。  
  
尽管跨站点脚本和 SQL 注入的全面讨论已超出了本文档的范围[Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page)发布的漏洞的深入讨论了前 10 列表和对策。  
  
而不考虑基础结构安全性方面的投资，效果不佳设计和编写应用程序部署在该基础结构，如果环境是由易受到攻击。 甚至良好保护基础结构通常不能提供有效的对策，这些应用程序的攻击。 设计欠佳的应用程序组合问题，可能需要服务帐户被授予为函数的应用程序过多的权限。  
  
Microsoft 安全开发生命周期 (SDL) 是一组结构过程控件，它们共同以提高安全在要求收集和扩展的应用程序的生命周期，直到已停用的早期阶段开始。 有效的安全控件的此集成不只是关键从安全角度来看，它至关重要，可确保应用程序的安全成本和计划有效。 评估安全问题的应用程序，它实际上是完整的代码时需要组织做出有关应用程序安全性或仅在之前即使在部署应用程序。 组织可以选择部署在生产中，不会产生成本和延迟，应用程序之前解决应用程序缺陷，也可以与已知的安全缺陷，从而使组织破坏的生产环境中部署应用程序。  
  
某些组织将放置在上述每个问题，10,000 美元的生产代码中修复安全问题的完整的成本和没有有效的 SDL 的情况下开发的应用程序可以平均每 100,000 行的代码的 10 个以上高严重级别问题。 在大型应用程序快速升级成本。 与此相反，许多公司在 SDL 的最终代码评审阶段设置每 100,000 行的代码不超过 1 问题的基准，并在生产环境中的高风险应用程序中的零个问题的目标是。  
  
实现 SDL 提高了安全性需求收集在早期阶段包括安全要求和设计的应用程序提供了威胁建模的高风险应用程序;需要有效的培训和开发人员; 的监视并且需要清晰、 一致的代码标准和做法。 SDL 的净效果是同时降低成本进行开发、 部署、 维护和停用应用程序中应用程序安全性的重大改进。 虽然设计的详细的讨论和 SDL 的实现不属于本文档的范围，请参阅[Microsoft 安全开发生命周期](https://www.microsoft.com/security/sdl/default.aspx)详细的指南和信息。  
  

