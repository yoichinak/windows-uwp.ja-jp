---
title: アプリからの 3D 印刷
description: ユニバーサル Windows アプリに 3D 印刷機能を追加する方法について説明します。 このトピックでは、3D モデルが印刷可能であり、正しい形式になっていることを確認した後で 3D 印刷ダイアログを起動する方法について説明します。
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp、3dprinting、3d 印刷
ms.localizationpriority: medium
ms.openlocfilehash: 357d8bd3a460e61c436750fc4c9cbfbf8a8fcbfc
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362855"
---
# <a name="3d-printing-from-your-app"></a>アプリからの 3D 印刷

**重要な API**

-   [**Printing3D**](/uwp/api/Windows.Graphics.Printing3D)

ユニバーサル Windows アプリに 3D 印刷機能を追加する方法について説明します。 このトピックでは、アプリに 3D 形状データを読み込んだ後、その 3D モデルが印刷可能であり、正しい形式になっていることを確認してから 3D 印刷ダイアログを起動する方法について説明します。 以下の手順の実例については、[3D 印刷の UWP サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)をご覧ください。

> [!NOTE]
> このガイドのサンプル コードでは、簡潔にするためにエラー レポートと処理が大幅に簡略化されています。

## <a name="setup"></a>セットアップ


3D 印刷機能を搭載するアプリケーション クラスで、[**Windows.Graphics.Printing3D**](/uwp/api/Windows.Graphics.Printing3D) 名前空間を追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="Snippet3DPrintNamespace":::

このガイドでは、次の追加の名前空間を使います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetOtherNamespaces":::

次に、有用なメンバー フィールドをクラスに追加します。 プリンター ドライバーに渡す印刷タスクを表すために、[**Print3DTask**](/uwp/api/Windows.Graphics.Printing3D.Print3DTask) オブジェクトを宣言します。 アプリに読み込まれる元の 3D データ ファイルを保持するために、[**StorageFile**](/uwp/api/Windows.Storage.StorageFile) オブジェクトを宣言します。 必要なすべてのメタデータが含まれた、印刷準備が完了した 3D モデルを表す [**Printing3D3MFPackage**](/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage) オブジェクトを宣言します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetDeclareVars":::

## <a name="create-a-simple-ui"></a>シンプルな UI の作成

このサンプルでは、プログラム メモリにファイルを読み込む読み込みボタン、必要に応じてファイルを変更する修正ボタン、印刷ジョブを開始する印刷ボタンの 3 つのユーザー コントロールを使います。 次のコードで、これらのボタン (クリック イベント ハンドラー付き) を .cs クラスの対応する XAML ファイルに作成します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml" id="SnippetButtons":::

UI フィードバック用に **TextBlock** を追加します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml" id="SnippetOutputText":::



## <a name="get-the-3d-data"></a>3D データの取得


アプリでは、さまざまな方法で、3D 形状データを取得することができます。 たとえば、3D スキャンからデータを取得したり、Web リソースからのモデル データをダウンロードしたり、数式やユーザー入力を使ってプログラムによって 3D メッシュを生成したりできます。 ここでは簡単にするために、3D データ ファイル (一般的なファイルの種類のいずれか) をデバイスのストレージからプログラム メモリに読み込む方法を示します。 [3D Builder モデル ライブラリ](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing)には幅広いモデルが用意されており、デバイスに簡単にダウンロードできます。

メソッドでは `OnLoadClick` 、 [**Fileopenpicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) クラスを使用して、アプリのメモリに1つのファイルを読み込みます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetFileLoad":::

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>3D Builder による 3D Manufacturing Format (.3mf) への変換

これで、3D データ ファイルをアプリのメモリに読み込むことができます。 ただし、3D 形状データには、さまざまな形式がありますが、すべてが 3D 印刷に効率的であるわけではありません。 Windows 10 では、すべての 3D 印刷タスクについて 3D Manufacturing Format (.3mf) というファイル形式を使います。

