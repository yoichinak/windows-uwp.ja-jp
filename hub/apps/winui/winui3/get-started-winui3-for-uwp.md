---
description: このガイドでは、WinUI 3 UI を使用して UWP アプリの作成を開始する方法について説明します。
title: UWP アプリ用の WinUI 3 の概要 (プレビュー)
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0f6f8a17a4f3d6ca684854a64de05da87b27c44c
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730536"
---
# <a name="get-started-with-winui-3-for-uwp-apps-preview"></a>UWP アプリ用の WinUI 3 の概要 (プレビュー)

[WinUI 3 - Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md) に付属するプロジェクト テンプレートを使用すると、完全に WinUI 3 上でビルドされたユーザー インターフェイスを備えたユニバーサル Windows プラットフォーム (UWP) アプリを作成できます。 これらのプロジェクト テンプレートを使用してアプリを作成すると、アプリケーションのユーザー インターフェイス全体が、WinUI 3 で提供されるウィンドウ、コントロール、スタイルを使用して実装されます。 サポートされる WinUI 3 プロジェクト テンプレートの完全な一覧については、「[WinUI 3 のプロジェクト テンプレート](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3)」を参照してください。

WinUI 3 は、Project Reunion パッケージに含まれます。 Project Reunion の詳細については、「[Project Reunion 0.5 を使用してデスクトップ Windows アプリをビルドする](../../project-reunion/index.md)」を参照してください。

> [!NOTE] 
> WinUI 3 での UWP アプリのビルドのサポートは、現在はプレビュー段階であり、運用環境ではサポートされていません。 WinUI 3 UWP アプリを Microsoft Store に公開することはできません。

## <a name="prerequisites"></a>前提条件

UWP プレビュー用の WinUI 3 プロジェクト テンプレートを使用するには、Project Reunion の「[開発環境を設定する](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)」のガイドに説明されている手順に従って開発用コンピューターを構成します。 

> [!NOTE]
> Project Reunion 0.5 VSIX を使用して WinUI 3 UWP アプリを作成することはできません。 UWP プレビュー プロジェクト テンプレートを取得して、WinUI 3 を使用して UWP アプリをビルドするには、[Project Reunion 0.5 **Preview** VSIX](https://aka.ms/projectreunion/previewdownload) をダウンロードする必要があります。 

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>"UWP の WinUI 3 アプリ" を C# で作成する

1. Visual Studio 2019 を使用して新しいプロジェクトを作成します。
   - Visual Studio が既に実行されている場合は、 **[ファイル]**  ->  **[新規]**  ->  **[プロジェクト]** を選択します。

       :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 - [ファイル] -> [新規] -> [プロジェクト] メニュー":::

   - それ以外の場合は、Visual Studio を起動し、 **[新しいプロジェクトの作成]** を選択します。

       :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 - [新しいプロジェクトの作成]":::

2. **[新しいプロジェクトの作成]** ダイアログのプロジェクトのドロップダウン フィルターで、 **[C#]** 、 **[Windows]** 、および **[WinUI]** をそれぞれ選択します。

3. プロジェクトの種類として、 **[空のアプリ (WinUI in UWP) - プレビュー]** を選択し、 **[次へ]** をクリックします。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 - [新しいプロジェクトの作成] ダイアログ":::

4. プロジェクト名を入力し、必要に応じてその他のオプションを選択して、 **[作成]** をクリックします。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="[場所] テキスト ボックスと [作成] オプションが強調表示された、[新しいプロジェクトの構成] ダイアログ ボックスのスクリーンショット。":::

5. 次のダイアログ ボックスで、 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1809 (ビルド 17763) に設定し、 **[OK]** をクリックします。

    :::image type="content" source="images/WinUI-min-target-version.png" alt-text="ターゲット バージョンと最小バージョンのダイアログ":::

6. Visual Studio によって、次のオブジェクトを含む **UWP の WinUI** プロジェクトが生成されます。

    - ***<プロジェクト名>* (ユニバーサル Windows)** :アプリケーション コードが含まれます。 これは、プロジェクト ソリューションの既定のスタートアップ プロジェクトです。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="ユニバーサル Windows ソリューションが強調表示された [ソリューション エクスプローラー] パネルのスクリーンショット。":::

    - **Package.appxmanifest**:システムがアプリをデプロイ、表示、または更新するために必要な情報が含まれます。 詳細については、「[アプリ パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest)」もご覧ください。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 - アプリ パッケージ マニフェスト":::

    - **App.xaml/App.xaml.cs**:アプリ インスタンスを表す `Application` クラスを定義するコード ファイル。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 - App.xaml ファイル":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 - App.xaml.cs ファイル":::

    - **MainPage.xaml/MainPage.xaml.cs**:アプリによって表示されるメイン ウィンドウを表すコード ファイル。 これらのクラスは、WinUI に用意されている **Microsoft.UI.Xaml** 名前空間の型から派生します。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 - MainPage.xaml ファイル":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 - MainPage.xaml.cs ファイル":::

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー** で **[ *<プロジェクト名>* (ユニバーサル Window)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 使用可能な項目の詳細については、「[WinUI 3 の項目テンプレート](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)」を参照してください。

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 - [新しい項目の追加] ダイアログ":::

8. アプリをビルド、デプロイ、起動して、どうなるかを確認します。

    1. アプリは、ローカル コンピューター、シミュレーターかエミュレーター、またはリモート デバイスでデバッグできます。 ドロップ ダウンからターゲット デバイスを選択します。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="[ローカル コンピューター] ドロップダウン リストのスクリーンショット。":::

    1. F5 キーを押して **[ビルド]** ボタンをクリックするか、 **[デバッグ] -> [デバッグ開始]** を選択して、ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="[Click Me] ボタンを表示している、実行中のアプリのスクリーンショット。":::

## <a name="known-issues-and-limitations"></a>既知の問題と制限事項

「[Windows UI ライブラリ 3 - Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)」の「[制限事項と既知の問題](index.md#limitations-and-known-issues)」のセクションを参照してください。

## <a name="related-topics"></a>関連トピック

- [Windows UI ライブラリ 3 - Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)
- [初めてのアプリの作成](/windows/uwp/get-started/your-first-app)
