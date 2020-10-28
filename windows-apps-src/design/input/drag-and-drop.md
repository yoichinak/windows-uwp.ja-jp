---
description: この記事では、Windows アプリでドラッグアンドドロップを追加する方法について説明します。
title: ドラッグ アンド ドロップ
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a0005ecf7d51cc6b08bc5cc61350489839d568f
ms.sourcegitcommit: 047004e2bf100e319d134c18518062bf7f3efb5d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92763106"
---
# <a name="drag-and-drop"></a>ドラッグ アンド ドロップ

ドラッグ アンド ドロップは、Windows デスクトップでアプリケーション内またはアプリケーション間でデータを転送するための直感的な方法です。 ドラッグ アンド ドロップを利用すると、ユーザーは、標準ジェスチャ (指で押したままパン、またはマウスやスタイラスでボタンを押したままパン) 使ってアプリケーション間やアプリケーション内でデータを転送できます。

> **重要な API** : [CanDrag プロパティ](/uwp/api/windows.ui.xaml.uielement.candrag)、 [AllowDrop プロパティ](/uwp/api/windows.ui.xaml.uielement.allowdrop) 

ドラッグ ソース (ドラッグ ジェスチャがトリガーされたアプリケーションや領域) は、標準的なデータ形式 (テキスト、RTF、HTML、ビットマップ、ストレージ項目) やカスタム データ形式を含むことができるデータ パッケージ オブジェクトに入力することによって、転送されるデータを提供します。 ソースは、ソースがサポートする操作の種類 (コピー、移動、リンク) も示します。 ポインターが離されたときにドロップが発生します。 ドロップ ターゲット (ポインターの下にあるアプリケーションや領域) は、データ パッケージを処理し、実行される操作の種類を返します。

ドラッグ アンド ドロップを行っているとき、ドラッグ UI によって、実行されているドラッグ アンド ドロップ操作の種類が視覚的に示されます。 この視覚的なフィードバックは、ソースによって最初に提供されますが、ポインターがターゲットの上に移動したときに、ターゲットによって変更することができます。

最新のドラッグ アンド ドロップは、UWP をサポートするすべてのデバイスで利用できます。 このドラッグ アンド ドロップでは、すべての種類のアプリケーション間やアプリケーション内でデータを転送できます。こうしたアプリケーションには従来の Windows アプリも含まれますが、この記事では、最新のドラッグ アンド ドロップについては XAML API に焦点を当てています。 ドラッグ アンド ドロップを実装すると、アプリからアプリ、アプリからデスクトップ、デスクトップからアプリなど、あらゆる方向でシームレスに機能します。

アプリでドラッグ アンド ドロップを有効にする場合に必要となることの概要を次に示します。

1. 要素に対してドラッグを有効にするには、要素の **CanDrag** プロパティを true に設定します。  
2. データ パッケージを作成します。 システムでは画像とテキストが自動的に処理されますが、他のコンテンツについては、 **DragStarted** イベントと **DragCompleted** イベントを手動で処理し、これらを使って独自のデータ パッケージを作成する必要があります。 
3. ドロップされるコンテンツを受け取ることができるすべての要素に対して、 **AllowDrop** プロパティを **true** に設定することで、ドロップを有効にします。 
4. **DragOver** イベントを処理して、要素が受け取ることができるドラッグ操作の種類をシステムに通知します。 
5. **Drop** イベントを処理して、ドロップされるコンテンツを受け取ります。 



## <a name="enable-dragging"></a>ドラッグを有効にする

要素に対してドラッグを有効にするには、要素の [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) プロパティを **true** に設定します。 これにより、ListView などのコレクションがドラッグ可能な場合に、要素とそれに含まれる要素が作成されます。

どの要素をドラッグ可能にするかを、明確にしておいてください。 ユーザーは、アプリ内にあるすべての項目をドラッグできるようにするのではなく、画像やテキストなどの特定の項目のみをドラッグすることを必要としています。 

[**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) の設定方法を次に示します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDragArea":::

UI をカスタマイズする場合 (この記事の後半で説明します) を除き、他には何もしなくてもドラッグ操作を有効にできます。 ドロップ操作には、あといくつかの手順が必要です。

## <a name="construct-a-data-package"></a>データ パッケージを作成する 

ほとんどの場合、システムによってデータ パッケージが自動的に作成されます。 システムでは、次のコンテンツが自動的に処理されます。
* イメージ
* Text 

他のコンテンツについては、 **DragStarted** イベントと **DragCompleted** イベントを手動で処理し、これらを使って独自の [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage) を作成する必要があります。

## <a name="enable-dropping"></a>ドロップを有効にする

