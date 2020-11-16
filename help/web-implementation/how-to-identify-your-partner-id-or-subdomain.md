---
title: Audience ManagerパートナーIDまたはサブドメインの識別方法
description: 一部のExperience Cloud機能を実装する場合、Audience Managerの「パートナーID」（「クライアントID」または「サブドメイン」とも呼ばれる）が何かを知っておく必要があります。 このビデオでは、Audience ManagerUIでこのIDを取得できる場所を2つ示します。
feature: implementation basics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 0%

---


# Audience Managerサブドメインの識別方法 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

一部のExperience Cloud機能を実装する場合、Audience Managerの概要(またはとも呼ばれ `Subdomain` る場合があります `client ID``Partner ID`)を知る必要があります。 このビデオでは、Audience ManagerUIでこの情報を取得できる場所を2つ示します。

## 結末を… {#giving-away-the-ending}

この短いビデオを見ないでジャンプインして見つけたい場合は、UIの2か所で検索でき `Partner Subdomain` ます。

1. 既に [!UICONTROL rule-based][!UICONTROL trait]**[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] がそのフォルダー [!UICONTROL trait] 内ののリスト内のの横にあ [!UICONTROL traits] り、URLにはそのサブドメインが含まれます。
1. >のインター **[!UICONTROL Tools]** フェイスに移動し、コンテナのをクリック **[!UICONTROL Tags]****[!UICONTROL Get code]** すると、サブドメインはAkamai行の終わりに近づきます

これらのクイックリファレンスですばやく見つけられない場合、ビデオは短い時間で公開されます。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各顧客に割り当てられる数値IDがあり、これは「PID」またはパートナーIDと呼ばれることが多いです。 この記事とビデオで扱っているIDではありません。 代わりに、「パートナーサブドメイン」（パートナーIDと呼ばれることもあります）は、通常、クライアント名のバージョンで、データの送信先のサーバーのサブドメインです。 例えば、会社が「Bob&#39;s Knobs」（Bob&#39;s Knobs、もちろん、haha）の場合、パートナーのサブドメインは「bobsknobs」、「PID」は「12345」のようになります。 通常、PIDを知る必要はありませんが、サブドメインを知ることが重要なので、Audience Manager実装を設定できます。

