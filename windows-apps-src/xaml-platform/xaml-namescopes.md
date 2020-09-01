---
description: XAML 名前スコープには、XAML で定義されたオブジェクトの名前とそれに対応するインスタンスとの関係が格納されます。 この概念は、他のプログラミング言語やテクノロジで使われている用語 "名前スコープ" と広い意味で似ています。
title: XAML 名前スコープ
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4fa430cdd6c2f7b47576478ec30f0f139b80363
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173756"
---
# <a name="xaml-namescopes"></a>XAML 名前スコープ


*XAML 名前スコープ* には、XAML で定義されたオブジェクトの名前とそれに対応するインスタンスとの関係が格納されます。 この概念は、他のプログラミング言語やテクノロジで使われている用語 "*名前スコープ*" と広い意味で似ています。

## <a name="how-xaml-namescopes-are-defined"></a>XAML 名前スコープの定義方法

XAML 名前スコープ内で名前を使うと、最初に XAML で定義されたオブジェクトをユーザー コードで参照できます。 XAML を解析すると、内部的に、XAML 宣言で保持していた一部またはすべての関係を維持する一連のオブジェクトが作成されます。 これらの関係が、作成されたオブジェクトの特定のオブジェクト プロパティとして保持されるか、プログラミング モデル API のユーティリティ メソッドとして公開されます。

XAML 名前スコープ内の名前の最も一般的な用途は、オブジェクト インスタンスに対する直接参照です。これは、マークアップ コンパイル パスを部分クラス テンプレートに生成された **InitializeComponent** メソッドと組み合わせてプロジェクト ビルド アクションとして使うことで、有効化されます。

[**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) ユーティリティ メソッドを実行時に使って、XAML マークアップで名前を定義されているオブジェクトへの参照を返すこともできます。

### <a name="more-about-build-actions-and-xaml"></a>ビルド アクションと XAML の詳細

技術的には、XAML 自体はマークアップ コンパイラ パスを使います。この際、XAML と、コード ビハインド用に定義した部分クラスがまとめてコンパイルされます。 マークアップで定義された **Name** 属性または [x:Name](x-name-attribute.md) 属性を持つ各オブジェクト要素が、XAML と同名の内部フィールドを生成します。 初期状態では、このフィールドは空です。 続いて、クラスが **InitializeComponent** メソッドを生成します。このメソッドは、すべての XAML が読み込まれた後にのみ呼び出されます。 **InitializeComponent** ロジックにおいて、各内部フィールドには、名前文字列が等しいかどうかを評価した結果を表す [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) の戻り値が設定されます。 このインフラストラクチャは、コンパイル後に Windows ランタイム アプリ プロジェクトの /obj サブフォルダーに XAML ページごとに作成される ".g" (生成) ファイルで確認できます。 リフレクションを実行した場合は、結果として作成されるアセンブリのメンバーとしてのフィールドや **InitializeComponent** メソッドの存在を確認することもできます。それ以外の場合は、インターフェイス言語の内容を調べます。

