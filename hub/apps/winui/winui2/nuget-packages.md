---
title: Windows UI ライブラリの NuGet パッケージの一覧
description: Windows UI ライブラリの NuGet パッケージの一覧を示します
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, ツールキット sdk
ms.openlocfilehash: ca3f2915d77bb83f45744e90bd86e82edba013b8
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492887"
---
# <a name="windows-ui-library-nuget-packages"></a>Windows UI ライブラリ NuGet パッケージ

NuGet は、Visual Studio に組み込まれている .Net アプリケーション用の標準パッケージ マネージャーです。 開いているソリューションから、 *[ツール]* メニュー、 *[NuGet パッケージ マネージャー]* 、 *[ソリューションの NuGet パッケージの管理...]* を選択して UI を開きます。  次のいずれかのパッケージ名を入力して、オンラインで検索します。

| NuGet パッケージ名 | 説明 |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | UWP アプリのコントロール。 次の名前空間の API が含まれています。[Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml)、[Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers)、[Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls)、[Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives)、[Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes)、[Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media)、[Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | 以前のバージョンの Windows 10 で [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) API を使用でき、複数のターゲット Windows 10 バージョンを処理する特別なコードを記述する必要はありません。 |


## <a name="search-in-visual-studio"></a>Visual Studio 内での検索

Visual Studio パッケージ マネージャー内で検索すると、このような一覧が表示されます (バージョン番号は異なる可能性がありますが、名前は同じです)。

![NuGet パッケージ マネージャー](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>NuGet パッケージの更新

Windows UI ライブラリは、新しいコントロール、サービス、API で定期的に更新され、さらに重要なバグ修正も行われています。 最新バージョンであることを確認するには、Visual Studio 内でプロジェクトを開き、 **[ツール]** メニューの **[NuGet パッケージ マネージャー]**  ->  **[ソリューションの NuGet パッケージの管理...]** を選択し、 *[更新]* タブを選択します。更新するパッケージを選択し、[インストール] をクリックして最新バージョンに更新します。

## <a name="getting-started"></a>はじめに

独自のプロジェクトでこれらの NuGet パッケージを使用する方法について詳しくは、「[Windows UI ライブラリの概要](getting-started.md)」を参照してください。
