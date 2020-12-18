---
title: リポジトリにマニフェストを送信する
description: アプリケーションを記述するパッケージ マニフェストを作成したら、マニフェストを Windows パッケージ マネージャー リポジトリに送信する準備ができています。
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fef6cc96604639f84ee2c81e2de4fb28442e3f8d
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598803"
---
# <a name="submit-your-manifest-to-the-repository"></a>リポジトリにマニフェストを送信する

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

アプリケーションを記述する[パッケージ マニフェスト](manifest.md)を作成したら、マニフェストを Windows パッケージ マネージャー リポジトリに送信する準備ができています。 これは、**winget** ツールがアクセスできるマニフェストのコレクションを含む公開リポジトリです。 マニフェストを送信するには、GitHub のオープン ソース [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) リポジトリにアップロードします。

プル要求を送信して GitHub リポジトリに新しいマニフェストを追加すると、自動プロセスによってマニフェスト ファイルが検証され、パッケージが悪意のあるものではないことが確認されます。 この検証が成功すると、パッケージは公開されている Windows パッケージ マネージャー リポジトリに追加されるため、**winget** クライアント ツールで検出できるようになります。 オープン ソースの GitHub リポジトリのマニフェストと、公開されている Windows パッケージ マネージャー リポジトリの違いに注意してください。

> [!IMPORTANT]
> Microsoft は、任意の理由で送信を拒否する権利を留保します。

## <a name="third-party-repositories"></a>サードパーティ リポジトリ

現在、既知のサードパーティ リポジトリはありません。 Microsoft は、複数のパートナーと協力して、サードパーティのリポジトリを有効にするプロトコルまたは API を開発しています。

## <a name="manifest-validation"></a>マニフェストの検証

GitHub の [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) リポジトリにマニフェストを送信すると、マニフェストが Windows エコシステムの安全性に関して自動的に検証および評価されます。 マニフェストは手動で確認される場合もあります。

## <a name="how-to-submit-your-manifest"></a>マニフェストの送信方法

マニフェストをリポジトリに送信するには、次の手順を実行します。

### <a name="step-1-validate-your-manifest"></a>手順 1:マニフェストを検証する

**winget** tool では、マニフェストが正しく作成されたことを確認するための [validate](..\winget\validate.md) コマンドが提供されています。 マニフェストを検証するには、このコマンドを使用します。

```CMD
winget validate \<manifest-file>
```

検証が失敗した場合は、エラーを使用して行番号を特定し、修正を行います。 マニフェストが検証されたら、それをリポジトリに送信できます。

### <a name="step-2-clone-the-repository"></a>手順 2:リポジトリのクローン

次に、リポジトリのフォークを作成して複製します。

1. ブラウザーで [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) にアクセスし、 **[Fork]** をクリックします。
    ![フォークの画像](images\fork.png)

2. Windows コマンド プロンプトや PowerShell などのコマンド ライン環境から、次のコマンドを使用してフォークを複製します。
    ```CMD
    git clone \<your-fork-name>
    ```

 3. 複数の送信を行う場合は、フォークではなくブランチを作成します。 現在、送信ごとに 1 つのマニフェスト ファイルのみが許可されます。
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>手順 3:ローカル リポジトリにマニフェストを追加する

次のフォルダー構造で、リポジトリにマニフェスト ファイルを追加する必要があります。

**manifests** / **publisher** / **application** / **version.yaml**

* **manifests** フォルダーは、リポジトリ内のすべてのマニフェストのルート フォルダーです。
* **publisher** フォルダーは、ソフトウェアを発行する会社の名前です。 たとえば、**Microsoft** です。
* **application** フォルダーは、アプリケーションまたはツールの名前です。 たとえば、**VSCode** です。
* **version.yaml** はマニフェストのファイル名です。 ファイル名は、アプリケーションの現在のバージョンに設定する必要があります。 たとえば **1.0.0.yaml** です。

>[!IMPORTANT]
> マニフェスト内の `Id` 値は、マニフェスト フォルダー パス内の発行元とアプリケーションの名前と一致する必要があります。マニフェストの `version` 値は、ファイル名のバージョンと一致している必要があります。 詳しくは、「[パッケージ マニフェストを作成する](manifest.md#tips-and-best-practices)」をご覧ください。

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>手順 4:リモート リポジトリにマニフェストを送信する

これで、リモート リポジトリに新しいマニフェストをプッシュする準備ができました。

1. `add` コマンドを使用して、送信を準備します。
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. `commit` コマンドを使用して変更をコミットし、送信に関する情報を指定します。
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. `push` コマンドを使用して、リモート リポジトリに変更をプッシュします。
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>手順 5:プル要求の作成

変更をプッシュした後、[https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) に戻り、フォークまたはブランチをメイン ブランチにマージする pull request を作成します。

![[プル要求] タブの画像](images\pull-request.png)

## <a name="validation-process"></a>検証プロセス

プル要求を作成すると、マニフェストを検証し、プル要求を処理するオートメーション プロセスが開始されます。 プル要求にラベルを追加して、進行状況を追跡できるようにします。

### <a name="submission-expectations"></a>送信の必要事項

Windows パッケージ マネージャー リポジトリへのすべてのアプリケーション送信は、正しい設定になっている必要があります。 次に、送信に関するいくつかの必要事項を示します。

* マニフェストは、[スキーマの要件](manifest.md#manifest-contents)に準拠していること。
* マニフェスト内のすべての URL は、安全な Web サイトにつながること。
* インストーラーとアプリケーションは、ウイルスに感染していないこと。 パッケージは、誤ってマルウェアとして識別される場合があります。 偽陽性であると思われる場合は、インストーラーを[こちら](https://www.microsoft.com/wdsi/filesubmission)から Defender チームに送信して分析を受けることができます。
* 管理者と非管理者の両方に対して、アプリケーションが正しくインストールされ、アンインストールされること。
* インストーラーは非対話型モードをサポートしていること。
* すべてのマニフェスト エントリは正確で、誤解を招くことがないこと。
* インストーラーは、発行元の Web サイトから直接取得されること。

### <a name="pull-request-labels"></a>プル要求のラベル

検証中に、進行状況を伝えるための一連のラベルがプル要求に適用されます。

* **Needs: author feedback (必要: 作成者フィードバック)** :送信でエラーが発生しています。 プル要求はユーザー再割り当てされます。 10 日以内に問題に対処しない場合、プル要求は終了されます。
* **Manifest-Validation-Error**:送信されたマニフェストに構文エラーが含まれています。
* **URL-Validation-Error**:送信内の 1 つ以上の URL が [SmartScreen](/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview) 検証に失敗しました。
* **Binary-Validation-Error**:送信されたアプリケーション インストーラがウイルス スキャン テストに失敗したか、またはハッシュが一致していません。
* **Pull-Request-Error**:プル要求に問題があります。 たとえば、フォルダー構造に[必要な形式](#step-3-add-your-manifest-to-the-local-repository)がありません。
* **Validation-Error**:送信されたアプリケーションは、一般的な検証テストに失敗しました。
* **Validation-Installation-Error**:送信されたアプリケーションは、インストール テストに失敗しました。
* **Validation-Uninstall-Error**:送信されたアプリケーションは、アンインストール テストに失敗しました。
* **Validation-Virus-Scan-Error**:送信されたアプリケーションは、ウイルス スキャンのテストに失敗しました。
* **Azure-Pipeline-Passed**:マニフェストは検証の最初の部分を完了しました。 この手順の後に、プル要求が Microsoft のテスト チームに割り当てられ、最終的な検証が実行されます。
* **Validation-Completed**:検証は完了し、プル要求はマージされます。