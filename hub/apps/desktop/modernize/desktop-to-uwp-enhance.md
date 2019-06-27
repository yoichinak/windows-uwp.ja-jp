---
Description: ユニバーサル Windows プラットフォーム (UWP) Api を使用して、Windows 10 ユーザー向けのデスクトップ アプリケーションを強化します。
title: デスクトップ アプリでの UWP Api を使用します。
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4846a29e914ffed15e4c3dea938cc51cefd566e0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131944"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>デスクトップ アプリでの UWP Api を呼び出す

ユニバーサル Windows プラットフォーム (UWP) Api を使用すると、最新のエクスペリエンスを Windows 10 ユーザー向けに磨きをかける、デスクトップ アプリに追加します。

最初に、必要な参照を使用してプロジェクトを設定します。 次に、Windows 10 のエクスペリエンスをデスクトップ アプリに追加するコードから UWP Api を呼び出します。 Windows 10 のユーザーを個別にビルドまたは実行する Windows のバージョンに関係なくすべてのユーザーに同じバイナリを配布できます。

一部の UWP Api はパッケージ化されているデスクトップ アプリでのみサポートされている、 [MSIX パッケージ](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)します。 詳細については、次を参照してください。[利用可能な UWP Api](desktop-to-uwp-supported-api.md)します。

## <a name="set-up-your-project"></a>プロジェクトを設定します。

UWP API を使用するには、プロジェクトにいくつかの変更を加える必要があります。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows ランタイム Api を使用する .NET プロジェクトを変更します。

1. **[参照マネージャー]** ダイアログ ボックスを開き、 **[参照]** ボタンを選択して、 **[すべてのファイル]** を選択します。

    ![[参照の追加] ダイアログ ボックス](images/desktop-to-uwp/browse-references.png)

