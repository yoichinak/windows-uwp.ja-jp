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
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580769"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>デスクトップ アプリ用の WinUI 3.0 の概要

WinUI 3.0 Preview 1 では、全面的に WinUI ベースのユーザー インターフェイスを使用する、マネージド デスクトップ C#/.NET およびネイティブ C++/Win32 デスクトップ アプリを作成できる新しいプロジェクト テンプレートが導入されています。 これらのプロジェクト テンプレートを使用してアプリを作成すると、アプリケーションのユーザー インターフェイス全体が、WinUI 3.0 で提供されるウィンドウ、コントロール、その他の種類の UI を使用して実装されます。 

WinUI 3.0 Preview 1 により、Visual Studio 2019 には次の **WinUI in Desktop** プロジェクト テンプレートが追加されます。

* .NET 5 を対象とする C# アプリとライブラリ:
  * [Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)
  * [Class Library (WinUI in Desktop)]\(クラス ライブラリ (WinUI in Desktop)\)

* C++/Win32 アプリ:
  * [Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)

このアプリ プロジェクト テンプレートにより、WinUI アプリ プロジェクトと、アプリを展開するために [MSIX パッケージ](https://docs.microsoft.com/windows/msix/overview)に組み込むよう構成されている [Windows アプリケーション パッケージ プロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)が生成されます。

## <a name="prerequisites"></a>前提条件

この記事で説明されているデスクトップ プロジェクト テンプレートに WinUI 3 を使用するには、次の手順に従って開発用コンピューターを構成します。

1. 開発用コンピューターに Windows 10 のバージョン 1803 (ビルド 17134) または最新バージョンがインストールされていることを確認します。 デスクトップ アプリ用の WinUI 3 では、1803 以降の OS バージョンが必要です。

2. Visual Studio 2019 バージョン 16.7 Preview 1 をインストールします。 詳細については、[こちらの手順](index.md#configure-your-dev-environment)を参照してください。

3. .NET 5 Preview 4 の x64 版と x86 版の両方をインストールします。
    * x64: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Visual Studio 2019 用の WinUI 3.0 Preview 1 プロジェクト テンプレートを含む VSIX 拡張機能をインストールします。 詳細については、[こちらの手順](index.md#visual-studio-project-templates)を参照してください。

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

    * ***<プロジェクト名>* (パッケージ)** : これは、アプリを展開するために MSIX パッケージに組み込むよう構成されている [Windows アプリケーション パッケージ プロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 このプロジェクトは、アプリの[パッケージ マニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![アプリ プロジェクト](images/WinUI-csharp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー**で **[ *<プロジェクト名>* (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 次の種類の項目から選択できます。

    * **空白のページ**
    * **空のウィンドウ**
    * **カスタム コントロール**
    * **リソース ディクショナリ**
    * **リソース ファイル**
    * **ユーザー コントロール**

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

    * ***<プロジェクト名>* (パッケージ)** : これは、アプリを展開するために MSIX パッケージに組み込むよう構成されている [Windows アプリケーション パッケージ プロジェクト](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 このプロジェクトは、アプリの[パッケージ マニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![パッケージ プロジェクト](images/WinUI-cpp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー**で **[ *<プロジェクト名>* (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 次の種類の項目から選択できます。

    * **空白のページ**
    * **空のウィンドウ**
    * **カスタム コントロール**
    * **リソース ディクショナリ**
    * **リソース ファイル**
    * **ユーザー コントロール**

    ![新しい項目](images/WinUI-cpp-newitem.png)

8. ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

## <a name="known-issues-and-limitations"></a>既知の問題と制限事項

Preview 1 の既知の問題と制限事項の一覧については、[こちらのセクション](index.md#preview-1-limitations-and-known-issues)を参照してください。

## <a name="related-topics"></a>関連トピック

* [WinUI 3.0](index.md)