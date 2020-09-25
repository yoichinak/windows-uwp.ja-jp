---
description: アプリのパッケージをユーザーが使用できるようになるしくみと、特定のパッケージ シナリオを管理する方法について説明します。
title: アプリ パッケージ管理のガイダンス
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 497282632e9690cedd65788dcef18937d5c5cc74
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219165"
---
# <a name="guidance-for-app-package-management"></a>アプリ パッケージ管理のガイダンス

アプリのパッケージをユーザーが使用できるようになるしくみと、特定のパッケージ シナリオを管理する方法について説明します。

-   [OS のバージョンとパッケージの配布](#os-versions-and-package-distribution)
-   [以前に公開したアプリに Windows 10 用のパッケージを追加する](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [アプリを Microsoft Store から削除する](#removing-an-app-from-the-store)
-   [これまでサポートされていたデバイス ファミリ用のパッケージを削除する](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>OS のバージョンとパッケージの配布

さまざまなオペレーティング システムで異なる種類のパッケージを実行できます。 ユーザーのデバイスで複数のパッケージを実行できる場合、Microsoft Store は使用可能な最適のパッケージを提供します。

一般に、新しい OS バージョンでは、同じデバイス ファミリの以前の OS バージョンを対象にしたパッケージを実行できます。 Windows 10 デバイスは、以前にサポートされていたすべての OS バージョン (デバイスファミリごと) を実行できます。 Windows 10 のデスクトップ デバイスでは Windows 8.1 または Windows 8 用に構築されたアプリを実行でき、Windows 10 のモバイル デバイスでは Windows Phone 8.1、Windows Phone 8、さらには Windows Phone 7.x 用に構築されたアプリまで実行できます。 ただし、Windows 10 のお客様は、該当するデバイスファミリをターゲットとする UWP パッケージがアプリに含まれていない場合にのみ、これらのパッケージを取得します。

> [!IMPORTANT]
> Windows Phone 8. x SDK を使用してビルドされた新しい XAP パッケージをアップロードすることはできなくなりました。 既に XAP パッケージと共にストアに格納されているアプリは、引き続き Windows 10 Mobile デバイスで動作します。 詳細については、こちらの [ブログ投稿](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)を参照してください。


## <a name="removing-an-app-from-the-store"></a>アプリを Microsoft Store から削除する

ユーザーへのアプリの提供を停止し、事実上 "非公開" にする必要が生じることがあります。 これを行うには、**[アプリの概要]** ページで **[アプリの提供を停止する]** をクリックします。 アプリを入手不可にすることを確認すると、そのアプリは数時間以内に Store に表示されなくなり、([プロモーション コード](generate-promotional-codes.md)があり、Windows 10 デバイスを使用している場合を除き) 新しいユーザーがアプリを入手することはできなくなります。

> [!IMPORTANT]
> このオプションは、申請時に選択した[表示](choose-visibility-options.md#discoverability)設定よりも優先されます。 

このオプションは、申請を作成し、**[購入の停止]** オプションと同時に **[この製品を Microsoft Store で提供しますが、検索はできないようにします]** を選択した場合と同じ効果があります。 ただし、新しい申請を作成する必要はありません。

アプリを既に持っているユーザーは使用し続けることができ、もう一度アプリをダウンロードできることに注意してください (後で新しいパッケージを申請した場合には更新プログラムを入手することもできます)。

アプリを使用できなくなった後も、パートナーセンターに表示されます。 アプリをもう一度ユーザーに提供する場合は、[アプリの概要] ページで **[アプリを提供する]** をクリックします。 確認後、数時間以内に新しいユーザーがアプリを入手できるようになります (前回の申請時の設定により制限されている場合を除く)。

> [!NOTE]
> アプリの提供は継続しながら、特定の OS バージョンで新しいユーザーへの提供を終了する場合は、新しい申請を作成して、新規の取得を許可しない OS バージョン用のパッケージをすべて削除できます。 たとえば、以前に Windows Phone 8.1 と Windows 10 用のパッケージを提供していて、Windows Phone 8.1 の新しいユーザーへのアプリの提供を終了する場合は、申請から Windows Phone 8.1 用のパッケージを削除します。 更新プログラムの公開後、Windows Phone 8.1 では新しいユーザーがアプリを入手できなくなります (ただし、既に取得しているユーザーは使い続けることができます)。 ただし、Windows 10 では、引き続き新しいユーザーにアプリが提供されます。

## <a name="removing-packages-for-a-previously-supported-device-family"></a>これまでサポートされていたデバイス ファミリ用のパッケージを削除する

アプリで以前にサポートされていた特定のデバイスファミリ ( [拡張 sdk を使用](/uwp/extension-sdks/device-families-overview)したプログラミングに関するページを参照) のすべてのパッケージを削除した場合は、[ **パッケージ** ] ページで変更を保存する前に、これが目的であることを確認するメッセージが表示されます。

アプリが以前にサポートしていたデバイスファミリで実行可能なすべてのパッケージを削除する送信を発行すると、新しい顧客はそのデバイスファミリでアプリを取得できなくなります。 そのデバイス ファミリ向けのパッケージを提供するための別の更新プログラムは、後でいつでも公開することができます。

特定のデバイス ファミリをサポートするパッケージをすべて削除した場合でも、該当する種類のデバイスにアプリを既にインストールしているユーザーは、そのアプリを使うことができますが、後で提供される更新プログラムを入手することになります。

<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>以前に公開したアプリに Windows 10 用のパッケージを追加する

Windows 8.x および/または Windows Phone 2.x のパッケージのみを含むアプリがストアにある場合、Windows 10 用にアプリを更新するには、新しい送信を作成し、 [パッケージ](upload-app-packages.md) の手順の間に msixupload または .appxupload パッケージを追加する必要があります。 アプリが認定プロセスを通過した後、UWP パッケージは Windows 10 のお客様による新しい買収にも利用できます。

> [!NOTE]
> Windows 10 のユーザーが UWP パッケージを入手した場合、以前の OS バージョン用のパッケージを使うようにそのユーザーをロールバックすることはできません。 

Windows 10 パッケージのバージョン番号は、使用している Windows 8、Windows 8.1、または Windows Phone 8.1 パッケージのバージョン番号よりも大きくする必要があることに注意してください。 詳しくは、「[パッケージ バージョンの番号付け](package-version-numbering.md)」をご覧ください。

Microsoft Store 用の UWP アプリのパッケージ化について詳しくは、「[アプリのパッケージ化](../packaging/index.md)」をご覧ください。
