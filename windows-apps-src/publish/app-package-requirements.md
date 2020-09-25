---
Description: Microsoft Store に申請するために、次のガイドラインに従ってアプリのパッケージを準備してください。
title: アプリ パッケージの要件
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, パッケージ要件, パッケージ, パッケージ形式, サポートされているバージョン, 提出
ms.localizationpriority: medium
ms.openlocfilehash: 848adbab20765a65ef4673219c55dfbf076e47e0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219775"
---
# <a name="app-package-requirements"></a>アプリ パッケージの要件

Microsoft Store に申請するために、次のガイドラインに従ってアプリのパッケージを準備してください。

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Microsoft Store に向けてアプリのパッケージを構築する前に

[Windows アプリ認定キットでアプリをテスト](../debug-test-perf/windows-app-certification-kit.md) したことを確認してください。 また、さまざまな種類のハードウェアでアプリをテストすることをお勧めします。 アプリが認定され、Microsoft Store から入手できるようになるまでの間、アプリをインストールして実行できるのは、開発者用ライセンスを持つコンピューター上のみになります。

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Microsoft Visual Studio を使ったアプリ パッケージのビルド

開発環境として Microsoft Visual Studio を使っている場合は、アプリ パッケージを迅速かつ簡単に作成するための組み込みツールが既に用意されています。 詳しくは、「[アプリのパッケージ化](../packaging/index.md)」をご覧ください。

> [!NOTE]
> すべてのファイル名に ANSI を使っていることを確認してください。 

Visual Studio でパッケージを作るときは、必ず、開発者アカウントに関連付けられている同じアカウントでサインインしてください。 パッケージ マニフェストの一部には、お使いのアカウントに関連する固有の詳細情報が含まれています。 この情報は自動的に検出されて追加されます。 マニフェストにこの追加情報が追加されていない場合、パッケージのアップロードでエラーが発生する可能性があります。 

アプリの UWP パッケージをビルドすると、Visual Studio では、msix または appx ファイル、または msixupload または .appxupload ファイルを作成できます。 UWP アプリの場合は、[ [パッケージ](upload-app-packages.md) ] ページで常に msixupload または .appxupload ファイルをアップロードすることをお勧めします。 Microsoft Store 用の UWP アプリのパッケージ化について詳しくは、「[Visual Studio での UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」をご覧ください。

アプリのパッケージに、信頼された証明機関が発行する証明書で署名する必要はありません。


### <a name="app-bundles"></a>アプリ バンドル

UWP アプリの場合、Visual Studio はアプリバンドル (. .msixbundle または .appxbundle) を生成して、ユーザーがダウンロードするアプリのサイズを小さくすることができます。 その利便性が発揮されるのは、言語固有のアセットや多様な画像倍率のアセット、特定バージョンの Microsoft DirectX に適用されるリソースを定義した場合などです。

> [!NOTE]
> 1 つのアプリ バンドルには、すべてのアーキテクチャ用のパッケージを含めることができます。

アプリ バンドルが使われている場合、ユーザーは、自分に関係したファイルだけをダウンロードすればよく、すべてのリソースをダウンロードする必要はありません。 アプリ バンドルについて詳しくは、「[アプリのパッケージ化](../packaging/index.md)」と「[Visual Studio で UWP アプリをパッケージ化する](/windows/msix/package/packaging-uwp-apps)」をご覧ください。


## <a name="building-the-app-package-manually"></a>手動によるアプリ パッケージのビルド

パッケージの作成に Visual Studio を使わない場合は、[パッケージ マニフェストを手動で作成](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually) する必要があります。

マニフェストの詳細や要件については、[アプリ パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest) に関するドキュメントをご覧ください。 認定に合格するには、マニフェストがパッケージ マニフェスト スキーマに従っている必要があります。

マニフェストには、アカウントとアプリに関するいくらかの具体的な情報を含める必要があります。 この情報は、ダッシュボードにあるアプリの概要ページの [**アプリの管理**] セクションで [アプリの ID の詳細情報を表示する](view-app-identity-details.md) ことで確認できます。

> [!NOTE]
> マニフェスト内の値は、大文字と小文字が区別されます。 スペースや句読点なども一致する必要があります。 注意して入力し、間違いがないか確認してください。


アプリバンドル (. .msixbundle または .appxbundle) は、別のマニフェストを使用します。 アプリ バンドル マニフェストの詳細や要件については、[バンドル マニフェスト](/uwp/schemas/bundlemanifestschema/bundle-manifest)に関するドキュメントをご覧ください。 .Msixbundle または .appxbundle では、含まれている各パッケージのマニフェストは、 [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)要素の**ProcessorArchitecture**属性を除き、同じ要素と属性を使用する必要があることに注意してください。

> [!TIP]
> パッケージを送信する前に、必ず [Windows アプリ認定キット](../debug-test-perf/windows-app-certification-kit.md) を実行してください。 これによって、認定や提出の失敗の原因となる可能性がある問題がマニフェストに含まれているかどうかを判断できます。


## <a name="package-format-requirements"></a>パッケージの形式の要件

アプリのパッケージは、次の要件に準拠している必要があります。

| アプリ パッケージの性質 | 要件                                                          |
|----------------------|----------------------------------------------------------------------|
| パッケージのサイズ         | .msixbundle または .appxbundle: バンドルあたりの最大 25 GB <br>. パッケージあたりの Windows 10:25 GB の最大サイズを対象とする msix または .appx パッケージ<br>Windows 8.1 を対象とする .appx パッケージ: パッケージあたり最大 8 GB <br> Windows 8 を対象とする .appx パッケージ: パッケージあたり最大 2 GB <br> Windows Phone 8.1 を対象とする .appx パッケージ: パッケージあたり最大 4 GB <br> .xap パッケージ: パッケージあたり最大 1 GB                                                                           |
| ブロック マップ ハッシュ     | SHA2-256 アルゴリズム                                                   |

> [!IMPORTANT]
> Windows Phone 8. x SDK を使用してビルドされた新しい XAP パッケージをアップロードすることはできなくなりました。 既に XAP パッケージと共にストアに格納されているアプリは、引き続き Windows 10 Mobile デバイスで動作します。 詳細については、こちらの [ブログ投稿](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)を参照してください。

## <a name="supported-versions"></a>サポートされているバージョン

UWP アプリの場合、すべてのパッケージは Microsoft Store によりサポートされている Windows 10 のバージョンをターゲットとする必要があります。 パッケージがサポートするバージョンは、アプリ マニフェストの [TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 要素の **MinVersion** 属性と **MaxVersionTested** 属性で指定されています。

現時点でサポートされているバージョンの範囲は以下のとおりです。 
- 最小: 10.0.10240.0
- 最大値: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>StoreManifest XML ファイル

StoreManifest.xml は、必要に応じてアプリ パッケージに含めることのできる構成ファイルです。 その目的は、Microsoft Store デバイス アプリとしてアプリを宣言する機能や、パッケージ マニフェストの対象外となるデバイスに適用される要件を宣言する機能などを有効にすることです。 使用する場合、StoreManifest.xml はアプリパッケージと一緒に送信され、アプリのメインプロジェクトのルートフォルダーに存在する必要があります。 詳しくは、「[StoreManifest スキーマ](/uwp/schemas/storemanifest/store-manifest-schema-portal)」をご覧ください。

 

 
