---
description: このトピックでは、シンプルな C# コンポーネントを C++/WinRT アプリに追加する処理を順を追って説明します
title: C++/WinRT アプリから使用するための C# Windows ランタイム コンポーネントの作成
ms.date: 12/30/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, C#
ms.localizationpriority: medium
ms.openlocfilehash: cc341f6fd716daf0474fdbcb25f567a7c727d507
ms.sourcegitcommit: 7d542c6367b3b441044225431ee69d869ed0ff4b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2021
ms.locfileid: "102402338"
---
# <a name="authoring-a-c-windows-runtime-component-for-use-from-a-cwinrt-app"></a>C++/WinRT アプリから使用するための C# Windows ランタイム コンポーネントの作成

このトピックでは、シンプルな C# コンポーネントを C++/WinRT プロジェクトに追加する処理を順を追って説明します。

Visual Studio を使用すると、C# または Visual Basic で記述された Windows ランタイム コンポーネント (WRC) プロジェクト内に独自のカスタム Windows ランタイム型を簡単に作成して配置し、次に C++ アプリケーション プロジェクトからその WRC を参照して、そのアプリケーションからそれらのカスタム型を使用することができます。

内部的には、UWP アプリケーションで許可されているすべての .NET 機能をその Windows ランタイム型で使用できます。

