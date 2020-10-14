---
description: このチュートリアルでは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを最新化する方法が示されています。
title: MSIX によるパッケージとデプロイ
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2ab83b2c9253e8cdd9adc403909a1bd03db1ccda
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762977"
---
# <a name="part-5-package-and-deploy-with-msix"></a>パート 5: MSIX によるパッケージとデプロイ

これは、Contoso Expenses という名前のサンプル WPF デスクトップ アプリを最新化する方法を示すチュートリアルの最後の部分です。 チュートリアルの概要、前提条件、サンプル アプリをダウンロードする手順については、「[チュートリアル:WPF アプリの最新化](modernize-wpf-tutorial.md)」を参照してください。 この記事では、読者が[パート 4](modernize-wpf-tutorial-4.md) を既に完了していることを前提にしています。

[パート 4](modernize-wpf-tutorial-4.md) では、Notifications API を含む一部の WinRT API で、アプリで API が使用される前にパッケージ ID が必要であることを学習しました。 パッケージ ID は、Windows アプリケーションをパッケージ化して展開する Windows 10 で導入されたパッケージ化形式である [MSIX](/windows/msix) を使用して Contoso の経費をパッケージ化することで取得できます。 MSIX には、次の利点が開発者や IT プロフェッショナルにあります。

- ネットワークの使用と記憶域スペースを最適化する。
- アプリが軽量のコンテナーで実行されることにより、クリーン アンインストールを実行できる。 システムにレジストリ キーと一時ファイルが残されない。
- アプリケーションの更新プログラムとカスタマイズから、OS の更新プログラムが切り離される。
- インストール、更新、およびアンインストール プロセスが単純。

このチュートリアルのこの部分では、MSIX パッケージで Contoso の経費アプリをパッケージ化する方法について説明します。

## <a name="package-the-application"></a>アプリケーションをパッケージ化する

Visual Studio 2019 の Windows アプリケーション パッケージ プロジェクトを使用すると、デスクトップ アプリケーションを簡単にパッケージ化できます。 

1. **ソリューション エクスプローラー**で **[ContosoExpenses]** ソリューションを右クリックし、 **[追加]、[新しいプロジェクト]** を選択します。

    ![新しいプロジェクトを追加する](images/wpf-modernize-tutorial/AddNewProject.png)

3. **[新しいプロジェクトの追加]** ダイアログボックスで `packaging` を検索し、C# カテゴリの **[Windows アプリケーション パッケージ プロジェクト]** プロジェクト テンプレートを選択して、 **[次へ]** をクリックします。

    ![Windows アプリケーション パッケージ プロジェクト](images/wpf-modernize-tutorial/WAP.png)

4. 新しいプロジェクトに `ContosoExpenses.Package` という名前を付け、 **[作成]** をクリックします。

5. **[ターゲット バージョン]** と **[最小バージョン]** の両方に、 **[Windows 10, version 1903 (10.0; Build 18362)]** \(Windows 10 バージョン 1903 (10.0、ビルド 18362)\) を選択し、 **[OK]** をクリックします。

    **ContosoExpenses.Package** プロジェクトが **ContosoExpenses** ソリューションに追加されます。 このプロジェクトには、アプリケーションを説明する[パッケージ マニフェスト](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)と、[プログラム] メニューのアイコンやスタート画面のタイルなどの項目に使用される既定の資産が含まれています。 ただし、このパッケージ プロジェクトには、UWP プロジェクトとは異なりコードは含まれせん。 これは、既存のデスクトップ アプリをパッケージ化するためにあります。

6. **[ContosoExpenses.Package]** プロジェクトで、 **[アプリケーション]** ノードを右クリックして **[参照の追加]** を選択します。 このノードでは、ご自分のソリューション内のどのアプリケーションをパッケージに含めるかを指定します。

6. プロジェクトの一覧で **[ContosoExpenses.Core]** を選択し、 **[OK]** をクリックします。

7. **[アプリケーション]** ノードを展開し、 **[ContosoExpense.Core]** プロジェクトが参照されて太字で強調表示されていることを確認します。 これは、これがパッケージの開始点として使用されることを意味します。

8. **[ContosoExpenses.Package]** プロジェクトを右クリックし、 **[スタートアップ プロジェクトに設定]** を選択します。

9. **F5** を押して、デバッガーでパッケージ アプリを起動します。

この時点では、アプリがパッケージとして実行されていることを示す変更がいくつかあるはずです。

- タスクバーまたは [スタート] メニューのアイコンが、すべての **Windows アプリケーション パッケージ プロジェクト**に含まれる既定の資産になりました。
- [スタート] メニューに表示される **[ContosoExpense.Package]** アプリケーションを右クリックすると、Microsoft Store からダウンロードしたアプリ用に一般に予約されている、 **[アプリの設定]** 、 **[評価とレビュー]** 、 **[共有]** などのオプションが表示されます。

    ![[スタート] メニューの ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- このアプリをアンインストールする場合は、[スタート] メニューの **[ContosoExpense.Package]** を右クリックし、 **[アンインストール]** を選択します。 このアプリは、システムに何も残さず直ちに削除されます。

## <a name="test-the-notification"></a>通知をテストする

これで、MSIX で Contoso の経費アプリをパッケージ化したので、[パート 4](modernize-wpf-tutorial-4.md) の最後で動作していなかった通知シナリオをテストできます。

1. Contoso Expenses アプリで一覧から従業員を選択して、 **[新しい経費の追加]** ボタンをクリックします。
2. フォームのすべてのフィールドを入力し、 **[保存]** をクリックします。
3. OS の通知が表示されることを確認します。

![トースト通知](images/wpf-modernize-tutorial/ToastNotification.png)