---
description: Windows ランタイム API を使用して Windows 10 ユーザー向けデスクトップ アプリケーションを強化します。
title: デスクトップ アプリで Windows ランタイム API を呼び出す
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e58315ed70b889e1369e8c13a563f320c0ca1948
ms.sourcegitcommit: a222ad0e2d97e35a60000c473808c678395376ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2020
ms.locfileid: "89479082"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>デスクトップ アプリで Windows ランタイム API を呼び出す

ユニバーサル Windows プラットフォーム (UWP) API を使用して、Windows 10 ユーザーの利便性を高める最新のエクスペリエンスをデスクトップ アプリに追加できます。

まず、必要な参照を含むプロジェクトを設定します。 次に、コードから Windows ランタイム API を呼び出して、Windows 10 エクスペリエンスをデスクトップ アプリに追加します。 Windows 10 ユーザー向けに個別にビルドすることも、実行する Windows のバージョンに関係なくすべてのユーザー向けに同じバイナリを配布することもできます。

一部の Windows ランタイム API は、[パッケージ ID](modernize-packaged-apps.md) を持つデスクトップ アプリのみでサポートされています。 詳しくは、[利用可能な Windows ランタイム API](desktop-to-uwp-supported-api.md) に関する記事をご覧ください。

## <a name="set-up-your-project"></a>プロジェクトを設定する

Windows ランタイム API を使用するには、プロジェクトにいくつかの変更を加える必要があります。

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Windows ランタイム API を使用するように .NET プロジェクトを変更する

.NET プロジェクトには、次の 2 つのオプションがあります。

* アプリが Windows 10 バージョン1803 以降を対象としている場合は、必要なすべての参照が提供される NuGet パッケージをインストールできます。
* または、手動で参照を追加できます。

#### <a name="to-use-the-nuget-option"></a>NuGet オプションを使用するには

1. [パッケージ参照](/nuget/consume-packages/package-references-in-project-files)が有効になっていることを確認します。

    1. Visual Studio で、 **[ツール] -> [NuGet パッケージ マネージャー] -> [パッケージ マネージャー設定]** の順にクリックします。
    2. **[既定のパッケージ管理形式]** に **[PackageReference]** が選択されていることを確認します。

2. Visual Studio 上でプロジェクトを開いた状態で、**ソリューション エクスプローラー**でプロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

3. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** タブを選択して、`Microsoft.Windows.SDK.Contracts` を検索します。

4. `Microsoft.Windows.SDK.Contracts` パッケージが見つかったら、 **[NuGet パッケージ マネージャー]** ウィンドウの右側のペインで、ターゲットにする Windows 10 のバージョンに基づいて、インストールするパッケージの**バージョン**を選択します。

    * **10.0.18362.xxxx**:Windows 10 バージョン 1903 の場合は、これを選択します。
    * **10.0.17763.xxxx**:Windows 10 バージョン 1809 の場合は、これを選択します。
    * **10.0.17134.xxxx**:Windows 10 バージョン 1803 の場合は、これを選択します。

5. **[インストール]** をクリックします。

#### <a name="to-add-the-required-references-manually"></a>必要な参照を手動で追加するには

1. **[参照マネージャー]** ダイアログ ボックスを開き、 **[参照]** ボタンを選択して、 **[すべてのファイル]** を選択します。

    ![[参照の追加] ダイアログ ボックス](images/desktop-to-uwp/browse-references.png)

2. 次のすべてのファイルへの参照を追加します。

    |ファイル|インストール先|
    |--|--|
    |System.Runtime.WindowsRuntime.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime.dll|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*SDK バージョン*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*SDK バージョン*>\Windows.Foundation.UniversalApiContract\\<*バージョン*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*SDK バージョン*>\Windows.Foundation.FoundationContract\\<*バージョン*>|

