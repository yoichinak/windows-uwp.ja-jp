---
description: この移植ガイドは最後まで読むことを強くお勧めしますが、早く先へ進んで、プロジェクトのビルドと実行の段階まで到達したいと思われるのも無理のないことです。
title: Windows Phone Silverlight から UWP への移植に関するトラブルシューティング
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a2c1353882ade36b5c1b82d0b75967d010ae0dc7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174826"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>Windows Phone Silverlight から UWP への移植に関するトラブルシューティング


前のトピックは、「[プロジェクトの移植](wpsl-to-uwp-porting-to-a-uwp-project.md)」でした。

この移植ガイドは最後まで読むことを強くお勧めしますが、早く先へ進んで、プロジェクトのビルドと実行の段階まで到達したいと思われるのも無理のないことです。 このために、重要でないコードに対してコメント アウトやスタブの挿入を行って一時的に先に進み、後でその部分に戻って対応することもできます。 このトピックには、トラブルシューティングの現象とその対処法を示す表が記載されており、以降のいくつかのトピックに示されている情報に代わるものではありませんが、この段階での作業に役立ちます。 以降のトピックを読み進む中で、いつでもこの表に戻って参考にすることができます。

## <a name="tracking-down-issues"></a>問題の検出

XAML 解析例外は診断が難しい場合があります。特に、わかりやすいエラー メッセージが例外に含まれていない場合は、診断が難しくなります。 デバッガーが初回例外をキャッチするように構成されていることを確してください (早い段階で解析例外のキャッチを試行するため)。 デバッガーで例外変数を調べて、HRESULT やメッセージ内に役立つ情報が含まれているかどうかを確認できます。 また、XAML パーサーを使って、Visual Studio の出力ウィンドウを調べ、エラー メッセージの出力を確認することもできます。

アプリが終了したとき、確認できたことが、ハンドルされていない例外が XAML マークアップの解析中にスローされたことのみである場合は、存在しないリソース (システム **TextBlock** スタイル キーなどのキーが Windows 10 アプリには存在せず、Windows Phone Silverlight アプリに存在するリソース) への参照が原因であると考えられます。 または、**UserControl**、カスタム コントロール、カスタム レイアウト パネルの内部で例外がスローされたことも考えられます。

最終手段として、バイナリ分割を使うことができます。 ページからマークアップのおよそ半分を削除し、アプリを再実行します。 これによって、エラーが削除した半分で発生しているか (いずれの場合でも、削除した部分はここで元に戻す必要があります)、または削除*しなかった*半分で発生しているかがわかります。 問題が特定されるまで、エラーを含む半分をさらに分割するプロセスを繰り返します。

## <a name="targetplatformversion"></a>TargetPlatformVersion

このセクションでは、Visual Studio で Windows 10 プロジェクトを開いたとき、"Visual Studio 更新プログラムが必要。 1 つ以上のプロジェクトでは、インストールされていないか、Visual Studio に対する今後の更新の一部として含まれるプラットフォーム SDK &lt;バージョン&gt; が必要です。" というメッセージが表示されます。

-   まず、インストールされている SDK for Windows 10 のバージョン番号を確認します。 **C: \\ Program Files (x86) \\ Windows kit \\ 10 \\ \\ &lt; &gt; **に移動し、 * &lt; versionfoldername &gt; *をメモします。このバージョンは、"メジャー. マイナー. ビルド. リビジョン" のようになります。
-   編集用のプロジェクト ファイルを開き、`TargetPlatformVersion` 要素と `TargetPlatformMinVersion` 要素を探します。 次のように編集します。 * &lt; versionfoldername &gt; *は、ディスク上で検出されたクワッド表記のバージョン番号に置き換えます。

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>各現象のトラブルシューティングと対処法

この表の対処法に関する情報では、自身で問題を解消するために十分な情報を提供することが意図されています。 以降のトピックを読み進むことで、こうした各問題についてさらに詳細な情報が提供されます。

