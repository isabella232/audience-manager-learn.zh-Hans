---
title: 将站点的AAM实施从客户端DIL迁移到服务器端转发
description: 本教程适用于您(如果您同时拥有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前使用“DIL”(Data Integration Library)代码从页面发送点击到AAM，并且还从页面发送点击到Adobe Analytics。 由于您有这两种解决方案，而且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“服务器端转发(SSF)”的最佳实践，这使Analytics数据收集服务器能够将站点分析数据实时转发给Audience Manager，而不是让客户端代码从页面向AAM再发送点击。 本教程将引导您完成从旧的“客户端DIL”实现切换到新的“服务器端转发”方法的步骤。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
translation-type: tm+mt
source-git-commit: 133279f589bd58aef36a980c2b7248ae00fd9496
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 0%

---


# 将站点的AAM实施从 [!DNL Client-Side] DIL [!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

本教程适用于您(如果您同时拥有Adobe Audience Manager(AAM)和Adobe Analytics，并且您当前使用“DIL”()代码从页面发送点击到AAM,[!DNL Data Integration Library]同时还从页面发送点击到Adobe Analytics。 由于您有这两种解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循打开“[!DNL Server-Side Forwarding] (SSF)”的最佳实践，这使数据收集服务器能够将站点分析数据实时转发到Audience Manager，而 [!DNL Analytics][!DNL client-side] 不是让代码从页面向AAM发送额外的点击。 本教程将引导您完成从旧“”实现切换到新[!DNL Client-Side DIL]“”方法的[!DNL Server-Side forwarding]步骤。

## [!DNL Client-Side] (DIL)与 [!DNL Server-Side] {#client-side-dil-vs-server-side}

在比较和对比这两种将Adobe Analytics数据导入AAM的方法时，首先可以在以下图像中直观显示差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL实施 {#client-side-dil-implementation}

如果您使用此方法将Adobe Analytics数据导入AAM，则意味着您的网页有两个点击：一个转 [!DNL Analytics]到，一个转到AAM(在将数据复制到网 [!DNL Analytics] 页上之后)。 [!UICONTROL Segments] 将从AAM返回到页面，在页面中可用于个性化等。 这被视为“旧”实施，不再建议这样做。

除了不遵循最佳做法之外，使用此方法的缺点还包括：

* 页面中有两次点击，而不是只有一次
* [!UICONTROL Server-Side Forwarding] 要求实时共享AAM受众到，因此实 [!DNL Analytics]施 [!DNL Client-side] 不允许此功能（以及将来可能的其他功能）

建议您转到AAM实 [!UICONTROL Server-Side Forwarding] 现方法。

### [!UICONTROL Server-Side Forwarding] 实施 {#server-side-forwarding-implementation}

如上图所示，点击从网页到Adobe Analytics。 [!DNL Analytics] 然后将该数据实时转发到AAM,访客将被评估为AAM [!UICONTROL traits] 和 [!UICONTROL segments]，就像点击直接来自页面一样。

[!UICONTROL Segments] 返回到的内容， [!DNL Analytics]将响应转发到网页进行个性化等。

转到服务器端转发没有时间限制。 我们强烈建议任何同时具有Audience Manager和使用此 [!DNL Analytics] 实现方法的人。

## 您有两个主要任务 {#you-have-two-main-tasks}

这页上有很多信息，当然，这都很重要。 然而，归 **根到底，您需要做两件主要事情**:

1. 将代码从 [!DNL Client-Side] DIL代码更 [!UICONTROL Server-Side Forwarding] 改为
1. 在中翻动交换机 [!DNL Analytics] , [!DNL Admin Console] 开始实际的数据转发(每 [!UICONTROL report suite]个)

如果跳过这两个选项之一，SSF将无法正常工作。 此文档中已添加步骤和其他数据，以帮助您正确执行这两个步骤以完成设置。

## 实施选项 {#implementation-options}

当您从 [!DNL client-side] 到 [!DNL server-side]时，您将拥有的任务之一将代码更改为新代 [!UICONTROL Server-Side Forwarding] 码。 使用以下选项之一完成此操作：

* Adobe Experience Platform Launch-我们建议的Web属性实施选项。 您将看到，这是一个非常简单的任务, [!DNL Launch] 就像您做了所有艰难的事情一样。
* 在页面上——如果您尚未（尚未）使用Adobe启 `doPlugins` 动，还可以将 [!DNL appMeasurement.js] 新的SSF代码直接放入文件的函数中
* 其他标签管理器——这些标签管理器可以像上一个（在页面上）选项一样处理，因为您仍将将SSF代码放 `doPlugins`入到其他标签管理器存储代码的 [!DNL AppMeasurement] 任何地方

我们将在“更新代码”部分查看以下各项。

## 实施步骤 {#implementation-steps}

### 步骤0:入门项目：Experience CloudID服务(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

迁移到的主要先决条件 [!UICONTROL Server-Side Forwarding] 是实现Experience CloudID服务。 如果您使用Experience Platform Launch，则最容易完成此操作，在这种情况下，您只需安装ECID扩展，它将完成其余的操作。

如果您使用的是非AdobeTMS，或根本没有TMS，请实施ECID以在任何其他Adobe解决 **方案** 之前运行。 有关更多 [详细信息](https://marketing.adobe.com/resources/help/zh_CN/mcvid/) ，请参阅ECID文档。 唯一的其他先决条件是有关代码版本，因此，只需在以下步骤中应用代码的最新版本，您就可以了。

>[!NOTE]
>
>请在实施之前阅读整个文档。 下面的“时间”部分包含有关您应 *当何时* 实施每个片段(包括ECID（如果尚未实现）的重要信息。

### 第1步：从DIL代码记录当前使用的选项 {#step-record-currently-used-options-from-dil-code}

在您准备好从DIL代 [!DNL Client-Side] 码移动到 [!UICONTROL Server-Side Forwarding]AAM时，第一步是识别您正在使用DIL代码执行的所有操作，包括自定义设置和发送到的数据。 需要注意和考虑的事项包括：

* 正 [!DNL Analytics] 常变量，使 [!DNL siteCatalyst.init][!DNL Analytics] 用DIL模块——您无需担心此变量，因为它的工作只是发送正常变量，而只需启用SSF即可实现。
* Partner Subdomain —— 在DIL.create函数中，记下该参 `partner` 数。 这称为您的“合作伙伴子域”，有时也称为“合作伙伴ID”，在您放置新的SSF代码时将需要它。
* [!DNL Visitor Service Namespace] -也称为“”[!DNL Org ID]或“[!DNL IMS Org ID]”，在设置新的SSF代码时，您也需要此代码。 记下来。
* containerNSID、uuidCookie和其他高级选项——记下您正在使用的任何其他高级选项，以便您也可以在SSF代码中设置它们。
* 其他页面变量——如果其他变量从页面发送到AAM(除了siteCatalyst.init处理的 [!DNL Analytics] 常规变量外)，您需要记下这些变量，以便通过SSF发送它们(扰动程序警报：(通过 [!DNL contextData] 变量)。

### 第2步：更新代码 {#step-updating-the-code}

在上面标题为“实施选项”的部分中，会提供多个关于您如何／在哪里实施的选项 [!UICONTROL Server-Side Forwarding]。 为了使本节有效，我们需要将其分解为以下几节（其中两节合并）。 转到本节最能描述您需求的方法。

#### Adobe Experience Platform Launch {#launch-by-adobe}

观看以下视频，了解如何将实施选项从 [!DNL Client-Side] DIL代码移 [!UICONTROL Server-Side Forwarding] 入Experience Platform Launch。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 《在页上》或非Adobe Tag Manager {#on-the-page-or-non-adobe-tag-manager}

观看以下视频，了解如何将实施选项从 [!DNL Client-Side] DIL代 [!UICONTROL Server-Side Forwarding] 码移 [!DNL AppMeasurement] 动到代码中，驻留在文件或非Adobe标签管理系统中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 第3步：启用转发(每 [!UICONTROL Report Suite]个) {#step-enabling-the-forwarding-per-report-suite}

在本教程中，我们一直将时间用在将代码从代码切换 [!DNL Client-Side DIL] 到代码上 [!UICONTROL Server-Side Forwarding]。 这没关系，因为这是更困难的部分。 此部分虽然非常简单，但与更新代码同样重要。 在此视频中，您将看到如何打开交换机，以实际将数据从Analytics转发到Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如视频中所述，请记住，在Experience Cloud后端上完全实现转发需要4个小时。

## 计时 {#timing}

作为提醒，有两个主要任务可以从移动 [!DNL Client-Side DIL] 到 [!UICONTROL Server-Side Forwarding]:

1. 更新代码
1. 在 [!DNL Analytics] [!DNL Admin Console]

但问题是，你先做哪个？ 重要吗？ 好，抱歉，那是两个问题。 但答案是，这取决于情况，是的，这 *很重* 要。 这样很模糊？ 让我们将其分解。 但首先，如果您是拥有大量站点的大型组织，还会出现一个额外的问题：我必须立刻做所有事吗？ 那个比较容易。 不。 你可以一个接一个地做。:)

### 更深入一点 {#a-little-deeper-dive}

时间和顺序问题的原因在于转发的实际工作方式，这可以概括为以下几个技术事实：

* 如果您已实现Experience CloudID服务(ECID)，并且（“开关”）中的 [!DNL Analytics] 开关 [!DNL Admin Console] 已打开，则数据将从 [!DNL Analytics] AAM转发，即使您尚未更新代码。
* 如果未实现ECID，则数据将不会转发，即使您已打开开关并且具有SSF代码。
* SSF代码(无论在页 [!DNL Launch] 面中还是在页面上)确实会处理响应，当然，完成迁移是必需的。
* 请记住，SSF开关由启 [!UICONTROL Report Suite]用，但代码由中的属性 [!DNL Launch]处理， [!DNL AppMeasurement] 或者由文件处理(如果您不使用 [!DNL Launch]

### 最佳实践 {#best-practices}

根据这些技术细节，以下建议了“何时做什么”的时间安排：

#### 如果尚未实现ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 为SSF启用 [!DNL Analytics] 的每 [!UICONTROL report suite] 个开关打开开关

   1. 转发尚未开始，因为您没有ECID

1. 每个站点，将您的代 [!DNL Client-Side DIL] 码从更新到SSF(该代码可 [!DNL Launch] 能位于页面中或页面上，如上面另一节所述)

   1. 转发现在将进行（因为您已添加了ECID），您还应收到对信标的正确JSON响应(有关更多详细信 [!DNL Analytics] 息，请参阅下面的验证和疑难解答部分)

#### 如果您已实现ECID {#if-you-do-have-ecid-implemented}

1. 准备并计划，以便您准备好将代码从DIL更新为SSF PER, [!UICONTROL report suite] 并为SSF启用：

   1. 打开交换机以 [!DNL Analytics] 启用SSF

      1. 转发WILL开始，因为您已启用ECID
   1. 尽快将您的代码从更新 [!DNL Client-Side DIL] 到SSF(该代码可能在页 [!DNL Launch] 面中或页面上，如上面另一节所述)

      1. 您应收到对信标的正确JSON响 [!DNL Analytics] 应（有关更多详细信息，请参阅下面的验证和疑难解答部分）


**附注1:** 这两个步骤必须尽可能接近，因为在以上步骤1和步骤2之间，您将会有重复的数据流入AAM。 换言之，SSF将开始从AAM发送数据 [!DNL Analytics] ，由于DIL代码仍在页面上，因此也会有从页面直接点击到AAM的数据，从而使数据翻倍。 一旦将代码从DIL更新到SSF，这将得到缓解。

**附注2:** 如果您希望在数据方面存在细微差异，而不是少量重复数据，您可以切换上面步骤1和2的顺序。 将代码从DIL移到SSF将停止数据流到AAM，直到您能够打开交换机以打开SSF [!UICONTROL report suite]。 通常，客户宁可将数据量翻一倍，也不愿错过让访客进入 [!UICONTROL traits] 和的 [!UICONTROL segments]机会。

#### 拥有多个站点和 [!UICONTROL Report Suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

在前几节中，本主题将作一简要介绍，主要战略可概括如下：

一次迁移一个站点[!UICONTROL report suite] /(或一组站点[!UICONTROL report suites]/)。

但是，根据一些可能的情况，这可能会变得有些棘手：

* 您的站点包含多个不同的 [!UICONTROL report suites]
* 您的站点 [!UICONTROL report suite] 包括多个站点(如全局站 [!UICONTROL report suite]点)
* 使用一个属 [!DNL Launch] 性覆盖多个站点
* 您拥有针对不同站点的不同开发团队

由于这些项目，它会变得有点复杂。 我最能建议的是：

* 根据上述说明制定迁移到SSF的策略需要一些时间
* 根据（或单个文件）中 [!DNL Launch] 的单个属 [!DNL AppMeasurement][!UICONTROL report suites]性通常映射到1或2个不同的事实，您可能能够逐个地制定适用于这些不同组的计划，将您的企业更新到SSF
* 如果您与Adobe咨询部门合作，请与他们讨论您的迁移计划，以便他们能够根据需要提供帮助

## 验证和疑难解答 {#validation-and-troubleshooting}

验证该应用程序是否已 [!UICONTROL Server-Side Forwarding] 启动并正在运行的主要方法是查看您对来自该应用程序的任何Adobe Analytics点击的响应。

如果您不从Audience Manager [!UICONTROL server-side forwarding] 到 [!DNL Analytics] ，则对信标的响应将真的不响应( [!DNL Analytics] 除2x2像素外)。 但是，如果您正在执行SSF，则在请求和响应中可以验证一些项 [!DNL Analytics] ，这将让您知道 [!DNL Analytics] 是否与Audience Manager正确通信、转发点击和获得响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 谨防虚假的“成功”-如果有响应，并且一切似乎都正常，请确保响应中包含“物品”对象。 如果您不这样做，您可能会看到一条消息说 [!DNL "status":"SUCCESS"]。 尽管这听起来很疯狂，但这实际上是验证，它不能正确工作。 如果您看到这一点，则表示您已在或中完成 [!DNL Launch] 了代 [!DNL AppMeasurement]码更新，但中的转发 [!DNL Analytics][!DNL Admin Console] 尚未完成。 在这种情况下，您需要验证是否已在中为您的SSF [!DNL Analytics] 启 [!DNL Admin Console] 用了 [!UICONTROL report suite]。 如果您已经做过，而且还没有4小时，请耐心等待，因为可能需要这么长时间才能在后端进行所有必要的更改。

![虚假成功](assets/falsesuccess.png)

For more information about [!UICONTROL Server-Side Forwarding], please see the [documentation](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html).
