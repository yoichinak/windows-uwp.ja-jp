---
title: Windows UI ライブラリ (WinUI)
description: Windows アプリ開発用の WinUI ライブラリ。
ms.topic: article
ms.date: 03/19/2021
keywords: windows 10, uwp, ツールキット sdk, winui, Windows UI ライブラリ
ms.openlocfilehash: 20f99f5e747d95b4ec0e806976393e209d3c2582
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730366"
---
# <a name="windows-ui-library-winui"></a>Windows UI ライブラリ (WinUI)

![WinUI ロゴ](../images/logo-winui.png)

Windows UI ライブラリ (WinUI) は、Windows デスクトップと UWP の両方のアプリケーションに対応したネイティブ ユーザー エクスペリエンス (UX) フレームワークです。

WinUI では、すべてのエクスペリエンス、コントロール、およびスタイルに [Fluent Design システム](https://www.microsoft.com/design/fluent/#/)を採用することにより、最新のユーザー インターフェイス (UI) パターンを使用した、一貫性のある直感的でアクセスしやすいエクスペリエンスが提供されます。

デスクトップと UWP の両方のアプリをサポートしているため、最初から WinUI を使用して構築できるほか、既存の MFC、WinForms、または WPF のアプリを、C++、C#、Visual Basic、Javascript などの使い慣れた言語で ([React Native for Windows](https://microsoft.github.io/react-native-windows/) を使用して) 徐々に移行することもできます。

> [!Important]
> WinUI には、2 つのバージョンがあります。**WinUI 2.x** と **WinUI 3** です。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x ライブラリ

WinUI 2.x は UWP アプリケーションで使用でき、[XAML Islands](../desktop/modernize/xaml-islands.md) を使用して、新規または既存のデスクトップ アプリケーションに組み込むことができます。

> [!NOTE]
> WinUI 2.x の最新のリリースは WinUI 2.5 です。 次のリリースで計画されている作業の一覧については、[WinUI 2.6 のマイルストーン](https://github.com/microsoft/microsoft-ui-xaml/milestone/11)を参照してください。

WinUI 2.x ライブラリは [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) と密接に結び付けられており、UWP アプリ用の公式のネイティブ Windows UI コントロールとその他の UI 要素を提供します。

以前のバージョンの Windows 10 との下位互換性が維持されるため、WinUI 2.x のコントロールはユーザーが最新の OS を使用していない場合でも動作します。

![WinUI 2.x プラットフォームのサポート](../images/platforms-winui2.png)

**インストール手順については、「[Windows UI ライブラリの使用を開始する](winui2/getting-started.md)」を参照してください。**

### <a name="related-links-for-winui-2x"></a>WinUI 2.x の関連リンク

- [WinUI 2.x ライブラリの概要](winui2/index.md)
- [API ドキュメント](/windows/winui/api/)
- [ソース コード](https://aka.ms/winui)
- [XAML Controls Gallery アプリ](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-project-reunion-05"></a>Windows UI 3 ライブラリ (Project Reunion 0.5)

WinUI 3 は WinUI の次期バージョンであり、[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) から完全に切り離されたネイティブ Windows 10 UI プラットフォームです。

> [!Important]
> WinUI 3 - Project Reunion 0.5 は、安定した、サポートされている最初のバージョンの WinUI 3 です。 このバージョンの WinUI 3 では、実稼働アプリを作成して、Microsoft Store に発行することができます。
>
> [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、フィードバックを提供し、提案と問題をログに記録してください。

XAML、コンポジション、および入力 API を [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) から 完全に分離することにより、WinUI 3 には、Windows 10 のネイティブ UI プラットフォーム全体が含まれます。

WinUI 3 は [Project Reunion](../project-reunion/index.md) のコンポーネントであり、提供される API とツールの統合セットは、対象の幅広い Windows 10 OS バージョン上のどのデスクトップ アプリからでも一貫した方法で使用できます。 Project Reunion のコンポーネントとして、WinUI 3 は Project Reunion パッケージに含まれます。詳細については、[Windows UI ライブラリ 3 - Project Reunion 0.5](winui3/index.md) に関する記事を参照してください。

WinUI 3 は、すべての Windows アプリがこれから進む道です。ネイティブの UWP アプリや Win32 アプリの UI レイヤーとして使用することも、[XAML Islands](../desktop/modernize/xaml-islands.md) を使用してデスクトップ アプリを少しずつ最新化することもできます。

XAML のすべての新機能は、最終的には WinUI の一部として同梱されます。 OS の一部として同梱されている既存の UWP XAML API に対して、新しい機能更新プログラムが提供されることはなくなります。 ただし、Windows 10 のサポート ライフサイクルに従って、セキュリティ更新プログラムと重要な修正プログラムが引き続き提供されます。

ユニバーサル Windows プラットフォームには XAML フレームワークだけが含まれているわけではないことに注意してください。 アプリケーション モデルとセキュリティ モデル、メディア パイプライン、Xbox と Windows 10 シェルの統合、多様なデバイスとの互換性などの機能の開発とサポートは、引き続き行われます。

![WinUI 3 プラットフォームのサポート](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>WinUI 3 の関連リンク

- [Windows UI ライブラリ 3 - Project Reunion 0.5](winui3/index.md)
- [WinUI 3 コントロール ギャラリーのサンプル アプリ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3)

## <a name="winui-resources"></a>WinUI リソース

**GitHub**:WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [WinUI リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、機能リクエストやバグ報告を行ったり、WinUI チームと対話したりできます。WinUI 3 以降のチームの計画を[ロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)で確認することもできます。

**Web サイト**: [WinUI Web サイト](https://aka.ms/winui)には、製品の比較と WinUI の利点に関する説明があり、製品と製品チームの最新の動向を常にチェックできます。