| 症状 | 解決方法 |
|---------|--------|
| XAML パーサーまたはコンパイラで、エラー "_名前 "&lt;型名&gt;" が名前空間 […] に存在しません_" が発生します。 | XAML マークアップの名前空間プレフィックス宣言で &lt;型名&gt; がカスタム型である場合は、"clr-namespace" を "using" に変更し、すべてのアセンブリ トークンを削除します。 プラットフォームの種類では、この型はユニバーサル Windows プラットフォーム (UWP) に適用されないことを示すので、相当する要素を見つけ、マークアップを更新します。 直ちに発生する例としては、`phone:PhoneApplicationPage`、`shell:SystemTray.IsVisible` などが挙げられます。 | 
| XAML パーサーまたはコンパイラで、エラー "_メンバー "&lt;メンバー名&gt;" が認識されないか、アクセスできません_" または "_プロパティ "&lt;プロパティ名&gt;" が型 [...] で見つかりませんでした_" が発生します。 | これらのエラーは、ルート **Page** など、一部の型名を移植した後に発生し始めます。 メンバーまたはプロパティでは、この型は UWP に適用されないので、相当する要素を見つけ、マークアップを更新します。 直ちに発生する例としては、`SupportedOrientations`、`Orientation` などが挙げられます。 |
| XAML パーサーまたはコンパイラで、エラー "_接続可能なプロパティ [...] が見つかりませんでした [...]_" または "_接続可能なメンバー [...] が不明です_" が発生します。 | これは、添付プロパティではなく、型によって発生します。この場合には、既に該当する型に対してエラーが発生しており、そのエラーを修正すれば、このエラーも解消されます。 直ちに発生する例としては、`phone:PhoneApplicationPage.Resources`、`phone:PhoneApplicationPage.DataContext` などが挙げられます。 | 
|XAML のパーサーやコンパイラ、またはランタイム例外で、"_リソース '"&lt;リソースキー&gt;" を解決できませんでした_" というエラーが発生します。 | リソース キーは、ユニバーサル Windows プラットフォーム (UWP) アプリに適用されません。 同等の正しいリソースを探し、マークアップを更新します。 直ちに発生する例としては、`PhoneTextNormalStyle` などのシステム **TextBlock** スタイル キーが挙げられます。 |
| C# コンパイラで、エラー "_型または名前空間名 '&lt;名前&gt;' が見つかりませんでした [...]_" または "_型または名前空間名 '&lt;名前&gt;' が名前空間 [...] に存在しませんでした_" または "_型または名前空間名 '&lt;名前&gt;' が現在のコンテキストに存在しません_" が発生します。 | これはおそらく、コンパイラで型に対する適切な UWP の名前空間がまだ認識されていないことを示します。 問題解決のために Visual Studio の **[解決]** コマンドを使うことができます。 <br/>API がユニバーサル デバイス ファミリと呼ばれる API のセットに含まれていない場合 (つまり、API が拡張 SDK で実装されている場合)、[拡張 SDK](wpsl-to-uwp-porting-to-a-uwp-project.md) を使います。<br/>移植がそれほど簡単ではない場合もあります。 直ちに発生する例としては、`DesignerProperties`、`BitmapImage` などが挙げられます。 | 
|デバイスで実行すると、アプリが終了するか、Visual Studio から起動すると、"Windows ランタイム 8. x アプリをアクティブにできません [...]" というエラーが表示されます。 アクティベーション要求がエラー "Windows はターゲット アプリケーションと通信できませんでした。 これは通常、ターゲット アプリケーションのプロセスが中止したことを示します。 […]”. | この問題は、初期化時に独自のページ、またはバインド プロパティ (またはその他の型) で実行する命令型コードにあることが考えられます。 また、アプリの終了時に表示される XAML ファイルの解析でエラーが発生した可能性があります (Visual Studio から起動した場合は、これは起動ページになります)。 無効なリソース キーを検索するか、このトピックの「[問題の検出](#tracking-down-issues)」セクションのガイダンスを実行します。|
| _XamlCompiler error WMC0055: &lt; &gt; ' RectangleGeometry ' 型のプロパティ ' Clip ' にテキスト値 ' stream geometry ' を割り当てることはできません_ | UWP では、[Microsoft DirectX](/windows/desktop/directx) と XAML C++ UWP アプリの型を使います。 |
| _XamlCompiler エラー WMC0001: XML 名前空間 [...] で 'RadialGradientBrush' 型が不明です_ | UWP には、**RadialGradientBrush** 型がありません。 マークアップから **RadialGradientBrush** を削除し、[Microsoft DirectX](/windows/desktop/directx) と XAML C++ UWP アプリのその他の型を使います。 |
| _XamlCompiler error WMC0011: 要素 ' &lt; UIElement type ' で不明なメンバー ' OpacityMask ' が見つかりました &gt;_ | UWP の  [Microsoft DirectX](/windows/desktop/directx) と XAML C++ UWP アプリを使います。 |
| _SYSTEM.NI.DLL で、型 ' COMException ' の初回例外が発生しました。追加情報: アプリケーションは、別のスレッドに対してマーシャリングされたインターフェイスを呼び出しました。(HRESULT からの例外: 0x8001010E (RPC_E_WRONG_THREAD))。_ | ここで実行する作業は、UI スレッド上で行う必要があります。 [**CoreWindow.GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)) を呼び出します。 |
| アニメーションは実行中ですが、ターゲット プロパティに対しては無効です。 | アニメーションを独立して実行するか、またはアニメーションに対して `EnableDependentAnimation="True"` を設定します。 「 [アニメーション](wpsl-to-uwp-porting-xaml-and-ui.md)」を参照してください。 |
| Visual Studio で Windows 10 プロジェクトを開いたとき、"Visual Studio 更新プログラムが必要。 1 つ以上のプロジェクトでは、インストールされていないか、Visual Studio に対する今後の更新の一部として含まれるプラットフォーム SDK &lt;バージョン&gt; が必要です。" というメッセージが表示されます。 | このトピックの「[TargetPlatformVersion](#targetplatformversion)」セクションをご覧ください。 |
| InitializeComponent が xaml.cs ファイルで呼び出されると、System.InvalidCastException がスローされます。 | これは、同じ xaml.cs ファイルを共有している xaml ファイルが複数あり (少なくとも 1 つは MRT 修飾されたファイル)、要素が持つ x:Name 属性が 2 つの xaml ファイル間で整合性がとれていない場合に発生することがあります。 両方の xaml ファイルで同じ要素に同じ名前を追加してみるか、どちらの名前も省略してください。 | 

次のトピックは、「[XAML と UI の移植](wpsl-to-uwp-porting-xaml-and-ui.md)」です。