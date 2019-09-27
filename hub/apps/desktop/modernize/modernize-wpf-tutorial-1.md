---
description: このチュートリアルでは、UWP XAML ユーザーインターフェイスを追加する方法、MSIX パッケージを作成する方法、およびその他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: Contoso Expenses　アプリの .NET Core 3 への移行
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317103"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>作業 1:Contoso Expenses　アプリの .NET Core 3 への移行

これは、Contoso の支出という名前の WPF デスクトップアプリのサンプルを最新化する方法を示すチュートリアルの最初の部分です。 サンプルアプリをダウンロードするためのチュートリアル、前提条件、および手順の概要につい[ては、「チュートリアル:WPF アプリ](modernize-wpf-tutorial.md)を最新化します。
  
チュートリアルのこの部分では、.NET Framework 4.7.2 から[.Net Core 3](modernize-wpf-tutorial.md#net-core-3)に Contoso の経費アプリ全体を移行します。 チュートリアルのこの部分を開始する前に、Visual Studio 2019 で[ContosoExpenses サンプルを開いてビルド](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app)してください。

> [!NOTE]
> WPF アプリケーションを .NET Framework から .NET Core 3 に移行する方法の詳細については、[このブログシリーズ](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/)を参照してください。

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>ContosoExpenses プロジェクトを .NET Core 3 に移行する

このセクションでは、Contoso の経費アプリの ContosoExpenses プロジェクトを .NET Core 3 に移行します。 これを行うには、既存の ContosoExpenses プロジェクトと同じファイルを含む新しいプロジェクトファイルを作成しますが、.NET Framework 4.7.2 の代わりに .NET Core 3 を対象とします。 これにより、.NET Framework と .NET Core の両方のバージョンのアプリで1つのソリューションを管理できます。

1. ContosoExpenses プロジェクトが現在 .NET Framework 4.7.2 を対象としていることを確認します。 ソリューションエクスプローラーで、 **ContosoExpenses**プロジェクトを右クリックし、 **[プロパティ]** を選択し、 **[アプリケーション]** タブの **[ターゲットフレームワーク]** プロパティが .NET Framework 4.7.2 に設定されていることを確認します。

    ![プロジェクトの .NET Framework バージョン4.7.2](images/wpf-modernize-tutorial/NETFramework472.png)

3. Windows エクスプローラーで、 **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**フォルダーに移動し、 **ContosoExpenses**という名前の新しいテキストファイルを作成します。

4. ファイルを右クリックし、[ファイルを**開くアプリケーション**の選択] をクリックします。次に、メモ帳、Visual Studio Code、Visual Studio など、任意のテキストエディターでファイルを開きます。

5. 次のテキストをファイルにコピーして保存します。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. ファイルを閉じて、Visual Studio の**ContosoExpenses**ソリューションに戻ります。

7. **ContosoExpenses**ソリューションを右クリックし、 **[既存のプロジェクトの追加 >]** を選択します。 `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses`フォルダーに作成した**ContosoExpenses**ファイルを選択して、ソリューションに追加します。

**ContosoExpenses**には、次の要素が含まれています。

* **Project**要素には、 **Microsoft .net**sdk バージョンの .net が指定されています。 これは、Windows デスクトップ用の .NET アプリケーションを指し、WPF と Windows フォームアプリのコンポーネントを含みます。
* **PropertyGroup**要素には、プロジェクト出力が (DLL ではなく) 実行可能ファイルであり、.net Core 3 を対象とし、WPF を使用する子要素が含まれています。 Windows フォームアプリの場合は、 **Usewpf**要素の代わりに**UseWinForms**要素を使用します。

> [!NOTE]
> .NET Core 3.0 で導入された .csproj 形式を使用すると、.csproj と同じフォルダー内のすべてのファイルがプロジェクトの一部と見なされます。 そのため、プロジェクトに含まれるすべてのファイルを指定する必要はありません。 カスタムビルドアクションを定義するファイル、または除外するファイルのみを指定する必要があります。

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>ContosoExpenses プロジェクトを .NET Standard に移行します。

**ContosoExpenses**ソリューションには、.net 4.7.2 のサービスとターゲットのモデルとインターフェイスを含む**ContosoExpenses**クラスライブラリが含まれています。 .Net Core 3.0 アプリは、.NET Core で使用できない Api を使用していない限り、.NET Framework ライブラリを使用できます。 ただし、最も効果的な方法は、ライブラリを .NET Standard に移動することです。 これにより、ライブラリが .NET Core 3.0 アプリによって完全にサポートされるようになります。 また、ライブラリは、web (ASP.NET Core) や mobile (Xamarin を通じて) などの他のプラットフォームでも再利用できます。

**ContosoExpenses**プロジェクトを .NET Standard に移行するには、次のようにします。

1. Visual Studio で、 **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**アンロード**] を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses の編集]** を選択します。

