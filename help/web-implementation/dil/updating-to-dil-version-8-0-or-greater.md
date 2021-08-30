---
title: Adobe Audience ManagerDILバージョン8.0以降への更新
description: この記事では、Adobe Audience Manager(AAM)Data Integration Library(DIL)コードをバージョン8.0以降に更新する手順と推奨事項について説明します。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」のDIL実装を指し、Adobeタグマネージャーソリューションを使用しないDTM、Launch by Adobe、実装を扱います。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# Adobe Audience ManagerDILバージョン8.0以降への更新 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

この記事では、Adobe Audience Manager(AAM)の[!DNL Data Integration Library](DIL)コードをバージョン8.0以降に更新する手順と推奨事項について説明します。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」のDIL実装を指し、Adobeタグマネージャーソリューションを使用しないDTM、Launch by Adobe、実装を扱います。

## 概要 {#overview}

Audience Managerの[!DNL Data Integration Library](DIL)コードを使用すると、Webサイト*にAAMを実装できます。 以前のバージョンのDILを実装する場合、AdobeのExperience CloudIDサービス(ECID)も実装する必要はありませんでした（ただし、実際には非常に良いアイデアでした）。 DILバージョン8.0以降は、ECIDバージョン3.3以降に強く依存します。 ECID 3.3を使用せずにDIL8.0以降を実装した場合、または以前のバージョンを使用した場合、エラーが発生し、機能しません。 AAMの実装には複数の方法があるので、このページを作成して手順と推奨事項を示します。 以下に、これらの手順と推奨事項をプラットフォーム/実装方法別に示します。 DILの詳細については、[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)を参照してください。

* このページの説明に記載されているように、これは、Adobe Analyticsを持たないAAMのお客様が使用する、「クライアント側」のDIL実装のみをカバーします。 Adobe Analyticsがある場合は、AAMを実装するサーバー側転送方法を使用する必要があります。 このメソッドは、[ドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)で説明しています。

## 重複および非推奨の要素およびメソッド {#duplicate-and-deprecated-elements-and-methods}

以前のバージョンのDILとECIDには、重複するメソッド(DILとECIDの両方で同じ機能を実行するメソッド)があり、どちらを使用するかに関して混乱を招いていました。 通常は、両方を使用してマッチさせる必要があり、そのメッセージはお客様には伝えきれませんでした。 DIL8.0以降、これらの重複メソッドおよび要素はDILで非推奨となり、ECIDバージョンを使用することをお勧めします。

例：

* [!DNL DIL.create]を使用する場合、一部の要素が廃止され、代わりにECID要素を使用する必要があります。 これらの要素は、[[!DNL DIL.create] ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)で呼び出されます。
* [!DNL idSync]インスタンスレベルのメソッドも廃止され、メソッドの[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)で呼び出されました。

## 顧客IDとのID同期 {#id-syncing-with-a-customer-id}

AAMでは、マシン上のUUID（匿名の一意のユーザーID）を顧客IDと同期して、その顧客に関するオフラインデータをアップロードし、オンライン動作と関連付けて、顧客の理解を深めることができます。 以前は、次の2つの方法のいずれかで行われていました。

* [!DNL idSync]インスタンスレベルのメソッド
* [!DNL DIL.create]の[!DNL declaredId]要素

顧客IDとの同期にこれらの古い方法のいずれかを使用している場合は、ECIDサービスの一部である[!DNL setCustomerIDs]メソッドを使用するように更新することを強くお勧めします。 [!DNL setCustomerIDs]に関する詳細は、メソッドの[ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)を参照してください。

**クイックヒント：** 以前に上記のいずれかのメソッドを使用している場合、AAM [!UICONTROL Data Source] を [!UICONTROL Data Source] ID（別名「DPID」）で参照していました。[!DNL setCustomerIDs]に更新する場合は、代わりにAAM [!UICONTROL Data Source]の「[!UICONTROL Integration Code]」を使用する必要があります。 引き続き同じ[!UICONTROL Data Source]を指しますが、単に別の識別子になります。 以下のビデオで示します。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

次の節では、実装方法に基づいてDIL8.0に更新するための手順と推奨事項を示します。

## Adobe Experience Platform LaunchでのDIL8.0への更新 {#updating-to-dil-in-experience-platform-launch}

DIL8.0への更新の基本手順

1. 8.0より前のDILを使用している場合は、アップグレードする前にAAM拡張機能のDIL設定に移動し、使用している高度なオプション（後の手順で使用）を控えておきます
1. AAM拡張機能をバージョン8.0以降に更新する
1. Experience CloudIDサービス拡張機能がバージョン3.3.0以降であることを確認します。
1. 8.0より前のAAM拡張機能またはDIL用のカスタムコードにある、非推奨のメソッドや要素（「[!DNL disableIDSyncs]」など）に対して、ECID拡張機能でECIDメソッドを有効にします。

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL) iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) declaredId -> (ECID) setCustomerIDs

1. 変更の公開

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## AdobeDTMのDIL8.0への更新 {#updating-to-dil-in-adobe-dtm}

1. AAMツールをバージョン8.0以降に更新します。 このバージョン設定は、AAMツールの「一般」セクションにあります。
1. 8.0より前のAAM ToolのDIL用カスタムコードに含まれていた非推奨のメソッドや要素（「disableIDSyncs」など）については、それらをメモして（ECIDツールに追加できるように）、AAMツールのカスタム[!DNL DIL code]から削除します。
1. Experience CloudIDサービス拡張機能をバージョン3.3.0以降に更新する
1. AAMツールのカスタムコードから削除したECIDツールに高度なオプションを追加します。
1. 変更の公開

## DILタグ管理ソリューションを使用しないAdobe8.0への更新 {#additional-resources}

ページ上でコードを直接更新する場合は、前述のように、メソッドや要素をDILからECIDに移動する必要がある場合を除き、古い項目を新しい項目に置き換えるだけです。 その場合は、DILの場所にある古いメソッドまたは要素を、ECIDの場所にあるECIDメソッドまたは要素に置き換えます。

同じことが、Adobe以外のタグマネージャーにも当てはまります。 そのタグ管理ソリューションに古いバージョンがある場合は、次の手順に従って、新しいコードに置き換えます。

1. DILライブラリを最新DIL（8.0以降）に更新します — 現在公共の場所では利用できないので、AdobeコンサルティングまたはAdobeカスタマーケアから最新のバージョンコードを入手する必要があります。 次に、古いDILライブラリコードを新しいDILライブラリコードに置き換え、次の手順に進みます(今すぐ停止しないでください。
1. [!DNL ECID Service]をインストールするか、既存のバージョンを3.3.0以降に更新します。 最新のExperience CloudIDサービスリリース[は、アドビのGitHubページ](https://github.com/Adobe-Marketing-Cloud/id-service/releases)からダウンロードできます。 この問題に関するサポートが必要な場合は、[ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html)を参照するか、Adobeコンサルタントにお問い合わせください。

1. DIL用のカスタムコードにある非推奨のメソッドまたは要素が、ECIDメソッドに移動されたことを確認します。

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) declaredId -> (ECID) setCustomerIDs

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
