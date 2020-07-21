---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: このトピックでは、機能しているピアツーピアクイズゲームの WinRT 8.1 サンプルアプリを Windows 10 ユニバーサル Windows プラットフォーム (UWP) アプリに移植するケーススタディについて説明します。
title: Windows ランタイム 8.x から UWP へのケース スタディ - QuizGame ピア ツー ピアのサンプル アプリ
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b03e13352717c5e414dda60fe413b00edc9ba827
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260118"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Windows ランタイム 8.x から UWP へのケース スタディ - QuizGame サンプル アプリ




このトピックでは、機能しているピアツーピアクイズゲームの WinRT 8.1 サンプルアプリを Windows 10 ユニバーサル Windows プラットフォーム (UWP) アプリに移植するケーススタディについて説明します。

ユニバーサル8.1 アプリとは、同じアプリの2つのバージョン (Windows 8.1 用の1つのアプリパッケージと Windows Phone 8.1 用の別のアプリケーションパッケージをビルドするアプリのことです。 QuizGame の WinRT 8.1 バージョンでは、ユニバーサル Windows アプリ プロジェクトに用意されているデータを使いますが、独自の方法が採用されており、2 つのプラットフォームに対して機能的に異なるアプリがビルドされます。 Windows 8.1 アプリケーションパッケージはクイズゲームセッションのホストとして機能し、Windows Phone 8.1 アプリケーションパッケージはホストに対してクライアントの役割を果たします。 クイズ ゲーム セッションの 2 つの要素は、ピア ツー ピア ネットワークを経由して通信します。

これら 2 つの要素を PC や電話向けにそれぞれ調整することで、適切なアプリを作ることができます。 また、必要となるほとんどのデバイスでホストとクライアントの両方を実行できたら、より便利ではないでしょうか。 このケーススタディでは、両方のアプリを Windows 10 に移植します。各アプリは、ユーザーがさまざまなデバイスにインストールできる1つのアプリパッケージに組み込まれます。

アプリでは、ビューとビュー モデルを使うパターンを採用します。 このパターンではビューとビュー モデルが明確に分離されているため、以下の説明をご覧になるとわかりますが、このアプリの移植プロセスは非常に簡単です。

**注**  このサンプルでは、ネットワークがカスタム UDP グループマルチキャストパケットを送受信するように構成されていることを前提としています (ほとんどのホームネットワークは、会社のネットワークがではない場合もあります)。 また、このサンプルでは TCP パケットを送受信します。

 

Visual Studio で QuizGame10 を開いたときに、"Visual Studio 更新プログラムが必要です" というメッセージが表示された場合は、 [Targetplatformversion](w8x-to-uwp-troubleshooting.md)の手順に従っ**て   ます**。

 

## <a name="downloads"></a>ダウンロード

[QuizGame ユニバーサル 8.1 アプリをダウンロードします](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame)。 これは、移植前の初期状態のアプリです。 

[QuizGame10 Windows 10 アプリをダウンロード](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/QuizGame10)します。 これは、移植直後の状態のアプリです。 

[このサンプルの最新バージョンについては GitHub をご覧ください](https://github.com/microsoft/Windows-appsample-networkhelper)。

## <a name="the-winrt-81-solution"></a>WinRT 8.1 ソリューション


次に、ここで移植するアプリ QuizGame の外観を示します。

![Windows で実行されている QuizGame ホスト アプリ](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

Windows で実行されている QuizGame ホスト アプリ

 

![Windows Phone で実行されている QuizGame クライアント アプリ](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

Windows Phone で実行されている QuizGame クライアント アプリ

## <a name="a-walkthrough-of-quizgame-in-use"></a>このケース スタディで使う QuizGame のチュートリアル

ここで使うアプリの簡単な説明を示します。これは架空のアプリの説明ですが、ワイヤレス ネットワークでアプリを使う場合に役立つ情報も記載されています。

楽しいクイズ ゲームがバーで行われています。 バーには大型テレビがあり、全員がそのテレビを見ることができます。 クイズ番組の司会者は PC を使っており、その出力画面が TV に映し出されています。 その PC では "ホスト アプリ" が実行されています。 クイズへの参加を希望する視聴者は、"クライアント アプリ" を自分の携帯電話や Surface にインストールするだけです。

ホスト アプリはロビー モードになっており、大型テレビでは、クライアント アプリ接続の準備ができていることを通知しています。 Joan は、モバイル デバイスでクライアント アプリを起動します。 彼女は、 **[Player name] (プレーヤー名)** テキスト ボックスに自分の名前を入力してから **[Join game] (ゲームに参加)** をタップします。 ホスト アプリでは Joan の名前が表示され、彼女の参加が認識されました。また、Joan のクライアント アプリでは、ゲームの開始を待機していることが示されています。 次に、Maxwell が同じ手順を自分のモバイル デバイスで実行しました。

クイズ番組の司会者が **[Start game] (ゲームの開始)** をクリックすると、ホスト アプリには質問と正しいと思われる回答がいくつか表示されます (参加しているプレイヤーの一覧も表示されます。この一覧では、フォントの太さは通常、色は灰色で表示されます)。 同時に、参加しているクライアント デバイスでは、回答がボタンで表示されます。 Joan は "1975" と示されている回答のボタンをタップしました。するとすぐに、すべてのボタンが無効になりました。 ホスト アプリでは、Joan の名前が太字の緑色で表示され、彼女の回答が受信されたことが示されました。 Maxwell の回答しました。 クイズ番組の司会者はすべてプレーヤーの名前が緑色になったことを確認して、 **[Next question] (次の質問)** をクリックしました。

引き続き、同じ手順で質問の表示と回答が繰り返されました。 最後の質問がホスト アプリに表示されると、ボタンの内容は **[Next question] (次の質問)** ではなく、 **[Show results] (結果の表示)** となりました。 **[Show results] (結果の表示)** をクリックすると、結果が表示されました。 **[Return to lobby] (ロビーに戻る)** をクリックすると、ゲームのライフ サイクルの最初に戻りますが、プレイヤーは参加したままになります。 ただし、ロビーに戻ると、新しいプレイヤーが参加できるようになります。このとき、参加していたプレイヤーはゲームの参加をやめることができます ( **[Leave game] (ゲームの参加をやめる)** をタップすることで、参加しているプレイヤーはいつでもゲームをやめることができます)。

## <a name="local-test-mode"></a>ローカル テスト モード

アプリとその操作を、(複数のデバイスに分散された状態ではなく) 1 台の PC でテストする場合は、ローカル テスト モードでホスト アプリをビルドできます。 このモードでは、ネットワークはまったく使われません。 代わりに、ホスト アプリの UI では、ウィンドウの左側にはホスト部分の画面が表示され、右側にはクライアント アプリの UI をコピーした 2 つの画面が上下に並べて表示されます (このバージョンでは、ローカル テスト モードの UI は PC のディスプレイ用に固定されており、小型のデバイスには対応していません)。 UI のこれらのセグメント (すべて同じアプリに属しています) は、モック クライアント コミュニケーターを経由して相互に通信します。このコミュニケーターは、ネットワークを通じて実行される操作をシミュレートします。

ローカル テスト モードを有効にするには、**LOCALTESTMODEON** (プロジェクトのプロパティ) を条件付きコンパイル シンボルとして定義して、リビルドします。

## <a name="porting-to-a-windows10-project"></a>Windows 10 プロジェクトへの移植

QuizGame には、次の要素が含まれています。

-   P2PHelper。 これは、ピア ツー ピアのネットワーク ロジックを含むポータブル クラス ライブラリです。
-   QuizGame.Windows。 これは、Windows 8.1 を対象とするホストアプリのアプリパッケージをビルドするプロジェクトです。
-   QuizGame.WindowsPhone。 これは、Windows Phone 8.1 を対象とするクライアント アプリのアプリ パッケージをビルドするプロジェクトです。
-   QuizGame.Shared。 これは、他の 2 つのプロジェクトの両方で使われるソース コード、マークアップ ファイル、および他のアセットやリソースを含むプロジェクトです。

このケース スタディでは、サポートするデバイスに関して、「[ユニバーサル 8.1 アプリがある場合](w8x-to-uwp-root.md)」で説明した通常のオプションを使います。

これらのオプションに基づいて、QuizGame を QuizGameHost という新しい Windows 10 プロジェクトに移植します。 さらに、QuizGameClient という新しい Windows 10 プロジェクトに QuizGame を移植します。 これらのプロジェクトはユニバーサル デバイス ファミリを対象としているため、どのようなデバイスでも実行できます。 また、QuizGame.Shared ソース ファイルなどのファイルを独自のフォルダーに保存し、これらの共有ファイルを 2 つの新しいプロジェクトにリンクします。 これまでと同様に、すべてのデータを 1 つのソリューションに保存し、QuizGame10 という名前を付けます。

**QuizGame10 ソリューション**

-   新しいソリューション (**新しいプロジェクト**&gt; **Visual Studio ソリューション**&gt;**のその他のプロジェクトの種類**) を作成し、QuizGame10 という名前を指定します。

**P2PHelper**

-   ソリューションで、新しい Windows 10 クラスライブラリプロジェクトを作成し ([ **Windows ユニバーサル**&gt;**クラスライブラリ (windows Universal)** &gt;**新しいプロジェクト**])、P2PHelper という名前を指定します。
-   新しいプロジェクトから Class1.cs を削除します。
-   P2PSession.cs、P2PSessionClient.cs、P2PSessionHost.cs を新しいプロジェクトのフォルダーにコピーし、コピーしたファイルを新しいプロジェクトに追加します。
-   プロジェクトをビルドします。その他の変更は必要ありません。

**共有ファイル**

-   共通、モデル、ビュー、およびビュービューのフォルダーを \\QuizGame\\ から \\QuizGame10\\にコピーします。
-   Common、Model、View、ViewModel は、ディスク上の共有フォルダーを参照するときに意味のある名前です。

**QuizGameHost**

-   新しい Windows 10 アプリプロジェクトを作成し (&gt;**新しいプロジェクト**&gt; **Windows universal** &gt;**空のアプリケーション (Windows ユニバーサル)** ) を**追加**し、名前を QuizGameHost にします。
-   P2PHelper への参照を追加します (**参照の追加**&gt;**プロジェクト**&gt;**ソリューション**&gt; **P2PHelper**)。
-   **ソリューション エクスプ ローラー**で、ディスク上の各共有フォルダー用に新しいフォルダーを作成します。 さらに、作成した各フォルダーを右クリックし、[**既存の項目**の**追加**&gt;] をクリックして、フォルダーに移動します。 適切な共有フォルダーを開き、すべてのファイルを選んで、 **[リンクとして追加]** をクリックします。
-   Mainpage.xaml を \\\\ QuizGame から \\QuizGameHost\\ にコピーし、名前空間を QuizGameHost に変更します。
-   \\QuizGame の\\ から \\QuizGameHost\\ に app.xaml をコピーし、名前空間を QuizGameHost に変更します。
-   app.xaml.cs を上書きせずに、このファイルのバージョンを新しいプロジェクトに保存しておきます。ローカル テスト モードをサポートするように、対象となる変更を 1 つだけそのファイルに加えます。 app.xaml.cs で、次のコード行を置き換えます。

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

上のコード行を次のコード行と置き換えます。

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   [**プロパティ**&gt; &gt;**条件付きコンパイルシンボル**を**作成**する] で、localtestmodeon を追加します。
-   これで、app.xaml.cs に追加したコードに戻り、TestView 型を解決できます。
-   package.appxmanifest で、機能名を internetClient から internetClientServer に変更します。

**QuizGameClient**

-   新しい Windows 10 アプリプロジェクトを作成し (&gt;**新しいプロジェクト**&gt; **Windows universal** &gt;**空のアプリケーション (Windows ユニバーサル)** ) を**追加**し、名前を QuizGameClient にします。
-   P2PHelper への参照を追加します (**参照の追加**&gt;**プロジェクト**&gt;**ソリューション**&gt; **P2PHelper**)。
-   **ソリューション エクスプ ローラー**で、ディスク上の各共有フォルダー用に新しいフォルダーを作成します。 さらに、作成した各フォルダーを右クリックし、[**既存の項目**の**追加**&gt;] をクリックして、フォルダーに移動します。 適切な共有フォルダーを開き、すべてのファイルを選んで、 **[リンクとして追加]** をクリックします。
-   \\QuizGame から Mainpage.xaml\\ を \\QuizGameClient\\ にコピーし、名前空間を QuizGameClient に変更します。
-   \\QuizGame の\\ から \\QuizGameClient\\ に app.xaml をコピーし、名前空間を QuizGameClient に変更します。
-   package.appxmanifest で、機能名を internetClient から internetClientServer に変更します。

これで、ビルドして実行することができます。

## <a name="adaptive-ui"></a>アダプティブ UI

アプリがワイドウィンドウで実行されている場合、QuizGameHost Windows 10 アプリは正常に見えます (これは、画面が大きいデバイスでのみ可能です)。 ただし、アプリのウィンドウが狭い場合は (小型のデバイスが該当しますが、大型のデバイスの場合もあり得ます)、UI がかなり縮小され、認識するのが難しくなります。

この問題を解決するには、アダプティブな Visual State Manager 機能を使うことができます。これについては、「[ケース スタディ: Bookstore2](w8x-to-uwp-case-study-bookstore2.md)」で説明しています。 最初に、既定で UI が幅の狭い状態でレイアウトされるように、視覚要素のプロパティを設定します。 これらの変更はすべて \\ビュー\\HostView .xaml で行われます。

-   メインの **Grid** で、最初の **RowDefinition** の **Height** を、"140" から "Auto" に変更します。
-   **という名前の**TextBlock**を含んでいる**Grid`pageTitle` で、`x:Name="pageTitleGrid"` と `Height="60"` を設定します。 最初の 2 つの手順は、表示状態の setter を使って **RowDefinition** の高さを効果的に制御できるようにするための手順です。
-   `pageTitle` で、`Margin="-30,0,0,0"` を設定します。
-   コメント **で示されている**Grid`<!-- Content -->` で、`x:Name="contentGrid"` と `Margin="-18,12,0,0"` を設定します。
-   コメント **のすぐ上にある**TextBlock`<!-- Options -->` で、`Margin="0,0,0,24"` を設定します。
-   既定の **TextBlock** スタイル (ファイル内の最初のリソース) で、**FontSize** setter の値を "15" に変更します。
-   `OptionContentControlStyle` で、**FontSize** setter の値を "20" に変更します。 この手順と前の手順によって、すべてのデバイスで適切に動作する優れた書体見本を使うことができます。 これらは、Windows 8.1 アプリに使用していた "30" よりもはるかに柔軟なサイズです。
-   最後に、適切な Visual State Manager のマークアップをルートの **Grid** に追加します。

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>ユニバーサル スタイル設定


Windows 10 では、ボタンのテンプレートに同じタッチターゲットの埋め込みが使用されていないことがわかります。 小規模な変更を 2 つ行うことによって、この問題が解決されます。 最初に、QuizGameHost と QuizGameClient の両方の app.xaml に、次のマークアップを追加します。

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

次に、この setter を \\ビュー\\ClientView の `OptionButtonStyle` に追加します。

```xml
<Setter Property="Margin" Value="6"/>
```

上に示した最後の調整を行うことで、アプリの動作と外観が移植前と同じになります。また、アプリはどのデバイスでも実行できるという機能が追加されます。

## <a name="conclusion"></a>まとめ

このケース スタディで移植したアプリは、複数のプロジェクト、1 つのクラス ライブラリ、および多くのコードやユーザー インターフェイスを含んでいるため、比較的複雑なアプリになっています。 それでも、移植は非常に簡単に行われました。 移植性の点の一部は、Windows 10 developer platform と Windows 8.1 と Windows Phone 8.1 プラットフォームとの類似性に直接起因します。 また、元のアプリがモデル、ビュー モデル、およびビューを別個に維持するように設計されていたことも、その原因の 1 つです。
