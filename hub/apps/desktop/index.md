---
Description: Windows PC 用のデスクトップ アプリの開発を開始する方法を説明します。これには、新しいアプリ用の適切なアプリ プラットフォームを選択する方法、Windows 10 用の既存のアプリを現代化する方法も含まれます。
title: Windows PC 用のデスクトップ アプリの構築
ms.topic: article
ms.date: 11/04/2019
keywords: windows win32, デスクトップ開発
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5a55a1578fba7052396ecec725db2678ea8bd6ff
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580019"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Windows PC 用のデスクトップ アプリの構築

この記事では、Windows 用のデスクトップ アプリの構築を開始するために必要な情報、または既存のデスクトップ アプリを更新して Windows 10 で最新のエクスペリエンスを導入するために必要な情報を提供します。

## <a name="platforms-for-desktop-apps"></a>デスクトップ アプリのプラットフォーム

Windows PC 用のデスクトップ アプリを構築するプラットフォームは、主に 4 つあります。 各プラットフォームには、アプリのライフサイクル、UI コントロールの完全なセット、Windows 機能を使用するための包括的な一連のマネージド API またはネイティブ API へのアクセスを定義するアプリ モデルが用意されています。

次の表は、プラットフォームについて説明しています。 これらのプラットフォームと、各プラットフォームのその他のリソースの詳細な比較については、「[アプリのプラットフォームを選択する](choose-your-platform.md)」を参照してください。

<br/>

