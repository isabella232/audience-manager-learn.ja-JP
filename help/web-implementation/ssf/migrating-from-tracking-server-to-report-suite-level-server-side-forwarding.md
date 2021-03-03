---
title: トラッキングサーバーからレポートスイートレベルのサーバー側転送への移行
description: この記事とビデオでは、Analyticsデータのサーバー側転送を、トラッキングサーバーレベルではなく、レポートスイートレベルでAudience Managerに有効にする方法を示します。
product: audience manager, analytics
feature: Adobe Analytics との統合
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: 「開発者、データ・エンジニア」
level: 中間
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 2%

---


# [!UICONTROL Tracking Server]から[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}への移行

この記事とビデオでは、[!DNL Analytics]の[!UICONTROL server-side forwarding]を[!UICONTROL tracking server]レベルではなく[!UICONTROL report suite]レベルでAudience Managerする方法を説明します。

## はじめに {#introduction}

Adobe Audience ManagerとAdobe Analyticsをお持ちの場合は、[!DNL Analytics]データの「[!UICONTROL Server-side Forwarding]」をAudience Managerに実装できます。 これは、ページが2つのヒット(1つは[!DNL Analytics]に、もう1つはAudience Managerに)を送る代わりに、単にヒットを[!DNL Analytics]に送ることができ、[!DNL Analytics]はそのデータをAudience Managerに転送することを意味します。 既にこの機能を使用し、2017年10月以前に有効化/実装していた場合、[!UICONTROL server-side forwarding]は、AdobeのカスタマーケアまたはAdobeコンサルティングが有効にする必要がある「[!UICONTROL Tracking Server]」を基にしている可能性があります。 2017年10月現在、[!UICONTROL server-side forwarding]を自分で設定し、[!UICONTROL Report Suite]レベル（転送PER [!UICONTROL Report Suite]）で行うことができます。 これには大きな利点がありますが、以下で説明します。

## [!UICONTROL Tracking Server] 転送  {#tracking-server-forwarding}

[!UICONTROL tracking server]は、[!DNL Analytics]データの送信先であり、また、イメージリクエストとcookieが書き込まれているドメインでもあります。 DTMまたは[!DNL Experience Platform Launch]、または[!DNL AppMeasurement.js]ファイルで設定する必要があり、通常は次のようになります。サイト名やビジネス名は「mysite」に置き換えられます。

`s.trackingServer = "mysite.sc.omtrdc.net";`

[!UICONTROL server-side forwarding]が[!UICONTROL tracking server]レベルで転送するように設定されている場合、この[!UICONTROL tracking server]に送信されるヒット(Experience CloudIDサービスも有効な場合)は、Audience Managerに転送されます。 これは、AdobeカスタマーケアまたはAdobeコンサルティングが有効にする必要がありました。 [!UICONTROL report suite]転送に切り替えた後、以下に説明するように、これを無効にすることもできます。

[!DNL tracking server forwarding]が有効になっているかどうかがわからない場合は、AdobeカスタマーケアまたはAdobeコンサルティングにお問い合わせください。

## [!UICONTROL Report Suite]-Level  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

[!UICONTROL tracking server]転送から[!UICONTROL report suite]転送に移る最大の利点の1つは、Audience Manager[!UICONTROL segments]を詳細な[!UICONTROL segment]分析に戻す機能である「Audience Analytics」を使えるようになることです。 [!UICONTROL tracking server]転送中で[!UICONTROL report suite]転送が行われていない場合、この優れた機能はサポートされません。 Audience Analyticsに関する詳細は、[ドキュメント](https://marketing.adobe.com/resources/help/ja_JP/analytics/audiences/)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要なヒント{#additional-resources}

上のビデオで述べたように、[!UICONTROL report suites]のすべてをAudience Managerに転送するように設定したら、AdobeカスタマーケアまたはAdobeコンサルティングに連絡して、[!UICONTROL tracking server]転送を無効にするように依頼する必要があります。 [!UICONTROL tracking server]転送と[!UICONTROL report suite]転送の両方を行っても重複ヒットにはならないので、緊急に行う必要はありません。 ただし、[!UICONTROL report suite]転送のみを有効にすることをお勧めします。 [!UICONTROL tracking server]転送を有効にしておくと、[!UICONTROL report suites]からの転送を行わないデータを転送するだけでなく、将来、[!UICONTROL tracking server]転送が有効になったことを忘れた後、（レポートスイートレベルでは有効にならないので）[!UICONTROL report suite]の転送は行われないa4/>。 [!UICONTROL tracking server]そうすれば、転送の理由を知る時間と費用を無駄にし、予期しなかったAAMサーバの呼び出しに対する料金も支払うことになります。 だから、[!UICONTROL tracking server]の転送を、お客様のビジネスニーズに合った転送を行うように[!UICONTROL report suites]のすべてを設定し終えたら、すぐに無効にすることをお勧めします。
