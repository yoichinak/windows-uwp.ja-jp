---
Description: ユニバーサル Windows プラットフォーム (UWP) Api を使用して、Windows 10 ユーザー向けのデスクトップアプリケーションを拡張します。
title: デスクトップアプリで UWP Api を使用する
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ca9e91233206f0e97d17fdbdd7b0fd09a2897cd8
ms.sourcegitcommit: 3710117f24adb8555aa94b372db814e5d30ae45a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73427087"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>デスクトップアプリで UWP Api を呼び出す

ユニバーサル Windows プラットフォーム (UWP) Api を使用して、Windows 10 ユーザー向けにセットアップされたデスクトップアプリに最新のエクスペリエンスを追加することができます。

まず、必要な参照を使用してプロジェクトを設定します。 次に、コードから UWP Api を呼び出して、Windows 10 エクスペリエンスをデスクトップアプリに追加します。 Windows 10 ユーザー用に個別にビルドすることも、実行する Windows のバージョンに関係なく、すべてのユーザーに同じバイナリを配布することもできます。

一部の UWP Api は、[パッケージ id](modernize-packaged-apps.md)を持つデスクトップアプリでのみサポートされています。 詳細については、「[使用可能な UWP api](desktop-to-uwp-supported-api.md)」を参照してください。

## <a name="set-up-your-project"></a>プロジェクトを設定する

UWP API を使用するには、プロジェクトにいくつかの変更を加える必要があります。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows ランタイム Api を使用するように .NET プロジェクトを変更する

.NET プロジェクトには、次の2つのオプションがあります。

* アプリが Windows 10 バージョン1803以降を対象としている場合は、必要なすべての参照を提供する NuGet パッケージをインストールできます。
* または、手動で参照を追加することもできます。

#### <a name="to-use-the-nuget-option"></a>NuGet オプションを使用するには

