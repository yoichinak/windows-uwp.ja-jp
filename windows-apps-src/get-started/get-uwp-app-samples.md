---
title: UWP アプリのサンプルを取得する
description: GitHub から UWP コードのサンプルをダウンロードする方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、サンプル コード、コード サンプル
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 6b0e30804eabb7e50c5a7319bba9a6b2c83e1d7e
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487854"
---
# <a name="get-uwp-app-samples"></a>UWP アプリのサンプルを取得する

ユニバーサル Windows プラットフォーム (UWP) アプリのサンプルは、GitHub のリポジトリで入手できます。 検索可能なカテゴリ別の一覧については、[サンプル](https://developer.microsoft.com/windows/samples)のページを参照してください。または、[Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "ユニバーサル Windows プラットフォーム アプリのサンプル GitHub リポジトリ") のリポジトリを参照できます。 Windows-universal-samples リポジトリには、すべての UWP 機能とその API 使用パターンを示すサンプルが含まれています。

![GitHub UWP サンプル リポジトリ](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>コードのダウンロード

サンプルをダウンロードするには、[リポジトリ](https://github.com/Microsoft/Windows-universal-samples "ユニバーサル Windows プラットフォーム アプリのサンプル GitHub リポジトリ")にアクセスします。 **[Clone or download]\(クローンまたはダウンロード\)** を選択し、 **[Download ZIP]\(ZIP のダウンロード\)** を選択します。 

![サンプルのダウンロード](images/SamplesDownloadButton.png)

この記事から、[サンプルをダウンロード](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "ユニバーサル Windows プラットフォーム アプリのサンプル zip ファイルのダウンロード")することもできます。

このサンプル ダウンロード .zip ファイルには、常に最新のサンプルが含まれています。 ファイルをダウンロードする際に GitHub のアカウントは必要ありません。 SDK の更新プログラムがリリースされた場合、または最新の変更内容や追加内容を選ぶ場合は、最新の zip ファイルをダウンロードしてください。

> [!NOTE]
> UWP のサンプルを開いたり、作成や実行したりするには、Visual Studio 2015 以降と Windows SDK が必要になります。 [Visual Studio Community の無料コピー](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Windows 開発ツールのダウンロード")を入手できます。 Visual Studio Community は、UWP アプリのビルドをサポートしています。  
>
> サンプルを正常に動作させるには、個別のサンプルではなくアーカイブ全体を解凍してください。 すべてのサンプルは、アーカイブ内の SharedContent フォルダーに依存しているためです。 UWP 機能のサンプルは、Visual Studio のリンク ファイルを使用して、共通ファイル (サンプルのテンプレート ファイルや画像アセットなど) の重複を減らします。 共通ファイルは、リポジトリのルートにある SharedContent フォルダーに格納されます。 リンクは、共通ファイルを参照するプロジェクト ファイルで使用されます。
> 

## <a name="open-the-samples"></a>サンプルを開く

.zip ファイルをダウンロードしたら、Visual Studio でサンプルを開きます。

1.  アーカイブを解凍する前に、ファイルを右クリックし、 **[プロパティ]**  >  **[ブロックの解除]**  >  **[適用]** の順に選択します。 次に、コンピューターのローカル フォルダーにアーカイブを解凍します。

    ![解凍されたアーカイブ](images/SamplesUnzip1.png)
2.  Samples フォルダー内の各フォルダーには、UWP 機能のサンプルが含まれています。

    ![サンプルのフォルダー](images/SamplesUnzip2.png)
3.  Altimeter などのサンプルを選択します。 サブフォルダーはサポートされている言語を示します。

    ![言語フォルダー](images/SamplesUnzip3.png)
4.  使用する言語のフォルダーを選択します。 フォルダーの内容には、Visual Studio で開くことができる Visual Studio ソリューション (.sln) ファイルが表示されます。 たとえば *Altimeter.sln* です。

    ![VS ソリューション](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>フィードバックの提供、質問の投稿、問題の報告

問題やご不明な点がある場合は、リポジトリの **[Issues]\(問題\)** タブを使用して新しい問題を作成してください。 可能な限り解決をお手伝いします。

![フィードバックの画像](images/GitHubUWPSamplesFeedback.png)
