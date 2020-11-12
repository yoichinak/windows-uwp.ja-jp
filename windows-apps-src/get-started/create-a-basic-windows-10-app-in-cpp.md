---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: C++/CX を使った Hello World アプリの作成 (Windows 10)
description: Microsoft Visual Studio 2019 では、C++/CX を使って、Windows 10 (Windows 10 を実行するスマートフォンも含む) で実行されるアプリを開発できます。 これらのアプリでは、Extensible Application Markup Language (XAML) で定義された UI を使います。
ms.date: 06/11/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b1420fc5b5e74cba46b486b57be1098ac17ad1f5
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339830"
---
# <a name="create-a-hello-world-app-in-ccx"></a>"Hello, World!" アプリを C++/CX で作成する

> [!IMPORTANT]
> このチュートリアルでは C++/CX を使います。 Microsoft は C++/WinRT をリリースしました。これは、Windows ランタイム (WinRT) API 用に完全に標準化された、最新の C++17 言語プロジェクションです。 この言語の詳細については、「[C++/WinRT](../cpp-and-winrt-apis/index.md)」をご覧ください。

Microsoft Visual Studio では、Extensible Application Markup Language (XAML) で定義されている UI を使い、Windows 10 で動作するアプリを C++/CX で開発できます。

> [!NOTE]
> このチュートリアルでは、Visual Studio Community 2019 を使います。 異なるバージョンの Visual Studio を使っている場合には、見た目が多少異なることがあります。

## <a name="before-you-start"></a>開始する前に

