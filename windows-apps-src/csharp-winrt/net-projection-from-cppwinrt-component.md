---
description: このチュートリアルでは、C#/Winrt を使用して C++/winrt コンポーネントの .NET 5 プロジェクションを生成する方法について説明します。
title: C++/WinRT コンポーネントから .NET 5 プロジェクションを生成し、NuGet を配布するチュートリアル
ms.date: 11/12/2020
ms.topic: article
keywords: windows 10、c#、winrt、cswinrt、投影
ms.localizationpriority: medium
ms.openlocfilehash: 57bc5c49d47dacee910cd3d80964f797633ef587
ms.sourcegitcommit: 6da85cc75c02a5a7417966abddc8824ac87fb619
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964735"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>チュートリアル: C++/WinRT コンポーネントから .NET 5 プロジェクションを生成し、NuGet を配布する

このチュートリアルでは、 [c#/winrt](index.md) を使用して C++/winrt コンポーネント用の .net 5 プロジェクションを生成する方法、関連付けられた nuget パッケージを作成する方法、および .Net 5 c# コンソールアプリケーションから nuget パッケージを参照する方法について説明します。

このチュートリアルの完全なサンプルは、GitHub から [こちら](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)からダウンロードできます。

## <a name="prerequisites"></a>前提条件

このチュートリアルと対応するサンプルには、次のツールとコンポーネントが必要です。

- ユニバーサル Windows プラットフォーム開発ワークロードがインストールされた[Visual Studio 16.8](https://visualstudio.microsoft.com/downloads/) (またはそれ以降)。 [**インストールの詳細**  >  **ユニバーサル Windows プラットフォーム開発**] で、[ **C++ (v14x) ユニバーサル Windows プラットフォームツール**] オプションをオンにします。
- [.Net 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)。
- C++/winrt [VSIX extension](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) for c++/winrt プロジェクトテンプレート。

## <a name="create-a-simple-cwinrt-runtime-component"></a>単純な C++/WinRT ランタイムコンポーネントを作成する

このチュートリアルを実行するには、最初に .NET 5 プロジェクションを作成する C++/WinRT コンポーネントが必要です。 このチュートリアルでは、GitHub の関連サンプルで **Simplemのコンポーネント** プロジェクトを [使用します](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample/SimpleMathComponent)。 これは、 [c++/WINRT VSIX 拡張機能](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)を使用して作成された **Windows ランタイムコンポーネント (c++/winrt)** プロジェクトです。 開発用コンピューターにプロジェクトをコピーした後、Visual Studio 2019 Preview でソリューションを開きます。

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

C++/winrt コンポーネントを作成し、winmd ファイルを生成する方法の詳細については、「 [c++/winrt を使用したコンポーネントの Windows ランタイム](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)」を参照してください。

> [!NOTE]
> コンポーネントで [IInspectable:: GetRuntimeClassName](/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) を実装する場合は、有効な WinRT クラス名を返す **必要があり** ます。 C#/WinRT は相互運用にクラス名文字列を使用するため、不適切なランタイムクラス名は **InvalidCastException** を発生させます。

## <a name="add-a-projection-project-to-the-component-solution"></a>コンポーネントソリューションにプロジェクションプロジェクトを追加する

リポジトリからサンプルを複製した場合は、最初に **Simplemのプロジェクション** プロジェクトを削除して、チュートリアルの手順に従ってください。

1. ソリューションに新しい **クラスライブラリ (.Net Core)** プロジェクトを追加します。

    1. **ソリューションエクスプローラー** で、ソリューションノードを右クリックし、 [  ->  **新しいプロジェクト** の追加] をクリックします。
    2. [ **新しいプロジェクトの追加] ダイアログボックス** で、[ **クラスライブラリ (.net Core)** ] プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。
    3. 新しいプロジェクトに「 **Simplem」プロジェクション** という名前を指定し、[ **作成**] をクリックします。

2. 空の **Class1.cs** ファイルをプロジェクトから削除します。

3. [C#/WinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)をインストールします。

    1. **ソリューションエクスプローラー** で、 **Simplemのプロジェクション** プロジェクトを右クリックし、[ **NuGet パッケージの管理**] を選択します。 
    2. **Microsoft. Windows. CsWinRT** NuGet パッケージを検索し、最新バージョンをインストールします。

4. **Simplemのコンポーネント** プロジェクトにプロジェクト参照を追加します。 **ソリューションエクスプローラー** で、 **Simplemのプロジェクション** プロジェクトの下にある [**依存関係**] ノードを右クリックし、[**プロジェクト参照の追加**] を選択して、 **simplemのコンポーネント** プロジェクトを選択します。

これらの手順を実行すると、 **ソリューションエクスプローラー** は次のようになります。

![プロジェクションプロジェクトの依存関係を示すソリューションエクスプローラー](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>プロジェクトファイルを編集して C#/WinRT を実行する

**cswinrt.exe** を呼び出して射影アセンブリを生成する前に、射影プロジェクトのプロジェクトファイルを編集する必要があります。

1. **ソリューションエクスプローラー** で、[ **simplemのプロジェクション**] ノードをダブルクリックして、エディターでプロジェクトファイルを開きます。

2. `TargetFramework`Windows SDK を参照するように要素を更新します。 これにより、相互運用とプロジェクションのサポートに必要なアセンブリ depedencies が追加されます。 このサンプルでは、このチュートリアルの **net 5.0-windows 10.0.19041.0** (SDK バージョン2004とも呼ばれます) の最新の Windows 10 リリースを対象としています。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. `PropertyGroup`複数の **cswinrt** プロパティを設定する新しい要素を追加します。

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    この例の設定の詳細を次に示します。

    - プロパティは、 `CsWinRTIncludes` プロジェクトに使用する名前空間を指定します。
    - プロパティは、 `CsWinRTGeneratedFilesDir` プロジェクションからのファイルが生成される出力ディレクトリを設定します。これは、ソースからの構築に関する次のセクションで設定します。

4. このチュートリアルの最新の C#/WinRT バージョンでは、Windows メタデータの指定が必要になる場合があります。 これは、次のいずれかの方法で指定できます。

    - NuGet パッケージの参照 (たとえば [、次の]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/)ような場合)。

      ```xml
      <ItemGroup>
        <PackageReference Include="Microsoft.Windows.SDK.Contracts" Version="10.0.19041.1" />
      </ItemGroup>
      ```

    - 別の方法として、手順 3. のに次のプロパティを追加することもでき `CsWinRTWindowsMetadata` `PropertyGroup` ます。

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. **Simplemの射影 .csproj** ファイルを保存して閉じます。

## <a name="build-projects-out-of-source"></a>ソースからプロジェクトをビルドする

[関連するサンプル](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)では、ビルドは **ディレクトリ.** .. props ファイルで構成されます。 **Simplemのコンポーネント** と **Simplemのプロジェクション** プロジェクトの両方を構築するために生成されたファイルは、ソリューションレベルの *_build* フォルダーに表示されます。 ソースからビルドするようにプロジェクトを構成するには、ソリューションファイルが格納されているディレクトリに以下の **ディレクトリ** をコピーします。

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

相互運用機能アセンブリを配布して使用するには、追加のプロジェクトプロパティを追加して、ソリューションをビルドするときに NuGet パッケージを自動的に作成します。 .NET 5.0 ターゲットの場合、パッケージには、必要な c#/Winrt ランタイムアセンブリの **WinRT.Runtime.dll** の相互運用機能アセンブリ、実装アセンブリ、および C#/winrt NuGet パッケージに対する依存関係が含まれている必要があります。

1. Nuspec プロジェクトに NuGet 仕様 (...) ファイルを **追加します** 。

    1. **ソリューションエクスプローラー** で、[ **simplemのプロジェクション**] ノードを右クリックし、[新しいフォルダーの **追加**] を選択  ->  して、フォルダーに「 **nuget**」という名前を指定します。 
    2. **Nuget** フォルダーを右クリックし、[新しい項目の **追加**] を選択し、  ->  XML ファイルを選択して、 **nuspec** という名前を指定します。 

2. 次のを追加して、パッケージを自動的に生成するようにし **ます。** これらのプロパティは、 `NuspecFile` NuGet パッケージを生成するディレクトリとディレクトリを指定します。

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example NuGet spec for distributing the interop assembly from the C++/WinRT component. Note that for .NET 5.0 targets, under the `dependencies` node there is a dependency on CsWinRT, and under the `files` node **SimpleMathProjection.dll** is specified instead of **SimpleMathComponent.winmd** for the target `lib\net5.0\SimpleMathProjection.dll`. This behavior is new in .NET 5.0 and enabled by C#/WinRT. The implementation assembly, **SimpleMathComponent.dll**, must also be deployed for .NET 5.0 targets. 

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
            <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support netcore3, uap, net46+, net5, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>ソリューションをビルドしてプロジェクションと NuGet パッケージを生成する

この時点で、ソリューションをビルドできるようになりました。ソリューションノードを右クリックし、[ **ソリューションのビルド**] を選択します。 これにより、最初にコンポーネントプロジェクトがビルドされ、次に射影プロジェクトが作成されます。 コンポーネントプロジェクトのメタデータファイルに加えて、interop **.cs** ファイルとアセンブリが出力ディレクトリに生成されます。 また、 **nuget** フォルダーで、生成された Nuget パッケージ **Simplem0.1.0 component** を確認することもできます。

![投影の生成を示すソリューションエクスプローラー](images/projection-generated-files.png)

## <a name="reference-the-nuget-package-in-a-c-net-50-console-application"></a>C# .NET 5.0 コンソールアプリケーションで NuGet パッケージを参照する

投影された **Simplemのコンポーネント** を使用するには、アプリケーションに新しく作成された NuGet パッケージへの参照を追加するだけです。 次の手順では、別のソリューションで簡単なコンソールアプリを作成することによって、これを行う方法を示します。

1. **コンソールアプリ (.Net Core)** プロジェクトを使用して、新しいソリューションを作成します。

    1. Visual Studio で、 **[ファイル]**  ->  **[新規]**  ->  **[プロジェクト]** の順に選択します。
    2. [ **新しいプロジェクトの追加] ダイアログボックス** で、[ **コンソールアプリ (.net Core)** ] プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。
    3. 新しいプロジェクトに **SampleConsoleApp** という名前を指定し、[ **作成**] をクリックします。 新しいソリューションでこのプロジェクトを作成すると、 **Simplemのコンポーネント** NuGet パッケージを個別に復元することができます。

2. **ソリューションエクスプローラー** で、[ **SampleConsoleApp** ] ノードをダブルクリックして **SampleConsoleApp** プロジェクトファイルを開き、次の例に示すように、ターゲットフレームワークモニカーとプラットフォーム構成を更新します。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. **SampleConsoleApp** プロジェクトに **Simplem コンポーネント** NuGet パッケージを追加します。 また、 [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) NuGet パッケージも必要です。これは、msix パッケージにパッケージ化されていないアプリで必要になります。 プロジェクトをビルドするときに **Simplemのコンポーネント** nuget を復元するには、 `RestoreSources` プロパティをコンポーネントソリューションの **nuget** フォルダーへのパスと共に使用します。

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

4. **Program.cs** ファイルを編集して、 **Simplem コンポーネント** によって提供される機能を使用します。

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. コンソールアプリをビルドして実行します。 次の出力が表示されます。

    ![Console NET5 の出力](images/console-output.png)

## <a name="resources"></a>リソース

- [このチュートリアルの完全なコードサンプル](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)
