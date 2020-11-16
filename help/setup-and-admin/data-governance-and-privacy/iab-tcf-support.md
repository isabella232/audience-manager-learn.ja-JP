---
title: Audience ManagerでのIAB TCF 2.0のサポート
description: Adobeは、オプトイン機能を通じて、またはIAB Transparency and Consent Framework 2.0(TCF 2.0)サポートへのAudience Managerプラグインを通じて、ユーザーのプライバシー選択を管理および伝える手段を提供します。 この記事は、IAB TCFへのAudience Managerプラグインの理解を助けるドキュメントと共に機能し、Adobeのオプトインオブジェクトおよび同意管理プロバイダ(CMP)との連携方法を説明します。
feature: data governance & privacy
topics: null
audience: implementer, architect
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
translation-type: tm+mt
source-git-commit: df8afb50ed3971dc47e6506d31a8222a7f488b25
workflow-type: tm+mt
source-wordcount: '1123'
ht-degree: 2%

---


# IAB TCF 2.0 Support in Audience Manager {#iab-tcf-support-in-audience-manager}

Adobeは、オプトイン機能を通じて、またはIAB Transparency and Consent Framework 2.0(TCF 2.0)サポートへのAudience Managerプラグインを通じて、ユーザーのプライバシー選択を管理および伝える手段を提供します。 この記事は、IAB TCFへのAudience Managerプラグインの理解を助けるドキュメントと共に機能し、Adobeのオプトインオブジェクトおよび同意管理プロバイダ(CMP)との連携方法を説明します。 IABの詳細については、https://www.iabeurope.eu/のWebサイトを参照してくだ [さい](https://www.iabeurope.eu/)。

## 最初の手順：ECIDのオプトインについて理解する {#first-step-understand-ecid-s-opt-in}

IAB TCFの使用方法を理解するには、まず、Experience CloudIDサービス(ECID)ライブラリに含まれる [!DNL Opt-in] 機能を理解する必要があります。 オプトインの動作に精通していない場合は、 [この役立つ記事を](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) 最初にご覧ください。 また、オプトインに関する [ドキュメントも参照してください](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)。 これらのリソースを調べたら、このページに戻って操作を続行します。

## The Audience Manager Plug-In for IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの動作方法に関する基本的な理解が得られたので、Audience Managerはサポートの上に重ねて配置できます。これは、オプトインオブジェクトへのプラグインを使用して行います。 [!DNL IAB Transparency and Consent Framework (TCF)]

IAB TCF用のAudience Managerプラグインは、オプトインの機能を拡張し、AAMのお客様は、IAB TCFに従って、ユーザーのプライバシー選択を評価し、尊重し、下流のパートナーに転送できます。 データコントローラ(Adobeのお客様として)とベンダ(DMP、DSP、SSP、Ad Serversなど)の標準を提供します。 を使用して、同意の場合の同意を理解できます。

## IAB TCFの有効化 {#enabling-iab-tcf}

以下の短いビデオに示すように、Adobe Experience Platform Launchを使用している場合は、IAB TCF用のAudience Managerプラグインを有効にするのが簡単です。これはシンプルなチェックボックスです。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

または、「起動」を使用しない場合は、を使用して、Experience Cloud訪問者 `isIabContext=true` をインスタンス化する際に有効にすることができます。 これによりIAB TCFフローが開始されます。つまり、IAB TCFを使用してIAB TC文字列のクエリにIAB TCFを使用して同意収集のための別のステップが追加され、オプトインに返され、Experience Cloudソリューションと通信します。

## IAB TC文字列 {#iab-tcf-consent-string}

IABが提供する規格の1つは「同意文字列」（別名「デイジービット」）です。これは実際には2つのリストを組み合わせたものです。

1. 目的： **何をすれば** 、同意が得られるのか。
1. ベンダ： **誰に同意を与えられる** ?

### 目的 {#purposes}

IAB TCF 2.0では、10の「目的」を持ち、ベンダーが同意を得る(訪問者のデータを使用してベンダーが実行できること)ことができます。 Adobe Audience Managerは10人全員を必要とせず、ベンダーの同意に加えて、次の目的の同意のみを必要とします。

* **目的1:** デバイス上に情報を保存/アクセスする。
* **目的10:** 製品の開発と改善
* **特別な目的1:** セキュリティの確保、不正の防止、デバッグ。

これはIAB TC文字列の最初の部分で、1と0と記録されています。これは、その目的/アクティビティが承認されたかどうかを示すものです。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1（セキュリティの確保、不正の防止、デバッグ）には常に同意するものとし、ユーザーは異議を唱えることはできません。

### ベンダ {#vendors}

IAB TC文字列のもう1つの部分は数百社のベンダーから成る長いリストです。そのため、サイトにタグを付け、どのベンダーを使用するかを訪問者にリストできます。 ベンダーはリスト上でその場所を維持します。 例えば、このリストのAdobe Audience Managerの仕入先番号は565です。 リスト内のその番号が「1」である場合は、Audience Managerはリストの前面から承認された目的を実行できます。 AAMのスポットに「0」がある場合、データとは何も行えません。

**IAB TCFを使用してこれらの目的とベンダーを選択したり、すべてのアクティビティを承認/却下したりするUIをAudience Managerするには、IAB TCFに登録されたCMPパートナーを使用するか、IAB TCFをサポートし、IAB TCFに登録されたCMPパートナーを作成する必要があります。**

## オプトイン：IABとAdobeソリューション間の変換 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCFを使用する利点の1つは、上記の標準的な目的によって、エンドユーザーはおそらく、Adobeソリューションのリストよりも、何を承認しているかを知ることができるということです。 エンドユーザーは、「Audience Managerの承認」や「製品の保存やアクセス」の意味がわからない場合があります [!DNL Target]が、「製品の開発と改善」を理解し、同意する方が簡単であると思われます。

Audience Managerを承認するには(例：AAMに「はい」の投票を行うためのオプトインの目的でのIABの翻訳)、上記の目的1と10は、エンドユーザーの同意を得る必要があります。 これらのいずれかが承認されない場合、またはベンダーが承認されない場合、AAMはピクセルが実行されず、cookieが設定されません。 多くのお客様は、「オールオアナッシング」UIをエンドユーザーに提供するだけで、Audience Manager(およびその他のExperience Cloudソリューション)の使用を許可または禁止することを選択できるのでご注意ください。

ドキュメントには [](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html) 、IAB用Audience ManagerプラグインのTCFフローがパブリッシャーと広告主の両方の使用例にどのように適用されるかを示す優れた情報がいくつか記載されています。

## IAB:同意の下流への送信 {#iab-sending-consent-downstream}

IAB TCF用Audience Managerプラグインを使用する場合、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ）ID同期にもユーザーの同意の選択が送信され、パートナーはユーザーの同意情報を持ち、それに基づいて行動できます。 この情報は、2つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [エンコードされた同意文字列]

ユーザーがIABコンテキストにあり、同意を行わない（または否定的な同意を行わない）場合、Audience ManagerはIAB TC文字列をまったく収集せず、呼び出しを削除することに注意してください。 だからその場合は…下流の同意は通らない。

## デモ{#demo}

次のビデオでは、IABのユーザー選択によるcookieとビーコンへの影響をECIDとソリューションから確認します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

IAB TCF 2.0向けAudience Managerプラグインの導入方法、テスト方法、使用例、ワークフローについて詳しくは、 [ドキュメントを参照してください](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
