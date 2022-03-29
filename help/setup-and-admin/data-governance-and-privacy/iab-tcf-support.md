---
title: IAB TCF 2.0 のサポート
description: IAB TCF へのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび同意管理プロバイダー (CMP) との連携について説明します。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 2%

---

# Audience Managerでの IAB TCF 2.0 のサポート {#iab-tcf-support-in-audience-manager}

Adobeは、オプトイン機能とAudience Managerプラグインを通じて IAB Transparency and Consent Framework 2.0(TCF 2.0) のサポートにユーザーのプライバシー選択を管理および伝達する手段を提供します。 この記事はドキュメントと共に使用して、IAB TCF へのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび同意管理プロバイダー (CMP) との連携の仕組みを理解できます。 IAB の詳細については、Web サイト ( [https://www.iabeurope.eu/](https://www.iabeurope.eu/).

## 最初の手順：Experience CloudID オプトインについて {#first-step-understand-ecid-s-opt-in}

IAB TCF の使用方法を理解するには、まず理解する必要があります [!DNL Opt-in] 機能 (Experience CloudID サービス (ECID) ライブラリの一部 ) に含まれています。 オプトインの仕組みがわからない場合は、 [この役立つ記事はこちら](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) 1 つ目は また、オプトイン [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja). これらのリソースを確認したら、このページに戻って続行します。

## IAB TCF 用のAudience Managerプラグイン {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの仕組みに関する基本的な知識ができたので、Audience Managerをその上に重ねることができます [!DNL IAB Transparency and Consent Framework (TCF)] サポート（プラグインを介してオプトインオブジェクトに送信されます）。

IAB TCF 用Audience Managerプラグインは、オプトインの機能を拡張し、AAMのお客様は、IAB TCF に従って、ユーザーのプライバシー選択を評価、尊重し、ダウンストリームパートナーに転送できます。 データ管理者 (Adobeのお客様 ) とベンダー (DMP、DSP、SSP、広告サーバーなど ) の標準を提供します。 を使用して、同意ランドスケープ全体にわたる同意を理解できます。

## IAB TCF の有効化 {#enabling-iab-tcf}

Adobe Experience Platform Launchを使用している場合は、IAB TCF 用のAudience Managerプラグインを簡単に有効にできます。これは、以下の短いビデオに示すように、シンプルなチェックボックスです。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

または、Launch を使用していない場合、 `isIabContext=true` を有効にして、訪問者をインスタンス化する際にExperience Cloudを有効にします。 これにより、IAB TCF フローが開始され、つまり、IAB TCF を使用して IAB TC 文字列に対するクエリを実行し、オプトインに返送します。その後、Experience Cloudソリューションと通信します。

## IAB TC 文字列 {#iab-tcf-consent-string}

IAB が提供する標準の 1 つは、「同意文字列」（「DaisyBit」とも呼ばれます）です。これは、実際には次の 2 つのリストがまとめられています。

1. 目的： **What** 同意が与えられているか
1. ベンダー： **対象ユーザー** に対する同意が得られるか

### 目的 {#purposes}

IAB TCF 2.0 では、同意を収集するための「目的」が 10 個あります（ベンダーは訪問者のデータを使用して何をおこなうことができます）。 Adobe Audience Managerは 10 人全員を必要とせず、ベンダーの同意に加えて、次の目的に対する同意のみを必要とします。

* **目的 1:** デバイス上に情報を保存し、その情報にアクセスする
* **目的 10:** 製品の開発と改善
* **特別な目的 1:** セキュリティの確保、不正の防止、デバッグ。

これは IAB TC 文字列の最初の部分で、1 と 0 として記録され、その目的/アクティビティが承認されたかどうかを示します。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1（セキュリティの確保、不正の防止、デバッグ）には常に同意するものとし、ユーザーは異議を唱えることはできません。

### ベンダー {#vendors}

IAB TC 文字列の別の部分は、数百のベンダーの長いリストなので、サイトにタグを付け、使用するベンダーを選択できる、該当するベンダーのリストを訪問者に表示できます。 ベンダーはリスト上の自分の場所を維持します。 例えば、このリストのAdobe Audience Managerのベンダー番号は 565 です。 リスト内のその数が「1」である場合、Audience Managerはリストの前から承認された目的を実行できます。 AAM Spot に「0」が設定されている場合、そのデータは処理されません。

**顧客が IAB TCF を使用してこれらの目的やベンダーを選択したり、すべてのアクティビティを承認/却下したりするための UI を提供するには、IAB TCF に登録された CMP パートナーを使用するか、IAB TCF をサポートする CMP パートナーを作成する必要があります。**

## オプトイン：IAB アプリケーションとAdobeアプリケーション間の翻訳 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCF を使用する利点の 1 つは、上記の標準的な目的により、エンドユーザーはおそらくAdobeソリューションのリストよりも承認内容の概念を把握できることです。 エンドユーザーは、Audience Manager、または [!DNL Target]ただし、「デバイス上に情報を保存してアクセスする」または「製品を開発して改善する」は、おそらく理解し、同意するのに簡単です。

Audience Managerが承認される ( つまり、オプトインのための IAB 目的の翻訳でAAMに「はい」の投票を行う ) には、上記の目的 1 および 10 で、エンドユーザーから同意を得る必要があります。 これらのいずれかが承認されていない場合、またはベンダーが承認されていない場合、AAMはピクセルを実行せず、cookie を設定しません。 また、多くのお客様は、エンドユーザーに対して「すべてまたは何も」の UI を提供するだけで、もちろん、Audience Manager( および他のExperience Cloudソリューション ) の使用を許可または禁止することを選択していることを知っておくと良いです。

には素晴らしい情報がいくつかあります [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en) IAB TCF 用Audience Managerプラグインのフローが Publisher と Advertiser の両方のユースケースにどのように適用されるかについて説明します。

## IAB:同意のダウンストリーム送信 {#iab-sending-consent-downstream}

IAB TCF 用Audience Managerプラグインを使用すると、ユーザーの同意選択肢がプラットフォームレベル（サードパーティ）の ID 同期にも送信され、グローバルベンダーリストに存在するパートナーはユーザー同意情報を持ち、それに基づいても機能します。 この情報は、次の 2 つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [エンコードされたコンセントストリング]

注意点は、Audience Managerが IAB コンテキストに属し、同意を提供しない（または否定的な同意を提供する）場合、ユーザーは IAB TC 文字列をまったく収集せず、呼び出しを破棄するということです。 その場合は…下流の同意は通過しません。

## Demo {#demo}

以下のビデオでは、ECID およびソリューションの cookie とビーコンが IAB ユーザーによる選択の影響を受けるしくみについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

実装およびテスト方法、使用例、ワークフローなど、IAB TCF 2.0 のAudience Managerプラグインについて詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html).
