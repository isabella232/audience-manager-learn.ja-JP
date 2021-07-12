---
title: Audience ManagerパートナーIDまたはサブドメインの識別方法
description: 一部のExperience Cloud機能を実装する場合は、Audience Managerの「パートナーID」（「クライアントID」や「サブドメイン」とも呼ばれます）を把握しておく必要があります。 このビデオでは、Audience ManagerUIでこのIDを取得できる2つの場所を示します。
feature: 実装の基本
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# サブドメインの識別Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

一部のExperience Cloud機能を実装する場合は、Audience Manager`Subdomain`（`client ID`または`Partner ID`とも呼ばれます）を把握しておく必要があります。 このビデオでは、Audience ManagerUIでこの情報を取得できる2つの場所を示します。

## 終わりを言い渡す… {#giving-away-the-ending}

この短いビデオを見ずにジャンプして見つけたい場合は、UIの2か所で`Partner Subdomain`を見つけることができます。

1. 既に[!UICONTROL rule-based] [!UICONTROL trait]を作成している場合は、「**[!UICONTROL Get Trait URL]**」をクリックします。
   [!UICONTROL Get Trait URL] がそのフォルダ [!UICONTROL trait] ーのリストの [!UICONTROL traits] 横にあり、URLにサブドメインが含まれます。
1. **[!UICONTROL Tools]** / **[!UICONTROL Tags]**&#x200B;インターフェイスに移動してコンテナの「**[!UICONTROL Get code]**」をクリックすると、サブドメインはAkamai行の終わりに近づきます

これらのクイックリファレンスですばやく見つけられない場合、ビデオは短時間のコミットメントです。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各顧客に割り当てられる数値IDがあり、これは「PID」やパートナーIDとも呼ばれます。 この記事とビデオで説明しているIDではありません。 代わりに、「パートナーサブドメイン」（パートナーIDとも呼ばれます）は通常、クライアント名のバージョンで、データの送信先のサーバーのサブドメインになります。 例えば、会社が「Bob&#39;s Knobs」（もちろん、すべてのもののドアノブ）の場合、パートナーサブドメインは「bobsknobs」ですが、「PID」は「12345」のような値になる可能性が高くなります。 通常はPIDを知る必要はありませんが、サブドメインを知っておくことが重要です。そうすれば、Audience Manager実装を設定できます。