1. [パッケージ参照](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、ツール、 **NuGet パッケージマネージャー-> パッケージマネージャーの設定** の順にクリックし > ます。
    2. **既定のパッケージ管理形式**として**PackageReference**が選択されていることを確認します。

2. Visual Studio でプロジェクトを開き、**ソリューションエクスプローラー**でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

3. **[NuGet パッケージマネージャー]** ウィンドウで、 **[参照]** タブを選択し、`Microsoft.Windows.SDK.Contracts`を検索します。

4. `Microsoft.Windows.SDK.Contracts` パッケージが見つかったら、 **[NuGet パッケージマネージャー]** ウィンドウの右側のウィンドウで、ターゲットにする Windows 10 のバージョンに基づいて、インストールするパッケージの**バージョン**を選択します。

    * **10.0.18362**: Windows 10 バージョン1903では、このオプションを選択します。
    * **10.0.17763**: Windows 10 バージョン1809では、このオプションを選択します。
    * **10.0.17134**: Windows 10 バージョン1803では、このオプションを選択します。

5. **[インストール]** をクリックします。

#### <a name="to-add-the-required-references-manually"></a>必要な参照を手動で追加するには

1. **[参照マネージャー]** ダイアログ ボックスを開き、 **[参照]** ボタンを選択して、 **[すべてのファイル]** を選択します。

    ![[参照の追加] ダイアログ ボックス](images/desktop-to-uwp/browse-references.png)

2. これらのファイルへの参照を追加します。

    |ファイル|位置情報|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows. winmd|C:\Program Files (x86) \Windows Kits\10\UnionMetadata\\<*sdk バージョン*>/ファサード|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*sdk バージョン*> \Windows.Foundation.UniversalApiContract\<*バージョン*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\\<*sdk バージョン*> \Windows.Foundation.FoundationContract\<*バージョン*>|

3. **[プロパティ]** ウィンドウで、各 *.winmd* ファイルの **[ローカルにコピー]** フィールドを **[False]** に設定します。

    ![[ローカルにコピー] フィールド](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Windows ランタイム Api C++を使用するように Win32 プロジェクトを変更する

Windows ランタイム api を使用するには、 [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/)を使用します。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。

/WinRT 用にプロジェクトC++を構成するには、次のようにします。

* 新しいプロジェクトの場合は、 [ C++Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)をインストールし、その拡張機能にC++含まれているいずれかのプロジェクトテンプレートを使用することができます。
* 既存のプロジェクトの場合は、プロジェクトに[Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをインストールできます。

これらのオプションの詳細については、こちらの[記事](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を参照してください。

## <a name="add-windows-10-experiences"></a>Windows 10 エクスペリエンスの追加

これで、Windows 10 でアプリケーションをユーザーが実行する際に便利になる最新のエクスペリエンスを追加する準備ができました。 次の設計フローを使用します。

:white_check_mark: **追加するエクスペリエンスを最初に決定する**

選択肢はたくさんあります。 たとえば、[収益化 api](/windows/uwp/monetize)を使用して注文書フローを簡素化したり、他のユーザーが投稿した新しい画像など、共有の興味深いものがある場合は[アプリケーションに直接注目](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)したりすることができます。

![トースト](images/desktop-to-uwp/toast.png)

ユーザーがメッセージを無視したり、非表示にした場合でも、ユーザーはアクション センターで再度メッセージを表示し、クリックすることで、アプリを開くことができます。 これにより、アプリケーションとの連携が向上し、アプリケーションがオペレーティングシステムと密接に統合されるようになりました。 この記事の後半では、その操作のコードについて説明します。

詳細については、 [UWP のドキュメント](/windows/uwp/get-started/)を参照してください。

:white_check_mark: **強化するのか、拡張するのかを決定する**

多くの場合、"拡張と*拡張*" という用語を使用*して、* これらの各用語の意味を正確に説明します。

デスクトップアプリから直接呼び出すことができる Windows ランタイム Api (アプリケーションを MSIX パッケージでパッケージ化することを選択したかどうかにかかわらず) を説明するために、*拡張*機能という用語を使用します。 Windows 10 エクスペリエンスを選択したら、それを作成するために必要な Api を特定し、その API が[この一覧](desktop-to-uwp-supported-api.md)に表示されているかどうかを確認します。 これは、デスクトップアプリから直接呼び出すことができる Api の一覧です。 API がこの一覧に表示されていない場合、その API に関連付けられている機能が UWP プロセス内でしか実行できないことが理由です。 多くの場合、UWP マップコントロールや Windows Hello セキュリティプロンプトなどの UWP XAML をレンダリングする Api が含まれます。

> [!NOTE]
> UWP XAML をレンダリングする Api は通常、デスクトップから直接呼び出すことができませんが、別の方法を使用することもできます。 UWP XAML コントロールまたはその他のカスタムビジュアルエクスペリエンスをホストする場合は、 [Xaml アイランド](xaml-islands.md)(windows 10、バージョン1903以降) と[ビジュアル層](visual-layer-in-desktop-apps.md)(windows 10 version 1803 以降) を使用できます。 これらの機能は、パッケージまたはパッケージ化されていないデスクトップアプリで使用できます。

MSIX パッケージでデスクトップアプリをパッケージ化することを選択した場合、別のオプションとして、UWP プロジェクトをソリューションに追加してアプリケーションを*拡張*することができます。 デスクトッププロジェクトはアプリケーションのエントリポイントですが、UWP プロジェクトを使用すると、[この一覧](desktop-to-uwp-supported-api.md)に表示されていないすべての api にアクセスできます。 デスクトップアプリは、app service を使用して UWP プロセスと通信できます。これを設定する方法については多くのガイダンスがあります。 UWP プロジェクトを必要とするエクスペリエンスを追加する場合は、「 [uwp コンポーネントによる拡張](desktop-to-uwp-extend.md)」を参照してください。

:white_check_mark: **API コントラクトを参照する**

デスクトップアプリから API を直接呼び出すことができる場合は、ブラウザーを開き、その API のリファレンストピックを検索します。
API の概要の下に、その API の API コントラクトを説明する表があります。 以下に表の例を示します。

![API コントラクト表](images/desktop-to-uwp/contract-table.png)

.NET ベースのデスクトップ アプリの場合、その API コントラクトへの参照を追加します。その後、そのファイルの **[ローカルにコピー]** プロパティを **[False]** に設定します。 C++ ベースのプロジェクトの場合、 **[追加のインクルード ディレクトリ]** に、このコントラクトを含むフォルダーのパスを追加します。

:white_check_mark: **エクスペリエンスを追加するための API を呼び出す**

以下に、前述の通知ウィンドウの表示に使用するコードを示します。 これらの Api はこの[一覧](desktop-to-uwp-supported-api.md)に表示されるので、このコードをデスクトップアプリに追加して、すぐに実行することができます。

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

新しいブランチを作成したり、別のコードベースを保持したりすることなく、Windows 10 のアプリケーションを最新化することができます。

Windows 10 ユーザー向けに個別のバイナリをビルドする場合は、条件付きコンパイルを使用します。 すべての Windows ユーザーに対して 1 組のバイナリをビルドして展開する場合は、ランタイム チェックを使用します。

各オプションを簡単に見ていきましょう。

### <a name="conditional-compilation"></a>条件付きコンパイル

1 つのコードベースを維持して、Windows 10 ユーザー向けだけに一連のバイナリをコンパイルします。

最初に、新しいビルド構成をプロジェクトに追加します。

![ビルド構成](images/desktop-to-uwp/build-config.png)

そのビルド構成に対して、Windows ランタイム Api を呼び出すコードを識別する定数を作成します。  

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

ユーザーが実行する Windows のバージョンに関係なく、1 組のバイナリをすべての Windows ユーザー向けにコンパイルできます。 アプリケーションは、ユーザーが Windows 10 でパッケージアプリケーションとしてアプリケーションを実行している場合にのみ、Windows ランタイム Api を呼び出します。

コードにランタイムチェックを追加する最も簡単な方法は、この Nuget パッケージをインストールすることです:[デスクトップブリッジヘルパー](https://www.nuget.org/packages/DesktopBridge.Helpers/) 。次に、``IsRunningAsUWP()`` メソッドを使用して、Windows ランタイム api を呼び出すすべてのコードをゲートします。 詳細については、[デスクトップ ブリッジを使用したアプリケーションのコンテキストの特定](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/)に関するブログ記事を参照してください。

## <a name="related-samples"></a>関連するサンプル

* [Hello World サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [セカンダリタイル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [ストア API のサンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask を実装する WinForms アプリケーション](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [デスクトップアプリブリッジと UWP サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>サポートとフィードバック

**質問に対する回答を検索する**

ご質問がある場合は、 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。

**フィードバックの提供または機能に関する提案**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial) のページをご覧ください。