**メモ**   特に Visual C++ コンポーネント拡張 (C++/CX) アプリでは、XAML ファイルのルート要素に対して、 **x:Name**参照のバッキングフィールドは作成されません。 C++/CX のコード ビハインドからルート オブジェクトを参照する必要がある場合は、他の API またはツリー走査を使ってください。 たとえば、既知の名前付き子要素に対して [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出した後、[**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) を呼び出します。

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>XamlReader.Load を使った実行時におけるオブジェクトの作成

XAML は、初期の XAML ソース解析操作に対して同じように動作する [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) メソッドの文字列入力としても使用できます。 **XamlReader.Load** は、実行時にオブジェクトの切断されたツリーを新規作成します。 この切断されたツリーは、メイン オブジェクト ツリーのいずれかのポイントにアタッチできます。 作成したオブジェクト ツリーは、**Children** などのコンテンツ プロパティ コレクションに追加するか、オブジェクト値を受け取る他のプロパティを設定する (たとえば、[**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) プロパティの値に新しい [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) を読み込む) ことで、明示的に接続する必要があります。

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>XamlReader.Load が XAML 名前スコープに与える影響

[**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) によって作成された新しいオブジェクト ツリーで定義される暫定的な XAML 名前スコープは、指定された XAML で定義名が一意であるかどうかを評価します。 指定された XAML 内の名前がこの時点で内部的に一意でない場合、**XamlReader.Load** によって例外がスローされます。 切断されたオブジェクト ツリーがメインのアプリ オブジェクト ツリーに接続されている場合、切断されたオブジェクト ツリーの XAML 名前スコープは、メインのアプリ XAML 名前スコープには結合されません。 ツリーの接続後、アプリのオブジェクト ツリーは結合されていますが、そのツリーの XAML 名前スコープは分離されています。 オブジェクト間の接続ポイントでは、分岐が発生します。ここで、**XamlReader.Load** の呼び出しによって返される値をプロパティに設定します。

切断された異なる XAML 名前スコープを持つことに伴う問題は、マネージ オブジェクトの直接参照と同様に、[**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) メソッドの呼び出しが、結合された XAML 名前スコープでは動作しなくなることです。 代わりに、**FindName** の呼び出しの対象となった特定のオブジェクトがスコープを意味し、そのスコープは呼び出し元オブジェクトが存在する XAML 名前スコープになります。 マネージ オブジェクトの直接参照では、スコープは、コードが存在するクラスによって暗黙的に指定されます。 一般に、アプリ コンテンツの "ページ" における実行時の対話に使うコード ビハインドは、ルート "ページ" をサポートする部分クラスに存在するため、XAML 名前スコープはルート XAML 名前スコープになります。

ルート XAML 名前スコープで指定したオブジェクトを取得するために [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出した場合、[**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) によって作成された、異なる XAML 名前スコープのオブジェクトは検索されません。 逆に、異なる XAML 名前スコープの外部で取得したオブジェクトから **FindName** を呼び出した場合は、ルート XAML 名前スコープで指定したオブジェクトは検索されません。

異なる XAML 名前スコープの問題によって影響を受けるのは、[**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 呼び出しを使って XAML 名前スコープで名前に基づいてオブジェクトを検索する処理だけです。

異なる XAML 名前スコープに定義されているオブジェクトを参照するには、次のような手法を使用できます。

-   オブジェクトツリー構造に存在することがわかっている [**親**](/uwp/api/windows.ui.xaml.frameworkelement.parent) またはコレクションのプロパティ (たとえば、 [**Panel. Children**](/uwp/api/windows.ui.xaml.controls.panel.children)によって返されたコレクション) を使用して、ツリー全体を個別のステップで段階的にウォークします。
-   異なる XAML 名前スコープから呼び出していて、ルート XAML 名前スコープを対象とする必要がある場合は、現在表示されているメイン ウィンドウへの参照を取得すると簡単です。 `Window.Current.Content`を呼び出す 1 行のコードを使って、現在のアプリ ウィンドウからの表示ルート (ルート XAML 要素、"コンテンツ ソース" とも呼ばれます) を取得できます。 次に [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) にキャストし、このスコープから [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出すことができます。
-   ルート XAML 名前スコープから呼び出していて、異なる XAML 名前スコープ内のオブジェクトを対象とする必要がある場合は、[**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) から返されてメイン オブジェクト ツリーに追加されたオブジェクトへの参照を、あらかじめコードに記述して保持しておくことをお勧めします。 これにより、このオブジェクトが、異なる XAML 名前スコープ内で [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出すための有効なオブジェクトになります。 このオブジェクトはグローバル変数として保持できるほか、メソッドのパラメーターを使って渡すこともできます。
-   ビジュアル ツリーを走査することにより、名前と XAML 名前スコープの考慮事項を完全に回避できます。 [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) API を使うと、位置とインデックスだけを基に、親オブジェクトと子コレクションの観点からビジュアル ツリーを走査できます。

## <a name="xaml-namescopes-in-templates"></a>テンプレートにおける XAML 名前スコープ

XAML のテンプレートを使うと、コンテンツを簡単に再利用および再適用できます。ただし、テンプレートには、テンプレート レベルで定義された名前を持つ要素も含まれている可能性があります。 その同じテンプレートが 1 ページで複数回使用される可能性があります。 このため、テンプレートに、適用先のページに依存しない独自の XAML 名前スコープを定義します。 次の例を考えてみましょう。

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

ここでは、同一のテンプレートを 2 つの異なるコントロールに適用しています。 これらのテンプレートがそれぞれ異なる XAML 名前スコープを持たなければ、テンプレート内で使われている "MyTextBlock" という名前により、名前の競合が発生することになります。 テンプレートのインスタンス化ごとに独自の XAML 名前スコープがあるため、この例では、インスタンス化されるテンプレートの各 XAML 名前スコープには、名前が 1 つだけ含まれます。 ただし、ルート XAML 名前スコープには、どちらのテンプレートの名前も含まれていません。

XAML 名前スコープが複数存在するため、テンプレートが適用されるページのスコープからテンプレート内の指定要素を検索するには、別の手法が必要になります。 オブジェクト ツリー内のオブジェクトに対して [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) を呼び出すのではなく、まずテンプレートが適用されているオブジェクトを取得し、その後で [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) を呼び出します。 コントロールの作成者が生成している規則で、適用されたテンプレートの特定の指定要素を、コントロール自体によって定義される動作の対象とする場合は、コントロール実装コードから **GetTemplateChild** メソッドを使用できます。 **GetTemplateChild** メソッドはプロテクトされているため、コントロールの作成者のみがアクセスできます。 また、パーツやテンプレート パーツに名前を付け、これらをコントロール クラスに適用される属性値として報告するために、コントロールの作成者が従う規則があります。 この方法を使うと、異なるテンプレートを適用しようとしているコントロール ユーザーは重要なパーツ名を検出できるようになります。コントロールの機能を維持するには、名前付きパーツをそのテンプレートに置き換える必要があります。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [x:Name 属性](x-name-attribute.md)
* [クイック スタート: コントロール テンプレート](/previous-versions/windows/apps/hh465374(v=win.10))
* [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)
* [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)
 