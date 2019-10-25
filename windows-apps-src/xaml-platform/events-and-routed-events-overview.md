---
description: ここでは、プログラミング言語として、Visual Basic またはビジュアルC# C++コンポーネント拡張 (C++/cx) を使用する場合、UI 定義に XAML を使用する場合の、Windows ランタイムアプリでのイベントのプログラミング概念について説明します。
title: イベントとルーティングイベントの概要
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10、uwp
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690399"
---
# <a name="events-and-routed-events-overview"></a>イベントとルーティングイベントの概要

**重要な Api**
- [**System.windows.uielement>** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**System.windows.routedeventargs.handled**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

ここでは、プログラミング言語として、Visual Basic またはビジュアルC# C++コンポーネント拡張 (C++/cx) を使用する場合、UI 定義に XAML を使用する場合の、Windows ランタイムアプリでのイベントのプログラミング概念について説明します。 イベントのハンドラーは、XAML の UI 要素の宣言の一部として割り当てることができます。また、コードでハンドラーを追加することもできます。 Windows ランタイムは*ルーティングイベント*をサポートします。特定の入力イベントとデータイベントは、イベントを発生させたオブジェクトを超えてオブジェクトで処理できます。 ルーティングイベントは、コントロールテンプレートを定義する場合や、ページまたはレイアウトコンテナーを使用する場合に便利です。

## <a name="events-as-a-programming-concept"></a>プログラミング概念としてのイベント

一般に、Windows ランタイムアプリのプログラミング時のイベントの概念は、ほとんどの一般的なプログラミング言語でのイベントモデルに似ています。 Microsoft .NET またはイベントを既に使用するC++方法がわかっている場合は、先頭にがあります。 ただし、ハンドラーのアタッチなどのいくつかの基本的なタスクを実行するために、イベントモデルの概念について理解する必要はありません。

プログラミング言語とC#して、 C++Visual Basic または/cx を使用する場合は、マークアップ (XAML) で UI が定義されます。 XAML マークアップ構文では、マークアップ要素とランタイムコードエンティティ間でイベントを接続する際の原則の一部は、ASP.NET や HTML5 などの他の Web テクノロジに似ています。

XAML で定義された UI のランタイムロジックを提供するコードは、多くの場合、*分離コード*ファイルまたは分離コードファイルと呼ばれ**ます  。** Microsoft Visual Studio ソリューションビューでは、このリレーションシップがグラフィカルに表示されます。分離コードファイルは、それが参照する XAML ページとは関係なく、入れ子になったファイルになります。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>ボタンをクリックします。イベントと XAML の概要

Windows ランタイムアプリの最も一般的なプログラミングタスクの1つは、ユーザー入力を UI にキャプチャすることです。 たとえば、UI には、ユーザーが情報を送信したり、状態を変更したりするためにクリックする必要があるボタンが含まれている場合があります。

XAML を生成することによって、Windows ランタイムアプリの UI を定義します。 この XAML は、通常、Visual Studio のデザイン画面からの出力です。 また、プレーンテキストエディターまたはサードパーティの XAML エディターで XAML を記述することもできます。 XAML の生成中に、ui 要素のプロパティ値を確立する他のすべての XAML 属性を定義するのと同時に、個々の UI 要素のイベントハンドラーをワイヤ化できます。

XAML でイベントを送信するには、既に定義されている、または後でコードビハインドで定義するハンドラーメソッドの文字列形式の名前を指定します。 たとえば、次の XAML は、属性として割り当てられた他のプロパティ ([x:Name 属性](x-name-attribute.md)、[**コンテンツ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) を持つ[**button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)オブジェクトを定義し、`ShowUpdatesButton_Click`という名前のメソッドを参照することによって、ボタンの[**click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)イベントのハンドラーをワイヤにします。

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**ヒント**  *イベントの配線*は、プログラミング用語です。 これは、イベントの発生が名前付きハンドラーメソッドを呼び出す必要があることを示すプロセスまたはコードを指します。 ほとんどの手続き型コードモデルでは、イベントの配線は、イベントとメソッドの両方に名前を持つ暗黙的または明示的な "AddHandler" コードであり、通常はターゲットオブジェクトのインスタンスが関係します。 XAML では、"AddHandler" は暗黙的であり、イベントの配線は、オブジェクト要素の属性名としてイベントに名前を付け、その属性の値としてハンドラーに名前を付けるだけで構成されます。

アプリのすべてのコードと分離コードに使用しているプログラミング言語で、実際のハンドラーを記述します。 属性 `Click="ShowUpdatesButton_Click"`では、XAML がマークアップコンパイルおよび解析されるときに、IDE のビルドアクションの XAML マークアップコンパイルステップと、アプリの読み込み時の最終的な XAML 解析の両方で、アプリの co の一部として `ShowUpdatesButton_Click` という名前のメソッドを見つけることができるコントラクトを作成しました。解放. `ShowUpdatesButton_Click` は、[**クリック**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)イベントの任意のハンドラーに対して、(デリゲートに基づく) 互換性のあるメソッドシグネチャを実装するメソッドである必要があります。 たとえば、次のコードでは、`ShowUpdatesButton_Click` ハンドラーを定義しています。

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

この例では、`ShowUpdatesButton_Click` メソッドは[**Routedeventhandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler)デリゲートに基づいています。 これは、[**クリック**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)メソッドの構文でという名前のデリゲートが表示されるため、これが使用するデリゲートであることがわかります。

**ヒント**: Visual Studio では、XAML を編集するときに、イベントハンドラーに名前を指定し、ハンドラーメソッドを定義するための便利な方法が用意されてい  ます。 XAML テキストエディターでイベントの属性名を指定する場合は、Microsoft IntelliSense の一覧が表示されるまでしばらく待ちます。 一覧から [**新しいイベントハンドラー&gt;を&lt;** ] をクリックすると、Microsoft Visual Studio によって、要素の**x:Name** (または型名)、イベント名、および数値サフィックスに基づいてメソッド名が提案されます。 次に、選択したイベントハンドラー名を右クリックし、[**イベントハンドラーに移動] を**クリックします。 これにより、XAML ページの分離コードファイルのコードエディタービューに表示される、新しく挿入されたイベントハンドラー定義に直接移動します。 イベントハンドラーには、*送信側*のパラメーターと、イベントが使用するイベントデータクラスを含む、正しいシグネチャが既に存在します。 また、適切なシグネチャを持つハンドラーメソッドが既に分離コードに存在する場合、そのメソッドの名前は、[**新しいイベントハンドラーを&lt;]&gt;** オプションと共に [オートコンプリート] ドロップダウンに表示されます。 また、IntelliSense の一覧の項目をクリックする代わりに、ショートカットとして Tab キーを押すこともできます。

## <a name="defining-an-event-handler"></a>イベントハンドラーの定義

UI 要素であり、XAML で宣言されているオブジェクトの場合、イベントハンドラーコードは、XAML ページの分離コードとして機能する部分クラスで定義されます。 イベントハンドラーは、XAML に関連付けられている部分クラスの一部として記述するメソッドです。 これらのイベントハンドラーは、特定のイベントが使用するデリゲートに基づいています。 イベントハンドラーメソッドは、パブリックまたはプライベートにすることができます。 XAML によって作成されたハンドラーとインスタンスが最終的にコード生成によって結合されるため、プライベートアクセスが機能します。 一般に、イベントハンドラーメソッドはクラス内でプライベートにすることをお勧めします。

**注**  のイベントハンドラー C++が部分クラスで定義されていないため、ヘッダーでプライベートクラスメンバーとして宣言されています。 C++プロジェクトのビルドアクションでは、XAML 型システムとのC++分離コードモデルをサポートするコードの生成が行われます。

### <a name="the-sender-parameter-and-event-data"></a>*Sender*パラメーターとイベントデータ

イベントに対して記述するハンドラーは、ハンドラーが呼び出される各ケースの入力として使用できる2つの値にアクセスできます。 最初の値は*sender*で、ハンドラーがアタッチされているオブジェクトへの参照です。 *Sender*パラメーターは、基本**オブジェクト**型として型指定されます。 一般的な方法は、*送信者*をより正確な型にキャストすることです。 この手法は、*送信側*オブジェクト自体の状態を確認または変更する場合に便利です。 独自のアプリ設計に基づいて、通常は、ハンドラーがアタッチされている場所または他のデザインの詳細に基づいて、*送信者*を安全にキャストできる型がわかっています。

2番目の値はイベントデータであり、通常は、 *e*パラメーターとして構文定義に表示されます。 使用できるイベントデータのプロパティを検出するには、処理する特定のイベントに割り当てられているデリゲートの*e*パラメーターを参照し、Visual Studio で IntelliSense またはオブジェクトブラウザーを使用します。 または、Windows ランタイムリファレンスドキュメントを使用することもできます。

イベントによっては、イベントが発生したことを把握するために、イベントデータの固有のプロパティ値が重要になります。 これは、特に入力イベントの場合に当てはまります。 ポインターイベントの場合、イベントが発生したときのポインターの位置が重要になることがあります。 キーボードイベントの場合、すべての可能なキー押下で[**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)イベントと[**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)イベントが起動されます。 ユーザーが押したキーを確認するには、イベントハンドラーで使用できる[**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs)にアクセスする必要があります。 入力イベントの処理の詳細については、「[キーボード操作](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)」および「[ポインター入力の処理](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input)」を参照してください。 多くの場合、入力イベントと入力シナリオには、ポインターイベントのポインターキャプチャや、キーボードイベントの修飾子キーやプラットフォームキーコードなど、このトピックでは説明されていない追加の考慮事項があります。

### <a name="event-handlers-that-use-the-async-pattern"></a>**非同期**パターンを使用するイベントハンドラー

場合によっては、イベントハンドラー内で**非同期**パターンを使用する api を使用する必要があります。 たとえば、 [**Appbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)の[**ボタン**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)を使用して、ファイルピッカーを表示し、操作することができます。 ただし、ファイルピッカー Api の多くは非同期です。 これらは**非同期**/awaitable スコープ内で呼び出す必要があり、コンパイラによって強制的に適用されます。 それでは、イベントハンドラーに**async**キーワードを追加して、ハンドラーが**非同期**に**void**になるようにします。 これで、イベントハンドラーは**非同期**の/awaitable 呼び出しを行うことができます。

**非同期**パターンを使用したユーザー操作イベント処理の例については、「[ファイルアクセスとピッカー](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10)) 」 (「」[または「 C# Visual Basic シリーズを使用した初めての Windows ランタイムアプリの作成](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10))」を参照してください) を参照してください。 「C での非同期 Api の呼び出し」も参照してください。

## <a name="adding-event-handlers-in-code"></a>コードでのイベントハンドラーの追加

XAML は、イベントハンドラーをオブジェクトに割り当てる唯一の方法ではありません。 XAML で使用できないオブジェクトを含め、コード内の特定のオブジェクトにイベントハンドラーを追加するには、言語固有の構文を使用してイベントハンドラーを追加します。

でC#は、構文は`+=`演算子を使用します。 ハンドラーを登録するには、演算子の右側にあるイベントハンドラーメソッド名を参照します。

コードを使用して、実行時の UI に表示されるオブジェクトにイベントハンドラーを追加する場合の一般的な方法は、オブジェクトの有効期間イベントまたはコールバック ( [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) 、 [**onapplytemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)など) に応答してそのようなハンドラーを追加して、関連オブジェクトは、実行時にユーザーが開始したイベントに対して準備ができています。 この例では、ページ構造の XAML アウトラインを示し、イベントC#ハンドラーをオブジェクトに追加するための言語構文を示します。

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

より詳細な構文が存在する  に**注意**してください。 2005ではC# 、デリゲートの推論と呼ばれる機能を追加しました。これにより、コンパイラは新しいデリゲートインスタンスを推論し、以前の単純な構文を有効にすることができます。 Verbose 構文は、前の例と機能的には同じですが、登録前に新しいデリゲートインスタンスが明示的に作成されるため、デリゲートの推定を利用することはできません。 この明示的な構文はあまり一般的ではありませんが、コード例によっては依然として表示される場合があります。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 構文には2つの可能性があります。 1つは、構文C#を並列処理して、ハンドラーをインスタンスに直接アタッチすることです。 これには、 **AddHandler**キーワードと、ハンドラーメソッド名を逆参照する**AddressOf**演算子が必要です。

Visual Basic 構文のもう1つのオプションは、イベントハンドラーで**Handles**キーワードを使用することです。 この手法は、ハンドラーが読み込み時にオブジェクトに存在し、オブジェクトの有効期間全体にわたって保持される場合に適しています。 XAML で定義されているオブジェクトで**ハンドル**を使用するには、 **x:Name** / **名前**を指定する必要があります。 この名前は、インスタンスに必要なインスタンス修飾子になります。 **handle**構文の*イベント*部分。 この場合、他のイベントハンドラーのアタッチを開始するために、オブジェクトの有効期間ベースのイベントハンドラーは必要ありません。**ハンドル**接続は、XAML ページをコンパイルすると作成されます。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

Visual Studio とその XAML デザイン**サーフェイス  、** 通常、 **Handles**キーワードではなく、インスタンス処理の手法を昇格させます。 これは、XAML でイベントハンドラーの配線を確立することが、一般的なデザイナーの開発者のワークフローの一部で**あり、xaml**でのイベントハンドラーの配線とは互換性がないためです。

/Cx C++では、 **+=** 構文も使用しますが、基本的なC#形式とは異なる点があります。

- デリゲートの推論は存在しないため、delegate インスタンスには**ref new**を使用する必要があります。
- デリゲートコンストラクターには2つのパラメーターがあり、最初のパラメーターとしてターゲットオブジェクトが必要です。 通常は、**これ**を指定します。
- デリゲートコンストラクターでは、2番目のパラメーターとしてメソッドアドレスが必要なので、 **&** 参照演算子はメソッド名の前にあります。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>コード内のイベントハンドラーの削除

通常、コードでイベントハンドラーを追加した場合でも、コード内でイベントハンドラーを削除する必要はありません。 ページやコントロールなど、ほとんどの Windows ランタイムオブジェクトのオブジェクトの有効期間の動作は、メイン[**ウィンドウ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)とそのビジュアルツリーから切断されたときにオブジェクトを破棄し、デリゲート参照も破棄されます。 .NET では、ガベージコレクションを通じてC++これを実行し、/cx を使用して Windows ランタイム既定で弱い参照を使用します。

イベントハンドラーを明示的に削除することが必要になる場合があります。 これには次のものが含まれます。

- 静的イベント用に追加したハンドラーは、従来の方法ではガベージコレクトできません。 Windows ランタイム API の静的イベントの例として、 [**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget)クラスと[**Clipboard**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard)クラスのイベントがあります。
- ハンドラーの削除のタイミングを即時にするコードをテストするか、実行時にイベントの古い/新しいイベントハンドラーを交換するコードをテストします。
- カスタム**remove**アクセサーの実装。
- カスタムの静的イベント。
- ページナビゲーションのハンドラー。

[**FrameworkElement. アンロード**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded)または[**NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)は、他のイベントのハンドラーを削除するために使用できる、状態管理とオブジェクトの有効期間の適切な位置を持つイベントトリガーです。

たとえば、このコードを使用して、ターゲットオブジェクト**textBlock1**から入力され**た textBlock1\_ポインター**という名前のイベントハンドラーを削除できます。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

また、イベントが XAML 属性を使用して追加された場合のハンドラーを削除することもできます。これは、生成されたコードでハンドラーが追加されたことを意味します。 ハンドラーがアタッチされた要素の**名前**値を指定した場合は、後でコードのオブジェクト参照が提供されるため、これは簡単に実行できます。ただし、オブジェクトに**名前**がない場合に必要なオブジェクト参照を見つけるために、オブジェクトツリーをウォークすることもできます。

/Cx でC++イベントハンドラーを削除する必要がある場合は、登録トークンが必要です。これは、`+=`イベントハンドラーの登録の戻り値から受け取る必要があります。 これは、 C++/cx 構文で `-=` 登録解除の右側に使用する値が、メソッド名ではなくトークンであるためです。 C++/Cx では、 C++/cx によって生成されたコードでトークンが保存されないため、XAML 属性として追加されたハンドラーを削除することはできません。

## <a name="routed-events"></a>ルーティングイベント

Windows ランタイム、Microsoft C#Visual Basic またはC++/cx では、ほとんどの UI 要素に存在する一連のイベントのルーティングイベントの概念がサポートされています。 これらのイベントは、入力およびユーザー操作のシナリオに使用され、 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)の基底クラスに実装されます。 ルーティングイベントの入力イベントの一覧を次に示します。

- [**Bring@ View要求**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**受信した文字**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**System.windows.dragdrop.dragenter>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**System.windows.dragdrop.dragleave>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**System.windows.dragdrop.dragover>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [ **」** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**System.windows.uielement.gotfocus>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**保留**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**System.windows.uielement.manipulationcompleted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**System.windows.uielement.manipulationdelta>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**System.windows.uielement.manipulationinertiastarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**System.windows.uielement.manipulationstarted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**System.windows.uielement.manipulationstarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**ポインタが取り消されました**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**入力されたポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**終了したポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**移動されたポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**押されたポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**解放されたポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**プレビューの Keydown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**プレビューの Keyup**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**変圧**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

ルーティングイベントは、子オブジェクトからオブジェクトツリー内の連続する各親オブジェクトに対して (*ルーティング*) される可能性のあるイベントです。 UI の XAML 構造は、このツリーのルートを XAML のルート要素として、このツリーに近似します。 オブジェクトツリーには、プロパティ要素タグなどの XAML 言語機能が含まれていないため、実際のオブジェクトツリーは XAML 要素の入れ子とは多少異なる場合があります。 ルーティングイベントは、イベントを発生させるすべての XAML オブジェクト要素の子要素から、それを含む親オブジェクト要素に対して、*バブル*として思い描くできます。 イベントとそのイベントデータは、イベントルートに沿って複数のオブジェクトで処理できます。 要素がハンドラーを持っていない場合は、ルート要素に到達するまでルートが維持される可能性があります。

ダイナミック HTML (DHTML) や HTML5 などの Web テクノロジがわかっている場合は、*バブル*イベントの概念を既に理解している可能性があります。

ルーティングイベントがイベントルートを通じてバブルする場合、すべてのアタッチされたイベントハンドラーは、イベントデータの共有インスタンスにアクセスします。 したがって、いずれかのイベントデータがハンドラーによって書き込み可能である場合、イベントデータに加えられた変更は、次のハンドラーに渡され、イベントからの元のイベントデータを表すことができなくなる可能性があります。 イベントにルーティングイベントの動作がある場合、参照ドキュメントには、ルーティング動作に関する注釈やその他の表記が含まれます。

### <a name="the-originalsource-property-of-routedeventargs"></a>**System.windows.routedeventargs.handled**の**originalsource**プロパティ

イベントがイベントルートをバブルアップすると、*送信側*はイベント発生オブジェクトと同じオブジェクトではなくなります。 代わりに、 *sender*は、呼び出されるハンドラーがアタッチされているオブジェクトです。

場合によっては、*送信側*が興味深いものではなく、ポインターイベントが発生したときにポインターが置かれている子オブジェクトのうちのどれか、またはユーザーがキーボードキーを押したときにフォーカスを保持した、より大きな UI 内のどのオブジェクトかなどの情報が必要になることがあります。 このような場合は、 [**Originalsource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource)プロパティの値を使用できます。 ルート上のすべてのポイントで、 **Originalsource**は、ハンドラーがアタッチされているオブジェクトではなく、イベントを発生させた元のオブジェクトを報告します。 ただし、 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)入力イベントの場合、元のオブジェクトは、ページレベルの UI 定義の XAML にすぐに表示されないオブジェクトであることがよくあります。 代わりに、元のソースオブジェクトは、コントロールのテンプレート化された部分である可能性があります。 たとえば、ユーザーが[**ボタン**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)の端の上にポインターを置いた場合、ほとんどのポインターイベントに対して、 **originalsource**は[**テンプレート**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template)内の[**境界**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)テンプレートパーツであり、**ボタン**自体ではありません。

**ヒント**  入力イベントバブルは、テンプレートコントロールを作成する場合に特に便利です。 テンプレートを持つコントロールは、そのコンシューマーによって適用された新しいテンプレートを持つことができます。 作業用テンプレートを再作成しようとしているコンシューマーは、既定のテンプレートで宣言されている一部のイベント処理を意図せず排除する場合があります。 クラス定義で[**Onapplytemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)オーバーライドの一部としてハンドラーをアタッチすることで、引き続きコントロールレベルのイベント処理を行うことができます。 その後、インスタンス化時にコントロールのルートにバブルアップする入力イベントをキャッチできます。

### <a name="the-handled-property"></a>**処理**されたプロパティ

特定のルーティングイベントのいくつかのイベントデータクラスには、 **Handled**という名前のプロパティが含まれています。 例については、「 [**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled)、 [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)、 [**system.windows.drageventargs.effects**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled)」を参照してください。 **処理**されるすべてのケースでは、設定可能なブール型プロパティです。

**Handled**プロパティを**true**に設定すると、イベントシステムの動作に影響します。 **処理**が**true**の場合、ほとんどのイベントハンドラーでルーティングが停止します。イベントは、その特定のイベントケースの他のアタッチされたハンドラーに通知するために、ルートに沿って続行されません。 イベントのコンテキストにおける "処理済み" とは、アプリがどのように反応するかということです。 基本的に、"**処理**済み" は、イベントの発生がどのコンテナーにもバブルされる必要がないことをアプリコードで示すことができる単純なプロトコルで、アプリロジックは、実行する必要がある処理を実行しています。 逆に、組み込みのシステムまたはコントロールの動作が動作するように、バブルする必要があるイベントを処理しないように注意する必要があります。たとえば、選択コントロールの1つまたは複数の項目内で低レベルのイベントを処理すると、悪影響を及ぼす可能性があります。 選択コントロールでは、選択内容を変更する必要があることを認識するために入力イベントを探している場合があります。

ルーティングイベントによっては、このようにルートを取り消すことができないものもあります。また、これらのイベントには、**処理**されたプロパティがないため、このような通知を受け取ることができます。 たとえば、 [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)と[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)はバブルを行いますが、常にルートにバブルするので、イベントデータクラスにはその動作に影響を与える**ハンドル**されるプロパティがありません。

##  <a name="input-event-handlers-in-controls"></a>コントロール内のイベントハンドラーの入力

特定の Windows ランタイムコントロールは、内部的に入力イベントに対して**処理**された概念を使用することがあります。 これは、ユーザーコードが処理できないため、入力イベントが発生しないように見える可能性があります。 たとえば、 [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)クラスには、押された一般的な入力イベント[**ポインター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)を意図的に処理するロジックが含まれています。 これは、ポインターが押された入力によって開始される[**クリック**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)イベントと、フォーカスされたときにボタンを呼び出すことができる Enter キーのようなキーの処理など、他の入力モードによって起動されるためです。 **ボタン**のクラスデザインのために、未加工の入力イベントは概念的に処理され、ユーザーコードなどのクラスコンシューマーは、代わりにコントロール関連の**Click**イベントと対話できます。 Windows ランタイム API リファレンスの特定のコントロールクラスに関するトピックでは、多くの場合、クラスが実装するイベント処理動作に注意してください。 場合によっては、_イベント_メソッド**でを**オーバーライドすることによって動作を変更できます。 たとえば、 [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown)をオーバーライドすることによって、[**テキストボックス**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)の派生クラスがキー入力にどのように反応するかを変更できます。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>既に処理されているルーティングイベントのハンドラーを登録する

前に、[**処理**済み] を**true**に設定すると、ほとんどのハンドラーが呼び出されなくなりました。 ただし、 [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)メソッドは、ルートの前にある他のハンドラーが共有イベントデータで **[true]** に設定されている場合でも、ルートに対して**常に呼び出さ**れるハンドラーをアタッチできる手法を提供します。 この手法は、使用しているコントロールが内部の複合またはコントロール固有のロジックでイベントを処理した場合に便利です。 それでも、コントロールインスタンスまたはアプリ UI から応答する必要があります。 ただし、この手法は、**処理**の目的と矛盾し、場合によってはコントロールの操作が中断される可能性があるため、注意して使用してください。

識別子が**addhandler**メソッドの必須の入力であるため、対応するルーティングイベント識別子を持つルーティングイベントのみが、 [**addhandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)イベント処理手法を使用できます。 ルーティングイベント識別子を使用できるイベントの一覧については、 [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)のリファレンスドキュメントを参照してください。 ほとんどの場合、これは前に示したルーティングイベントのリストと同じです。 例外として、リスト内の最後の2つの ( [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)と[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) ) はルーティングイベント識別子を持たないので、これらの id には**AddHandler**を使用できません。

## <a name="routed-events-outside-the-visual-tree"></a>ビジュアルツリー外のルーティングイベント

特定のオブジェクトは、概念的にメインビジュアルに重ねて表示されている主要なビジュアルツリーとの関係に関与します。 これらのオブジェクトは、すべてのツリー要素をビジュアルルートに接続する通常の親子リレーションシップの一部ではありません。 これは、表示されている[**ポップアップ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)または[**ツールヒント**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)の場合です。 **ポップアップ**またはツール**ヒント**からのルーティングイベントを処理する**場合は、** ポップアップ**またはツール**ヒントの要素ではなく、**ポップアップまたは** **ツールヒント**内の特定の UI 要素にハンドラーを配置します。 **ポップアップ**または**ツールヒント**のコンテンツに対して実行される複合では、ルーティングに依存しないでください。 これは、ルーティングイベントのイベントルーティングは、メインビジュアルツリーに沿ってのみ動作するためです。 **ポップアップ**または**ツールヒント**は、関連する UI 要素の親とは見なされません。また、入力イベントのキャプチャ領域として**ポップアップ**の既定の背景のようなものを使用しようとしている場合でも、ルーティングイベントを受け取ることはありません。

## <a name="hit-testing-and-input-events"></a>ヒットテストと入力イベント

UI 内の要素がマウス、タッチ、およびスタイラス入力に対して認識されるかどうかを判断することを、*ヒットテスト*と呼びます。 Touch アクションや、タッチアクションの結果である相互作用固有または操作イベントの場合は、要素をイベントソースにするためにヒットテストを表示し、アクションに関連付けられたイベントを発生させる必要があります。 それ以外の場合、アクションは、要素を、その入力と対話できるビジュアルツリー内の基になる要素または親要素に渡します。 ヒットテストに影響する要因はいくつかありますが、 [**system.windows.uielement.ishittestvisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible)プロパティをチェックすることによって、特定の要素が入力イベントを起動できるかどうかを判断できます。 このプロパティは、要素が次の条件を満たしている場合にのみ**true**を返します。

- 要素の[**可視性**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility)プロパティ値が[**表示**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility)されます。
- 要素の**背景**または**Fill**プロパティの値が**null**ではありません。 [**ブラシ**](/uwp/api/Windows.UI.Xaml.Media.Brush)の値が**null**の場合は、透明度とヒットテストが表示されません。 (要素を透明にし、テスト可能にするには、 **null**ではなく[**透明**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent)なブラシを使用します)。

**メモの**  **背景**と**塗りつぶし**は、 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)によって定義されていません。代わりに、[**コントロール**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)や[**図形**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)などのさまざまな派生クラスによって定義されます。 ただし、前景と背景のプロパティに使用するブラシの影響は、プロパティを実装するサブクラスに関係なく、ヒットテストと入力イベントで同じです。

- 要素がコントロールの場合、その[**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)プロパティの値は**true**である必要があります。
- 要素のレイアウトには実際のディメンションが必要です。 [**Actualheight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)と[**actualheight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)が0の要素は、入力イベントを起動しません。

一部のコントロールには、ヒットテスト用の特別なルールがあります。 たとえば、 [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)には**Background**プロパティはありませんが、そのディメンションの領域全体でテスト可能な状態になります。 [**画像**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)および[**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)コントロールは、表示されるメディアソースファイル内のアルファチャネルなどの透明なコンテンツに関係なく、定義されている四角形の寸法に対してヒットテストが行われます。 Web[**ビューコントロールに**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)は、ホストされた HTML および起動スクリプトイベントによって入力を処理できるため、特別なヒットテスト動作があります。

ほとんどの[**パネル**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)クラスと[**境界線**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)は、独自の背景ではヒットテストが行われませんが、含まれている要素からルーティングされるユーザー入力イベントを処理できます。

要素のヒットテストが発生したかどうかに関係なく、ユーザー入力イベントと同じ位置にある要素を特定できます。 これを行うには、 [**Findelementsinhostcoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)メソッドを呼び出します。 名前が示すように、このメソッドは、指定されたホスト要素を基準とした相対的な位置にある要素を検索します。 ただし、適用された変換とレイアウトの変更によって、要素の相対座標系を調整できるため、特定の位置にある要素に影響します。

## <a name="commanding"></a>コマンド実行

いくつかの UI 要素は、*コマンドのコマンド*をサポートしています。 コマンドは、基になる実装で入力関連のルーティングイベントを使用し、単一のコマンドハンドラーを呼び出して関連する UI 入力 (特定のポインターアクション、特定のアクセラレータキー) の処理を有効にします。 UI 要素に対してコマンドを使用できる場合は、不連続な入力イベントではなく、コマンドのコマンドを使用することを検討してください。 通常は、データのビューモデルを定義するクラスのプロパティに**バインド**参照を使用します。 プロパティは、言語固有の**ICommand**コマンドパターンを実装する名前付きコマンドを保持します。 詳細については、「 [**Buttonbase. コマンド**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)」を参照してください。

## <a name="custom-events-in-the-windows-runtime"></a>Windows ランタイム内のカスタムイベント

カスタムイベントを定義する目的で、イベントをどのように追加するか、およびクラスの設計でどのような意味を持つかは、使用しているプログラミング言語に大きく依存しています。

- C#と Visual Basic では、CLR イベントを定義します。 カスタムアクセサー (**追加**/**削除**) を使用していない限り、標準の .net イベントパターンを使用できます。 その他のヒント:
    - イベントハンドラーには、Windows ランタイム汎用イベントデリゲート[**EventHandler<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)に組み込みの変換が含まれているため、 [**eventhandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)を使用することをお勧めします。
    - Windows ランタイムに変換されないため、イベントデータクラスを[**EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs)に基づいて作成しないでください。 既存のイベントデータクラスを使用するか、基底クラスをまったく使用しません。
    - カスタムアクセサーを使用している場合は、「 [Windows ランタイムコンポーネント」の「カスタムイベントとイベントアクセサー](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140))」を参照してください。
    - 標準的な .NET イベントパターンの詳細については、「[カスタム Silverlight クラスのイベントの定義](https://docs.microsoft.com/previous-versions/windows/)」を参照してください。 これは Microsoft Silverlight 用に記述されていますが、標準的な .NET イベントパターンのコードと概念を十分に理解しています。
- /Cx C++については、「[イベント (C++/cx)](https://docs.microsoft.com/cpp/cppcx/events-c-cx)」を参照してください。
    - 独自のカスタムイベントを使用する場合でも、名前付き参照を使用します。 カスタムイベントにはラムダを使用しないでください。循環参照を作成できます。

Windows ランタイムに対してカスタムルーティングイベントを宣言することはできません。ルーティングイベントは、Windows ランタイムに由来するセットに限定されます。

カスタムイベントの定義は、通常、カスタムコントロールを定義する作業の一部として行われます。 これは、プロパティ変更コールバックを持つ依存関係プロパティを持つ一般的なパターンであり、一部またはすべてのケースで依存関係プロパティのコールバックによって発生するカスタムイベントも定義します。 コントロールのコンシューマーには、定義したプロパティ変更コールバックへのアクセス権はありませんが、通知イベントが使用可能な場合は、次のことをお勧めします。 詳細については、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [クイックスタート: タッチ入力](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [キーボード操作](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [.NET のイベントとデリゲート](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [Windows ランタイムコンポーネントの作成](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
