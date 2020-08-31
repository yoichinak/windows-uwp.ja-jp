---
description: このトピックでは、Windows ランタイム アプリで、プログラミング言語に C#、Visual Basic、または Visual C++ コンポーネント拡張機能 (C++/CX)、UI 定義に XAML を使う場合のイベントのプログラミングの概念について説明します。
title: イベントとルーティング イベントの概要
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67ee1f31e1480e0aa805e161433976d5e789d00c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155096"
---
# <a name="events-and-routed-events-overview"></a>イベントとルーティング イベントの概要

**重要な API**
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

このトピックでは、Windows ランタイム アプリで、プログラミング言語に C#、Visual Basic、または Visual C++ コンポーネント拡張機能 (C++/CX)、UI 定義に XAML を使う場合のイベントのプログラミングの概念について説明します。 イベントのハンドラーは、UI 要素の宣言の一部として XAML で割り当てることも、コードで追加することもできます。 Windows ランタイムは*ルーティング イベント*をサポートしており、特定の入力イベントとデータ イベントを、その発生元オブジェクト以外のオブジェクトで処理できます。 ルーティング イベントは、コントロール テンプレートを定義する際や、ページまたはレイアウト コンテナーを使う際に役立ちます。

## <a name="events-as-a-programming-concept"></a>プログラミングの概念としてのイベント

一般に、Windows ランタイム アプリのプログラミングを行う際のイベントの概念は、最も一般的なプログラミング言語のイベント モデルと似ています。 Microsoft .NET または C++ のイベントの操作方法を把握していれば、スムーズに理解できます。 ただし、ハンドラーのアタッチのような基本的なタスクを行うために、イベント モデルの概念について詳しく学ぶ必要はありません。

プログラミング言語として C#、Visual Basic、または C++/CX を使用する場合、UI はマークアップ (XAML) で定義されます。 XAML マークアップの構文で、マークアップ要素とランタイム コード エンティティの間でイベントを関連付けるときの原則の一部は、他の Web テクノロジ (ASP.NET、HTML5 など) と似ています。

**メモ**   XAML で定義された UI のランタイムロジックを提供するコードは、多くの場合、*分離コード*ファイルまたは分離コードファイルと呼ばれます。 Microsoft Visual Studio のソリューション ビューでは、コード ビハインド ファイルが参照先の XAML ページに対して依存する入れ子のファイルとして表示されて、この関係がグラフィカルに示されます。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click: イベントと XAML の概要

Windows ランタイム アプリの最も一般的なプログラミング タスクの 1 つは、ユーザー入力を UI に取り込むことです。 たとえば、UI には、ユーザーが情報を送信、または状態を変更するためにクリックする必要のあるボタンが組み込まれることがあります。

Windows ランタイム アプリの UI を定義するには、XAML を生成します。 この XAML は、通常は Visual Studio のデザイン サーフェイスからの出力です。 また、プレーンテキスト エディターやサードパーティ製の XAML エディターで記述することもできます。 この XAML を生成するときに、個々の UI 要素のプロパティ値を設定するすべての XAML 属性を定義する際に、その UI 要素にイベント ハンドラーを関連付けることができます。