> [!NOTE]  
> .3mf ファイル形式には、このチュートリアルで扱っている機能以外にも多くの機能が用意されています。 3MF と 3D 製品のプロデューサーおよびコンシューマー向けに用意されたその機能について詳しくは、[3MF の仕様](https://3mf.io/what-is-3mf/3mf-specification/)をご覧ください。 Windows 10 API を使ってこれらの機能を利用する方法については、「[3MF パッケージの生成](./generate-3mf.md)」チュートリアルをご覧ください。

[3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) アプリでは、一般的なほとんどの 3D 形式のファイルを開くことができ、それらを .3mf ファイル形式で保存することができます。 この例では、ファイルの種類が異なる場合に、簡単な解決策として、3D Builder アプリを開き、インポートしたデータを .3mf ファイルとして保存し再度読み込むようユーザーに求めます。

> [!NOTE]  
> 
3D Builder には、ファイル形式の変換以外にも、モデルを編集したり色データを追加したりといった、印刷に固有の操作を行うための簡単なツールが用意されているため、多くの場合、3D 印刷を処理するアプリに統合するだけの価値があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetFileCheck":::

## <a name="repair-model-data-for-3d-printing"></a>3D 印刷可能なモデル データへの修復

.3mf 形式であっても、すべての 3D モデル データが印刷可能なわけではありません。 どこを埋めて何を空洞のままにするかをプリンターに正しく判断させるには、印刷する (各) モデルが 1 つのシームレスなメッシュであること、モデルの面の法線が外側を向いていること、またモデルがマニホールド形状であることが必要条件となります。 これらの問題は、さまざまな形で現れることがあり、図形が複雑な場合は、見つけるのが難しいことがあります。 ただし、最新のソフトウェア ソリューションは、多くの場合、元データの形状を印刷可能な 3D 図形に変換するのに十分な機能を備えています。 これはモデルの*修復*と呼ばれ、`OnFixClick` メソッドで行われます。

3D データファイルは、 [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream)を実装するように変換する必要があります。これは、 [**Printing3DModel**](/uwp/api/Windows.Graphics.Printing3D.Printing3DModel) オブジェクトの生成に使用できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetRepairModel":::

これで、**Printing3DModel** オブジェクトを印刷できる状態に修復できました。 [**SaveModelToPackageAsync**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) を使って、クラスを作成したときに宣言した **Printing3D3MFPackage** オブジェクトにモデルを割り当てます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetSaveModel":::

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>印刷タスクの実行: TaskRequested ハンドラーの作成


3D 印刷ダイアログをユーザーに表示してユーザーが印刷を開始したときに、アプリが目的のパラメーターを 3D 印刷パイプラインに渡す必要があります。 3D 印刷 API は **[Taskrequested](/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** されたイベントを発生させます。 このイベントを適切に処理するメソッドを記述する必要があります。 通常どおり、ハンドラー メソッドはイベントと同じ型である必要があります。**TaskRequested** イベントには、パラメーター [**Print3DManager**](/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (センダー オブジェクトへの参照) と [**Print3DTaskRequestedEventArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) オブジェクト (ほとんどの関連情報を保持するオブジェクト) があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetMyTaskTitle":::

このメソッドの主な目的は、*args* パラメーターを使って、**Printing3D3MFPackage** をパイプラインに送信することです。 **Print3DTaskRequestedEventArgs** 型には、[**Request**](/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request) という 1 つのプロパティがあります。 その型は [**Print3DTaskRequest**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) で、1 つの印刷ジョブ要求を表します。 そのメソッドである [**CreateTask**](/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) を使って、プログラムは印刷ジョブに関する適切な情報を送信します。このメソッドは、3D 印刷パイプラインに送信された **Print3DTask** オブジェクトへの参照を返します。

**CreateTask** には、印刷ジョブ名を表す string、使うプリンターの ID を表す string、および [**Print3DTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler) デリゲートという入力パラメーターがあります。 このデリゲートは、**3DTaskSourceRequested** イベントが発生したときに自動的に呼び出されます (これは API によって行われます)。 重要なのは、印刷ジョブが開始されたときにこのデリゲートが呼び出され、適切な 3D 印刷パッケージを提供する役割を果たすということです。

**Print3DTaskSourceRequestedHandler** は、1つのパラメーターを受け取ります。これは、送信されるデータを提供する [**Print3DTaskSourceRequestedArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) オブジェクトです。 このクラスの 1 つのパブリック メソッドである [**SetSource**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource) が、印刷するパッケージを受け取ります。 次のように **Print3DTaskSourceRequestedHandler** デリゲートを実装します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetSourceHandler":::

次に、新しく定義したデリゲート `sourceHandler` を使って、**CreateTask** を呼び出します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetCreateTask":::

返された **Print3DTask** が、最初に宣言したクラス変数に割り当てられます。 これで、必要に応じてこの参照を使い、タスクによってスローされた特定のイベントを処理できるようになりました。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetOptional":::

> [!NOTE]  
> これらのイベントに `Task_Submitting` および `Task_Completed` メソッドを登録するには、それらを実装する必要があります。

## <a name="execute-printing-task-open-3d-print-dialog"></a>印刷タスクの実行: 3D 印刷ダイアログを開く


最後に 3D 印刷ダイアログを起動する短いコードが必要になります。 従来の印刷ダイアログ ウィンドウと同じように、3D 印刷ダイアログでも、最後に使った印刷オプションがいくつか表示され、ユーザーは使うプリンターを (USB かネットワーク接続かに関係なく) 選択することができます。

`MyTaskRequested` メソッドを **TaskRequested** イベントに登録します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetRegisterMyTaskRequested":::

**TaskRequested** イベント ハンドラーを登録したら、メソッド [**ShowPrintUIAsync**](/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync) を呼び出して、現在のアプリケーション ウィンドウに 3D 印刷ダイアログを表示することができます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetShowDialog":::

最後に、アプリにコントロールが戻ったら、イベント ハンドラーの登録を解除することをお勧めします。  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetDeregisterMyTaskRequested":::

## <a name="related-topics"></a>関連トピック

[3MF パッケージの生成](./generate-3mf.md)  
[3D 印刷の UWP サンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
