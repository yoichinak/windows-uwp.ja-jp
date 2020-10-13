---
description: このチュートリアルでは、C#/Winrt を使用して C++/winrt コンポーネントの .NET 5 プロジェクションを生成する方法について説明します。
title: C++/WinRT コンポーネントから .NET 5 プロジェクションを生成し、NuGet を配布するチュートリアル
ms.date: 10/12/2020
ms.topic: article
keywords: windows 10、c#、winrt、cswinrt、投影
ms.localizationpriority: medium
ms.openlocfilehash: bc5c8e39b808fd1a8bc557fd29ba828d33d8dde4
ms.sourcegitcommit: df4d99f9950655be725afa83f1ee7c3b73dff923
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "92001392"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>チュートリアル: C++/WinRT コンポーネントから .NET 5 プロジェクションを生成し、NuGet を配布する

このチュートリアルでは、 [c#/winrt](index.md) を使用して C++/winrt コンポーネント用の .net 5 プロジェクションを生成する方法、関連付けられた nuget パッケージを作成する方法、および .Net 5 c# コンソールアプリケーションから nuget パッケージを参照する方法について説明します。

このチュートリアルの完全なサンプルは、GitHub から [こちら](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)からダウンロードできます。

> [!NOTE]
> このチュートリアルは、C#/WinRT (RC2) の最新プレビュー用に書かれています。 今後の1.0 リリースでは、開発者エクスペリエンスの更新と改善が行われる予定です。

## <a name="prerequisites"></a>前提条件

このチュートリアルと対応するサンプルには、次のツールとコンポーネントが必要です。

- ユニバーサル Windows プラットフォーム開発ワークロードがインストールされた[Visual Studio 16.8 Preview 3](https://visualstudio.microsoft.com/vs/preview/) (またはそれ以降)。 [**インストールの詳細**  >  **ユニバーサル Windows プラットフォーム開発**] で、[ **C++ (v14x) ユニバーサル Windows プラットフォームツール**] オプションをオンにします。
- [.Net 5.0 RC2 SDK](https://github.com/dotnet/installer)。
- C++/winrt [VSIX extension](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) for c++/winrt プロジェクトテンプレート。

## <a name="create-a-simple-cwinrt-runtime-component"></a>単純な C++/WinRT ランタイムコンポーネントを作成する

このチュートリアルを実行するには、最初に .NET 5 プロジェクションを作成する C++/WinRT コンポーネントが必要です。 このチュートリアルでは、GitHub の関連サンプルで **Simplemのコンポーネント** プロジェクトを [使用します](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample/SimpleMathComponent)。 これは、 [c++/WINRT VSIX 拡張機能](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を使用して作成された**Windows ランタイムコンポーネント (c++/winrt)** プロジェクトです。 開発用コンピューターにプロジェクトをコピーした後、Visual Studio 2019 Preview でソリューションを開きます。

このプロジェクトのコードは、次のヘッダーファイルに示されている基本的な算術演算の機能を提供します。 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

C++/winrt コンポーネントを作成し、winmd ファイルを生成する方法の詳細については、「 [c++/winrt を使用したコンポーネントの Windows ランタイム](https://docs.microsoft.com/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)」を参照してください。

> [!NOTE]
> コンポーネントで [IInspectable:: GetRuntimeClassName](https://docs.microsoft.com/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) を実装する場合は、有効な WinRT クラス名を返す **必要があり** ます。 C#/WinRT は相互運用にクラス名文字列を使用するため、不適切なランタイムクラス名は **InvalidCastException**を発生させます。

## <a name="add-a-projection-project-to-the-component-solution"></a>コンポーネントソリューションにプロジェクションプロジェクトを追加する

リポジトリからサンプルを複製した場合は、最初に **Simplemのプロジェクション** プロジェクトを削除して、チュートリアルの手順に従ってください。

1. ソリューションに新しい **クラスライブラリ (.Net Core)** プロジェクトを追加します。

    1. **ソリューションエクスプローラー**で、ソリューションノードを右クリックし、 **Add**[  ->  **新しいプロジェクト**の追加] をクリックします。
    2. [ **新しいプロジェクトの追加] ダイアログボックス**で、[ **クラスライブラリ (.net Core)** ] プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。
    3. 新しいプロジェクトに「 **Simplem」プロジェクション** という名前を指定し、[ **作成**] をクリックします。

2. 空の **Class1.cs** ファイルをプロジェクトから削除します。

3. [C#/WinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)をインストールします。

    1. **ソリューションエクスプローラー**で、 **Simplemのプロジェクション**プロジェクトを右クリックし、[ **NuGet パッケージの管理**] を選択します。 
    2. **Microsoft. Windows. CsWinRT** NuGet パッケージを検索し、最新バージョンをインストールします。

4. **Simplemのコンポーネント**プロジェクトにプロジェクト参照を追加します。 **ソリューションエクスプローラー**で、 **Simplemのプロジェクション**プロジェクトの下にある [**依存関係**] ノードを右クリックし、[**プロジェクト参照の追加**] を選択して、 **simplemのコンポーネント**プロジェクトを選択します。

    > [!NOTE]
    > Visual Studio 16.8 Preview 4 以降を使用している場合は、手順 4. を完了した後でこのセクションを実行します。 Visual Studio 16.8 Preview 3 を使用している場合は、手順 5. も完了する必要があります。

5. Visual Studio 16.8 Preview 3 を使用している場合: **ソリューションエクスプローラー**で、[ **simplemのプロジェクション** ] ノードをダブルクリックしてエディターでプロジェクトファイルを開き、ファイルに次の要素を追加して、ファイルを保存して閉じます。

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Net.Compilers.Toolset" Version="3.8.0-4.20472.6" />
    </ItemGroup>

    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json
      </RestoreSources>
    </PropertyGroup>
    ```

    これらの要素は、最新の C# コンパイラを含む、必要なバージョンの **Microsoft.Net** NuGet パッケージをインストールします。 このチュートリアルでは、これらのプロジェクトファイル参照を使用して、この NuGet パッケージをインストールします。これは、このパッケージの必要なバージョンが既定のパブリック NuGet フィードで利用できない可能性があるためです。

これらの手順を実行すると、 **ソリューションエクスプローラー** は次のようになります。

![プロジェクションプロジェクトの依存関係を示すソリューションエクスプローラー](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>プロジェクトファイルを編集して C#/WinRT を実行する

**cswinrt.exe**を呼び出して射影アセンブリを生成する前に、射影プロジェクトのプロジェクトファイルを編集する必要があります。

1. **ソリューションエクスプローラー**で、[ **simplemのプロジェクション**] ノードをダブルクリックして、エディターでプロジェクトファイルを開きます。

2. `TargetFramework`Windows SDK を参照するように要素を更新します。 これにより、相互運用とプロジェクションのサポートに必要なアセンブリ depedencies が追加されます。 このサンプルでは、このチュートリアルの **net 5.0-windows 10.0.19041.0** (SDK バージョン2004とも呼ばれます) の最新の Windows 10 リリースを対象としています。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. `PropertyGroup`複数の**cswinrt**プロパティを設定する新しい要素を追加します。

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    この例の設定の詳細を次に示します。

    - プロパティは、 `CsWinRTIncludes` プロジェクトに使用する名前空間を指定します。
    - プロパティは、 `CsWinRTGeneratedFilesDir` プロジェクションからのファイルが生成される出力ディレクトリを設定します。これは、ソースからの構築に関する次のセクションで設定します。

4. **Simplemの射影 .csproj**ファイルを保存して閉じます。

## <a name="build-projects-out-of-source"></a>ソースからプロジェクトをビルドする

[関連するサンプル](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)では、ビルドは**ディレクトリ.** .. props ファイルで構成されます。 **Simplemのコンポーネント**と**Simplemのプロジェクション**プロジェクトの両方を構築するために生成されたファイルは、ソリューションレベルの *_build*フォルダーに表示されます。 ソースからビルドするようにプロジェクトを構成するには、ソリューションファイルが格納されているディレクトリに以下の **ディレクトリ** をコピーします。

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

この手順は射影を生成するためには必要ありませんが、同じディレクトリ内の両方のプロジェクトからビルドファイルを生成し、ビルドのクリーンアップを簡単にすることによって簡単に実現できます。 ソースを使用しない場合は、各プロジェクトフォルダー内の異なるディレクトリに、 **Simplemと** interop assembly **SimpleMathComponent.dll** の両方が生成されることに注意してください。 これらのファイルは、次の **nuspec** で参照されます。そのため、パスはそれに応じて変更する必要があります。

## <a name="create-a-nuget-package-from-the-projection"></a>プロジェクションから NuGet パッケージを作成する

相互運用機能アセンブリを配布して使用するには、追加のプロジェクトプロパティを追加して、ソリューションをビルドするときに NuGet パッケージを自動的に作成します。 このパッケージには、必要な c#/Winrt ランタイムアセンブリの相互運用機能アセンブリと、c#/Winrt NuGet パッケージに対する依存関係が含まれます。 このランタイムアセンブリには、.NET 5.0 ターゲットの **winrt.runtime.dll** という名前が付けられています。

1. Nuspec プロジェクトに NuGet 仕様 (...) ファイルを **追加します** 。

    1. **ソリューションエクスプローラー**で、[ **simplemのプロジェクション**] ノードを右クリックし、[新しいフォルダーの**追加**] を選択  ->  **New Folder**して、フォルダーに「 **nuget**」という名前を指定します。 
    2. **Nuget**フォルダーを右クリックし、[新しい項目の**追加**] を選択し、  ->  **New Item**XML ファイルを選択して、 **nuspec**という名前を指定します。 

2. 次のを追加して、パッケージを自動的に生成するようにし**ます。** これらのプロパティは、 `NuspecFile` NuGet パッケージを生成するディレクトリとディレクトリを指定します。

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example of a C++/WinRT component NuGet spec. Notice the `dependency` on CsWinRT for the `net5.0` target framework moniker, as well as the target for `lib\net5.0\SimpleMathProjection.dll`, which points to the projection assembly **SimpleMathComponent.dll** instead of **SimpleMathComponent.winmd**. This behavior is new in .NET 5.0 and enabled by C#/WinRT.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="0.8.0" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support net46+, netcore3, net5, uap, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>ソリューションをビルドしてプロジェクションと NuGet パッケージを生成する

この時点で、ソリューションをビルドできるようになりました。ソリューションノードを右クリックし、[ **ソリューションのビルド**] を選択します。 これにより、最初にコンポーネントプロジェクトがビルドされ、次に射影プロジェクトが作成されます。 コンポーネントプロジェクトのメタデータファイルに加えて、interop **.cs** ファイルとアセンブリが出力ディレクトリに生成されます。 また、 **nuget**フォルダーで、生成された Nuget パッケージ**Simplem0.1.0 component**を確認することもできます。

![投影の生成を示すソリューションエクスプローラー](images/projection-generated-files.png)

## <a name="referencethenugetpackage-inacnet50consoleapplication"></a>C# .NET 5.0 コンソールアプリケーションで NuGet パッケージを参照する

投影された **Simplemのコンポーネント**を使用するには、アプリケーションに新しく作成された NuGet パッケージへの参照を追加するだけです。 次の手順では、別のソリューションで簡単なコンソールアプリを作成することによって、これを行う方法を示します。

1. **コンソールアプリ (.Net Core)** プロジェクトを使用して、新しいソリューションを作成します。

    1. Visual Studio で、 **[ファイル]**  ->  **[新規]**  ->  **[プロジェクト]** の順に選択します。
    2. [ **新しいプロジェクトの追加] ダイアログボックス**で、[ **コンソールアプリ (.net Core)** ] プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。
    3. 新しいプロジェクトに **SampleConsoleApp** という名前を指定し、[ **作成**] をクリックします。 新しいソリューションでこのプロジェクトを作成すると、 **Simplemのコンポーネント** NuGet パッケージを個別に復元することができます。

2. **ソリューションエクスプローラー**で、[ **SampleConsoleApp** ] ノードをダブルクリックして**SampleConsoleApp**プロジェクトファイルを開き、次の例に示すように、ターゲットフレームワークモニカーとプラットフォーム構成を更新します。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. **SampleConsoleApp**プロジェクトに**Simplem コンポーネント**NuGet パッケージを追加します。 また、 [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) NuGet パッケージも必要です。これは、msix パッケージにパッケージ化されていないアプリで必要になります。 プロジェクトをビルドするときに **Simplemのコンポーネント** nuget を復元するには、 `RestoreSources` プロパティをコンポーネントソリューションの **nuget** フォルダーへのパスと共に使用します。

    ```xml
    <PropertyGroup>
      <RestoreSources>
          https://api.nuget.org/v3/index.json;
          ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
        <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    このチュートリアルでは、 **Simplemare コンポーネント** の NuGet 復元パスでは、両方のソリューションファイルが同じディレクトリにあることを前提としています。 または、 [ローカルの NuGet パッケージフィード](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) をソリューションに追加することもできます。

4. **Program.cs**ファイルを編集して、 **Simplem コンポーネント**によって提供される機能を使用します。

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. コンソールアプリをビルドして実行します。 次の出力が表示されます。

    ![Console NET5 の出力](images/console-output.png)

## <a name="resources"></a>リソース

- [このチュートリアルの完全なコードサンプル](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)
