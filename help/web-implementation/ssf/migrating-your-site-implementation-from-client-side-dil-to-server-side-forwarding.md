---
title: 将您网站的AAM实施从客户端DIL迁移到服务器端转发
description: 如果您同时具有Adobe Audience Manager(AAM)和Adobe Analytics，并且当前使用“DIL”(Data Integration Library)代码从页面向AAM发送点击，同时从页面向Adobe Analytics发送点击，则本教程将适用于您。 由于您有这两个解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循开启“服务器端转发(SSF)”的最佳实践，该实践允许Analytics数据收集服务器将网站分析数据实时转发到Audience Manager，而无需让客户端代码从页面向AAM发送额外的点击。 本教程将指导您完成以下步骤：从旧版“客户端DIL”实施切换到较新的“服务器端转发”方法。
product: audience manager
feature: Adobe Analytics 集成
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '2322'
ht-degree: 0%

---

# 将网站的AAM实施从[!DNL Client-Side]DIL迁移到[!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同时具有Adobe Audience Manager(AAM)和Adobe Analytics，并且当前使用“DIL”([!DNL Data Integration Library])代码从页面向AAM发送点击，同时从页面向Adobe Analytics发送点击，则本教程将适用于您。 由于您有这两个解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“[!DNL Server-Side Forwarding](SSF)”的最佳实践，该实践允许[!DNL Analytics]数据收集服务器将网站分析数据实时转发到Audience Manager，而不是让[!DNL client-side]代码从页面向AAM发送额外的点击。 本教程将指导您完成将交换机从旧的“[!DNL Client-Side DIL]”实施转换到较新的“[!DNL Server-Side forwarding]”方法的步骤。

## [!DNL Client-Side] (DIL)与  [!DNL Server-Side] {#client-side-dil-vs-server-side}

