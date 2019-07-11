---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: Contoso Expenses アプリの .NET Core 3 への移行
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420133"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>作業 1: Contoso Expenses アプリの .NET Core 3 への移行

これは、Contoso の経費をという名前のサンプル WPF デスクトップ アプリの近代化する方法を説明するチュートリアルの最初の部分です。 チュートリアル、前提条件、およびサンプル アプリをダウンロードする手順の概要については、次を参照してください。[チュートリアル。WPF アプリの近代化](modernize-wpf-tutorial.md)します。
  
このチュートリアルは、.NET Framework 4.7.2 から Contoso 経費アプリ全体を移行します[.NET Core 3](modernize-wpf-tutorial.md#net-core-3)します。 このチュートリアルのこの部分を開始する前に、以下のことを確認次の操作を行います。

* [開き、ContosoExpenses サンプルをビルド](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)Visual Studio 2019 でします。
* Visual Studio 2019 のリリース バージョンを使用している場合は、.NET Core SDK のプレビュー バージョンを有効にします。 Visual Studio に移動します。**ツール > オプション**、検索ボックスに「プレビュー」と入力し、選択 **、.NET Core SDK のプレビューを使用して、** します。 使用している場合、[プレビュー バージョンの Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)既定で .NET Core のプレビューが有効になっているため、このオプションを選択する必要はありません。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>.NET Core 3 ContosoExpenses プロジェクトに移行します。

このセクションでは、Contoso 経費アプリで ContosoExpenses プロジェクトを .NET Core 3 を移行します。 既存の ContosoExpenses プロジェクトが .NET Framework 4.7.2 ではなく .NET Core 3 のターゲットと同じファイルを含む新しいプロジェクト ファイルを作成して、これを行うします。 これにより、.NET Framework と .NET Core の両方のバージョンのアプリを 1 つのソリューションを維持することができます。

1. ContosoExpenses プロジェクトの現在、.NET Framework 4.7.2 を対象とすることを確認します。 ソリューション エクスプ ローラーで右クリックし、 **ContosoExpenses**プロジェクトで、選択**プロパティ**、ことを確認します、**ターゲット フレームワーク**プロパティを**アプリケーション** タブは、.NET Framework 4.7.2 に設定されます。

    ![.NET framework バージョン 4.7.2 プロジェクト](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows エクスプ ローラーに移動、 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**フォルダーという新しいテキスト ファイルを作成および**ContosoExpenses.Core.csproj**します。

4. ファイルを右クリックして選択**を開く**、し、メモ帳、Visual Studio Code または Visual Studio など、好みのテキスト エディターで開きます。

5. 次のテキストをファイルにコピーし、保存します。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. ファイルを閉じに戻り、 **ContosoExpenses** Visual Studio でソリューション。

7. 右クリックし、 **ContosoExpenses**ソリューション選択**追加]、[既存のプロジェクト**します。 選択、 **ContosoExpenses.Core.csproj**ファイルだけで作成した、`C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`ソリューションに追加するフォルダー。

**ContosoExpenses.Core.csproj**次の要素が含まれています。

* **プロジェクト**要素の SDK のバージョンを指定する**Microsoft.NET.Sdk.WindowsDesktop**します。 これは Windows desktop では、.NET アプリケーションを参照し、WPF と Windows フォーム アプリのコンポーネントが含まれています。
* **PropertyGroup**要素には、実行可能ファイル (DLL ではありません) は、.NET Core の 3 をターゲットし、WPF を使用して、プロジェクト出力を示す子要素が含まれています。 Windows フォーム アプリでは、使用すると、 **UseWinForms**要素の代わりに、 **UseWPF**要素。

> [!NOTE]
> .NET Core 3.0 で導入された .csproj 形式を使用する場合、.csproj と同じフォルダー内のすべてのファイルは、プロジェクトの一部と見なされます。 そのため、プロジェクトに含まれるすべてのファイルを指定する必要はありません。 カスタム ビルド アクションを定義するファイルのみが除外するかを指定する必要があります。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>ContosoExpenses.Data プロジェクトを .NET Standard に移行します。

**ContosoExpenses**ソリューションが含まれています、 **ContosoExpenses.Data**クラス ライブラリ モデルとのサービスのインターフェイスを含み、.NET 4.7.2 を対象とします。 .NET core 3.0 アプリは、.NET Core では使用できない Api を使用していない限り、.NET Framework のライブラリを使用できます。 ただし、最適な最新化パスは、.NET Standard ライブラリに移動するがします。 これにより、ライブラリが .NET Core 3.0 アプリから完全にサポートされていることを確認します。 さらに、(ASP.NET Core) 経由の web およびモバイル (Xamarin) 経由など、他のプラットフォームでも、ライブラリを再利用することができます。

移行する、 **ContosoExpenses.Data**を .NET Standard プロジェクト。

1. Visual Studio で、右クリックし、 **ContosoExpenses.Data**プロジェクト**プロジェクトのアンロード**します。 プロジェクトをもう一度右クリックし、**編集 ContosoExpenses.Data.csproj**します。

2. プロジェクト ファイルの内容全体を削除します。

3. コピーし、次の XML を貼り付け、ファイルを保存します。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. 右クリックし、 **ContosoExpenses.Data**プロジェクト**プロジェクトの再読み込み**します。

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet パッケージと依存関係を構成します。

移行したときに、 **ContosoExpenses.Core**と**ContosoExpenses.Data**プロジェクト前のセクションで、プロジェクトから NuGet パッケージの参照を削除します。 このセクションでは、戻る、これらの参照を追加します。

NuGet パッケージの構成を**ContosoExpenses.Data**プロジェクト。

1.  **ContosoExpenses.Data**プロジェクトで、展開、**依存関係**ノード。 なお、 **NuGet**セクションがありません。

    ![NuGet パッケージ](images/wpf-modernize-tutorial/NuGetPackages.png)

    開く場合、 **Packages.config**で、**ソリューション エクスプ ローラー** 'old' NuGet パッケージの参照が完全な .NET Framework を使用していたときに、プロジェクトを使用と表示されます。

    ![依存関係とパッケージ](images/wpf-modernize-tutorial/Packages.png)

    内容を次に示します、 **Packages.config**ファイル。 すべての NuGet パッケージが完全な .NET Framework 4.7.2 を対象に表示されます。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. **ContosoExpenses.Data**プロジェクトで、削除、 **Packages.config**ファイル。

4. **ContosoExpenses.Data**プロジェクトを右クリックして、**依存関係**ノード選択**NuGet パッケージの管理**します。

  ![NuGet パッケージの管理.](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 検索、`Bogus`パッケージを探してインストールします。

    ![//偽の NuGet パッケージ](images/wpf-modernize-tutorial/Bogus.png)

6. 検索、`LiteDB`パッケージを探してインストールします。

    ![LiteDB NuGet パッケージ](images/wpf-modernize-tutorial/LiteDB.png)

    疑問 NuGet パッケージのこれらの一覧が格納されている場所プロジェクト packages.config ファイルがある不要になったためです。 参照される NuGet パッケージは、.csproj ファイルに直接格納されます。 これを確認するにはの内容を表示することによって、 **ContosoExpenses.Data.csproj**テキスト エディターでプロジェクト ファイル。 ファイルの末尾に追加された次の行が表示されます。

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > .NET Framework 4.7.2 のプロジェクトで使用されるものとして、この .NET Core 3 プロジェクトに同じパッケージをインストールしている場合もあります。 NuGet パッケージは、複数バージョン対応をサポートします。 ライブラリの作成者は、さまざまなアーキテクチャとプラットフォーム用にコンパイルされる、同じパッケージで別のバージョンのライブラリを含めることができます。 これらのパッケージは .NET Core 3 プロジェクトと互換性がある完全な .NET Framework と .NET Standard 2.0 をサポートします。 .NET Framework、.NET Core および .NET Standard の違いについての詳細については、次を参照してください。 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)します。

NuGet パッケージの構成を**ContosoExpenses.Core**プロジェクト。

1. **ContosoExpenses.Core**プロジェクトを開き、 **packages.config**ファイル。 現在、.NET Framework 4.7.2 を対象とする次の参照を含むことに注意してください。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    .NET Standard のバージョンが、次の手順で、`MvvmLightLibs`と`Unity`パッケージ。 他の 2 つには、これら 2 つのライブラリをインストールするときに、NuGet で自動的にダウンロードされる依存があります。

2. **ContosoExpenses.Core**プロジェクトで、削除、 **Packages.config**ファイル。

3. 右クリックし、 **ContosoExpenses.Core**プロジェクト**NuGet パッケージの管理**します。

4. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 検索、`Unity`パッケージを探してインストールします。

    ![Unity パッケージ](images/wpf-modernize-tutorial/UnityPackage.png)

5. 検索、`MvvmLightLibsStd10`パッケージを探してインストールします。 これは、.NET Standard のバージョン、`MvvmLightLibs`パッケージ。 このパッケージは、作成者は、.NET Standard のバージョンの .NET Framework のバージョンよりも別のパッケージ ライブラリをパッケージ化する選択しました。

    !MvvmLightsLibs パッケージ[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. **ContosoExpenses.Core**プロジェクトを右クリックして、**依存関係**ノード選択**参照の追加**します。

7. **プロジェクト > ソリューション**カテゴリで、 **ContosoExpenses.Data**  をクリック**OK**します。

    ![リファレンスの追加](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>自動生成されたアセンブリの属性を無効にします。

この時点で、移行プロセスを構築しようとする場合、 **ContosoExpenses.Core**プロジェクトの一部のエラーが表示されます。

![.NET core 3 ビルドの新しいエラー](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

この問題が発生しているため、プロジェクト ファイル内のアセンブリ情報ストアの .NET Core 3.0 で導入された新しい .csproj 形式ではなく、 **AssemblyInfo.cs**ファイル。 これらのエラーを修正するには、この動作を無効にして、プロジェクトを引き続き使用できるように、 **AssemblyInfo.cs**ファイル。

1. Visual Studio で、右クリックし、 **ContosoExpenses.Core**プロジェクト**プロジェクトのアンロード**します。 プロジェクトをもう一度右クリックし、**編集 ContosoExpenses.Core.csproj**します。

1. 次の要素を追加、 **PropertyGroup**セクションし、ファイルを保存します。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加した後、 **PropertyGroup**セクションが次のようになります。

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. 右クリックし、 **ContosoExpenses.Core**プロジェクト**プロジェクトの再読み込み**します。

4. 右クリックし、 **ContosoExpenses.Data**プロジェクト**プロジェクトのアンロード**します。 プロジェクトをもう一度右クリックし、**編集 ContosoExpenses.Data.csproj**します。

5. 内の同じエントリを追加、 **PropertyGroup**セクションし、ファイルを保存します。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加した後、 **PropertyGroup**セクションが次のようになります。

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. 右クリックし、 **ContosoExpenses.Data**プロジェクト**プロジェクトの再読み込み**します。

## <a name="add-the-windows-compatibility-pack"></a>Windows 互換機能パックを追加します。

コンパイルすると場合、 **ContosoExpenses.Core**と**ContosoExpenses.Data**プロジェクトでは、以前のエラーが解決されましたが、まだいくつかのエラーがあるが表示されます、 **ContosoExpenses.Data**ライブラリの次のようにします。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

これらのエラーに変換した結果、 **ContosoExpenses.Data** Linux、Android、iOS など、複数のプラットフォームで実行できる、.NET Standard ライブラリに (これは Windows の特定) .NET Framework ライブラリからのプロジェクトその他 **ContosoExpenses.Data**というクラスがプロジェクトに含まれている**RegistryService**レジストリ、Windows 専用の概念と対話します。

これらのエラーを解決するには、インストール、 [Windows 互換性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet パッケージ。 このパッケージは、.NET Standard ライブラリで使用される多くの Windows 固有の Api のサポートを提供します。 ライブラリしなくなるクロスプラット フォーム対応が、このパッケージを使用して、まだ .NET Standard をターゲットにした後。 

1. 右クリックし、 **ContosoExpenses.Data**プロジェクト。
2. 選択**NuGet パッケージの管理**します。
3. **NuGet パッケージ マネージャー**ウィンドウで、をクリックして**参照**します。 検索、`Microsoft.Windows.Compatibility`パッケージを探してインストールします。 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 右クリックして、プロジェクトのコンパイルを再び、 **ContosoExpenses.Data**プロジェクトと選択**ビルド**します。

今度は、ビルド プロセスはエラーなく完了します。

## <a name="test-and-debug-the-migration"></a>テストおよびデバッグの移行

これで、プロジェクトは正常にビルドを実行して、ランタイム エラーがあるかどうか、アプリをテストする準備が整いました。

1. 右クリックし、 **ContosoExpenses.Core**プロジェクト**スタートアップ プロジェクトとして設定**します。

2. 開始 f5 キーを押して、 **ContosoExpenses.Core**デバッガーでのプロジェクト。 次のような例外が表示されます。

    ![Visual Studio に表示される例外](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    に関する情報を削除した移行の先頭に .csproj ファイルからコンテンツを削除したときにあるために、この例外は生成されている、**ビルド アクション**イメージ ファイル。 次の手順では、この問題を解決します。

3. デバッガーを停止します。

4. 右クリックし、 **ContosoExpenses.Core**プロジェクト**プロジェクトのアンロード**します。 プロジェクトをもう一度右クリックし、**編集 ContosoExpenses.Core.csproj**します。

5. 閉じる前に**プロジェクト**要素、次のエントリを追加します。

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. 右クリックし、 **ContosoExpenses.Core**プロジェクト**プロジェクトの再読み込み**します。

7. アプリに割り当てる、Contoso.ico には、右クリックし、 **ContosoExpenses.Core**プロジェクト**プロパティ**します。 開かれた ページで、下のドロップダウンをクリックします。**アイコン**選択`Images\contoso.ico`します。

    ![Contoso プロジェクトのプロパティ アイコン](images/wpf-modernize-tutorial/ContosoIco.png)

8. **[保存]** をクリックします。

9. 開始 f5 キーを押して、 **ContosoExpenses.Core**デバッガーでのプロジェクト。 アプリを今すぐ実行することを確認します。

## <a name="next-steps"></a>次のステップ

チュートリアルのこの時点で、Contoso Expenses アプリは .NET Core 3 に正常に移行されました、これで、[第 2 部:XAML Islands を使って UWP InkCanvas コントロールを追加](modernize-wpf-tutorial-2.md) の準備が整いました。

> [!NOTE]
> 高解像度画面であれば、アプリが非常に小さいことに注意してください可能性があります。 このチュートリアルの次の手順では、この問題を取り上げます。
