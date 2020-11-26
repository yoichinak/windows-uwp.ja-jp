---
description: このガイドでは、WinUI 3 UI を使用して .NET および C++/Win32 デスクトップ アプリの作成を開始する方法について説明します。
title: デスクトップ アプリ用の WinUI 3 の概要
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 067e6a6798fbfc2633c3e356be64ecae0403cc6d
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025158"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>デスクトップ アプリ用の WinUI 3 の概要

WinUI 3 Preview 3 には、全面的に WinUI ベースのユーザー インターフェイスを使用する、マネージド デスクトップ C#/.NET Core およびネイティブ C++/Win32 デスクトップ アプリを作成できるプロジェクト テンプレートが用意されています。 これらのプロジェクト テンプレートを使用してアプリを作成すると、アプリケーションのユーザー インターフェイス全体が、WinUI 3 で提供されるウィンドウ、コントロール、その他の種類の UI を使用して実装されます。 プロジェクト テンプレートの完全な一覧については、[こちらのセクション](index.md#project-templates-for-winui-3)を参照してください。

## <a name="prerequisites"></a>前提条件

この記事で説明されているデスクトップ プロジェクト テンプレート用の WinUI 3 を使用するには、[こちら](index.md#install-winui-3-preview-3)の手順に従って開発用コンピューターを構成し、WinUI 3 Preview 3 をインストールします。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>C# および .NET 5 用の WinUI 3 デスクトップ アプリを作成する

1. Visual Studio 2019 で、 **[ファイル]**  ->  **[新規作成]**  ->  **[プロジェクト]** の順に選択します。

2. プロジェクトのドロップダウン フィルターで、 **[C#]** 、 **[Windows]** 、および **[WinUI]** をそれぞれ選択します。

3. プロジェクトの種類として **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)** を選択し、 **[次へ]** をクリックします。

    ![新しいプロジェクトの作成ウィザードで空のアプリ パッケージ (デスクトップ内の Win U I) オプションが強調表示されているスクリーンショット。](images/WinUI-csharp-newproject.png)

4. プロジェクト名を入力し、必要に応じてその他のオプションを選択して、 **[作成]** をクリックします。

5. 次のダイアログ ボックスで、 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定し、 **[OK]** をクリックします。

    ![ターゲットおよび最小バージョン](images/WinUI-min-target-version.png)

6. この時点で、Visual Studio では次の 2 つのプロジェクトが生成されます。

    * **_<プロジェクト名>_ (デスクトップ)** : このプロジェクトには、アプリのコードが格納されています。 **App.xaml** ファイルと **App.xaml.cs** 分離コード ファイルでは、アプリ インスタンスを表す `Application` クラスが定義されます。 **MainWindow.xaml** ファイルと **MainWindow.xaml.cs** 分離コード ファイルでは、アプリによって表示されるメイン ウィンドウを表す `MainWindow` クラスが定義されます。 これらのクラスは、WinUI に用意されている **Microsoft.UI.Xaml** 名前空間の型から派生します。

        ![Visual Studio で、ソリューション エクスプローラー ペインと、メインの Windows X A M L ドット C S ファイルの内容を表示しているスクリーンショット。](images/WinUI-csharp-appproject.png)

    * **_<プロジェクト名>_ (パッケージ)** : これは、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むように構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 このプロジェクトは、アプリの[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![Visual Studio で、ソリューション エクスプローラー ペインと、パッケージ アプリ x マニフェスト ファイルの内容を表示しているスクリーンショット。](images/WinUI-csharp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー** で **[ _<プロジェクト名>_ (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 使用可能な項目の詳細については、[こちらのセクション](index.md#item-templates-for-winui-3)を参照してください。

    ![[新しい項目の追加] ダイアログ ボックスで、[インストール済み] > [Visual C シャープの項目] > [Win U I] を選択し、[空白のページ] オプションが強調表示されているスクリーンショット。](images/WinUI-csharp-newitem.png)

8. ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>C++/Win32 用の WinUI 3 デスクトップ アプリを作成する

1. Visual Studio 2019 で、 **[ファイル]**  ->  **[新規作成]**  ->  **[プロジェクト]** の順に選択します。

2. プロジェクトのドロップダウン フィルターで、 **[C++]** 、 **[Windows]** 、および **[WinUI]** を選択します。

3. プロジェクトの種類として **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\)** を選択し、 **[次へ]** をクリックします。

    ![新しいプロジェクトの作成ウィザードで空のアプリ パッケージ (デスクトップ内の Win U I) オプションが強調表示されている別のスクリーンショット。](images/WinUI-cpp-newproject.png)

4. プロジェクト名を入力し、必要に応じてその他のオプションを選択して、 **[作成]** をクリックします。

5. 次のダイアログ ボックスで、 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定し、 **[OK]** をクリックします。

    ![ターゲットおよび最小バージョン](images/WinUI-min-target-version.png)

6. この時点で、Visual Studio では次の 2 つのプロジェクトが生成されます。

    * **_<プロジェクト名>_ (デスクトップ)** : このプロジェクトには、アプリのコードが格納されています。 **App.xaml** と各種 **App** コード ファイルでは、アプリ インスタンスを表す `Application` クラスを定義します。また、**MainWindow.xaml** と各種 **MainWindow** コード ファイルでは、アプリによって表示されるメイン ウィンドウを表す `MainWindow` クラスを定義します。 これらのクラスは、WinUI に用意されている **Microsoft.UI.Xaml** 名前空間の型から派生します。

        ![Visual Studio で、ソリューション エクスプローラー ペインと、メインの Windows X A M L ファイルの内容を表示しているスクリーンショット。](images/WinUI-cpp-appproject.png)

    * **_<プロジェクト名>_ (パッケージ)** : これは、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むように構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)です。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。 このプロジェクトは、アプリの[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を格納しており、既定では、ソリューションのスタートアップ プロジェクトになります。

        ![Visual Studio で、ソリューション エクスプローラー ペインと、パッケージ アプリ x マニフェスト ファイルの内容を表示している別のスクリーンショット。](images/WinUI-cpp-packageproject.png)

7. アプリ プロジェクトに新しい項目を追加するには、**ソリューション エクスプローラー** で **[ _<プロジェクト名>_ (デスクトップ)]** プロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログ ボックスで、 **[WinUI]** タブを選択し、追加する項目を選択して、 **[追加]** をクリックします。 使用可能な項目の詳細については、[こちらのセクション](index.md#item-templates-for-winui-3)を参照してください。

    ![新しい項目](images/WinUI-cpp-newitem.png)

8. ソリューションをビルドして実行し、アプリがエラーなしで実行されることを確認します。

   > [!NOTE]
   > パッケージ化されたプロジェクトのみが起動されるため、スタートアップ プロジェクトとして設定されていることを確認してください。

## <a name="localizing-your-winui-desktop-app"></a>WinUI デスクトップ アプリのローカライズ

WinUI デスクトップ アプリで複数の言語をサポートし、パッケージ化されたプロジェクトの適切なローカライズを確認するには、適切なリソースをプロジェクトに追加し (「[アプリ リソースとリソース管理システム](/windows/uwp/app-resources/)」を参照)、プロジェクトの `package.appxmanifest` ファイル内でサポートされている各言語を宣言します。 プロジェクトをビルドすると、指定した言語が生成されたアプリ マニフェスト (`AppxManifest.xml`) に追加され、対応するリソースが使用されます。

1. テキスト エディターで .wapproj の `package.appxmanifest` を開き、次のセクションを探します。

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. `<Resource Language="x-generate">` を、サポートされている各言語の `<Resource />` 要素に置き換えます。 たとえば、次のマークアップにより、"en-US" および "es-ES" のローカライズされたリソースが使用可能であることが指定されます。

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```

## <a name="known-issues-and-limitations"></a>既知の問題と制限事項

既知の問題と制限事項の一覧については、[こちらのセクション](index.md#preview-3-limitations-and-known-issues)を参照してください。

## <a name="related-topics"></a>関連トピック

* [Windows UI ライブラリ 3](index.md)