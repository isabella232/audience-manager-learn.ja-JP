---
title: Audience ManagerでのIAB TCF 2.0のサポート
description: Adobeは、オプトイン機能とAudience Managerプラグインを通じてIAB Transparency and Consent Framework 2.0(TCF 2.0)のサポートにユーザーのプライバシー選択を管理および伝達する手段を提供します。 この記事は、IAB TCFへのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび同意管理プロバイダー(CMP)との連携方法を理解するのに役立つドキュメントと連携します。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1120'
ht-degree: 1%

---

# Audience ManagerでのIAB TCF 2.0のサポート {#iab-tcf-support-in-audience-manager}

Adobeは、オプトイン機能とAudience Managerプラグインを通じてIAB Transparency and Consent Framework 2.0(TCF 2.0)のサポートにユーザーのプライバシー選択を管理および伝達する手段を提供します。 この記事は、IAB TCFへのAudience Managerプラグインと、Adobeのオプトインオブジェクトおよび同意管理プロバイダー(CMP)との連携方法を理解するのに役立つドキュメントと連携します。 IABの詳細については、Webサイト([https://www.iabeurope.eu/](https://www.iabeurope.eu/))を参照してください。

## 最初の手順：ECIDのオプトインについて {#first-step-understand-ecid-s-opt-in}

IAB TCFの使用方法を理解するには、まず、Experience CloudIDサービス(ECID)ライブラリの一部である[!DNL Opt-in]機能について理解する必要があります。 オプトインの仕組みに慣れていない場合は、まず[この役に立つ記事](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)を参照してください。 また、オプトインの[ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html)も参照してください。 これらのリソースを確認したら、このページに戻って続行します。

## IAB TCF用Audience Managerプラグイン {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの動作に関する基本的な理解ができたので、Audience Managerは[!DNL IAB Transparency and Consent Framework (TCF)]サポートにレイヤー化できます。このサポートは、プラグインを介してオプトインオブジェクトに組み込みます。

IAB TCF用Audience Managerプラグインは、オプトインの機能を拡張し、AAMのお客様がIAB TCFに従ってユーザーのプライバシー選択を評価、遵守、ダウンストリームパートナーに転送できるようにします。 データ管理者(Adobeのお客様)とベンダー(DMP、DSP、SSP、Ad Serverなど)の標準を提供します。 を使用して、同意の場面全体で同意を理解できます。

## IAB TCFの有効化 {#enabling-iab-tcf}

Adobe Experience Platform Launchを使用している場合は、IAB TCF用Audience Managerプラグインの有効化が簡単になります。これは、以下の短いビデオに示すように、シンプルなチェックボックスです。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

また、Launchを使用していない場合は、`isIabContext=true`を使用して、Experience Cloud訪問者をインスタンス化する際に有効にできます。 これにより、IAB TCFフローが開始され、つまり、IAB TCFを使用してIAB TC文字列に対するクエリを実行し、オプトインに返します。その後、Experience Cloudソリューションと通信します。

## IAB TC文字列 {#iab-tcf-consent-string}

IABが提供する標準の1つは、「同意文字列」（「DaisyBit」とも呼ばれます）です。これは、実際には次の2つのリストがまとめられています。

1. 目的：**同意を得る方法は何ですか。**
1. ベンダー：**誰に**&#x200B;同意を与えますか？

### 目的 {#purposes}

IAB TCF 2.0では、同意を収集する「目的」が10個あります（ベンダーは訪問者のデータを使用して何をおこなうことができます）。 Adobe Audience Managerは、ベンダーの同意に加え、10個すべてを必要とせず、次の目的に対する同意のみを必要とします。

* **目的1:** デバイス上に情報を保存し、その情報にアクセスする
* **目的10:** 製品の開発と改善
* **特別な目的1:** セキュリティの確保、不正の防止、デバッグ。

これはIAB TC文字列の最初の部分で、1と0として記録され、その目的/アクティビティが承認されたかどうかを示します。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1（セキュリティの確保、不正の防止、デバッグ）には常に同意するものとし、ユーザーは異議を唱えることはできません。

### ベンダー {#vendors}

IAB TC文字列の別の部分は、数百のベンダーの長いリストです。これにより、訪問者は、サイトにタグを持ち、使用するベンダーを選択できる、該当するベンダーのリストを表示できます。 ベンダーはリスト上の自分の場所を維持します。 例えば、このリストのAdobe Audience Managerのベンダー番号は565です。 リスト内のその番号が「1」である場合、Audience Managerはリストの前から承認された目的を実行できます。 AAMスポットに「0」がある場合、データは処理されません。

**IAB TCFを使用してこれらの目的とベンダーを選択するUIを提供したり、すべてのアクティビティを承認/不承認するには、IAB TCFに登録されたCMPパートナーまたはIAB TCFをサポートしIAB TCFに登録されたCMPパートナーを使用する必要があります。**

## オプトイン：IABとAdobeソリューション間の翻訳 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCFを使用する利点の1つは、上記の標準的な目的により、エンドユーザーがAdobeソリューションのリストよりも承認内容のアイデアを得られる可能性が高いということです。 エンドユーザーは、Audience Managerや[!DNL Target]を「承認」する意味がわからない場合がありますが、「デバイス上での情報の保存やアクセス」や「製品の開発と改善」を理解し、同意を得るのが容易です。

Audience Managerが承認される(つまり、オプトイン用のIAB目的の翻訳でAAMに「はい」の投票を行うために)には、上記の目的1および10がエンドユーザーから同意を得る必要があります。 これらのいずれかが承認されない場合、またはベンダーが承認されない場合、AAMはピクセル実行またはcookieの設定を実行しません。 また、多くのお客様は、エンドユーザーに対して「オールオアナッシング」のUIを提供するだけで、当然、Audience Manager(および他のExperience Cloudソリューション)の使用を許可または禁止することを選択することもできます。

[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en)には、IAB TCF用Audience Managerプラグインのフローがパブリッシャーと広告主の両方のユースケースにどのように適用されるかに関する優れた情報が記載されています。

## IAB:同意のダウンストリーム送信 {#iab-sending-consent-downstream}

IAB TCF用Audience Managerプラグインを使用すると、ユーザーの同意選択内容も、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ）ID同期に送信され、パートナーはユーザー同意情報を持ち、それに基づいて行動できます。 この情報は、次の2つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [エンコードされたコンセントストリング]

注意点は、ユーザーがIABコンテキストに属し、同意を提供しない（または否定的な同意を提供する）場合、Audience ManagerはIAB TC文字列をまったく収集せず、呼び出しを破棄することです。 だから、その場合は…同意は下流で渡さない。

## Demo {#demo}

以下のビデオでは、ECIDおよびソリューションのcookieとビーコンがIABユーザーによる選択の影響を受ける仕組みを確認します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

実装およびテスト、使用例、ワークフローの方法など、IAB TCF 2.0のAudience Managerプラグインについて詳しくは、[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)を参照してください。
