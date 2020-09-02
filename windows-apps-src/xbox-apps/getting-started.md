---
title: Xbox One の UWP アプリ開発の概要
description: Xbox One でユニバーサル Windows プラットフォーム (UWP) アプリの開発を開始するために、開発用 PC と Xbox One コンソールを設定する方法について説明します。
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 243f8972f1ea5879ee77f036d97c05e29d446b12
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304704"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Xbox One の UWP アプリ開発の概要

ユニバーサル Windows プラットフォーム (UWP) 開発のために PC と Xbox One を設定する方法の手順に**注意深く**従います。 この設定の完了後に、Xbox One の開発者モードと UWP アプリの構築について詳しくは、「[Xbox one の UWP](index.md)」ページをご覧ください。 

## <a name="before-you-start"></a>開始する前に

開始する前に、次の操作をする必要があります。
-   最新バージョンの Windows 10 を使用して PC をセットアップします。
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Xbox One 本体上に少なくとも 5 GB の空き領域が必要です。

## <a name="setting-up-your-development-pc"></a>開発用 PC のセットアップ

1.  Visual Studio 2015 Update 3、Visual Studio 2017、または Visual Studio 2019 をインストールします。

    Visual Studio 2015 Update 3 をインストールする場合は、[ **カスタム** インストール] を選択し、[ **ユニバーサル Windows アプリ開発ツール** ] チェックボックスがオンになっていることを確認します。これは、既定のインストールの一部ではありません。 C++ 開発者の場合は、**カスタム インストール**と **C++** を選択してください。

    Visual Studio 2017 または Visual Studio 2019 をインストールする場合は、必ず **ユニバーサル Windows プラットフォームの開発** ワークロードを選択してください。 C++ 開発者の場合は、右側の [ **概要** ] ウィンドウの [ **ユニバーサル Windows プラットフォーム開発**] で、[ **C++ ユニバーサル Windows プラットフォームツール** ] チェックボックスがオンになっていることを確認します。 既定のインストールには含まれていません。

    詳細については、「 [Xbox 開発環境での UWP の設定](development-environment-setup.md)」を参照してください。

2.  最新の [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)をインストールします。

3.  開発用 PC の開発者モードを有効にします (**設定/更新 & セキュリティ/開発者向け、開発者向け機能/開発者モードを使用**します)。

これで開発用 PC の準備は完了です。次のビデオをご覧ください。続きのセクションでは、Xbox One を開発用に設定し、UWP アプリを作成して展開する方法について説明します。
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Xbox One 本体の設定

1.  Xbox One の開発者モードを有効にします。 アプリをダウンロードし、ライセンス認証コードを取得して、パートナーセンターアプリ開発者アカウントの [ **Xbox One コンソールの管理** ] ページに入力します。 詳しくは、「[Xbox One の開発者モードのアクティブ化](devkit-activation.md)」をご覧ください。 

2.  **開発モードのアクティブ化**アプリを開き、[**切り替えと再起動**] を選択します。 これで Xbox One は開発者モードとなりました。
  
  > [!NOTE]
  > 市販のゲームやアプリは開発者モードでは実行できません。自分で作成するアプリまたはゲームを実行できます。 市販のゲームやアプリを実行するには、リテール モードに切り替えます。
    
  > [!NOTE]
  > 開発者モードでアプリを Xbox One に展開するには、ユーザーを本体にサインインさせる必要があります。 既存の Xbox Live アカウントを使用することも、開発者モードで本体の新しいアカウントを作成することもできます。 

## <a name="creating-your-first-project-in-visual-studio"></a>Visual Studio での最初のプロジェクトの作成

詳細については、「 [Xbox 開発環境での UWP の設定](development-environment-setup.md)」を参照してください。

1.  **C# の場合**: 新しいユニバーサル Windows プロジェクトを作成し、 **ソリューションエクスプローラー**でプロジェクトを右クリックし、[ **プロパティ**] を選択します。 [**デバッグ**] タブを選択し、[**ターゲットデバイス**] を [**リモートコンピューター**] に変更し、Xbox One コンソールの IP アドレスまたはホスト名を [**リモートコンピューター** ] フィールドに入力し、[**認証モード**] ボックスの一覧で [**ユニバーサル (暗号化**されていないプロトコル)] を選択します。   

    本体で Dev Home (ホーム画面の右側の大きなタイル) を開始すると、左上隅に Xbox One の IP アドレスが表示されます。 Dev Home について詳しくは、「[Xbox One ツールの概要](introduction-to-xbox-tools.md)」をご覧ください。  

2.  **C++ および HTML/Javascript プロジェクトの**場合: C# プロジェクトと同様のパスに従いますが、プロジェクトのプロパティで [**デバッグ**] タブに移動し、デバッガーで [**リモートコンピューター** ] を選択してドロップダウンリストを開き、[**コンピューター名**] フィールドにコンソールの IP アドレスまたはホスト名を入力し、[**認証の種類**] フィールドで [**ユニバーサル (暗号化**されて

3. 上部のメニューバーの緑色の再生ボタンの左側にあるドロップダウンから [ **x64** ] を選択します。
   
4.  F5 キーを押してアプリをビルドして、Xbox One での展開を開始します。
  
5.  初めてこれを行う際には、Visual Studio に Xbox One の PIN の入力を求められます。 Xbox One で Dev Home を起動し、[ **Visual Studio の Pin を表示** ] ボタンを選択することで、pin を取得できます。
  
6.  ペアリングを行うと、アプリの展開が開始されます。 初めてこれを行う際には、(すべてのツールを Xbox にコピーする必要があるため) 少し時間がかかることがありますが、数分以上かかる場合には、何か問題がある場合があります。 上記のすべての手順に従っていることを確認してください (特に、 **認証モード** を **Universal**に設定した場合)。また、Xbox One へのワイヤード (有線) ネットワーク接続を使用していることを確認します。  

7. 用意ができました。 本体での初めてのアプリの実行をお楽しみください。  

## <a name="thats-it"></a>以上で作業は終了です。

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>こちらもご覧ください  
- [よく寄せられる質問](frequently-asked-questions.md)  
- [Xbox 開発者プログラムの UWP の既知の問題](known-issues.md)
- [Xbox One の UWP](index.md) 