XAML でイベントを記述する場合は、コード ビハインドで既に定義してあるものも、これから定義するものも含め、ハンドラー メソッドの文字列形式の名前を指定します。 たとえば、この XAML は、他のプロパティ ([x:Name 属性](x-name-attribute.md)、[**コンテンツ**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) を属性として割り当てたうえで[**ボタン**](/uwp/api/Windows.UI.Xaml.Controls.Button) オブジェクトを定義し、`ShowUpdatesButton_Click` というメソッドを参照してボタンの[**クリック**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントのハンドラーを関連付けます。

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**ヒント:**  *イベントの関連付け*は、プログラミング用語です。 これは、イベントが発生して名前付けされたハンドラー メソッドを呼び出すことを示すプロセスやコードのことを指します。 ほとんどの手続き型コード モデルで、イベントの関連付けはイベントとメソッドの両方の名前を付ける暗黙的または明示的な "AddHandler" コードで、通常ターゲット オブジェクト インスタンスが関係しています。 XAML では、「AddHandler」は暗黙的であり、イベントの関連付けは、すべてオブジェクト要素の属性名としてのイベントの名前付けと、属性値としてのハンドラーの名前付けで構成されています。

実際のハンドラーは、アプリのすべてのコードとコード ビハインドで使っているプログラミング言語で記述します。 ここでは、`Click="ShowUpdatesButton_Click"` という属性を使ってコントラクトを作成しています。このコントラクトにより、XAML のマークアップ コンパイルと解析の際に、IDE のビルド アクションの XAML マークアップ コンパイル ステップと、アプリの読み込み時の最終的な XAML の解析で、`ShowUpdatesButton_Click` という名前のメソッドをアプリのコードの中から検出できます。 `ShowUpdatesButton_Click` は、 [**クリック**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントのハンドラーに対して、(デリゲートに基づく) 互換性のあるメソッドシグネチャを実装するメソッドである必要があります。 たとえば、次のコードは `ShowUpdatesButton_Click` ハンドラーを定義します。

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

この例では、`ShowUpdatesButton_Click` メソッドは [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) デリゲートに基づいています。 これは、 [**クリック**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) メソッドの構文でという名前のデリゲートが表示されるため、これが使用するデリゲートであることがわかります。

**ヒント**   Visual Studio には、XAML の編集中にイベントハンドラーに名前を指定し、ハンドラーメソッドを定義する便利な方法が用意されています。 XAML テキスト エディターでイベントの属性名を入力する際には、Microsoft IntelliSense リストが表示されるまで少し待ってください。 一覧から [ ** &lt; 新しいイベントハンドラー &gt; ** ] をクリックすると、Microsoft Visual Studio によって、要素の**x:Name** (または型名)、イベント名、および数値サフィックスに基づいてメソッド名が提案されます。 その後、選択したイベント ハンドラー名を右クリックし、**[イベント ハンドラーへ移動]** をクリックできます。 そうすると、新たに挿入されたイベント ハンドラー定義に直接移動します。これは、XAML ページのコード ビハインド ファイルのコード エディター ビューで確認できます。 イベント ハンドラーには既に正しいシグネチャが設定されています (*sender* パラメーターや、イベントが使う特定のイベント データ クラスなど)。 また、コードビハインドに正しいシグネチャを持つハンドラーメソッドが既に存在する場合、そのメソッドの名前は、[ ** &lt; 新しいイベントハンドラー &gt; ** ] オプションと共に [オートコンプリート] ドロップダウンに表示されます。 また、IntelliSense のリスト項目をクリックする代わりに、ショートカットとして Tab キーを押すこともできます。

## <a name="defining-an-event-handler"></a>イベント ハンドラーの定義

オブジェクトが UI 要素であり、XAML で宣言される場合、イベント ハンドラー コードは、XAML ページのコード ビハインドとなる部分クラスに定義します。 イベント ハンドラーは、XAML に関連付けられた部分クラスの一部として記述するメソッドです。 これらのイベント ハンドラーは、特定のイベントが使用するデリゲートに基づきます。 イベント ハンドラー メソッドは、public と private のどちらでもかまいません。 アクセス レベルを private に設定できるのは、XAML によって作成されるハンドラーとインスタンスが最終的にコード生成によって結合されるためです。 通常は、イベント ハンドラー メソッドをクラス内で private にすることをお勧めします。

**メモ**   C++ のイベントハンドラーは部分クラスで定義されていません。ヘッダーでプライベートクラスメンバーとして宣言されています。 C++ プロジェクトのビルド アクションでは、XAML の型システムと C++ のコード ビハインド モデルをサポートするコードが生成されます。

### <a name="the-sender-parameter-and-event-data"></a>*sender* パラメーターとイベント データ

イベント用に記述したハンドラーは、そのハンドラーが呼び出された際に、その都度入力として使える 2 つの値にアクセスできます。 その最初の値が *sender* です。これは、ハンドラーがアタッチされているオブジェクトへの参照です。 *sender* パラメーターは、**Object** 基本型として型指定されます。 *sender* をより正確な型にキャストするという手法がよく使用されます。 この手法は、*sender* オブジェクト自体で状態を確認または変更する必要がある場合に便利です。 通常は、それぞれのアプリ設計に基づき、*sender* のキャスト先として安全な型をハンドラーのアタッチ先やその他の設計の情報を基に把握します。

2 つ目の値はイベント データです。これは通常、*e* パラメーターとして構文の定義に表示されます。 使用できるイベント データのプロパティを見つけるには、処理対象のイベントに割り当てられているデリゲートの *e* パラメーターを参照した後、Visual Studio の IntelliSense かオブジェクト ブラウザーを使います。 Windows ランタイム リファレンス ドキュメントを使うこともできます。

イベントによっては、イベントの発生を検知することと同様にイベント データの特定のプロパティ値が重要となります。 これが特に当てはまるのが入力イベントです。 ポインター イベントの場合は、イベントが発生したときのポインターの位置が重要です。 キーボード イベントの場合、キーボード上のいずれのキーが押されても [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) イベントと [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) イベントが発生します。 ユーザーがどのキーを押したかを判定するには、イベント ハンドラーで使うことができる [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) にアクセスする必要があります。 入力イベントの処理について詳しくは、「[キーボード操作](../design/input/keyboard-interactions.md)」と「[ポインター入力の処理](../design/input/handle-pointer-input.md)」をご覧ください。 入力イベントと入力シナリオに対処するには、通常、ポインター イベントのポインター キャプチャや、キーボード イベントの修飾キーとプラットフォーム キー コードなど、このトピックで取り上げていない事柄についても考慮する必要があります。

### <a name="event-handlers-that-use-the-async-pattern"></a>**async** パターンを使うイベント ハンドラー

ときには、イベント ハンドラー内で **async** パターンを使う API を使う必要が生じることもあります。 たとえば、ファイル ピッカーを表示して操作するには、[**AppBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) で [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) を使います。 ただし、ファイル ピッカー API の多くは非同期です。 それらの API は **async**/awaitable スコープ内で呼び出す必要があります。これはコンパイラの要件になります。 そのため、**async** キーワードをイベント ハンドラーに追加して、そのハンドラーを **async** **void** にする必要があります。 これで、イベント ハンドラーは **async**/awaitable での呼び出しが可能になります。

**async** パターンを使ったユーザー操作イベントの処理の例については、「[ファイル アクセスとファイル ピッカー](/previous-versions/windows/apps/jj655411(v=win.10))」(「[C# または Visual Basic を使った初めての Windows ランタイム アプリの作成](/previous-versions/windows/apps/hh974581(v=win.10))」シリーズの一部) をご覧ください。 「C での非同期 API の呼び出し」もご覧ください。

## <a name="adding-event-handlers-in-code"></a>コードでのイベント ハンドラーの追加

イベント ハンドラーをオブジェクトに割り当てる手段は、XAML 以外にもあります。 イベント ハンドラーをコードで特定のオブジェクト (XAML では使用できないオブジェクトも含む) に追加するには、言語固有のイベント ハンドラー追加構文を使用します。

C# の構文では、`+=` 演算子を使用します。 演算子の右側でイベント ハンドラー メソッド名を参照することによって、ハンドラーを登録します。

ランタイム UI に表示されるオブジェクトにイベント ハンドラーをコードで追加する場合、[**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) や [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) など、オブジェクトの有効期間イベントまたはコールバックに応じてハンドラーを追加するのが一般的です。これにより、該当するオブジェクトのイベント ハンドラーは、実行時にユーザーが発生させるイベントに対応できるようになります。 次の例は、ページ構造の XAML の概略と、イベント ハンドラーをオブジェクトに追加するための C# 言語の構文を示しています。

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

**メモ**   より詳細な構文が存在します。 2005 年に、コンパイラで新しいデリゲート インスタンスを推論できるようにするデリゲートの推論という機能が C# に追加されて、上のより単純な形式の構文を使えるようになりました。 冗長な構文は、機能的には上の例と同じですが、新しいデリゲート インスタンスを登録する前に明示的に作成します。したがって、デリゲートの推論は使用されません。 この明示的な構文は、あまり一般的ではありませんが、コード例で使わることがあります。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 構文の場合は 2 とおりの方法があります。 1 つは、C# 構文と同じように記述し、ハンドラーを直接インスタンスにアタッチする方法です。 この場合は、**AddHandler** キーワードに加え、ハンドラー メソッド名を逆参照する **AddressOf** 演算子が必要です。

もう 1 つは、イベント ハンドラーに **Handles** キーワードを指定するという方法です。 この方法は、読み込みの時点からオブジェクトの有効期間を通じてオブジェクト上にハンドラーが存在している必要がある場合に適しています。 XAML に定義されたオブジェクトで **Handles** を使うには、**Name** / **x:Name** を指定する必要があります。 この名前は、**Handles** 構文の *Instance.Event* 部分に必要なインスタンス修飾子となります。 この場合は、他のイベント ハンドラーのアタッチを開始する、オブジェクトの有効期間に基づくイベント ハンドラーは不要です。**Handles** の関連付けは、XAML ページのコンパイル時に作成されます。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**メモ**   通常、Visual Studio とその XAML デザインサーフェイスでは、 **handle**キーワードではなく、インスタンス処理の手法が昇格されます。 その理由は、XAML でのイベント ハンドラーの関連付けがデザイナーと開発者が対処する一般的なワークフローに含まれるだけでなく、**Handles** キーワードを使用する手法に XAML でのイベント ハンドラーの関連付けとの互換性がないためです。

C++/CX でも構文を使用し **+=** ますが、基本的な C# 形式とは異なる点があります。

- デリゲートの推論は行われないため、**ref new** でデリゲート インスタンスを作成する必要があります。
- デリゲート コンストラクターにパラメーターが 2 つあり、最初のパラメーターでターゲット オブジェクトを指定する必要があります。 通常は **this** を指定します。
- デリゲートコンストラクターは、2番目のパラメーターとしてメソッドアドレスを必要とするため、 **&** 参照演算子はメソッド名の前にあります。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>コードでのイベント ハンドラーの削除

イベント ハンドラーをコードで追加した場合であっても、通常はコード内のイベント ハンドラーを削除する必要はありません。 ページやコントロールなど、ほとんどの Windows ランタイム オブジェクトには、その有効期間の動作として、メインの [**Window**](/uwp/api/Windows.UI.Xaml.Window) とそのビジュアル ツリーから切断されると破棄されるという動作が備わっています。また、デリゲート参照がある場合には、それも破棄されます。 これには、.NET であればガベージ コレクション、C++/CX を備えた Windows ランタイムであれば弱参照が、それぞれ既定で使われます。

まれに、イベント ハンドラーを明示的に削除する必要が生じることがあります。 次の設定があります。

- 静的イベントのためにハンドラーを追加したものの、従来の方法でガベージ コレクションを実行できない。 Windows ランタイム API の静的イベントの例は、[**CompositionTarget**](/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) クラスと [**Clipboard**](/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) クラスのイベントです。
- テスト コードでハンドラーの削除のタイミングを即時にする必要がある、またはコードで実行時にイベントの古いイベント ハンドラーを新しいものに置き換える必要がある。
- カスタム **remove** アクセサーを実装する。
- カスタム静的イベント。
- ページ ナビゲーションのハンドラー。

他のイベントのハンドラーを削除するために使用できるよう、状態管理とオブジェクトの有効期間の適切な位置に配置できるイベント トリガーは、[**FrameworkElement.Unloaded**](/uwp/api/windows.ui.xaml.frameworkelement.unloaded) と [**Page.NavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) are possible event triggers that have appropriate positions in のいずれかです。

たとえば、このコードを使用して、ターゲットオブジェクト**textBlock1**から入力され** \_ た textBlock1 ポインター**という名前のイベントハンドラーを削除できます。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

また、XAML 属性によってイベントが追加された場合 (つまり、生成されたコードにハンドラーが追加された場合) にも、ハンドラーを削除できます。 ハンドラーがアタッチされた要素に **Name** 値を指定した場合には、後でコードにオブジェクト参照が設定されるため、こちらの方法の方が簡単です。ただし、オブジェクトに **Name** がない場合は、必要なオブジェクト参照を探すにあたって、オブジェクト ツリーを辿るという方法もあります。

C++/CX でイベント ハンドラーを削除する場合には、登録トークンが必要です。このトークンは、`+=` イベント ハンドラー登録の戻り値から受け取ります。 メソッド名ではなく、C++/CX の構文で `-=` 登録解除の右側に使われる値がトークンであるためです。 C++/CX で生成されたコードではトークンが保存されないため、C++/CX では、XAML 属性として追加されたハンドラーを削除できません。

## <a name="routed-events"></a>ルーティング イベント

Windows ランタイムと C#、Microsoft Visual Basic、または C++/CX では、ほとんどの UI 要素に存在する一連のイベントのルーティング イベントの概念がサポートされています。 これらのイベントは、入力やユーザー操作のシナリオ用であり、[**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) 基底クラスに実装されています。 ルーティング イベントである入力イベントの一覧を次に示します。

- [**Bring@ View要求**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**受信した文字**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**」**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**株式公開状況**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**プレビューの Keydown**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**プレビューの Keyup**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

ルーティング イベントとは、オブジェクト ツリーの子オブジェクトから渡され (*ルーティング*され)、一連の親オブジェクトまでルーティングされる可能性のあるイベントのことです。 UI の XAML 構造はこのツリーに類似した構造となり、このツリーのルートは XAML におけるルート要素に相当します。 実際のオブジェクト ツリーは、プロパティ要素タグなどの XAML 言語機能が含まれていないので、XAML 要素のネスト構造とはやや異なります。 ルーティング イベントは、イベント発生元の XAML オブジェクト子要素からその親オブジェクト要素へと*バブル* ルーティングされるイベントと考えることができます。 イベントとそのイベント データはイベント ルートをたどって複数のオブジェクトで処理される場合があります。 どの要素にもハンドラーがない場合は、ルート要素に達するまでイベント ルートをたどっていくことになります。

ダイナミック HTML (DHTML) や HTML5 などの Web テクノロジについて知識がある場合は、既に*バブル* イベントの概念をご存じかもしれません。

ルーティング イベントがイベント ルートをたどってバブル ルーティングされるとき、アタッチされたすべてのイベント ハンドラーはイベント データのインスタンスを共有し、同じインスタンスにアクセスします。 したがって、ハンドラーによる書き込みが可能なイベント データがある場合は、イベント データが変更されると変更後のイベント データが次のハンドラーに渡されるため、そのイベントの元のイベント データを表さなくなる可能性があります。 ルーティング イベントの動作を持つイベントは、リファレンス ドキュメントにルーティング動作に関する注釈が含まれています。

### <a name="the-originalsource-property-of-routedeventargs"></a>**RoutedEventArgs** の **OriginalSource** プロパティ

イベントがバブル ルーティングによって移動すると、*sender* はイベントが発生したオブジェクトと同じものではなくなります。 *sender* は、呼び出されたハンドラーがアタッチされているオブジェクトです。

場合によっては、注目されるのは *sender* ではなく、ポインター イベントが発生したときにどの子オブジェクトにポインターが置かれているかや、ユーザーがキーボードのキーを押したときに上位の UI のどのオブジェクトにフォーカスがあったかなどの情報です。 そのような場合には、[**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) プロパティの値を利用できます。 **OriginalSource** は、ルートのすべての位置で、ハンドラーがアタッチされているオブジェクトではなく、イベントを発生させた元のオブジェクトを報告します。 ただし、[**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) の入力イベントでは、そのイベント発生元のオブジェクトはページ レベルの UI 定義 XAML ですぐに見つかるオブジェクトではありません。 コントロールのテンプレート パーツである場合もよくあります。 たとえば、ユーザーが [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) の端にポインターをホバーした場合、ほとんどのポインター イベントでは、**OriginalSource** は **Button** そのものではなく、[**Template**](/uwp/api/windows.ui.xaml.controls.control.template) の [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) テンプレート パーツになります。

**ヒント**   入力イベントバブルは、テンプレートコントロールを作成する場合に特に便利です。 テンプレート化されたすべてのコントロールでは、ユーザーによって新しいテンプレートが適用される可能性があるためです。 作業テンプレートを再作成しようとしているユーザーによって、既定のテンプレートで宣言されているイベント処理が誤って削除される可能性もあるためです。 そのような場合でも、クラス定義内でオーバーライドした [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) の一部としてハンドラーをアタッチすることで、コントロール レベルのイベント処理を提供できます。 これにより、インスタンス化時にコントロールのルートまでバブル ルーティングされる入力イベントをキャッチできます。

### <a name="the-handled-property"></a>**Handled** プロパティ

特定のルーティング イベントのイベント データ クラスには、**Handled** というプロパティが含まれているものがあります。 その例として、[**PointerRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled)、[**KeyRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)、[**DragEventArgs.Handled**](/uwp/api/windows.ui.xaml.drageventargs.handled) があります。 これらのどのクラスでも、**Handled** は設定可能なブール型プロパティとして使用されます。

**Handled** プロパティを **true** に設定すると、イベント システムの動作に影響します。 **Handled** が **true** に設定されると、その時点でほとんどのイベント ハンドラーへのイベントのルーティングは停止し、それ以降イベントはルート上にあるアタッチされている他のハンドラーによってキャッチされなくなります。 "Handled" になったときイベントのコンテキストでどのような結果になるか、またアプリがどのように応答するかは、アプリの設計しだいです。 基本的に **Handled** は、イベントの発生時にそのイベントをどのコンテナーにもバブル ルーティングする必要がないことをアプリのコードで指定できるシンプルなプロトコルです。そのときに何を実行する必要があるかはアプリのロジックで扱います。 ただし逆に注意すべきなのは、組み込みシステムやコントロールの動作用にバブル ルーティングが必要になることが多いイベントは処理しないということです。たとえば、選択コントロールの一部または項目内での低レベルのイベントを処理することで悪影響が出る場合があります。 選択コントロールは入力イベントを検索して選択の変更を調べる場合があるためです。

一部のルーティング イベントではこの方法でルーティングを停止できません。それらのイベントに **Handled** プロパティがないためです。 たとえば、[**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) と [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) はバブル ルーティングされますが、対応するイベント データ クラスにルーティングを停止する **Handled** プロパティがないため、常にルートまでバブル ルーティングされます。

##  <a name="input-event-handlers-in-controls"></a>コントロールの入力イベント ハンドラー

特定の Windows ランタイム コントロールでは、入力イベントに **Handled** の概念が内部的に使用されていることがあります。 このような場合、ユーザー コードからはそれを操作できないので、入力イベントが発生することはないようにも見えます。 たとえば、[**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) クラスには、一般的な入力イベント [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) を意図的に処理するロジックが含まれています。 これは、PointerPressed 入力やその他の入力モード (たとえば、フォーカスがあるときにボタンを起動できる、Enter キーなどのキーの処理) によって開始される [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントを、ボタンが発生させるためです。 **Button** クラスの設計上、未加工入力イベントは概念的に処理され、ユーザー コードなどのクラス コンシューマーは、コントロール関連の **Click** イベントとやり取りできます。 Windows ランタイム API リファレンスでは、特定のコントロール クラスのリファレンス トピックに、そのクラスが実装しているイベント処理動作の説明が含まれています。 場合によっては、**On**_Event_ メソッドをオーバーライドすることで、動作を変更できます。 たとえば、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 派生クラスがキー入力に対してどう反応するかは、[**Control.OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) をオーバーライドすることで変更できます。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>処理済みのルーティング イベントに対するハンドラー登録

既に説明したように、**Handled** を **true** に設定すると、ほとんどのハンドラーは呼び出されなくなります。 ただし、[**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) メソッドを活用すると、ルート内の他のハンドラーが共有イベント データで **Handled** を **true** に設定していても、ルートに対して必ず呼び出されるハンドラーをアタッチできます。 この手法が役立つのは、使っているコントロールが内部の合成ロジックまたはコントロール固有のロジックでイベントを処理済みにするが、 コントロールのインスタンスまたはアプリの UI からそのイベントに応答する必要がある場合です。 ただし、この手法は **Handled** の用途と矛盾し、コントロールの本来の対話操作を中断させる可能性があるため、使う際は注意が必要です。

[**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) によるイベント処理を使えるのは、対応するルーティング イベント識別子を持つルーティング イベントだけです。これは、**AddHandler** メソッドの入力として、その識別子が必要なためです。 ルーティング イベント識別子を使用可能にするイベントの一覧については、[**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) のリファレンス ドキュメントをご覧ください。 ほとんどの部分は、ここまでに示したルーティング イベントの一覧と同じです。 例外は、一覧の最後の 2 つである [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) と [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) です。この 2 つにはルーティング イベント識別子がないため、**AddHandler** を使えません。

## <a name="routed-events-outside-the-visual-tree"></a>ビジュアル ツリー外のルーティング イベント

一部のオブジェクトは、プライマリ ビジュアル ツリー (概念的にはメイン ビジュアル上のオーバーレイのようなもの) と関係しています。 このようなオブジェクトは、すべてのツリー要素と表示ルートを関連付ける通常の親子関係には含まれません。 たとえば、表示される [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) や [**ToolTip**](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) などがこれに該当します。 **Popup** または **ToolTip** からのルーティング イベントを処理する場合は、**Popup** 要素または **ToolTip** 要素そのものではなく、**Popup** または **ToolTip** 内の特定の UI 要素にハンドラーを配置してください。 **Popup** または **ToolTip** のコンテンツに対して実行される合成の内部のルーティングには依存しないようにする必要があります。 ルーティング イベントのイベント ルーティングは、メイン ビジュアル ツリーに沿った形でしか機能しないためです。 **Popup** も **ToolTip** も、従属する UI 要素の親とは見なされず、(たとえば、**Popup** の既定の背景を入力イベントのキャプチャ領域として使用しようとしても) ルーティング イベントを受け取ることはできません。

## <a name="hit-testing-and-input-events"></a>ヒット テストと入力イベント

ある要素が、UI でマウス入力、タッチ入力、スタイラス入力の対象として表示されるかどうかと場所を確認することを、*ヒット テスト*と呼びます。 タッチ操作や、タッチ操作の結果に発生する対話/操作イベントについては、ヒット テストで要素が表示されない場合、イベント ソースとして使用したり、操作に関連付けられたイベントを起動することはできません。 それ以外の場合、操作はその要素を通過し、その入力を操作する基になる要素またはビジュアル ツリー内の親要素へと渡されます。 ヒット テストに影響を与える要因はいくつかありますが、指定された要素の [**IsHitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) プロパティを確認すると、その要素が入力イベントを発生できるかどうかを判別できます。 このプロパティは、要素が次の条件を満たす場合にのみ、**true** を返します。

- 要素の [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) プロパティの値が [**Visible**](/uwp/api/Windows.UI.Xaml.Visibility) である。
- 要素の **Background** または **Fill** プロパティの値が **null** ではない。 **null** [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 値は、要素は透明で、ヒット テストで不可視になります  (要素を透明にしつつ、ヒット テストも可能にするには、**null** ではなく [**Transparent**](/uwp/api/windows.ui.colors.transparent) を使います)。

**注**  **Background** と **Fill** は [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) では定義されません。[**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) や [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) などの別の派生クラスによって定義されます。 ただし、フォアグラウンドやバックグラウンド プロパティに使用するブラシの影響は、それらのプロパティをどのサブクラスが実装するかに関係なく、ヒット テストや入力イベントに対して同様です。

- 要素がコントロールの場合、[**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) プロパティの値は **true** である必要がある。
- 要素はレイアウトで実際のサイズを持ったものである必要がある。 [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) と [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) のいずれかが 0 である要素は、入力イベントを発生させません。

一部のコントロールでは、ヒット テストに特別な規則があります。 たとえば、[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) には **Background** プロパティがありませんが、そのサイズの領域全体の中ではヒット テストできます。 [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) コントロールと [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) コントロールは、透明なコンテンツ (表示されているメディア ソース ファイル内のアルファ チャネルなど) の存在に関係なく、定義された四角形の上でヒット テストできます。 [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) コントロールには特別なヒット テストの動作があります。ホストされる HTML で入力が処理されて、スクリプト イベントが発生する場合があるためです。

ほとんどの [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) クラスと [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) は、自身のバックグラウンド内ではヒット テストできませんが、含んでいる要素からルーティングされたユーザー入力イベントを処理することはできます。

要素がヒット テストできるかどうかにかかわらず、どの要素がユーザー入力イベントと同じ位置にあるかどうかを判別することができます。 そのためには、[**FindElementsInHostCoordinates**](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) メソッドを呼び出します。 このメソッドは名前のとおり、指定されたホスト要素からの相対的な位置にある要素を見つけます。 ただし、適用された変換やレイアウト変更により要素の相対座標系が調整されて、指定された場所でどの要素が見つかるかに影響する場合があります。

## <a name="commanding"></a>コマンド実行

*コマンド実行*がサポートされる UI 要素は少数です。 コマンド実行では、基になる実装で入力関連のルーティング イベントを使用し、1 つのコマンド ハンドラーを呼び出して関連する UI 入力 (特定のポインター操作、特定のショートカット キー) の処理を有効にします。 UI 要素でコマンド実行を使うことができる場合は、個々の入力イベントではなく、コマンド実行 API を使うことを検討してください。 通常は **Binding** を使って、データのビュー モデルを定義するクラスのプロパティを参照します。 それらのプロパティに対応する名前付きコマンドで、言語固有の **ICommand** コマンド実行パターンを実装します。 詳細については、「 [**Buttonbase. コマンド**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)」を参照してください。

## <a name="custom-events-in-the-windows-runtime"></a>Windows ランタイムでのカスタム イベント

カスタム イベントを定義するにあたっては、使われるプログラミング言語に応じて、イベントの追加方法や、それがクラスの設計でどのような意味を帯びるのかが大きく異なります。

- C# と Visual Basic では、CLR のイベントを定義します。 カスタムアクセサー (remove**add**) を使用していない限り、標準の .net イベントパターンを使用でき / **remove**ます。 次の点にも注意してください。
    - イベント ハンドラーには、Windows ランタイムの汎用イベント デリゲート [**EventHandler<T>**](/uwp/api/windows.foundation.eventhandler) への組み込みの変換がある [**System.EventHandler<TEventArgs>**](/dotnet/api/system.eventhandler-1) を使うことをお勧めします。
    - Windows ランタイムに変換されないため、イベント データ クラスが [**System.EventArgs**](/dotnet/api/system.eventargs) に基づくことのないようにしてください。 既にあるイベント データ クラスを使うか、基底クラスをまったく使わないかのいずれかにします。
    - カスタムアクセサーを使用している場合は、「 [Windows ランタイムコンポーネント」の「カスタムイベントとイベントアクセサー](/previous-versions/windows/apps/hh972883(v=vs.140))」を参照してください。
    - 標準の .NET イベントのパターンがよくわからない場合には、「[Silverlight のカスタム クラスのイベントの定義](/previous-versions/windows/)」をご覧ください。 これは、Microsoft Silverlight 向けに書かれたものですが、.NET イベントのパターンのコードと概念がよくまとまっています。
- C++/CX については、「[イベント (C++/CX)](/cpp/cppcx/events-c-cx)」をご覧ください。
    - カスタム イベントを自ら使う場合であっても、名前付き参照を使ってください。 カスタム イベントにラムダは使えません。ラムダを使うと、循環参照が作られることになります。

Windows ランタイムでカスタム ルーティング イベントは宣言できません。ルーティング イベントは、Windows ランタイムのセットに限定されます。

カスタム イベントの定義は通常、カスタム コントロールを定義する一環として実行されます。 プロパティ変更コールバックのある依存関係プロパティだけでなく、その依存関係プロパティのコールバックで (必ずまたはときどき) 発生するカスタム イベントを定義するのがよくあるパターンです。 コントロールのユーザーには定義したプロパティ変更コールバックに対するアクセス権がないものの、通知イベントは利用可能にするというのが次善策です。 詳しくは、「[カスタム依存関係プロパティ](custom-dependency-properties.md)」をご覧ください。

## <a name="related-topics"></a>関連トピック

* [XAML の概要](xaml-overview.md)
* [クイック スタート: タッチ入力](/previous-versions/windows/apps/hh465387(v=win.10))
* [キーボード操作](../design/input/keyboard-interactions.md)
* [.NET イベントとデリゲート](/previous-versions/17sde2xt(v=vs.110))
* [Windows ランタイムコンポーネントの作成](/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)