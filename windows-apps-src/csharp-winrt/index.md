---
description: C#/WinRT は、C# コードに対する WinRT プロジェクション サポートを提供するツール セットです。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C#, winrt, cswinrt, プロジェクション
ms.localizationpriority: medium
ms.openlocfilehash: 0704a7e9c731c6f60c59615b964b51e0ded242c2
ms.sourcegitcommit: 1022e8819e75484ca0cd94f8baf4f4d11900e0e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "98206091"
---
# <a name="cwinrt"></a>C#/WinRT

C#/WinRT は、C# 言語に対する Windows ランタイム (WinRT) プロジェクション サポートを提供する NuGet パッケージのツールキットです。 "*プロジェクション*" は、ターゲット言語に対して自然で使い慣れた方法での WinRT API のプログラミングを可能にする、相互運用機能アセンブリなどの変換レイヤーです。 たとえば、C#/WinRT プロジェクションは、C# と WinRT インターフェイス間の相互運用の詳細を隠し、多くの WinRT 型と、文字列、URI、共通値型、およびジェネリック コレクションなど、適切な .NET の同等物との間のマッピングを提供します。

C#/WinRT では現在、WinRT 型を使用するためのサポートを提供しており、最新バージョンを使用すると WinRT 相互運用機能アセンブリを[作成](#create-an-interop-assembly)および[参照](#reference-an-interop-assembly)できます。 C#/WinRT の今後のリリースでは、WinRT 型を C# で作成するためのサポートが追加される予定です。

C#/WinRT の詳細については、[C#/WinRT GitHub リポジトリ](https://aka.ms/cswinrt/repo)に関するページを参照してください。

## <a name="motivation-for-cwinrt"></a>C#/WinRT の動機

[.NET Core](/dotnet/core/) は、.NET プラットフォームの焦点であり、.NET 5 は最新のメジャー リリースです。 これは、デバイス、クラウド、IoT のアプリケーションを構築するために使用できる、オープンソースのクロスプラットフォーム ランタイムです。

以前のバージョンの .NET Framework と .NET Core には、Windows 固有のテクノロジである WinRT に関する知識が組み込まれていました。 .NET 5 の移植性と効率性の目標をサポートするために、WinRT プロジェクション サポートを .NET コンパイラとランタイムから取り出して、C# /WinRT ツールキットに移動しました。 C#/WinRT の目標は、以前のバージョンの C# コンパイラおよび .NET ランタイムで提供されている組み込みの WinRT サポートと同等の機能を提供することです。 詳細については、「[Windows ランタイム型の .NET マッピング](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)」を参照してください。

C#/WinRT では、WinUI 3.0 もサポートされています。 このリリースの WinUI は、ネイティブの Microsoft UI コントロールと機能をオペレーティング システムから取り出します。 これにより、アプリ開発者は、Windows 10 バージョン1803 以降のリリースで最新のコントロールとビジュアルを使用できます。

最後に、C#/WinRT は一般的なツールキットであり、 C# コンパイラまたは .NET ランタイムで WinRT の組み込みサポートが使用できない、他のシナリオをサポートすることを目的としています。

## <a name="create-an-interop-assembly"></a>相互運用機能アセンブリを作成する

WinRT API は、Windows メタデータ (*.winmd) ファイルに定義されています。 C#/WinRT NuGet パッケージ ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) には C#/WinRT コンパイラ、**cswinrt.exe** が含まれています。これを使用して、Windows メタデータ ファイルを処理し、.NET 5.0 C# コードを生成することができます。 これらのソース ファイルを相互運用機能アセンブリにコンパイルできます。これは、[C++/WinRT](../cpp-and-winrt-apis/index.md) が C++ 言語プロジェクションのヘッダーを生成する方法と同様です。 次に、アプリケーションで参照される C#/WinRT 相互運用機能アセンブリを、C#/WinRT ランタイム アセンブリと共に配布することができます。

NuGet パッケージとして相互運用機能アセンブリを作成および配布する方法を示すチュートリアルについては、[C++/WinRT コンポーネントからの .NET 5 プロジェクションの生成と NuGet の更新に関するチュートリアル](net-projection-from-cppwinrt-component.md)をご覧ください。

### <a name="invoke-cswinrtexe"></a>cswinrt.exe を呼び出す

