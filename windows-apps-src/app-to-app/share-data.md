---
description: この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで、共有コントラクトをサポートする方法について説明します。
title: データの共有
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ed74149552e6582bf133550d4db1a45625e8c39
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364045"
---
# <a name="share-data"></a>データの共有


この記事では、ユニバーサル Windows プラットフォーム (UWP) アプリで、共有コントラクトをサポートする方法について説明します。 共有コントラクトは、テキスト、リンク、写真、ビデオなどのデータをアプリ間ですばやく共有するための簡単な方法です。 たとえば、ユーザーがソーシャル ネットワーキング アプリを使って友人と Web ページを共有する場合や、後で参照するためにリンクをメモ帳アプリで保存する場合があります。

> [!NOTE]
> この記事のコード例は、UWP アプリ向けに書かれています。 WPF、Windows フォーム、および C++/Win32 デスクトップアプリでは、 [Idatatransの Managerinterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) インターフェイスを使用して、特定のウィンドウの [Datatransのマネージャー](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) オブジェクトを取得する必要があります。 詳細については、 [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) サンプルを参照してください。

## <a name="set-up-an-event-handler"></a>イベント ハンドラーの設定

ユーザーが共有を呼び出したときに呼び出される [**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) イベント ハンドラーを追加します。 このイベントは、ユーザーがアプリ内のコントロール (ボタンやアプリ バー コマンドなど) をタップした場合に発生します。ユーザーがあるレベルをクリアしてハイ スコアを獲得した場合など、特定のシナリオで自動的に発生することもあります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetPrepareToShare":::

[**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) イベントが発生すると、アプリは [**DataRequest**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest) オブジェクトを受け取ります。 このオブジェクトに含まれている [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) を使って、ユーザーが共有するコンテンツを提供することができます。 共有するデータとタイトルを指定する必要があります。 説明は省略することもできますが、指定することをお勧めします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetCreateRequest":::

## <a name="choose-data"></a>データの選択

次のようなさまざまな種類のデータを共有することができます。

-   プレーンテキスト
-   Uniform Resource Identifier (URI)
-   HTML
-   書式付きテキスト
-   ビットマップ
-   ファイル
-   開発者が定義したカスタム データ

[**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) オブジェクトには、これらの 1 つ以上の形式を任意に組み合わせて格納することができます。 次の例は、テキストの共有を示しています。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetContent":::

## <a name="set-properties"></a>プロパティの設定

共有用にデータをパッケージ化するときに、共有されるコンテンツの情報を追加で提供するさまざまなプロパティを指定できます。 これらのプロパティは、ターゲット アプリでのユーザー エクスペリエンスを高めるために役立ちます。 たとえば、ユーザーが複数のアプリでコンテンツを共有している場合に、説明があると便利です。 画像や Web ページへのリンクを共有する場合にサムネイルを追加すると、ユーザーが視覚的に確認できます。 詳しくは、「[**DataPackagePropertySet**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)」を参照してください。

タイトルを除くすべてのプロパティは任意です。 タイトルのプロパティは必須です。必ず設定してください。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetProperties":::

## <a name="launch-the-share-ui"></a>共有 UI の起動

共有用の UI は、システムによって提供されます。 起動するには、[**ShowShareUI**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui) メソッドを呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetShowUI":::

## <a name="handle-errors"></a>エラーの処理

ほとんどの場合、コンテンツの共有は難しいプロセスではありません。 しかし、どのような場合であっても、予期しない問題が発生することは必ずあります。 たとえば、共有するコンテンツをユーザーが選ぶ必要がある状況で、ユーザーが選んでいない場合などです。 このような状況を処理するには、[**FailWithDisplayText**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_) メソッドを使います。このメソッドでは、問題が発生すると、ユーザーにメッセージが表示されます。

## <a name="delay-share-with-delegates"></a>デリゲートによる共有の遅延

場合によっては、ユーザーが共有するデータをすぐに準備しても効果的でないことがあります。 たとえば、複数の異なる形式の大きな画像ファイルの送信をサポートしているアプリの場合、ユーザーが選択する前にこれらの画像をすべて作成することは非効率的です。

この問題を解決するために、[**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) にはデリゲートも格納できます。デリゲートとは、受け取る側のアプリでデータを要求するときに呼び出される関数です。 リソースを大量に消費するデータを共有する場合はデリゲートを使うことをお勧めします。

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>関連項目 

* [アプリ間通信](index.md)
* [データの受信](receive-data.md)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [DataRequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 
