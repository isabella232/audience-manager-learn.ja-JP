---
title: パートナー ID またはサブドメインの識別方法
description: 一部のExperience Cloud機能を実装する際にパートナー ID またはサブドメインを識別する方法、およびAudience ManagerUI でこの ID を取得できる 2 つの場所について説明します。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Audience Managerサブドメインの識別方法 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

一部のExperience Cloud機能を実装する場合は、Audience Managerを知っておく必要があります `Subdomain` は、 `client ID` または `Partner ID`) をクリックします。 このビデオでは、この情報をAudience ManagerUI で取得できる 2 つの場所を示します。

## 終わりを渡す… {#giving-away-the-ending}

この短いビデオを見ずに飛び込んで見つけたい場合は、 `Partner Subdomain` UI の 2 か所：

1. 既に [!UICONTROL rule-based] 特性、クリック **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] は、そのフォルダーの特性のリストで特性の横にあり、URL にはサブドメインが含まれます。
1. もし **[!UICONTROL Tools]** > **[!UICONTROL Tags]** インターフェイスを開き、 **[!UICONTROL Get code]** コンテナの場合、サブドメインは Akamai 行の終わりに向かっています。

これらのクイックリファレンスですばやく見つからない場合、ビデオは短時間のコミットメントです。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各顧客に割り当てられた数値 ID があります。これは、「PID」（パートナー ID）とも呼ばれます。 この記事とビデオで説明している ID ではありません。 代わりに、「パートナーサブドメイン」（パートナー ID とも呼ばれます）は通常、クライアント名のバージョンで、データの送信先のサーバーのサブドメインです。 例えば、会社が「Bob&#39;s Knobs」（すべてのもののドアノブ、もちろん、笑）の場合、パートナーサブドメインは「bobsknobs」であるのに対して、「PID」は「12345」のような値になる可能性が高くなります。 通常、PID を知る必要はありませんが、サブドメインを知っておく必要があるので、Audience Manager実装を設定できます。
