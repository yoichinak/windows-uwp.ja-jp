---
Description: TextBox、RichEditBox などの UWP テキスト コントロール (および、同様のテキスト入力エクスペリエンスを提供する AutoSuggestBox のようなコントロール) によってサポートされる、テキスト入力に対してインク用に組み込まれた手書きビューをカスタマイズします。
title: 手書きビューでのテキスト入力
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "63774765"
---
# <a name="text-input-with-the-handwriting-view"></a>手書きビューでのテキスト入力

![ペンでタップするとテキスト ボックスが展開する](images/handwritingview/handwritingview2.gif)

[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) などの UWP テキスト コントロール、および [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) などから派生したコントロールによってサポートされる、テキスト入力に対してインク用に組み込まれた手書きビューをカスタマイズします。

## <a name="overview"></a>概要

XAML テキスト入力ボックスでは、[Windows Ink](../input/pen-and-stylus-interactions.md) を使用したペン入力の埋め込みがサポートされています。 ユーザーが Windows ペンを使用してテキスト入力ボックスでタップすると、別の入力パネルを開くことなく、テキスト ボックスが手書きサーフェスに変換されます。

ユーザーがテキスト ボックスの任意の場所に書き込むと、テキストが認識され、候補ウィンドウには認識結果が表示されます。 ユーザーは結果をタップしてそれを選択したり、さらに書き込んで提案された候補を受け入れたりすることができます。 リテラル (1 文字ずつ) による認識結果は候補ウィンドウに含まれているため、認識はディクショナリ内の単語に制限されません。 ユーザーが手書きで入力すると、受け入れられたテキスト入力は自然な手書き感を維持して Script フォントに変換されます。

> [!NOTE]
> 既定で手書きビューは有効にされていますが、これはコントロールごとに無効にすることができ、代わりに元のテキスト入力パネルに戻すことができます。

![インクと修正候補を含むテキスト ボックス](images/handwritingview/handwritingview-inksuggestion1.gif)

ユーザーは、次のような標準的なジェスチャや操作を使用してテキストを編集できます。

- _取り消し線_または_インクを消す_ - 単語や単語の一部を削除します
- _結合_ - 単語間に円弧を描き、単語間のスペースを削除します
- _挿入_- スペースを挿入するキャレット記号を描画します
- _上書き_- 既存のテキストの上に書き込み、それを置き換えます

![インクの修正を含むテキスト ボックス](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>手書きビューを無効にする

既定では、組み込みの手書きビューは有効になっています。

ご利用のアプリケーションで同等のインクをテキストに変換する機能を既に提供している場合、または、テキスト入力エクスペリエンスが手書きによって使用できない何らかの書式または特殊文字 (タブなど) に依存する場合、手書きビューを無効にする必要がある可能性があります。

この例では、[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) コントロールの [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) プロパティを false に設定することで、手書きビューを無効にします。 手書きビューをサポートするすべてのテキスト コントロールで、同様のプロパティがサポートされます。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>手書きビューの配置を指定する

手書きビューは、ユーザーの手書きの基本設定を反映するために、基になるコントロールの上に配置されサイズが変更されます ( **[設定] -> [デバイス] -> [ペンと Windows Ink] -> [手書き] -> [Size of font when writing directly into text field]\(テキスト フィールドに直接入力するときのフォトのサイズ\)** を参照してください)。 また、ビューの配置も、アプリ内のテキスト コントロールとその場所に合わせて自動的に行われます。

アプリケーション UI はより大きなコントロールに対応するためにリフローされないので、システムによってビューで重要な UI が見えなくなる可能性があります。

ここで、[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) の [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) プロパティを使用する方法を示し、手書きビューを整列するために使用される基になるテキスト コントロール上のアンカーを指定します。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>オートコンプリートの候補を無効にする

上位のインク認識の一覧を提供するために、既定で入力ヒントのポップアップは有効にされています。上位の候補が正しくない場合、ユーザーはここから選択することができます。

ご利用のアプリケーションで既に堅牢なカスタムの認識機能が提供されている場合、次の例に示すように、[AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) プロパティを使用して組み込みの修正候補を無効にすることができます。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>手書きフォントの基本設定を使用する

インク認識に基づいてテキストをレンダリングするときに、ユーザーは手書きベースのフォントの事前に定義されたコレクションから使用するものを選択することができます ( **[設定] -> [デバイス] -> [ペンと Windows Ink] -> [手書き] -> [Font when using handwriting]\(手書きを使用するときのフォント\) を参照してください**)。

> [!NOTE]
> ユーザーは独自の手書き入力に基づいてフォントを作成することもできます。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

ご利用のアプリでは、この設定にアクセスし、テキスト コントロールで認識されたテキスト用に選択されたフォントを使用することができます。

この例では、[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) の [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) イベントをリッスンし、テキストの変更が HandwritingView から発生した場合、ユーザーが選択したフォントが適用されます (または、選択していない場合は、既定のフォントが適用されます)。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>複合コントロールで HandwritingView にアクセスする

[TextBox ](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) コントロールまたは [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) コントロールを使用する複合コントロール ([AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) など) では、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) もサポートされます。

