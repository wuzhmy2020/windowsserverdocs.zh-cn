---
title: 无线访问部署
description: 本主题是 Windows Server 2016 网络指南 "部署基于密码的 802.1 X 身份验证无线访问" 的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ddc5ebd5f2e00251bcd1cdd915702902dcdb14ae
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318096"
---
# <a name="wireless-access-deployment"></a>无线访问部署

>适用于：Windows Server（半年频道）、Windows Server 2016

按照以下步骤部署无线访问：

- [部署和配置无线 Ap](#bkmk_aps)

- [创建无线用户安全组](#bkmk_groups)

- [\(IEEE 802.11\) 策略配置无线网络](#bkmk_policies)

- [配置 NPSs](#bkmk_nps)

- [将新的无线计算机加入到域](#bkmk_domain)

## <a name="deploy-and-configure-wireless-aps"></a><a name="bkmk_aps"></a>部署和配置无线 Ap

按照以下步骤部署和配置无线 Ap：

- [指定无线 AP 通道频率](#bkmk_channel)

- [配置无线 Ap](#bkmk_wirelessaps)

>[!NOTE]
>本指南中的过程不包含有关在其中打开 "**用户帐户控制**" 对话框请求你允许继续操作的情况的说明。 如果在执行本指南中的过程时出现了此对话框，并且该对话框是因响应你的操作而出现的，则请单击 **“继续”** 。

### <a name="specify-wireless-ap-channel-frequencies"></a><a name="bkmk_channel"></a>指定无线 AP 通道频率

当你在一个地理位置部署多个无线 Ap 时，必须配置具有重叠信号的无线 Ap，以使用唯一通道频率来减少无线 Ap 之间的干扰。

你可以使用下列准则来帮助你选择不与无线网络地理位置的其他无线网络冲突的通道频率。

- 如果有其他组织的办公地点接近或与您的组织在同一大楼，则确定这些组织是否拥有任何无线网络。 查明其无线 AP 的位置和分配的通道频率，因为需要为 AP 分配不同的通道频率，需要确定安装 AP 的最佳位置。

- 确定你的组织内相邻地面上的重叠无线信号。 在确定组织外和内的重叠覆盖区之后，为无线 Ap 分配通道频率，以确保为具有重叠覆盖范围的任何两个无线 Ap 分配不同的通道频率。

### <a name="configure-wireless-aps"></a><a name="bkmk_wirelessaps"></a>配置无线 Ap

使用以下信息以及无线 AP 制造商提供的产品文档来配置无线 Ap。

此过程枚举在无线 AP 上经常配置的项目。 项名称可能因品牌和型号而异，可能与以下列表中的不同。 有关具体的详细信息，请参阅你的无线 AP 文档。

#### <a name="to-configure-your-wireless-aps"></a>配置无线 Ap  

- **SSID**。 指定\(\) \(的无线网络的名称，例如 ExampleWLAN\)。 这是播发到无线客户端的名称。

- **加密**。 根据无线客户端计算机网络适配器所支持的版本，指定 WPA2\-Enterprise \(首选\) 或 WPA\-Enterprise 以及 AES \(首选\) 或 TKIP 加密密码。

- **无线 AP IP 地址 \(静态\)** 。 在每个 AP 上，配置属于子网的 DHCP 作用域的排除范围内的唯一静态 IP 地址。 使用 DHCP 从分配中排除的地址可防止 DHCP 服务器将相同的 IP 地址分配给计算机或其他设备。

- **子网掩码**。 将此项配置为与已连接到无线 AP 的 LAN 的子网掩码设置匹配。  

- **DNS 名称**。 某些无线 Ap 可以配置为使用 DNS 名称。 网络上的 DNS 服务可以将 DNS 名称解析为 IP 地址。 在支持此功能的每个无线 AP 上，输入 DNS 解析的唯一名称。  

- **DHCP 服务**。 如果无线 AP 在 DHCP 服务中已建立\-，请将其禁用。  

- **RADIUS 共享机密**。 为每个无线 AP 使用唯一的 RADIUS 共享机密，除非你打算将 Ap 配置为 NPS 中的 RADIUS 客户端。 如果计划按组在 NPS 中配置 Ap，则组的每个成员的共享机密必须相同。 此外，使用的每个共享机密应该是至少22个字符的随机序列，其中混合了大写和小写字母、数字和标点。 若要确保随机性，可以使用随机字符生成器，如在 NPS **Configure 802.1 x**向导中找到的随机字符生成器来创建共享机密。

>[!TIP]
>记录每个无线 AP 的共享机密，并将其存储在安全的位置，例如办公室安全。 在 NPS 中配置 RADIUS 客户端时，必须知道每个无线 AP 的共享机密。  

- **RADIUS 服务器 IP 地址**。 键入运行 NPS 的服务器的 IP 地址。

- **UDP 端口\(s\)** 。 默认情况下，NPS 将 UDP 端口1812和1645用于身份验证消息，并将 UDP 端口1813和1646用于记帐消息。 建议你在你的 Ap 上使用这些相同的 UDP 端口，但如果你有一个有效的理由来使用不同的端口，则请确保你不只使用新的端口号配置 Ap，同时还要重新配置所有 NPSs，以使用与 Ap 相同的端口号。 如果未用相同的 UDP 端口配置 Ap 和 NPSs，则 NPS 将无法接收或处理来自 Ap 的连接请求，并且网络上的所有无线连接尝试都会失败。

- **Vsa**。 某些无线 Ap 需要供应商\-特定属性 \(Vsa\)，以提供完整的无线 AP 功能。 在 NPS 网络策略中添加了 Vsa。

- **DHCP 筛选**。 配置无线 Ap，阻止无线客户端将 IP 数据包从 UDP 端口68发送到网络，如无线 AP 制造商所述。

- **DNS 筛选**。 配置无线 Ap，阻止无线客户端将 IP 数据包从 TCP 或 UDP 端口53发送到网络，如无线 AP 制造商所述。

## <a name="create-security-groups-for-wireless-users"></a>为无线用户创建安全组

按照以下步骤创建一个或多个无线用户安全组，然后将用户添加到相应的无线用户安全组：

- [创建无线用户安全组](#bkmk_groups)

- [将用户添加到无线安全组](#bkmk_addusers)

### <a name="create-a-wireless-users-security-group"></a><a name="bkmk_groups"></a>创建无线用户安全组

你可以使用此过程在 Active Directory 用户和计算机 Microsoft 管理控制台 \(MMC\) snap\-中创建无线安全组。  

“Domain Admins”成员身份或同等身份是执行此过程的最低要求。

#### <a name="to-create-a-wireless-users-security-group"></a>创建无线用户安全组

1. 依次单击 **“开始”** 、 **“管理工具”** 和 **“Active Directory 用户和计算机”** 。 Active Directory 的 "用户和计算机" snap\-打开。 单击域的节点（如果尚未选定）。 例如，如果域为 example.com，请单击 " **example.com**"。

2. 在详细信息窗格中，右键\-单击要在其中添加新组的文件夹 \(例如，右键\-单击 "**用户**\)"，指向 "**新建**"，然后单击 "**组**"。

3. 在 **“新建对象 - 组”** 的 **“组名”** 中，键入新组的名称。 例如，键入 "**无线组**"。

4. 在 "**组作用域**" 中，选择下列选项之一：

    - **本地域**

    - **全局**

    - **全局**

5. 在 **“组类型”** 中，选择 **“安全组”** 。

6. 单击“确定”。

如果需要为无线用户提供多个安全组，请重复这些步骤以创建其他无线用户组。 稍后，你可以在 NPS 中创建单独的网络策略，以将不同的条件和约束应用于每个组，并为它们提供不同的访问权限和连接规则。

### <a name="add-users-to-the-wireless-users-security-group"></a><a name="bkmk_addusers"></a>将用户添加到无线用户安全组

你可以使用此过程将用户、计算机或组添加到 Active Directory 用户和计算机 "中的" 用户和计算机 "Microsoft 管理控制台 \(MMC\)"\-中的 ""。

“Domain Admins”中的成员身份或同等身份是执行此过程的最低要求。

#### <a name="to-add-users-to-the-wireless-security-group"></a>将用户添加到无线安全组

1. 依次单击 **“开始”** 、 **“管理工具”** 和 **“Active Directory 用户和计算机”** 。 将打开“Active Directory 用户和计算机”MMC。 单击域的节点（如果尚未选定）。 例如，如果域为 example.com，请单击 " **example.com**"。

2. 在详细信息窗格中，\-双击包含无线安全组的文件夹。

3. 在详细信息窗格中，右键\-单击 "无线" 安全组，然后单击 "**属性**"。 此时将打开安全组的 "**属性**" 对话框。

4. 在 "**成员**" 选项卡上，单击 "**添加**"，然后完成以下过程之一，以添加计算机或添加用户或组。

##### <a name="to-add-a-user-or-group"></a>添加用户或组

1. 在 "**输入要选择的对象名称**" 中，键入要添加的用户或组的名称，然后单击 **"确定"** 。

2. 若要将组成员身份分配给其他用户或组，请重复此过程的步骤1。  

##### <a name="to-add-a-computer"></a>添加计算机的步骤

1. 单击**对象类型**。 此时将打开 "**对象类型**" 对话框。

2. 在 "**对象类型**" 中，选择 "**计算机**"，然后单击 **"确定"** 。

3. 在 "**输入要选择的对象名称**" 中，键入要添加的计算机的名称，然后单击 **"确定"** 。

4. 若要将组成员身份分配给其他计算机，请重复此过程的步骤 1\-3。

## <a name="configure-wireless-network-ieee-80211-policies"></a><a name="bkmk_policies"></a>\(IEEE 802.11\) 策略配置无线网络

请按照以下步骤配置 \(IEEE 802.11\) 策略组策略扩展中的无线网络：

- [打开或添加并打开组策略对象](#bkmk_opengpme)

- [激活 \(IEEE 802.11\) 策略的默认无线网络](#bkmk_activate)

- [配置新的无线网络策略](#bkmk_policyconfig)

### <a name="open-or-add-and-open-a-group-policy-object"></a><a name="bkmk_opengpme"></a>打开或添加并打开组策略对象

默认情况下，如果安装了 Active Directory 域服务 \(AD DS\) Server 角色并将服务器配置为域控制器，则组策略管理功能安装在运行 Windows Server 2016 的计算机上。 以下过程描述如何在域控制器上打开 GPMC\) 组策略管理控制台 \(。 然后，该过程介绍如何打开现有域\-级别的组策略对象 \(GPO\) 进行编辑，或创建新的域 GPO 并将其打开进行编辑。

“Domain Admins”成员身份或同等身份是执行此过程的最低要求。

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>打开或添加并打开组策略对象

1. 在域控制器上，依次单击 "**开始**"、" **Windows 管理工具**"，然后单击 "**组策略管理**"。 此时会打开组策略管理控制台。  

2. 在左窗格中，双击 "\-"。 例如，double\-单击 "**林： example.com**"。  

3. 在左窗格中，双击 "**域**"，然后双击 "\-单击要为其管理组策略对象的域\-。 例如，\-双击 " **example.com**"。  

4. 执行以下操作之一：

    -   **若要打开现有域\-级别 GPO 进行编辑**，请双击包含要管理的组策略对象的域，右键\-单击要管理的域策略，如 "默认域策略"，然后单击 "**编辑**"。 **组策略管理编辑器**打开。

    -   **若要创建新的组策略对象并打开进行编辑**，请右键\-单击要为其创建新组策略对象的域，然后单击 "**在此域中创建 GPO 并在此处链接"** 。

        在“新建 GPO”中的“名称”内，键入新组策略对象的名称，然后单击“确定”。

        右键\-单击新的组策略对象，然后单击 "**编辑**"。 **组策略管理编辑器**打开。

在下一部分中，你将使用组策略管理编辑器创建无线策略。

### <a name="activate-default-wireless-network-ieee-80211-policies"></a><a name="bkmk_activate"></a>激活 \(IEEE 802.11\) 策略的默认无线网络

此过程介绍如何使用组策略管理编辑器 \(GPME\)激活 \(IEEE 802.11\) 策略的默认无线网络。

>[!NOTE]
>激活**Windows Vista 和更高**版本的无线网络 \(IEEE 802.11\) 策略或**Windows XP**版本后，当你右键\-单击 "**无线网络 \(IEEE 802.11\) 策略**时，版本选项会自动从选项列表中删除。 出现这种情况的原因是，在选择策略版本后，当你选择**无线网络 \(IEEE 802.11\) 策略**"节点时，会将该策略添加到" GPME "的" 详细信息 "窗格中。 此状态将保持不变，除非你删除无线策略版本，此时无线策略版本返回右侧\-单击 "无线网络的菜单" **\(IEEE 802.11\)** GPME 中的 "策略"。 此外，在选择**无线网络 \(IEEE 802.11\) 策略**"节点时，无线策略只会在" GPME 详细信息 "窗格中列出。

“Domain Admins”成员身份或同等身份是执行此过程的最低要求。

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>激活 \(IEEE 802.11\) 策略的默认无线网络  

1. 按照前面的过程操作，**打开或添加并打开组策略对象**以打开 GPME。

2. 在 GPME 的左窗格中，\-双击 "**计算机配置**"，\-双击 "**策略**"，\-双击 " **Windows 设置**"，然后双击 "安全设置"\-单击 "**安全设置**"。

![802.1 x 无线组策略](../../../media/Wireless-GP/Wireless-GP.jpg)

3. 在 "**安全设置**" 中，右键\-单击 "**无线网络 \(IEEE 802.11\) 策略**"，然后单击 "新建**Windows Vista 和更高版本的无线策略**"。 

![802.1 x 无线策略](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. 此时将打开 "**新建无线网络策略属性**" 对话框。 在 "**策略名称**" 中，键入策略的新名称或保留默认名称。 单击 **“确定”** 保存策略。 将激活默认策略，并在 GPME 的详细信息窗格中列出，并在其中显示新名称或默认名称 "**新建无线网络策略**"。

![新建无线网络策略属性](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. 在详细信息窗格中，\-双击 "**新建无线网络策略**" 将其打开。

在下一部分中，你可以执行策略配置、策略处理首选项顺序和网络权限。

### <a name="configure-the-new-wireless-network-policy"></a><a name="bkmk_policyconfig"></a>配置新的无线网络策略

你可以使用本部分中的过程来配置 \(IEEE 802.11\) 策略的无线网络。 此策略可让你配置安全和身份验证设置，管理无线配置文件，并指定未配置为首选网络的无线网络的权限。

- [为 PEAP\-MS\-CHAP v2 配置无线连接配置文件](#bkmk_configureprofile)  

- [设置无线连接配置文件的首选顺序](#bkmk_preferenceorder)  

- [定义网络权限](#bkmk_permissions)  

#### <a name="configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a><a name="bkmk_configureprofile"></a>为 PEAP\-MS\-CHAP v2 配置无线连接配置文件

此过程提供配置 PEAP\-MS\-CHAP v2 无线配置文件所需的步骤。  

若要完成该过程，必须至少具有 **Domain Admins** 的成员资格或同等权限。

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>为 PEAP\-MS\-CHAP v2 配置无线连接配置文件

1. 在 GPME 的 "无线网络属性" 对话框中，在刚刚创建的策略的 "**常规**" 选项卡和 "**描述**" 中，键入策略的简短描述。

2. 若要指定 WLAN 自动配置用于配置无线网络适配器设置，请确保已选中 "**为客户端使用 WINDOWS WLAN 自动配置服务**"。  

3. 在 **"按下列配置文件的顺序连接到可用网络**" 中，单击 "**添加**"，然后选择 "**基础结构**"。 "**新建配置文件属性**" 对话框将打开。

4. 在 "**新建配置文件属性**" 对话框的 "**连接**" 选项卡上的 "**配置文件名称**" 字段中，键入配置文件的新名称。 例如，键入**适用于 Windows 10 的 EXAMPLE.COM WLAN 配置文件**。

5. 在 "**网络名称\(s\) \(ssid"\)** 中，键入与在无线 ap 上配置的 ssid 对应的 ssid，然后单击 "**添加**"。

    如果你的部署使用多个 SSID，并且每个无线 AP 都使用相同的无线安全设置，则重复此步骤来为你希望应用此配置文件的每个无线 AP 添加 SSID。

    如果你的部署使用多个 SSID，并且每个 SSID 的安全设置不匹配，请为使用相同安全设置的每组 SSID 配置单独的配置文件。 例如，如果有一组无线 Ap 配置为使用 WPA2\-Enterprise and AES，而另一组无线 Ap 使用 WPA\-Enterprise 和 TKIP，则为每组无线 Ap 配置一个配置文件。

6. 如果存在默认文本**NEWSSID** ，请将其选中，然后单击 "**删除**"。

7. 如果你已部署了为取消广播信号而配置的无线访问点，则选择“即使网络未进行广播也连接”。

    > [!NOTE]
    > 启用此选项会造成安全风险，因为无线客户端将探测并试图连接至任何无线网络。 默认情况下不启用此设置。  

8. 依次单击“安全” 选项卡、“高级”，然后配置以下内容：

    1. 若要配置高级 802.1X 设置，请在“IEEE 802.1X”中选择“强制高级 802.1X 设置”。

        当强制使用 advanced 802.1 X 设置时，"**最大 Eapol\-启动消息**数"、"**保持时间**"、"**启动时间**" 和 "**验证时间**" 的默认值就足以满足典型的无线部署。 因此，您无需更改默认值，除非您有具体的原因。

    2. 若要启用单一登录，请选择“为此网络启用单一登录”。

    3. “单一登录”中的其余默认值能够满足典型的无线部署。

    4. 在 "**快速漫游**" 中，如果无线 AP 配置为使用预先\-身份验证，请选择 "**此网络使用预\-身份验证**"。

9. 若要指定无线通信满足 FIPS 140\-2 标准，请选择 "**在 fips 140\-2" 认证模式下执行加密**。

10. 单击 **"确定"** 以返回到 "**安全**" 选项卡。在 "**选择此网络的安全方法**" 的 "**身份验证**" 中，如果受无线 AP 和无线客户端网络适配器的支持，请选择 " **WPA2\-Enterprise** "。 否则，请选择 " **WPA\-Enterprise**"。

11. 在 "**加密**" 中，如果受无线 AP 和无线客户端网络适配器的支持，则选择 " **AES-ccmp 报头**"。 如果你使用的是支持 802.11 ac 的访问点和无线网络适配器，请选择 " **GCMP**"。 否则，请选择“TKIP”。

    > [!NOTE]  
    > **身份验证**和**加密**的设置都必须与在无线 ap 上配置的设置匹配。 默认情况下，**身份验证模式**、**最大身份验证失败次数**和**缓存用户信息以便以后连接到此网络**足以满足典型的无线部署。  

12. 在 "**选择网络身份验证方法**" 中，选择 "**受保护的 EAP \(PEAP\)** ，然后单击"**属性**"。 "**受保护的 EAP 属性**" 对话框将打开。

13. 在 "**受保护的 EAP 属性**" 中，确认已选中 **"验证证书" 来验证服务器的标识**。

14. 在 "**受信任的根证书颁发机构**" 中，选择将服务器证书颁发给 NPS \(CA\) 的受信任的根证书颁发机构。

    > [!NOTE]  
    > 此设置将客户端信任的根 CA 限制为所选的 CA。 如果未选择任何受信任的根 Ca，则客户端将信任在其受信任的根证书颁发机构证书存储中列出的所有根 Ca。  

15. 在 "**选择身份验证方法**" 列表中，选择 "**安全密码 \(EAP\-MS\-CHAP v2\)** "。

16. 单击“配置”。 在 " **EAP Eap-mschapv2 属性**" 对话框中，验证 "**自动使用我的 Windows 登录名和密码 \(和域（如果选择了任何\)** ），然后单击 **" 确定 "** 。

17. 若要启用 PEAP 快速重新连接，请确保选择 "**启用快速重新连接**"。

18. 若要在连接尝试时要求服务器加密了 TLV，请选择 "**如果服务器不显示加密绑定的 tlv 则断开**连接"。

19. 若要指定在身份验证的第一阶段中屏蔽用户标识，请选择 "**启用标识隐私**"，然后在文本框中键入匿名标识名称，或将文本框留空。

    > [!本票
    > - 必须使用 NPS**连接请求策略**创建适用于 802.1 x 无线的 nps 策略。 如果使用 NPS**网络策略**创建 nps 策略，则标识隐私将不起作用。
    > - EAP 标识隐私由某些 EAP 方法提供，在这些方法中，空的或匿名身份 \(与实际标识\) 不同，后者是为了响应 EAP 标识请求而发送的。 PEAP 在身份验证过程中发送两次标识。 在第一阶段，以纯文本格式发送标识，此标识用于路由，而不是用于客户端身份验证。 实际标识（用于身份验证）是在身份验证的第二个阶段（在第一个阶段中建立的安全隧道中）发送的。 如果选择了 "**启用标识隐私**" 复选框，则会将该用户名替换为文本框中指定的条目。 例如，假设选择了 "**启用标识隐私**"，并在文本框中**指定了 "标识隐私别名"** 。 对于<strong>jdoe@example.com</strong>实际标识别名的用户，在第一次身份验证阶段发送的标识将更改为<strong>anonymous@example.com</strong>。 不会修改第一个阶段标识的领域部分，因为它用于路由目的。  

20. 单击 **"确定"** 以关闭 "**受保护的 EAP 属性**" 对话框。
21. 单击 **"确定"** 以关闭 "**安全**" 选项卡。
22. 如果要创建其他配置文件，请单击 "**添加**"，然后重复前面的步骤，为想要应用配置文件的无线客户端和网络自定义每个配置文件的不同选择。 添加完配置文件后，单击 **"确定"** 以关闭 "无线网络策略属性" 对话框。

在下一部分中，你可以对策略配置文件进行排序以获得最佳安全性。

#### <a name="set-the-preference-order-for-wireless-connection-profiles"></a><a name="bkmk_preferenceorder"></a>设置无线连接配置文件的首选顺序
如果在无线网络策略中创建了多个无线配置文件，并且想要对配置文件进行排序以获得最佳的效果和安全性，则可以使用此过程。

为了确保无线客户端连接到它们可以支持的最高安全级别，请将最严格的策略置于列表的顶部。

例如，如果有两个配置文件，一个用于支持 WPA2 的客户端，另一个用于支持 WPA 的客户端，请将此 WPA2 配置文件置于列表的上方。 这可确保支持 WPA2 的客户端将使用该方法进行连接，而不是使用较不安全的 WPA。

此过程提供了指定无线连接配置文件用于将域成员无线客户端连接到无线网络的顺序的步骤。

若要完成该过程，必须至少具有 **Domain Admins** 的成员资格或同等权限。

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>设置无线连接配置文件的首选顺序

1. 在 GPME 的 "无线网络属性" 对话框中，单击刚配置的策略，然后单击 "**常规**" 选项卡。

2. 在 "**常规**" 选项卡上的 **"按下列配置文件的顺序连接到可用网络**" 中，选择要在列表中移动的配置文件，然后单击 "上箭头" 按钮或 "向下箭头" 按钮，将配置文件移动到列表中的所需位置。

3.  对于要在列表中移动的每个配置文件，请重复步骤2。  

4.  单击 **"确定"** 保存所有更改。

在下一部分中，你可以定义无线策略的网络权限。

#### <a name="define-network-permissions"></a><a name="bkmk_permissions"></a>定义网络权限
你可以在 "**网络权限**" 选项卡上配置 \(IEEE 802.11\) 策略所应用到的无线网络的域成员的设置。

对于未在 "**无线网络策略属性**" 页的 "**常规**" 选项卡上配置的无线网络，只能应用以下设置：

- 允许或拒绝通过网络类型和服务集标识符指定的特定无线网络连接 \(SSID\)

- 允许或拒绝到即席网络的连接

- 允许或拒绝连接到基础结构网络

- 允许或拒绝用户查看被拒绝访问的网络类型 \(即席或基础结构\)

- 允许或拒绝用户创建适用于所有用户的配置文件

- 用户只能使用组策略配置文件连接到允许的网络

**Domain Admins**中的成员身份或同等身份是完成这些过程所需的最低要求。

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>允许或拒绝与特定无线网络的连接

1. 在 GPME 的 "无线网络属性" 对话框中，单击 "**网络权限**" 选项卡。

2. 在 "**网络权限**" 选项卡上，单击 "**添加**"。 此时将打开 "**新建权限条目**" 对话框。

3. 在 "**新建权限项目**" 对话框的 "**网络名称 \(ssid\)** " 字段中，键入要为其定义权限的网络的网络 SSID。

4.  在 "**网络类型**" 中选择 "**基础结构** **" 或 "临时"** 。

    > [!NOTE]  
    > 如果你不确定广播网络是基础结构网络还是即席网络，则可以配置两个网络权限条目，分别用于每个网络类型。

5. 在 "**权限**" 中，选择 "**允许**" 或 "**拒绝**"。

6. 单击 **"确定**" 以返回到 "**网络权限**" 选项卡。

##### <a name="to-specify-additional-network-permissions-optional"></a>若要指定其他网络权限 \(可选\)

1.  在 "**网络权限**" 选项卡上，配置以下任一或全部：  

    -   若要拒绝域成员访问即席网络，请选择 "**阻止连接到 ad\-hoc 网络**"。

    -   若要拒绝域成员访问基础结构网络，请选择 "**阻止连接到基础结构网络**"。  

    -   若要允许域成员查看网络类型 \(要拒绝其访问的临时或基础结构\)，请选择 "**允许用户查看拒绝的网络**"。

    -   若要允许用户创建应用于所有用户的配置文件，请选择 "**允许每个人创建所有用户配置文件**"。

    -   若要指定用户只能使用组策略配置文件连接到允许的网络，请选择 "**仅对允许的网络使用组策略配置文件**"。

## <a name="configure-your-npss"></a><a name="bkmk_nps"></a>配置 NPSs
按照以下步骤配置 NPSs，为无线访问执行 802.1 X 身份验证：

- [在 Active Directory 域服务中注册 NPS](#bkmk_npsreg)

- [将无线 AP 配置为 NPS RADIUS 客户端](#bkmk_radiusclient)

- [使用向导为 802.1 X 无线创建 NPS 策略](#bkmk_npspolicy)

### <a name="register-nps-in-active-directory-domain-services"></a><a name="bkmk_npsreg"></a>在 Active Directory 域服务中注册 NPS
你可以使用此过程将运行网络策略服务器 \(NPS\) 的服务器注册到 NPS 所属域中的 Active Directory 域服务 \(AD DS\)。 为了使 NPSs 在授权过程中被授予读取用户帐户属性中的拨号\-的权限，必须在 AD DS 中注册每个 NPS。 注册 NPS 会将服务器添加到 AD DS 中的**RAS 和 IAS 服务器**安全组。

>[!NOTE]
>可以在域控制器或专用服务器上安装 NPS。 运行以下 Windows PowerShell 命令以安装 NPS （如果尚未这样做）：
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
若要完成该过程，必须至少具有 **Domain Admins** 的成员资格或同等权限。

#### <a name="to-register-an-nps-in-its-default-domain"></a>在其默认域中注册 NPS

1. 在 NPS 上**服务器管理器**中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 将打开中的 NPS snap\-。

2. 右键\-单击**NPS \(本地\)** ，然后单击 "**在 Active Directory 中注册服务器**"。 打开 **“网络策略服务器”** 对话框。

3. 在 **“网络策略服务器”** 中，单击 **“确定”** ，然后再次单击 **“确定”** 。

### <a name="configure-a-wireless-ap-as-an-nps-radius-client"></a><a name="bkmk_radiusclient"></a>将无线 AP 配置为 NPS RADIUS 客户端
你可以使用此过程将 AP （也称为*网络访问服务器 \(NAS\)* ）配置为用户服务中的远程身份验证拨号\-\(RADIUS\) 客户端，方法是使用中的 NPS snap\-。 

>[!IMPORTANT]
>客户端计算机（例如运行客户端操作系统的无线便携式计算机和其他计算机）不是 RADIUS 客户端。 RADIUS 客户端是网络访问服务器（如无线访问点、802.1 X\-功能交换机、虚拟专用网络 \(VPN\) 服务器和拨号\-服务器），因为它们使用 RADIUS 协议来与 RADIUS 服务器（如 NPSs）进行通信。

若要完成该过程，必须至少具有 **Domain Admins** 的成员资格或同等权限。

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>在 NPS 中将网络访问服务器添加为 RADIUS 客户端

1. 在 NPS 上**服务器管理器**中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 将打开中的 NPS snap\-。

2. 在的 NPS snap\-中，\-双击 " **RADIUS 客户端和服务器**"。 右键\-单击 " **RADIUS 客户端**"，然后单击 "**新建**"。

3. 在 "**新建 Radius 客户端**" 中，验证是否选中了 "**启用此 radius 客户端**" 复选框。

4. 在 "**新建 RADIUS 客户端**" 的 "**友好名称**" 中，键入无线接入点的显示名称。

    例如，如果要添加 \(AP\) 的无线接入点\-01，请键入 " **ap\-01**"。

5. 在 " **\(ip 或 DNS\)的地址**" 中，键入 NAS 的 ip 地址或完全限定的域名 \(FQDN\)。

    如果输入 FQDN，验证名称是否正确并映射到有效的 IP 地址，请单击 "**验证**"，然后在 "**验证地址**" 中的 "**地址**" 字段中单击 "**解析**"。 如果 FQDN 名称映射到有效的 IP 地址，则该 NAS 的 IP 地址将自动显示在 " **ip 地址**" 中。 如果 FQDN 未解析为 IP 地址，你将收到一条消息，指出此主机未知。 如果出现这种情况，请验证你的 AP 名称是否正确，以及 AP 是否已开机并连接到网络。  

    单击 **"确定"** 关闭**验证地址**。  

6. 在 "**新建 RADIUS 客户端**" 的 "**共享密钥**" 中，执行下列操作之一：  

    -   若要手动配置 RADIUS 共享机密，请选择 "**手动**"，然后在 "**共享密钥**" 中，键入还在 NAS 上输入的强密码。 在 "**确认共享机密**" 中重新输入共享机密。  

    -   若要自动生成共享机密，请选中 "**生成**" 复选框，然后单击 "**生成**" 按钮。 保存生成的共享机密，并使用该值来配置 NAS，使其能够与 NPS 通信。  

        >[!IMPORTANT]
        >在 NPS 中为虚拟 AP 输入的 RADIUS 共享机密必须与在实际无线 AP 上配置的 RADIUS 共享机密完全匹配。 如果使用 NPS 选项生成 RADIUS 共享机密，则必须使用 NPS 生成的 RADIUS 共享机密来配置匹配的实际无线 AP。

7. 在 "**新建 RADIUS 客户端**" 的 "**高级**" 选项卡上的 "**供应商名称**" 中，指定 NAS 制造商名称。 如果不确定 NAS 制造商名称，请选择 " **RADIUS 标准**"。

8. 在**其他选项**中，如果你使用的是 EAP 和 PEAP 以外的任何身份验证方法，并且如果你的 NAS 支持使用消息身份验证器属性，则选择 **"访问请求消息必须包含消息\-验证器属性"** 。

9. 单击“确定”。 NAS 出现在 NPS 上配置的 RADIUS 客户端列表中。

### <a name="create-nps-policies-for-8021x-wireless-using-a-wizard"></a><a name="bkmk_npspolicy"></a>使用向导为 802.1 X 无线创建 NPS 策略
你可以使用此过程来创建连接请求策略和网络策略，以将 802.1 X\-功能的无线访问点部署为用户服务中的远程身份验证拨号\-\(RADIUS\) 客户端连接到运行网络策略服务器 \(NPS\)的 RADIUS 服务器。  
运行向导后，将创建下列策略：

- 一个连接请求策略

- 一个网络策略

>[!NOTE]
>每次需要为 802.1 X 身份验证访问创建新策略时，可以运行新的 IEEE 802.1 X 安全有线和无线连接向导。

若要完成该过程，必须至少具有 **Domain Admins** 的成员资格或同等权限。

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>使用向导为 802.1 X 身份验证无线创建策略

1. 打开中的 NPS snap\-。 如果尚未选择，请单击 " **NPS \(本地\)** "。 如果在中运行 NPS MMC 管理单元\-并且想要在远程 NPS 上创建策略，请选择该服务器。

2. 在**入门**的 "**标准配置**" 中，选择 " **RADIUS 服务器" 802.1 x 无线或有线连接**。 文本和文本下面的链接更改为反映您的选择。

3. 单击 "**配置 802.1 x**"。 此时将打开 Configure 802.1 X 向导。

4.  在 "**选择 802.1 x 连接类型**" 向导页上的 " **802.1 x 连接" 类型**中，选择 "**安全无线连接**"，然后在 "**名称**" 中键入策略的名称，或保留默认名称 "**安全无线连接**"。 单击 **“下一步”** 。

5.  在 "**指定 802.1 x 交换机**" 向导页上的 " **radius 客户端**" 中，显示了在 NPS snap\-中添加为 RADIUS 客户端的所有 802.1 x 开关和无线访问点。 执行以下任意操作：

    -   若要添加其他网络访问服务器 \(Nas\)，如无线 Ap，请在**RADIUS 客户**端中，单击 "**添加**"，然后在 "**新建 RADIUS 客户端**" 中，输入以下信息：**友好名称**、**地址 \(IP 或 DNS\)** 以及**共享机密**。

    -   若要修改任何 NAS 的设置，请在 " **RADIUS 客户端**" 中选择要修改其设置的 AP，然后单击 "**编辑**"。 根据需要修改设置。

    -   若要从列表中删除 NAS，请在**RADIUS 客户端**中，选择 NAS，然后单击 "**删除**"。

        >[!WARNING]
        >从**配置 802.1 x**向导中删除 RADIUS 客户端将从 NPS 配置中删除该客户端。 在的 "**配置 802.1 x** " 向导中对 radius 客户端进行的所有添加、修改和删除操作都将\-反映在中的 " **radius 客户端**" 节点 \/ 下的 "radius 客户端" 节点中的 "Radius 客户端**和服务器** **" 下。** 例如，如果使用向导删除 802.1 X 开关，则还会从中的 NPS snap\-中删除此开关。

6. 单击 **“下一步”** 。 在 "**配置身份验证方法**向导" 页上，在 "**基于访问和网络配置的方法\)\(** " 中，选择 " **MICROSOFT：受保护的 EAP \(PEAP\)** "，然后单击 "**配置**"。

    >[!TIP]
    >如果您收到一条错误消息，指出找不到与身份验证方法一起使用的证书，并且已将 Active Directory 证书服务配置为向网络上的 RAS 和 IAS 服务器自动颁发证书，请首先确保已遵循在 Active Directory 域服务中注册 NPS 的步骤，然后使用以下步骤更新组策略：单击 "**开始**"，单击 " **Windows 系统**"，单击 "**运行**"，然后在 "**打开**" 中键入**gpupdate**，然后按 ENTER。 如果命令返回结果，指示用户和计算机组策略已成功更新，请再次选择 " **Microsoft：受保护的 EAP \(PEAP\)** ，然后单击"**配置**"。
    >
    >如果刷新后组策略继续收到指出无法与身份验证方法一起使用的错误消息，则表示证书未显示，因为它不满足核心网络助理指南：为[802.1 x 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)中所述的最低服务器证书要求。 如果发生这种情况，则必须中止 NPS 配置，撤消颁发给 NPS\(的证书\)，然后按照说明使用服务器证书部署指南来配置新证书。

7.  在 "**编辑受保护的 EAP 属性**" 向导页上的 "**颁发的证书**" 中，确保选择了正确的 NPS 证书，然后执行以下操作：

    >[!NOTE]
    >验证颁发者中所选证书的**颁发者**中的值是否**正确。** 例如，在域 contoso.com 中名为 corp\DC1 的 Active Directory CA 颁发的证书的预期颁发者 \(AD CS\)，它是**公司\-DC1\-CA**。

    -   若要允许用户在访问点之间漫游无线计算机，而无需在每次与新的 AP 关联时重新进行身份验证，请选择 "**启用快速重新连接**"。

    -   若要指定如果 RADIUS 服务器没有显示加密绑定类型\-长度\-值 \(TLV\)，则连接无线客户端将结束网络身份验证过程，请选择 "**断开客户端而不进行加密**。"。  

    -   要修改 EAP 类型的策略设置，请在 " **Eap 类型**" 中单击 "**编辑**"，在 " **eap eap-mschapv2 属性**" 中，根据需要修改设置，然后单击 **"确定"** 。  

8.  单击“确定”。 "编辑受保护的 EAP 属性" 对话框将关闭，并返回到 "**配置 802.1 x**向导"。 单击 **“下一步”** 。

9. 在 "**指定用户组**" 中，单击 "**添加**"，然后在 "Active Directory 用户和计算机" snap\-中键入你为无线客户端配置的安全组的名称。 例如，如果将无线安全组命名为无线组，请键入**无线组**。 单击 **“下一步”** 。

10. 单击 "**配置**" 以根据需要配置 RADIUS 标准属性和供应商\-虚拟 LAN \(VLAN\) 的特定属性，并按无线 AP 硬件供应商提供的文档指定的。 单击 **“下一步”** 。

11. 查看配置摘要详细信息，然后单击 "**完成**"。

此时会创建 NPS 策略，你可以继续将无线计算机加入到域。

## <a name="join-new-wireless-computers-to-the-domain"></a><a name="bkmk_domain"></a>将新的无线计算机加入到域
最简单的方法是将新的无线计算机加入到域中，以物理方式将计算机连接到有线 LAN 的一段 \(在将计算机加入域之前未由 802.1 X 交换机\) 控制的段。 这是最简单的，因为无线组策略设置会自动并立即应用，如果你部署了自己的 PKI，则计算机将接收 CA 证书，并将其放在 "受信任的根证书颁发机构" 证书存储中。允许无线客户端与 CA 颁发的服务器证书信任 NPSs。

同样，将新的无线计算机加入到域后，用户登录到域的首选方法是使用连接到网络的有线连接来执行登录。

### <a name="other-domain-join-methods"></a>其他域联接方法
在不可行的情况下，如果使用有线以太网连接将计算机加入到域，或者用户无法使用有线连接首次登录到域，则必须使用另一种方法。

- **IT 人员计算机配置**。 IT 人员的成员将无线计算机加入到域，并配置单一登录启动无线配置文件。 利用此方法，IT 管理员将无线计算机连接到有线以太网网络并将计算机加入域。 然后，管理员将计算机分发给用户。 当用户在不使用有线连接的情况下启动计算机时，他们为用户登录手动指定的域凭据用于建立与无线网络的连接并登录到域。

有关详细信息，请参阅 "[使用 IT 人员计算机配置方法联接域并登录](#bkmk_itstaff)" 部分

-   **用户的启动无线配置文件配置**。 用户使用启动无线配置文件手动配置无线计算机，并根据从 IT 管理员处获取的说明加入域。 启动无线配置文件允许用户建立无线连接，然后加入域。 将计算机加入域并重新启动计算机之后，用户可以使用无线连接和其域帐户凭据登录到域。

有关详细信息，请参阅[通过使用用户的启动无线配置文件配置联接域并登录](#bkmk_userbootstrap)部分。

### <a name="join-the-domain-and-log-on-by-using-the-it-staff-computer-configuration-method"></a><a name="bkmk_itstaff"></a>加入域并使用 IT 员工计算机配置方法登录
如果域成员用户的域\-加入了域，则无线客户端计算机可以使用临时无线配置文件连接到 802.1 X\-身份验证的无线网络，而无需首先连接到有线 LAN。 此临时无线配置文件称为*启动无线配置文件*。

启动无线配置文件要求用户手动指定其域用户帐户凭据，并且不会在运行网络策略服务器 \(NPS\)的用户服务 \(RADIUS\) 服务器中验证远程身份验证拨号\-的证书。

建立无线连接后，将在无线客户端计算机上应用组策略，并自动发出新的无线配置文件。 新策略使用计算机和用户帐户凭据进行客户端身份验证。 

另外，作为 PEAP\-MS 的一部分，使用新配置文件而不是启动配置文件来\-CHAP v2 相互身份验证，客户端将验证 RADIUS 服务器的凭据。

将计算机加入域后，请使用此过程配置单一登录启动无线配置文件，然后再将无线计算机分发给域\-成员用户。

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>配置单一登录启动无线配置文件

1. 使用本指南中名为[PEAP 的无线连接配置文件\-MS\-CHAP v2](#bkmk_configureprofile)）创建启动配置文件，并使用以下设置：

    - PEAP\-MS\-CHAP v2 身份验证

    - 验证 RADIUS 服务器证书已禁用

    - 已启用单一登录

2. 在创建新的启动配置文件的无线网络策略的属性中，在 "**常规**" 选项卡上，选择 "启动" 配置文件，然后单击 "**导出**"，将配置文件导出到网络共享、USB 闪存驱动器或其他可轻松访问的位置。 该配置文件将作为 * .xml 文件保存到你指定的位置。

3. 将新的无线计算机加入域 \(例如，通过不需要 IEEE 802.1 X authentication\) 的以太网连接，并使用**netsh wlan add profile**命令将启动无线配置文件添加到计算机。

    >[!NOTE]
    >有关详细信息，请参阅用于无线局域网的 Netsh 命令 \(WLAN\) [http：\/\/technet.microsoft.com\/库\/dd744890](https://technet.microsoft.com/library/dd744890)。

4. 使用 "使用运行 Windows 10 的计算机登录到域" 过程将新的无线计算机分发给用户。

当用户启动计算机时，Windows 将提示用户输入其域用户帐户名称和密码。 由于启用了单一登录，因此计算机使用域用户帐户凭据首先建立与无线网络的连接，然后登录到域。

#### <a name="log-on-to-the-domain-using-computers-running-windows-10"></a><a name="bkmk_w10"></a>使用运行 Windows 10 的计算机登录到域

1. 注销计算机，或重新启动计算机。

2. 按键盘上的任意键，或单击桌面。 此时将显示登录屏幕，其中显示了本地用户帐户名称，名称下面有一个密码输入字段。 不要用本地用户帐户登录。

3. 在屏幕左下角，单击 "**其他用户**"。 此时将显示 "其他用户登录" 屏幕，其中包含两个字段： "用户名" 和 "密码"。 密码字段下面是文本 "**登录到**"，然后是计算机加入的域的名称。 例如，如果你的域的名称为 example.com，则该文本将读取 "**登录到：" 示例**。

4. 在 "**用户名**" 中键入域用户名。

5. 在 **“密码”** 中，键入域密码，然后单击箭头，或按 ENTER 键。

>[!NOTE]
>如果 "**其他用户**" 屏幕不包括文本 "**登录到：** " 和你的域名，你应以 "*域\\用户*" 格式输入你的用户名。 例如，若要使用名为**user\-01**的帐户登录到域 example.com，请键入 " **example\\user\-01**"。

### <a name="join-the-domain-and-log-on-by-using-bootstrap-wireless-profile-configuration-by-users"></a><a name="bkmk_userbootstrap"></a>使用启动无线配置文件配置通过用户加入域并登录
使用此方法，你可以完成常规步骤部分中的步骤，然后向你的域\-成员用户提供有关如何使用启动无线配置文件手动配置无线计算机的说明。 启动无线配置文件允许用户建立无线连接，然后加入域。 将计算机加入域并重新启动后，用户可以通过无线连接登录到域。

#### <a name="general-steps"></a>一般步骤

1. 为用户配置 **"控制面板**" 中的本地计算机管理员帐户。

    >[!IMPORTANT]
    >若要将计算机加入域，用户必须使用本地 Administrator 帐户登录到计算机。 或者，用户在将计算机加入域的过程中，必须提供本地管理员帐户的凭据。 此外，用户必须在域中具有用户要加入计算机的用户帐户。 在将计算机加入域的过程中，系统会提示用户输入 \(用户名和密码\)的域帐户凭据。

2. 按照以下步骤**配置启动无线配置文件**中所述，为域用户提供配置启动无线配置文件的说明。
3. 此外，为用户提供本地计算机凭据 \(用户名和密码\)，以及以*DomainName\\用户名*形式 \(域用户帐户名称和密码\) 的域凭据，以及 "将计算机加入域" 和 "登录到域" 的过程，如 Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)中所述。

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>配置启动无线配置文件

1. 使用网络管理员或 IT 支持专业人员提供的凭据，使用本地计算机的管理员帐户登录计算机。

2. 右键\-单击桌面上的网络图标，然后单击 "**打开网络和共享中心**"。 这将打开“网络和共享中心”。 在 "**更改网络设置**" 中，单击 "**设置新的连接或网络**"。 此时将打开 "**设置连接或网络**" 对话框。

3. 单击 "**手动连接到无线网络**"，然后单击 "**下一步**"。

4. 在 "**手动连接到无线网络**" 的 "**网络名称**" 中，键入 AP 的 SSID 名称。  

5. 在 "**安全类型**" 中，选择管理员提供的设置。

6. 在 "**加密类型**和**安全密钥**" 中，选择或键入管理员提供的设置。

7. 选择 "**自动启动此连接**"，然后单击 "**下一步**"。

8. 若要*成功添加 * * * 网络 SSID*，请单击 "**更改连接设置**"。

9. 单击 "**更改连接设置**"。 此时将打开*网络 SSID*无线网络属性对话框。

10. 单击 "**安全**" 选项卡，然后在 "**选择网络身份验证方法**" 中，选择 "**受保护的 EAP \(PEAP\)** "。

11. 单击 **“设置”** 。 "**受保护的 EAP \(PEAP\)" 属性**"页将打开。

12. 在 "**受保护的 EAP \(PEAP\) 属性**" 页上，确保未选中 "**验证服务器证书**"，单击 **"确定"** 两次，然后单击 "**关闭**"。

13. 然后，Windows 将尝试连接到无线网络。 启动无线配置文件的设置指定必须提供域凭据。 当 Windows 提示你提供帐户名称和密码时，请键入你的域帐户凭据，如下所示：*域名\\用户名*、*域密码*。

##### <a name="to-join-a-computer-to-the-domain"></a>将计算机加入域

1. 使用本地 Administrator 帐户登录到计算机。

2. 在 "搜索" 文本框中，键入**PowerShell**。 在搜索结果中，右键单击 " **Windows PowerShell**"，然后单击 "以**管理员身份运行**"。 Windows PowerShell 将打开，并提供提升的提示。

3. 在 Windows PowerShell 中，键入以下命令，然后按 ENTER。 请确保将变量 DomainName 替换为要加入的域的名称。
    
    添加-计算机 DomainName
    
4. 出现提示时，键入你的域用户名和密码，然后单击 **"确定"** 。
5. 重启计算机。
6. 按照上一部分中的说明[使用运行 Windows 10 的计算机登录到域](#bkmk_w10)。
