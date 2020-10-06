---
title: Xbox での音声対応シェル (VES)
description: 音声認識シェル (VES) を使用して、Xbox 上のユニバーサル Windows プラットフォーム (UWP) アプリに音声コントロールサポートを追加する方法について説明します。
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10、uwp、xbox、音声、音声対応シェル
ms.openlocfilehash: fa0f56a6821fd8858cab317654cd0ead5d731693
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750268"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Speech を使用した UI 要素の呼び出し

音声認識シェル (VES) は、Windows Speech プラットフォームの拡張機能であり、アプリ内でファーストクラスの音声エクスペリエンスを可能にします。これにより、ユーザーは、画面上のコントロールを呼び出したり、ディクテーションを使用してテキストを挿入したりできます。 VES は、すべての Windows シェルおよびデバイスで、エンドツーエンドの一般情報を提供します。アプリ開発者にとって最小限の労力が必要になります。  これを実現するために、Microsoft Speech Platform と UI Automation (UIA) フレームワークを活用しています。

## <a name="user-experience-walkthrough"></a>ユーザーエクスペリエンスのチュートリアル ##
次に示すのは、Xbox で VES を使用する場合のユーザーエクスペリエンスの概要であり、VES の動作の詳細について説明する前に、コンテキストを設定するのに役立ちます。

- ユーザーは Xbox コンソールをオンにして、アプリを参照して目的のものを見つける必要があります。

    > ユーザー: "Cortana、ゲームとアプリを開く"

- ユーザーは、アクティブリスニングモード (ALM) のままになっています。これは、コンソールが、毎回 "Cortana" を知らなくても、画面上に表示されるコントロールを呼び出すことを待機していることを意味します。  ユーザーは、アプリの表示に切り替えて、アプリの一覧をスクロールできるようになりました。

    > ユーザー: "アプリケーション"

- ビューをスクロールするために、ユーザーは次のように簡単に表示できます。

    > ユーザー: "下へスクロール"

- ユーザーには、関心のあるアプリケーションのボックスアートが表示されますが、名前は忘れられています。  ユーザーが音声ヒントラベルを表示するよう求められます。

    > ユーザー: "ラベルの表示"

- 言うまでもありませんが、アプリを起動できます。

    > ユーザー: "映画と TV"

- アクティブなリスニングモードを終了するために、ユーザーは Xbox にリッスンを停止するように指示します。

    > ユーザー: "リッスンの停止"

- 後で、次のようにして、新しいアクティブなリッスンセッションを開始できます。

    > ユーザー: "Cortana について、選択する" または "Cortana を選択してください"

## <a name="ui-automation-dependency"></a>UI オートメーションの依存関係 ##
VES は UI オートメーションクライアントであり、UI オートメーションプロバイダーを通じてアプリによって公開される情報に依存します。 これは、Windows プラットフォームのナレーター機能によって既に使用されているインフラストラクチャと同じです。  UI オートメーションを使用すると、コントロールの名前、その型、実装するコントロールパターンなど、ユーザーインターフェイス要素へのプログラムによるアクセスが可能になります。  アプリで UI が変更されると、VES は UIA update イベントに応答し、更新された UI オートメーションツリーを再解析して、この情報を使用して音声認識文法を構築し、アクション可能なすべての項目を検索します。 

すべての UWP アプリは UI オートメーションフレームワークにアクセスでき、UI に関する情報を、構築されたグラフィックスフレームワーク (XAML、DirectX、Direct3D、Xamarin など) に依存せずに公開できます。  場合によっては、XAML と同様に、大量の処理の大部分がフレームワークによって実行されるため、ナレーターと VES をサポートするために必要な作業が大幅に減少します。