-   このチュートリアルを完了するには、Windows 10 が実行されているコンピューターで、Visual Studio Community、または Community バージョン以外のいずれかの Visual Studio を使う必要があります。 ツールをダウンロードするには、[ツールの入手に関するページ](https://visualstudio.microsoft.com/downloads/)をご覧ください。
-   このトピックは、C++/CX、XAML、そして「[XAML の概要](../xaml-platform/xaml-overview.md)」で説明されている概念について基本的な知識があることを前提としています。
-   Visual Studio の既定のウィンドウ レイアウトを使用することを前提としています。 既定のレイアウトに戻すには、メニュー バーで **[ウィンドウ]**  >  **[ウィンドウ レイアウトのリセット]** の順に選択します。

## <a name="comparing-c-desktop-apps-to-windows-apps"></a>C++ デスクトップ アプリと Windows アプリの比較

C++ を使った Windows デスクトップのプログラミングに関する経験がある場合、UWP 用アプリのプログラミングには、通常の C++ プログラミングと同じ部分もありますが、新しい知識が必要になる部分もあります。

### <a name="whats-the-same"></a>これまでと同じ点

-   Windows ランタイム環境からアクセスできる Windows 関数のみをコードから呼び出す場合に限り、STL と CRT (一部例外あり)、さらに他のあらゆる C++ ライブラリを使うことができます。

-   使い慣れたビジュアル デザイナーがある場合は、Microsoft Visual Studio に組み込まれたデザイナーを引き続き使うことができます。また、より多機能な Blend for Visual Studio を使うこともできます。 手動で UI のコーディングを行うことに慣れている場合は、XAML を手動で記述できます。

-   Windows オペレーティング システムの型と独自のカスタム型を使うアプリをこれまでどおり作成します。

-   Visual Studio のデバッガー、プロファイラー、その他開発ツールをこれまでどおり使います。

-   Visual C++ コンパイラでネイティブ マシン コードにコンパイルされるアプリを引き続き作成します。 C++/CX で作成された UWP アプリは、マネージド ランタイム環境では実行されません。

### <a name="whats-new"></a>新機能

-   UWP アプリとユニバーサル Windows アプリの設計原則は、デスクトップ アプリの設計原則とは大きく異なります。 ウィンドウの境界線、ラベル、ダイアログ ボックスなどの強調は解除され、 コンテンツが最も目立つように表示されます。 優れたユニバーサル Windows アプリには、こうした原則が計画段階の初期から活かされています。

-   XAML を使って UI 全体を定義します。 Windows ユニバーサル アプリでは、MFC アプリや Win32 アプリよりもはるかに明確に UI と基本的なプログラム ロジックが分離されます。 XAML を使った UI の見た目の調整と、コード ファイルでの動作のプログラミングを、異なる担当者が並行して行うことができます。

-   プログラミングの対象は主に Windows ランタイム (操作しやすい、オブジェクト指向の新しい API) ですが、Windows デバイス上で一部の機能にこれまでどおり Win32 を使うこともできます。

-   Windows ランタイムのオブジェクトを利用したり作成したりするには C++/CX を使います。 C++/CX は、C++ の例外処理、デリゲート、イベントのほか、動的に作成されるオブジェクトの自動参照カウントに対応しています。 C++/CX を使うとき、ベースの COM と Windows アーキテクチャの詳細は、アプリ コードからは見えません。 詳しくは、「[C++/CX 言語のリファレンス](/cpp/cppcx/visual-c-language-reference-c-cx)」をご覧ください。

-   アプリは、そのアプリ自体に含まれる型、アプリで使うリソース、必要な機能 (ファイル アクセス、インターネット アクセス、カメラ アクセスなど) に関するメタデータも含むパッケージにコンパイルされます。

-   Microsoft Store と Windows Phone ストアでは、認定プロセスを通じてアプリの安全性が検証され、何百万ものユーザーに公開されます。

## <a name="hello-world-store-app-in-ccx"></a>C++/CX を使った Hello World ストア アプリ

初めてのアプリは "Hello World" という名前です。このアプリでは、インタラクティビティ、レイアウト、スタイルに関する基本的な機能の使い方を学びます。 アプリは、Windows ユニバーサル アプリ プロジェクト テンプレートを使って作成します。 Windows 8.1 や Windows Phone 8.1 用のアプリを開発したことがあれば、Visual Studio で 3 つのプロジェクト (Windows アプリ用、Windows Phone アプリ用、共有コード用) を使った経験があるでしょう。 Windows 10 ユニバーサル Windows プラットフォーム (UWP) を使うと、プロジェクトを 1 つ開発すれば、すべてのデバイス (Windows 10 が実行されているデスクトップ コンピューターやノート PC、タブレット、携帯電話、VR デバイスなどのデバイス) で動作させることができます。

それでは、次に示す基礎から始めましょう。

-   Visual Studio でユニバーサル Windows プロジェクトを作成する方法。

-   作成されるプロジェクトとファイルを把握する方法。

-   Visual C++ コンポーネント拡張機能 (C++/CX) の拡張機能を把握する方法と、それを使うタイミング。

**まず Visual Studio でソリューションを作成する**

1.  Visual Studio のメニュー バーから **[ファイル]**  >  **[新規作成]**  >  **[プロジェクト...]** の順に選択します。

2.  **[新しいプロジェクトの作成]** ダイアログ ボックスで、 **[空のアプリ (ユニバーサル Windows - C++/CX)]** を選択します。  このオプションが見つからない場合は、ユニバーサル Windows アプリ開発ツールがインストールされていることを確認します。 詳しくは、「[準備](/windows/apps/get-started/get-set-up)」をご覧ください。

![[新しいプロジェクトの作成] ダイアログ ボックスの C++/CX プロジェクト テンプレート ](images/vs2019-uwp-01.png)

3.  **[次へ]** を選択し、プロジェクトの名前を入力します。 ここでは、名前を "HelloWorld" とします。

4.  **[作成]** を選択します。

> [!NOTE]
> Visual Studio を初めて使う場合は、[設定] ダイアログ ボックスが表示され、 **開発者モード** を有効にするよう求められることがあります。 開発者モードは、アプリをストアからだけではなく、直接実行するためのアクセス許可など、特定の機能を有効にする特別な設定です。 詳しくは、「[デバイスを開発用に有効にする](/windows/apps/get-started/enable-your-device-for-development)」をご覧ください。 先に進むには、 **[開発者モード]** を選択し、 **[はい]** をクリックしてダイアログ ボックスを閉じます。

   プロジェクト ファイルが作られます。

先に進む前に、ソリューションの構成内容を調べてみましょう。

![ユニバーサル アプリ ソリューション (ノードを折りたたんだところ)](images/vs2017-uwp-02.png)

### <a name="about-the-project-files"></a>プロジェクト ファイルについて

プロジェクト フォルダー内のすべての .xaml ファイルには、対応する .xaml.h ファイルと .xaml.cpp ファイルが同じフォルダーに、.g ファイルと .g.hpp ファイルが [生成されたファイル] フォルダーにそれぞれ存在します。このフォルダーはプロジェクト内ではなく、ディスク上にあります。 UI 要素を作成してデータ ソースに接続 (DataBinding) するには、XAML ファイルに変更を加えます。 イベント ハンドラーにカスタム ロジックを追加するには、.h ファイルと .cpp ファイルに変更を加えます。 自動生成ファイルは、XAML マークアップから C++/CX への変換を表します。 これらのファイルは変更しないでください。ただし、ファイルを観察すると、分離コードの働きをより深く理解できます。 基本的に、生成されたファイルには、XAML ルート要素の部分クラスの定義が記述されています。これは、\*.xaml.h ファイルや .cpp ファイルで変更するクラスと同じです。 生成されたファイルには、XAML UI の子要素がクラスのメンバーとして宣言されており、開発者がコードの中で参照することができます。 ビルド時には、生成されたコードと自分で記述したコードとがマージされてクラス定義が完成し、コンパイルされます。

まずプロジェクト ファイルを見てみましょう。

-   **App.xaml、App.xaml.h、App.xaml.cpp:** アプリのエントリ ポイントとなる Application オブジェクトを表します。 App.xaml に、ページ固有の UI マークアップは含まれませんが、UI のスタイルなどの要素を追加して任意のページからアクセスすることができます。 分離コード ファイルには、 **OnLaunched** イベントと **OnSuspending** イベントのハンドラーが含まれます。 通常、ここには、起動時にアプリを初期化したり、中断または終了時にクリーンアップ処理を実行したりするためのカスタム コードを追加します。
-   **MainPage.xaml、MainPage.xaml.h、MainPage.xaml.cpp:** アプリの既定の "スタート" ページに関する XAML マークアップと分離コードが含まれます。 ナビゲーション サポートやビルトイン コントロールはありません。
-   **pch.h、pch.cpp:** プリコンパイル済みヘッダー ファイルと、それをプロジェクトにインクルードするファイルです。 pch.h には、変更頻度が低く、ソリューション内の他のファイルにインクルードされるヘッダーを含めることができます。
-   **Package.appxmanifest:** アプリが必要とするデバイスの機能、そしてアプリのバージョン情報やその他のメタデータを表す XML ファイルです。 このファイルをダブルクリックすると、 **マニフェスト デザイナー** で開くことができます。
-   **HelloWorld\_TemporaryKey.pfx:** Visual Studio からこのコンピューターにアプリを展開できるようにするキーです。

## <a name="a-first-look-at-the-code"></a>初めてのコード

共有プロジェクト内の App.xaml.h と App.xaml.cpp のコードをよく見ると、ほとんどが、見覚えのある ISO C++ コードであることがわかります。 ただし、Windows ランタイム アプリの開発が初めての場合や、これまで C++/CLI で作業を行ってきた場合は、あまり馴染みのない構文要素も中には存在します。 C++/CX でよく使われる標準的ではない構文要素を以下に示します。

**Ref クラス**

Windows ランタイムのほぼすべてのクラスは、Windows API のすべての型 (XAML コントロール、アプリ内のページ、App クラス自体、すべてのデバイス オブジェクトとネットワーク オブジェクト、すべてのコンテナー型) を含んでおり、 **ref クラス** として宣言されます ( **value class** または **value struct** の Windows 型もわずかに存在します)。 ref クラスはすべての言語から利用できます。 C++/CX では、これらの型の有効期間が (ガベージ コレクションではなく) 自動参照カウントによって制御されるため、それらのオブジェクトを明示的に削除することはできません。 ref クラスは独自に作成することもできます。

```cpp
namespace HelloWorld
{
   /// <summary>
   /// An empty page that can be used on its own or navigated to within a Frame.
   /// </summary>
   public ref class MainPage sealed
   {
      public:
      MainPage();
   };
}
```    

Windows ランタイムのすべての型は名前空間内で宣言する必要があり、ISO C++ の場合は異なり、型そのものがアクセシビリティ修飾子を持ちます。 **public** 修飾子を持つクラスは、その名前空間の外の Windows ランタイム コンポーネントから参照することができます。 **sealed** キーワードは、基底クラスとして使うことができないことを意味します。 ほとんどの ref クラスは sealed です。クラスの継承は、JavaScript が認識しないため、あまり広くは使われません。

**ref new** と **^ (ハット)**

ref クラスの変数は ^ (ハット) 演算子を使って宣言します。ref new キーワードでオブジェクトをインスタンス化することができます。 それ以降、オブジェクトのインスタンス メソッドにアクセスする際は、C++ のポインターと同様の -> 演算子を使います。 静的メソッドにアクセスする場合は、ISO C++ と同様、:: 演算子を使います。

次のコードは、完全修飾名を使ってオブジェクトをインスタンス化し、-> 演算子を使ってインスタンス メソッドを呼び出しています。

```cpp
Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
     ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();

bitmapImage->SetSource(fileStream);
```

このコードは、.cpp ファイルで、`using namespace  Windows::UI::Xaml::Media::Imaging` ディレクティブと auto キーワードを追加し、次のように記述するのが一般的です。

```cpp
auto bitmapImage = ref new BitmapImage();
bitmapImage->SetSource(fileStream);
```

**[プロパティ]**

ref クラスにはプロパティを与えることができます。プロパティは、マネージ言語のプロパティと同様、それを利用する側のコードからはフィールドとして見える特殊なメンバー関数です。

```cpp
public ref class SaveStateEventArgs sealed
{
   public:
   // Declare the property
   property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
   {
      Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
   }
   ...
};

   ...
   // consume the property like a public field
   void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
   {    
      if (mruToken != nullptr && !mruToken->IsEmpty())
   {
      e->PageState->Insert("mruToken", mruToken);
   }
}
```

**デリゲート**

マネージ言語と同様、デリゲートは、特定のシグニチャの関数をカプセル化する参照型です。 ほとんどの場合、イベントとそのハンドラーに使われます。

```cpp
// Delegate declaration (within namespace scope)
public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

// Event declaration (class scope)
public ref class NavigationHelper sealed
{
   public:
   event LoadStateEventHandler^ LoadState;
};

// Create the event handler in consuming class
MainPage::MainPage()
{
   auto navigationHelper = ref new Common::NavigationHelper(this);
   navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
}
```

## <a name="adding-content-to-the-app"></a>アプリへのコンテンツの追加

それでは、アプリにコンテンツを追加しましょう。

**手順 1: スタート ページの変更**

1.  **ソリューション エクスプローラー** で、MainPage.xaml を開きます。
2.  ルート [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) の終了タグの直前に次の XAML を追加して、UI に使うコントロールを作成します。 この XAML には、ユーザーの名前をたずねる [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、ユーザーの名前を受け取る [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) 要素、 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)、別の **TextBlock** 要素を持つ [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) が含まれます。

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
    ```

3.  ここまでの作業で、ごく基本的なユニバーサル Windows アプリが作成されました。 UWP アプリの動作や外観を確かめるには、F5 キーを押してデバッグ モードでアプリをビルド、展開、起動します。

最初に、既定のスプラッシュ画面が表示されます。 この画面には、アプリのマニフェスト ファイルに指定された画像 (Assets\\SplashScreen.scale-100.png) と背景色があります。 スプラッシュ画面をカスタマイズする方法については、「[スプラッシュ画面の追加](/previous-versions/windows/apps/hh465332(v=win.10))」をご覧ください。

スプラッシュ画面が消えると、アプリが表示されます。 アプリのメインページが表示されます。

![コントロールがある UWP アプリ画面](images/xaml-hw-app2.png)

特別な機能はありませんが、ともかくこれで、初めてのユニバーサル Windows プラットフォーム アプリの作成は完了です。

アプリのデバッグを停止して閉じるには、Visual Studio に戻り、Shift キーを押しながら F5 キーを押します。

詳しくは、「[Visual Studio からのストア アプリの実行](/visualstudio/debugger/run-store-apps-from-visual-studio)」をご覧ください。

アプリの [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) に文字を入力することはできますが、この時点では [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) をクリックしても何も起こりません。 この後の手順で、ユーザーに合わせたあいさつを表示する、ボタンの [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベント用のイベント ハンドラーを作成します。

## <a name="step-2-create-an-event-handler"></a>手順 2:イベント ハンドラーの作成

1.  MainPage.xaml (XAML ビューまたはデザイン ビュー) で、先に追加した [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) の "Say Hello" [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) を選びます。
2.  F4 キーを押して **プロパティ ウィンドウ** を開き、[イベント] ボタン (![[イベント] ボタン](images/eventsbutton.png)) を選択します。
3.  [  **Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントを探します。 このテキスト ボックスに、 **Click** イベントを処理する関数の名前を入力します。 この例では、「Button\_Click」と入力します。

    ![プロパティ ウィンドウのイベント ビュー](images/xaml-hw-event.png)

4.  Enter キーを押します。 MainPage.xaml.cpp にイベント ハンドラー メソッドが作成され、イベントの発生時に実行されるコードを追加できるように開きます。

   同時に、MainPage.xaml で、 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) の XAML が更新されて、次のように [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベント ハンドラーが宣言されます。

```xaml
<Button Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```

   これを XAML コードに手動で入力することもできます。これは、デザイナーが読み込みに失敗する場合に役立つことがあります。 手動で入力する場合は、「Click」と入力すると、IntelliSense によって新しいイベント ハンドラーを追加するオプションがポップアップされます。 このように、Visual Studio によって必要なメソッド宣言とスタブが作成されます。

   デザイナーは、レンダリング中にハンドルされない例外が発生すると、読み込みに失敗します。 デザイナーでのレンダリングでは、ページの設計時のバージョンが実行されます。 これは、ユーザー コードの実行を無効にする場合に便利です。 そのためには、 **[ツール]、[オプション]** の順にクリックして、開いたダイアログ ボックスで設定を変更します。 **[XAML デザイナー]** で、 **[プロジェクト コードを XAML デザイナーで実行する (サポートされている場合)]** チェック ボックスをオフにします。

5.  MainPage.xaml.cpp で、作成した **Button\_Click** イベント ハンドラーに次のコードを追加します。 このコードは、`nameInput` [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) コントロールからユーザー名を取得し、それを使ってあいさつを作ります。 結果は、`greetingOutput` [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) に表示されます。

```cpp
void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    greetingOutput->Text = "Hello, " + nameInput->Text + "!";
}
```

6.  プロジェクトをスタートアップ プロジェクトとして設定し、F5 キーを押してアプリをビルド、実行します。 テキスト ボックスに名前を入力してボタンをクリックすると、ユーザーに合わせたあいさつが表示されます。

![メッセージが表示されたアプリ画面](images/xaml-hw-app4.png)

## <a name="step-3-style-the-start-page"></a>手順 3:スタート ページのスタイルを設定する

### <a name="choosing-a-theme"></a>テーマを選ぶ

アプリの外観は簡単にカスタマイズできます。 既定では、アプリは淡色スタイルのリソースを使います。 システム リソースには、淡色テーマも含まれています。 試しにそれを使って、どのように表示されるか見てみましょう。

**濃色テーマに切り替えるには**

1.  App.xaml を開きます。
2.  開始 [**Application**](/uwp/api/Windows.UI.Xaml.Application) タグで、 [**RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme) プロパティを編集し、その値を **Dark** に設定します。

    ```xaml
    RequestedTheme="Dark"
    ```

    濃色テーマを追加した [**Application**](/uwp/api/Windows.UI.Xaml.Application) タグ全体を次に示します。

    ```xaml
    <Application
    x:Class="HelloWorld.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    RequestedTheme="Dark">
    ```

3.  F5 キーを押して、アプリをビルドし、実行します。 濃色テーマが使われていることに注目してください。

    ![濃色テーマのアプリ画面](images/xaml-hw-app3.png)

どちらを使えばいいでしょうか。 どちらでも好きなほうを使用できます。 お勧めするとすれば、主に画像やビデオを表示するアプリには濃色テーマ、テキストが大量に含まれるアプリには淡色テーマです。 カスタム配色を使う場合は、アプリの外観に最もよく合ったテーマを使ってください。 このチュートリアルの残りの部分では、スクリーンショットで淡色テーマを使います。

**注:** テーマは、アプリの起動時に適用されます。アプリの実行中にテーマを変更することはできません。

### <a name="using-system-styles"></a>システム スタイルの使用

現時点では、Windows アプリ内のテキストが小さすぎて判読困難です。 システム スタイルを適用してこの点を修正してみましょう。

**要素のスタイルを変更するには**

1.  Windows プロジェクトで、MainPage.xaml を開きます。
2.  XAML ビューまたはデザイン ビューで、前に追加した "What's your name?" [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) を選びます。
3.  **[プロパティ]** ウィンドウ ( **F4** ) の右上にある [プロパティ] ボタン (![Properties button](images/propertiesbutton.png)) をクリックします。
4.  **[Text]** グループを展開し、フォント サイズを 18px に設定します。
5.  **[その他]** グループを展開し、 **Style** プロパティを探します。
6.  プロパティ マーカー ( **Style** プロパティの右にある緑色のボックス) をクリックし、その後、メニューから **[システム リソース]**  >  **[BaseTextBlockStyle]** の順に選択します。

     **BaseTextBlockStyle** は、<root>\\Program Files\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml の [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) で定義されているリソースです。

    ![プロパティ ウィンドウのプロパティ ビュー](images/xaml-hw-style-cpp.png)

     XAML デザイン サーフェイスで、テキストの外観が変化します。 XAML エディターで、 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) の XAML が更新されます。

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

7.  このプロセスを繰り返してフォント サイズを設定し、 **BaseTextBlockStyle** を `greetingOutput`[**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 要素に割り当てます。

    **ヒント:** この [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) にはテキストはありませんが、マウス ポインターを XAML デザイン サーフェイスの上に移動すると、対応する位置に青色の輪郭が表示されて、選択できます。  

    XAML は次のようになります。

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
    </StackPanel>
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" x:Name="greetingOutput"/>
</StackPanel>
```

