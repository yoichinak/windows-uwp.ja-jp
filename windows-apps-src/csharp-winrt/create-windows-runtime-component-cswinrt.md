---
description: C#/WinRT で Windows ランタイムコンポーネントを作成し、ネイティブアプリケーションから使用する
title: C#/Winrt コンポーネントを作成し、C++/winrt から使用する
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d489e293ca40aa38c27e4c3e19bba6f8a6705e3b
ms.sourcegitcommit: 6f15cc14e0c4c13999c862664fa7a70de8730b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987103"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>チュートリアル: C#/Winrt コンポーネントを作成し、C++/winrt から使用する

> [!NOTE]
> この記事で説明されている c#/Winrt 作成サポートは、現在、c#/Winrt バージョン1.1.1 の時点でプレビュー段階です。 このリリースでは、初期のフィードバックと評価にのみ使用することを意図しています。

C#/WinRT を使用すると、.NET 5 開発者はクラスライブラリプロジェクトを使用して c# で独自の Windows ランタイムコンポーネントを作成できます。 作成されたコンポーネントは、パッケージ参照または **winmd** ファイル参照を持つネイティブデスクトップアプリケーションで使用できます。

このチュートリアルでは、C#/Winrt を使用して独自の Windows ランタイム型を作成し、Windows ランタイムコンポーネントとしてパッケージ化し、C++/winrt コンソールアプリケーションからコンポーネントを使用する方法について説明します。

ランタイムコンポーネントを作成するときに、 [この記事](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) で説明されているガイドラインと型の制限に従って内部的に、コンポーネント内の Windows ランタイム型は、UWP アプリで許可されているすべての .net 機能を使用できます。 詳細については、「 [UWP アプリ用 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)」を参照してください。 外部的には、型のメンバーは、パラメーターと戻り値に対して Windows ランタイム型のみを公開できます。

> [!NOTE]
> [.Net 型にマップ](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)される Windows ランタイムの型がいくつかあります。 これらの .NET 型は、Windows ランタイムコンポーネントのパブリックインターフェイスで使用できます。また、コンポーネントのユーザーには、対応する Windows ランタイムの種類として表示されます。

## <a name="prerequisites"></a>前提条件

このチュートリアルには、次のツールとコンポーネントが必要です。

- Visual Studio 2019
- .NET 5.0 SDK
- C++/winrt [VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)プロジェクトテンプレート用

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>C#/WinRT を使用して単純な Windows ランタイムコンポーネントを作成する

まず、Visual Studio 2019 で新しいプロジェクトを作成します。 [ **クラスライブラリ (.Net Core)** ] プロジェクトテンプレートを選択し、「 **authoringdemo**」という名前を指定します。 次の追加と変更をプロジェクトに加える必要があります。

