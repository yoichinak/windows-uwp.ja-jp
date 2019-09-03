---
description: このチュートリアルでは、UWP XAML ユーザーインターフェイスを追加する方法、MSIX パッケージを作成する方法、およびその他の最新のコンポーネントを WPF アプリに組み込む方法について説明します。
title: MSIX によるパッケージとデプロイ
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、uwp、windows フォーム、wpf、xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6f5c01b23f02bb9c116ddaaec698612aa539539d
ms.sourcegitcommit: e9dc2711f0a0758727468f7ccd0d0f0eee3363e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69979348"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部:MSIX によるパッケージとデプロイ

これは、Contoso の支出という名前の WPF デスクトップアプリのサンプルを最新化する方法を示すチュートリアルの最後の部分です。 サンプルアプリをダウンロードするためのチュートリアル、前提条件、および手順の概要につい[ては、「チュートリアル:WPF アプリ](modernize-wpf-tutorial.md)を最新化します。 この記事では、[パート 4](modernize-wpf-tutorial-4.md)が既に完了していることを前提としています。

[パート 4](modernize-wpf-tutorial-4.md)では、notification api を含む一部の WinRT api は、アプリで使用する前に、パッケージ id が必要であることを学習しました。 Windows アプリケーションをパッケージ化して展開するために Windows 10 で導入されたパッケージ形式である[Msix](https://docs.microsoft.com/windows/msix)を使用して Contoso の経費をパッケージ化することで、パッケージ id を取得できます。 MSIX には、開発者や IT プロフェッショナルにとって、次のような利点があります。

- ネットワーク使用量と記憶域スペースを最適化します。
- アプリが実行されている軽量のコンテナーを利用して、クリーンアンインストールを完了します。 レジストリキーと一時ファイルがシステムに残っていません。
- OS の更新プログラムをアプリケーションの更新プログラムとカスタマイズから切り離します。
- インストール、更新、およびアンインストールのプロセスを簡略化します。

このチュートリアルのパートでは、MSIX パッケージで Contoso の経費のアプリをパッケージ化する方法について説明します。

## <a name="package-the-application"></a>アプリケーションのパッケージ化

Visual Studio 2019 では、Windows アプリケーションパッケージプロジェクトを使用してデスクトップアプリケーションを簡単にパッケージ化することができます。 

1. **ソリューションエクスプローラー**で、 **ContosoExpenses**ソリューションを右クリックし、 **> 追加**、新しいプロジェクト の順に選択します。

    ![新しいプロジェクトの追加](images/wpf-modernize-tutorial/AddNewProject.png)

3. **[新しいプロジェクトの追加]** ダイアログボックスで、を`packaging`検索し、 C#カテゴリで**Windows アプリケーションパッケージプロジェクト**プロジェクトテンプレートを選択して、 **[次へ]** をクリックします。

    ![Windows アプリケーション パッケージ プロジェクト](images/wpf-modernize-tutorial/WAP.png)

4. 新しいプロジェクト`ContosoExpenses.Package`に名前を指定し、 **[作成]** をクリックします。

5. **Windows 10 バージョン 1903 (10.0; を選択します。ビルド 18362)** を**ターゲットバージョン**と**最小バージョン**の両方に使用し、[ **OK]** をクリックします。

    **ContosoExpenses**プロジェクトが**ContosoExpenses**ソリューションに追加されます。 このプロジェクトには、アプリケーションを記述する[パッケージマニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)と、[プログラム] メニューのアイコンやスタート画面のタイルなどの項目に使用される既定のアセットが含まれています。 ただし、UWP プロジェクトとは異なり、パッケージプロジェクトにコードは含まれていません。 その目的は、既存のデスクトップアプリをパッケージ化することです。

6. **ContosoExpenses**プロジェクトで、 **[アプリケーション]** ノードを右クリックし、 **[参照の追加]** を選択します。 このノードは、ソリューション内のどのアプリケーションをパッケージに含めるかを指定します。

6. プロジェクトの一覧で **[ContosoExpenses]** を選択し、 **[OK]** をクリックします。

7. **[アプリケーション]** ノードを展開し、 **ContosoExpense**プロジェクトが参照され、太字で強調表示されていることを確認します。 これは、パッケージの開始点として使用されることを意味します。

8. **ContosoExpenses**プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。

9. ソリューションエクスプローラーで、 **ContosoExpenses**プロジェクトノードを右クリックし、 **[プロジェクトファイルの編集]** を選択します。

10. ファイル内の `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 要素を見つけます。

11. この要素を次の XML に置き換えます。

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject></SourceProject>
            <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

12. プロジェクトファイルを保存して閉じます。

13. **F5**キーを押して、デバッガーでパッケージアプリを起動します。

この時点で、アプリがパッケージとして実行されていることを示す変更がいくつかあります。

- これで、タスクバーまたは [スタート] メニューのアイコンが、すべての**Windows アプリケーションパッケージプロジェクト**に含まれる既定の資産になりました。
- [スタート] メニューに表示されている**ContosoExpense**アプリケーションを右クリックすると、Microsoft Store からダウンロードしたアプリ用に通常予約されているオプションが表示されます (**アプリの設定**、**レート、レビュー** 、共有など)。.

    ![[スタート] メニューの ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- アプリをアンインストールする場合は、スタート メニューの  **ContosoExpense** を右クリックし、**アンインストール** を選択します。 アプリケーションはすぐに削除され、システムに残ったままになりません。

## <a name="test-the-notification"></a>通知をテストする

これで、MSIX を使用して Contoso の経費アプリをパッケージ化したので、[パート 4](modernize-wpf-tutorial-4.md)の最後で動作しなかった通知シナリオをテストできます。

1. Contoso の経費アプリで、一覧から従業員を選択し、[Add new] \ (**新しい費用の追加**\) ボタンをクリックします。 
2. フォーム内のすべてのフィールドを入力し、 **[保存]** をクリックします。
3. OS の通知が表示されていることを確認します。

![トースト通知](images/wpf-modernize-tutorial/ToastNotification.png)
