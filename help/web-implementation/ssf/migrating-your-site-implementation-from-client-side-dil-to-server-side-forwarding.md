---
title: 将网站的Audience Manager实施从客户端DIL迁移到服务器端转发
description: 了解如何将网站的Audience Manager(AAM)实施从客户端DIL迁移到服务器端转发。 如果您同时具有AAM和Adobe Analytics，并且使用DIL(Data Integration Library)代码将页面中的点击发送到AAM，同时将页面中的点击发送到Adobe Analytics，则本教程将适用。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# 将网站的Audience Manager实施从客户端DIL迁移到服务器端转发 {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同时具有Adobe Audience Manager (AAM)和Adobe Analytics AAM，并且当前正在使用DIL([!DNL Data Integration Library])代码，还可以将点击从页面发送到Adobe Analytics。 由于您同时拥有这两种解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循启用服务器端转发的最佳实践，该实践将启用 [!DNL Analytics] 数据收集服务器将网站分析数据实时转发到Audience Manager，而不是让客户端代码将额外的点击从页面发送到AAM。 本教程将指导您完成从旧版客户端DIL实现切换到新版服务器端转发方法的步骤。

## 客户端(DIL)与服务器端 {#client-side-dil-vs-server-side}

在比较和对比将Adobe Analytics数据获取到AAM的这两种方法时，可能首先需要在以下图像中显示差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### 客户端DIL实施 {#client-side-dil-implementation}

