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
ms.openlocfilehash: adbe1556f48e2f9b362faa303be52586714c73d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157026"
---
# <a name="control-patterns-and-interfaces"></a>コントロール パターンとインターフェイス  



Microsoft UI オートメーションのコントロール パターン、それらにアクセスするためにクライアントが使うクラス、それらを実装するためにプロバイダーが使うインターフェイスを紹介します。

このトピックの表は、Microsoft UI オートメーションのコントロール パターン、 それらにアクセスするために UI オートメーション クライアントが使うクラス、それらを実装するために UI オートメーション プロバイダーが使うインターフェイスを示しています。 **Control pattern** 列には、[**Control Pattern Availability Property Identifiers**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) に一覧表示される定数値として、UI オートメーション クライアントから見たパターン名が表示されます。 UI オートメーション プロバイダーの側から見ると、これらのパターンは [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) の定数名です。 **"クラス プロバイダー インターフェイス"** 列には、カスタム XAML コントロールにこのパターンを提供するためにプロバイダーが実装するインターフェイスの名前が表示されます。

コントロール パターンを公開してインターフェイスを実装するカスタム オートメーション ピアの実装方法について詳しくは、「[カスタム オートメーション ピア](custom-automation-peers.md)」をご覧ください。

コントロール パターンを実装する場合は、実装のために使う UI フレームワークにかかわらず、コントロール パターンに対するクライアントの想定について UI オートメーション プロバイダーのドキュメントもご覧ください。 一般的な UI オートメーション プロバイダーのドキュメントには、ピアの実装方法およびそのパターンを正しくサポートする方法に関連する情報が含まれます。 実装するパターンについては、「[UI オートメーション コントロール パターンの実装](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)」をご覧ください。

| コントロール パターン | クラス プロバイダー インターフェイス | 説明 |
|-----------------|--------------------------|-------------|
| **注釈** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | ドキュメント内の注釈のプロパティを公開するために使われます。 |
| **ドッキング** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | ドッキング コンテナーにドッキングすることができるコントロールに使用されます。 たとえば、ツールバーやツール パレットです。 |
| **ドラッグ** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | ドラッグ可能なコントロール、またはドラッグ可能な項目を含むコントロールをサポートするために使われます。 |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | ドラッグ アンド ドロップ操作のターゲットにできるコントロールをサポートするために使われます。 |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | コンテンツの表示拡大のために展開し、コンテンツの非表示のために折りたたむコントロールをサポートするために使われます。 |
| **グリッド** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | サイズ変更や指定したセルへの移動などグリッド機能をサポートするコントロールに使用されます。 グリッドはレイアウトを提供しますがコントロールではないため、グリッド自体はこのパターンを実装しません。 |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | グリッド内にセルを持つコントロールに使用されます。 |
| **Invoke** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | [**ボタン**](/uwp/api/Windows.UI.Xaml.Controls.Button)など、呼び出すことができるコントロールに使用されます。 |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 仮想化されたリストなどのコンテナー内の要素をアプリが見つけられるようにします。 |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 情報、データ、子の同じセットの複数の表現の間で切り替えることができるコントロールに使用されます。 |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | ドキュメントの基になるオブジェクト モデルにポインターを公開するために使われます。 |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | コントロールに適用できる値の範囲を持つコントロールに使用されます。 たとえば、年を含むスピン ボックス コントロールの値の範囲は 1900 年から現在の年までになり、月を提示するスピン ボックス コントロールの値の範囲は 1 ～ 12 になります。 |
| **スクロール** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | スクロールできるコントロールに使用されます。 たとえば、コントロールの表示可能領域に表示できる量より多くの情報があるとアクティブになるスクロール バーを持つコントロール。 |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | スクロールされるリスト内に個々の項目を持つコントロールに使用されます。 たとえば、コンボ ボックス コントロールなどスクロール リストに個々の項目を持つリスト コントロール。 |
| **選択内容** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 選択コンテナー コントロールに使用されます。 ([**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox)、[**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) など)。 |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | リスト ボックスやコンボ ボックスなどの選択コンテナー コントロールの個々の項目に使用されます。 |
| **スプレッドシート** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | スプレッドシートまたは他のグリッド ベースのドキュメントのコンテンツを公開するために使われます。 |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | スプレッドシートまたは他のグリッド ベースのドキュメントでセルのプロパティを公開するために使われます。 |
| **スタイル** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 特定のスタイル、塗りつぶしの色、塗りつぶしパターン、または図形を含む UI 要素を記述するために使われます。 |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | UI オートメーション クライアント アプリでマウスまたはキーボード入力を特定の UI 要素に転送することを可能にします。 |
| **Table** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | グリッドとヘッダー情報を持つコントロールに使用されます。 (表形式のカレンダー コントロールなど)。 |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | テーブル内の項目に使用されます。 |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | テキストの情報を公開するエディット コントロールとドキュメントに使用されます。 また、「[**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider)」および「[**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)」もご覧ください。 |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | **Text** コントロール パターンをサポートする、要素に最も近い祖先にアクセスするために使われます。 |
| **TextEdit** | 使用できるマネージ クラスがありません | テキストを変更するコントロール (たとえば、自動修正の実行、入力方式エディター (IME) を通じた入力合成の有効化を行うコントロールなど) へのアクセスを提供します。 |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | [**ITextProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider) を実装するテキスト コンテナー内の一続きのテキストへのアクセスを提供します。 [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2) もご覧ください。 |
| **トグル** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 状態を切り替えることができるコントロールに使用されます。 ([**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox)、オン/オフを切り替えることのできるメニュー項目など)。 |
| **変換** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | サイズ変更、移動、または回転を行えるコントロールに使用されます。 Transform コントロール パターンの一般的な用途は、デザイナー、フォーム、グラフィカル エディター、および描画アプリケーションでの使用です。 |
| **値** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | クライアントで、値の範囲をサポートしないコントロールで値を取得したり、設定したりできます。 |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 仮想化されていて、UI オートメーション要素として完全にアクセスできるようにする必要があるコンテナー内の項目を公開します。 |
| **ウィンドウ** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | ウィンドウ固有の情報を公開します。ウィンドウは、Microsoft Windows オペレーティング システムの基本概念です。 たとえば、子ウィンドウ、ダイアログなどのコントロールはウィンドウです。 |

> [!NOTE]
> 既存の XAML コントロールで、必ずしもこれらすべてのパターンの実装が見られるわけではありません。 一部のパターンでは、パターンの一般的な UI オートメーション フレームワーク定義との等価性を維持し、そのパターンをサポートするための純粋なカスタム実装を必要とするオートメーション ピア シナリオに対応することだけを目的としてインターフェイスが使用されている場合があります。

> [!NOTE]
> Windows Phone ストア アプリでは、ここに示されているすべての UI オートメーションのコントロール パターンがサポートされているわけではありません。 **Annotation**、**Dock**、**Drag**、**DropTarget**、**ObjectModel** がサポートされていないパターンの例です。

<span id="related_topics"/>

## <a name="related-topics"></a>関連トピック  
* [カスタム オートメーション ピア](custom-automation-peers.md)
* [アクセシビリティ](accessibility.md)