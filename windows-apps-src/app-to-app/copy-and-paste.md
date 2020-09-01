---
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで、クリップボードを使用してコピーと貼り付けをサポートする方法について説明します。
title: コピーと貼り付け
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 117c09eb2dd0f24a330060da9b9766cb33e90d58
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175766"
---
# <a name="copy-and-paste"></a>コピーと貼り付け

この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで、クリップボードを使用してコピーと貼り付けをサポートする方法について説明します。 コピーと貼り付けはアプリ間やアプリ内でデータを交換するための従来の方法であり、クリップボード操作はほとんどすべてのアプリである程度サポートできます。 さまざまなコピーと貼り付けのシナリオを示す完全なコード例については、 [クリップボードのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)を参照してください。

## <a name="check-for-built-in-clipboard-support"></a>組み込みのクリップボード サポートの確認

多くの場合、クリップボード操作をサポートするためのコードを記述する必要はありません。 アプリの作成に使うことができる既定の XAML コントロールの多くは、クリップボード操作をサポートしています。 

## <a name="get-set-up"></a>準備

まず、アプリに [**Windows.ApplicationModel.DataTransfer**](/uwp/api/Windows.ApplicationModel.DataTransfer) 名前空間を含めます。 次に、[**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) オブジェクトのインスタンスを追加します。 このオブジェクトには、ユーザーがコピーするデータと開発者が含めるプロパティ (説明など) の両方が格納されます。

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>コピーと切り取り

コピーと切り取り (*移動*とも呼ばれます) には、ほぼ同じ機能があります。 [**RequestedOperation**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) プロパティを使用して、必要な操作を選択します。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>コピーしたコンテンツを設定する

次に、ユーザーが選択したデータを [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) オブジェクトに追加できます。 このデータが**DataPackage**クラスでサポートされている場合は、 **DataPackage**オブジェクトの対応する[メソッド](/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods)のいずれかを使用できます。 次に、 [**SetText**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) メソッドを使用してテキストを追加する方法を示します。

```cs
dataPackage.SetText("Hello World!");
```

最後に、静的な [**SetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent) メソッドを呼び出すことによって [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) をクリップボードに追加します。

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>貼り付け

クリップボードの内容を取得するには、静的な [**GetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent) メソッドを呼び出します。 このメソッドは、コンテンツを含む [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) を返します。 このオブジェクトは、コンテンツが読み取り専用であることを除いて [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) オブジェクトとほぼ同じです。 このオブジェクトがあれば、[**AvailableFormats**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) または [**Contains**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) のメソッドを使って使用可能な形式を特定できます。 その後、対応する [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) メソッドを呼び出してデータを取得できます。

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>クリップボードへの変更の追跡

コピーと貼り付けのコマンドに加えて、クリップボードへの変更を追跡することもできます。 これを行うには、クリップボードの [**ContentChanged**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) イベントを処理します。

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>関連項目

* [クリップボードのサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [アプリ間通信](index.md)
* [DataTransfer](/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)
* [DataRequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataTransferManager)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](../design/controls-and-patterns/index.md)
* [SetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Contains](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [ContentChanged](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)