---
title: Windows UI ライブラリ (WinUI)
description: Windows アプリ開発用の WinUI ライブラリ。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, ツールキット sdk, winui, Windows UI ライブラリ
ms.openlocfilehash: eb87744ed5d3eb5882b4ebae75b8dcf295d89f10
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166756"
---
# <a name="windows-ui-library-winui"></a>Windows UI ライブラリ (WinUI)

![WinUI ロゴ](../images/logo-winui.png)

Windows UI ライブラリ (WinUI) は、Windows デスクトップ アプリケーションと UWP アプリケーションの両方に対応したネイティブ ユーザー エクスペリエンス (UX) フレームワークです。

WinUI では、すべてのエクスペリエンス、コントロール、およびスタイルに [Fluent Design システム](https://www.microsoft.com/design/fluent/#/)を採用することにより、最新のユーザー インターフェイス (UI) パターンを使用した、一貫性のある直感的でアクセスしやすいエクスペリエンスが提供されます。

デスクトップ アプリと UWP アプリの両方をサポートしているため、最初から WinUI を使用してアプリを構築できるほか、既存の MFC アプリ、WinForms アプリ、または WPF アプリを、C++、C#、Visual Basic、Javascript などの使い慣れた言語で ([React Native for Windows](https://microsoft.github.io/react-native-windows/) を使用して) 徐々に移行することもできます。

> [!Important]
> WinUI には、2 つのバージョンがあります。**WinUI 2.x** と **WinUI 3** です。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x ライブラリ

WinUI 2.x は UWP アプリケーションで使用でき、[XAML Islands](../desktop/modernize/xaml-islands.md) を使用して、新規または既存のデスクトップ アプリケーションに組み込むことができます。

> [!NOTE]
> WinUI 2.x の最新のリリースは WinUI 2.4 です。 次のリリースで計画されている作業の一覧については、[WinUI 2.5 のマイルストーン](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)を参照してください。

WinUI 2.x ライブラリは [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) と密接に結び付けられており、UWP アプリ用の公式のネイティブ Windows UI コントロールとその他の UI 要素を提供します。

以前のバージョンの Windows 10 との下位互換性が維持されるため、WinUI 2.x のコントロールはユーザーが最新の OS を使用していない場合でも動作します。

![WinUI 2.x プラットフォームのサポート](../images/platforms-winui2.png)

**インストール手順については、「[Windows UI ライブラリの使用を開始する](winui2/getting-started.md)」を参照してください。**

### <a name="related-links-for-winui-2x"></a>WinUI 2.x の関連リンク

- [WinUI 2.x ライブラリの概要](winui2/index.md)
- [API ドキュメント](/uwp/api/overview/winui/)
- [ソース コード](https://aka.ms/winui)
- [XAML Controls Gallery アプリ](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-3-library-preview-2"></a>Windows UI 3 ライブラリ (Preview 2)

WinUI 3 は WinUI の次期バージョンであり、[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) から完全に切り離されたネイティブ Windows 10 UI プラットフォームです。

> [!Important]
> この WinUI 3 Preview リリースは、早期評価と、開発者コミュニティからのフィードバックの収集を目的としています。 実稼働アプリには使用**できません**。
>
> WinUI 3 Preview のリリースは 2020 年から 2021 年前半にかけて継続され、その後、最初の公式リリースが利用可能になる予定です。
>
> [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、フィードバックを提供し、提案と問題をログに記録してください。

XAML、コンポジション、および入力 API を [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) から 完全に分離することにより、WinUI 3 には、Windows 10 のネイティブ UI プラットフォーム全体が含まれます。

WinUI は、すべての Windows アプリへのパスです。ネイティブの UWP アプリまたは Win32 アプリの UI レイヤーとして使用したり、[XAML Islands](../desktop/modernize/xaml-islands.md) で使用してデスクトップ アプリを少しずつ最新化したりできます。

XAML のすべての新機能は、最終的には WinUI の一部として同梱されます。 OS の一部として同梱されている既存の UWP XAML API に対して、新しい機能更新プログラムが提供されることはなくなります。 ただし、Windows 10 のサポート ライフサイクルに従って、セキュリティ更新プログラムと重要な修正プログラムが引き続き提供されます。

ユニバーサル Windows プラットフォームには XAML フレームワークだけが含まれているわけではないことに注意してください。 アプリケーション モデルとセキュリティ モデル、メディア パイプライン、Xbox と Windows 10 シェルの統合、多様なデバイスとの互換性などの機能の開発とサポートは、引き続き行われます。

![WinUI 3 プラットフォームのサポート](../images/platforms-winui3.png)

### <a name="related-links-for-winui-3"></a>WinUI 3 の関連リンク

- [Windows UI ライブラリ 3 Preview 2 (2020 年 7 月)](winui3/index.md)
- [XAML コントロール ギャラリー (WinUI 3 Preview 2) アプリ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)

## <a name="winui-resources"></a>WinUI リソース

**GitHub**:WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [WinUI リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、機能リクエストやバグ報告を行ったり、WinUI チームと対話したりできます。WinUI 3 以降のチームの計画を[ロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)で確認することもできます。

**Web サイト**:[WinUI Web サイト](https://aka.ms/winui)には、製品の比較と WinUI の利点に関する説明があり、製品と製品チームの最新の動向を常にチェックするために役立ちます。