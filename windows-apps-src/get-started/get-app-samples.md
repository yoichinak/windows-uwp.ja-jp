---
title: Windows アプリのサンプルの使用
description: ほとんどの Windows 機能とその API の使用パターンを示す GitHub コードのサンプルを参照、ダウンロードし、開く方法について説明します。
ms.date: 06/30/2020
ms.topic: article
keywords: Windows 10、サンプル コード、コード サンプル
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: e08a81ac6f9bca2e4fe4b7451df830e20c714893
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162946"
---
# <a name="get-windows-app-samples"></a>Windows アプリのサンプルの使用

[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/microsoft/Windows-universal-samples)や [Windows クラシック サンプル](https://github.com/microsoft/Windows-classic-samples)などの多くの公式 Windows コード サンプルに加えて、[Windows 開発者向けドキュメントのサンプル](#windows-developer-documentation-samples)のコレクションが、さまざまな GitHub リポジトリで入手できます。 これらのサンプルで、Windows のほとんどの機能とその API 使用パターンをデモンストレーションできます。

:::image type="content" source="images/github-windows-samples-page.png" alt-text="GitHub の Windows ユニバーサル サンプル リポジトリ":::

[サンプル ブラウザー](/samples/browse/)を使うと、さまざまな Microsoft 開発者ツールやテクノロジに関するコード サンプルをカテゴリ別のコレクションとして表示して検索できるので、特定のサンプルが少し見つけやすくなります。

:::image type="content" source="images/samples-browser-windows.png" alt-text="Microsoft サンプル ブラウザー":::

### <a name="windows-developer-documentation-samples"></a>Windows 開発者向けドキュメントのサンプル

Windows 開発者向けドキュメントをサポートするために作成されたミニアプリのサンプルの一覧を次に示します。 特に記載のない限り、次のサンプルはすべて、最新の [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4) コントロールを使用するように更新されたユニバーサル Windows プラットフォーム (UWP) アプリです。

- [Rss Reader](https://github.com/Microsoft/Windows-appsample-rssreader) - RSS フィードの取得と記事の表示
- [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes) - さまざまな入力モダリティとユーザー認識のシナリオの説明
- [Customer Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) - Azure Active Directory (AAD) 認証、UI コントロール (データ グリッドなど)、Sqlite および SQL Azure データベース統合、Entity Framework、クラウド API サービスのような、エンタープライズ開発者の役に立つ機能
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) - 友人や同僚とのランチのスケジュール
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook) - Windows Ink (Windows Ink ツールバーなど) とラジアル コントローラー (Surface Dial などのホイール デバイス) の機能
- [Network Helper (Quiz Game)](https://github.com/Microsoft/Windows-appsample-networkhelper) - ネットワーク検出と通信
- [HUE Lights Controller](https://github.com/Microsoft/Windows-appsample-huelightcontroller) - Cortana と Bluetooth Low Energy (Bluetooth LE) を使用したインテリジェント ホーム オートメーション
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze) - DirectX を使用した基本的な 3D ゲーム
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab) - イメージ ファイルの表示と編集

## <a name="download-the-code"></a>コードのダウンロード

サンプルをダウンロードするには、[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/microsoft/Windows-universal-samples)などの Microsoft リポジトリのいずれかにアクセスします。 **[Clone or download]\(クローンまたはダウンロード\)** を選択し、 **[Download ZIP]\(ZIP のダウンロード\)** を選択します。

![サンプルのダウンロード](images/SamplesDownloadButton.png)

このサンプル ダウンロード .zip ファイルには、常に最新のサンプルが含まれています。 ファイルをダウンロードする際に GitHub のアカウントは必要ありません。 SDK の更新プログラムがリリースされた場合、または最新の変更内容や追加内容を選ぶ場合は、最新の zip ファイルをダウンロードしてください。

> [!NOTE]
> Windows のサンプルを開いたり、作成や実行したりするには、Visual Studio と Windows SDK が必要になります。 [Visual Studio Community](https://www.microsoft.com/?ref=go) の無料コピーを入手できます。  
>
> サンプルを正常に動作させるには、個別のサンプルではなくアーカイブ全体を解凍してください。 サンプルの多くは、*SharedContent* フォルダー内の共通ファイルに依存しており、サンプル テンプレート ファイルやイメージ資産などのリンク ファイルを使用して重複を減らしています。

## <a name="open-the-samples"></a>サンプルを開く

.zip ファイルをダウンロードしたら、Visual Studio でサンプルを開きます。

1. アーカイブを解凍する前に、ファイルを右クリックし、 **[プロパティ]**  >  **[ブロックの解除]**  >  **[適用]** の順に選択します。 次に、コンピューターのローカル フォルダーにアーカイブを解凍します。

    ![解凍されたアーカイブ](images/SamplesUnzip1.png)

2. Samples フォルダー内の各フォルダーには、Windows 機能のサンプルが含まれています。

    ![サンプルのフォルダー](images/SamplesUnzip2.png)

3. サンプルを選択します。 サポートされている言語は、言語固有のサブフォルダーによって示されます。

    ![言語フォルダー](images/SamplesUnzip3.png)

4. 使用する言語のフォルダーを選択します。 フォルダーの内容には、Visual Studio で開くことができる Visual Studio ソリューション (.sln) ファイルが表示されます。

    ![VS ソリューション](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>フィードバックの提供、質問の投稿、問題の報告

問題やご不明な点がある場合は、リポジトリの **[Issues]\(問題\)** タブを使用して新しい問題を作成してください。

![フィードバックの画像](images/GitHubUWPSamplesFeedback.png)