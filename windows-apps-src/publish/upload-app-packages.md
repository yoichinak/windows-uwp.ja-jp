---
Description: '[パッケージ] ページでは、送信しているアプリのすべてのパッケージファイル (.appxupload、.appx、.appxbundle、および/または .xap) をアップロードします。'
title: アプリ パッケージのアップロード
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10、uwp、パッケージ、アップロード、パッケージアップロード
ms.localizationpriority: medium
ms.openlocfilehash: 0b4fc0c9dfeed1183a1653b525d0f8cc8a62a4c1
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220215"
---
# <a name="upload-app-packages"></a>アプリ パッケージのアップロード

[ **パッケージ** ] ページでは、送信しているアプリのすべてのパッケージファイル (. msix、. msixupload、. .msixbundle、.appx、. .appxupload、.appxbundle) をアップロードします。 このページでは、同じアプリのすべてのパッケージをアップロードできます。また、顧客がアプリをダウンロードすると、ストアは各顧客に対して、デバイスに最適なパッケージを自動的に提供します。 開発者がパッケージをアップロードした後は、[特定の Windows 10 デバイス ファミリ (および該当する場合は以前の OS バージョン) に対して提供されるパッケージ](#device-family-availability)をランキング順に示すテーブルが表示されます。

> [!IMPORTANT]
> Windows Phone 8. x SDK を使用してビルドされた新しい XAP パッケージをアップロードすることはできなくなりました。 既に XAP パッケージと共にストアに格納されているアプリは、引き続き Windows 10 Mobile デバイスで動作します。 詳細については、こちらの [ブログ投稿](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)を参照してください。

パッケージに含まれる内容やパッケージの構成方法について詳しくは、「[アプリ パッケージの要件](app-package-requirements.md)」をご覧ください。 また、 [バージョン番号が特定の顧客に配信されるパッケージ](package-version-numbering.md) にどのように影響するか、および [さまざまなシナリオでパッケージを管理する](guidance-for-app-package-management.md)方法についても説明します。


## <a name="uploading-packages-to-your-submission"></a>申請へのパッケージのアップロード

パッケージをアップロードするには、アップロード フィールドにパッケージをドラッグするか、クリックしてファイルを参照します。 [ **パッケージ** ] ページでは、. msix、. msixupload、.msixbundle、.appx、.appxupload、および .appxbundle ファイルをアップロードできます。

> [!IMPORTANT]
> Windows 10 の場合は、. msix、.appx、.msixbundle、.appxbundle ではなく、msixupload または .appxupload ファイルをここでアップロードすることをお勧めします。  ストア用の UWP アプリのパッケージ化について詳しくは、「[Visual Studio での UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」をご覧ください。

アプリの[パッケージ フライト](package-flights.md)を作成すると、いずれかのパッケージ フライトからパッケージをコピーするオプションがドロップダウンに表示されます。 必要なパッケージが含まれているパッケージ フライトを選びます。 その後で、いずれかまたはすべてのパッケージを選んで、この申請に含めることができます。

検証中にパッケージでエラーが検出された場合は、何が間違っているかを知らせるメッセージが表示されます。 パッケージを削除して問題を修正してから、もう一度アップロードしてください。 また、問題を引き起こす可能性はあるが、申請の続行は妨げない事柄について通知する警告が表示されることもあります。


## <a name="device-family-availability"></a>デバイス ファミリの利用可否

パッケージのアップロードが正常に行われると、**[Device family availability]** (デバイス ファミリの利用可否) セクションに、特定の Windows 10 デバイス ファミリ (および該当する場合は以前の OS バージョン) に対して提供されるパッケージをランキング順に示すテーブルが表示されます。 このセクションでは、申請するアプリを特定の Windows 10 デバイス ファミリのユーザーに提供するかどうかも選択できます。

詳しくは、「[デバイス ファミリの利用可否](device-family-availability.md)」をご覧ください。


## <a name="package-details"></a>パッケージの詳細

アップロードしたパッケージは、ターゲットのオペレーティングシステム別にグループ化された一覧に表示されます。 パッケージの名前、バージョン、アーキテクチャが表示されます。 各パッケージのサポートされる言語、アプリの機能、ファイル サイズなどの詳しい情報については、**[詳細の表示]** をクリックします。

提出からパッケージを削除する必要がある場合は、各パッケージの **[詳細]** セクションの下部にある **[削除]** リンクをクリックします。


## <a name="removing-redundant-packages"></a>冗長なパッケージの削除

1 つ以上のパッケージが重複している場合、この申請から冗長なパッケージを削除することを提案する警告が表示されます。 これは多くの場合、以前にパッケージをアップロードした後で、同じユーザーをサポートする新しいバージョンのパッケージを提供するときに発生します。 この場合、これらのユーザーをサポートするより良い (より高いバージョンの) パッケージがあるため、ユーザーが冗長なパッケージを取得することはありません。

冗長なパッケージが存在することが検出された場合、すべての冗長なパッケージをこの申請から自動的に削除するオプションを提供します。 必要に応じて、パッケージを申請から個別に削除することもできます。


## <a name="gradual-package-rollout"></a>段階的なパッケージのロールアウト

申請が前に公開したアプリに対する更新プログラムの場合は、**[Roll out update gradually after this submission is published (to Windows 10 customers only)]** (この申請が公開された後で段階的に更新プログラムをロールアウトする (Windows 10 ユーザーのみ)) チェック ボックスが表示されます。 これにより、申請からパッケージを取得するユーザーの割合を選択でき、フィードバックや分析データを監視して、自信を持って更新プログラムのロールアウト範囲を広げることができます。 この割合は、新しい申請を作成することなく、いつでも増やす (または更新を停止する) ことができます。 

詳しくは、「[段階的なパッケージのロールアウト](gradual-package-rollout.md)」をご覧ください。


## <a name="mandatory-update"></a>必須の更新プログラム

申請が以前に公開したアプリに対する更新プログラムの場合、**[Make this update mandatory]** (この更新プログラムを必須にする) チェック ボックスが表示されます。 Windows.Services.Store API を使うことで、アプリがプログラムでパッケージの更新プログラムを確認し、更新されたパッケージをダウンロードしてインストールできるようにしてある場合は、このチェック ボックスをオンにすると、必須の更新の日時を設定できます。 このオプションを使うには、アプリの対象を Windows 10 Version 1607 以降にする必要があります。

詳しくは、「[アプリのパッケージの更新をダウンロードしてインストールする](../packaging/self-install-package-updates.md)」をご覧ください。

 