8.  F5 キーを押して、アプリをビルドし、実行します。 次のように表示されます。

![アプリ画面のテキストが大きくなった](images/xaml-hw-app5.png)

### <a name="step-4-adapt-the-ui-to-different-window-sizes"></a>手順 4:異なるウィンドウ サイズに合わせて UI を調整する

次に、モバイル デバイスで適切に表示されるように、さまざまな画面サイズに合わせて UI を調整します。 これを行うには、 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) を追加して、さまざまな表示状態に適用されるプロパティを設定します。

**UI レイアウトを調整するには**

1.  XAML エディターで、ルートの [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) 要素の開始タグの後に、この XAML ブロックを追加します。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="wideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="641" />
            </VisualState.StateTriggers>
        </VisualState>
        <VisualState x:Name="narrowState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="0" />
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

2.  ローカル コンピューターでアプリをデバッグします。 UI は、ウィンドウが 641 DIP (デバイスに依存しないピクセル) より狭くならない限り、前と同じように表示されることに注意してください。
3.  モバイル デバイス エミュレーターでアプリをデバッグします。 `narrowState` で定義したプロパティが UI に使用され、小さい画面で適切に表示されていることに注意してください。

![スタイル付きテキストを表示するモバイル アプリ画面](images/hw10-screen2-mob.png)

