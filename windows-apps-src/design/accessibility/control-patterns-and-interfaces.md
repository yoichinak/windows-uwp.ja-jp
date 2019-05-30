---
Description: Microsoft UI オートメーションのコントロール パターン、それらにアクセスするためにクライアントが使うクラス、それらを実装するためにプロバイダーが使うインターフェイスを紹介します。
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: コントロール パターンとインターフェイス
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc8018a4ccf53c106a1e10410593e214d9dcead7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359565"
---
# <a name="control-patterns-and-interfaces"></a>コントロール パターンとインターフェイス  



Microsoft UI オートメーションのコントロール パターン、それらにアクセスするためにクライアントが使うクラス、それらを実装するためにプロバイダーが使うインターフェイスを紹介します。

このトピックの表は、Microsoft UI オートメーションのコントロール パターン、 それらにアクセスするために UI オートメーション クライアントが使うクラス、それらを実装するために UI オートメーション プロバイダーが使うインターフェイスを示しています。 **Control pattern** 列には、[**Control Pattern Availability Property Identifiers**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) に一覧表示される定数値として、UI オートメーション クライアントから見たパターン名が表示されます。 UI オートメーション プロバイダーの側から見ると、これらのパターンは [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) の定数名です。 **"クラス プロバイダー インターフェイス"** 列には、カスタム XAML コントロールにこのパターンを提供するためにプロバイダーが実装するインターフェイスの名前が表示されます。

コントロール パターンを公開してインターフェイスを実装するカスタム オートメーション ピアの実装方法について詳しくは、「[カスタム オートメーション ピア](custom-automation-peers.md)」をご覧ください。

コントロール パターンを実装する場合は、実装のために使う UI フレームワークにかかわらず、コントロール パターンに対するクライアントの想定について UI オートメーション プロバイダーのドキュメントもご覧ください。 一般的な UI オートメーション プロバイダーのドキュメントには、ピアの実装方法およびそのパターンを正しくサポートする方法に関連する情報が含まれます。 実装するパターンについては、「[UI オートメーション コントロール パターンの実装](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)」をご覧ください。