<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>プラットフォーム</th>
<th>説明</th>
<th>ドキュメントとリソース</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://docs.microsoft.com/windows/uwp/">ユニバーサル Windows プラットフォーム (UWP)</a></td>
<td><p>Windows 10 のアプリおよびゲーム用の最先端のプラットフォームです。 UWP コントロールと API を排他的に使用する UWP アプリをビルドすることも、他のプラットフォームのいずれかを使用して構築されたデスクトップ アプリで UWP コントロールと API を使用することもできます。</p></td>
<td><a href="/windows/uwp/get-started/">作業開始</a><br/><a href="/uwp/">API リファレンス</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">サンプル</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>Windows とハードウェアへの直接アクセスを必要とするネイティブ C/C++ の Windows アプリのために用意されたプラットフォームです。</p></td>
<td><a href="/windows/win32/desktop-programming/">作業開始</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API リファレンス</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">サンプル</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>XAML UI モデルを使用して多彩なグラフィックで管理される Windows アプリ用に構築された .NET ベースのプラットフォームです。 これらのアプリは、<a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> または完全な .NET Framework を対象にすることができます。</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">作業開始</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API リファレンス (.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">サンプル</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows フォーム</a></td>
<td><p>軽量の UI モデルを使用して管理される基幹業務アプリ用に設計された .NET ベースのプラットフォームです。 これらのアプリは、<a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> または完全な .NET Framework を対象にすることができます。</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">作業開始</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API リファレンス (.NET)</a><br/><a href="https://code.msdn.microsoft.com/windowsdesktop/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=Windows%20Forms">サンプル</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> これらのアプリケーション プラットフォームは、一連の UI フレームワークおよび UI コントロールを備えています。従来の Windows デスクトップで実行される Word、Excel、Photoshop などのデスクトップ アプリを作成し、その環境固有の機能を最大限に活用できます。 Windows 10 では、この各プラットフォームで Windows UI (WinUI) ライブラリ を使用したユーザー インターフェイスの作成がサポートされています。 デスクトップ アプリ用の WinUI の詳細については、[こちらのセクション](choose-your-platform.md#windows-ui-library)を参照してください。

## <a name="update-existing-desktop-apps-for-windows-10"></a>Windows 10 用に既存のデスクトップ アプリを更新する

既存の WPF、Windows フォーム、またはネイティブの Win32 デスクトップ アプリをお持ちの場合は、Windows 10 とユニバーサル Windows プラットフォーム (UWP) に用意された多数の機能を使用して、アプリでモダンなエクスペリエンスを実現できます。 これらのほとんどの機能は、独自のペースでアプリに採用できるモジュラー コンポーネントとして利用できます。別のプラットフォーム用にアプリを書き換える必要がありません。

次に示すのは、既存のデスクトップ アプリを強化するために使用できる機能のほんの一部です。

* デスクトップ アプリをパッケージ化して展開するには、[MSIX](/windows/msix/) を使用します。 MSIX は、すべての Windows アプリ用の汎用パッケージ化エクスペリエンスを提供するモダンな Windows アプリ パッケージ形式です。 MSIX は、MSI、.appx、App-V および ClickOnce インストール テクノロジの優れた部分が組み合わされ、安全かつセキュアで信頼性の高いものとなるように構築されています。
* デスクトップ アプリと Windows 10 のエクスペリエンスを統合するには、[パッケージ拡張](/windows/apps/desktop/modernize/desktop-to-uwp-extensions)を使用します。 たとえば、アプリの開始タイルをポイントしたり、アプリを共有先にしたり、アプリからトースト通知を送信したりできます。
* デスクトップ アプリで UWP XAML コントロールをホストするには、[XAML Islands](/windows/apps/desktop/modernize/xaml-islands) を使用します。 最新の Windows 10 の UI 機能の多くは、UWP XAML コントロールでのみ使用できます。

詳しくは、次の記事をご覧ください。

<br/>

| アーティクル | 説明 |
|---------|-------------|
| [デスクトップ アプリの現代化](/windows/apps/desktop/modernize) | WPF、Windows フォーム、C++ Win32 アプリなどの、任意のデスクトップ アプリで使用できる最新の Windows 10 および UWP 開発機能について説明します。 |
| [チュートリアル: WPF アプリの現代化](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | UWP インクと予定表のコントロールをアプリに追加し、MSIX パッケージにパッケージ化することで既存の WPF の基幹業務サンプル アプリを現代化する手順について説明します。  |

## <a name="create-new-desktop-apps"></a>新しいデスクトップ アプリを作成する

Windows 用の新しいデスクトップ アプリを作成する場合の、作業の開始に役立つリソースを次に示します。

<br/>

| アーティクル | 説明 |
|---------|-------------|
| [アプリ プラットフォームの選択](choose-your-platform.md) | 主要なデスクトップ アプリ プラットフォームの詳細な比較が提供されるため、ニーズに適したプラットフォームを選択するのに役立ちます。 この記事では、各プラットフォームのドキュメントへの便利なリンクも提供します。 |
| [デスクトップ アプリの現代化](/windows/apps/desktop/modernize) | WPF、Windows フォーム、C++ Win32 アプリなどの、任意のデスクトップ アプリで使用できる最新の Windows 10 および UWP 開発機能について説明します。 |
| [機能とテクノロジ](/windows/apps/features-and-technologies) | 主要なデスクトップ アプリ プラットフォームを通じてアクセスできる Windows 機能の概要と、関連ドキュメントへのリンクを示します。 |

## <a name="related-documentation-and-technologies"></a>関連ドキュメントとテクノロジ

| リソース | 説明 |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | .NET Core 3.0 の最新機能について説明します。これには、WPF と Windows フォーム アプリの機能強化が含まれます。 |
| [WPF および .NET Core 3.0 用のデスクトップ ガイド](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | 完全な .NET Framework ではなく、.NET Core 3.0 を対象とする WPF アプリを開発します。  |
| [Azure](https://docs.microsoft.com/azure/) | Azure クラウド サービスを使用してアプリの公開範囲を拡張します。 |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | Visual Studio を使用してアプリとサービスを開発する方法について説明します。 |
| [MSIX](https://docs.microsoft.com/windows/msix/) | Windows アプリをパッケージ化して、最新の汎用パッケージ化形式で展開します。 |
| [Windows AI](https://docs.microsoft.com/windows/ai/) | Windows AI を使用して、アプリの複雑な問題に対するインテリジェントなソリューションを構築します。 |
| [Windows コンテナー](https://docs.microsoft.com/virtualization/windowscontainers/) | アプリケーションとその依存関係を、完全に分離された高速な Windows 環境でパッケージ化します。 |
| [プログレッシブ Web アプリ](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | Web アプリを、Windows 10 で UWP アプリとして配布して実行できるプログレッシブ Web アプリに変換します。 |
| [Xamarin](https://docs.microsoft.com/xamarin/) | .NET コードおよびプラットフォーム固有のユーザー インターフェイスを使用して、Windows、Android、iOS、および macOS 用のクロスプラットフォーム アプリを構築します。 |
| [Windows 8.x 以前のドキュメント アーカイブ](https://docs.microsoft.com/previous-versions/windows/) | Windows 8.x およびそれ以前のバージョン用のアプリの構築に関するアーカイブ ドキュメントにアクセスします。 |
