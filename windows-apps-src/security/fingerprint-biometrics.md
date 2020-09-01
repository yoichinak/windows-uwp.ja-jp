---
title: 指紋生体認証
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリに指紋生体認証を追加する方法について説明します。
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, セキュリティ
ms.localizationpriority: medium
ms.openlocfilehash: bcb82280d80467157faa8aa5195ad7b9d9a7336b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167216"
---
# <a name="fingerprint-biometrics"></a>指紋生体認証




この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリに指紋生体認証を追加する方法について説明します。 特定の操作に対してユーザーの同意を得る必要がある場合は、指紋認証の要求を含めると、アプリのセキュリティを高めることができます。 たとえば、アプリ内購入を承認する前や制限されたリソースにアクセスする前に指紋認証を要求できます。 指紋認証は、 [**Windows**](/uwp/api/Windows.Security.Credentials.UI)の[**Userconの verifier**](/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier)クラスを使用して管理されます。

## <a name="check-the-device-for-a-fingerprint-reader"></a>デバイスに指紋リーダーがあるかどうかをチェックする


デバイスに指紋リーダーがあるかどうかを調べるには、[**UserConsentVerifier.CheckAvailabilityAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.checkavailabilityasync) メソッドを呼び出します。 デバイスで指紋認証がサポートされている場合も、アプリでは、指紋認証を有効または無効にする設定オプションをユーザーに提供する必要があります。

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>同意を求めて、結果を返す


指紋のスキャンによってユーザーの同意を求めるには、[**UserConsentVerifier.RequestVerificationAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync) メソッドを呼び出します。 指紋認証を行うには、あらかじめユーザーが指紋データベースに指紋の "署名" を追加している必要があります。

[**UserConsentVerifier.RequestVerificationAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync) を呼び出すと、ユーザーに指紋のスキャンを求めるモーダル ダイアログが表示されます。 **UserConsentVerifier.RequestVerificationAsync** メソッドには、次の図に示すように、モーダル ダイアログの一部として表示されるメッセージを指定できます。

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```