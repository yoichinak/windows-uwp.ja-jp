---
description: C#/WinRT は、C# コードに対する WinRT プロジェクション サポートを提供するツール セットです。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C#, winrt, cswinrt, プロジェクション
ms.localizationpriority: medium
ms.openlocfilehash: 8fb098cb247890dc1b3919f6123b76b54366d60f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154326"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT はパブリック プレビュー段階であり、最終リリースの前に大幅に変更される可能性があります。 本書に記載された情報について、Microsoft は明示または黙示を問わずいかなる保証をするものでもありません。

C#/WinRT は、C# 言語に対する Windows ランタイム (WinRT) プロジェクション サポートを提供する NuGet パッケージのツールキットです。 "*プロジェクション*" は、ターゲット言語に対して自然で使い慣れた方法での WinRT API のプログラミングを可能にする、相互運用機能アセンブリなどの変換レイヤーです。 たとえば、C#/WinRT プロジェクションは、C# と WinRT インターフェイス間の相互運用の詳細を隠し、多くの WinRT 型と、文字列、URI、共通値型、およびジェネリック コレクションなど、適切な .NET の同等物との間のマッピングを提供します。

C#/WinRT は現在、WinRT 型を使用するためのサポートを提供しており、現在のプレビューを使用すると WinRT 相互運用機能アセンブリを[作成](#create-an-interop-assembly)および[参照](#reference-an-interop-assembly)できます。 C#/WinRT の今後のリリースでは、WinRT 型を C# で作成するためのサポートが追加される予定です。

## <a name="motivation-for-cwinrt"></a>C#/WinRT の動機

[.NET Core](/dotnet/core/) は、.NET プラットフォームの焦点であり、.NET 5 は次のメジャー リリースです。 これは、デバイス、クラウド、IoT のアプリケーションを構築するために使用できる、オープンソースのクロスプラットフォーム ランタイムです。

以前のバージョンの .NET Framework と .NET Core には、Windows 固有のテクノロジである WinRT に関する知識が組み込まれていました。 .NET 5 の移植性と効率性の目標をサポートするために、WinRT プロジェクション サポートを .NET コンパイラとランタイムから取り出して、C# /WinRT ツールキットに移動しました。 C#/WinRT の目標は、以前のバージョンの C# コンパイラおよび .NET ランタイムで提供されている組み込みの WinRT サポートと同等の機能を提供することです。 詳細については、「[Windows ランタイム型の .NET マッピング](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)」を参照してください。

C#/WinRT では、WinUI 3.0 もサポートされています。 このリリースの WinUI は、ネイティブの Microsoft UI コントロールと機能をオペレーティング システムから取り出します。 これにより、アプリ開発者は、Windows 10 バージョン1803 以降のリリースで最新のコントロールとビジュアルを使用できます。

最後に、C#/WinRT は一般的なツールキットであり、 C# コンパイラまたは .NET ランタイムで WinRT の組み込みサポートが使用できない、他のシナリオをサポートすることを目的としています。 C#/WinRT は、Mono 5.4 など、.NET Standard 2.0 まで互換性がある .NET ランタイムのバージョンをサポートしています。

C#/WinRT の詳細については、[C#/WinRT GitHub リポジトリ](https://aka.ms/cswinrt/repo)に関するページを参照してください。

## <a name="create-an-interop-assembly"></a>相互運用機能アセンブリを作成する

WinRT API は、Windows メタデータ (*.winmd) ファイルに定義されています。 C#/WinRT NuGet パッケージには C#/WinRT コンパイラの **cswinrt** が含まれています。これを使用して、Windows メタデータ ファイルを処理し、.NET Standard 2.0 C# コードを生成することができます。 これらのソース ファイルを相互運用機能アセンブリにコンパイルできます。これは、[C++/WinRT](../cpp-and-winrt-apis/index.md) が C++ 言語プロジェクションのヘッダーを生成する方法と同様です。 次に、アプリケーションで参照される C#/WinRT 相互運用機能アセンブリを、C#/WinRT ランタイム アセンブリと共に配布することができます。

### <a name="invoke-cswinrtexe"></a>cswinrt.exe を呼び出す

コマンド ライン オプションを表示するには、`cswinrt -?` を実行します。 プロジェクトから cswinrt.exe を呼び出すには、Directory.Build.Targets ファイルを使用することをお勧めします。 次のプロジェクト フラグメントは、Contoso 名前空間で型のプロジェクション ソースを生成するための **cswinrt** の簡単な呼び出し示しています。 これらのソースは、プロジェクトのビルドに含まれます。

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>相互運用機能アセンブリを配布する

