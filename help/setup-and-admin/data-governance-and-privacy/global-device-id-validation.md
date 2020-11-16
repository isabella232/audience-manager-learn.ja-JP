---
title: グローバルデバイス ID の検証
description: デバイスの広告識別子(iDFA、GAID、Roku ID)には、デジタル広告エコシステムで使用できるようにするために満たす必要がある形式標準が含まれています。 現在、お客様とパートナーは、IDが正しく形式設定されているかどうかを通知されることなく、任意の形式でグローバルデータソースにIDをアップロードできます。 この機能では、適切な形式設定を行うためにグローバルデータソースに送信されるデバイスIDの検証が導入され、IDの形式が正しくない場合にエラーメッセージが表示されます。 起動時のiDFA、Google Advertising、Roku IDの検証をサポートします。
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 2%

---


# グローバルデバイス ID の検証 {#global-device-id-validation}

デバイスの広告識別子(iDFA、GAID、Roku ID)には、デジタル広告エコシステムで使用できるようにするために満たす必要がある形式標準が含まれています。 現在、お客様とパートナーは、IDが正しくフォーマットされているかどうかを通知さ [!UICONTROL data sources] れることなく、任意のフォーマットでアドビのグローバルにIDをアップロードできます。 この機能では、適切な形式設定を行うためにグローバルに送信されるデバイスIDの検証が導入され、IDの形式が正しくない場合 [!UICONTROL data sources] にエラーメッセージが表示されます。 アドビは、の検証をサポートし [!DNL iDFA]ます。ま [!DNL Google Advertising][!DNL Roku IDs] た、起動時にも検証をサポートします。

## フォーマット標準の概要 {#overview-of-format-standards}

次に、AAMで現在認識され、サポートされているグローバルなデバイス広告IDプールを示します。 これらは共有として実装され、これらのプラットフォームのユーザー [!UICONTROL Data Sources] に結び付けられたデータを操作する任意の顧客またはデータパートナーが使用できます。

<table>
  <tr>
   <td>プラットフォーム </td>
   <td>AAMデータソースID </td>
   <td>ID形式 </td>
   <td>AAM PID </td>
   <td>メモ </td>
  </tr>
  <tr>
   <td>Google Android(GAID)</td>
   <td>20914</td>
   <td>32の16進数。通常は8-4-4-4-12<em>の例、97987bca-ae59-4c7d-94ba-ee4f19ab8c21のように表示されます。<br/> </em> </td>
   <td>1352</td>
   <td>このIDは、生の/非ハッシュ/変更のないフォームリファレンスで収集する必要があります。 - <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32の16進数。通常は8-4-4-12 <em>例、6D92078A-8246-4BA4-AE5B-76104861E7DCとして表示されます。<br /> </em> </td>
   <td>3560</td>
   <td>このIDは、生の/非ハッシュ/変更のないフォームリファレンスで収集する必要があります。 - <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>六(RIDA)</td>
   <td>121963</td>
   <td>32の16進数。通常は8-4-4-12 <em>の例</em> です。 <em>fcb2a29c-315a-5e6b-bcfd-d889ba19ada</em></td>
   <td>11536</td>
   <td>このIDは、生の/非ハッシュ/変更のないフォームリファレンスで収集する必要があります。 - <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft広告ID （メイド）</td>
   <td>389146</td>
   <td>英数字の文字列</td>
   <td>14593</td>
   <td>このIDは、生の/非ハッシュ/変更のないフォームリファレンス — https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager. <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid"></a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">advertisingidhttps://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspxで収集する必要があります。</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英数字文字列の例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>このIDは、生の/非ハッシュ/変更のないフォームリファレンスで収集する必要があります。 - <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## アプリでの広告IDの設定 {#setting-an-advertising-identifier-in-the-app}

アプリで広告主IDを設定する手順は、まず広告主IDを取得し、次にExperience Cloudに送信する2つの手順です。 これらの手順を実行する際のリンクを以下に示します。

1. IDを取得する
   1. [!DNL Apple] に関する情報は、こち [!DNL advertising ID] らを参照し [てください](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 開発者向けにの設定に関する情報 [!DNL advertiser ID] は、こちらを参照してく [!DNL Android] ださい [](http://www.androiddocs.com/google/play-services/id.html)。
1. SDKの [!DNL setAdvertisingIdentifier] メソッドを使用して、Experience Cloudに送信します
   1. の使用に関する情報 `setAdvertisingIdentifier` は、との両方の [ドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) に記載され [!DNL iOS] てい [!DNL Android]ます。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCSエラーメッセージ（正しくないID）  {#dcs-error-messaging-for-incorrect-ids}

誤ったグローバルデバイスID（IDFA、GAIDなど）がAudience Managerに対してリアルタイムで送信されると、ヒット時にエラーコードが返されます。 次の例は、IDが大文字のみを含む [!DNL Apple IDFA]必要があり、IDに小文字の「x」が含まれているために返されるエラーの例です。

![エラー画像](assets/image_4_.png)

エラーコードのリストについては、 [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) を参照してください。

## Onboarding Global Device IDs {#onboarding-global-device-ids}

グローバル・デバイスIDのリアルタイム送信に加えて、IDに対してデータを「[!DNL onboard]」（アップロード）することもできます。 このプロセスは、顧客IDに対して（通常はキー/値のペアを介して）データをオンボーディングする場合と同じですが、適切なデータソースIDを使用するだけで、データがグローバルデバイスIDに割り当てられます。 オンボーディングプロセスに関するドキュメントは、 [ドキュメントを参照してください](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)。 使用しているプラットフォームに応じて、グローバル [!UICONTROL data source] IDを忘れずに使用してください。

正しくないグローバルデバイスIDがオンボーディングプロセスを通じて送信されると、エラーがに表示され [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)ます。

以下は、そのレポートから発生するエラーの例です。

![エラー画像](assets/image_5_.png)