3. **[プロパティ]** ウィンドウで、各 *.winmd* ファイルの **[ローカルにコピー]** フィールドを **[False]** に設定します。

    ![[ローカルにコピー] フィールド](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Windows ランタイム API を使用するように C++ Win32 プロジェクトを変更する

Windows ランタイム API を使用するには、[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) を使います。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。

C++/WinRT 用にプロジェクトを構成するには:

* 新しいプロジェクトの場合は、[C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) をインストールして、その拡張機能に含まれているいずれかの C++/WinRT プロジェクト テンプレートを使用できます。
* 既存のプロジェクトの場合は、[Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet パッケージをプロジェクトにインストールできます。

これらのオプションの詳細については、[こちらの記事](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を参照してください。

## <a name="add-windows-10-experiences"></a>Windows 10 エクスペリエンスの追加

これで、Windows 10 でアプリケーションをユーザーが実行する際に便利になる最新のエクスペリエンスを追加する準備ができました。 次の設計フローを使用します。

:white_check_mark:**最初に、追加するエクスペリエンスを決定する**

選択肢はたくさんあります。 たとえば、[収益化 API](/windows/uwp/monetize) を使用して発注フローを簡略化できます。また、別のユーザーが投稿した新しい写真など、共有すべき興味深いコンテンツがある場合に、[アプリケーションへの注目を促す](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)ことができます。

![トースト通知](images/desktop-to-uwp/toast.png)

ユーザーがメッセージを無視したり、非表示にした場合でも、ユーザーはアクション センターで再度メッセージを表示し、クリックすることで、アプリを開くことができます。 これにより、アプリケーションの利用度が高まります。さらに、アプリケーションがオペレーティング システムと密接に統合されているように見えるというメリットがあります。 この記事では、そのエクスペリエンスのコードについて後で少し紹介します。

詳細については、[UWP のドキュメント](/windows/uwp/get-started/)を参照してください。

:white_check_mark:**強化するのか、拡張するのかを決定する**

マイクロソフトでは "*強化*" と "*拡張*" という用語をよく使用しています。ここで、少し時間を使って、これらの各用語が厳密にはどのような意味なのかを説明しましょう。

マイクロソフトでは、(アプリケーションを MSIX パッケージにパッケージ化することを選択したかどうかにかかわらず、) デスクトップ アプリから直接呼び出すことができる Windows ランタイム API を表すために、"*強化*" という用語を使用します。 Windows 10 エクスペリエンスを選択した場合、その作成に必要な API を特定して、該当の API が[こちらの一覧](desktop-to-uwp-supported-api.md)に示されているかどうかを確認します。 これは、デスクトップ アプリから直接呼び出すことができる API の一覧です。 API がこの一覧に表示されていない場合、その API に関連付けられている機能が UWP プロセス内でしか実行できないことが理由です。 多くの場合、これらには、UWP マップ コントロールや Windows Hello のセキュリティの確認など、UWP XAML をレンダリングする API が含まれます。

> [!NOTE]
> UWP XAML をレンダリングする API は通常、デスクトップから直接呼び出すことはできませんが、別の方法を使用できる場合があります。 UWP XAML コントロールまたはその他のカスタム ビジュアル エクスペリエンスをホストする場合は、[XAML Islands](xaml-islands.md) (Windows 10 バージョン 1903 以降) と [ビジュアル レイヤー](visual-layer-in-desktop-apps.md) (Windows 10 バージョン 1803 以降) を使用できます。 これらの機能は、パッケージ化されているデスクトップ アプリでも、パッケージ化されていないデスクトップ アプリでも使用できます。

デスクトップ アプリを MSIX パッケージにパッケージ化することを選択した場合、別の方法として、UWP プロジェクトをソリューションに追加することで、アプリケーションを "*拡張*" することができます。 引き続きデスクトップ プロジェクトがアプリケーションのエントリ ポイントになりますが、UWP プロジェクトがあることで、[こちらの一覧](desktop-to-uwp-supported-api.md)に示されていないすべての API を利用できるようになります。 デスクトップ アプリでは、アプリ サービスを利用して UWP プロセスと通信できます。そのセットアップ方法については、多数のガイダンスが用意されています。 UWP プロジェクトを必要とするエクスペリエンスを追加するには、[UWP コンポーネントによる拡張](desktop-to-uwp-extend.md)に関する記事を参照してください。

:white_check_mark:**API コントラクトを参照する**

デスクトップ アプリから API を直接呼び出すことができる場合は、ブラウザーを開いて、その API のリファレンス トピックを検索します。
API の概要の下に、その API の API コントラクトを説明する表があります。 以下に表の例を示します。

![API コントラクト表](images/desktop-to-uwp/contract-table.png)

.NET ベースのデスクトップ アプリの場合、その API コントラクトへの参照を追加します。その後、そのファイルの **[ローカルにコピー]** プロパティを **[False]** に設定します。 C++ ベースのプロジェクトの場合、 **[追加のインクルード ディレクトリ]** に、このコントラクトを含むフォルダーのパスを追加します。

:white_check_mark:**エクスペリエンスを追加するための API を呼び出す**

以下に、前述の通知ウィンドウの表示に使用するコードを示します。 これらの API はこちらの[一覧](desktop-to-uwp-supported-api.md)に示されているので、このコードをデスクトップ アプリに追加してすぐに実行できます。

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

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
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

通知の詳細については、「[アダプティブ トースト通知と対話型トースト通知](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)」を参照してください。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Windows XP、Windows Vista、および Windows 7/8 インストール ベースのサポート

新しいブランチを作成して個別のコード ベースを保守しなくても、Windows 10 向けにアプリケーションを最新化できます。

Windows 10 ユーザー向けに個別のバイナリをビルドする場合は、条件付きコンパイルを使用します。 すべての Windows ユーザーに対して 1 組のバイナリをビルドして展開する場合は、ランタイム チェックを使用します。

各オプションを簡単に見ていきましょう。

### <a name="conditional-compilation"></a>条件付きコンパイル

1 つのコードベースを維持して、Windows 10 ユーザー向けだけに一連のバイナリをコンパイルします。

最初に、新しいビルド構成をプロジェクトに追加します。

![ビルド構成](images/desktop-to-uwp/build-config.png)

そのビルド構成に対して、Windows ランタイム API を呼び出すコードを識別する定数を作成します。  

.NET ベースのプロジェクトの場合、この定数は**条件付きコンパイル定数**と呼ばれます。

![条件付きコンパイル定数](images/desktop-to-uwp/compilation-constants.png)

C++ ベースのプロジェクトの場合、この定数は**プリプロセッサ定義**と呼ばれます。

![プリプロセッサ定義定数](images/desktop-to-uwp/pre-processor.png)

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

ユーザーが実行する Windows のバージョンに関係なく、1 組のバイナリをすべての Windows ユーザー向けにコンパイルできます。 アプリケーションでは、ユーザーが Windows 10 上でアプリケーションをパッケージ化されたアプリケーションとして実行している場合にのみ、Windows ランタイム API を呼び出します。

コードにランタイム チェックを追加する最も簡単な方法は、Nuget パッケージである [Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/) をインストールしてから、``IsRunningAsUWP()`` メソッドを使用して、Windows ランタイム API を呼び出すすべてのコードを利用することです。 詳細については、こちらのブログ投稿「[デスクトップ ブリッジ - アプリケーションのコンテキストの識別](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context)」を参照してください。

## <a name="related-samples"></a>関連するサンプル

* [Hello World サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [セカンダリ タイル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [ストア API サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [UWP UpdateTask を実装する WinForms アプリケーション](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [デスクトップ アプリから UWP へのブリッジのサンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>質問に対する回答を見つける

ご質問があるでしょうか。 Stack Overflow でお問い合わせください。 Microsoft のチームでは、これらの[タグ](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)をチェックしています。 [こちら](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)から質問することもできます。