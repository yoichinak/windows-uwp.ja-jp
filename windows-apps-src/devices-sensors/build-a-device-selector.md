---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: デバイス セレクターのビルド
description: デバイス セレクターを作成すると、デバイスを列挙するときに、検索するデバイスを絞り込むことができるようになります。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 67d83a66687bb8719dc374a2a8a3e30eaac82c71
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684831"
---
# <a name="build-a-device-selector"></a>デバイス セレクターのビルド



**重要な API**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

デバイス セレクターを作成すると、デバイスを列挙するときに、検索するデバイスを絞り込むことができるようになります。 これにより、関連する結果のみを取得することができ、システムのパフォーマンスも向上します。 多くのシナリオでは、デバイス スタックからデバイス セレクターを取得します。 たとえば、USB 経由で検出したデバイスに [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) を使うとします。 これらのデバイス セレクターは高度なクエリ構文 (AQS) 文字列を返します。 AQS 形式を初めて使う場合は、「[プログラムでの高度なクエリ構文の使用](https://docs.microsoft.com/windows/desktop/search/-search-3x-advancedquerysyntax)」をご覧ください。

## <a name="building-the-filter-string"></a>フィルター文字列の作成

デバイスを列挙する必要があるにもかかわらず、提供されたデバイス セレクターを目的のシナリオで利用できないことがあります。 デバイス セレクターは、次の情報が含まれる AQS フィルター文字列です。 フィルター文字列を作成する前に、列挙するデバイスに関して、いくつかの重要な情報を知っておく必要があります。

-   目的のデバイスの [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)。 デバイスの列挙への **DeviceInformationKind** の影響について詳しくは、「[デバイスの列挙](enumerate-devices.md)」をご覧ください。
-   このトピックで説明されている、AQS フィルター文字列を作成する方法。
-   目的のプロパティ。 使用可能なプロパティは [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) によって異なります。 詳しくは、「[デバイス情報プロパティ](device-information-properties.md)」をご覧ください。
-   照会で経由するプロトコル。 ワイヤレスまたはワイヤード ネットワーク経由でデバイスを検索する場合にのみ必要です。 そのための方法について詳しくは、「[ネットワーク経由でデバイスを列挙する](enumerate-devices-over-a-network.md)」をご覧ください。

[  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API を使うときは、多くの場合、デバイス セレクターを目的のデバイスの種類と組み合わせます。 利用可能なデバイスの種類の一覧は、[**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) 列挙値で定義されています。 この組み合わせは、利用可能なデバイスを目的のデバイスの種類に限定するために役立ちます。 **DeviceInformationKind** を指定しない場合、つまり、使うメソッドに **DeviceInformationKind** パラメーターを渡さない場合、既定の種類は **DeviceInterface** です。

[  **Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API では、AQS の標準的な構文が使われますが、一部の演算子はサポートされていません。 フィルター文字列の作成に使えるプロパティの一覧については、「[デバイス情報プロパティ](device-information-properties.md)」をご覧ください。

**注意**  `{GUID} PID` 形式を使用して定義されたカスタムプロパティは、aqs フィルター文字列を構築するときには使用できません。 これは、プロパティの型が一般的な既知のプロパティ名から派生しているためです。

 

次の表は、AQS 演算子とそれがサポートするパラメーターの型の一覧です。

| ［演算子］                       | サポートされる型                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_等しい**                 | 文字列、ブール値、GUID、UInt16、UInt32                                       |
| **COP\_注の QUAL**              | 文字列、ブール値、GUID、UInt16、UInt32                                       |
| **COP\_**              | UInt16、UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16、UInt32                                                              |
| **COP\_(OREQUAL)**       | UInt16、UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16、UInt32                                                              |
| **COP\_値\_含まれています**       | 文字列、文字列配列、ブール値配列、GUID 配列、UInt16 配列、UInt32 配列 |
| **COP\_値\_NOTCONTAINS**    | 文字列、文字列配列、ブール値配列、GUID 配列、UInt16 配列、UInt32 配列 |
| **COP\_値\_STARTSWITH**     | String                                                                      |
| **COP\_値\_ENDSWITH**       | String                                                                      |
| **COP\_DOSWILDCARDS**          | サポートされていません。                                                               |
| **COP\_WORD\_EQUAL**           | サポートされていません。                                                               |
| **COP\_WORD\_STARTSWITH**      | サポートされていません。                                                               |
| **COP\_アプリケーション\_固有** | サポートされていません。                                                               |


> **ヒント**   **\_EQUAL**または**COP\_** に**NULL**を指定できます。 これは空のプロパティに変換されます。つまり、値は存在しません。 AQS では、空のかっこ \[\]を使用して**NULL**を指定します。

> **重要**  **COP\_値\_CONTAINS**および**COP\_値\_notcontains**演算子では、文字列と文字列配列とで動作が異なります。 文字列の場合、大文字と小文字を区別する検索が実行され、デバイスに部分文字列として指定された文字列が含まれているかどうかを確認します。 文字列配列の場合、部分文字列は検索されません。 文字列配列を使って、配列を検索し、指定された文字列全体が含まれているかどうかを確認します。 配列内の要素に部分文字列が含まれているかどうかを確認するために、文字列配列を検索することはできません。

1 つの AQS フィルター文字列により結果を適切に絞り込むことができない場合は、受け取った結果をさらにフィルター処理できます。 ただしその場合は、最初の AQS フィルター文字列によりできる限り結果を絞り込んでから、[**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API に渡すことをお勧めします。 これにより、アプリのパフォーマンスを向上させることができます。

## <a name="aqs-string-examples"></a>AQS 文字列の例

ここで示している例では、AQS 構文を使って、列挙するデバイスを制限する方法を説明しています。 以下のフィルター文字列はすべて、[**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングされており、完全なフィルターを作成できます。 どの種類も指定しない場合、既定の種類は **DeviceInterface** になります。

このフィルターを **DeviceInterface** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、オーディオ キャプチャ インターフェイス クラスを含むオブジェクトと、現在有効なオブジェクトがすべて列挙されます。 **=** は**COP\_EQUALS**に変換されます。

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

このフィルターを **Device** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、GenCdRom のハードウェア ID を 1 つ以上持つオブジェクトがすべて列挙されます。 **~~** は、 **CONTAINS\_\_値**に変換されます。

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

このフィルターを **DeviceContainer** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、部分文字列として Microsoft を含むモデル名を持つオブジェクトがすべて列挙されます。 **~~** は、 **CONTAINS\_\_値**に変換されます。

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

このフィルターを **DeviceInterface** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、部分文字列の Microsoft から始まる名前を持つオブジェクトがすべて列挙されます。 **~&lt;** は**COP\_STARTSWITH**に変換されます。

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

このフィルターを **Device** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、**System.Devices.IpAddress** プロパティ セットを持つオブジェクトがすべて列挙されます。 **&lt;&gt;\[\]** は、 **NULL**値と組み合わされた**COP\_** に変換されます。

``` syntax
System.Devices.IpAddress:<>[]
```

このフィルターを **Device** の [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) とペアリングすると、**System.Devices.IpAddress** プロパティ セットを持たないオブジェクトがすべて列挙されます。 **=\[\]** は、 **NULL**値と組み合わせて**COP\_EQUALS**に変換されます。

``` syntax
System.Devices.IpAddress:=[]
```

 

 
