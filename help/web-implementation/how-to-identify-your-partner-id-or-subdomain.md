---
title: Audience ManagerパートナーIDまたはサブドメインの識別方法
description: 一部のExperience Cloud機能を実装する場合、Audience Managerの「パートナーID」（「クライアントID」または「サブドメイン」とも呼ばれる）が何かを知っておく必要があります。 このビデオでは、Audience ManagerUIでこのIDを取得できる場所を2つ示します。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: "Developer, Data Engineer"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---


# Audience Managerサブドメインの識別方法{#how-to-identify-your-audience-manager-partner-id-or-subdomain}

一部のExperience Cloud機能を実装する場合は、Audience Manager`Subdomain`が何か（`client ID`または`Partner ID`とも呼ばれます）を知っておく必要があります。 このビデオでは、Audience ManagerUIでこの情報を取得できる場所を2つ示します。

## 終了を渡しています… {#giving-away-the-ending}

この短いビデオを見ないで飛び込んで見つけたい場合には、UIの2か所で`Partner Subdomain`を見つけることができます。

1. [!UICONTROL rule-based] [!UICONTROL trait]を既に作成済みの場合は、「**[!UICONTROL Get Trait URL]**」をクリックします
   [!UICONTROL Get Trait URL] がそのフォルダー [!UICONTROL trait] 内ののリスト [!UICONTROL traits] 内のの横にあり、URLにはそのサブドメインがURLに含まれます。
1. **[!UICONTROL Tools]** > **[!UICONTROL Tags]**&#x200B;のインターフェイスに移動し、コンテナの&#x200B;**[!UICONTROL Get code]**&#x200B;をクリックした場合、サブドメインはAkamaiの行の終わりに近づきます

これらのクイックリファレンスですばやく見つけられない場合、ビデオは短い時間で公開されます。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各顧客に割り当てられる数値IDがあり、これは「PID」またはパートナーIDと呼ばれることが多いです。 この記事とビデオで扱っているIDではありません。 代わりに、「パートナーサブドメイン」（パートナーIDと呼ばれることもあります）は、通常、クライアント名のバージョンで、データの送信先のサーバーのサブドメインです。 例えば、会社が「Bob&#39;s Knobs」（Bob&#39;s Knobs、もちろん、haha）の場合、パートナーのサブドメインは「bobsknobs」、「PID」は「12345」のようになります。 通常、PIDを知る必要はありませんが、サブドメインを知ることが重要なので、Audience Manager実装を設定できます。