| コントロール パターン | クラス プロバイダー インターフェイス | 説明 |
|-----------------|--------------------------|-------------|
| **注釈** | [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | ドキュメント内の注釈のプロパティを公開するために使われます。 |
| **ドッキング ステーション** | [**IDockProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | ドッキング コンテナーにドッキングできるコントロールに使われます  (ツール バー、ツール パレットなど)。 |
| **ドラッグ** | [**IDragProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | ドラッグ可能なコントロール、またはドラッグ可能な項目を含むコントロールをサポートするために使われます。 |
| **DropTarget** | [**IDropTargetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | ドラッグ アンド ドロップ操作のターゲットにできるコントロールをサポートするために使われます。 |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | コンテンツの表示拡大のために展開し、コンテンツの非表示のために折りたたむコントロールをサポートするために使われます。 |
| **グリッド** | [**IGridProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | サイズ指定、指定したセルへの移動などのグリッド機能をサポートするコントロールに使われます。 グリッドはレイアウトを提供しますがコントロールではないため、グリッド自体はこのパターンを実装しません。 |
| **GridItem** | [**IGridItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | グリッド内のセルを持つコントロールに使われます。 |
| **呼び出す** | [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | 呼び出すことができるコントロールに使われます ([**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) など)。 |
| **ItemContainer** | [**IItemContainerProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 仮想化されたリストなどのコンテナー内の要素をアプリが見つけられるようにします。 |
| **MultipleView** | [**IMultipleViewProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 同じ情報、データ、または子のセットの複数の表現を切り替えることができるコントロールに使われます。 |
| **Objectmodel です。** | [**IObjectModelProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | ドキュメントの基になるオブジェクト モデルにポインターを公開するために使われます。 |
| **RangeValue** | [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | 適用できる値の範囲を持つコントロールに使われます。 たとえば、年を含むスピン ボックス コントロールの値の範囲は 1900 年から現在の年までになり、月を提示するスピン ボックス コントロールの値の範囲は 1 ～ 12 になります。 |
| **スクロール** | [**IScrollProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | スクロールできるコントロールに使われます  (表示可能領域に表示しきれない情報がある場合にアクティブになるスクロール バーを持つコントロールなど)。 |
| **ScrollItem** | [**IScrollItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | スクロールするリストの個々の項目を持つコントロールに使われます  (コンボ ボックス コントロールなどのスクロール リストの個々の項目を持つリスト コントロールなど)。 |
| **選択** | [**ISelectionProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 選択コンテナー コントロールに使われます  ([**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)、[**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) など)。 |
| **SelectionItem** | [**ISelectionItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | リスト ボックス、コンボ ボックスなどの選択コンテナー コントロールの個々の項目に使われます。 |
| **スプレッドシート** | [**ISpreadsheetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | スプレッドシートまたは他のグリッド ベースのドキュメントのコンテンツを公開するために使われます。 |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | スプレッドシートまたは他のグリッド ベースのドキュメントでセルのプロパティを公開するために使われます。 |
| **スタイル** | [**IStylesProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 特定のスタイル、塗りつぶしの色、塗りつぶしパターン、または図形を含む UI 要素を記述するために使われます。 |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | UI オートメーション クライアント アプリでマウスまたはキーボード入力を特定の UI 要素に転送することを可能にします。 |
| **Table** | [**ITableProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | グリッドとヘッダー情報を持つコントロールに使われます  (表形式のカレンダー コントロールなど)。 |
| **TableItem** | [**ITableItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | 表の項目に使われます。 |
| **Text** | [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | 編集コントロールやテキスト情報を表示するドキュメントに使われます。 また、「[**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider)」および「[**ITextProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)」もご覧ください。 |
| **TextChild** | [**ITextChildProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | **Text** コントロール パターンをサポートする、要素に最も近い祖先にアクセスするために使われます。 |
| **TextEdit** | 使用できるマネージ クラスがありません | テキストを変更するコントロール (たとえば、自動修正の実行、入力方式エディター (IME) を通じた入力合成の有効化を行うコントロールなど) へのアクセスを提供します。 |
| **TextRange** | [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | [  **ITextProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider) を実装するテキスト コンテナー内の一続きのテキストへのアクセスを提供します。 [  **ITextRangeProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2) もご覧ください。 |
| **トグル** | [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 状態を切り替えることができるコントロールに使われます  ([**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox)、オン/オフを切り替えることのできるメニュー項目など)。 |
| **変換** | [**ITransformProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | サイズ変更、移動、回転が可能なコントロールに使われます。 デザイナー、フォーム、グラフィカル エディター、描画アプリなどでよく使われます。 |
| **値** | [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | 値の範囲をサポートしないコントロールの値をクライアントが取得または設定できるようにします。 |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 仮想化されていて、UI オートメーション要素として完全にアクセスできるようにする必要があるコンテナー内の項目を公開します。 |
| **ウィンドウ** | [**IWindowProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Windows に固有の情報の基本概念、Microsoft Windows オペレーティング システムを公開します。 たとえば、子ウィンドウ、ダイアログなどのコントロールはウィンドウです。 |

> [!NOTE]
> 既存の XAML コントロールで、必ずしもこれらすべてのパターンの実装が見られるわけではありません。 一部のパターンでは、パターンの一般的な UI オートメーション フレームワーク定義との等価性を維持し、そのパターンをサポートするための純粋なカスタム実装を必要とするオートメーション ピア シナリオに対応することだけを目的としてインターフェイスが使用されている場合があります。

> [!NOTE]
> Windows Phone ストア アプリでは、ここに示されているすべての UI オートメーションのコントロール パターンがサポートされているわけではありません。 **Annotation**、**Dock**、**Drag**、**DropTarget**、**ObjectModel** がサポートされていないパターンの例です。

<span id="related_topics"/>

## <a name="related-topics"></a>関連トピック  
* [カスタム オートメーション ピア](custom-automation-peers.md)
* [ユーザー補助](accessibility.md) 
