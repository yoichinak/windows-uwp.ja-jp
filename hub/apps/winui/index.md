---
title: Windows UI ライブラリ (WinUI)
description: Windows アプリ開発用の WinUI ライブラリ。
ms.topic: article
ms.date: 05/11/2020
keywords: windows 10, uwp, ツールキット sdk, winui, Windows UI ライブラリ
ms.openlocfilehash: 2afa6b1eadc98300e3de76a1dfc6ede66a2a56e5
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580179"
---
# <a name="windows-ui-library-winui"></a>Windows UI ライブラリ (WinUI)

![ツールキットのヒーロー イメージ](../images/logo-winui.png)

Windows UI ライブラリ (WinUI) は、Win32 と UWP の両方における Windows アプリ用の最新のネイティブ ユーザー インターフェイス (UI) プラットフォームです。

WinUI は、すべてのコントロールとスタイルに [Fluent Design システム](https://www.microsoft.com/design/fluent/#/)を採用することにより、最新の UI パターンを使用した、一貫性のある直感的でアクセスしやすいエクスペリエンスを提供します。

また、Win32 アプリと UWP アプリの両方をサポートしているため、最初から WinUI を使用してアプリを構築できるほか、既存の MFC アプリ、WinForms アプリ、または WPF アプリを、自分のペースで C++、C#、Visual Basic、Javascript などの使い慣れた言語で [React Native for Windows](https://microsoft.github.io/react-native-windows/) を使って移行することもできます。

WinUI には、**winui 2.x** と **WinUI 3.0** という 2 つのバージョンがあることに注意してください。

## <a name="windows-ui-2x-library"></a>Windows UI 2.x ライブラリ

現在、WinUI 2.x は UWP アプリケーションで使用されており、[XAML Islands](/windows/apps/desktop/modernize/xaml-islands) を使って既存のデスクトップ アプリケーションに組み込むことができます。

WinUI 2.x ライブラリは UWP SDK と密接に結び付けられており、UWP アプリ用の公式のネイティブ Windows UI コントロールやその他の UI 要素を提供します。

以前のバージョンの Windows 10 との下位互換性が維持されるため、WinUI 2.x のコントロールはユーザーが最新の OS を使用していない場合でも動作します。

![WinUI 2.x プラットフォームのサポート](../images/platforms-winui2.png)

> [!NOTE]
> WinUI 2.x の最新のリリースは WinUI 2.4 です。 次のリリースで計画されている作業の一覧については、[WinUI 2.5 のマイルストーン](https://github.com/microsoft/microsoft-ui-xaml/milestone/10)を参照してください。

インストール手順については、「[Windows UI ライブラリの使用を開始する](winui2/getting-started.md)」を参照してください。

### <a name="related-links"></a>関連リンク

- [WinUI 2.x ライブラリの概要](winui2/index.md)
- [API ドキュメント](https://docs.microsoft.com/uwp/api/overview/winui/)
- [ソース コード](https://aka.ms/winui)
- [XAML Controls Gallery アプリ](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

## <a name="windows-ui-30-library-preview-1"></a>Windows UI 3.0 ライブラリ (Preview 1)

WinUI 3 は WinUI の時期バージョンで、UWP SDK から完全に切り離されたネイティブ Windows 10 UI プラットフォームです。

Windows UI 3.0 ライブラリは、UWP SDK から XAML、コンポジション、入力の API を完全に分離することにより、Windows 10 のネイティブ UI プラットフォーム全体を含むように、WinUI のスコープを大幅に拡張できます。

WinUI は、すべての Windows アプリへのパスとして機能します。ネイティブの UWP アプリまたは Win32 アプリの UI レイヤーとして使用できるほか、[XAML Islands](https://docs.microsoft.com/windows/apps/desktop/modernize/xaml-islands) と共に使用して、デスクトップ アプリを少しずつ最新化することもできます。
 
> [!NOTE]
> WinUI 3.0 Preview 1 は、WinUI 3.0 のプレリリース ビルドです。 [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)にフィードバックをお寄せください。

XAML のすべての新機能は、WinUI の一部として出荷されます。 OS の一部として出荷される既存の UWP XAML API は、新しい機能更新プログラムを受信しなくなります。 ただし、Windows 10 のサポート ライフサイクルに従って、セキュリティ更新プログラムと重要な修正プログラムが引き続き提供されます。

ユニバーサル Windows プラットフォームには、XAML フレームワーク以外 (アプリケーションとセキュリティ モデル、メディア パイプライン、Xbox および Windows 10 のシェル統合、広範なデバイス サポートなど) も含まれており、引き続きサポートされます。

![WinUI 3.0 プラットフォームのサポート](../images/platforms-winui3.png)

> [!Important]
> WinUI 3.0 Preview 1 は、早期評価と、開発者コミュニティからのフィードバックの収集を目的としたもので、 実稼働アプリには使用**できません**。

### <a name="related-links"></a>関連リンク

- [WinUI 3.0 Preview 1 (2020 年 5 月)](winui3/index.md)
- [XAML コントロール ギャラリー (WinUI 3.0 Preview 1) アプリ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3alpha)

## <a name="winui-resources"></a>WinUI リソース

**GitHub**:WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [WinUI リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)では、機能リクエストやバグ報告を行ったり、WinUI チームと対話したりできます。WinUI 3 以降のチームの計画を[ロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)で確認することもできます。

**Web サイト**:[WinUI Web サイト](https://aka.ms/winui) には、製品の比較、WinUI の利点に関する有益な情報があり、製品と製品チームの最新の動向を常にチェックできます。
