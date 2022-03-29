---
title: グローバルデバイス ID の検証
description: 適切な形式を設定するためにグローバルデータソースに送信されるデバイス ID の検証と、ID が正しくフォーマットされていない場合のエラーメッセージについて説明します。
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 1%

---

# グローバルデバイス ID の検証 {#global-device-id-validation}

デバイス広告識別子 (iDFA、GAID、Roku ID) には、デジタル広告エコシステムで使用できるようにするために満たす必要のある形式設定標準が含まれています。 現在、お客様およびパートナーは、ID が適切に形式設定されているかどうかを通知されることなく、任意の形式で ID をグローバルデータソースにアップロードできます。 この機能では、適切な形式を設定するためにグローバルデータソースに送信されるデバイス ID の検証が導入され、ID の形式が正しくない場合にエラーメッセージが表示されます。 の検証をサポートします。 [!DNL iDFA], [!DNL Google Advertising] および [!DNL Roku IDs] 起動時に

## 形式標準の概要 {#overview-of-format-standards}

次に、AAMで現在認識およびサポートされているグローバルデバイス広告 ID プールを示します。 これらは共有として実装されます [!UICONTROL Data Sources] これらのプラットフォームのユーザーに結び付けられたデータを操作する任意の顧客またはデータパートナーが使用できる

<table>
  <tr>
   <td>プラットフォーム </td>
   <td>AAM Data Source ID </td>
   <td>ID 形式 </td>
   <td>AAM PID </td>
   <td>メモ </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>20914</td>
   <td>32 桁の 16 進数。通常は8-4-4-4-12と表示されます<em>例、 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>この ID は、生の、ハッシュ化されていない、または変更されていない形式の参照で収集する必要があります。 <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32 桁の 16 進数。通常は8-4-4-4-12と表示されます <em>例： 6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>この ID は、生の、ハッシュ化されていない、または変更されていない形式の参照で収集する必要があります。 <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku(RIDA)</td>
   <td>121963</td>
   <td>32 桁の 16 進数。通常は8-4-4-4-12と表示されます <em>例：</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>この ID は、生の、ハッシュ化されていない、または変更されていない形式の参照で収集する必要があります。 <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID (MAID)</td>
   <td>389146</td>
   <td>英数字の文字列</td>
   <td>14593</td>
   <td>この ID は、生の、ハッシュ化されていない、または変更されていない形式の参照で収集する必要があります。 <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英数字の例、7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>この ID は、生の、ハッシュ化されていない、または変更されていない形式の参照で収集する必要があります。 <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## アプリでの広告識別子の設定 {#setting-an-advertising-identifier-in-the-app}

アプリでの広告主 ID の設定は、実際には 2 つの手順で行います。まず広告主 ID を取得し、次にExperience Cloudに送信します。 これらの手順を実行するためのリンクを以下に示します。

1. ID の取得
   1. [!DNL Apple] に関する情報 [!DNL advertising ID] は、 [ここ](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. の設定に関する情報 [!DNL advertiser ID] 対象 [!DNL Android] 開発者が見つかる [ここ](http://android.cn-mirrors.com/google/play-services/id.html).
1. を使用してExperience Cloudに送信します。 [!DNL setAdvertisingIdentifier] SDK のメソッド
   1. 使用に関する情報 `setAdvertisingIdentifier` が [ドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) 両方 [!DNL iOS] および [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## 間違った ID に対する DCS エラーメッセージ  {#dcs-error-messaging-for-incorrect-ids}

Audience Managerに対して誤ったグローバルデバイス ID（IDFA、GAID など）がリアルタイムに送信されると、ヒット時にエラーコードが返されます。 次に、ID が [!DNL Apple IDFA]：大文字のみを含める必要があり、ID には小文字の「x」が含まれます。

![エラー画像](assets/image_4_.png)

詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) エラーコードのリスト。

## グローバルデバイス ID のオンボーディング {#onboarding-global-device-ids}

グローバルデバイス ID をリアルタイムで送信する以外に、[!DNL onboard]」 ID に対するデータのアップロードもおこないます。 このプロセスは、（通常はキー/値のペアを使用して）顧客 ID に対してデータをオンボーディングする場合と同じですが、データがグローバルデバイス ID に割り当てられるように、適切なデータソース ID を使用するだけです。 オンボーディングプロセスに関するドキュメントについては、 [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). 使用しているプラットフォームに応じて、グローバルデータソース ID を忘れずに使用してください。

オンボーディングプロセスで間違ったグローバルデバイス ID が送信された場合、 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

次に、そのレポートで発生するエラーの例を示します。

![エラー画像](assets/image_5_.png)
