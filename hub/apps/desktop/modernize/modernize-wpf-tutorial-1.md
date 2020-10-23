---
description: このチュートリアルでは、Contoso Expenses アプリ全体を .NET Framework 4.7.2 から .NET Core 3 に移行する方法について説明します。
title: Contoso Expenses　アプリの .NET Core 3 への移行
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: cab839e4f9e8c60f2f3f7c1b043f8a3af77bd4e7
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133085"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>パート 1: Contoso Expenses　アプリの .NET Core 3 への移行

これは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを最新化する方法を示すチュートリアルの最初の部分です。 チュートリアルの概要、前提条件、サンプル アプリをダウンロードするための手順については、「[チュートリアル:WPF アプリの最新化](modernize-wpf-tutorial.md)」をご覧ください。
  
チュートリアルのこの部分では、.NET Framework 4.7.2 から [.NET Core 3](modernize-wpf-tutorial.md#net-core-3) に Contoso Expenses アプリ全体を移行します。 チュートリアルのこの部分を開始する前に、Visual Studio 2019 で [ContosoExpenses サンプルを開いてビルド](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)してください。

> [!NOTE]
> WPF アプリケーションを .NET Framework から .NET Core 3 に移行する方法について詳しくは、[こちらのブログ シリーズ](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)をご覧ください。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>ContosoExpenses プロジェクトを .NET Core 3 に移行する

このセクションでは、Contoso Expenses アプリの ContosoExpenses プロジェクトを .NET Core 3 に移行します。 これを行うには、既存の ContosoExpenses プロジェクトと同じファイルを含む新しいプロジェクト ファイルを作成しますが、.NET Framework 4.7.2 の代わりに .NET Core 3 を対象とします。 これにより、.NET Framework と .NET Core の両方のバージョンのアプリで 1 つのソリューションを管理できます。

1. ContosoExpenses プロジェクトが現在 .NET Framework 4.7.2 を対象としていることを確認します。 ソリューション エクスプローラーで、**ContosoExpenses** プロジェクトを右クリックし、 **[プロパティ]** を選択して、 **[アプリケーション]** タブの **[ターゲット フレームワーク]** プロパティが .NET Framework 4.7.2 に設定されていることを確認します。

    ![プロジェクトの .NET Framework バージョン 4.7.2](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows エクスプローラーで、**C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** フォルダーに移動し、**ContosoExpenses.Core.csproj** という名前の新しいテキスト ファイルを作成します。

4. ファイルを右クリックし、 **[プログラムから開く]** を選択して、メモ帳、Visual Studio Code、Visual Studio などの任意のテキスト エディターで開きます。

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

6. ファイルを閉じて、Visual Studio の **[ContosoExpenses]** ソリューションに戻ります。

7. **ContosoExpenses** ソリューションを右クリックし、 **[追加] -> [新しいプロジェクト]** を選択します。 `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` フォルダーに作成した **ContosoExpenses.Core.csproj** ファイルを選択して、ソリューションに追加します。

**ContosoExpenses.Core.csproj** には、次の要素が含まれています。

* **Project** 要素では、**Microsoft.NET.Sdk.WindowsDesktop** の SDK バージョンを指定します。 これは、Windows デスクトップ用の .NET アプリケーションを指し、WPF と Windows フォーム アプリのコンポーネントを含みます。
* **PropertyGroup** 要素には、プロジェクト出力が (DLL ではなく) 実行可能ファイルであり、.NET Core 3 を対象とし、WPF を使用することを示す子要素が含まれています。 Windows フォーム アプリの場合は、**UseWPF** 要素の代わりに、**UseWinForms** 要素を使用します。

> [!NOTE]
> .NET Core 3.0 で導入された .csproj 形式を使用すると、.csproj と同じフォルダー内のすべてのファイルがプロジェクトの一部と見なされます。 そのため、プロジェクトに含まれるすべてのファイルを指定する必要はありません。 カスタム ビルド アクションを定義するファイル、または除外するファイルのみを指定する必要があります。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>ContosoExpenses.Data プロジェクトを .NET Standard に移行する

**ContosoExpenses** ソリューションには、.NET 4.7.2 のサービスとターゲットのモデルとインターフェイスを含む **ContosoExpenses.Data** クラス ライブラリが含まれています。 .NET Core 3.0 3.0 アプリでは、.NET Core では使用できない API を使用していない限り、.NET Framework ライブラリを使用できます。 ただし、最善の最新化パスは、ライブラリを .NET Standard に移動することです。 これにより、ライブラリが .NET Core 3.0 アプリによって完全にサポートされるようになります。 また、ライブラリは、Web (ASP.NET Core 経由) やモバイル (Xamarin 経由) などの他のプラットフォームでも再利用できます。

**ContosoExpenses.Data** プロジェクトを .NET Standard に移行するには:

1. Visual Studio で **ContosoExpenses.Data** プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses.Data.csproj の編集]** を選択します。

2. プロジェクト ファイルの内容全体を削除します。

3. 次の XML をコピーして貼り付け、ファイルを保存します。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. **ContosoExpenses.Data** プロジェクトを右クリックし、 **[プロジェクトの再読み込み]** を選択します。

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet パッケージと依存関係の構成

前のセクションで **ContosoExpenses.Core** および **ContosoExpenses.Data** プロジェクトを移行したときに、プロジェクトから NuGet パッケージ参照を削除しました。 このセクションでは、これらの参照を再び追加します。

**ContosoExpenses.Data** プロジェクト用に NuGet パッケージを構成するには:

1. **ContosoExpenses.Data** プロジェクトで、 **[依存関係]** ノードを展開します。 **NuGet** セクションがないことに注意してください。

    ![NuGet パッケージ](images/wpf-modernize-tutorial/NuGetPackages.png)

    **ソリューション エクスプローラー**で **Packages.config** を開くと、完全な .NET Framework を使用していたときに NuGet パッケージの "古い" 参照がプロジェクトで使用されていたことがわかります。

    ![依存関係とパッケージ](images/wpf-modernize-tutorial/Packages.png)

    **Packages.config** ファイルの内容を次に示します。 すべての NuGet パッケージが完全な .NET Framework 4.7.2 を対象としていることがわかります。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. **ContosoExpenses.Data** プロジェクトで、**Packages.config** ファイルを削除します。

4. **ContosoExpenses.Data** プロジェクトで、 **[依存関係]** ノードを右クリックし、 **[NuGet パッケージの管理]** を選択します。

  ![NuGet パッケージの管理...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Bogus` パッケージを探し、最新の安定したバージョンをインストールします。

    ![Bogus NuGet パッケージ](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB` パッケージを探し、最新の安定したバージョンをインストールします。

    ![LiteDB NuGet パッケージ](images/wpf-modernize-tutorial/LiteDB.png)

    このプロジェクトには、packages.config ファイルが含まれなくなっているため、これらの NuGet パッケージの一覧が保存される場所について疑問に思うかもしれません。 参照される NuGet パッケージは、.csproj ファイルに直接格納されます。 これを確認するには、テキスト エディターで **ContosoExpenses.Data.csproj** プロジェクト ファイルの内容を表示します。 ファイルの末尾に次の行が追加されていることがわかります。

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > また、この .NET Core 3 プロジェクトには、.NET Framework 4.7.2 プロジェクトで使用されているものと同じパッケージをインストールしていることもわかります。 NuGet パッケージではマルチターゲットがサポートされます。 ライブラリの作成者は、異なるバージョンのライブラリを同じパッケージに含め、さまざまなアーキテクチャやプラットフォーム用にコンパイルすることができます。 これらのパッケージでは、完全な .NET Framework と、.NET Core 3 プロジェクトと互換性がある .NET Standard 2.0 がサポートされます。 .NET Framework、.NET Core、.NET Standard の相違点について詳しくは、「[.NET Standard](/dotnet/standard/net-standard)」をご覧ください。

**ContosoExpenses.Core** プロジェクト用に NuGet パッケージを構成するには:

1. **ContosoExpenses.Core** プロジェクトで、**packages.config** ファイルを開きます。 現在、.NET Framework 4.7.2 を対象とする次の参照が含まれていることに注意してください。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    次の手順では、`MvvmLightLibs` および `Unity` パッケージの .NET Standard バージョンを使用します。 他の 2 つは、これら 2 つのライブラリをインストールするときに NuGet によって自動的にダウンロードされる依存関係です。

2. **ContosoExpenses.Core** プロジェクトで、**Packages.config** ファイルを削除します。

3. **ContosoExpenses.Core** プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

4. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Unity` パッケージを探し、最新の安定したバージョンをインストールします。

    ![Unity パッケージ](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10` パッケージを探し、最新の安定したバージョンをインストールします。 これは `MvvmLightLibs` パッケージの .NET Standard バージョンです。 このパッケージでは、作成者は、ライブラリの .NET Standard バージョンを .NET Framework バージョンとは別のパッケージにパッケージ化することを選択しました。

    ![MvvmLightsLibs パッケージ](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. **ContosoExpenses.Core** プロジェクトで、 **[依存関係]** ノードを右クリックして **[参照の追加]** を選択します。

7. **[プロジェクト] > [ソリューション]** カテゴリで、 **[ContosoExpenses.Data]** を選択し、 **[OK]** をクリックします。

    ![リファレンスの追加](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>自動生成されたアセンブリ属性を無効にする

移行プロセスのこの時点で、**の ContosoExpenses.Core** プロジェクトをビルドしようとすると、いくつかのエラーが表示されます。

![.NET Core 3 ビルドの新しいエラー](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

この問題が発生するのは、.NET Core 3.0 で導入された新しい .csproj 形式により、アセンブリ情報が **AssemblyInfo.cs** ファイルではなくプロジェクト ファイルに格納されるためです。 これらのエラーを修正するには、この動作を無効にして、プロジェクトで引き続き **AssemblyInfo.cs** ファイルを使用します。

1. Visual Studio で **ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses.Core.csproj の編集]** を選択します。

1. **PropertyGroup** セクションに次の要素を追加し、ファイルを保存します。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加すると、**PropertyGroup** セクションは次のようになります。

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. **ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロジェクトの再読み込み]** を選択します。

4. **ContosoExpenses.Data** プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses.Data.csproj の編集]** を選択します。

5. **PropertyGroup** セクションに同じエントリを追加し、ファイルを保存します。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加すると、**PropertyGroup** セクションは次のようになります。

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. **ContosoExpenses.Data** プロジェクトを右クリックし、 **[プロジェクトの再読み込み]** を選択します。

## <a name="add-the-windows-compatibility-pack"></a>Windows 互換機能パックを追加する

ここで、**ContosoExpenses.Core** および **ContosoExpenses.Data** プロジェクトをコンパイルしようとすると、以前のエラーは修正されていますが、**ContosoExpenses.Data** ライブラリにはまだこれらと同様のエラーがあります。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

これらのエラーは、**ContosoExpenses.Data** プロジェクトを .NET Framework ライブラリ (Windows に固有) から、Linux、Android、iOS などの複数のプラットフォーム上で実行できる .NET Standard ライブラリに変換した結果です。 **ContosoExpenses.Data** プロジェクトには **RegistryService**というクラスが含まれています。このクラスは、Windows のみの概念であるレジストリを操作します。

これらのエラーを解決するには、[Windows 互換機能](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet パッケージをインストールします。 このパッケージでは、.NET Standard ライブラリ内で使用される多数の Windows 固有 API のサポートが提供されます。 このパッケージを使用した後、ライブラリはクロスプラットフォームではなくなりますが、引き続き .NET Standard を対象とします。 

1. **ContosoExpenses.Data** プロジェクトを右クリックします。
2. **[NuGet パッケージの管理]** を選択します。
3. **[NuGet パッケージ マネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Microsoft.Windows.Compatibility` パッケージを探し、最新の安定したバージョンをインストールします。

    ![NuGet パッケージのインストール](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 次に、**ContosoExpenses.Data** プロジェクトを右クリックし、 **[ビルド]** を選択して、プロジェクトのコンパイルを再試行します。

今回は、ビルド プロセスがエラーなしで完了します。

## <a name="test-and-debug-the-migration"></a>移行のテストとデバッグ

プロジェクトが正常にビルドされたので、ランタイム エラーが発生しているかどうかを確認するために、アプリを実行してテストする準備ができました。

1. **ContosoExpenses.Core** プロジェクトを右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

2. F5 キーを押して、デバッガーで **ContosoExpenses.Core** プロジェクトを開始します。 次のような例外が表示されます。

    ![Visual Studio に表示される例外](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    この例外は、移行の開始時に .csproj ファイルからコンテンツを削除したときに、イメージ ファイルの**ビルド アクション**に関する情報を削除したために発生しています。 この問題は、以下の手順で修正されます。

3. デバッガーを停止します。

4. **ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses.Core.csproj の編集]** を選択します。

5. **Project** 要素を終了する前に、次のエントリを追加します。

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. **ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロジェクトの再読み込み]** を選択します。

7. アプリに Contoso.ico を割り当てるには、**ContosoExpenses.Core** プロジェクトを右クリックし、 **[プロパティ]** を選択します。 開いたページで、 **[アイコン]** の下にあるドロップダウンをクリックし、[`Images\contoso.ico`] を選択します。

    ![プロジェクトのプロパティの [Contoso] アイコン](images/wpf-modernize-tutorial/ContosoIco.png)

8. **[Save]** (保存) をクリックします。

9. F5 キーを押して、デバッガーで **ContosoExpenses.Core** プロジェクトを開始します。 アプリが現在実行されていることを確認します。

## <a name="next-steps"></a>次の手順

このチュートリアルのこの時点では、Contoso Expenses アプリを .NET Core 3 に正常に移行しました。 これで、「[パート 2: XAML Islands を使用した UWP InkCanvas コントロールの追加](modernize-wpf-tutorial-2.md)」の準備が整いました。

> [!NOTE]
> 高解像度の画面では、アプリがとても小さいことに気づく可能性があります。 この問題は、チュートリアルの次のステップで対処します。