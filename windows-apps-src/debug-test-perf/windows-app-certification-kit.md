---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows アプリ認定キット
description: 作成したアプリを Microsoft Store に公開する、または Windows 認定を受ける最善の方法は、認定のためにアプリを提出する前に、ローカルでアプリの検証とテストを行うことです。 このトピックでは、Windows アプリ認定キットのインストール方法と実行方法について説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, アプリ認定
ms.localizationpriority: medium
ms.openlocfilehash: be02f9b049a1beb1866d21c97f11fe3efeb815f3
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339350"
---
# <a name="windows-app-certification-kit"></a>Windows アプリ認定キット

アプリ [Windows Certified](/windows/win32/win_cert/windows-certification-portal) を取得するか、または [Microsoft Store への発行](../publish/app-submissions.md)の準備をするには、最初にローカルで検証してテストする必要があります。 このトピックでは、[Windows アプリ認定キット](https://developer.microsoft.com/windows/develop/app-certification-kit)をインストールして実行し、アプリが安全で効率的であることを確認する方法について説明します。

## <a name="prerequisites"></a>前提条件

ユニバーサル Windows アプリのテストの前提条件:

- Windows 10 をインストールして実行する必要があります。
- Windows 10 用 Windows ソフトウェア開発キット (Windows SDK) に含まれる [Windows アプリ認定キット](https://developer.microsoft.com/windows/downloads/app-certification-kit/)をインストールする必要があります。
- [開発用にデバイスを有効にする](/windows/apps/get-started/enable-your-device-for-development)必要があります。
- テストする Windows アプリをコンピューターに展開する必要があります。

> [!NOTE]
> **一括アップグレード:** より新しい [Windows アプリ認定キット](https://developer.microsoft.com/windows/develop/app-certification-kit)をインストールすると、以前にインストールされていたバージョンのキットがすべて置き換えられます。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Windows アプリ認定キットを使った Windows アプリをインタラクティブに検証する

1. **[スタート]** メニューから、 **[アプリ]** 、 **[Windows キット]** の順に進み、 **[Windows アプリ認定キット]** をクリックします。

2. [Windows アプリ認定キット] で、実行する検証のカテゴリを選びます。 たとえば、次のように入力します。Windows アプリを検証する場合、 **[Validate a Windows app]** (Windows アプリの検証) を選択します。

    テストするアプリを直接参照するか、UI で一覧からアプリを選ぶことができます。 Windows アプリ認定キットを初めて実行すると、UI にはコンピューターにインストールされているすべての Windows アプリが一覧表示されます。 以降の実行では、UI には検証済みの最新の Windows アプリが表示されます。 テストするアプリが表示されていない場合は、 **[自分のアプリが表示されない]** をクリックして、システムにインストールされているすべてのアプリを一覧表示できます。

3. テストするアプリを入力するか選択したら **[次へ]** をクリックします。

4. 次の画面からは、テストするアプリの種類に合ったテスト ワークフローが表示されます。 一覧でテストが淡色されている場合、お使いの環境にはそのテストが適用されません。 たとえば、Windows 7 で Windows 10 アプリをテストする場合、静的テストのみがワークフローに適用されます。 Microsoft Store にはこのワークフローのすべてのテストを適用できる点に注意してください。 実行するテストを選んで **[次へ]** をクリックします。

    Windows アプリ認定キットによってアプリの検証が開始されます。

5. テストが終わった後のプロンプトで、テスト レポートを保存するフォルダーのパスを入力します。

    Windows アプリ認定キットによって XML 形式のレポートと共に HTML が作成され、このフォルダーに保存されます。

6. レポート ファイルを開いて、テストの結果を確認します。

> [!NOTE]
> Visual Studio を使っている場合は、アプリ パッケージを作るときに Windows アプリ認定キットを実行できます。 方法については、「[UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」をご覧ください。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>コマンド ラインから Windows アプリ認定キットを使った Windows アプリを検証する

> [!IMPORTANT]
> Windows アプリ認定キットは、アクティブなユーザー セッションで実行する必要があります。

1. コマンド ウィンドウで、Windows アプリ認定キットを含むディレクトリに移動します。

    **注**   既定のパスは C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\ です。

2. 次のコマンドをこの順序で入力し、テスト コンピューターにすでにインストールされているアプリをテストします。

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    または、アプリがインストールされていない場合は次のコマンドを使うことができます。 Windows アプリ認定キットにパッケージが開き、適切なテスト ワークフローが適用されます。

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. テストが完了したら、`[report file name]` という名前のレポート ファイルを開いて、テスト結果を確認します。

**注:** Windows アプリ認定キットはサービスから実行できますが、サービスはアクティブなユーザー セッションでキットのプロセスを開始する必要があり、Session0 では実行できません。

**注**   Windows アプリ認定キットのコマンド ラインについて詳しく知るには、コマンド「`appcert.exe /?`」を入力します。

## <a name="testing-with-a-low-power-computer"></a>低電力コンピューターでのテスト

Windows アプリ認定キットで使用するパフォーマンス テストのしきい値は、低電力コンピューターのパフォーマンスに基づいて設定します。

テストを実行するコンピューターの特性がテスト結果に影響することがあります。 アプリのパフォーマンスが [Microsoft Store ポリシー](/legal/windows/agreements/store-policies)を満たしているかどうかを判断するには、アプリを低電力コンピューター (たとえば画面の解像度が 1366x768 またはそれ以上で、ソリッド ステート ハード ドライブではなく回転式ハード ドライブを搭載した Intel Atom プロセッサ ベースのコンピューター) 上でテストすることをお勧めします。

低電力コンピューターの進化に伴い、パフォーマンスの特性が時間の経過と共に変化する可能性があります。 アプリが最新のパフォーマンス要件を満たすように、最新の [Microsoft Store ポリシー](/legal/windows/agreements/store-policies)を参照し、最新版の Windows アプリ認定キットでアプリをテストしてください。

## <a name="related-topics"></a>関連トピック

- [Windows アプリ認定キットのテスト](windows-app-certification-kit-tests.md)
- [Microsoft Store ポリシー](/legal/windows/agreements/store-policies)