以前のバージョンの XAML で [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) を使ったことがある場合は、この XAML では簡素化された構文が使用されていることに気付くかもしれません。

`wideState` という名前の [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) で、 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) の [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) プロパティが 641 に設定されています。 これは、ウィンドウの幅が 641 DIP という最小値以上である場合に限って、状態が適用されることを意味します。 この状態には [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) オブジェクトを定義していないため、XAML でページのコンテンツに対して定義したレイアウト プロパティが使用されます。

2 つ目の [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) である `narrowState` で、 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) の [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) プロパティが 0 に設定されています。 この状態は、ウィンドウの幅が 0 より大きく 641 DIP より小さい場合に適用されます (641 DIP では、`wideState` が適用されます)。この状態では、いくつかの [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) オブジェクトを設定して、UI のコントロールのレイアウト プロパティを変更します。

-   `contentPanel` の左側の余白を 120 から 20 に減らします。
-   `inputPanel` 要素の [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) を **Horizontal** から **Vertical** に変更します。
-   4 DIP の上余白を `inputButton` 要素に追加します。

### <a name="summary"></a>要約

これで、最初のチュートリアルは終わりです。 このチュートリアルでは、Windows ユニバーサル アプリにコンテンツを追加する方法、そのコンテンツで対話式操作を実現する方法、見た目を変更する方法の 3 点について説明しました。

