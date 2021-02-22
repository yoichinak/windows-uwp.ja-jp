---
title: Windows UI ライブラリの概要
description: Windows UI ライブラリをインストールして使用する方法。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, ツールキット sdk
ms.openlocfilehash: 801c1f578c08df627264f542cbe1496d275afc0a
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334957"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Windows UI 2.x ライブラリの概要

[WinUI 2.5](release-notes/winui-2.5.md) は、WinUI の最新の安定したバージョンであり、運用環境のアプリで使用する必要があります。

ライブラリは、新規または既存の任意の Visual Studio プロジェクトに追加できる NuGet パッケージとして提供されています。

> [!NOTE]
> WinUI 3 の早期プレビューを試す方法の詳細については、「[Windows UI ライブラリ 3 Preview 4 (2021 年 2 月)](../winui3/index.md)」を参照してください。

## <a name="download-and-install-the-windows-ui-library"></a>Windows UI ライブラリのダウンロードとインストール

1. [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) をダウンロードし、Visual Studio インストーラーで **ユニバーサル Windows プラットフォーム開発** ワークロードをインストールします。

2. 既存のプロジェクトを開くか、[Visual C#] > [Windows] -> [ユニバーサル] にある [空のアプリ] テンプレート、または言語プロジェクションに適したテンプレートを使用して、新しいプロジェクトを作成します。  

    > [!IMPORTANT]
    > WinUI 2.5 を使用するには、プロジェクトのプロパティで TargetPlatformVersion >= 10.0.18362.0 と TargetPlatformMinVersion >= 10.0.15063.0 を設定する必要があります。

3. [ソリューション エクスプローラー] パネルで、プロジェクト名を右クリックし、 **[NuGet パッケージの管理]** を選択します。 

    :::image type="content" source="images/ManageNugetPackages.png" alt-text="[ソリューション エクスプローラー] パネルで、プロジェクトが右クリックされ、[NuGet パッケージの管理] オプションが強調表示されているスクリーンショット。":::<br/>*プロジェクトが右クリックされ、[NuGet パッケージの管理] オプションが強調表示されている [ソリューション エクスプローラー] パネル。*

4. **NuGet パッケージ マネージャー** で、 **[参照]** タブを選択し、「**Microsoft.UI.Xaml**」または「**WinUI**」を検索します。 使用する [Windows UI ライブラリの NuGet パッケージ](nuget-packages.md)を選択します (**Microsoft.UI.Xaml** パッケージにはすべてのアプリに適した Fluent コントロールと機能が含まれています)。 [インストール] をクリックします。 

    [プレリリースを含める] チェックボックスをオンにして、試験的な新機能が含まれている最新のプレリリース バージョンを確認することもできます。

    :::image type="content" source="images/NugetPackages.png" alt-text="検索フィールドに winui と入力され、[プレリリースを含める] がオンになっている状態の [参照] タブを示す [NuGet パッケージ マネージャー] ダイアログ ボックスのスクリーンショット。":::<br/>*検索フィールドに winui と入力され、[プレリリースを含める] がオンになっている状態の [参照] タブを示す [NuGet パッケージ マネージャー] ダイアログ ボックス。*

5. App.xaml ファイルに Windows UI (WinUI) テーマ リソースを追加します。

    これを行うには、他のアプリケーション リソースがあるかどうかに応じて 2 つの方法があります。

    a。 他のアプリケーション リソースが不要な場合は、次の例に示すように、WinUI リソース要素 `<XamlControlsResources` を追加します。

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>

    </Application>
    ```

    b. 複数のアプリケーション リソースが必要な場合は、次に示すように、`<ResourceDictionary.MergedDictionaries>` に WinUI リソース要素 `<XamlControlsResources` を追加します。

    ``` XAML
    <Application
        x:Class="ExampleApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        RequestedTheme="Light">

        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                    <ResourceDictionary Source="/Styles/Styles.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>

    </Application>
    ```

    > [!IMPORTANT]
    > ResourceDictionary にリソースが追加される順序は、それらが適用される順序に影響します。 `XamlControlsResources` ディクショナリによって多くの既定リソース キーがオーバーライドされるため、アプリ内の他のカスタム スタイルやリソースがオーバーライドされないように、まずこのディクショナリを `Application.Resources` に追加する必要があります。 リソースの読み込みについて詳しくは、「[ResourceDictionary と XAML リソースの参照](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」をご覧ください。

6. XAML ページと分離コード ページの両方に、WinUI パッケージへの参照を追加します。

    * XAML ページで、ページの上部に参照を追加します

        ```xaml
        xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
        ```

    * コード内で (型名を修飾せずに使用する場合)、using ディレクティブを追加できます。

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>C++/WinRT プロジェクトの追加手順

NuGet パッケージを C++/WinRT プロジェクトに追加すると、ツールによって、プロジェクトの `\Generated Files\winrt` フォルダーに一連のプロジェクション ヘッダーが生成されます。 これらのヘッダー ファイルをプロジェクトに取り込んで、これらの新しい型への参照が解決されるようにするには、プリコンパイル済みヘッダー ファイル (通常は `pch.h`) 内でそれらをインクルードできます。 次の例では、**Microsoft.UI.Xaml** パッケージ用に生成されたヘッダー ファイルをインクルードしています。

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

Windows UI ライブラリの単純なサポートを C++/WinRT プロジェクトに追加する詳細な手順を説明したチュートリアルについては、「[単純な C++/WinRT Windows UI ライブラリの例](/windows/uwp/cpp-and-winrt-apis/simple-winui-example)」を参照してください。

## <a name="contributing-to-the-windows-ui-library"></a>Windows UI ライブラリへの投稿

WinUI は、GitHub でホストされているオープン ソース プロジェクトです。

[Windows UI ライブラリ リポジトリ](https://aka.ms/winui)のバグ レポート、機能要求、およびコミュニティ コードの投稿を歓迎します。

## <a name="other-resources"></a>その他のリソース

UWP を初めて使用する場合は、開発者ポータルの「[UWP 開発の概要](https://developer.microsoft.com/windows/getstarted)」ページにアクセスすることをお勧めします。