次のマークアップは、XAML で [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) を使って、アプリの特定の領域をドロップ操作に有効な領域として設定する方法を示しています。 ユーザーが他の場所へのドロップを試みても、ドロップすることはできません。 アプリ内のすべての領域でユーザーが項目をドロップできるようにする場合は、背景全体をドロップ先として設定します。

:::code language="xml" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml" id="SnippetDropArea":::


## <a name="handle-the-dragover-event"></a>DragOver イベントを処理する

[**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover) イベントは、ユーザーがアプリに項目をドラッグし、まだドロップしていないときに発生します。 このハンドラーでは、 [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation) プロパティを使って、アプリがサポートしている操作の種類を指定する必要があります。 最も一般的な操作はコピーです。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOver":::

## <a name="process-the-drop-event"></a>Drop イベントを処理する

[**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) イベントは、有効なドロップ領域内でユーザーが項目を放したときに発生します。 放した項目を処理するには [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview) プロパティを使います。

次の例では、わかりやすくするために、ユーザーが単一の写真をドロップして直接それにアクセスしたとします。 実際には、ユーザーがさまざまな形式の複数の項目を同時にドロップすることもあります。 アプリで、ドロップされたファイルの種類とファイル数を確認することでこの状況を処理し、状況に応じてそれぞれを処理する必要があります。 また、ユーザーがアプリでサポートされていない処理を実行しようとしたときに、ユーザーに通知することを検討する必要があります。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_Drop":::

## <a name="customize-the-ui"></a>UI のカスタマイズ

システムでは、ドラッグ アンド ドロップ用に既定の UI が提供されています。 ただし、カスタムのキャプションやグリフを設定して UI のさまざまな部分をカスタマイズすることも、UI をまったく表示しないこともできます。 UI をカスタマイズするには、 [**DragEventArgs.DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride) プロパティを使います。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/design/input/drag_drop/cs/MainPage.xaml.cs" id="SnippetGrid_DragOverCustom":::

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>タッチによるドラッグが可能な項目のコンテキスト メニューを開く

タッチを使い、 [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) をドラッグして、コンテキスト メニューを開くには、類似したタッチ ジェスチャを使用します。それぞれ長押しから始まります。 アプリの要素が 2 つのアクションをサポートしている場合に、システムがそれを区別する方法を次に示します。 

* ユーザーがある項目を長押しし、500 ミリ秒以内にドラッグを開始した場合、項目はドラッグされ、コンテキスト メニューは表示されません。 
* ユーザーが長押ししてから 500 ミリ秒以内にドラッグしなかった場合には、コンテキスト メニューが開きます。 
* コンテキスト メニューが開いた後に、ユーザーが指を離すことなく、項目をドラッグしようとすると、コンテキスト メニューは閉じられ、ドラッグが開始されます。

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>ListView または GridView の項目をフォルダーとして指定する

[**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) または [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) をフォルダーとして指定できます。 これは、ツリー ビューとエクスプローラーのシナリオで特に便利です。 これを行うには、その項目で [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) プロパティを明示的に **True** に設定します。 

(非フォルダー項目にではなく) フォルダーにドロップするための、適切なアニメーションが自動的に表示されます。 アプリのコードは、フォルダー項目の [**ドロップ**](/uwp/api/windows.ui.xaml.uielement.drop) イベントの処理 (および非フォルダー項目の処理) を継続し、データ ソースを更新して、ドロップ先のフォルダーにドロップされた項目を追加する必要があります。

## <a name="enable-drag-and-drop-reordering-within-listviews"></a>[ListViews 内でドラッグアンドドロップの並べ替えを有効にする]

[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)は、この記事で説明されている **candrop** api と非常によく似た api を使用して、すぐに使用できるドラッグベースの並べ替えをサポートしています。 少なくとも、 **system.windows.uielement.allowdrop** プロパティと **CanReorderItems** プロパティを追加します。

詳細については、「 [**ListViewBase. CanReorderItems**](/uwp/api/windows.ui.xaml.controls.listviewbase.canreorderitems) 」を参照してください。

## <a name="implementing-custom-drag-and-drop"></a>カスタムのドラッグ アンド ドロップを実装する

[UIElement](/uwp/api/windows.ui.xaml.uielement) クラスは、ドラッグ アンド ドロップを実装するためのほとんどの処理を自動的に実行します。 ただし、必要に応じて、 [DataTransfer 名前空間](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)の api を使用して独自のバージョンを実装することもできます。

| 機能 | WinRT API |
| --- | --- |
|  ドラッグを有効にする | [CoreDragOperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  データ パッケージを作成する | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| ドラッグをシェルに渡す  | [CoreDragOperation.StartAsync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| シェルからドロップを受け取る  | [CoreDragDropManager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>関連項目

* [アプリ間通信](index.md)
* [AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [DragOver](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [」](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)