## <a name="next-steps"></a>次の手順

Windows 8.1 や Windows Phone 8.1 を対象とする Windows ユニバーサル アプリ プロジェクトがある場合は、そのプロジェクトを Windows 10 に移植できます。 この移植を自動的に行うプロセスはありませんが、手動で実行することができます。 新しい Windows ユニバーサル プロジェクトを使って開発を始めることで、最新のプロジェクト システム構造を入手し、お使いのコード ファイルをプロジェクトのディレクトリ構造にコピーしたり、項目をプロジェクトに追加したりすることができます。また、このトピックのガイダンスに従い、 [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) を使って XAML を書き換えることもできます。 詳しくは、「[Windows ランタイム 8 プロジェクトのユニバーサル Windows プラットフォーム (UWP) プロジェクトへの移植](../porting/w8x-to-uwp-porting-to-a-uwp-project.md)」と「[ユニバーサル Windows プラットフォームへの移植 (C++)](/cpp/porting/porting-to-the-universal-windows-platform-cpp)」をご覧ください。

UWP アプリと統合する既存の C++ コードがある場合、たとえば、既存のアプリケーションに新しい UWP UI を作成する場合は、[ユニバーサル Windows プロジェクトでの既存の C++ コードの使用](/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app)に関するページをご覧ください。