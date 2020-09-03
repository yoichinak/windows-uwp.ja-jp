---
description: このガイドでは、WinUI 3 UI を使用して .NET および C++/Win32 デスクトップ アプリの作成を開始する方法について説明します。
title: デスクトップ アプリ用の WinUI 3 の概要
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 2b946047602013704b27fb5c5565155d38dbb7f8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154826"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>デスクトップ アプリ用の WinUI 3 の概要

WinUI 3 Preview 2 では、全面的に WinUI ベースのユーザー インターフェイスを使用する、マネージド デスクトップ C#/.NET Core およびネイティブ C++/Win32 デスクトップ アプリを作成できる新しいプロジェクト テンプレートが導入されています。 これらのプロジェクト テンプレートを使用してアプリを作成すると、アプリケーションのユーザー インターフェイス全体が、WinUI 3 で提供されるウィンドウ、コントロール、その他の種類の UI を使用して実装されます。 プロジェクト テンプレートの完全な一覧については、[こちらのセクション](index.md#project-templates-for-winui-3)を参照してください。

## <a name="prerequisites"></a>前提条件

この記事で説明されているデスクトップ プロジェクト テンプレート用の WinUI 3 を使用するには、[こちら](index.md#install-winui-3-preview-2)の手順に従って開発用コンピューターを構成し、WinUI 3 Preview 2 をインストールします。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>C# および .NET 5 用の WinUI 3 デスクトップ アプリを作成する

1. Visual Studio 2019 で、 **[ファイル]**  ->  **[新規作成]**  ->  **[プロジェクト]** の順に選択します。

2. プロジェクトのドロップダウン フィルターで、 **[C#]** 、 **[Windows]** 、および **[WinUI]** をそれぞれ選択します。

3. プロジェクトの種類として **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)** を選択し、 **[次へ]** をクリックします。

    ![空のアプリ プロジェクト テンプレート](images/WinUI-csharp-newproject.png)

4. プロジェクト名を入力し、必要に応じてその他のオプションを選択して、 **[作成]** をクリックします。

5. 次のダイアログ ボックスで、 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定し、 **[OK]** をクリックします。

    ![ターゲットおよび最小バージョン](images/WinUI-min-target-version.png)

6. この時点で、Visual Studio では次の 2 つのプロジェクトが生成されます。

    * ***<プロジェクト名>* (デスクトップ)** : このプロジェクトには、アプリのコードが格納されています。 **App.xaml.cs** コード ファイルでは、アプリ インスタンスを表す `Application` クラスを定義します。また、**MainWindow.xaml.cs** コード ファイルでは、アプリによって表示されるメイン ウィンドウを表す `MainWindow` クラスを定義します。 これらのクラスは、WinUI に用意されている **Microsoft.UI.Xaml** 名前空間の型から派生します。

        ![アプリ プロジェクト](images/WinUI-csharp-appproject.png)

    * ***<プロジェクト名>* (パッケージ)** : これは、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むように構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 このプロジェクトは、アプリの[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![アプリ プロジェクト](images/WinUI-csharp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー**で **[ *<プロジェクト名>* (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 使用可能な項目の詳細については、[こちらのセクション](index.md#item-templates-for-winui-3)を参照してください。

    ![新しい項目](images/WinUI-csharp-newitem.png)

8. ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>C++/Win32 用の WinUI 3 デスクトップ アプリを作成する

1. Visual Studio 2019 で、 **[ファイル]**  ->  **[新規作成]**  ->  **[プロジェクト]** の順に選択します。

2. プロジェクトのドロップダウン フィルターで、 **[C++]** 、 **[Windows]** 、および **[WinUI]** を選択します。

3. プロジェクトの種類として **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)** を選択し、 **[次へ]** をクリックします。

    ![空のアプリ プロジェクト テンプレート](images/WinUI-cpp-newproject.png)

4. プロジェクト名を入力し、必要に応じてその他のオプションを選択して、 **[作成]** をクリックします。

5. 次のダイアログ ボックスで、 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定し、 **[OK]** をクリックします。

    ![ターゲットおよび最小バージョン](images/WinUI-min-target-version.png)

6. この時点で、Visual Studio では次の 2 つのプロジェクトが生成されます。

    * ***<プロジェクト名>* (デスクトップ)** : このプロジェクトには、アプリのコードが格納されています。 **App.xaml** と各種 **App** コード ファイルでは、アプリ インスタンスを表す `Application` クラスを定義します。また、**MainWindow.xaml** と各種 **MainWindow** コード ファイルでは、アプリによって表示されるメイン ウィンドウを表す `MainWindow` クラスを定義します。 これらのクラスは、WinUI に用意されている **Microsoft.UI.Xaml** 名前空間の型から派生します。

        ![アプリ プロジェクト](images/WinUI-cpp-appproject.png)

    * ***<プロジェクト名>* (パッケージ)** : これは、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むように構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 このプロジェクトは、アプリの[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![パッケージ プロジェクト](images/WinUI-cpp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー**で **[ *<プロジェクト名>* (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 使用可能な項目の詳細については、[こちらのセクション](index.md#item-templates-for-winui-3)を参照してください。

    ![新しい項目](images/WinUI-cpp-newitem.png)

8. ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

## <a name="known-issues-and-limitations"></a>既知の問題と制限事項

既知の問題と制限事項の一覧については、[こちらのセクション](index.md#preview-2-limitations-and-known-issues)を参照してください。

## <a name="related-topics"></a>関連トピック

* [Windows UI ライブラリ 3](index.md)