---
description: このトピックでは、C++/WinRT プロジェクト内に WinUI の基本サポートを追加する処理を順を追って説明します。
title: 単純な C++/WinRT Windows UI ライブラリの例
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, Windows UI ライブラリ, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: b7323ba63bedfc1ed560effb4da8ea61d1527897
ms.sourcegitcommit: 71701f5ffc540951f86d6f77a52416c6d75fe305
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100632683"
---
# <a name="a-basic-cwinrt-windows-ui-library-example"></a>基本的な C++/WinRT Windows UI ライブラリの例

このトピックでは、C++/WinRT プロジェクトに [Windows UI (WinUI) ライブラリ](https://github.com/Microsoft/microsoft-ui-xaml)の基本サポートを追加する処理を順を追って説明します。 ちなみに、Windows UI ライブラリ自体は C++/WinRT で記述されています。

> [!NOTE]
> Windows UI (WinUI) ライブラリ ツールキットは NuGet パッケージとして提供されており、このトピックで説明するように、Visual Studio を使用して既存または新規のプロジェクトに追加できます。 背景、セットアップ、およびサポート情報の詳細については、「[Windows UI ライブラリの概要](/uwp/toolkits/winui/getting-started)」を参照してください。

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>空のアプリ (HelloWinUICppWinRT) を作成する

Visual Studio で **[空のアプリ (C++WinRT)]** プロジェクト テンプレートを使用して新しいプロジェクトを作成します。 **(ユニバーサル Windows)** テンプレートではなく、 **(C++/WinRT)** テンプレートを使用していることをご確認ください。

新しいプロジェクトの名前を *HelloWinUICppWinRT* に設定し、(フォルダー構造がチュートリアルと一致するように) **[ソリューションとプロジェクトを同じディレクトリに配置する]** チェック ボックスをオフにします。

## <a name="install-the-microsoftuixaml-nuget-package"></a>Microsoft.UI.Xaml NuGet パッケージをインストールする

**[プロジェクト]** \> **[NuGet パッケージの管理...]** \> **[参照]** の順にクリックし、検索ボックスに「**Microsoft.UI.Xaml**」と入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックして自分のプロジェクトにパッケージをインストールします (使用許諾契約のメッセージも表示されます)。 **Microsoft.UI.Xaml** パッケージのみをインストールし、**Microsoft.UI.Xaml.Core.Direct** をインストールしないように注意してください。

## <a name="declare-winui-application-resources"></a>WinUI アプリケーション リソースを宣言する

`App.xaml` を開き、既存の **Application** の開始タグと終了タグの間に次のマークアップを貼り付けます。

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>MainPage に WinUI コントロールを追加する

次に、`MainPage.xaml` を開きます。 既存の **Page** の開始タグには、いくつかの xml 名前空間宣言があります。 xml 名前空間宣言 `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"` を追加します。 次に、既存の **Pages** の開始タグと終了タグの間に次のマークアップを貼り付けて、既存の **StackPanel** 要素を上書きします。

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-pchh-as-necessary"></a>必要に応じて pch.h を編集する

NuGet パッケージを C++/WinRT プロジェクト (先ほど追加した **Microsoft.UI.Xaml** パッケージなど) に追加して、プロジェクトをビルドすると、ツールによって、プロジェクトの `\Generated Files\winrt` フォルダーに一連のプロジェクション ヘッダー ファイルが生成されます。 チュートリアルに従うと、`\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt` フォルダーが作成されます。 これらのヘッダー ファイルをプロジェクトに取り込んで、これらの新しい型への参照が解決されるようにするには、プリコンパイル済みヘッダー ファイル (通常は `pch.h`) 内でそれらをインクルードできます。

使用する型に対応するヘッダーのみを含める必要があります。 ただし、次の例は、**Microsoft.UI.Xaml** パッケージ用に生成されたすべてのヘッダー ファイルをインクルードしています。

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>MainPage.cpp を編集する

`MainPage.cpp` で、**MainPage::ClickHandler** の実装内のコードを削除します。これは、XAML マークアップに *myButton* がなくなったためです。

これでプロジェクトをビルドして実行できるようになりました。

![単純な C++/WinRT Windows UI ライブラリのスクリーンショット](images/winui.png)

## <a name="related-topics"></a>関連トピック
* [Windows UI ライブラリの概要](/uwp/toolkits/winui/getting-started)