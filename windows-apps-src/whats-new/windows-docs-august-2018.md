---
title: Windows ドキュメントの最新情報、2018 年 8 月 - UWP アプリの開発
description: 2018 年 8 月版の Windows 10 開発者向けドキュメントには、新しい機能、ビデオ、サンプル、開発者向けガイダンスが追加されました。
keywords: 最新情報, 更新, 機能, 開発者向けガイダンス, Windows 10, 8 月
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 92ed09940800160af8fcff6aec7dde26144cffe9
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220335"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Windows 開発者向けドキュメントの最新情報、2018 年 8 月

Windows 開発者向けドキュメントは、Windows プラットフォームで開発者に提供される新機能の情報を反映して継続的に更新されています。 次の機能概要、開発者向けガイダンス、およびビデオは 8 月に公開されたものです。

Windows 10 の[ツールと SDK をインストール](https://developer.microsoft.com/windows/downloads#_blank)すると、[新しいユニバーサル Windows アプリを作成](../get-started/create-uwp-apps.md)したり、[Windows の既存のアプリ コード](../porting/index.md)がどのように使えるかを試したりすることができます。

## <a name="features"></a>機能

### <a name="design"></a>設計

次の機能が Windows Insider Preview ビルドに追加されました。これらは、[Windows Insider](https://insider.windows.com/) プログラムを通じて使用できます。

* [Windows UI ライブラリ](/uwp/toolkits/winui/)は、UWP アプリ用のコントロールとその他のユーザー インターフェイス要素を提供する NuGet パッケージのセットです。 これらのパッケージは Windows 10 の以前のバージョンにも対応しているため、ユーザーが最新の OS を持っていない場合でも、アプリは動作します。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)、および [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) は、アプリのユーザー インターフェイスを強化するための特別な機能を持つボタン コントロールを提供します。

![前景色を選択するための分割ボタン](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView で[上部のナビゲーション](../design/controls-and-patterns/navigationview.md)がサポートされるようになり、アプリのナビゲーション オプションが少なく、アプリのコンテンツにより多くの領域が必要な場合に対応します。

* TreeView が、[データ バインディング、項目テンプレート、ドラッグ アンド ドロップ](../design/controls-and-patterns/tree-view.md)をサポートするように拡張されました。

### <a name="package-support-framework"></a>パッケージ サポート フレームワーク

パッケージ サポート フレームワークは、ソース コードにアクセスできないときに win32 アプリケーションに修正を適用して、win32 アプリケーションが MSIX コンテナー内で実行できるようにするのに役立つオープン ソース キットです。

詳細については、[パッケージ サポート フレームワークを使用した MSIX パッケージへのランタイム修正の適用](/windows/msix/psf/package-support-framework)に関するページをご覧ください。

## <a name="developer-guidance"></a>開発者ガイド

### <a name="web-api-extensions"></a>Web API の拡張機能

クロスブラウザー Web 開発用の Mozilla Developer Network ドキュメントに、[従来の Microsoft API 拡張機能](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)の一覧が追加されました。 これらの API 拡張機能は Internet Explorer または Microsoft Edge に固有のもので、MDN の Web ドキュメントの互換性とブラウザーのサポートに関する既存の情報を補足します。従来の Microsoft [CSS 拡張機能](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)と [JavaScript 拡張機能](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)も利用でき、リッチな Web API 情報を、[Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) で直接表示される MDN から見つけることができます。

### <a name="cwinrt-code-examples"></a>C++/WinRT コード例

既存の C++/CX コード例に加えて、250 件の [C++/WinRT](../cpp-and-winrt-apis/index.md) コード リストをドキュメントのトピックに追加しました。

### <a name="project-rome"></a>Project Rome

[Project Rome ドキュメント](/windows/project-rome/) サイトが、機能ファーストの方式に再編成されました。 これにより、開発者は、自分が探しているものを見つけやすくなり、また選択した機能を複数のプラットフォームにわたって実装しやすくなります。

## <a name="videos"></a>ビデオ

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity プラグイン

Unity 用の Xbox Live プラグインには、Xbox Live の署名、統計、フレンド リスト、クラウド ストレージ、およびランキングをタイトルに追加するためのサポートが含まれています。 [ビデオを見て](https://youtu.be/fVQZ-YgwNpY)詳細を確認したうえで、[GitHub パッケージをダウンロードして](/gaming/xbox-live/get-started/setup-ide/creators/unity-win10/live-cr-unity-win10-nav?WT.mc_id=windowsdocs-twi)開始してください。

### <a name="one-dev-question"></a>One Dev Question

One Dev Question ビデオ シリーズでは、ベテランの Microsoft 開発者が Windows 開発、チーム カルチャ、および歴史に関する一連の質問に答えています。 最新の質問に対してお答えした内容を以下に示します。

Raymond Chen: 

* [カーネルはビデオ ドライバーを再起動するタイミングをどのように認識しますか?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman: 

* [Windows の Burgermaster オブジェクトの背景にあるストーリーを教えてください。](https://youtu.be/0TDSbyAIvX0)