相互運用機能アセンブリは通常、必要な C#/WinRT ランタイム アセンブリ **winrt.runtime.dll** のための C#/WinRT NutGet パッケージへの依存関係と共に、NuGet パッケージとして配布されます。 C#/WinRT ランタイム アセンブリには、.NET Standard 2.0 をターゲットとするものと .NET 5.0 をターゲットとするものの 2 つのバージョンがあります。 アプリケーションのターゲット フレームワークに応じて、このうちの 1 つだけがデプロイされます。 

* .NET Standard 2.0 をターゲットにすることは、Windows でライトアップ機能を提供するダウンレベルのクロスプラットフォーム アプリケーションに適しています。
* .NET 5.0 をターゲットにすることは、XAML アプリなど、ネイティブ オブジェクト参照全体で正しいガーベッジ コレクションを必要とする最新の Windows アプリで推奨されます。

相互運用機能アセンブリは、nuspec ファイルに `targetFramework` 条件を含めることで、アプリに対して正しいバージョンの C#/WinRT ランタイムが確実にデプロイされるようにすることができます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0 のターゲット フレームワーク モニカーは、".NETCoreApp5.0" から ".NET5.0" に移行します。 C#/WinRT プレリリースでは、どちらか一方が使用されます。

## <a name="reference-an-interop-assembly"></a>相互運用機能アセンブリを参照する

通常、C#/WinRT 相互運用機能アセンブリは、アプリケーション プロジェクトによって参照されます。 ただし、それらは中間相互運用機能アセンブリによっても参照される場合があります。 たとえば、WinUI 相互運用機能アセンブリは Windows SDK 相互運用機能アセンブリを参照します。

投影された C#/WinRT 型を使用するには、適切な C#/WinRT 相互運用機能 NuGet パッケージへの参照をプロジェクトに追加します。 これにより、相互運用機能アセンブリと C#/WinRT ランタイム アセンブリの両方がプロジェクトに追加されます。

公式の相互運用機能アセンブリを含めずにサードパーティの WinRT コンポーネントを配布する場合、アプリケーション プロジェクトでは、[相互運用機能アセンブリの作成](#create-an-interop-assembly)手順に従って、独自のプライベート プロジェクション ソースを生成することができます。 この方法は、プロセス内に同じ型の競合するプロジェクションを生成する可能性があるため、推奨されません。 NuGet のパッケージングは、[セマンティックのバージョン管理](https://semver.org)スキームに従って、これを防ぐように設計されています。 公式のサードパーティ製相互運用機能アセンブリをお勧めします。

### <a name="winrt-type-activation"></a>WinRT 型のアクティブ化

C#/WinRT は、オペレーティング システムでホストされている WinRT 型、および [Win2D](https://www.nuget.org/packages/Win2D.uwp/) などのサードパーティ コンポーネントのアクティブ化をサポートしています。 デスクトップ アプリケーションでのサードパーティ コンポーネントのアクティブ化に関するサポートは、Windows 10 バージョン 1903 以降で利用可能な[登録無料の WinRT のアクティブ化](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/)を使用して有効化されます。 コンポーネントが UWP アプリをターゲットとして構築された場合は、[VCRT Forwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) パッケージの使用も必要になる場合があります。

Windows が前述の型のアクティブ化に失敗した場合、C#/WinRT にはアクティブ化のフォールバック パスも用意されています。 この場合、C#/WinRT は、完全修飾型名に基づき、要素を段階的に削除して、ネイティブ実装 DLL を見つけようとします。 たとえば、フォールバック ロジックは、次のモジュールから順番に Contoso.Controls.Wiget 型のアクティブ化を試行します。

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT は、[LoadLibrary 代替検索順序](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications)を使用して実装 DLL を見つけます。 このフォールバック動作に依存するアプリは、アプリ モジュールと共に実装 DLL をパッケージする必要があります。

## <a name="known-issues"></a>既知の問題

C#/WinRT の現在のプレビューには、相互運用に関連するパフォーマンス上の既知の問題がいくつかあります。 これらは、2020 年後半の最終リリース前に対処されます。

C#/WinRT NuGet パッケージ、cswinrt.exe コンパイラ、または生成されたプロジェクション ソースで機能上の問題が発生した場合は、[C#/WinRT の問題のページ](https://github.com/microsoft/CsWinRT/issues)から問題を送信してください。

## <a name="additional-resources"></a>その他の資料

* [C#/WinRT GitHub リポジトリ](https://aka.ms/cswinrt/repo)