UI オートメーションの詳細については、「 [Ui オートメーションの基礎](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI オートメーションの基礎")」を参照してください。

## <a name="control-invocation-name"></a>コントロールの呼び出し名 ##
VES では、コントロールの名前として音声認識エンジンに登録する語句を決定するために、次のヒューリスティックを使用します (つまり、ユーザーがコントロールを呼び出すために読み上げる必要があること)。  これは、音声ヒントラベルに表示される語句でもあります。

優先順位に従った名前のソース:

1. 要素に添付プロパティがある場合 `LabeledBy` 、VES は `AutomationProperties.Name` このテキストラベルのを使用します。
2. `AutomationProperties.Name` 要素の。  XAML では、コントロールのテキストコンテンツがの既定値として使用され `AutomationProperties.Name` ます。
3. コントロールが ListItem または Button の場合、VES は、有効なを持つ最初の子要素を検索し `AutomationProperties.Name` ます。

## <a name="actionable-controls"></a>アクション可能なコントロール ##
VES は、次のオートメーションコントロールパターンのいずれかを実装すると、コントロールが実行可能であると見なされます。

- **Invokepattern** (例 ボタン)-1 つの明確なアクションを開始または実行し、アクティブになったときに状態を保持しないコントロールを表します。

- **TogglePattern** (例 チェックボックス)-一連の状態を順番に切り替え、状態を一度設定したコントロールを表します。

- **SelectionItemPattern** (例 コンボボックス)-選択可能な子項目のコレクションのコンテナーとして機能するコントロールを表します。

- **ExpandCollapsePattern** (例 コンボボックス)-コンテンツを表示するために視覚的に展開し、コンテンツを非表示にするために折りたたむコントロールを表します。

- **Scrollpattern** (例 List)-子要素のコレクションのスクロール可能なコンテナーとして機能するコントロールを表します。

## <a name="scrollable-containers"></a>スクロール可能なコンテナー ##
ScrollPattern をサポートするスクロール可能なコンテナーの場合、VES は、"左へスクロール"、"右にスクロール" などの音声コマンドをリッスンし、ユーザーがこれらのコマンドのいずれかをトリガーするときに、適切なパラメーターを使用してスクロールを呼び出します。  Scroll コマンドは、プロパティとプロパティの値に基づいて挿入され `HorizontalScrollPercent` `VerticalScrollPercent` ます。  たとえば、 `HorizontalScrollPercent` が0より大きい場合は、[左にスクロール] が追加され、100未満の場合は [右にスクロール] が追加されます。

## <a name="narrator-overlap"></a>ナレーターの重なり ##
ナレーターアプリケーションは、UI オートメーションクライアントでもあり、プロパティを、 `AutomationProperties.Name` 現在選択されている ui 要素に対して読み取るテキストのソースの1つとして使用します。  ユーザー補助機能を向上させるために、多くのアプリ開発者は、 `Name` ナレーターで読み取られるときにより多くの情報とコンテキストを提供することを目的として、長い説明的なテキストを使用してプロパティをオーバーロードするようにしました。  ただし、これにより、2つの機能の間で競合が発生します。 VES では、コントロールの表示テキストに一致するか、またはそれに一致する短い語句が必要です。また、ナレーターは長い時間がかかり、より適切なコンテキストを提供するためのわかりやすい語句が得られます。

これを解決するには、Windows 10 の作成者の更新プログラムから、ナレーターが更新され、プロパティも表示されるようになりました `AutomationProperties.HelpText` 。  このプロパティが空でない場合は、に加えて、ナレーターによって内容が読み上げられ `AutomationProperties.Name` ます。  `HelpText`が空の場合、ナレーターは名前の内容のみを読み取ります。  これにより、必要に応じてより長い説明的な文字列を使用できるようになりますが、プロパティにはより短い音声認識のわかりやすい語句が保持され `Name` ます。

![AutomationProperties.Name と Automationproperties.automationid が含まれているボタンの背後にあるコードを示す図。これは、音声対応シェルが構成名をリッスンすることを示しています。](images/ves_narrator.jpg)

詳細については、「 [UI でユーザー補助機能をサポートするためのオートメーションプロパティ](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI でのアクセシビリティサポートのオートメーションプロパティ")」を参照してください。

## <a name="active-listening-mode-alm"></a>アクティブリスニングモード (ALM) ##
### <a name="entering-alm"></a>ALM の入力 ###
Xbox では、VES が音声入力を常にリッスンしているわけではありません。  ユーザーは次のようにして、アクティブなリッスンモードに明示的に入力する必要があります。

- 「Cortana、select」、または
- "Cortana さん、選択してください"

他にもいくつかの Cortana コマンドを使用して、ユーザーが完了時にアクティブになるのを待っています。たとえば、"コルタナさん、サインイン"、"Cortana さん、帰宅しています" などです。 

ALM を入力すると、次のようになります。

- Cortana オーバーレイが右上隅に表示され、ユーザーに表示される内容を伝えることができます。  ユーザーが話している間、音声認識エンジンによって認識される語句フラグメントもこの場所に表示されます。
- VES は、UIA ツリーを解析し、実行可能なすべてのコントロールを検索し、音声認識文法にそのテキストを登録して、継続的なリスニングセッションを開始します。

    ![[ラベルの表示] オプションが強調表示されているというラベルを表示するスクリーンショット。](images/ves_overlay.png)

### <a name="exiting-alm"></a>ALM の終了 ###
ユーザーが音声を使用して UI を操作している間、システムは ALM のままになります。  ALM を終了するには、次の2つの方法があります。

- ユーザーが明示的に "リッスンを停止する"、または
- ALM を入力してから17秒以内に肯定認識がない場合、または最後の肯定認識後に、タイムアウトが発生します。

## <a name="invoking-controls"></a>呼び出し (コントロールを) ##
ALM では、ユーザーは音声を使用して UI を操作できます。  UI が正しく構成されている (表示されているテキストと一致する名前プロパティがある) 場合、音声を使用してアクションを実行するのはシームレスで、自然なエクスペリエンスです。  ユーザーは、画面に表示されている内容を簡単に読み上げることができます。

## <a name="overlay-ui-on-xbox"></a>Xbox でのオーバーレイ UI ##
VES がコントロールに対して派生する名前は、UI で実際に表示されるテキストとは異なる場合があります。  これは、 `Name` コントロールのプロパティ、またはアタッチされた `LabeledBy` 要素が別の文字列に明示的に設定されていることが原因である可能性があります。  または、コントロールに GUI テキストはありませんが、icon 要素または image 要素のみが含まれています。

このような場合、ユーザーは、このようなコントロールを呼び出すために、どのようなことが必要かを確認する方法を必要とします。  したがって、アクティブなリスニングでは、"ラベルを表示する" という音声ヒントを表示できます。  これにより、すべてのアクション可能なコントロールの上に音声ヒントラベルが表示されます。

1つのラベルには100の制限があります。そのため、アプリの UI に100よりも多くのアクション可能なコントロールがある場合は、音声ヒントのラベルが表示されないものがあります。  この場合、どのラベルが選択されるかは決定的ではありません。これは、UIA ツリーで最初に列挙される現在の UI の構造と構成に依存しているためです。

音声ヒントのラベルを表示した後は、非表示にするコマンドがないため、次のいずれかのイベントが発生するまで表示されたままになります。

- ユーザーがコントロールを呼び出す
- ユーザーが現在のシーンから移動する
- ユーザーは "リッスンを停止する"
- アクティブなリッスンモードのタイムアウト

## <a name="location-of-voice-tip-labels"></a>音声ヒントラベルの場所 ##
音声ヒントラベルは、コントロールの BoundingRectangle 内で水平方向および垂直方向に中央揃えで配置されます。  コントロールが小さく、緊密にグループ化されていると、ラベルが重なったり、他のユーザーによって隠されたりする可能性があります。 VES は、これらのラベルを分割し、それらが表示されるようにします。  ただし、これは100% の時間にわたって動作することは保証されていません。  非常に混雑している UI がある場合は、一部のラベルが他のユーザーによって隠されている可能性があります。 "ラベルの表示" を使用して UI を確認し、音声ヒントを表示するための十分な領域があることを確認してください。

![コントロールの外接する四角形内で水平方向および垂直方向に配置される、音声ヒントラベルのスクリーンショット。](images/ves_labels.png)

## <a name="combo-boxes"></a>コンボボックス ##
コンボボックスが展開されると、コンボボックス内の個々の項目が独自の音声ヒントラベルを取得し、多くの場合、ドロップダウンリストの背後にある既存のコントロールの上に表示されます。  コンボボックスが展開されているときに、その子項目のラベルだけが表示されるようにするには、(コンボボックスの項目ラベルがコンボボックスの背後にあるコントロールのラベルと混在している) muddle のラベルを表示しないようにします。 その他のすべての音声ヒントラベルは非表示になります。  ユーザーは、ドロップダウン項目のいずれかを選択するか、コンボボックスを閉じることができます。

- 折りたたまれたコンボボックスのラベル:

    ![折りたたまれたコンボボックスにラベルが表示された [ディスプレイとサウンド] ビデオ出力ウィンドウのスクリーンショット。](images/ves_combo_closed.png)

- 展開されたコンボボックスのラベル:

    ![展開されたコンボボックスにラベルが表示された [ディスプレイとサウンド] ビデオ出力ウィンドウのスクリーンショット。](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>スクロール可能なコントロール ##
スクロール可能なコントロールの場合、スクロールコマンドの音声ヒントは、コントロールの各端の中央に配置されます。  音声ヒントは、実行可能なスクロール方向に対してのみ表示されます。たとえば、垂直スクロールが使用できない場合、"上へスクロール" と "下へスクロール" が表示されません。  複数のスクロール可能な領域が存在する場合、VES は序数を使用してそれらを区別します (例として、 "Right 1"、"Scroll right 2" など)。

![水平スクロール U I に対して、左にスクロールして、右にスクロールするためのスクリーンショット。](images/ves_scroll.png) 

## <a name="disambiguation"></a>曖昧性の除去 ##
複数の UI 要素に同じ名前が付けられている場合、または音声認識エンジンが複数の候補に一致した場合、VES は非不明瞭モードになります。  このモードでは、ユーザーが適切な要素を選択できるように、関連する要素に対して音声ヒントラベルが表示されます。 ユーザーは、"キャンセル" という指示によって非不明瞭モードを取り消すことができます。

次に例を示します。

- アクティブなリスニングモードで、あいまいさを解消する前。ユーザーは、「あいまいな」と言っています。

    ![アクティブなリスニングモードのスクリーンショットは、表示されているオプションと、ボタンにラベルが表示されていないことを示すことができるようになりました。](images/ves_disambig1.png) 

- 両方のボタンが一致しました。非不明瞭の開始:

    ![オプションが表示されているアクティブなリスニングモードのスクリーンショット、および項目1と項目2のラベルがボタンに表示されます。](images/ves_disambig2.png) 

- "選択 2" が選択されたときのクリックアクションの表示:

    ![アクティブなリスニングモードのスクリーンショットは、表示されているオプションと、最初のボタンのラベルがあいまいであることを言うことができます。](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>サンプル UI ##
XAML ベースの UI の例を次に示します。さまざまな方法で AutomationProperties.Name を設定します。

```xaml
<Page
    x:Class="VESSampleCSharp.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:VESSampleCSharp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
        <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
        <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
        <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
            <ComboBoxItem Content="Monday" IsSelected="True"/>
            <ComboBoxItem Content="Tuesday"/>
            <ComboBoxItem Content="Wednesday"/>
            <ComboBoxItem Content="Thursday"/>
            <ComboBoxItem Content="Friday"/>
            <ComboBoxItem Content="Saturday"/>
            <ComboBoxItem Content="Sunday"/>
        </ComboBox>
        <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
            <Grid>
                <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
            </Grid>
        </Button>
    </Grid>
</Page>
```

上記のサンプルを使用すると、UI は、音声ヒントラベルの有無にかかわらず、次のようになります。
 
- アクティブなリスニングモードでは、ラベルは表示されません。

    ![[ラベルの表示] オプションが表示されていて、ラベルが表示されていない、アクティブなリスニングモードのスクリーンショット。ラベルを表示します。](images/ves_alm_nolabels.png) 

- アクティブなリスニングモードでは、ユーザーは "ラベルの表示" を実行します。

    ![を使用したアクティブなリスニングモードのスクリーンショット。完了したら、[リッスンを停止する] オプションが表示され、ラベルが U I コントロールに表示されます。](images/ves_alm_labels.png) 

の場合 `button1` 、XAML は、 `AutomationProperties.Name` コントロールの表示テキストコンテンツのテキストを使用してプロパティを自動設定します。  このため、明示的なセットがない場合でも、音声ヒントラベルがあり `AutomationProperties.Name` ます。

で `button2` は、を `AutomationProperties.Name` コントロールのテキスト以外のものに明示的に設定します。

では `comboBox` 、プロパティを使用し `LabeledBy` `label1` てオートメーションのソースとしてを参照 `Name` して `label1` います。また、は、 `AutomationProperties.Name` 画面に表示されるものよりも自然な語句に設定されています ("曜日の選択" ではなく "曜日")。

最後に、では、が `button3` `Name` `button3` 設定されていないため、VES は最初の子要素からを取得し `AutomationProperties.Name` ます。

## <a name="see-also"></a>参照
- [UI オートメーションの基礎](/dotnet/framework/ui-automation/ui-automation-fundamentals "UI オートメーションの基礎")
- [UI でのアクセシビリティサポートのオートメーションプロパティ](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "UI でのアクセシビリティサポートのオートメーションプロパティ")
- [よく寄せられる質問](frequently-asked-questions.md)
- [Xbox One の UWP](index.md)