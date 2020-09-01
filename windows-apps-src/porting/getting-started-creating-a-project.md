---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Windows にとっての Microsoft Visual Studio は、iOS や Mac OS にとっての Xcode に相当します。 このチュートリアルでは、Visual Studio の使い方に慣れる訓練を行います。
title: Visual Studio でのプロジェクトの作成
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be772573321e7f1cbf5e5861d5f7b15a1ba5183
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174896"
---
# <a name="getting-started-creating-a-project"></a>概要: プロジェクトの作成

## <a name="creating-a-project"></a>プロジェクトの作成

Windows にとっての Microsoft Visual Studio は、iOS や Mac OS にとっての Xcode に相当します。 このチュートリアルでは、Visual Studio の使い方に慣れる訓練を行います。 このチュートリアルを通して、開始にあたって知っておく必要がある最も基本的な事柄を確認できます。 アプリを作るたびに、この手順に従うことになります。

次のビデオでは、Xcode と Visual Studio を比較しています。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Comparing-Xcode-to-Visual-Studio/player]

この [Windows 用アプリ開発ブログの記事](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/)も非常に役立ちます。

Windows 10 用のアプリ (正式には、ユニバーサル Windows プラットフォーム (UWP) アプリと呼ばれます) の作成は、ストーリーボードを使った iOS アプリの作成に似ています。 Windows 10 アプリは、しばしば複数ページを使って作成され、Web サイトのように各ページにユーザー インターフェイスの異なる部分が含まれています。 各ページには通常、2 つのソース ファイルが関連付けられています。1 つは [XAML の概要](../xaml-platform/xaml-overview.md)形式のユーザー インターフェイスを格納するファイルで、もう 1 つはソース コードを記述したファイルです (多くの場合 C#)。 ユーザーは、アプリとやり取りするときに、これらのページの間を移動します。 このチュートリアルでは、ページを 2 つ持つアプリを作成します。

**メモ**   Windows 10 アプリの重要な機能は、プラットフォームに関係なく、同じソースコードと同じ API セットを利用できるということです。 ご存知のように、iPhone や iPad 向けのユニバーサル iOS アプリを作っている場合、実行時にアプリを実行するプラットフォームを決定して、適切なアクションを実行できます。 同様に、Windows 10 アプリは実行されているデバイスを実行時に見分けることができます。 UWP アプリでは、ソースコードで ifdef を使用して、電話とデスクトップの両方 \# のビルドを作成する必要はありません。 また、便利なことに、Windows 10 アプリはデバイスの種類に応じてユーザー インターフェイス コントロールをインテリジェントに使用します。たとえば、アプリが日付選択コントロールを参照する場合、コントロールは自動的に表示され、デスクトップと電話のどちらの画面で実行されているかによって異なる動作を実行します。 ただし、ソース コードは変わりません。

Windows 10 アプリを作成する方法を見てみましょう。 Visual Studio の実行から始めます。 Visual Studio を初めて起動すると、開発者用ライセンスを取得するように求められます。 開発者用ライセンスでは、UWP アプリを Microsoft Store に提出する前にローカル コンピューター上にインストールしてテストできます。 ライセンスを取得するには、画面の指示に従って、Microsoft アカウントを使ってサインインします。 Microsoft アカウントがない場合は、**[開発者用ライセンス]** ダイアログ ボックスの **[新規登録]** リンクをクリックし、画面の指示に従います。

Xcode では、初めて起動したときに次のような **[Welcome to Xcode]** (Xcode へようこそ) 画面が表示されます。比較してください。

![Xcode のようこそ画面](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio もこれに似ています。 次の図に示すように、**[スタート ページ]** が表示されます。

![Visual Studio のスタート画面](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

新しいアプリを作るには、次のどちらかの操作を行って、プロジェクトの作成を開始します。

-   **[開始]** 領域で、**[新しいプロジェクト]** をタップします。
-   **[ファイル]** メニュー、**[新しいプロジェクト]** の順にタップします。

Xcode で新しいプロジェクトを作るときは、次の図に示すようなプロジェクト テンプレートの一覧が表示されます。比較してください。

![Xcode のプロジェクトの新規作成ダイアログ ボックス](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

Visual Studio の場合も、次の図に示すように、いくつかのプロジェクト テンプレートから選択できるようになっています。

![このチュートリアルの visual studio の [新しいプロジェクト] ダイアログボックス ](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) で、[ **visual C#**] をタップし、[ **windows**]、[ **windows ユニバーサル** ]、[ **空のアプリ (windows ユニバーサル)**] の順にタップします。 **[名前]** ボックスに「MyApp」と入力し、**[OK]** をタップします。 Visual Studio によって初めてのプロジェクトが作成され、表示されます。 これで、アプリの設計を開始して、コードを追加できるようになりました。

## <a name="next-step"></a>次のステップ

[概要: プログラミング言語の選択](getting-started-choosing-a-programming-language.md)