> [!NOTE]
> 詳しくは、「[C# および Visual Basic を使用した Windows ランタイム コンポーネント](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)」および「[UWP アプリ用 .NET の概要](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)」を参照してください。

外部的には、型のメンバーによってパラメーターと戻り値の Windows ランタイム型のみを公開できます。 ソリューションをビルドすると、Visual Studio によって .NET WRC プロジェクトがビルドされ、Windows メタデータ (.winmd) ファイルを作成するビルド ステップが実行されます。 これが、Visual Studio によってアプリに含められる Windows ランタイム コンポーネント (WRC)です。

> [!NOTE]
> .NET により、一般的に使用される .NET の型 (プリミティブ データ型やコレクション型など) が対応する Windows ランタイム型に自動的にマップされます。 .NET のこれら型は、Windows ランタイム コンポーネントのパブリック インターフェイス内で使用でき、対応する Windows ランタイム型としてコンポーネントのユーザーに表示されます。 「[C# および Visual Basic を使用した Windows ランタイム コンポーネント](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)」を参照してください。

## <a name="prerequisites"></a>前提条件:

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-blank-app"></a>空のアプリの作成

Visual Studio で **[空のアプリ (C++WinRT)]** プロジェクト テンプレートを使用して新しいプロジェクトを作成します。 **(ユニバーサル Windows)** テンプレートではなく、 **(C++/WinRT)** テンプレートを使用していることをご確認ください。

新しいプロジェクトの名前を *CppToCSharpWinRT* に設定して、フォルダー構造がこのチュートリアルと一致するようにします。

## <a name="add-a-c-windows-runtime-component-to-the-solution"></a>C# Windows ランタイム コンポーネントをソリューションに追加する

Visual Studio で、コンポーネント プロジェクトを作成します。ソリューション エクスプローラーで、*CppToCSharpWinRT* ソリューションのショートカット メニューを開き、 **[追加]** 、 **[新しいプロジェクト]** の順にクリックして、新しい C# プロジェクトをソリューションに追加します。 **[新しいプロジェクトの追加]** ダイアログ ボックスの **[インストールされたテンプレート]** セクションで、 **[Visual C#]** を選択し、 **[Windows]** 、 **[ユニバーサル]** の順に選択します。 **[Windows ランタイム コンポーネント (ユニバーサル Windows)]** テンプレートを選択し、プロジェクト名として「**SampleComponent**」と入力します。 

> [!NOTE]
> **[新しいユニバーサル Windows プラットフォーム プロジェクト]** ダイアログ ボックスで、 **[Windows 10 Creators Update (10.0; Build 15063)]** を最小バージョンとして選択します。 詳しくは、後述の「[アプリケーションの最小バージョン](#application-minimum-version)」セクションを参照してください。

## <a name="add-the-c-getmystring-method"></a>C# GetMyString メソッドを追加する

*SampleComponent* プロジェクトで、クラスの名前を *Class1* から *Example* に変更します。 次に、2 つの単純なメンバー (プライベート `int` フィールドと、*GetMyString* という名前のインスタンス メソッド) をクラスに追加します。

> ```csharp
>     public sealed class Example
>     {
>         int MyNumber;
> 
>         public string GetMyString()
>         {
>             return $"This is call #: {++MyNumber}";
>         }
>     }
> ```

> [!NOTE]
> 既定では、クラスは **public sealed** とマークされます。 コンポーネントから公開するすべての Windows ランタイム クラスを **保護** する必要があります。

> [!NOTE]
> 省略可能: 新しく追加したメンバーで IntelliSense を有効にするには、ソリューション エクスプローラーで *SampleComponent* プロジェクトのショートカット メニューを開き、 **[ビルド]** を選びます。

## <a name="reference-the-c-samplecomponent-from-the-cpptocsharpwinrt-project"></a>CppToCSharpWinRT プロジェクトから C# SampleComponent を参照する

ソリューション エクスプローラーの C++/WinRT プロジェクトで、 **[参照]** のショートカット メニューを開き、 **[参照の追加]** を選択して **[参照の追加]** を開きます。 **[プロジェクト]** をクリックし、**[ソリューション]** をクリックします。 *SampleComponent* プロジェクトのチェック ボックスをオンにし、 **[OK]** をクリックして参照を追加します。

> [!NOTE]
> 省略可能: C++/WinRT プロジェクトで IntelliSense を有効にするには、ソリューション エクスプローラーで *CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[ビルド]** を選びます。

## <a name="edit-mainpageh"></a>MainPage.h を編集する

*CppToCSharpWinRT* プロジェクトで `MainPage.h` を開き、2 つの項目を追加します。  まず、`#include` ステートメントの最後に `#include "winrt/SampleComponent.h"` を追加してから、`winrt::SampleComponent::Example` フィールドを `MainPage` 構造体に追加します。

```cppwinrt
// MainPage.h
...
#include "winrt/SampleComponent.h"

namespace winrt::CppToCSharpWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        winrt::SampleComponent::Example myExample;
...
    };
}
```

> [!NOTE]
> Visual Studio では、`MainPage.h` が `MainPage.xaml` の下に一覧表示されます。

## <a name="edit-mainpagecpp"></a>MainPage.cpp を編集する

`MainPage.cpp` で、C# メソッド `GetMyString` を呼び出すように `Mainpage::ClickHandler` 実装を変更します。

```cppwinrt
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    //myButton().Content(box_value(L"Clicked"));

    hstring myString = myExample.GetMyString();

    myButton().Content(box_value(myString));
}
```
## <a name="run-the-project"></a>プロジェクトを実行する

これでプロジェクトをビルドして実行できるようになりました。 ボタンをクリックするたびに、ボタンの数値が増加します。

![C# コンポーネントを呼び出す C++/WinRT Windows のスクリーンショット](images/csharp-cpp-sample.png)

> [!TIP]
> Visual Studio で、コンポーネント プロジェクトを作成します。ソリューション エクスプローラーで、*CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[プロパティ]** を選択してから、 **[構成プロパティ]** の下の **[デバッグ]** をクリックします。 C# (マネージド)と C++ (ネイティブ) の両方のコードをデバッグする場合は、[デバッガーの種類] を " *[マネージド] または [ネイティブ]* " に設定します。
> ![C++ デバッグのプロパティ](images/cpp-debugging-properties.png)

## <a name="application-minimum-version"></a>アプリケーションの最小バージョン

アプリケーションのコンパイルに使用される .NET のバージョンは、C# プロジェクト バージョンの [ **[Application Minimum]\(アプリケーションの最小\)**](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) で制御されます。 たとえば、 **[Windows 10 Fall Creators Update (10.0; Build 16299)]** 以上を選ぶと、.NET Standard 2.0 と Windows ARM64 プロセッサのサポートが有効になります。 

> [!TIP]
> .NET Standard 2.0 または ARM64 のサポートが不要な場合は、16299 より前の **[Application Minimum]\(アプリケーションの最小\)** バージョンを使用して追加のビルド構成を回避することをお勧めします。

## <a name="configure-for-windows-10-fall-creators-update-100-build-16299"></a>Windows 10 Fall Creators Update (10.0; Build 16299) を構成する

次の手順に従って、C++/WinRT プロジェクトから参照されている C# プロジェクト内で .NET Standard 2.0 または Windows ARM64 のサポートを有効にします。 

Visual Studio で、ソリューション エクスプローラーにアクセスし、*CppToCSharpWinRT* プロジェクトのショートカット メニューを開きます。  **[プロパティ]** を選択し、ユニバーサル Windows アプリの最小バージョンを **Windows 10 Fall Creators Update (10.0; Build 16299)** (またはそれ以降) に設定します。 *SampleComponent* プロジェクトについても同じ操作を行います。

Visual Studio で、*CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[プロジェクトのアンロード]** を選んで `CppToCSharpWinRT.vcxproj` をテキスト エディターで開きます。 

次の XML をコピーして、`CPPWinRTCSharpV2.vcxproj` の最初の `PropertyGroup` に貼り付けます。 

```xml
   <!-- Start Custom .NET Native properties -->
   <DotNetNativeVersion>2.2.9-rel-29512-01</DotNetNativeVersion>
   <DotNetNativeSharedLibary>2.2.8-rel-29512-01</DotNetNativeSharedLibary>
   <UWPCoreRuntimeSdkVersion>2.2.11</UWPCoreRuntimeSdkVersion>
   <!--<NugetPath>$(USERPROFILE)\.nuget\packages</NugetPath>-->
   <NugetPath>$(ProgramFiles)\Microsoft SDKs\UWPNuGetPackages</NugetPath>
   <!-- End Custom .NET Native properties -->
```

`DotNetNativeVersion`、`DotNetNativeSharedLibary`、および `UWPCoreRuntimeSdkVersion` の値は、Visual Studio のバージョンによって異なる可能性があります。  正しい値に設定するには、`%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages` を開き、次の表の各値のサブディレクトリを確認します。  たとえば、`%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` ディレクトリには `2.2.9-rel-29512-01` という名前のディレクトリがあります。

> | MSBuild 変数 | ディレクトリ | 例
> |-| - | -
> | DotNetNativeVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` | `2.2.9-rel-29512-01`
> | DotNetNativeSharedLibary | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.SharedLibrary` | `2.2.8-rel-29512-01`
> | UWPCoreRuntimeSdkVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.UWPCoreRuntimeSdk` | `2.2.11`

次に、最初の `PropertyGroup` の直後に以下を (変更なしで) 追加します。

```xml
  <!-- Start Custom .NET Native targets -->
  <!-- Import all of the .NET Native / CoreCLR props at the beginning of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.props" />
  <!-- End Custom .NET Native targets -->
```

プロジェクト ファイルの最後で、終了 `Project` タグの直前に以下を (変更なしで) 追加します。

```xml
  <!-- Import all of the .NET Native / CoreCLR targets at the end of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.targets" />
  <!-- End Custom .NET Native targets -->
```

Visual Studio 内でプロジェクト ファイルを再読み込みします。 これを行うには、Visual Studio ソリューション エクスプローラーで *CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[プロジェクトの再読み込み]** を選択します。

## <a name="building-for-net-native"></a>.NET ネイティブのビルド

.NET ネイティブ用に構築された C# コンポーネントを使用して、アプリケーションのビルドとテストを行うことをお勧めします。 Visual Studio で、*CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[プロジェクトのアンロード]** を選んで `CppToCSharpWinRT.vcxproj` をテキスト エディターで開きます。 

次に、C++ プロジェクト ファイル内のリリースおよび ARM64 構成で、`UseDotNetNativeToolchain` プロパティを `true` に設定します。

Visual Studio ソリューション エクスプローラーで *CppToCSharpWinRT* プロジェクトのショートカット メニューを開き、 **[プロジェクトの再読み込み]** を選択します。 

```xml
  <PropertyGroup Condition="'$(Configuration)'=='Release'" Label="Configuration">
...
    <UseDotNetNativeToolchain>true</UseDotNetNativeToolchain>
  </PropertyGroup>
  <PropertyGroup Condition="$(Platform)'='ARM64'" Label="Configuration">
    <UseDotNetNativeToolchain Condition="'$(UseDotNetNativeToolchain)'==''">true</UseDotNetNativeToolchain>
  </PropertyGroup>
```

## <a name="related-topics"></a>関連トピック
* [C# および Visual Basic を使用した Windows ランタイム コンポーネント](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic)
* [C++/WinRT を使用した Windows ランタイム コンポーネント](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)