在比较这两种将Adobe Analytics数据导入AAM的方法并进行对比时，可能首先会帮助您直观地了解下图中的差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL实施 {#client-side-dil-implementation}

如果您使用此方法将Adobe Analytics数据导入AAM，则意味着您的网页有两个点击：一个转到[!DNL Analytics]，一个转到AAM（在复制了网页上的[!DNL Analytics]数据后）。 [!UICONTROL Segments] 会从AAM返回到页面，以便用于个性化等。这被视为“旧版”实施，不再建议这样做。

除了没有遵循最佳实践外，使用此方法的缺点还包括：

* 来自页面的两次点击，而不是仅一次点击
* [!UICONTROL Server-Side Forwarding] 要将AAM受众实时共享到， [!DNL Analytics]需要 [!DNL Client-side] 此参数，因此实施不允许使用此功能（将来可能还会使用其他功能）

建议您转到[!UICONTROL Server-Side Forwarding]方法进行AAM实施。

### [!UICONTROL Server-Side Forwarding] 实施 {#server-side-forwarding-implementation}

如上图所示，点击从网页到Adobe Analytics。 [!DNL Analytics] 然后将该数据实时转发到AAM，并对访客进行评估， [!UICONTROL traits] 如同点击直接来自页面一样 [!UICONTROL segments]，将评估到AAM和。

[!UICONTROL Segments] 在同一实时点击时返回， [!DNL Analytics]这会将响应转发到网页以进行个性化等。

移动到服务器端转发没有时间下限。 我们强烈建议任何同时具有Audience Manager和[!DNL Analytics]的人都使用此实施方法。

## 您有两项主要任务 {#you-have-two-main-tasks}

这页上有很多信息，当然，这都很重要。 但是，它&#x200B;**总之可以归结为您需要执行的两个主要操作：**

1. 将代码从[!DNL Client-Side]DIL代码更改为[!UICONTROL Server-Side Forwarding]代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转交换机，以开始实际转发数据（按[!UICONTROL report suite]）

如果您跳过这两个变量中的任何一个，则SSF将无法正常运行。 此文档中已添加步骤和其他数据，以帮助您为设置正确执行这两个步骤。

## 实施选项 {#implementation-options}

当您从[!DNL client-side]移动到[!DNL server-side]时，您需要执行的一项任务是将代码更改为新的[!UICONTROL Server-Side Forwarding]代码。 可使用以下选项之一完成此操作：

* Adobe Experience Platform Launch — 我们建议的Web资产实施选项。 您会看到，这是一项非常简单的任务，因为[!DNL Launch]已经为您完成了所有的困难工作。
* 在页面上 — 如果您尚未使用Launch，则还可以将新的SSF代码直接放入[!DNL appMeasurement.js]文件的`doPlugins`函数中，前提是您（尚未）使用AdobeLaunch
* 其他标签管理器 — 可以像上一个（在页面上）选项一样处理这些标签，因为无论其他标签管理器存储的是[!DNL AppMeasurement]代码，您仍会将SSF代码放置在`doPlugins`中

下面的更新代码部分中将分别介绍这些内容。

## 实施步骤 {#implementation-steps}

### 步骤0:先决条件：Experience CloudID服务(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

移动到[!UICONTROL Server-Side Forwarding]的主要先决条件是实施Experience CloudID服务。 如果您使用Experience Platform Launch，这非常容易完成，在这种情况下，您只需安装ECID扩展，然后它就会完成其余的操作。

如果您使用的是非AdobeTMS，或者根本没有TMS，请实施ECID以在&#x200B;**之前运行**&#x200B;任何其他Adobe解决方案。 有关更多详细信息，请参阅[ECID文档](https://marketing.adobe.com/resources/help/zh_CN/mcvid/) 。 其他唯一先决条件是与代码版本有关，因此，由于您只需在以下步骤中应用代码的最新版本，因此您将可以正常使用。

>[!NOTE]
>
>请在实施之前阅读此完整文档。 下面的“时间”部分包含有关&#x200B;*何时*&#x200B;应实施每个片段的重要信息，包括ECID（如果尚未实施）。

### 步骤1:从DIL代码中记录当前使用的选项 {#step-record-currently-used-options-from-dil-code}

在您准备好从[!DNL Client-Side]DIL代码移动到[!UICONTROL Server-Side Forwarding]时，第一步是识别您对DIL代码执行的所有操作，包括自定义设置和发送到AAM的数据。 需要注意和考虑的事项包括：

* 使用[!DNL siteCatalyst.init]DIL模块的常规[!DNL Analytics]变量 — 您无需担心此变量，因为其工作只是发送常规的[!DNL Analytics]变量，而且只需启用SSF即可。
* Partner Subdomain — 在DIL.create函数中，记下`partner`参数。 这称为您的“合作伙伴子域”，有时也称为“合作伙伴ID”，在放置新的SSF代码时，将需要使用此ID。
* [!DNL Visitor Service Namespace]  — 也称为“[!DNL Org ID]”或“[!DNL IMS Org ID]”，在设置新的SSF代码时，您也将需要此代码。记下来。
* containerNSID、uuidCookie和其他高级选项 — 记下您使用的任何其他高级选项，以便您也可以在SSF代码中设置它们。
* 其他页面变量 — 如果其他变量从页面发送到AAM（除了由siteCatalyst.init处理的常规[!DNL Analytics]变量之外），您需要记下这些变量，以便它们可以通过SSF(扰动程序警报：（通过[!DNL contextData]变量）。

### 步骤2:更新代码 {#step-updating-the-code}

在标题为“实施选项”的上述部分中，提供了多个有关如何/在何处实施[!UICONTROL Server-Side Forwarding]的选项。 为了使此部分生效，我们需要将其划分为以下部分（其中两个部分合并）。 转到此部分最能描述您需求的方法。

#### Adobe Experience Platform Launch {#launch-by-adobe}

请观看以下视频，了解如何在Experience Platform Launch中将实施选项从[!DNL Client-Side]DIL代码移动到[!UICONTROL Server-Side Forwarding]中。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### “在页面上”或非Adobe Tag Manager {#on-the-page-or-non-adobe-tag-manager}

请观看以下视频，了解如何将实施选项从[!DNL Client-Side]DIL代码移动到[!DNL AppMeasurement]代码中的[!UICONTROL Server-Side Forwarding]中，该代码驻留在文件或非Adobe标签管理系统中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步骤3:启用转发（每个[!UICONTROL Report Suite]）{#step-enabling-the-forwarding-per-report-suite}

在本教程中，我们已花费所有时间将代码从[!DNL Client-Side DIL]代码切换到[!UICONTROL Server-Side Forwarding]。 这没关系，因为那是比较困难的部分。 尽管您会看到此部分非常简单，但它与更新代码一样重要。 在此视频中，您将看到如何翻转开关，以启用数据从Analytics实际转发到Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如视频中所述，请记住，在Experience Cloud后端完全启用转发需要长达4小时的时间。

## 计时 {#timing}

请注意，从[!DNL Client-Side DIL]移至[!UICONTROL Server-Side Forwarding]的主要任务有两项：

1. 更新代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转开关

但问题是，你先做哪个？ 重要吗？ 好，抱歉，那是两个问题。 但答案是……取决于，是的，它&#x200B;*可以*。 这个怎么用含糊的？ 我们把它拆了。 但首先，如果您是一家拥有大量网站的大型组织，则还会出现一个额外的问题：我必须立刻做一切吗？ 那个比较容易。 不。 你可以一个接一个地做。:)

### 更深入一点 {#a-little-deeper-dive}

时间和顺序之所以重要，是因为转发*实际*工作方式，可以从以下几个技术事实中概括：

* 如果您实施了Experience CloudID服务(ECID)，并且[!DNL Analytics] [!DNL Admin Console]中的交换机(&quot;switch&quot;)已打开，则数据将从[!DNL Analytics]转发到AAM，即使您尚未更新代码。
* 如果您未实施ECID，则即使您已打开开关并且具有SSF代码，数据也将不会转发。
* SSF代码（无论是在[!DNL Launch]中还是在页面上）会实际处理响应，当然，完成迁移是必需的。
* 请记住，SSF交换机由[!UICONTROL Report Suite]启用，但代码由[!DNL Launch]中的属性处理，或者由[!DNL AppMeasurement]文件处理（如果不使用[!DNL Launch]）

### 最佳实践 {#best-practices}

根据这些技术细节，以下是关于“何时做什么”时间的建议：

#### 如果您尚未实施ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 在[!DNL Analytics]中为要为SSF启用的每个[!UICONTROL report suite]翻转交换机

   1. 由于您没有ECID，因此转发尚未启动

1. 对于每个网站，将您的代码从[!DNL Client-Side DIL]更新为SSF（这可以位于[!DNL Launch]中，也可以位于页面上，如上面其他部分中所述）

   1. 转发现在将会进行流动（您已添加ECID），并且您还应会收到对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的验证和疑难解答部分）

#### 如果您已实施ECID {#if-you-do-have-ecid-implemented}

1. 准备并规划，以便您准备好将代码从DIL更新为每个[!UICONTROL report suite]为SSF启用的SSF:

   1. 在[!DNL Analytics]中翻转交换机以启用SSF

      1. 将启动转发，因为您已启用ECID
   1. 请尽快将您的代码从[!DNL Client-Side DIL]更新为SSF（这可能位于[!DNL Launch]或页面上，如上面其他部分所述）

      1. 您应会收到对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的验证和疑难解答部分）


**注意1:** 务必要尽可能地将这两个步骤相互接近，因为在上面的第1步和第2步之间，您将会有进入AAM的重复数据。换言之，SSF将开始从[!DNL Analytics]向AAM发送数据，并且由于DIL代码仍在页面上，因此还会有一次点击从页面直接发送到AAM，从而使数据翻倍。 一旦您将代码从DIL更新为SSF，情况将会有所缓解。

**注意2:** 如果您希望在数据方面存在细微差异，而不是在数据方面有少量重复，则可以切换上面步骤1和2的顺序。将代码从DIL移动到SSF将停止数据流入AAM，直到您能够翻转交换机以打开[!UICONTROL report suite]的SSF。 通常，客户希望数据稍微翻倍，而不是错过将访客吸引到[!UICONTROL traits]和[!UICONTROL segments]的过程。

#### 迁移时间[!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

本主题在前几节中作了简要介绍，主要战略可概括如下：

一次迁移一个站点/[!UICONTROL report suite]（或一组站点/[!UICONTROL report suites]）。

但是，基于一些可能的情况，这可能会有些棘手：

* 您的站点包含多个不同的[!UICONTROL report suites]
* 您有一个[!UICONTROL report suite]，其中包含多个站点（如全局[!UICONTROL report suite]）
* 使用一个[!DNL Launch]属性覆盖多个站点
* 您有不同的开发团队，负责不同的站点

因为这些东西，它会变得有点复杂。 我最能建议的是：

* 请花些时间根据上面所述制定迁移到SSF的策略
* 根据[!DNL Launch]中的单个属性（或单个[!DNL AppMeasurement]文件）通常映射到1个或2个不同的[!UICONTROL report suites]事实，您可能能够逐一制定适用于这些不同组的计划，将企业更新为SSF
* 如果您与Adobe咨询团队合作，请与他们讨论您的迁移计划，以便他们在需要时获得帮助

## 验证和疑难解答 {#validation-and-troubleshooting}

验证[!UICONTROL Server-Side Forwarding]是否已启动且正在运行的主要方法是，查看应用程序对任意Adobe Analytics点击的响应。

如果您没有从[!DNL Analytics]Audience Manager[!UICONTROL server-side forwarding]数据，则实际上没有对[!DNL Analytics]信标的响应（除2x2像素以外）。 但是，如果您正在执行SSF，则可以在[!DNL Analytics]请求和响应中验证一些项目，以确认[!DNL Analytics]与Audience Manager正确通信、转发点击并获取响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 请注意虚假的“成功” — 如果存在响应，且一切似乎正常，请确保响应中包含“stuff”对象。如果没有，您可能会看到一条显示[!DNL "status":"SUCCESS"]的消息。 尽管这听起来很不可思议，但实际上却证明它没有正常运行。 如果您看到此消息，则表示您已完成[!DNL Launch]或[!DNL AppMeasurement]中的代码更新，但尚未完成[!DNL Analytics] [!DNL Admin Console]中的转发。 在这种情况下，您需要验证是否已在[!DNL Analytics] [!DNL Admin Console]中为[!UICONTROL report suite]启用SSF。 如果您已经执行了操作，但尚未满4小时，请耐心等待，因为在后端进行所有必要的更改可能需要那么长时间。

![误报](assets/falsesuccess.png)

有关[!UICONTROL Server-Side Forwarding]的更多信息，请参阅[文档](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)。