1. 最新バージョンの [C#/WinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)をインストールします。

    a. ソリューションエクスプローラーで、プロジェクトノードを右クリックし、[ **NuGet パッケージの管理**] を選択します。

    b. **Microsoft. Windows. CsWinRT** NuGet パッケージを検索し、最新バージョンをインストールします。 このチュートリアルでは、C#/WinRT バージョン1.1.1 を使用します。

2. `TargetFramework` **Authoringdemo .csproj** ファイルのを更新し、次の要素をに追加し `PropertyGroup` ます。

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
        <AssemblyVersion>1.0.0.0</AssemblyVersion>
    </PropertyGroup>
    ```

    要素には、 `TargetFramework` 次の [ターゲットフレームワークモニカー](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)のいずれかを使用できます。
    - **net 5.0-windows 10.0.17763.0**
    - **net 5.0-windows 10.0.18362.0**
    - **net 5.0-windows 10.0.19041.0**

    Windows ランタイムコンポーネントのを指定する必要もあり `AssemblyVersion` ます。

3. `PropertyGroup`複数の **cswinrt** プロパティを設定する新しい要素を追加します。

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
        <CsWinRTEnableLogging>true</CsWinRTEnableLogging>
        <GeneratedFilesDir Condition="'$(GeneratedFilesDir)'==''">$([MSBuild]::NormalizeDirectory('$(MSBuildProjectDirectory)', '$(IntermediateOutputPath)', 'Generated Files'))</GeneratedFilesDir>
    </PropertyGroup>
      ```

      ここでは、この例のプロパティについて詳しく説明します。 CsWinRT プロジェクトのプロパティの完全な一覧については、 [Cswinrt NuGet のドキュメント](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)を参照してください。

    - プロパティは、 `CsWinRTComponent` プロジェクトが Windows ランタイムコンポーネントであることを指定します。これにより、コンポーネントに対して WinMD ファイルが生成されます。
    - プロパティは、 `CsWinRTWindowsMetadata` Windows メタデータのソースを提供します。 これは、バージョン1.1.1 の場合に必要です。
    - プロパティは、 `CsWinRTEnableLogging` ランタイムコンポーネントのビルド時に詳細な出力を含む **log.txt** ファイルを生成します。
    - プロパティは、 `GeneratedFilesDir` 正しい出力ディレクトリに **winmd** ファイルを生成するために必要です。 これは、バージョン1.1.1 の場合に必要です。

4. ランタイムクラスは、ライブラリ **(.cs)** クラスファイルを使用して作成できます。 **Class1.cs** ファイルを右クリックし、名前を **Example.cs** に変更します。 このファイルに次のコードを追加します。これにより、パブリックプロパティとメソッドがランタイムクラスに追加されます。 ランタイムコンポーネントで公開するクラスはすべて、 **パブリック** にマークするようにしてください。

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. これで、プロジェクトをビルドして、コンポーネントのメタデータファイルを生成できるようになりました。 **ソリューションエクスプローラー** でプロジェクトを右クリックし、[**ビルド**] をクリックします。 生成された **Authoringdemo winmd** ファイルは、[**生成されたファイル**] フォルダーの下にある **ソリューションエクスプローラー** のに表示されます。また、ビルド出力フォルダーにも表示されます。

## <a name="generate-a-nuget-package-for-the-component"></a>コンポーネントの NuGet パッケージを生成します

ランタイムコンポーネントを NuGet パッケージとして配布するには、 **Authoringdemo** プロジェクトに次の変更を加える必要があります。 コンポーネントの NuGet パッケージを生成しないことを選択した場合、ネイティブアプリケーションは、次のセクションに示すように、生成された **winmd** ファイルへの直接参照を使用してコンポーネントを使用することもできます。

1. ターゲットファイルを追加して、ネイティブアプリケーションが生成された NuGet パッケージを参照し、コンポーネントを使用できるようにします。 **ソリューションエクスプローラー** で、 **authoringdemo** プロジェクトを右クリックし、[> 追加]、[**新しい項目**] の順に選択します。 **XML ファイル** テンプレートを検索し、ファイルに「 **authoringdemo**」という名前を指定します。

    > [!NOTE]
    > ターゲットファイルには、 *YourComponentName* という形式のコンポーネント名を使用して名前を付ける **必要があり** ます。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <Import Project="$(MSBuildThisDirectory)AuthoringDemo.CsWinRT.targets" />
    </Project> 
    ```

   インポートされた **Authoringdemo. CsWinRT. .targets** ファイルが NuGet パッケージに追加されます。このパッケージは、ネイティブアプリケーションからの使用を可能にするために、C#/WinRT ホスティングアセンブリを使用してパッケージを構成します。  

2. 次の要素を **Authoringdemo .csproj** プロジェクトファイルに追加します。

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

    <ItemGroup>
        <Content Include="AuthoringDemo.targets" PackagePath="build;buildTransitive"/>
    </ItemGroup>
    ```

    これらのプロパティは、コンポーネントの NuGet パッケージを生成し、ネイティブアプリケーションから使用するために CsWinRT ターゲットをパッケージに含めます。

3. **Authoringdemo** プロジェクトをもう一度ビルドします。 ビルド出力で、NuGet パッケージ "AuthoringDemo. 1.0.0" が正常に作成されたことがわかります。

## <a name="consume-the-component-in-cwinrt"></a>C++/WinRT でコンポーネントを使用する

C#/WinRT で作成された Windows ランタイムコンポーネントは、いくつかの変更でネイティブアプリケーションから使用できます。 次の手順では、ネイティブコンソールアプリケーションで作成したコンポーネントを呼び出す方法を示します。

1. 新しい **C++/WinRT コンソールアプリケーション** プロジェクトをソリューションに追加します。 このプロジェクトは、別のソリューションの一部として選択することもできます。

    a. **ソリューションエクスプローラー** で、ソリューションノードを右クリックし、 [  ->  **新しいプロジェクト** の追加] をクリックします。

    b. [ **新しいプロジェクトの追加] ダイアログボックス** で、 **C++/WinRT コンソールアプリケーション** プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。

    c. 新しいプロジェクトに「 **Cppconsoleapp** 」という名前を指定し、[ **作成**] をクリックします。

2. AuthoringDemo コンポーネントへの参照を追加します。 前のセクションから生成された NuGet パッケージへのパッケージ参照を追加するか、または **Authoringdemo. winmd** への直接参照を追加することができます。

    - **オプション 1 (パッケージリファレンス)**: **Cppconsoleapp** プロジェクトを右クリックし、[ **NuGet パッケージの管理**] を選択します。 AuthoringDemo NuGet パッケージへの参照を追加するには、パッケージソースを構成する必要がある場合があります。 これを行うには、NuGet パッケージマネージャーの [設定] 歯車をクリックし、適切なパスにパッケージソースを追加します。

        ![NuGet の設定](images/nuget-sources-settings.png)

        パッケージソースを構成したら、 **Authoringdemo** パッケージを検索し、[ **インストール**] をクリックします。

        ![NuGet パッケージのインストール](images/install-authoring-nuget.png)

    - **オプション 2 (直接参照)**: **Cppconsoleapp** プロジェクトを右クリックし、[ **> 参照**] をクリックします。 [**参照**] タブを選択し、 **authoringdemo** プロジェクトのビルド出力から **authoringdemo** ファイルを探して選択します。

3. コンポーネントのホストを支援するには、ファイルとマニフェストファイルに runtimeconfig.jsを追加する必要があります。 マネージコンポーネントホストの詳細については、 [次のホスティングドキュメント](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)を参照してください。

    a. ファイルに runtimeconfig.jsを追加するには、プロジェクトを右クリックし、[ **追加] > [新しい項目**] の順に選択します。 **テキストファイル** のテンプレートを検索し、 **WinRT.Host.runtimeconfig.js** 名前を入力します。 次の内容を貼り付けます。

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    注エントリについては、 `tfm` DOTNET_ROOT 環境変数を使用して、自己完結型のカスタム .net 5 インストールを参照できます。

    b. マニフェストファイルを追加するには、もう一度プロジェクトを右クリックし、[追加]、[ **新しい項目の >**] の順に選択します。 **テキストファイル** テンプレートを検索して、 **CppConsoleApp.exe .manifest** という名前を指定します。 次の内容を貼り付けます。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    パッケージ化されていないアプリケーションには、マニフェストファイルが必要です。 このファイルでは、上に示すように、アクティブ化可能なクラスの登録エントリを使用してランタイムクラスを指定します。

4. ネイティブプロジェクトファイルを編集して、プロジェクトの配置に runtimeconfig.template.json ファイルとマニフェストファイルを含めます。 プロジェクトを右クリックし、[ **プロジェクトのアンロード**] をクリックします。 アンロードが完了したら、もう一度プロジェクトを右クリックし、[ **プロジェクトファイルの編集**] を選択します。 **WinRT.Host.runtimeconfig.js** のエントリを探し、 **CppConsoleApp.exe します。** `DeploymentContent` 次に示すように、プロパティを追加します。

    ```xml
    <ItemGroup>
        <None Include="WinRT.Host.runtimeconfig.json">
            <DeploymentContent>true</DeploymentContent>
        </None>

        <Manifest Include="CppConsoleApp.exe.manifest">
            <DeploymentContent>true</DeploymentContent>
        </Manifest>
    </ItemGroup> 
    ```

5. プロジェクトのヘッダーファイルの下にある **.pch** を開き、コンポーネントを含めるための次のコード行を追加します。

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. プロジェクトのソースファイルの下にある **メイン .cpp** を開き、次の内容に置き換えます。

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. **Cppconsoleapp** プロジェクトをビルドして実行します。 これで、次の出力が表示されます。

    ![C++/WinRT コンソールの出力](images/consume-component-output.png)

## <a name="related-topics"></a>関連トピック

- [コンポーネントの作成](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [マネージコンポーネントのホスト](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