2. これらのファイルへの参照を追加します。

  |ファイル|Location|
  |--|--|
  |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
  |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
  |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
  |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. **[プロパティ]** ウィンドウで、各 *.winmd* ファイルの **[ローカルにコピー]** フィールドを **[False]** に設定します。

    ![[ローカルにコピー] フィールド](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Windows ランタイム Api を使用する C++ プロジェクトを変更します。

使用[C +/cli WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) Windows ランタイム Api を使用します。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。

C++ プロジェクトを構成する/cli WinRT を参照してください[C + を追加する Windows デスクトップ アプリケーション プロジェクトを変更する/cli WinRT サポート](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)します。

## <a name="add-windows-10-experiences"></a>Windows 10 エクスペリエンスの追加

これで、Windows 10 でアプリケーションをユーザーが実行する際に便利になる最新のエクスペリエンスを追加する準備ができました。 次の設計フローを使用します。

:white_check_mark:**最初に、追加するどのようなエクスペリエンスを決定します。**

選択肢はたくさんあります。 たとえば、購買注文の流れを簡略化を使用してできます[Api を収益化](/windows/uwp/monetize)、または[アプリケーションに向ける](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)を共有する興味深いものがある場合など、新しい画像を別のユーザーが投稿します。

![トースト](images/desktop-to-uwp/toast.png)

ユーザーがメッセージを無視したり、非表示にした場合でも、ユーザーはアクション センターで再度メッセージを表示し、クリックすることで、アプリを開くことができます。 これにより、アプリケーションで engagement を向上し、アプリケーションをオペレーティング システムに深く統合表示の追加されたボーナスが。 見せ経験があることのコードは、この記事で後で説明します。

参照してください、 [UWP ドキュメント](/windows/uwp/get-started/)他のアイデア。

:white_check_mark:**強化または拡張するかどうかを決定します。**

使用条件をよく耳を*強化*と*拡張*、平均値を正確にどのようなこれらの各用語の説明には少しご説明しましょう。

という用語*強化*(かどうかを選択した MSIX パッケージ内のアプリケーションをパッケージ化) デスクトップ アプリから直接呼び出すことができる Windows ランタイム Api を記述します。 Windows 10 エクスペリエンスを選択したら、識別を作成し、かどうか、その API に表示されます。 表示する必要のある Api[このリスト](desktop-to-uwp-supported-api.md)します。 これは、デスクトップ アプリから直接呼び出すことができる Api の一覧です。 API がこの一覧に表示されていない場合、その API に関連付けられている機能が UWP プロセス内でしか実行できないことが理由です。 多くの場合、UWP のマップ コントロールや Windows こんにちはセキュリティの確認など、UWP XAML をレンダリングする Api が含まれます。

> [!NOTE]
> 通常 UWP XAML をレンダリングする Api は、デスクトップから直接呼び出すことはできませんの代替アプローチを使用することができます。 UWP XAML コントロールまたはその他のカスタム ビジュアル エクスペリエンスをホストする場合は、使用[XAML Islands](xaml-islands.md)(Windows 10、バージョンが 1903 以降) および[ビジュアル層](visual-layer-in-desktop-apps.md)(Windows 10、バージョン 1803 以降)。 これらの機能は、パッケージまたはパッケージ化されていないデスクトップ アプリで使用できます。

MSIX パッケージでデスクトップ アプリをパッケージ化することを選択した場合に、別のオプションが、*拡張*UWP プロジェクトをソリューションに追加することで、アプリケーション。 デスクトップ プロジェクトは、アプリケーションのエントリ ポイントですが、UWP プロジェクトにアクセスできるすべての Api で表示されない[このリスト](desktop-to-uwp-supported-api.md)します。 デスクトップ アプリが UWP のプロセスを使用して通信を app service とそれらをセットアップする方法のガイダンスの多くがあります。 UWP プロジェクトを必要とするエクスペリエンスを追加する場合を参照してください[UWP コンポーネントを持つ拡張](desktop-to-uwp-extend.md)します。

:white_check_mark:**参照 API コントラクト**

デスクトップ アプリから直接 API を呼び出すことができる場合、ブラウザーとその API のリファレンス トピックの検索を開きます。
API の概要の下に、その API の API コントラクトを説明する表があります。 以下に表の例を示します。

![API コントラクト表](images/desktop-to-uwp/contract-table.png)

.NET ベースのデスクトップ アプリの場合、その API コントラクトへの参照を追加します。その後、そのファイルの **[ローカルにコピー]** プロパティを **[False]** に設定します。 C++ ベースのプロジェクトの場合、 **[追加のインクルード ディレクトリ]** に、このコントラクトを含むフォルダーのパスを追加します。

:white_check_mark:**お客様のエクスペリエンスを追加する Api を呼び出す**

以下に、前述の通知ウィンドウの表示に使用するコードを示します。 これでこれらの Api が表示される[一覧](desktop-to-uwp-supported-api.md)デスクトップ アプリに次のコードを追加して、すぐに実行できるようにします。

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

通知の詳細については、「[アダプティブ トースト通知と対話型トースト通知](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)」を参照してください。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Windows XP、Windows Vista、および Windows 7/8 インストール ベースのサポート

新しいブランチを作成し、別のコード ベースを管理することがなく、Windows 10 用のアプリケーションを最新化することができます。

Windows 10 ユーザー向けに個別のバイナリをビルドする場合は、条件付きコンパイルを使用します。 すべての Windows ユーザーに対して 1 組のバイナリをビルドして展開する場合は、ランタイム チェックを使用します。

各オプションを簡単に見ていきましょう。

### <a name="conditional-compilation"></a>条件付きコンパイル

1 つのコードベースを維持して、Windows 10 ユーザー向けだけに一連のバイナリをコンパイルします。

最初に、新しいビルド構成をプロジェクトに追加します。

![ビルド構成](images/desktop-to-uwp/build-config.png)

そのビルド構成の定数を作成する Windows ランタイム Api を呼び出すコードを識別するためにします。  

.NET ベースのプロジェクトの場合、この定数は**条件付きコンパイル定数**と呼ばれます。

![プリプロセッサ](images/desktop-to-uwp/compilation-constants.png)

C++ ベースのプロジェクトの場合、この定数は**プリプロセッサ定義**と呼ばれます。

![プリプロセッサ](images/desktop-to-uwp/pre-processor.png)

この定数を、任意の UWP コードのブロックの前に追加します。

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

この定数がアクティブなビルド構成で定義されている場合のみ、コンパイラでこのコードがビルドされます。

### <a name="runtime-checks"></a>ランタイム チェック

ユーザーが実行する Windows のバージョンに関係なく、1 組のバイナリをすべての Windows ユーザー向けにコンパイルできます。 アプリケーションは Windows ランタイム Api、ユーザーが実行する場合にのみアプリケーションをパッケージ化されたアプリケーションとして Windows 10

ランタイム チェックをコードに追加する最も簡単な方法では、この Nuget パッケージをインストールします。[デスクトップ ブリッジ ヘルパー](https://www.nuget.org/packages/DesktopBridge.Helpers/)しを使用して、``IsRunningAsUWP()``メソッド ゲートを Windows ランタイム Api を呼び出すコードをすべてオフにします。 このブログの詳細については投稿を参照してください。[デスクトップ ブリッジ - アプリケーションのコンテキストを識別する](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)します。

## <a name="related-samples"></a>関連するサンプル

* [Hello World サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [セカンダリ タイル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [ストア API サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [WinForms アプリケーション UWP UpdateTask を実装します。](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP サンプルへのブリッジのデスクトップ アプリ](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>サポートとフィードバック

**質問の回答を検索**

ご質問がある場合は、 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。

**ご意見や機能を提案します。**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) のページをご覧ください。
