---
title: Xbox One 開発者モードのアクティブ化
description: 開発者モードをアクティブ化して、リテール モードと開発者モードを切り替えることができるようにする方法を説明します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: d93fef1b06fa52616b7e5eddf7b3ac0530856dc2
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577104"
---
# <a name="xbox-one-developer-mode-activation"></a>Xbox One 開発者モードのアクティブ化

## <a name="how-developer-mode-works"></a>開発者モードの動作
この記事は、Xbox One と Xbox シリーズ X | にのみ適用されます。小売チャネルを通じて取得した S コンソール。 マネージ開発プログラムによって取得された開発キット HW の場合は、記事の最後にあるメモを参照してください。

Xbox リテールコンソールには、リテールモード (1) と開発者モード (2) という2つのモードがあります。 リテールモードでは、コンソールは通常の状態になります。ゲームをプレイし、Xbox ストアで取得したアプリを実行できます。 開発者モードでは、コンソール用にソフトウェアを開発してテストすることができますが、製品版のゲームや製品版アプリを実行することはできません。

開発者モードは、Xbox ストアにある "Retail to Dev Kit conversion" アプリを使用して、すべてのリテール Xbox コンソールで有効にすることができます。 リテールコンソールで開発者モードを有効にした後、製品版 (2a) と開発者モード (2b) を切り替えることができます。

> [!NOTE]
> この変換アプリは、Xbox マネージプログラムを通じて取得した Xbox 開発ハードウェア (たとえば、) では実行しないでください ID@Xbox 。または、ゲームの開発中にエラーや遅延が発生する可能性があります。 マネージドパートナーの場合は、開発ハードウェアのアクティブ化に関する詳細情報を確認できます。 https://developer.microsoft.com/en-us/games/xbox/docs/gdk/provisioning-role にアクセスします。

<br></br>

![Xbox One のモード](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-console"></a>リテール Xbox コンソールで開発者モードをアクティブ化する

1.  Xbox コンソールを起動します。

2.  Xbox One Store から、**開発者モードのアクティブ化** 用アプリを検索してインストールします。

    ![開発者モードのアクティブ化用アプリのインストール](images/devkit-activation-1.png)

3.  Microsoft Store ページからアプリを起動します。

    ![開発者モードのアクティブ化用アプリ](images/devkit-activation-2.png)

4.  開発者モードのアクティブ化用アプリに表示されたコードを書き留めます。

    ![アクティブ化手順 5](images/activation-step-5.png)  
    
5.  [パートナーセンターにアプリ開発者アカウントを登録](https://developer.microsoft.com/store/register)します。  これは、ゲームの発行に向けた最初の手順でもあります。

6.  有効な現在のパートナーセンターアプリ開発者アカウントを使用して、 [パートナーセンター](https://partner.microsoft.com/dashboard) にサインインします。  左側のナビゲーションウィンドウに複数のオプションが表示されない場合、または [**概要**] セクションの [**新しいアプリを作成する**] オプションが表示されない場合は、次の手順とライセンス認証のリンクは _機能しません_。前の手順でアプリ開発者アカウントが完全に登録されていることを確認します。

7.  [Partner.microsoft.com/xboxconfig/devices](https://partner.microsoft.com/xboxconfig/devices)にアクセスします。

8.  開発者モードのアクティブ化用アプリに表示されたアクティブ化コードを入力します。 アカウントに関連付けられているアクティブ化の数には制限があります。 開発者モードがアクティブ化されると、パートナーセンターは、アカウントに関連付けられているいずれかのライセンス認証を使用したことを示します。

    ![アクティブ化手順 8](images/activation-step-8-rs2.png)    
    
9.  **[Agree and activate]** (同意してアクティブ化) をクリックします。 ページの再読み込みが行われ、デバイスが表に追加されます。 Xbox One 開発者モードのライセンス認証プログラム契約は、[Xbox One開発者モードのライセンス認証プログラム](/legal/windows/agreements/xbox-one-developer-mode-activation) にあります。

10. アクティブ化コードを入力すると、アクティブ化プロセスの進行状況が表示されます。  
    
11. アクティブ化の完了後、開発者モードのアクティブ化用アプリを開き、**[Switch and restart]** (切り替えて再起動) をクリックして、開発者モードに移行します。 これは通常の再起動よりも時間がかかります。

    ![アクティブ化手順 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>リテール モードと開発者モードを切り替える
本体で開発者モードを有効にした後、リテール モードと開発者モードを切り替えるには、**Dev Home** を使います。 Dev Home の開始と使用の詳細については、「 [Xbox One tools の概要](introduction-to-xbox-tools.md)」を参照してください。

* リテール モードに切り替えるには、**Dev Home** を開きます。 **[クイック アクション]** で、**[開発者モードの終了]** を選択します。 コンソールがリテール モードで再起動します。    

  ![アクティブ化手順 13](images/activation-step-13-rs4.png)  
  
* 開発者モードに切り替えるには、開発者モードのアクティブ化用アプリを使います。 アプリを開き、**[Switch and restart]** (切り替えて再起動) を選びます。 これにより、本体が開発者モードで再起動します。  

  ![アクティブ化手順 14](images/activation-step-12.png)  

## <a name="see-also"></a>関連項目
- [Xbox One 開発者モードの非アクティブ化](devkit-deactivation.md)
- [Xbox One の UWP](index.md)
