---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: UWP アプリのパッケージ化
description: ユニバーサル Windows プラットフォーム (UWP) アプリを配布または販売するには、そのアプリのアプリ パッケージを作成する必要があります。
ms.date: 06/10/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 265e034b264cf82bacfa5a32141eb5d999d57108
ms.sourcegitcommit: aa5a055e3ff9ee9defc73ed9567196d59f59542a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2019
ms.locfileid: "66825033"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Visual Studio で UWP アプリをパッケージ化する

ユニバーサル Windows プラットフォーム (UWP) アプリを販売、または他のユーザーに配布するには、パッケージ化する必要があります。 Microsoft Store を通じてアプリを配布しない場合は、アプリ パッケージを直接デバイスにサイドローディングしたり、[Web インストール](installing-UWP-apps-web.md)を通じて配布したりすることができます。 この記事では、Visual Studio を使って UWP アプリ パッケージを構成、作成、テストする方法について説明します。 基幹業務 (LOB) アプリの管理および展開について詳しくは、[エンタープライズ アプリ管理に関するページ](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)をご覧ください。

アプリ パッケージをアプリのバンドル、またはに完成したアプリ パッケージのアップロード ファイルを送信する Windows 10 で[パートナー センター](https://partner.microsoft.com/dashboard)します。 これらのオプションのアプリ パッケージのアップロード ファイルを送信する最適なエクスペリエンスが提供されます。

## <a name="types-of-app-packages"></a>アプリ パッケージの種類

- **アプリ パッケージ (.appx または .msix)**  
    デバイスにサイドローディングできる形式でアプリが含まれているファイル。 Visual Studio によって作成されたいずれか 1 つのアプリ パッケージ ファイルが**いない**テスト目的でのみのパートナー センターに送信するためのものし、サイドローディングのために使用する必要があります。 パートナー センターへのアプリを送信する場合は、アプリ パッケージのアップロード ファイルを使用します。  

- **アプリケーション バンドル (.appxbundle または .msixbundle)**  
    アプリ バンドルは、複数のアプリ パッケージを含めることができるパッケージの種類であり、それぞれ特定のデバイス アーキテクチャをサポートするようにビルドされます。 たとえば、アプリ バンドルには x86、x64、および ARM 構成用の別個のアプリ パッケージを含めることができます。 アプリ バンドルによってできる限り広範なデバイスでアプリが利用できるようになるため、アプリ バンドルは可能であれば必ず生成してください。  

- **アプリ パッケージのアップロード ファイル (.appxupload または .msixupload)**  
    さまざまなプロセッサ アーキテクチャをサポートする複数のアプリ パッケージまたは 1 つのアプリ バンドルを含めることができる 1 つのファイルです。 アプリ パッケージのアップロード ファイルは、シンボル ファイルも含まれています。[アプリのパフォーマンスを分析](https://docs.microsoft.com/windows/uwp/publish/analytics)後、アプリを Microsoft Store に発行されています。 このファイルは自動的に作成する発行のためのパートナー センターに送信することを目的として Visual Studio でアプリをパッケージ化している場合。

アプリ パッケージを準備して作成する手順の概要は次のとおりです。

1.  [アプリ パッケージを作成する前に](#before-packaging-your-app)。 パートナー センターの送信にパッケージ化する準備ができて、アプリのためにこれらの手順に従います。
2.  [アプリ パッケージを構成する](#configure-an-app-package)。 Visual Studio マニフェスト デザイナーを使って、パッケージを構成します。 たとえば、タイル画像を追加し、アプリでサポートされる向きを選びます。
3.  [アプリ パッケージ アップロード ファイルを作成する](#create-an-app-package-upload-file)。 Visual Studio のアプリ パッケージ ウィザードを使ってアプリ パッケージを作成してから、Windows アプリ認定キットを使ってそのパッケージを認定します。
4.  [アプリ パッケージをサイドローディングする](#sideload-your-app-package)。 アプリをサイドローディングした後、そのアプリが期待どおりに動作することをテストできます。

これらの手順を完了したら、アプリを配布できるようになります。 内部ユーザーのみにあるために販売する予定がない基幹業務 (LOB) アプリがあれば、サイドロードに任意の Windows 10 デバイスにインストールするには、このアプリをできます。

## <a name="before-packaging-your-app"></a>アプリ パッケージを作成する前に

1.  **アプリをテストします。** パートナー センター送信するアプリをパッケージ化する前に、サポートを計画しているすべてのデバイス ファミリで期待どおりに動作を確認します。 これらのデバイス ファミリには、デスクトップ、モバイル、Surface Hub、Xbox、IoT デバイスなどが含まれる場合があります。 展開して、Visual Studio を使用して、アプリのテストの詳細については、次を参照してください。[の展開と UWP アプリのデバッグ](../debug-test-perf/deploying-and-debugging-uwp-apps.md)します。
2.  **アプリを最適化します。** Visual Studio のプロファイリングおよびデバッグ ツールを使って、UWP アプリのパフォーマンスを最適化できます。 たとえば、UI 応答のタイムライン ツール、メモリ使用率のツール、CPU 使用率のツールを使えます。 これらのコマンド ライン ツールについて詳しくは、[プロファイリング機能ツアーに関するページ](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour)をご覧ください。
3.  **.NET ネイティブの互換性を確認する (vb とC#アプリ)。** ユニバーサル Windows プラットフォームには、アプリの実行時のパフォーマンスを向上させるネイティブ コンパイラがあります。 この変更により、新しいコンパイル環境でアプリをテストする必要があります。 既定では、**リリース** ビルド構成により、.NET ネイティブ ツール チェーンが可能であるため、重要なのは、この**リリース**構成でアプリをテストし、想定どおりにアプリが動作することを確認することです。 .NET ネイティブで発生する可能性のあるいくつかの一般的なデバッグの問題について詳しくは、[.NET Native Windows ユニバーサル アプリのデバッグ](https://blogs.msdn.microsoft.com/devops/2015/07/29/debugging-net-native-windows-universal-apps/)に関するブログをご覧ください。

## <a name="configure-an-app-package"></a>アプリ パッケージを構成する

アプリ マニフェスト ファイル (Package.appxmanifest) は、アプリ パッケージの作成に必要なプロパティと設定が含まれている XML ファイルです。 たとえば、アプリ マニフェスト ファイル内のプロパティには、アプリのタイルとして使う画像や、ユーザーがデバイスを回転するときにアプリでサポートされる向きを定義します。

Visual Studio のマニフェスト デザイナーを使えば、生の XML を編集することなくマニフェスト ファイルを更新できます。

### <a name="configure-a-package-with-the-manifest-designer"></a>マニフェスト デザイナーを使ってパッケージを構成する

1.  **ソリューション エクスプローラー**で、UWP アプリのプロジェクト ノードを展開します。
2.  **[Package.appxmanifest]** ファイルをダブルクリックします。 マニフェスト ファイルが既に XML コード ビューで開かれている場合は、ファイルを閉じるよう指示するプロンプトが Visual Studio で表示されます。
3.  この時点で、アプリをどのように構成するかを決めることができます。 各タブには、アプリについて構成可能な情報や、さらに情報が必要なときのためのリンクがあります。  
    ![Visual Studio マニフェスト デザイナー](images/packaging-screen1.jpg)

    **[ビジュアル資産]** タブで、UWP アプリに必要なすべての画像があることを確認します。

    **[パッケージ化]** タブで、公開するデータを入力できます。 ここで、アプリの署名に使う証明書を選べます。 すべての UWP アプリは証明書で署名する必要があります。

    >[!IMPORTANT]
    >Microsoft Store でアプリを公開する場合、アプリが信頼済み証明書で自動的に署名されます。 これにより、ユーザーは関連付けられているアプリの署名証明書をインストールしなくてもアプリをインストールして実行できます。

    アプリを公開せず、アプリ パッケージをサイドローディングするだけの場合、まずパッケージを信頼する必要があります。 パッケージを信頼するには、証明書がユーザーのデバイスにインストールされていなければなりません。 サイドローディングについて詳しくは、「[デバイスを開発用に有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)」をご覧ください。

4.  アプリに必要な編集を行った後に、**Package.appxmanifest** ファイルを保存します。

Microsoft Store を使用してアプリを配布する場合、Visual Studio は、ストアと、パッケージを関連付けることができます。 これを行うには、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、選択**ストア**->**アプリケーションをストアと関連付ける**します。 実行することもできます、**アプリ パッケージの作成**ウィザードで、次のセクションで説明します。 アプリを関連付けると、マニフェスト デザイナーの [パッケージ化] タブの一部のフィールドが自動的に更新されます。

## <a name="create-an-app-package-upload-file"></a>アプリ パッケージ アップロード ファイルの作成

Microsoft Store からのアプリを配布するアプリ パッケージ (.appx または .msix)、アプリのバンドル (.appxbundle または .msixbundle)、またはアプリ パッケージのアップロード ファイル (.appxupload または .msixupload) を作成する必要がありますと[パートナー センターをパッケージ化されたアプリの提出](https://docs.microsoft.com/windows/uwp/publish/app-submissions). パートナー センターだけで、アプリ パッケージまたはアプリ バンドルを送信することはできますが、アプリ パッケージのアップロード ファイルを送信することをお勧めします。 使用して、アプリ パッケージのアップロード ファイルを作成することができます、**アプリ パッケージの作成**には、Visual Studio は、ウィザードには、既存のアプリ パッケージまたはアプリ バンドルから手動で作成できます。

>[!NOTE]
> アプリ パッケージ (.appx または .msix) またはアプリ バンドル (.appxbundle または .msixbundle) を手動で作成する場合を参照してください。 [MakeAppx.exe ツールを使用してアプリ パッケージの作成](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)です。

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Visual Studio を使用して、アプリ パッケージのアップロード ファイルを作成します。

1.  **ソリューション エクスプローラー**で、UWP アプリ プロジェクトのソリューションを開きます。
2.  プロジェクトを右クリックし、 **[Microsoft Store]** -> **[アプリ パッケージの作成]** の順に選択します。 このオプションが無効になっているか、まったく表示されない場合は、プロジェクトがユニバーサル Windows プロジェクトであることを確認します。  
    ![アプリ パッケージの作成へのナビゲーションを含むコンテキスト メニュー](images/packaging-screen2.jpg)

    **アプリ パッケージの作成**ウィザードが表示されます。

3.  選択**新しいアプリ名を使用して Microsoft Store にアップロードするパッケージを作成する**で最初のダイアログ ボックスをクリック**次**。  
    ![パッケージのダイアログ ウィンドウの表示を作成します。](images/packaging-screen3.jpg)

    ストアでのアプリで、プロジェクトが既に関連付けられている場合も、関連付けられたストア アプリのパッケージを作成するオプションがあります。 選択した場合**サイドロード用のパッケージを作成したい**、Visual Studio はパートナー センターのサブミッション用アプリ パッケージのアップロード (.msixupload または .appxupload) ファイルを生成できません。 アプリをサイドローディングして社内デバイスで実行するだけの場合やテスト目的の場合は、このオプションを選ぶことができます。 サイドローディングについて詳しくは、「[デバイスを開発用に有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)」をご覧ください。
4.  次のページでは、パートナー センターに、開発者アカウントでサインインします。 まだ開発者アカウントがない場合は、ウィザードで作成できます。
    ![アプリ名を選択した場合に表示のアプリ パッケージ ウィンドウを作成します。](images/packaging-screen4.jpg)
5.  アカウントに現在登録されているアプリの一覧から、パッケージのアプリ名を選択するか、新しい予約をまだ予約していないパートナー センターで 1 つ。  
6.  必ず、 **[パッケージの選択と構成]** ダイアログ ボックスで 3 つのアーキテクチャ構成 (x86、x64、ARM) をすべて選択し、できるだけ広範なデバイスにアプリを展開できるようにしてください。 **[アプリケーション バンドルの生成]** ボックスで **[常に行う]** を選びます。 アプリケーション バンドル (.appxbundle または .msixbundle) は、プロセッサ アーキテクチャの種類ごとに構成されているアプリ パッケージのコレクションを格納するため、1 つのアプリのパッケージ ファイルを優先します。 アプリ バンドルを生成することを選択するとは、デバッグとクラッシュ分析情報と共に最終的なアプリ パッケージのアップロード (.appxupload または .msixupload) ファイルで、アプリ バンドルに含まれています。 どのアーキテクチャを選べばよいかわからない場合や、各種デバイスにより使用されるアーキテクチャについて詳しく調べる場合は、「[アプリ パッケージのアーキテクチャ](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)」をご覧ください。  
    ![示されているパッケージの構成とアプリ パッケージ ウィンドウを作成します。](images/packaging-screen5.jpg)
7.  完全な PDB シンボル ファイルを含める[アプリのパフォーマンスを分析](https://docs.microsoft.com/windows/uwp/publish/analytics)アプリが公開された後に、パートナー センターから。 バージョンの番号付けやパッケージの出力場所など、他の詳細情報を構成します。
8.  **[作成]** をクリックして、アプリ パッケージを生成します。 いずれかを選択した場合、 **Microsoft Store にアップロードするパッケージを作成したい**パッケージのアップロード (.appxupload または .msixupload) ファイルを作成する、オプションの手順 3 とパートナー センターの送信用のパッケージを作成します。 選択した場合**サイドロード用のパッケージを作成したい**手順 3 で作成する 1 つのアプリのパッケージまたは手順 6. で選択内容に基づいて、アプリ バンドルのいずれか。
9. アプリが正常にパッケージ化するときは、このダイアログ ボックスが表示され、アプリ パッケージのアップロード ファイルを取得するには、指定した出力場所から。 この時点で実行できます[、ローカル コンピューターまたはリモート コンピューターでアプリ パッケージを検証](#validate-your-app-package)と[ストアへの送信を自動化](#automate-store-submissions)します。
    ![パッケージの作成完了ウィンドウに表示される検証オプション](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>アプリ パッケージのアップロード ファイルを手動で作成します。

1. 次のファイルをフォルダーに配置します。
    - 1 つまたは複数のアプリ パッケージ (.msix または .appx) または (.msixbundle または .appxbundle)、アプリ バンドルします。
    - .Appxsym ファイルの場合。 これは、圧縮された .pdb ファイルに使用される、アプリのパブリック シンボルを含む[クラッシュ分析](../publish/health-report.md)パートナー センターでします。 このファイルを省略できますが、場合は、クラッシュの分析やデバッグ情報はありません、アプリの使用可能です。
2. フォルダーを zip 圧縮します。
3. Zip 形式フォルダーの拡張機能の名前を .zip から .msixupload または .appxupload に変更します。

## <a name="validate-your-app-package"></a>アプリ パッケージを検証します。

ローカルまたはリモート コンピューター上の認定資格のパートナー センターに送信する前に、アプリを検証します。 アプリ パッケージのデバッグ ビルドではなくリリース ビルドのみを検証できます。 パートナー センターへのアプリの送信についての詳細については、次を参照してください。[アプリを送信する](https://docs.microsoft.com/windows/uwp/publish/app-submissions)します。

### <a name="validate-your-app-package-locally"></a>ローカルでアプリケーション パッケージを検証します。

1. 最終的な**パッケージの作成が完了**のページ、**アプリ パッケージの作成**ウィザードのままにしてください、**ローカル マシン**オプションを選択し、**起動Windows アプリ認定キット**します。 Windows アプリ認定キットでアプリをテストする方法について詳しくは、「[Windows アプリ認定キット](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)」をご覧ください。

    Windows アプリ認定キット (WACK) は、さまざまなテストを実行し、結果を返します。 より具体的な情報については、「[Windows アプリ認定キットのテスト](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)」をご覧ください。

    テストに使用するリモートの Windows 10 デバイスがあれば、そのデバイスで、Windows アプリ認定キットを手動でインストールする必要があります。 この手順については、次のセクションで説明します。 その後、 **[リモート コンピューター]** を選び、 **[Windows アプリ認定キットを起動する]** をクリックしてリモート デバイスに接続し、検証テストを実行します。

2. WACK が終了し、アプリが認定を渡されると、パートナー センターへのアプリ提出にする準備が整いました。 必ず正しいファイルをアップロードしてください。 ファイルの既定の場所は、ソリューションのルート フォルダーにあります`\[AppName]\AppPackages`末尾が .appxupload または .msixupload ファイル拡張子とします。 フォームの名前になります`[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload`または`[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload`すべて選択されているパッケージのアーキテクチャのアプリケーション バンドルを選択した場合。

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>リモート Windows 10 デバイスでアプリ パッケージを検証します。

1. 次で開発用の Windows 10 デバイスを有効にする、[開発用にデバイスを有効にする](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)指示します。
    >[!IMPORTANT]
    > Windows 10 のリモート ARM デバイスでアプリ パッケージを検証することはできません。
2. Visual Studio のリモート ツールをダウンロードしてインストールします。 これらのツールを使って Windows アプリ認定キットをリモートで実行します。 これらのツールについてダウンロード場所など詳しくは、[リモート コンピューターでの UWP アプリの実行](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015)に関するページをご覧ください。
3. 必要なダウンロード[Windows アプリ認定キット](https://go.microsoft.com/fwlink/p/?LinkID=309666)してから、リモートの Windows 10 デバイスにインストールします。
4. **パッケージの作成が完了しました**ウィザードのページで、 **[リモート コンピューター]** オプション ボタンを選び、 **[テスト接続]** ボタンの横にある省略記号ボタンをクリックします。
    >[!NOTE]
    > **リモート マシン**オプション ボタンが検証をサポートするソリューション構成を少なくとも 1 つを選択した場合にのみ使用できます。 WACK でアプリをテストする方法について詳しくは、「[Windows アプリ認定キット](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)」をご覧ください。
5. サブネットの内部にあるデバイスの形式を指定するか、サブネットの外部にあるデバイスのドメイン ネーム サーバー (DNS) 名または IP アドレスを指定します。
6. Windows 資格情報を使ってデバイスにログオンする必要がない場合は、 **[認証モード]** ボックスの一覧で **[なし]** を選びます。
7. **[選択]** 、 **[Windows アプリ認定キットを起動する]** の順に選びます。 デバイスでリモート ツールが実行されていれば、Visual Studio がデバイスに接続されてから、検証テストが実行されます。 「[Windows アプリ認定キットのテスト](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests)」をご覧ください。

## <a name="automate-store-submissions"></a>ストアへの送信を自動化します。

Visual Studio 2019 以降に送信する、生成された .appxupload ファイル Microsoft Store、IDE から直接選択して、**自動的に Windows アプリ認定キットの検証後に Microsoft Store に提出**オプションの最後に、[アプリ パッケージの作成ウィザード](#create-your-app-package-upload-file-using-visual-studio)します。 この機能は、アプリを発行するために必要なパートナー センター アカウントの情報にアクセスするための Azure Active Directory を活用します。 この機能を使用するには、必要があります、パートナー センター アカウントに Azure Active Directory を関連付けるし、送信に必要ないくつかの資格情報を取得します。

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Azure Active Directory をパートナー センター アカウントに関連付ける

ストアへの自動送信に必要な資格情報を取得するには、次の手順でを最初にする必要があります、[パートナー センター ダッシュ ボード](https://partner.microsoft.com/dashboard)を行っていない既に場合。

1. [パートナー センター アカウントを組織の Azure Active Directory に関連付ける](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center)します。 組織で Office 365 または Microsoft の他のビジネス サービスが既に使用されている場合は、既に Azure AD をお持ちです。 それ以外の場合、作成、新しい Azure AD テナントからパートナー センター内で無料になります。

2. [パートナー センター アカウントに Azure AD アプリケーションを追加](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account)します。 この Azure AD アプリケーションは、アプリや、デベロッパー センター アカウントの申請へのアクセスに使用するサービスを表します。 このアプリケーションを割り当てる必要があります、 **Manager**ロール。 このアプリケーションが既に Azure AD ディレクトリに存在する場合、 **[Azure AD アプリケーションの追加]** ページで選んでデベロッパー センター アカウントに追加できます。 それ以外の場合、 **[Azure AD アプリケーションの追加]** ページで新しい Azure AD アプリケーションを作成できます。

### <a name="retrieve-the-credentials-required-for-submissions"></a>送信に必要な資格情報を取得します。

次に、送信に必要なパートナー センターの資格情報を取得できます。 **Azure テナント ID**、**クライアント ID**と**クライアント キー**します。

1. 移動して、[パートナー センター ダッシュ ボード](https://partner.microsoft.com/dashboard)し、Azure AD 資格情報でサインインします。

2. パートナー センター ダッシュ ボードの右上隅のダッシュ ボード) の近くの歯車アイコンを選択し、選択**開発者設定が**します。

3. **設定** メニューの左側のウィンドウでクリックして**ユーザー**します。

4. アプリケーションの設定に進むには、Azure AD アプリケーションの名前をクリックします。 このページで、コピー、**テナント ID**と**クライアント ID**値。

5. **キー**セクションで、**新しいキーの追加**します。 次の画面では、コピー、**キー**値で、クライアント シークレットに対応します。 このページから移動すると、この情報を再度アクセスすることはできませんが失われないようにします。 詳しくは、「[Azure AD アプリケーションのキーを管理する方法](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application)」をご覧ください。

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Visual Studio でのストアへの自動送信を構成します。

前の手順を完了した後は、Visual Studio 2019 でストアへの自動送信を構成できます。

1. 最後に、[アプリ パッケージの作成ウィザード](#create-your-app-package-upload-file-using-visual-studio)を選択します**Windows アプリ認定キットの検証後に、自動的に Microsoft Store に送信**クリック**を再構成**.

2. **Microsoft Store の送信の構成の設定**ダイアログ ボックスで、Azure テナント ID、クライアント ID、およびクライアント キーを入力します。

    ![Microsoft Store の送信の設定を構成します。](images/packaging-screen8.jpg)

    > [!Important]
    > 資格情報は、将来の送信に使用するプロファイルに保存できます。

3. **[OK]** をクリックします。

WACK テストが完了した後、送信が開始されます。 送信の進行状況を追跡する、**検証および発行**ウィンドウ。

![確認し、発行の進行状況](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>アプリ パッケージをサイドローディングする

UWP アプリのパッケージではデスクトップ アプリを使用しているアプリがデバイスにインストールされていません。 通常、Microsoft Store から UWP アプリをダウンロードし、デバイスにアプリをインストールします。 アプリは Microsoft Store で公開せずにインストールできます (サイドローディング)。 アプリ パッケージを使用してアプリをテスト ファイルを作成して、インストールできます。 基幹業務 (LOB) アプリのように、ストアで販売しないアプリの場合は、そのアプリをサイドローディングして、社内の他のユーザーが使えるようにできます。

ターゲット デバイスでアプリをサイドロードすることができます、前にする必要があります[開発用にデバイスを有効にする](../get-started/enable-your-device-for-development.md)します。

サイドローディングを使用して、Windows 10 Mobile デバイスでアプリ、 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md)ツール。 デスクトップ、ラップトップ、およびタブレットでは、以下の手順に従います。

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>サイドロード アプリ パッケージの Windows 10 Anniversary Update 以降

Windows 10 Anniversary Update (Windows 10、バージョン 1607) で導入された、アプリ パッケージは、アプリのパッケージ ファイルをダブルクリックするだけでインストールできます。 を使用するには、、アプリのパッケージまたはアプリ バンドルのファイルに移動し、をダブルクリックします。 [アプリのインストーラー](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root)起動し、基本的なアプリの情報だけでなく、[インストール] ボタン、インストールの進行状況バー、および関連するエラー メッセージを提供します。

![Contoso というサンプル アプリをインストールするために、アプリのインストーラーが表示されます。](images/appinstaller-screen.png)

> [!NOTE]
> アプリのインストーラーでは、アプリがデバイスによって信頼されている前提としています。 開発者アプリまたはエンタープライズ アプリをサイドローディングする場合、デバイス上の信頼されたユーザー ストアまたは信頼された発行元証明機関ストアに署名証明書をインストールする必要があります。 この方法がわからない場合は、[テスト証明書のインストール](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)に関するページをご覧ください。

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Windows の以前のバージョンのパッケージ化、アプリのサイドロード

1.  インストールするアプリ バージョンのフォルダーをターゲット デバイスにコピーします。

    アプリ バンドルを作成した場合は、バージョン番号に基づいたフォルダーと `*_Test` フォルダーが必要です。 たとえば、2 つのフォルダーは次のようになります (インストールするバージョンは 1.0.2.0)。

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    アプリ バンドルがない場合は、適切な構造になっているフォルダーと対応する `*_Test` フォルダーをコピーします。 次の 2 つのフォルダーは、x64 アーキテクチャのアプリ パッケージとその `*_Test` フォルダーの例です。

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  ターゲット デバイスで `*_Test` フォルダーを開きます。
3.  **Add-AppDevPackage.ps1** ファイルを右クリックします。 **[PowerShell で実行]** を選んで、画面の指示に従います。  
    ![ファイル エクスプ ローラーが示すように PowerShell スクリプトに移動](images/packaging-screen7.jpg)

    アプリ パッケージがインストールされると、PowerShell ウィンドウには、このメッセージが表示されます。**アプリが正常にインストールします。**
    >[!TIP]
    > タブレットでショートカット メニューを開くには、右クリックし、完全な円が表示されるまで、指を放したし画面にタッチします。 指を放した後にショートカット メニューが表示されます。

4.  スタート ボタンをクリックし、アプリをアプリ名で検索してから起動します。
