---
title: トラッキングサーバーからレポートスイートレベルのサーバー側転送への移行
description: この記事とビデオでは、Analyticsデータのサーバー側転送を、トラッキングサーバーレベルではなく、レポートスイートレベルでAudience Managerに有効にする方法を示します。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---


# から [!UICONTROL Tracking Server][!UICONTROL Report Suite]レベルへの移行 [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

この記事とビデオでは、あるレベルではなく、ある [!UICONTROL server-side forwarding] レベルで [!DNL Analytics] Audience Managerに対する [!UICONTROL report suite] データを有効にする方法を説明し [!UICONTROL tracking server] ます。

## はじめに {#introduction}

Adobe Audience ManagerとAdobe Analyticsがある場合は、[!UICONTROL Server-side Forwarding]Audience Managerにデータの「 [!DNL Analytics] 」を実装できます。 つまり、ページから2つのヒット(1つはAudience Managerに対して)を送信する代わりに、単にヒットを送信し、そのデータをAudience Managerに転送するこ [!DNL Analytics] と [!DNL Analytics][!DNL Analytics] ができます。 既にこの設定を実行しており、2017年10月以前に有効化/実装していた場合、「 [!UICONTROL server-side forwarding][!UICONTROL Tracking Server]」を基にしている可能性があります。これは、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要があります。 2017年10月現在、自分で設定を行うこ [!UICONTROL server-side forwarding] とができ、 [!UICONTROL Report Suite] レベル(転送PER [!UICONTROL Report Suite])で行うことができます。 これには大きな利点がありますが、以下で説明します。

## [!UICONTROL Tracking Server] 転送 {#tracking-server-forwarding}

は、 [!UICONTROL tracking server][!DNL Analytics] データの送信先、およびイメージリクエストとCookieが書き込まれているドメインです。 DTMまたはフ [!DNL Experience Platform Launch][!DNL AppMeasurement.js] ァイルで設定する必要があり、通常は次のようになります。サイト名やビジネス名は「mysite」に置き換えられます。

`s.trackingServer = "mysite.sc.omtrdc.net";`

がレベルで転送 [!UICONTROL server-side forwarding] するように設定されている場合、 [!UICONTROL tracking server][!UICONTROL tracking server] このレベルに送信されるヒット(Experience CloudIDサービスも有効になっている場合)はすべてAudience Managerに転送されます。 これは、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要がありました。 後述のように、 [!UICONTROL report suite] 転送に切り替えた後に、この機能を無効にすることもできます。

が有効になっているかどうかが不明な場合 [!DNL tracking server forwarding] は、AdobeカスタマーケアまたはAdobeコンサルティングにお問い合わせください。

## [!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

転送から転送に移行する最大の利点の1つは、「Audience Analytics」を使用できることです。これは、Audience Managerを詳細な [!UICONTROL report suite] 分析 [!UICONTROL tracking server][!UICONTROL segments][!UICONTROL segment] のためにAdobe Analyticsに転送する機能です。 この優れた機能は、まだ転送中で、転送中でない場合にはサポートされません [!UICONTROL tracking server][!UICONTROL report suite] 。 Audience Analyticsの詳細については、 [ドキュメントを参照してください](https://marketing.adobe.com/resources/help/ja_JP/analytics/audiences/)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要なヒント {#additional-resources}

上記のビデオで説明したように、Audience Managerに転送するすべてのセットを転送する [!UICONTROL report suites] と、AdobeカスタマーケアまたはAdobeコンサルティングに問い合わせて、 [!UICONTROL tracking server] 転送を無効にするように依頼する必要があります。 これは、 [!UICONTROL tracking server][!UICONTROL report suite] 転送と転送の両方を行っても重複のヒットにはならないので、緊急に行う必要はありません。 ただし、 [!UICONTROL report suite] 転送のみを有効にすることをお勧めします。 転送をオンにした場合、転送を行わな [!UICONTROL tracking server] いデータを転送するだけでなく、今後、転送が有効になっていることを忘れた(会社の全員が)データ [!UICONTROL report suites] は特定のデータに転送されないと考えられる可能性があり [!UICONTROL tracking server][!UICONTROL report suite][!UICONTROL tracking server]ます。 そうすれば、転送の理由を知る時間と費用を無駄にし、予期しなかったAAMサーバの呼び出しに対する料金も支払うことになります。 したがって、ビジネスニーズに合わせて適切な転送がすべて設定されたら、 [!UICONTROL tracking server] 転送をすぐに無効にす [!UICONTROL report suites] ることをお勧めします。
