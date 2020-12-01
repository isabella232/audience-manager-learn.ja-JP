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


# Audience Manager{#iab-tcf-support-in-audience-manager}でのIAB TCF 2.0のサポート

Adobeは、オプトイン機能を通じて、またはIAB Transparency and Consent Framework 2.0(TCF 2.0)サポートへのAudience Managerプラグインを通じて、ユーザーのプライバシー選択を管理および伝える手段を提供します。 この記事は、IAB TCFへのAudience Managerプラグインの理解を助けるドキュメントと共に機能し、Adobeのオプトインオブジェクトおよび同意管理プロバイダ(CMP)との連携方法を説明します。 IABの詳細については、[https://www.iabeurope.eu/](https://www.iabeurope.eu/)のWebサイトを参照してください。

## 最初の手順：ECIDのオプトインについて{#first-step-understand-ecid-s-opt-in}

IAB TCFの使い方を理解するには、まず[!DNL Opt-in]機能を理解する必要があります。これはExperience CloudIDサービス(ECID)ライブラリの一部です。 オプトインの動作に詳しくない場合は、[最初に](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)この役立つ記事を参照してください。 また、オプトイン[ドキュメント](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)も確認する必要があります。 これらのリソースを調べたら、このページに戻って操作を続行します。

## IAB TCFのAudience Managerプラグイン{#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの動作に関する少なくとも基本的な理解ができたので、Audience Managerは[!DNL IAB Transparency and Consent Framework (TCF)]サポートに重ね合わせることができます。これは、オプトインオブジェクトへのプラグインを介して行われます。

IAB TCF用のAudience Managerプラグインは、オプトインの機能を拡張し、AAMのお客様は、IAB TCFに従って、ユーザーのプライバシー選択を評価し、尊重し、下流のパートナーに転送できます。 データコントローラ(Adobeのお客様として)とベンダ(DMP、DSP、SSP、Ad Serversなど)の標準を提供します。 を使用して、同意の場合の同意を理解できます。

## IAB TCF {#enabling-iab-tcf}を有効にする

以下の短いビデオに示すように、Adobe Experience Platform Launchを使用している場合は、IAB TCF用のAudience Managerプラグインを有効にするのが簡単です。これはシンプルなチェックボックスです。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

または、「起動」を使用しない場合は、Experience Cloud訪問者をインスタンス化する際に、`isIabContext=true`を使用して有効にすることができます。 これによりIAB TCFフローが開始されます。つまり、IAB TCFを使用してIAB TC文字列のクエリにIAB TCFを使用して同意収集のための別のステップが追加され、オプトインに返され、Experience Cloudソリューションと通信します。

## IAB TC文字列{#iab-tcf-consent-string}

IABが提供する規格の1つは「同意文字列」（別名「デイジービット」）です。これは実際には2つのリストを組み合わせたものです。

1. 目的：**何をすべきか**&#x200B;に同意を与えるのか？
1. ベンダ：**誰**&#x200B;に同意を与えているか

### 目的{#purposes}

IAB TCF 2.0では、10の「目的」を持ち、ベンダーが同意を得る(訪問者のデータを使用してベンダーが実行できること)ことができます。 Adobe Audience Managerは10人全員を必要とせず、ベンダーの同意に加えて、次の目的の同意のみを必要とします。

* **目的1：デバイス上の情報の** 保存/アクセス
* **目的10:** 製品の開発と改善
* **特別な目的1：セキュリティの** 確保、不正の防止、デバッグの実施。

これはIAB TC文字列の最初の部分で、1と0と記録されています。これは、その目的/アクティビティが承認されたかどうかを示すものです。

>[!NOTE]
>
>IAB 規制に従い、特別な目的 1（セキュリティの確保、不正の防止、デバッグ）には常に同意するものとし、ユーザーは異議を唱えることはできません。

### ベンダ{#vendors}

IAB TC文字列のもう1つの部分は数百社のベンダーから成る長いリストです。そのため、サイトにタグを付け、どのベンダーを使用するかを訪問者にリストできます。 ベンダーはリスト上でその場所を維持します。 例えば、このリストのAdobe Audience Managerの仕入先番号は565です。 リスト内のその番号が「1」である場合は、Audience Managerはリストの前面から承認された目的を実行できます。 AAMのスポットに「0」がある場合、データとは何も行えません。

**IAB TCFを使用してこれらの目的とベンダーを選択したり、すべてのアクティビティを承認/却下したりするUIをAudience Managerするには、IAB TCFに登録されたCMPパートナーを使用するか、IAB TCFをサポートし、IAB TCFに登録されたCMPパートナーを作成する必要があります。**

## オプトイン：IABとAdobeソリューション間の変換{#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCFを使用する利点の1つは、上記の標準的な目的によって、エンドユーザーはおそらく、Adobeソリューションのリストよりも、何を承認しているかを知ることができるということです。 エンドユーザーは、「Audience Managerの承認」や[!DNL Target]の意味が分からない場合がありますが、「Store and/or access information on a device」や「Develop and prove products」を理解し、同意する方が簡単でしょう。

Audience Managerを承認するには(例：AAMに「はい」の投票を行うためのオプトインの目的でのIABの翻訳)、上記の目的1と10は、エンドユーザーの同意を得る必要があります。 これらのいずれかが承認されない場合、またはベンダーが承認されない場合、AAMはピクセルが実行されず、cookieが設定されません。 多くのお客様は、「オールオアナッシング」UIをエンドユーザーに提供するだけで、Audience Manager(およびその他のExperience Cloudソリューション)の使用を許可または禁止することを選択できるのでご注意ください。

[ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html)には、IAB TCF用Audience Managerプラグインのフローがパブリッシャーと広告主の両方の使用例にどのように適用されるかに関する優れた情報が記載されています。

## IAB:同意を下流に送信{#iab-sending-consent-downstream}

IAB TCF用Audience Managerプラグインを使用する場合、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ）ID同期にもユーザーの同意の選択が送信され、パートナーはユーザーの同意情報を持ち、それに基づいて行動できます。 この情報は、2つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [エンコードされた同意文字列]

ユーザーがIABコンテキストにあり、同意を行わない（または否定的な同意を行わない）場合、Audience ManagerはIAB TC文字列をまったく収集せず、呼び出しを削除することに注意してください。 だからその場合は…下流の同意は通らない。

## デモ{#demo}

次のビデオでは、IABのユーザー選択によるcookieとビーコンへの影響をECIDとソリューションから確認します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

IAB TCF 2.0向けAudience Managerプラグインの実装方法、テスト方法、使用例、ワークフローなど、詳細については、[ドキュメント](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)を参照してください。
