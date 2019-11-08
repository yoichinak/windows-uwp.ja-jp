---
title: UWP アプリの自動ビルドを設定する
description: サイドロード パッケージやストア パッケージを生成する自動ビルドを構成する方法について説明します。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: b7d38464a26af0df03c1aa381b16fbddf1de55cc
ms.sourcegitcommit: e0644abf76a2535ea24758d1904ff00dfcd86a51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72008043"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP アプリの自動ビルドを設定する

Azure Pipelines を使用して、UWP プロジェクトの自動ビルドを作成できます。 この記事では、さまざまな方法を紹介します。 また、コマンドラインを使用してこれらのタスクを実行する方法についても説明します。これにより、他のビルドシステムと統合できます。

## <a name="create-a-new-azure-pipeline"></a>新しい Azure パイプラインを作成する

まだサインアップしていない場合は、 [Azure Pipelines に](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)サインアップすることから始めます。

次に、ソースコードのビルドに使用できるパイプラインを作成します。 GitHub リポジトリを構築するためのパイプラインの構築に関するチュートリアルについては、[最初のパイプラインの作成](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)に関するチュートリアルを参照してください。 Azure Pipelines は、[この記事に](https://docs.microsoft.com/azure/devops/pipelines/repos)記載されているリポジトリの種類をサポートしています。

## <a name="set-up-an-automated-build"></a>自動ビルドを設定する

まず、Azure Dev Ops で利用できる既定の UWP ビルド定義から始めて、パイプラインの構成方法を説明します。

ビルド定義テンプレートの一覧で、 **[ユニバーサル Windows プラットフォーム]** テンプレートを選択します。

![UWP テンプレートを選択します](images/select-yaml-template.png)

このテンプレートには、UWP プロジェクトをビルドするための基本的な構成が含まれています。

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

既定のテンプレートは、.csproj ファイルに指定されている証明書を使用してパッケージに署名しようとします。 ビルド中にパッケージに署名する場合は、秘密キーへのアクセス権を持っている必要があります。 それ以外の場合は、パラメーター `/p:AppxPackageSigningEnabled=false` を YAML ファイルの `msbuildArgs` セクションに追加することにより、署名を無効にすることができます。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>セキュリティで保護されたファイルライブラリにプロジェクト証明書を追加する

可能な限り、リポジトリに証明書を送信することは避けてください。既定では、git はそれらを無視します。 証明書などの機密性の高いファイルの安全な処理を管理するために、Azure DevOps は[secure files](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops)機能をサポートしています。

自動ビルドの証明書をアップロードするには、次のようにします。

1. Azure Pipelines で、ナビゲーションウィンドウの **[パイプライン]** を展開し、 **[ライブラリ]** をクリックします。
2. **[セキュリティで保護さ]** れたファイル タブをクリックし、 **[+ 保護されたファイル]** をクリックします。

    ![セキュリティで保護されたファイルをアップロードする方法](images/secure-file1.png)

3. 証明書ファイルを参照し、[ **OK]** をクリックします。
4. 証明書をアップロードしたら、証明書を選択してプロパティを表示します。 **[パイプラインのアクセス許可]** で、 **[すべてのパイプラインで使用することを承認]** する の切り替えを有効にします。

    ![セキュリティで保護されたファイルをアップロードする方法](images/secure-file2.png)

5. 証明書の秘密キーにパスワードが含まれている場合は、パスワードを[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates)に保存し、パスワードを[変数グループ](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)にリンクすることをお勧めします。 変数を使用すると、パイプラインからパスワードにアクセスできます。 パスワードは秘密キーに対してのみサポートされていることに注意してください。パスワードで保護されている証明書ファイルを使用することは、現在サポートされていません。

> [!NOTE]
> Visual Studio 2019 以降では、UWP プロジェクトで一時的な証明書が生成されなくなりました。 証明書を作成またはエクスポートするには、[この記事](/windows/msix/package/create-certificate-package-signing)で説明されている PowerShell コマンドレットを使用します。

## <a name="configure-the-build-solution-build-task"></a>ソリューションのビルドのビルド タスクを構成する

このタスクは、作業フォルダー内のすべてのソリューションをバイナリにコンパイルし、出力アプリパッケージファイルを生成します。 このタスクでは、MSBuild 引数を使用します。 これらの引数の値を指定する必要があります。 次の表をガイドとして使用してください。

|**MSBuild 引数**|**[値]**|**[説明]**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 生成された成果物を格納するフォルダーを定義します。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | バンドルに含めるプラットフォームを定義できます。 |
| AppxBundle | 常に | 指定されたプラットフォームの .msixbundle/.appxbundle ファイルを持つ... を作成します。 |
| UapAppxPackageBuildMode | StoreUpload | Msixupload/.appxupload ファイルと、サイドローディング用の**テスト**フォルダーを生成します。 |
| UapAppxPackageBuildMode | CI | では、msixupload/. .appxupload ファイルのみが生成されます。 |
| UapAppxPackageBuildMode | SideloadOnly | サイドローディング専用のテストフォルダーを生成します **(_d)** 。 |
| AppxPackageSigningEnabled | true | パッケージの署名を有効にします。 |
| PackageCertificateThumbprint | 証明書の拇印 | この値は、署名証明書の拇印と一致しているか、空の文字列で**ある必要があり**ます。 |
| PackageCertificateKeyFile | パス | 使用する証明書へのパス。 これは、セキュリティで保護されたファイルのメタデータから取得されます。 |
| PackageCertificatePassword | Password | 証明書の秘密キーのパスワード。 パスワードを[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates)に保存し、パスワードを[変数グループ](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)にリンクすることをお勧めします。 この引数に変数を渡すことができます。 |

### <a name="configure-the-build"></a>ビルドを構成する

コマンドラインまたは他のビルドシステムを使用してソリューションをビルドする場合は、これらの引数を指定して MSBuild を実行します。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>パッケージ署名の構成

MSIX (または APPX) パッケージに署名するには、パイプラインで署名証明書を取得する必要があります。 これを行うには、VSBuild タスクの前に DownloadSecureFile タスクを追加します。
これにより、```signingCert``` を使用して署名証明書にアクセスできるようになります。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

次に、VSBuild タスクを更新して、署名証明書を参照します。

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> PackageCertificateThumbprint 引数は、意図的に空の文字列に設定されます。 拇印がプロジェクトで設定されていても署名証明書と一致しない場合、ビルドは失敗し、`Certificate does not match supplied signing thumbprint` というエラーが表示されます。

### <a name="review-parameters"></a>パラメーターの確認

@No__t-0 構文で定義されたパラメーターは、ビルド定義で定義された変数であり、他のビルドシステムでは変更されます。

![既定の変数](images/building-screen5.png)

定義済みの変数をすべて表示するには、「[定義済みのビルド変数](https://docs.microsoft.com/azure/devops/pipelines/build/variables)」を参照してください。

## <a name="configure-the-publish-build-artifacts-task"></a>ビルド成果物の発行タスクを構成する

既定の UWP パイプラインでは、生成された成果物は保存されません。 発行機能を YAML 定義に追加するには、次のタスクを追加します。

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

生成された成果物は、ビルド結果 ページの **成果物** オプションで確認できます。

![成果物](images/building-screen6.png)

@No__t-0 引数を `StoreUpload` に設定したため、アーティファクトフォルダーには、ストアに送信するためのパッケージ (. msixupload/. .appxupload) が含まれています。 ストアには、通常のアプリケーションパッケージ (msix/.appx) またはアプリバンドル (. .msixbundle/.appxbundle/) を送信することもできます。 この資料の目的上、.appxupload ファイルを使います。

## <a name="address-bundle-errors"></a>アドレスバンドルエラー

複数の UWP プロジェクトをソリューションに追加した後でバンドルを作成しようとすると、次のようなエラーが表示されることがあります。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

このエラーが表示されるのは、ソリューション レベルで、バンドルに含めるアプリが明確ではないためです。 この問題を解決するには、各プロジェクトファイルを開き、最初の `<PropertyGroup>` 要素の末尾に次のプロパティを追加します。

|**作品**|**Properties**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

次に、ビルドステップから @no__t 0 の MSBuild 引数を削除します。

## <a name="related-topics"></a>関連トピック

- [Windows 用 .NET アプリをビルドする](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)
- [Windows 10 のサイドロード LOB アプリ](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [パッケージ署名用の証明書を作成する](/windows/msix/package/create-certificate-package-signing)
