---
description: このチュートリアルでは、UWP XAML ユーザー インターフェイスを追加、MSIX パッケージを作成およびその他の最新コンポーネントを WPF アプリに組み込む方法を示します。
title: パッケージ化し、MSIX とデプロイ
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml 諸島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420102"
---
# <a name="part-5-package-and-deploy-with-msix"></a>パート 5:パッケージ化し、MSIX とデプロイ

これは、Contoso の経費をという名前のサンプル WPF デスクトップ アプリの近代化する方法を説明するチュートリアルの最後の部分です。 チュートリアル、前提条件、およびサンプル アプリをダウンロードする手順の概要については、次を参照してください。[チュートリアル。WPF アプリの近代化](modernize-wpf-tutorial.md)します。 この記事では、既に完了している前提としています。[パート 4](modernize-wpf-tutorial-4.md)します。

[パート 4](modernize-wpf-tutorial-4.md)アプリで使用するには通知 API など、一部の WinRT Api がパッケージ id を必要とするについて説明しました。 パッケージ id を取得するには、パッケージを使用して Contoso 経費[MSIX](https://docs.microsoft.com/windows/msix)、パッケージ化し、Windows アプリケーションを展開する Windows 10 で導入された、パッケージ化形式。 MSIX は、開発者と IT プロフェッショナルなどの両方のテーブルに利点があります。

- ネットワーク使用率とストレージ領域が最適化されています。
- クリーン完了、アプリが実行される場所の軽量コンテナーに協力してくれた、アンインストールします。 システムのレジストリ キーと一時ファイルが残っていません。
- アプリケーションの更新とカスタマイズから OS の更新プログラムを分離します。
- インストールを簡略化更新、およびプロセスをアンインストールします。 

このチュートリアルのこの部分では、MSIX パッケージ内の Contoso 経費アプリケーションをパッケージ化する方法を学習します。

## <a name="package-the-application"></a>アプリケーションをパッケージします。

Visual Studio 2019 は、Windows アプリケーション パッケージ プロジェクトを使用して、デスクトップ アプリケーションをパッケージ化する簡単な方法を提供します。 

1. **ソリューション エクスプ ローラー**を右クリックし、 **ContosoExpenses**ソリューション選択**追加]、[新しいプロジェクト**します。

    ![新しいプロジェクトを追加します。](images/wpf-modernize-tutorial/AddNewProject.png)

3. **新しいプロジェクトを追加** ダイアログ ボックスで、検索`packaging`、選択、 **Windows アプリケーション パッケージ プロジェクト**プロジェクト テンプレート、C#カテゴリ、およびクリック**次へ**.

    ![Windows アプリケーション パッケージ プロジェクト](images/wpf-modernize-tutorial/WAP.png)

4. 新しいプロジェクトの名前`ContosoExpenses.Package`クリック**作成**です。

5. 選択**Windows 10、バージョンが 1903 (10.0;18362 をビルドする)** 両方の**ターゲット バージョン**と**最小バージョン** をクリック**OK**します。

    **ContosoExpenses.Package**プロジェクトに追加されます、 **ContosoExpenses**ソリューション。 このプロジェクトに含まれる、[パッケージ マニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)アプリケーション、およびいくつかのプログラム メニューのアイコンと、スタート画面にタイルなどの項目に使用される既定のアセットをについて説明します。 ただし、UWP プロジェクトとは異なり、packaging プロジェクトではコードがありません。 その目的は、既存のデスクトップ アプリをパッケージ化します。

6. **ContosoExpenses.Package**プロジェクトを右クリックして、**アプリケーション**ノード選択**参照の追加**します。 このノードは、ソリューションでは、どのアプリケーションをパッケージに含まれますを指定します。

7. プロジェクトの一覧で選択**ContosoExpenses.Core**  をクリック**OK**します。

8. 展開、**アプリケーション**ノードことを確認します、 **ContosoExpense.Core**プロジェクトが参照されているし、太字で強調表示されます。 これは、それが使用されることを出発点として、パッケージを意味します。

9. 右クリックし、 **ContosoExpenses.Package**プロジェクト**スタートアップ プロジェクトとして設定**します。

10. キーを押して**F5**をデバッガーでパッケージ化されたアプリケーションを開始します。

この時点では、として実行中のパッケージ化、アプリを示すいくつかの変更が発生することができます。

- タスク バーで、または、[スタート] メニューのアイコンが、既定の資産に含まれているすべて**Windows アプリケーション パッケージ プロジェクト**します。
- 右クリックした場合、 **ContosoExpense.Package** [スタート] メニューに表示されるアプリケーションなどの Microsoft Store からダウンロードされたアプリ用に予約が通常のオプションがわかります**アプリ設定**、**レートとレビュー**と**共有**します。

    ![[スタート] メニューで ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 右クリックして、アプリをアンインストールする場合は、 **ContosoExpense.Package** [スタート] メニューで選択**アンインストール**します。 アプリはすぐに削除されます、システム上、いまだにを離れることがなく。

## <a name="test-the-notification"></a>通知をテストします。

末尾が機能していない通知シナリオをテストできます MSIX で Contoso 経費アプリケーションをパッケージ化したことは、これで[パート 4](modernize-wpf-tutorial-4.md)します。

1. Contoso 経費アプリでは、従業員を一覧から選択し、**新しい経費の追加**ボタンをクリックします。 
2. キーを押して、フォームのすべてのフィールドを完了**保存**します。
3. OS 通知を表示することを確認します。

![トースト通知](images/wpf-modernize-tutorial/ToastNotification.png)
