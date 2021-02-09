---
description: C#/WinRT で Windows ランタイムコンポーネントを作成し、ネイティブアプリケーションから使用する
title: C#/WinRT コンポーネントを作成し、C++/WinRT から使用する
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989636"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>チュートリアル: C#/Winrt コンポーネントを作成し、C++/winrt から使用する

> [!NOTE]
> この記事で説明されている c#/Winrt 作成サポートは、現在、c#/Winrt バージョン 1.1.2 210208.6 の時点でプレビュー段階です。 このリリースでは、初期のフィードバックと評価にのみ使用することを意図しています。

C#/WinRT を使用すると、.NET 5 開発者はクラスライブラリプロジェクトを使用して c# で独自の Windows ランタイムコンポーネントを作成できます。 作成されたコンポーネントは、ネイティブデスクトップアプリケーションでパッケージ参照として使用することも、いくつかの変更を加えたプロジェクト参照として使用することもできます。

このチュートリアルでは、C#/Winrt を使用して単純な Windows ランタイムコンポーネントを作成し、そのコンポーネントを NuGet パッケージとして配布し、C++/winrt コンソールアプリケーションからコンポーネントを使用する方法について説明します。 このチュートリアルのサンプルコードについ [ては、Github を](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)参照してください。

ランタイムコンポーネントの作成中に、この記事で説明されているガイドラインと型の制限に従い [ます。](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 内部的には、コンポーネント内の Windows ランタイム型は、UWP アプリで許可されているすべての .NET 機能を使用できます。 詳細については、「 [UWP アプリ用 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)」を参照してください。 外部的には、型のメンバーは、パラメーターと戻り値に対して Windows ランタイム型のみを公開できます。

> [!NOTE]
> [.Net 型にマップ](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)される Windows ランタイムの型がいくつかあります。 これらの .NET 型は、Windows ランタイムコンポーネントのパブリックインターフェイスで使用できます。また、コンポーネントのユーザーには、対応する Windows ランタイムの種類として表示されます。

## <a name="prerequisites"></a>前提条件

このチュートリアルには、次のツールとコンポーネントが必要です。

- Visual Studio 2019
- .NET 5.0 SDK
- C++/winrt [VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)プロジェクトテンプレート用

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>C#/WinRT を使用して単純な Windows ランタイムコンポーネントを作成する

まず、Visual Studio 2019 で新しいプロジェクトを作成します。 [ **クラスライブラリ (.Net Core)** ] プロジェクトテンプレートを選択し、「 **authoringdemo**」という名前を指定します。 次の追加と変更をプロジェクトに加える必要があります。

1. `TargetFramework` **Authoringdemo .csproj** ファイルのを更新し、次の要素をに追加し `PropertyGroup` ます。

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    要素には、 `TargetFramework` 次の [ターゲットフレームワークモニカー](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)のいずれかを使用できます。
    - **net 5.0-windows 10.0.17763.0**
    - **net 5.0-windows 10.0.18362.0**
    - **net 5.0-windows 10.0.19041.0**

2. 最新バージョンの [C#/WinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6)をインストールします。

    a. ソリューションエクスプローラーで、プロジェクトノードを右クリックし、[ **NuGet パッケージの管理**] を選択します。

    b. **Microsoft. Windows. CsWinRT** NuGet パッケージを検索し、最新バージョンをインストールします。 このチュートリアルでは、C#/WinRT バージョンの 1.1.2 210208.6 を使用します。

3. `PropertyGroup`いくつかの C#/WinRT プロパティを設定する新しい要素を追加します。

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      ここでは、この例のプロパティについて詳しく説明します。 C#/Winrt プロジェクトのプロパティの完全な一覧については、 [C# の NuGet のドキュメント](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)を参照してください。

    - プロパティは、 `CsWinRTComponent` プロジェクトが Windows ランタイムコンポーネントであることを指定します。これにより、コンポーネントに対して WinMD ファイルが生成されます。
    - プロパティは、 `CsWinRTWindowsMetadata` Windows メタデータのソースを提供します。 これは、バージョン1.1.1 の場合に必要です。

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

次に、コンポーネントの NuGet パッケージを生成します。 パッケージを生成すると、C#/WinRT は、ネイティブアプリケーションから使用するために、パッケージ内のコンポーネントとホストアセンブリを構成します。

NuGet パッケージを生成するには、いくつかの方法があります。

* プロジェクトをビルドするたびに NuGet パッケージを生成する場合は、次のプロパティを **Authoringdemo** プロジェクトファイルに追加してから、プロジェクトをリビルドします。

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* または、**ソリューションエクスプローラー** で [ **authoringdemo** ] プロジェクトを右クリックして [ **Pack**] を選択し、NuGet パッケージを生成することもできます。

パッケージをビルドすると、NuGet パッケージが正常に作成されたことが **ビルド** ウィンドウに示され `AuthoringDemo.1.0.0.nupkg` ます。

## <a name="consume-the-component-in-cwinrt"></a>C++/WinRT でコンポーネントを使用する

C#/WinRT で作成された Windows ランタイムコンポーネントは、いくつかの変更でネイティブアプリケーションから使用できます。 次の手順では、ネイティブコンソールアプリケーションで作成したコンポーネントを呼び出す方法を示します。 

1. 新しい **C++/WinRT コンソールアプリケーション** プロジェクトをソリューションに追加します。 このプロジェクトは、別のソリューションの一部として選択することもできます。

    a. **ソリューションエクスプローラー** で、ソリューションノードを右クリックし、 [  ->  **新しいプロジェクト** の追加] をクリックします。

    b. [ **新しいプロジェクトの追加] ダイアログボックス** で、 **C++/WinRT コンソールアプリケーション** プロジェクトテンプレートを検索します。 テンプレートを選択し、[ **次へ**] をクリックします。

    c. 新しいプロジェクトに「 **Cppconsoleapp** 」という名前を指定し、[ **作成**] をクリックします。

2. NuGet パッケージまたはプロジェクト参照として、AuthoringDemo コンポーネントへの参照を追加します。

    - **オプション 1 (パッケージリファレンス)**:  

        a. **Cppconsoleapp** プロジェクトを右クリックし、[ **NuGet パッケージの管理**] を選択します。 AuthoringDemo NuGet パッケージへの参照を追加するには、パッケージソースを構成する必要がある場合があります。 これを行うには、NuGet パッケージマネージャーの **設定** アイコンをクリックし、適切なパスにパッケージソースを追加します。

        ![NuGet の設定](images/nuget-sources-settings.png)

        b. パッケージソースを構成したら、 **Authoringdemo** パッケージを検索し、[ **インストール**] をクリックします。

        ![NuGet パッケージのインストール](images/install-authoring-nuget.png)

    - **オプション 2 (プロジェクト参照)**:
        
        a. **Cppconsoleapp** プロジェクトを右クリックし、[参照の **追加**] を選択し  ->  ます。 [ **プロジェクト** ] ノードの下に、 **authoringdemo** プロジェクトへの参照を追加します。 このプレビューでは、**参照** ノードから **authoringdemo. winmd** へのファイル参照を追加する必要もあります。 生成された winmd ファイルは、 **Authoringdemo** プロジェクトの出力ディレクトリにあります。

        b. このプレビューでは、次のプロパティグループを **Cppconsoleapp. .vcxproj** に追加する必要もあります。 ネイティブアプリケーションプロジェクトファイルを編集するには、まず **Cppconsoleapp** プロジェクトノードを右クリックし、[ **プロジェクトのアンロード**] を選択します。

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

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

4. プロジェクトを変更して、プロジェクトの配置時に出力に runtimeconfig.jsとマニフェストファイルを含めます。 **WinRT.Host.runtimeconfig.js** と **CppConsoleApp.exe .manifest** ファイルの両方について、**ソリューションエクスプローラー** 内のファイルをクリックし、 **Content** プロパティを **True** に設定します。 この例を次に示します。

    ![コンテンツの展開](images/deploy-content.png)

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

- [サンプル コード](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [コンポーネントの作成](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [マネージコンポーネントのホスト](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
