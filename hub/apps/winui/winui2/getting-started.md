---
title: Windows UI ライブラリの概要
description: Windows UI ライブラリをインストールして使用する方法。
ms.topic: reference
ms.date: 07/15/2020
keywords: windows 10, uwp, ツールキット sdk
ms.openlocfilehash: 94c23ab9573df576af89d9211ced70938fd5105f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174136"
---
# <a name="getting-started-with-the-windows-ui-2x-library"></a>Windows UI 2.x ライブラリの概要

[WinUI 2.4](release-notes/winui-2.4.md) は、WinUI の最新の安定したバージョンであり、生産中のアプリで使用する必要があります。

ライブラリは、新規または既存の任意の Visual Studio プロジェクトに追加できる NuGet パッケージとして提供されています。

> [!NOTE]
> WinUI 3 の早期プレビューを試す方法の詳細については、「[Windows UI ライブラリ 3 Preview 2 (2020 年 7 月)](../winui3/index.md)」を参照してください。

## <a name="download-and-install-the-windows-ui-library"></a>Windows UI ライブラリのダウンロードとインストール

1. [Visual Studio 2019](https://developer.microsoft.com/windows/downloads) をダウンロードし、Visual Studio インストーラーで**ユニバーサル Windows プラットフォーム開発**ワークロードをインストールします。

2. 既存のプロジェクトを開くか、[Visual C#] > [Windows] -> [ユニバーサル] にある [空のアプリ] テンプレート、または言語プロジェクションに適したテンプレートを使用して、新しいプロジェクトを作成します。  

    > [!IMPORTANT]
    > WinUI 2.4 を使用するには、プロジェクトのプロパティで TargetPlatformVersion >= 10.0.18362.0 と TargetPlatformMinVersion >= 10.0.15063.0 を設定する必要があります。

3. [ソリューション エクスプローラー] パネルで、プロジェクト名を右クリックし、 **[NuGet パッケージの管理]** を選択します。 **[参照]** タブを選択し、**Microsoft.UI.Xaml** または **WinUI** を検索します。 次に、使用する [Windows UI ライブラリ NuGet パッケージ](nuget-packages.md)を選択します。
**Microsoft.UI.Xaml** パッケージには、すべてのアプリに適した Fluent コントロールと機能が含まれています。  
必要に応じて、[プレリリースを含める] をオンにして、試験的な新機能が含まれている最新のプレリリース バージョンを確認することもできます。

    ![NuGet パッケージ](images/ManageNugetPackages.png "NuGet パッケージのイメージの管理")

    ![NuGet パッケージ](images/NugetPackages.png)

4. App.xaml リソースに Windows UI (WinUI) テーマ リソースを追加します。 これを行うには、他のアプリケーション リソースがあるかどうかに応じて 2 つの方法があります。

    a。 他のアプリケーション リソースがない場合は、Application.Resources に `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` を追加します。

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. それ以外でアプリケーション リソースのセットが複数ある場合は、Application.Resources.MergedDictionaries に `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` を追加します。

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > ResourceDictionary にリソースが追加される順序は、それらが適用される順序に影響します。 `XamlControlsResources` ディクショナリによって多くの既定リソース キーがオーバーライドされるため、アプリ内の他のカスタム スタイルやリソースがオーバーライドされないように、まずこのディクショナリを `Application.Resources` に追加する必要があります。 リソースの読み込みについて詳しくは、「[ResourceDictionary と XAML リソースの参照](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」をご覧ください。

5. ツールキットへの参照を XAML ページと分離コード ページに追加します。

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