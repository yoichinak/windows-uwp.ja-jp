---
title: Windows UI ライブラリ
description: WinUI 2.x と Windows アプリの開発に関する情報を提供します。
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, ツールキット sdk, winui, Windows UI ライブラリ
ms.custom: RS5
ms.openlocfilehash: 42f790ed92a41f298465bcc42b21dcdb3fa8bc86
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493637"
---
# <a name="windows-ui-library-2x"></a>Windows UI ライブラリ 2.x

![WinUI コントロール](images/winUI-library-767.png)

Windows UI ライブラリでは、Windows アプリ向けに公式のネイティブ Windows UI コントロールおよびその他のユーザー インターフェイス要素が提供されます。

以前のバージョンの Windows 10 との下位互換性が維持されるため、ユーザーが最新の OS を使用していない場合でも、アプリが動作します。

> [!NOTE]
> Windows 10 UI プラットフォームのメジャー アップデートである、[Windows UI ライブラリ 3 Preview 2 (2020 年 7 月)](../winui3/index.md) を確認してください。

## <a name="features"></a>機能

* **新しいコントロール**:Windows UI ライブラリには、既定の Windows プラットフォームの一部として同梱されていない新しいコントロールが含まれています。

* **既存のコントロールの更新バージョン**:ライブラリには、以前のバージョンの Windows 10 で使用できる既存の Windows プラットフォーム コントロールの更新バージョンも含まれています。

* **以前のバージョンの Windows 10 のサポート**:Windows UI ライブラリ API は以前のバージョンの Windows 10 で動作するため、最新 OS を実行していない可能性のあるユーザーのサポート用にバージョン チェックや条件付き XAML を含める必要はありません。

* **XamlDirect のサポート**:ミドルウェア開発者向けに設計された Xaml Direct API を使用すると、CPU とワーキング セットのパフォーマンスを向上させる下位レベルの XAML 機能にアクセスできます。 XamlDirect では、以前のバージョンの Windows 10 で XamlDirect API を使用でき、複数のターゲット Windows 10 バージョンを処理する特別なコードを記述する必要はありません。

## <a name="examples"></a>例

XAML Controls Gallery サンプル アプリには、WinUI コントロールを使用するための対話型のデモとサンプル コードが含まれています。

* [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) から XAML Controls Gallery アプリをインストールします

* XAML Controls Gallery は [GitHub のオープン ソース](
https://github.com/Microsoft/Xaml-Controls-Gallery)でもあります

## <a name="documentation"></a>ドキュメント

Windows UI ライブラリ コントロールの操作方法に関する記事は、[ユニバーサル Windows プラットフォーム コントロール ドキュメント](/windows/uwp/design/controls-and-patterns/)に含まれています。

API リファレンスのドキュメントがある場所は、[Windows UI ライブラリ API](/uwp/api/overview/winui/) です。

## <a name="install-and-use-the-windows-ui-library"></a>Windows UI ライブラリをインストールして使用する

手順については、「[Windows UI ライブラリの使用を開始する](getting-started.md)」を参照してください。

## <a name="open-source-and-developer-roadmap"></a>オープン ソースと開発者向けのロードマップ

WinUI は、GitHub でホストされているオープン ソース プロジェクトです。 [Windows UI ライブラリ リポジトリ](https://aka.ms/winui)のバグ レポート、機能要求、およびコミュニティ コードの投稿を歓迎します。

より多くの開発者シナリオをサポートするために、今後も WinUI の開発と進化を続けます。 WinUI の計画に関する最新の詳細については、Windows UI ライブラリ リポジトリの[ロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)をご覧ください。

## <a name="nuget-package-list"></a>NuGet パッケージの一覧

Windows UI ライブラリに含まれる複数の NuGet パッケージ:[Windows UI ライブラリ NuGet パッケージの一覧](nuget-packages.md)。

## <a name="see-also"></a>関連項目

[Windows UI ライブラリ 2.x のリリース ノート](release-notes/index.md)
