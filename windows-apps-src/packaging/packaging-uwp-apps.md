---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: UWP アプリのパッケージ化
description: ユニバーサル Windows プラットフォーム (UWP) アプリを配布または販売するには、そのアプリのアプリ パッケージを作成する必要があります。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 1038913732c8b25a5baa3daf2c04176ff747a85f
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303593"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Visual Studio で UWP アプリをパッケージ化する

ユニバーサル Windows プラットフォーム (UWP) アプリを販売、または他のユーザーに配布するには、パッケージ化する必要があります。 Microsoft Store を通じてアプリを配布しない場合は、アプリ パッケージを直接デバイスにサイドローディングしたり、[Web インストール](installing-UWP-apps-web.md)を通じて配布したりすることができます。 この記事では、Visual Studio を使って UWP アプリ パッケージを構成、作成、テストする方法について説明します。 基幹業務 (LOB) アプリの管理および展開について詳しくは、[エンタープライズ アプリ管理に関するページ](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)をご覧ください。

Windows 10 では、[パートナーセンター](https://partner.microsoft.com/dashboard)にアプリケーションパッケージ、アプリバンドル、または完全なアプリパッケージアップロードファイルを送信できます。 これらのオプションの中で、アプリパッケージのアップロードファイルを送信すると、最適なエクスペリエンスが提供されます。

## <a name="types-of-app-packages"></a>アプリ パッケージの種類

- **アプリパッケージ (.appx または msix)**  
    デバイスにサイドローディングできる形式でアプリが含まれているファイル。 Visual Studio によって作成された1つのアプリパッケージファイルは、パートナーセンターに送信するためのものでは**なく**、サイドローディングとテストの目的でのみ使用する必要があります。 パートナーセンターにアプリを送信する場合は、アプリパッケージのアップロードファイルを使用します。  

- **アプリバンドル (.appxbundle または .msixbundle)**  
    アプリ バンドルは、複数のアプリ パッケージを含めることができるパッケージの種類であり、それぞれ特定のデバイス アーキテクチャをサポートするようにビルドされます。 たとえば、アプリ バンドルには x86、x64、および ARM 構成用の別個のアプリ パッケージを含めることができます。 アプリ バンドルによってできる限り広範なデバイスでアプリが利用できるようになるため、アプリ バンドルは可能であれば必ず生成してください。  

- **アプリケーションパッケージのアップロードファイル (. .appxupload または. msixupload)**  
    さまざまなプロセッサ アーキテクチャをサポートする複数のアプリ パッケージまたは 1 つのアプリ バンドルを含めることができる 1 つのファイルです。 アプリパッケージのアップロードファイルには、アプリが Microsoft Store に発行された後に[アプリのパフォーマンスを分析](https://docs.microsoft.com/windows/uwp/publish/analytics)するためのシンボルファイルも含まれています。 このファイルは、発行のためにパートナーセンターに送信することを目的として、Visual Studio でアプリをパッケージ化する場合に自動的に作成されます。

アプリ パッケージを準備して作成する手順の概要は次のとおりです。

1.  [アプリ パッケージを作成する前に](#before-packaging-your-app)。 次の手順に従って、パートナーセンターへの送信用にアプリをパッケージ化する準備ができていることを確認します。
2.  [アプリ パッケージを構成する](#configure-an-app-package)。 Visual Studio マニフェスト デザイナーを使って、パッケージを構成します。 たとえば、タイル画像を追加し、アプリでサポートされる向きを選びます。
3.  [アプリ パッケージ アップロード ファイルを作成する](#create-an-app-package-upload-file)。 Visual Studio のアプリ パッケージ ウィザードを使ってアプリ パッケージを作成してから、Windows アプリ認定キットを使ってそのパッケージを認定します。
4.  [アプリ パッケージをサイドローディングする](#sideload-your-app-package)。 アプリをサイドローディングした後、そのアプリが期待どおりに動作することをテストできます。

これらの手順を完了したら、アプリを配布できるようになります。 販売を予定していない基幹業務 (LOB) アプリがある場合は、それが社内ユーザー専用であるため、このアプリをサイドロードして、任意の Windows 10 デバイスにインストールすることができます。

## <a name="before-packaging-your-app"></a>アプリ パッケージを作成する前に

1.  **アプリをテストします。** パートナーセンターへの送信用にアプリをパッケージ化する前に、サポートする予定のすべてのデバイスファミリで期待どおりに動作することを確認してください。 これらのデバイス ファミリには、デスクトップ、モバイル、Surface Hub、Xbox、IoT デバイスなどが含まれる場合があります。 Visual Studio を使用したアプリの配置とテストの詳細については、「 [UWP アプリの配置とデバッグ](../debug-test-perf/deploying-and-debugging-uwp-apps.md)」を参照してください。
2.  **アプリを最適化します。** Visual Studio のプロファイリングおよびデバッグ ツールを使って、UWP アプリのパフォーマンスを最適化できます。 たとえば、UI 応答のタイムライン ツール、メモリ使用率のツール、CPU 使用率のツールを使えます。 これらのコマンド ライン ツールについて詳しくは、[プロファイリング機能ツアーに関するページ](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour)をご覧ください。
3.  **.NET ネイティブ互換性を確認します ( C# VB とアプリの場合)。** ユニバーサル Windows プラットフォームには、アプリの実行時のパフォーマンスを向上させるネイティブ コンパイラがあります。 この変更により、新しいコンパイル環境でアプリをテストする必要があります。 既定では、**リリース** ビルド構成により、.NET ネイティブ ツール チェーンが可能であるため、重要なのは、この**リリース**構成でアプリをテストし、想定どおりにアプリが動作することを確認することです。 .NET ネイティブで発生する可能性のあるいくつかの一般的なデバッグの問題について詳しくは、[.NET Native Windows ユニバーサル アプリのデバッグ](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/)に関するブログをご覧ください。

## <a name="configure-an-app-package"></a>アプリ パッケージを構成する

アプリ マニフェスト ファイル (Package.appxmanifest) は、アプリ パッケージの作成に必要なプロパティと設定が含まれている XML ファイルです。 たとえば、アプリ マニフェスト ファイル内のプロパティには、アプリのタイルとして使う画像や、ユーザーがデバイスを回転するときにアプリでサポートされる向きを定義します。

Visual Studio のマニフェスト デザイナーを使えば、生の XML を編集することなくマニフェスト ファイルを更新できます。

### <a name="configure-a-package-with-the-manifest-designer"></a>マニフェスト デザイナーを使ってパッケージを構成する

1.  **ソリューション エクスプローラー**で、UWP アプリのプロジェクト ノードを展開します。
2.  **[Package.appxmanifest]** ファイルをダブルクリックします。 マニフェスト ファイルが既に XML コード ビューで開かれている場合は、ファイルを閉じるよう指示するプロンプトが Visual Studio で表示されます。
3.  この時点で、アプリをどのように構成するかを決めることができます。 各タブには、アプリについて構成可能な情報や、さらに情報が必要なときのためのリンクがあります。  
    ![Visual Studio マニフェスト デザイナー](images/packaging-screen1.jpg)

    [**ビジュアル資産**] タブで、UWP アプリに必要なすべての画像があることを確認します。

    [**パッケージ化**] タブで、公開するデータを入力できます。 ここで、アプリの署名に使う証明書を選べます。 すべての UWP アプリは証明書で署名する必要があります。

    > [!NOTE]
    > Visual Studio 2019 以降では、UWP プロジェクトで一時的な証明書が生成されなくなりました。 証明書を作成またはエクスポートするには、[この記事](create-certificate-package-signing.md)で説明されている PowerShell コマンドレットを使用します。

    > [!IMPORTANT]
    > Microsoft Store でアプリを公開する場合、アプリが信頼済み証明書で自動的に署名されます。 これにより、ユーザーは関連付けられているアプリの署名証明書をインストールしなくてもアプリをインストールして実行できます。

    アプリを公開せず、アプリ パッケージをサイドローディングするだけの場合、まずパッケージを信頼する必要があります。 パッケージを信頼するには、証明書がユーザーのデバイスにインストールされていなければなりません。 サイドローディングについて詳しくは、「[デバイスを開発用に有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)」をご覧ください。

4.  アプリに必要な編集を行った後に、**Package.appxmanifest** ファイルを保存します。

Microsoft Store を使用してアプリを配布する場合は、Visual Studio でパッケージをストアに関連付けることができます。 これを行うには、ソリューションエクスプローラーでプロジェクト名を右クリックし、[**ストア**->] [**アプリをストアと関連付ける**] の順に選択します。 これは、次のセクションで説明する、**アプリパッケージの作成**ウィザードでも行うことができます。 アプリを関連付けると、マニフェスト デザイナーの [パッケージ化] タブの一部のフィールドが自動的に更新されます。

## <a name="create-an-app-package-upload-file"></a>アプリ パッケージ アップロード ファイルの作成

Microsoft Store 経由でアプリを配布するには、アプリパッケージ (.appx または msix)、アプリケーションバンドル (.appxbundle または .msixbundle)、またはアプリケーションパッケージのアップロードファイル (.appxupload または msixupload) を作成し、パッケージ[アプリをパートナーセンターに送信](https://docs.microsoft.com/windows/uwp/publish/app-submissions)する必要があります。 アプリパッケージまたはアプリケーションバンドルをパートナーセンターだけに送信することもできますが、アプリパッケージのアップロードファイルを送信することをお勧めします。 アプリパッケージのアップロードファイルは、Visual Studio のアプリパッケージの**作成**ウィザードを使用して作成するか、既存のアプリパッケージまたはアプリバンドルから手動で作成することができます。

>[!NOTE]
> アプリパッケージ (.appx または msix) またはアプリバンドル (.appxbundle または .msixbundle) を手動で作成する場合は、「 [MakeAppx を使用したアプリパッケージの作成](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)」を参照してください。

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Visual Studio を使用してアプリパッケージのアップロードファイルを作成する

1.  **ソリューション エクスプローラー**で、UWP アプリ プロジェクトのソリューションを開きます。
2.  プロジェクトを右クリックし、 **[Microsoft Store]** -> **[アプリ パッケージの作成]** の順に選択します。 このオプションが無効になっているか、まったく表示されない場合は、プロジェクトがユニバーサル Windows プロジェクトであることを確認します。  
    ![アプリパッケージを作成するためのナビゲーションを含むコンテキストメニュー](images/packaging-screen2.jpg)

    **アプリ パッケージの作成**ウィザードが表示されます。

3.  最初のダイアログで**新しいアプリ名を使用して [Microsoft Store にアップロードするパッケージを作成**します] を選択し、[**次へ**] をクリックします。  
    ![表示されたパッケージの作成ダイアログウィンドウ](images/packaging-screen3.jpg)

    プロジェクトがストア内のアプリに既に関連付けられている場合は、関連付けられているストアアプリのパッケージを作成するオプションもあります。 **サイドローディング用のパッケージを作成**することを選択した場合、パートナーセンターへの送信では、Visual Studio によってアプリパッケージのアップロード (. msixupload または .appxupload) ファイルが生成されません。 アプリをサイドローディングして社内デバイスで実行するだけの場合やテスト目的の場合は、このオプションを選ぶことができます。 サイドローディングについて詳しくは、「[デバイスを開発用に有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)」をご覧ください。
4.  次のページで、開発者アカウントを使用してパートナーセンターにサインインします。 まだ開発者アカウントがない場合は、ウィザードで作成できます。
    ![アプリ名を選択してアプリパッケージウィンドウを作成する](images/packaging-screen4.jpg)
5.  アカウントに現在登録されているアプリの一覧からパッケージのアプリ名を選択するか、パートナーセンターでまだ予約していない場合は新しいアプリを予約します。  
6.  必ず、 **[パッケージの選択と構成]** ダイアログ ボックスで 3 つのアーキテクチャ構成 (x86、x64、ARM) をすべて選択し、できるだけ広範なデバイスにアプリを展開できるようにしてください。 [**アプリケーション バンドルの生成**] ボックスで [**常に行う**] を選びます。 アプリバンドル (.appxbundle または .msixbundle) は、プロセッサアーキテクチャの種類ごとに構成されたアプリパッケージのコレクションを含むため、1つのアプリパッケージファイルよりも優先されます。 アプリバンドルを生成することを選択した場合、アプリバンドルは、デバッグおよびクラッシュ分析情報と共に、最終的なアプリケーションパッケージアップロード (. .appxupload または msixupload) ファイルに含まれます。 どのアーキテクチャを選べばよいかわからない場合や、各種デバイスにより使用されるアーキテクチャについて詳しく調べる場合は、「[アプリ パッケージのアーキテクチャ](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)」をご覧ください。  
    ![パッケージ構成を表示してアプリパッケージウィンドウを作成します](images/packaging-screen5.jpg)
7.  アプリが公開された後にパートナーセンターから[アプリのパフォーマンスを分析](https://docs.microsoft.com/windows/uwp/publish/analytics)するには、完全な PDB シンボルファイルを含めます。 バージョンの番号付けやパッケージの出力場所など、他の詳細情報を構成します。
8.  **[作成]** をクリックして、アプリ パッケージを生成します。 手順 3. で [ **Microsoft Store オプションにアップロードするパッケージを作成**し、パートナーセンターへの送信用のパッケージを作成する] を選択した場合は、ウィザードによってパッケージのアップロード (. .appxupload または msixupload) ファイルが作成されます。 手順 3. で**サイドローディング用のパッケージを作成**することを選択した場合、ウィザードでは、手順 6. で選択した内容に基づいて、単一のアプリパッケージまたはアプリバンドルが作成されます。
9. アプリが正常にパッケージ化されると、このダイアログが表示され、指定された出力場所からアプリパッケージアップロードファイルを取得できます。 この時点で、[ローカルコンピューターまたはリモートコンピューターでアプリパッケージを検証](#validate-your-app-package)し、ストアの[送信を自動化](#automate-store-submissions)できます。
    ![検証オプションが表示された、パッケージ作成の完了ウィンドウ](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>アプリパッケージのアップロードファイルを手動で作成する

1. 次のファイルをフォルダーに配置します。
    - 1つ以上のアプリパッケージ (......) またはアプリバンドル (. .msixbundle または .appxbundle)。
    - .Appxsym ファイル。 これは、パートナーセンターの[クラッシュ分析](../publish/health-report.md)に使用されるアプリのパブリックシンボルを含む、圧縮された .pdb ファイルです。 このファイルは省略できますが、実行すると、アプリケーションでクラッシュ分析またはデバッグ情報を使用できなくなります。
2. フォルダーを Zip 圧縮します。
3. Zip 形式のフォルダー拡張子の名前を .zip から msixupload または .appxupload に変更します。

## <a name="validate-your-app-package"></a>アプリパッケージを検証する

ローカルまたはリモートコンピューターで認定を取得するために、パートナーセンターに送信する前にアプリを検証します。 アプリ パッケージのデバッグ ビルドではなくリリース ビルドのみを検証できます。 パートナーセンターへのアプリの送信の詳細については、「[アプリの送信](https://docs.microsoft.com/windows/uwp/publish/app-submissions)」を参照してください。

### <a name="validate-your-app-package-locally"></a>アプリケーションパッケージをローカルで検証する

1. **アプリパッケージの作成**ウィザードの [最後の**パッケージ作成が完了しまし**た] ページで、[**ローカルコンピューター** ] オプションを選択したままにして、[ **Windows アプリ認定キットを起動**する] をクリックします。 Windows アプリ認定キットでアプリをテストする方法について詳しくは、「[Windows アプリ認定キット](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)」をご覧ください。

    Windows アプリ認定キット (WACK) は、さまざまなテストを実行し、結果を返します。 より具体的な情報については、「[Windows アプリ認定キットのテスト](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)」をご覧ください。

    テストに使用するリモート Windows 10 デバイスがある場合は、そのデバイスに Windows アプリ認定キットを手動でインストールする必要があります。 この手順については、次のセクションで説明します。 その後、[**リモート コンピューター**] を選び、[**Windows アプリ認定キットを起動する**] をクリックしてリモート デバイスに接続し、検証テストを実行します。

2. WACK が完了し、アプリが認定に合格すると、パートナーセンターにアプリを送信する準備が整います。 必ず正しいファイルをアップロードしてください。 ファイルの既定の場所は、ソリューション`\[AppName]\AppPackages`のルートフォルダーにあり、.appxupload または msixupload ファイル拡張子で終了します。 名前は、の形式`[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload`になります。または、すべてのパッケージアーキテクチャが選択されたアプリバンドルを選択した場合は`[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload`になります。

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>リモート Windows 10 デバイスでアプリパッケージを検証する

1. 「開発用の[デバイスを有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)」の手順に従って、開発用に Windows 10 デバイスを有効にします。
    >[!IMPORTANT]
    > Windows 10 のリモート ARM デバイスでアプリパッケージを検証することはできません。
2. Visual Studio のリモート ツールをダウンロードしてインストールします。 これらのツールを使って Windows アプリ認定キットをリモートで実行します。 これらのツールについてダウンロード場所など詳しくは、[リモート コンピューターでの UWP アプリの実行](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015)に関するページをご覧ください。
3. 必要な[Windows アプリ認定キット](https://go.microsoft.com/fwlink/p/?LinkID=309666)をダウンロードし、リモート windows 10 デバイスにインストールします。
4. **パッケージの作成が完了しました**ウィザードのページで、[**リモート コンピューター**] オプション ボタンを選び、[**テスト接続**] ボタンの横にある省略記号ボタンをクリックします。
    >[!NOTE]
    > [**リモートコンピューター** ] オプションボタンは、検証をサポートするソリューション構成を少なくとも1つ選択した場合にのみ使用できます。 WACK でアプリをテストする方法について詳しくは、「[Windows アプリ認定キット](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)」をご覧ください。
5. サブネットの内部にあるデバイスの形式を指定するか、サブネットの外部にあるデバイスのドメイン ネーム サーバー (DNS) 名または IP アドレスを指定します。
6. Windows 資格情報を使ってデバイスにログオンする必要がない場合は、 **[認証モード]** ボックスの一覧で **[なし]** を選びます。
7. **[選択]** 、 **[Windows アプリ認定キットを起動する]** の順に選びます。 デバイスでリモート ツールが実行されていれば、Visual Studio がデバイスに接続されてから、検証テストが実行されます。 「[Windows アプリ認定キットのテスト](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)」をご覧ください。

## <a name="automate-store-submissions"></a>ストアの送信を自動化する

Visual Studio 2019 以降では、.appxupload ファイルを IDE から直接 Microsoft Store に送信できます。そのためには、[ **Windows アプリ認定キットの検証後に Microsoft Store に自動的に送信する**] オプションを選択します。[アプリパッケージの作成ウィザード](#create-your-app-package-upload-file-using-visual-studio)。 この機能は、アプリの発行に必要なパートナーセンターのアカウント情報にアクセスするために Azure Active Directory を利用します。 この機能を使用するには、Azure Active Directory をパートナーセンターアカウントに関連付け、送信に必要ないくつかの資格情報を取得する必要があります。

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Azure Active Directory をパートナーセンターアカウントに関連付ける

自動ストア送信に必要な資格情報を取得するには、まず、[パートナーセンターのダッシュボード](https://partner.microsoft.com/dashboard)で次の手順に従う必要があります (まだ行っていない場合)。

1. [パートナーセンターアカウントを組織の Azure Active Directory に関連付け](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center)ます。 組織で Office 365 または Microsoft の他のビジネス サービスが既に使用されている場合は、既に Azure AD をお持ちです。 それ以外の場合は、追加料金なしで、パートナーセンター内から新しい Azure AD テナントを作成することができます。

2. [パートナーセンターアカウントに Azure AD アプリケーションを追加](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account)します。 この Azure AD アプリケーションは、デベロッパーセンターアカウントの送信にアクセスするために使用するアプリまたはサービスを表します。 このアプリケーションを**マネージャー**ロールに割り当てる必要があります。 このアプリケーションが既に Azure AD ディレクトリに存在する場合、 **[Azure AD アプリケーションの追加]** ページで選んでデベロッパー センター アカウントに追加できます。 それ以外の場合、 **[Azure AD アプリケーションの追加]** ページで新しい Azure AD アプリケーションを作成できます。

### <a name="retrieve-the-credentials-required-for-submissions"></a>送信に必要な資格情報を取得する

次に、送信に必要なパートナーセンターの資格情報 ( **Azure テナント id**、**クライアント ID** 、および**クライアントキー**) を取得できます。

1. [パートナーセンターのダッシュボード](https://partner.microsoft.com/dashboard)にアクセスし、Azure AD の資格情報でサインインします。

2. パートナーセンターのダッシュボードで、歯車アイコン (ダッシュボードの右上隅の近く) を選択し、[**開発者向け設定**] を選択します。

3. 左側のウィンドウの [**設定**] メニューで、[**ユーザー**] をクリックします。

4. アプリケーションの [設定] にアクセスするには、Azure AD アプリケーションの名前をクリックします。 このページで、[**テナント id** ] と [**クライアント ID** ] の値をコピーします。

5. [**キー** ] セクションで、[**新しいキーの追加**] をクリックします。 次の画面で、クライアントシークレットに対応する**キー**値をコピーします。 このページから移動すると、この情報に再度アクセスできなくなります。そのため、この情報は失わないようにしてください。 詳しくは、「[Azure AD アプリケーションのキーを管理する方法](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application)」をご覧ください。

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Visual Studio で自動ストア送信を構成する

前の手順を完了したら、Visual Studio 2019 で自動的にストアを送信するように構成できます。

1. [アプリパッケージの作成ウィザード](#create-your-app-package-upload-file-using-visual-studio)の最後で、[ **Windows アプリ認定キットの検証後に Microsoft Store に自動的に送信する**] を選択し、[**再構成**] をクリックします。

2. [ **Microsoft Store 送信設定の構成**] ダイアログで、AZURE テナント Id、クライアント id、およびクライアントキーを入力します。

    ![Microsoft Store 送信設定の構成](images/packaging-screen8.jpg)

    > [!Important]
    > 資格情報をプロファイルに保存し、今後の送信で使用することができます

3. **[OK]** をクリックします。

WACK テストが完了すると、送信が開始されます。 [**検証と発行**] ウィンドウで、送信の進行状況を追跡できます。

![検証と発行の進行状況](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>アプリ パッケージをサイドローディングする

UWP アプリパッケージでは、デスクトップアプリの場合と同じように、アプリはデバイスにインストールされません。 通常、Microsoft Store から UWP アプリをダウンロードし、デバイスにアプリをインストールします。 アプリは Microsoft Store で公開せずにインストールできます (サイドローディング)。 これにより、作成したアプリケーションパッケージファイルを使用して、アプリをインストールしてテストできます。 基幹業務 (LOB) アプリのように、ストアで販売しないアプリの場合は、そのアプリをサイドローディングして、社内の他のユーザーが使えるようにできます。

対象のデバイスでアプリをサイドロードする前に、デバイスを[開発用に有効に](../get-started/enable-your-device-for-development.md)する必要があります。

Windows 10 Mobile デバイスにアプリをサイドロードするには、 [Winappdeploycmd](install-universal-windows-apps-with-the-winappdeploycmd-tool.md)ツールを使用します。 デスクトップ、ラップトップ、およびタブレットの場合は、次の手順に従います。

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Windows 10 記念日更新以降のアプリパッケージのサイドロード

Windows 10 周年更新プログラム (Windows 10、バージョン 1607) で導入されたアプリパッケージは、アプリケーションパッケージファイルをダブルクリックするだけでインストールできます。 これを使用するには、アプリパッケージまたはアプリバンドルファイルに移動し、ダブルクリックします。 [アプリインストーラー](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root)が起動し、基本的なアプリ情報に加えて、[インストール] ボタン、インストールの進行状況バー、関連するエラーメッセージが表示されます。

![Contoso というサンプル アプリをインストールするために、アプリのインストーラーが表示されます。](images/appinstaller-screen.png)

> [!NOTE]
> アプリインストーラーでは、アプリがデバイスによって信頼されていることを前提としています。 開発者アプリまたはエンタープライズ アプリをサイドローディングする場合、デバイス上の信頼されたユーザー ストアまたは信頼された発行元証明機関ストアに署名証明書をインストールする必要があります。 この方法がわからない場合は、[テスト証明書のインストール](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)に関するページをご覧ください。

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>以前のバージョンの Windows でのアプリパッケージのサイドロード

1.  インストールするアプリ バージョンのフォルダーをターゲット デバイスにコピーします。

    アプリ バンドルを作成した場合は、バージョン番号に基づいたフォルダーと `*_Test` フォルダーが必要です。 たとえば、2 つのフォルダーは次のようになります (インストールするバージョンは 1.0.2.0)。

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    アプリ バンドルがない場合は、適切な構造になっているフォルダーと対応する `*_Test` フォルダーをコピーします。 次の 2 つのフォルダーは、x64 アーキテクチャのアプリ パッケージとその `*_Test` フォルダーの例です。

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  ターゲット デバイスで `*_Test` フォルダーを開きます。
3.  **Add-AppDevPackage.ps1** ファイルを右クリックします。 **[PowerShell で実行]** を選んで、画面の指示に従います。  
    ![エクスプローラーに表示された PowerShell スクリプトに移動](images/packaging-screen7.jpg)

    アプリパッケージがインストールされると、PowerShell ウィンドウに次のメッセージが表示されます。**アプリが正常にインストールされました。**
    >[!TIP]
    > タブレットでショートカットメニューを開くには、右クリックする画面をタッチし、完全な円が表示されるまで押したまま、指を離します。 指を放した後にショートカット メニューが表示されます。

4.  スタート ボタンをクリックし、アプリをアプリ名で検索してから起動します。
