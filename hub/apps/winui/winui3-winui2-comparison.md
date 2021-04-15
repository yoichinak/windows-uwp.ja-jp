---
title: WinUI 3 と WinUI 2 の比較
description: WinUI 3 と WinUI 2 の比較早見表。
ms.topic: article
ms.date: 03/19/2021
keywords: windows 10, uwp, ツールキット sdk, winui, Windows UI ライブラリ
ms.openlocfilehash: 8c415b3c08e0f7cd215cd99f2c54fc3cb760d3e0
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730680"
---
# <a name="comparison-of-winui-3-and-winui-2"></a>WinUI 3 と WinUI 2 の比較

:::image type="content" source="../images/logo-winui2-vs-winui3.png" alt-text="Win UI 2 のロゴと Win UI 3 のロゴ":::

現時点では、Windows UI ライブラリ (WinUI) に、WinUI 2 と WinUI 3 という異なる 2 つの世代が存在します。 どちらも最新の [Fluent Design System](https://www.microsoft.com/design/fluent) の原則とプロセスに基づいてユーザー インターフェイスとエクスペリエンスを提供しますが、それぞれの開発ターゲットは異なっており、スコープも開発トラックやリリース スケジュールの点で異なっています。

[WinUI 2](winui2/index.md) と [WinUI 3](winui3/index.md) はどちらも、Windows 10 での実稼働可能なアプリ開発をサポートしており、どちらも C# や C++/Win32 を使用したアプリのビルドをサポートしています。

どちらも、更新プログラムが定期的にリリースされており、アクティブに開発がなされています。

## <a name="the-major-differences"></a>主な相違点

| WinUI 3                                                                                                                                                                                                                                                                                                        | WinUI 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| UX スタックとコントロール ライブラリは、OS および [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) から完全に分離されています。これには、その UX スタックのコア フレームワーク、コンポジション、入力レイヤーが含まれます。                                                                        | UX スタックとコントロール ライブラリは、OS および [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) と密接に結び付いています。                                                                                                                                                                                                                                                                                                                                                          |
| WinUI 3 は、実稼働可能な Windows **デスクトップ/Win32** アプリのビルドに使用できます。 | WinUI 2 は、Windows **デスクトップ/Win32** アプリのビルドに使用できません。 |
| WinUI 3 は、[Project Reunion](../project-reunion/index.md) フレームワーク パッケージのコンポーネントとして、Project Reunion Visual Studio 拡張機能 (VSIX) の Visual Studio プロジェクト テンプレートと共に提供されます。 | WinUI 2 の一部は、オペレーティング システム自体 (UWP WinRT API の Windows.UI.* ファミリ) に含まれており、別の一部は、追加のコントロール、要素、およびオペレーティング システム自体に既に含まれているものに追加される最新のスタイルと共に、ライブラリ (“Windows UI Library 2”) として提供されます。 WinUI 2 では、これらの機能はダウンロード可能な NuGet パッケージに付属しています。 ただし、コア XAML フレームワーク、入力層、コンポジション層などの、UI スタックの他の重要な部分は、引き続き OS に組み込まれています。 |
| WinUI 3 では、デスクトップ アプリ用の C#/.NET 5 をサポートしています。 | WinUI 2 では、C#/.NET ネイティブ アプリのみをサポートしています。 |
| WinUI 3 での実稼働可能な UWP アプリのサポートは、現在はプレビュー段階です。[WinUI 3 - Project Reunion 0.5 Preview](winui3/release-notes/winui3-project-reunion-0.5-preview.md) に関するページを参照してください。                                                                                                                                | WinUI 2 は、新規または既存の UWP プロジェクトに NuGet パッケージをインストールすることにより、実稼働 UWP アプリに組み込むことができます。 そうすることで、WinUI コントロールとスタイルを新しいアプリで直接参照できるようになります。または、既存のアプリの "microsoft.ui." に対する "windows.ui." 名前空間参照を更新することによって 参照することもできます。                                                                                                                                                                                    |
| WinUI 3 では、Chromium ベースの [WebView2](/microsoft-edge/webview2/) コントロールをサポートします |  WinUI 2 では、[WebView](/windows/uwp/design/controls-and-patterns/web-view) コントロールをサポートします |
| WinUI 3 では、Windows 10 October 2018 Update (バージョン 1809、OS ビルド 17763) へのダウンレベルに対応します。 | WinUI 2 では、Windows 10 Creators Update (バージョン 1703、OS ビルド 15063) へのダウンレベルに対応します。 |

## <a name="see-also"></a>関連項目

- [Project Reunion 0.5 を使用してデスクトップ Windows アプリをビルドする](../project-reunion/index.md)

- [Windows UI ライブラリ (WinUI)](index.md)

- [Windows UI ライブラリ 2.x](winui2/index.md)

- [Windows UI ライブラリ 3 - Project Reunion 0.5 (2021 年 3 月)](winui3/index.md)