2. プロジェクトファイルの内容全体を削除します。

3. 次の XML をコピーして貼り付け、ファイルを保存します。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**再読み込み**] をクリックします。

## <a name="configure-nuget-packages-and-dependencies"></a>NuGet パッケージと依存関係の構成

前のセクションで**ContosoExpenses**プロジェクトと**ContosoExpenses**プロジェクトを移行すると、プロジェクトから NuGet パッケージ参照が削除されました。 このセクションでは、これらの参照を再び追加します。

**ContosoExpenses**プロジェクトの NuGet パッケージを構成するには、次のようにします。

1. **ContosoExpenses**プロジェクトで、 **[依存関係]** ノードを展開します。 **NuGet**セクションがないことに注意してください。

    ![NuGet パッケージ](images/wpf-modernize-tutorial/NuGetPackages.png)

    **ソリューションエクスプローラー**で**app.config**を開くと、完全 .NET Framework を使用したときにプロジェクトを使用した NuGet パッケージの "古い" 参照が検出されます。

    ![依存関係とパッケージ](images/wpf-modernize-tutorial/Packages.png)

    次に、**パッケージの .config**ファイルの内容を示します。 すべての NuGet パッケージが完全な .NET Framework 4.7.2 を対象としていることがわかります。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. **ContosoExpenses**プロジェクトで、 **app.config**ファイルを削除します。

4. **ContosoExpenses**プロジェクトで、 **[依存関係]** ノードを右クリックし、 **[NuGet パッケージの管理]** を選択します。

  ![NuGet パッケージの管理...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. **[NuGet パッケージマネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Bogus`パッケージを検索し、最新の安定したバージョンをインストールします。

    ![偽の NuGet パッケージ](images/wpf-modernize-tutorial/Bogus.png)

6. `LiteDB`パッケージを検索し、最新の安定したバージョンをインストールします。

    ![LiteDB NuGet パッケージ](images/wpf-modernize-tutorial/LiteDB.png)

    このプロジェクトには、パッケージの .config ファイルが含まれていないため、これらの NuGet パッケージの一覧が保存されている場所について疑問に思うかもしれません。 参照される NuGet パッケージは、.csproj ファイルに直接格納されます。 これを確認するには、テキストエディターで**ContosoExpenses**プロジェクトファイルの内容を表示します。 ファイルの末尾に次の行が追加されていることがわかります。

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > また、この .NET Core 3 プロジェクトには、.NET Framework 4.7.2 プロジェクトで使用されているものと同じパッケージをインストールしていることがわかります。 NuGet パッケージはマルチターゲットをサポートしています。 ライブラリの作成者は、異なるバージョンのライブラリを同じパッケージに含めることができ、さまざまなアーキテクチャやプラットフォーム用にコンパイルされます。 これらのパッケージは、完全な .NET Framework と .NET Standard 2.0 をサポートします。これは、.NET Core 3 プロジェクトと互換性があります。 .NET Framework、.NET Core、.NET Standard の相違点の詳細については、「 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)」を参照してください。

**ContosoExpenses**プロジェクトの NuGet パッケージを構成するには、次のようにします。

1. **ContosoExpenses**プロジェクトで、 **app.config**ファイルを開きます。 現在、.NET Framework 4.7.2 を対象とする次の参照が含まれていることに注意してください。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    次の手順では、 `MvvmLightLibs`および`Unity`パッケージのバージョンを .NET Standard します。 その他の2つは、これらの2つのライブラリをインストールするときに NuGet によって自動的にダウンロードされる依存関係です。

2. **ContosoExpenses**プロジェクトで、**パッケージの .config**ファイルを削除します。

3. **ContosoExpenses**プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。

4. **[NuGet パッケージマネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Unity`パッケージを検索し、最新の安定したバージョンをインストールします。

    ![Unity パッケージ](images/wpf-modernize-tutorial/UnityPackage.png)

5. `MvvmLightLibsStd10`パッケージを検索し、最新の安定したバージョンをインストールします。 これは、 `MvvmLightLibs`パッケージの .NET Standard バージョンです。 このパッケージでは、作成者は、ライブラリの .NET Standard バージョンを、.NET Framework バージョンとは別のパッケージにパッケージ化することを選択しました。

    ![MvvmLightsLibs パッケージ](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. **ContosoExpenses**プロジェクトで、 **[依存関係]** ノードを右クリックし、 **[参照の追加]** を選択します。

7. **[プロジェクト > ソリューション]** カテゴリで、 **[ContosoExpenses]** を選択し、 **[OK]** をクリックします。

    ![リファレンスの追加](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>自動生成されたアセンブリ属性を無効にする

移行プロセスのこの時点で、 **ContosoExpenses**プロジェクトをビルドしようとすると、いくつかのエラーが表示されます。

![.NET Core 3 ビルドの新しいエラー](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

この問題が発生するのは、.NET Core 3.0 で導入された新しい .csproj 形式で、アセンブリ情報が**AssemblyInfo.cs**ファイルではなくプロジェクトファイルに格納されるためです。 これらのエラーを修正するには、この動作を無効にして、プロジェクトで引き続き**AssemblyInfo.cs**ファイルを使用できるようにします。

1. Visual Studio で、 **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**アンロード**] を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses の編集]** を選択します。

1. **PropertyGroup**セクションに次の要素を追加し、ファイルを保存します。

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加すると、 **PropertyGroup**セクションは次のようになります。

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**再読み込み**] をクリックします。

4. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**アンロード**] を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses の編集]** を選択します。

