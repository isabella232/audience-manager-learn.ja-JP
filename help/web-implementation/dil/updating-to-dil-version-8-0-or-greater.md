---
title: Adobe Audience ManagerDILバージョン 8.0（またはそれ以降）への更新
description: この記事では、Adobe Audience Manager(AAM)Data Integration Library(DIL) コードをバージョン 8.0 以降に更新する手順と推奨事項を説明します。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」DILの実装を指し、Adobeタグマネージャーソリューションを使用しない DTM、Launch by Adobe、実装について説明します。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 5%

---

# Adobe Audience ManagerDILバージョン 8.0（またはそれ以降）への更新 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

この記事では、Adobe Audience Manager(AAM) の更新に関する手順と推奨事項を説明します [!DNL Data Integration Library] (DIL) コードをバージョン 8.0 以降に追加しました。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」DILの実装を指し、Adobeタグマネージャーソリューションを使用しない DTM、Launch by Adobe、実装について説明します。

## 概要 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL) コードを使用すると、Web サイト*にAAMを実装できます。 以前のバージョンのDILを実装する場合、AdobeのExperience CloudID サービス (ECID) も実装する必要はありませんでした（ただし、これは非常に良い考えでした）。 DILバージョン 8.0 以降は、ECID バージョン 3.3 以降に強く依存します。 ECID 3.3 を使用せずにDIL8.0 以降を実装した場合、または以前のバージョンを使用している場合、エラーが表示され、機能しません。 AAMの実装には複数の方法があるので、手順を説明するためにこのページを作成し、推奨事項もいくつか示します。 以下に、これらの手順と推奨事項を、プラットフォーム/実装方法別に示します。 DILの詳細については、 [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* このページの説明に記載されているように、これは、Adobe Analyticsを持たないAAMのお客様が使用する、「クライアント側」のDIL実装のみをカバーします。 Adobe Analyticsを使用している場合、AAMを実装するサーバー側転送方法を使用する必要があります。 このメソッドについては、 [ドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=ja).

## 重複要素および非推奨要素およびメソッド {#duplicate-and-deprecated-elements-and-methods}

以前のバージョンのDILおよび ECID には、重複するメソッド (DILと ECID の両方で同じ機能を実行するメソッド ) があったので、どちらを使用するかに関する混乱が生じていました。 通常は、両方を使用してマッチさせる必要があり、そのメッセージはお客様にうまく伝えられませんでした。 DIL8.0 以降、これらの重複メソッドおよび要素はDILで非推奨となりました。ECID バージョンを使用することをお勧めします。

例：

* を使用する場合 [!DNL DIL.create]の場合、一部の要素は非推奨となり、代わりに ECID 要素を使用する必要があります。 これらの要素は、 [[!DNL DIL.create] ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* この [!DNL idSync] インスタンスレベルのメソッドも、メソッドの [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## 顧客 ID との ID 同期 {#id-syncing-with-a-customer-id}

AAMでは、マシン上の UUID（匿名の一意のユーザー ID）を顧客 ID と同期して、その顧客に関するオフラインデータをアップロードし、オンライン動作と結び付けて、顧客の理解を深めることができます。 これまでは、次の 2 つの方法のいずれかで行われていました。

* この [!DNL idSync] インスタンスレベルのメソッド
* この [!DNL declaredId] 要素 [!DNL DIL.create]

顧客 ID との同期にこれらの古い方法のいずれかを使用している場合は、 [!DNL setCustomerIDs] メソッド（ECID サービスの一部）を使用します。 詳細情報： [!DNL setCustomerIDs] はメソッドの [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=ja).

**クイックヒント：** 以前に上記のいずれかのメソッドを使用している場合、AAM [!UICONTROL Data Source] と [!UICONTROL Data Source] ID（別名「DPID」）。 に更新する場合 [!DNL setCustomerIDs]AAM [!UICONTROL Data Source]&#39;s &quot;[!UICONTROL Integration Code]「」という代わりに使用します。 それでも同じことを指し示しています [!UICONTROL Data Source] ただ単に別の識別子に過ぎません。 これは以下のビデオで示されます。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

次の節では、実装方法に基づいてDIL8.0 に更新するための手順と推奨事項を示します。

## Adobe Experience PlatformタグのDIL8.0 に更新しました。 {#updating-to-dil-in-experience-platform-launch}

DIL8.0 への更新の基本手順

1. 8.0 より前のDILを使用している場合は、アップグレードする前にAAM拡張機能のDIL設定に移動し、使用している高度なオプション（後の手順で使用）を控えておきます。
1. AAM拡張機能をバージョン 8.0 以降に更新します。
1. Experience CloudID サービス拡張機能がバージョン 3.3.0 以降であることを確認します。
1. 非推奨 ( 例： `disableIDSyncs`) が 8.0 より前のAAM拡張機能にあった場合、またはDIL用のカスタムコードにある場合は、ECID 拡張で ECID メソッドを有効にします。

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. 変更を公開します。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## AdobeDTM のDIL8.0 への更新 {#updating-to-dil-in-adobe-dtm}

1. AAMツールをバージョン 8.0 以降に更新します。 このバージョン設定は、AAMツールの「一般」セクションにあります。
1. 非推奨 ( 例： `disableIDSyncs`) が 8.0 より前のAAMツールのDIL用カスタムコードに含まれていた場合、それらをメモしておき（ECID ツールに追加できるように）、カスタムから削除します。 [!DNL DIL code] をAAMツールに追加します。
1. Experience CloudID サービス拡張機能をバージョン 3.3.0 以降に更新します。
1. AAMツールのカスタムコードから削除した ECID ツールに詳細オプションを追加します。
1. 変更を公開

## DIL8.0 への更新 (AdobeなしTag Managementソリューション ) {#additional-resources}

ページ上でコードを直接更新する場合は、前述のように、メソッドや要素をDILから ECID に移動する必要がある場合を除き、古い項目を新しい項目に置き換えるだけです。 その場合は、DILーの場所にある古いメソッドまたは要素を、ECID の場所にある ECID のメソッドまたは要素に置き換えます。

同じことが、非Adobeのタグ管理者にも当てはまります。 そのタグ管理ソリューションに古いバージョンがある場合は、次の手順に従って、それを新しいコードに置き換えます。

1. DILライブラリを最新DIL（8.0 以降）に更新します — 現在、公共の場所では利用できないので、AdobeコンサルティングまたはAdobeカスタマーケアから最新のバージョンコードを入手する必要があります。 次に、古いDILライブラリコードを新しいDILライブラリコードに置き換え、次の手順に進みます（今すぐ停止しないでください。問題が発生する場合は、ha）。
1. インストール [!DNL ECID Service] または、既存のバージョンを 3.3.0 以降に更新します。 最新のExperience CloudID サービスリリースをダウンロードできます [アドビの GitHub ページから](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) または、Adobeコンサルタントにお問い合わせください。

1. カスタムコード内のDIL用の廃止されたメソッドまたは要素が、ECID メソッドに移動されたことを確認します。

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=ja)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