プロジェクトから cswinrt.exe を呼び出すには、最新の [C#/WinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)をインストールします。 次いで、**C# クラス ライブラリ (.NET Core)** プロジェクトに C#/WinRT 固有のプロジェクト プロパティを設定して、相互運用機能アセンブリを生成できます。 次のプロジェクト フラグメントは、Contoso 名前空間で型のプロジェクション ソースを生成するための **cswinrt** の簡単な呼び出し示しています。 これらのソースは、プロジェクトのビルドに含まれます。

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

このプロジェクトでは、投影しようとしている CsWinRT NuGet パッケージとプロジェクト固有の .winmd ファイルを参照する必要がある場合もあります。これは、NuGet パッケージ、プロジェクト参照、直接参照のいずれを通して行っても構いません。 既定では、**Windows** および **Microsoft** 名前空間は投影されません。 CsWinRT プロジェクトのプロパティの完全な一覧は、[CsWinRT NuGet のドキュメント](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)で参照してください。

### <a name="distribute-the-interop-assembly"></a>相互運用機能アセンブリを配布する

相互運用機能アセンブリは通常、必須の C#/WinRT ランタイム アセンブリ **WinRT.Runtime.dll** のための C#/WinRT NutGet パッケージへの依存関係と共に、NuGet パッケージとして配布されます。

正しいバージョンの C#/WinRT ランタイムが .NET 5.0 アプリケーションに確実に配置されるようにするため、.nuspec ファイルに、C#/WinRT NuGet パッケージへの依存関係を指定した `targetFramework` 条件を含めてください。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0 のターゲット フレームワーク モニカーは、".NETCoreApp5.0" から "net5.0" に移行します。

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

## <a name="common-errors-and-troubleshooting"></a>一般的なエラーとトラブルシューティング

- エラー:"Windows メタデータが指定されていないか、検出されません。"

  Windows メタデータを指定するには、`<CsWinRTWindowsMetadata>` プロジェクト プロパティを使用します。次に例を示します。
  ```xml
  <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
  ```
  
- エラー CS0246:'Windows' という名前の型または名前空間が見つかりませんでした (using ディレクティブまたはアセンブリ参照が不足しています)

  このエラーに対処するには、特定の Windows バージョンを対象とするように `<TargetFramework>` プロパティを編集します。次に例を示します。
  ```xml
  <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
  ```
  `<TargetFramework>` プロパティの指定の詳細については、[Windows ランタイム API の呼び出し](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)に関するドキュメントを参照してください。


### <a name="net-sdk-versioning-errors"></a>.NET SDK のバージョン管理エラー

そのすべての依存関係より以前のバージョンの .NET SDK を使用してビルドされたプロジェクトで、次のエラーまたは警告が発生する可能性があります。

| エラーまたは警告のメッセージ | 理由 |
|--------------------------|--------|
| 警告 MSB3277: Found conflicts between different versions of WinRT.Runtime or Microsoft.Windows.SDK.NET that could not be resolved. (解決できなかった異なるバージョンの WinRT.Runtime または Microsoft.Windows.SDK.NET 間に競合が見つかりました。) | This build warning occurs when referencing a library that exposes Windows SDK types on its API surface. (このビルド警告は、その API サーフェスに Windows SDK 型が公開されるライブラリを参照するときに発生します。) |
| [エラー CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705):Assembly 'AssemblyName1' uses 'TypeName' which has a higher version than referenced assembly 'AssemblyName2' (アセンブリ 'AssemblyName1' は、参照されるアセンブリ 'AssemblyName2' よりも新しいバージョンを持つ 'TypeName' を使用します) | このビルド コンパイラ エラーは、ライブラリ内の公開された Windows SDK 型を参照および使用するときに発生します。 |
| System.IO.FileLoadException | このランタイム エラーは、Windows SDK 型が公開されないライブラリで特定の API を呼び出すと発生する可能性があります。 |

これらのエラーを修正するには、.NET SDK を最新バージョンに更新します。 これにより、お使いのアプリケーションで使用されるランタイムおよび Windows SDK アセンブリのバージョンとすべての依存関係との互換性が確実に保たれます。 これらのエラーは、.NET 5 SDK への早期サービスまたは機能更新プログラムの適用で発生する可能性があります。これは、ランタイムの修正に、アセンブリのバージョンに対する更新プログラムが必要になる場合があるためです。

## <a name="known-issues"></a>既知の問題

既知の問題と破壊的変更については、[C#/WinRT の GitHub リポジトリ](https://aka.ms/cswinrt/repo)を参照してください。

C#/WinRT NuGet パッケージ、cswinrt.exe コンパイラ、または生成されたプロジェクション ソースで機能上の問題が発生した場合は、[C#/WinRT の問題のページ](https://github.com/microsoft/CsWinRT/issues)から問題を送信してください。

## <a name="additional-resources"></a>その他の資料

* [C#/WinRT GitHub リポジトリ](https://aka.ms/cswinrt/repo)