複合コントロールで [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) にアクセスするには、[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API を使用します。

次の XAML スニペットでは、[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) コントロールが表示されます。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

対応するコードビハインドでは、[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) で [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) を無効にする方法を示しています。

1. まず、FindInnerTextBox 関数を呼び出すアプリケーションで読み込まれたイベントを処理して、視覚的なツリー走査を開始します。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 次に、FindVisualChildByName への呼び出しで、FindInnerTextBox 関数の (AutoSuggestBox から開始される) ビジュアル ツリーを通して反復を開始します。

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. 最後に、この関数は、TextBox が取得されるまで、ビジュアル ツリーを通して反復されます。

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>HandwritingView の位置を変更する

場合によっては、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 以外ではカバーできない可能性のある UI 要素が確実にカバーされるようにする必要があります。

ここで、ディクテーションをサポートする TextBox を作成します (TextBox とディクテーション ボタンを StackPanel に配置することで実装されます)。

![ディクテーションでの TextBox](images/handwritingview/textbox-with-dictation.png)

StackPanel が TextBox より大きくなったため、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) によってすべての複合コントロールが遮られるとは限りません。

![ディクテーションでの TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

これに対処するには、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) の PlacementTarget プロパティに整列される必要がある UI 要素を設定します。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>HandwritingView のサイズを変更する

ビューにより重要な UI が遮られないことを確実にする必要があるときに便利な場合がある、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) のサイズを設定することもできます。

前の例のように、ディクテーションをサポートする TextBox を作成します (TextBox とディクテーション ボタンを StackPanel に配置することで実装されます)。

![ディクテーションでの TextBox](images/handwritingview/textbox-with-dictation.png)

この場合、確実にディクテーション ボタンが常に表示されているようにします。

![ディクテーションでの TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

これを行うには、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) の MaxWidth プロパティを、見えなくなる UI 要素の幅にバインドします。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>カスタム UI の位置を変更する

情報のポップアップなど、テキスト入力に応答して表示されるカスタムの UI がある場合、手書きビューが見えなくならないように、その UI の配置を変更する必要があります。

![カスタム UI を含む TextBox](images/handwritingview/textbox-with-customui.png)

次の例では、[ポップアップ](https://docs.microsoft.com/uwp/api/windows.ui.popups)の位置を設定するために、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) の [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)、[Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)、[SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) イベントをリッスンする方法を示します。

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>HandwritingView コントロールの再テンプレート化

すべての XAML フレームワーク コントロールで、特定の要件のために、[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) の視覚的な構造と視覚的な動作の両方をカスタマイズできます。

カスタム テンプレートを作成する完全な例を表示するには、「[カスタム トランスポート コントロールを作成する](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)」の方法または「[Custom Edit Control sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)」 (カスタムの編集コントロールのサンプル) を確認してください。