如果使用此方法将Adobe Analytics数据导入AAM，则您有两个来自网页的点击：一个转到 [!DNL Analytics]，并且一个转到AAM(在复制了 [!DNL Analytics] 数据。 [!UICONTROL Segments] 从AAM返回到页面，可在其中用于个性化等。 这被视为旧版实施，不再建议使用。

除了这并非遵循最佳实践之外，使用此方法的缺点包括：

* 来自页面的两个点击，而不是仅一个点击
* 要将AAM受众实时共享到，需要服务器端转发 [!DNL Analytics]，因此客户端实施不允许此功能（以及将来可能推出的其他功能）

建议您改用AAM实施的服务器端转发方法。

### 服务器端转发实施 {#server-side-forwarding-implementation}

如上图所示，点击来自访问Adobe Analytics的网页。 [!DNL Analytics] 然后，将该数据实时转发到AAM，并评估访客的AAM特征和 [!UICONTROL segments]，就好像点击直接来自页面一样。

[!UICONTROL Segments] 在同一实时点击中返回到 [!DNL Analytics]，将响应转发到网页以进行个性化等。

迁移到服务器端转发没有时间限制。 Adobe强烈建议任何同时拥有Audience Manager和 [!DNL Analytics] 使用此实施方法。

## 您有两个主要任务 {#you-have-two-main-tasks}

这个网页上有很多信息，当然都很重要。 但是， **所有这一切归结为您需要做的两个主要事情**：

1. 将您的代码从客户端DIL代码更改为服务器端转发代码
1. 将开关翻转到 [!DNL Analytics] [!DNL Admin Console] 开始实际转发数据(根据 [!UICONTROL report suite])

如果跳过其中任一任务，则服务器端转发将无法正常工作。 本文档中添加了步骤和其他数据，以帮助您正确执行这两个步骤以进行设置。

## 实施选项 {#implementation-options}

当您从客户端转发转移到服务器端转发时，您的一项任务是将代码更改为新的服务器端转发代码。 可使用以下选项之一完成此操作：

* Adobe Experience Platform标记 — Adobe为Web资产推荐的实施选项。 您会发现这是一个轻松的任务，因为Platform标记已为您完成了所有艰苦的工作。
* 在页面上 — 您还可以将新的SSF代码直接放入 `doPlugins` 函数中的 `appMeasurement.js` 文件(如果尚未使用AdobeLaunch)
* 其他标记管理器 — 这些标记管理器的处理方式与上一个（在页面上）选项相同，因为您仍会将SSF代码放入 `doPlugins`，无论其他标签管理器存储在何处 [!DNL AppMeasurement] 代码

我们将在 _更新代码_ 部分。

## 实施步骤 {#implementation-steps}

以下步骤描述了实施。

### 步骤0：先决条件：Experience CloudID服务(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

迁移到服务器端转发的主要先决条件是实施Experience CloudID服务。 如果您使用的是Experience Platform Launch，则最轻松地做到这一点，在这种情况下，您只需安装ECID扩展，然后它就会完成其余操作。

如果您使用的是非AdobeTMS，或根本没有TMS，请实施ECID以运行 **早于** 任何其他Adobe解决方案。 请参阅 [ECID文档](https://experienceleague.adobe.com/docs/id-service/using/home.html) 了解更多详细信息。 唯一的其他先决条件与代码版本有关，因此，由于您只需按以下步骤应用代码的最新版本即可，因此您无需担心。

>[!NOTE]
>
>在实施之前，请阅读此完整文档。 下面的“计时”部分包含有关以下内容的重要信息 *时间* 您应该实施每个片段，包括ECID（如果尚未实施）。

### 步骤1：从DIL代码中记录当前使用的选项 {#step-record-currently-used-options-from-dil-code}

当您准备好从客户端DIL代码迁移到服务器端转发时，第一步是识别您在使用DIL代码执行的所有操作，包括自定义设置以及发送到AAM的数据。 需要注意和考虑的事项包括：

* 普通 [!DNL Analytics] 变量，使用 `siteCatalyst.init` DIL模块 — 您无需担心此模块，因为其工作只是发送普通发送 [!DNL Analytics] 变量超过over后，只需启用服务器端转发即可实现此目的。
* 合作伙伴子域 — 在 `DIL.create` 功能，记下 `partner` 参数。 这称为您的“合作伙伴子域”，有时也称为“合作伙伴ID”，在放置新的服务器端转发代码时需要此ID。
* [!DNL Visitor Service Namespace]  — 也称为您的&quot;[!DNL Org ID]“或”[!DNL IMS Org ID]，当您设置新的服务器端转发代码时，也会需要此代码。 记下它。
* containerNSID、uuidCookie和其他高级选项 — 记下您使用的任何其他高级选项，以便您也可以在服务器端转发代码中设置它们。
* 其他页面变量 — 如果从页面将其他变量发送到AAM(除了正常的 [!DNL Analytics] 变量由siteCatalyst.init处理)，您需要记下这些变量，以便它们可以通过服务器端转发发送(破坏警告：通过 [!DNL contextData] 变量)。

### 步骤2：更新代码 {#step-updating-the-code}

In [实施选项](#implementation-options) （上文）提供了多个关于如何以及在何处实施服务器端转发的选项。 为了使本节有效，我们需要将其分成这些部分（其中两个是合并的）。 转到本节最能描述您的需求的方法。

#### Adobe Experience Platform标记 {#launch-by-adobe}

观看以下视频，了解如何在Experience Platform Launch中将实施选项从客户端DIL代码移动到服务器端转发。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### “在页面上”或非Adobe标签管理器 {#on-the-page-or-non-adobe-tag-manager}

请观看以下视频，了解如何在中将实施选项从客户端DIL代码移动到服务器端转发 [!DNL AppMeasurement] 代码，位于文件或非Adobe标签管理系统中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步骤3：启用转发(根据 [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

在本教程中，到目前为止，我们已将所有时间都花在了将代码从客户端DIL代码切换到服务器端转发上。 那没关系，因为这是更困难的部分。 虽然您会看到此部分非常简单，但它与更新代码一样重要。 在本视频中，您将了解如何翻转开关，以便能够将数据从Analytics实际转发到Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如视频中所述，请记住，在Experience Cloud后端完全实施转发功能最多需要4小时。

## 计时 {#timing}

提醒一下，从客户端DIL转移到服务器端转发有两个主要任务：

1. 更新代码
1. 将开关打开 [!DNL Analytics] [!DNL Admin Console]

但问题是，你首先要做的是哪一个？ 这重要吗？ 好吧，对不起，那只有两个问题。 但答案是，视情况而定，是的 *可以* 重要。 这话怎么说来含糊不清？ 我们来解释一下。 但是，首先还有一个问题，如果您是一个拥有众多网站的大型组织，可能会出现这个问题：我是否必须一次性完成所有操作？ 那个比较简单。 不行。 你可以逐一做。

### 再深入一点 {#a-little-deeper-dive}

时间和顺序之所以重要，是因为转发方式 _真的_ 作品，可归纳为以下几个技术事实：

* 如果您实施了Experience CloudID服务(ECID)，并且交换机位于 [!DNL Analytics] [!DNL Admin Console] （“交换机”）已打开，数据将从 [!DNL Analytics] 到AAM，即使您尚未更新代码。
* 如果您未实施ECID，数据将不会转发，即使您已打开了开关并且服务器端转发代码也是如此。
* 服务器端转发代码（无论在Platform标记中还是在页面上）将真正处理响应，并且是完成迁移所必需的。
* 请记住，服务器端转发交换机是由 [!UICONTROL report suite]，但代码由Platform标记中的属性处理，或由 [!DNL AppMeasurement] 文件（如果不使用Platform标记）。

### 最佳实践 {#best-practices}

根据这些技术详细信息，以下是有关何时做什么以及何时做什么的建议：

#### 如果您尚未实施ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 翻转开关 [!DNL Analytics] 每个 [!UICONTROL report suite] 将为服务器端转发启用的规则。

   1. 由于您没有ECID，转发尚未启动。

1. 根据站点，将代码从客户端DIL更新到服务器端转发（这可能在Platform标记中）或页面上，如上面的另一部分所述。

   1. 现在转发流（因为您已添加ECID），您还应该收到针对您的的正确JSON响应 [!DNL Analytics] 信标（有关更多详细信息，请参阅下面的“验证和疑难解答”部分）。

#### 如果您确实实施了ECID {#if-you-do-have-ecid-implemented}

1. 准备并规划，以便您准备好将代码从DIL更新到服务器端转发。 [!UICONTROL report suite] 要为服务器端转发启用的服务：

   1. 翻转开关 [!DNL Analytics] 以启用服务器端转发。

      1. 由于您启用了ECID，因此将开始转发。
   1. 请尽快将您的代码从客户端DIL更新为单端转发（这可以位于Platform标记中或页面上，如上面的另一部分所述）。

      1. 您应会收到针对的正确JSON响应 [!DNL Analytics] 信标(请参见 [验证和故障排除](#validation-and-troubleshooting) 部分（了解更多详细信息）。


>[!NOTE]
>
>执行这两个步骤时，请务必尽可能靠近，因为在上面的步骤1和2之间，您会看到进入AAM的重复数据。 换言之，单端转发将已开始从发送数据 [!DNL Analytics] 到AAM，并且由于DIL代码仍在页面上，因此还会有直接从页面进入AAM的点击，从而使数据翻倍。 一旦您将代码从DIL更新到服务器端转发，这种情况就会缓解。

>[!NOTE]
>
>如果您希望数据差异小而不是重复的数据量小，可以切换上面步骤1和2的顺序。 将代码从DIL移动到服务器端转发会停止数据流入AAM，直到您可以翻转交换机以打开的服务器端转发 [!UICONTROL report suite]. 通常，客户宁愿让数据翻一番，也不愿错过让访客了解特征和 [!UICONTROL segments].

#### 当您拥有多个网站和 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

在前几节中简要地谈到了这个主题，因为主要战略可归纳如下：

迁移一个站点/[!UICONTROL report suite] (或站点组/[!UICONTROL report suites])。

但是，根据以下几种可能的情况，这可能会变得有点棘手：

* 您有一个包含多个不同内容的站点 [!UICONTROL report suites]
* 您有一个 [!UICONTROL report suite] 其中包括多个站点(如全球站点 [!UICONTROL report suite])
* 您使用一个Platform标记属性来覆盖多个网站
* 您拥有适用于不同站点的不同开发团队

因为这些项目，它可能会变得有点复杂。 我能提出的最好的建议是：

* 请花些时间根据上文解释的内容，制定迁移到服务器端转发的策略
* 基于Platform标记中的单个属性(或单个 [!DNL AppMeasurement] 文件)通常映射到1或2个不同的 [!UICONTROL report suites]，您应该能够制定一个计划，逐个处理这些不同的组，将您的企业更新到服务器端转发
* 如果您正在与Adobe咨询团队合作，请与他们讨论您的迁移计划，以便他们可以根据需要提供帮助

## 验证和故障排除 {#validation-and-troubleshooting}

验证服务器端转发是否已启动并正在运行的主要方法是，查看应用程序针对任意Adobe Analytics点击的响应。

如果您没有执行来自的数据的服务器端转发 [!DNL Analytics] 对于Audience Manager，则不会有任何响应 [!DNL Analytics] 信标（除2x2像素以外）。 但是，如果您正在执行服务器端转发，则可以在以下位置验证某些项目： [!DNL Analytics] 请求和响应，让您知道 [!DNL Analytics] 正确与Audience Manager通信、转发点击以及获取响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>请注意虚假的“成功”消息。 如果有响应，且一切似乎运行正常，请确保您拥有 `stuff` 响应中的对象。 如果不能，您可能会看到一条消息，显示 `"status":"SUCCESS"`. 尽管这听起来很不可思议，但实际上却证明SSF没有正常运行。
>
>如果您看到此消息，则表示您已完成Platform标记中的代码更新，或者 [!DNL AppMeasurement]，但是转发位于 [!DNL Analytics] [!DNL Admin Console] 尚未完成。 在这种情况下，您需要验证是否已在 [!DNL Analytics] [!DNL Admin Console] 您的 [!UICONTROL report suite]. 如果您已启用，但尚未满4小时，请耐心等待，因为在后端进行所有必要的更改可能需要这么长时间。


![虚假成功](assets/falsesuccess.png)

有关服务器端转发的更多信息，请参阅 [文档](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).
