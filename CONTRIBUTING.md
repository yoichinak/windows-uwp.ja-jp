---
ms.openlocfilehash: ffd5481fec0ebf6db24d9af95b8a11d05a043170
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521223"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>UWP の概念に関するドキュメントへの投稿

ユニバーサル Windows プラットフォーム (UWP) に関するドキュメントに関心をお寄せいただき、ありがとうございます。 ドキュメントへのフィードバック、編集、追加に感謝いたします。

## <a name="writing-content"></a>コンテンツの記述

Microsoft のドキュメントは Markdown という簡易なテキスト スタイル構文で記述されています。 Markdown に馴染みがない場合は、[GitHub 上で基本を学習](https://guides.github.com/features/mastering-markdown/)することができます。 よくわからない場合は、Microsoft のドキュメント内の他のページからいつでも書式スタイルをコピーできます。

## <a name="public-contributions"></a>公開投稿

Microsoft の従業員**ではない**場合は、[パブリック コンテンツ リポジトリ](https://github.com/MicrosoftDocs/windows-uwp)から投稿することができます。 公開投稿は既存のページの変更および詳細説明に適しています。

### <a name="editing-a-file"></a>ファイルの編集

あなたが既にパブリック コンテンツ リポジトリにいる場合、まずは、変更するファイルに移動します。 そこから、表示されたコンテンツの上にある鉛筆アイコンを選択して編集を開始します。

または、docs.microsoft.com のページを表示している場合は、ページの右上部分にある **[編集]** ボタンをクリックします。 これにより、リポジトリ内にある関連付けされたソース ファイルにリダイレクトされます。

編集を開始すると、GitHub によって公式のリポジトリが個人用 GitHub アカウントに自動的にフォークされ、そのアカウントで変更を加えることができるようになります。 完了したら、**docs** ブランチに戻すプル要求を提出します。

### <a name="pull-requests"></a>Pull requests

ご自分のプル要求を送信すると、それはコンテンツ品質チェックリストに対して評価され、Microsoft の基本的な基準を満たしているかどうかが確認されます。 パスした場合は、それは、さらなるレビューのために、UWP ドキュメント チームのメンバーに割り当てられます。 パスしなかった場合、どのような変更を加えるべきかが示されます。

割り当てられたレビュー担当者は PR を承認または却下することも、あなたと協力してさらに変更を加えることもできます。

## <a name="internal-contributions"></a>内部投稿

Microsoft の従業員である場合は、[プライベート コンテンツ リポジトリ](https://github.com/microsoftdocs/windows-uwp-pr)から投稿することができます。 このリポジトリの使用に関するガイダンスは、[Windows オーサリング ガイド](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master)にあります。 今後の機能に関するドキュメントには必ず、プライベート リポジトリを介して投稿する必要があります。

### <a name="editing-a-file"></a>ファイルの編集

パブリック リポジトリと同様に、ローカルの複製を作成しなくても、ご利用のブラウザー内のプライベート リポジトリに小さな変更を加えることができます。 投稿は確実に正しいブランチに行う**必要があります**。 個人用ブランチの作成の詳細については、[Windows オーサリング ガイドの手順](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master)を参照してください。

### <a name="making-substantial-changes"></a>大幅な変更を加える

既存の記事をより幅広く変更する、画像を追加または変更する、新しい記事を投稿する場合は、プライベート コンテンツ リポジトリのローカルの複製を作成します。 詳細については、[Windows オーサリング ガイドの手順](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/)に従ってください。

### <a name="pull-requests"></a>Pull requests

内部リポジトリ内でプル要求を作成する場合は、ご自分のブランチをその作成元のブランチに必ずマージしてください。

送信されたプル要求は、[PR Merger](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master) で評価され、Microsoft の基本的な基準を満たしているかどうかが確認されます。 パスした場合は、コメント `#sign-off` を付けて、さらなるレビューのために、UWP ドキュメント チームのメンバーに渡すことができます。 パスしなかった場合は、サインオフする前に行う必要がある変更が示されます。

割り当てられたレビュー担当者は PR を承認または却下することも、あなたと協力してさらに変更を加えることもできます。 ご自分で PR を承認するまで、レビュー担当者によって PR はマージされません。

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>懸案事項を使用して UWP の概念に関するドキュメントへのフィードバックを提供する

自分で編集するのではなくドキュメントに関するフィードバックを提供する場合は、[公開リポジトリ内に懸案事項を作成](https://github.com/MicrosoftDocs/windows-uwp/issues)することができます。 **[Issues]\(懸案事項\)** タブを選択し、 **[New issue]\(新しい懸案事項\)** ボタンを選択します。 必ずトピックのタイトルとページの URL を含めてください。 その懸案事項はレビューのために UWP ドキュメンテーション チームのメンバーに割り当てられます。

* 内部の懸案事項については、[WDG Content Request Tool](http://sesuw2-iis02a/WSCPubRequest/WindowsContentRequestTool.aspx) を使用してください。