5. **PropertyGroup**セクションに同じエントリを追加し、ファイルを保存します。

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    この要素を追加すると、 **PropertyGroup**セクションは次のようになります。

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**再読み込み**] をクリックします。

## <a name="add-the-windows-compatibility-pack"></a>Windows 互換機能パックを追加する

これで、 **ContosoExpenses**プロジェクトと**ContosoExpenses**プロジェクトをコンパイルしようとすると、以前のエラーが修正されていますが、 **ContosoExpenses**ライブラリにはこのようなエラーが依然として存在します。

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

これらのエラーは、 **ContosoExpenses**プロジェクトを .NET Framework ライブラリ (Windows に固有) から .NET Standard ライブラリに変換した結果として得られます。これは、Linux、Android、iOS などの複数のプラットフォームで実行できます。 **ContosoExpenses**プロジェクトには、Windows のみの概念であるレジストリと対話する**registryservice**というクラスが含まれています。

これらのエラーを解決するには、 [Windows 互換性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)NuGet パッケージをインストールします。 このパッケージは、.NET Standard ライブラリで使用される多数の Windows 固有 Api のサポートを提供します。 このパッケージを使用した後、ライブラリがクロスプラットフォームになることはありませんが、引き続き .NET Standard を対象とします。 

1. **ContosoExpenses**プロジェクトを右クリックします。
2. **[NuGet パッケージの管理]** を選択します。
3. **[NuGet パッケージマネージャー]** ウィンドウで、 **[参照]** をクリックします。 `Microsoft.Windows.Compatibility`パッケージを検索し、最新の安定したバージョンをインストールします。

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. 次に、 **ContosoExpenses**プロジェクトを右クリックし、 **[ビルド]** を選択して、プロジェクトのコンパイルを再試行します。

今回は、ビルドプロセスはエラーなしで完了します。

## <a name="test-and-debug-the-migration"></a>移行のテストとデバッグ

プロジェクトが正常にビルドされたので、ランタイムエラーが発生しているかどうかを確認するために、アプリを実行してテストする準備ができました。

1. **ContosoExpenses**プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。

2. F5 キーを押して、デバッガーで**ContosoExpenses**プロジェクトを開始します。 次のような例外が表示されます。

    ![例外が Visual Studio に表示される](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    この例外は、移行の開始時に .csproj ファイルからコンテンツを削除すると、イメージファイルの**ビルドアクション**に関する情報が削除されたために発生しています。 この問題を解決するには、次の手順を実行します。

3. デバッガーを停止します。

4. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**アンロード**] を選択します。 プロジェクトをもう一度右クリックし、 **[ContosoExpenses の編集]** を選択します。

5. **プロジェクト**の終了要素の前に、次のエントリを追加します。

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. **ContosoExpenses**プロジェクトを右クリックし、[プロジェクトの**再読み込み**] をクリックします。

7. アプリケーションに Contoso .ico を割り当てるには、 **ContosoExpenses**プロジェクトを右クリックし、 **[プロパティ]** を選択します。 開いているページで、 **[アイコン]** の下にある`Images\contoso.ico`ドロップダウンをクリックしてを選択します。

    ![プロジェクトのプロパティの [Contoso] アイコン](images/wpf-modernize-tutorial/ContosoIco.png)

8. **[保存]** をクリックします。

9. F5 キーを押して、デバッガーで**ContosoExpenses**プロジェクトを開始します。 アプリが現在実行されていることを確認します。

## <a name="next-steps"></a>次の手順

このチュートリアルのこの時点で、Contoso の経費アプリが .NET Core 3 に正常に移行されました。 これで、第 2 [部の準備ができました。XAML アイランド](modernize-wpf-tutorial-2.md)を使用して UWP system.windows.controls.inkcanvas> コントロールを追加します。

> [!NOTE]
> 高解像度の画面では、アプリが非常に小さいことがわかります。 この問題に対処するには、チュートリアルの次の手順を実行します。
