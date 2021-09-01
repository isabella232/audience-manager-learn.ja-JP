---
title: グローバルデバイス ID の検証
description: デバイス広告識別子（iDFA、GAID、Roku IDなど）には、デジタル広告エコシステムで使用できるようにするために満たす必要のある書式設定標準があります。 現在、お客様やパートナーは、IDが正しくフォーマットされているかどうかの通知を受けることなく、任意の形式でGlobalデータソースにIDをアップロードできます。 この機能では、適切な形式を設定するためにグローバルデータソースに送信されるデバイスIDの検証を導入し、IDの形式が正しくない場合にエラーメッセージを表示します。 iDFA、Google Advertising、Roku IDの検証は、起動時にサポートされます。
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: a2bf5c6bdc7611cd6bc5d807e60ac6aa22d5c176
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 2%

---

# グローバルデバイス ID の検証 {#global-device-id-validation}

デバイス広告識別子（iDFA、GAID、Roku IDなど）には、デジタル広告エコシステムで使用できるようにするために満たす必要のある書式設定標準があります。 現在、お客様やパートナーは、IDが適切にフォーマットされているかどうかを通知されることなく、IDを任意の形式でGlobal [!UICONTROL data sources]にアップロードできます。 この機能では、適切な形式を設定するためにグローバル[!UICONTROL data sources]に送信されたデバイスIDの検証を導入し、IDが正しくフォーマットされていない場合にエラーメッセージを表示します。 起動時に[!DNL iDFA]、[!DNL Google Advertising]および[!DNL Roku IDs]の検証がサポートされます。

## 形式標準の概要 {#overview-of-format-standards}

次に、AAMで現在認識およびサポートされているグローバルデバイス広告IDプールを示します。 これらは共有[!UICONTROL Data Sources]として実装され、これらのプラットフォームのユーザーに結び付けられたデータを扱う任意の顧客またはデータパートナーが使用できます。

<table>
  <tr>
   <td>プラットフォーム </td>
   <td>AAM Data Source ID </td>
   <td>ID形式 </td>
   <td>AAM PID </td>
   <td>メモ </td>
  </tr>
  <tr>
   <td>Google Android(GAID)</td>
   <td>20914</td>
   <td>32桁の16進数。通常は8-4-4-4-12<em>の例です。97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>このIDは、生の/ハッシュ化されていない/変更されていない形式のリファレンスで収集する必要があります。 <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32桁の16進数。通常8-4-4-4-12 <em>として表されます。例：6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>このIDは、生の/ハッシュ化されていない/変更されていない形式のリファレンスで収集する必要があります。 <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku(RIDA)</td>
   <td>121963</td>
   <td>32桁の16進数。通常は8-4-4-4-12 <em>例</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em>と表示されます。</td>
   <td>11536</td>
   <td>このIDは、生の/ハッシュ化されていない/変更されていない形式のリファレンスで収集する必要があります。 <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft広告ID(MAID)</td>
   <td>389146</td>
   <td>英数字の文字列</td>
   <td>14593</td>
   <td>このIDは、生の/ハッシュ化されていない/変更されていないフォームのリファレンスで収集する必要があります。 <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英数字の例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>このIDは、生の/ハッシュ化されていない/変更されていない形式のリファレンスで収集する必要があります。 <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## アプリでの広告識別子の設定 {#setting-an-advertising-identifier-in-the-app}

アプリでの広告主IDの設定は、実際には2つの手順で行います。まず広告主IDを取得し、次にExperience Cloudに送信します。 これらの手順を実行するためのリンクを以下に示します。

1. IDの取得
   1. [!DNL Apple] に関する情報 [!DNL advertising ID] は、こちらを参 [照してください](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. [!DNL Android]開発者向けの[!DNL advertiser ID]の設定に関する情報は、[ここ](http://android.cn-mirrors.com/google/play-services/id.html)にあります。
1. SDKの[!DNL setAdvertisingIdentifier]メソッドを使用して、Experience Cloudに送信します。
   1. `setAdvertisingIdentifier`の使用に関する情報は、[!DNL iOS]と[!DNL Android]の両方に関する[ドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier)に記載されています。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCSエラーメッセージでの誤ったIDの表示  {#dcs-error-messaging-for-incorrect-ids}

誤ったグローバルデバイスID（IDFA、GAIDなど）がAudience Managerに対してリアルタイムに送信されると、ヒット時にエラーコードが返されます。 IDが[!DNL Apple IDFA]として送信され、大文字のみを含み、IDに小文字の「x」が含まれているので、返されるエラーの例を次に示します。

![エラー画像](assets/image_4_.png)

エラーコードのリストについては、[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code)を参照してください。

## グローバルデバイスIDのオンボーディング {#onboarding-global-device-ids}

グローバルデバイスIDのリアルタイム送信に加えて、IDに対して「[!DNL onboard]」（アップロード）データを送信することもできます。 このプロセスは、顧客IDに対してデータをオンボーディングする場合と同じですが（通常はキー/値のペアを使用して）、適切なデータソースIDを使用するだけで、データがグローバルデバイスIDに割り当てられます。 オンボーディングプロセスに関するドキュメントは、[ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)を参照してください。 使用しているプラットフォームに応じて、グローバル[!UICONTROL data source] IDを使用することを忘れないでください。

オンボーディングプロセスを通じて誤ったグローバルデバイスIDが送信された場合、エラーは[[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)に表示されます。

次に、そのレポートで発生するエラーの例を示します。

![エラー画像](assets/image_5_.png)
