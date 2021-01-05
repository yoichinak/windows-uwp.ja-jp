---
title: UWP アプリの自動ビルドを設定する
description: サイドロード パッケージやストア パッケージを生成する自動ビルドを構成する方法について説明します。
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: eec20ffc02263383cdbe670771143a6403177e7b
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860432"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP アプリの自動ビルドを設定する

Azure Pipelines を使って、UWP プロジェクトの自動ビルドを作成することができます。 この記事では、これを行うためのさまざまな方法を見ていきます。 他の任意のビルド システムと統合できるように、コマンド ラインを使用してこれらのタスクを実行する方法も示します。

## <a name="create-a-new-azure-pipeline"></a>新しい Azure パイプラインを作成する

まだの場合、まず、[Azure Pipelines にサインアップします](/azure/devops/pipelines/get-started/pipelines-sign-up)。

次に、ソース コードのビルドに使用できるパイプラインを作成します。 パイプラインをビルドして GitHub リポジトリをビルドするチュートリアルについては、「[最初のパイプラインを作成](/azure/devops/pipelines/get-started-yaml)」を参照してください。 Azure Pipelines では、[こちらの記事](/azure/devops/pipelines/repos)に一覧表示されているリポジトリの種類がサポートされています。

## <a name="set-up-an-automated-build"></a>自動ビルドを設定する

まず、Azure Dev Ops で利用できる既定の UWP ビルドの定義から始めて、パイプラインの構成方法を示します。

ビルド定義テンプレートの一覧で、 **[ユニバーサル Windows プラットフォーム]** テンプレートを選択します。

![UWP テンプレートを選択する](images/select-yaml-template.png)

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
- task: NuGetToolInstaller@1

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

既定のテンプレートでは、.csproj ファイルに指定されている証明書でのパッケージへの署名が試行されます。 ビルド中にパッケージに署名する場合は、秘密キーにアクセスできる必要があります。 それ以外の場合は、YAML ファイルの `msbuildArgs` セクションにパラメーター `/p:AppxPackageSigningEnabled=false` を追加することにより、署名を無効にできます。

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>セキュア ファイル ライブラリにプロジェクト証明書を追加する

可能な限り、リポジトリに証明書を送信することは避けてください。git では既定でそれらが無視されます。 証明書などの機密性の高いファイルの安全な処理を管理するために、Azure DevOps では、[セキュア ファイル](/azure/devops/pipelines/library/secure-files?view=azure-devops)機能がサポートされています。

自動ビルドの証明書をアップロードするには、次の手順に従います。

1. Azure Pipelines のナビゲーション ウィンドウで **[パイプライン]** を展開し、 **[ライブラリ]** をクリックします。
2. **[セキュア ファイル]** タブをクリックし、 **[+ セキュア ファイル]** をクリックします。

    ![Azure で [ライブラリ] オプションが強調表示され、[セキュア ファイル] ページが表示されているスクリーンショット。](images/secure-file1.png)

3. 証明書ファイルを参照し、 **[OK]** をクリックします。
4. 証明書をアップロードしたら、それを選択してそのプロパティを表示します。 **[パイプラインのアクセス許可]** で、 **[すべてのパイプラインで使用するために承認します。]** を有効に切り替えます。

    ![[パイプラインのアクセス許可] セクションで [すべてのパイプラインで使用するために承認します] オプションが選択されているスクリーンショット。](images/secure-file2.png)

5. 証明書の秘密キーにパスワードが含まれている場合は、パスワードを [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) に保存し、パスワードを[変数グループ](/azure/devops/pipelines/library/variable-groups)にリンクすることをお勧めします。 その変数を使用して、パイプラインからパスワードにアクセスできます。 パスワードは秘密キーに対してのみサポートされていることに注意してください。パスワードで保護されている証明書ファイルを使用することは、現在サポートされていません。

> [!NOTE]
> Visual Studio 2019 以降では、UWP プロジェクトで一時的な証明書が生成されなくなりました。 証明書を作成またはエクスポートするには、[こちらの記事](/windows/msix/package/create-certificate-package-signing)に記載されている PowerShell コマンドレットを使用します。

## <a name="configure-the-build-solution-build-task"></a>ソリューションのビルドのビルド タスクを構成する

このタスクでは、作業フォルダーにあるすべてのソリューションをバイナリにコンパイルし、出力アプリ パッケージ ファイルを生成します。 このタスクでは、MSBuild 引数を使用します。 これらの引数の値を指定する必要があります。 次の表をガイドとして使用してください。

|**MSBuild 引数**|**値**|**説明**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 生成された成果物を格納するフォルダーを定義します。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | バンドルに含めるプラットフォームを定義できます。 |
| AppxBundle | 常に表示する | 指定されているプラットフォームの .msix/.appx ファイルを含む .msixbundle/.appxbundle が作成されます。 |
| UapAppxPackageBuildMode | StoreUpload | サイドローディング用の .msixupload/.appxupload ファイルと **_Test** フォルダーが生成されます。 |
| UapAppxPackageBuildMode | CI | .msixupload/.appxupload ファイルのみが生成されます。 |
| UapAppxPackageBuildMode | SideloadOnly | サイドローディング用の **_Test** フォルダーのみが生成されます。 |
| AppxPackageSigningEnabled | true | パッケージの署名を有効にします。 |
| PackageCertificateThumbprint | 証明書の拇印 | この値は、署名証明書の拇印と一致しているか、空の文字列である **必要があります**。 |
| PackageCertificateKeyFile | パス | 使用する証明書へのパス。 これは、セキュア ファイルのメタデータから取得されます。 |
| PackageCertificatePassword | Password | 証明書の秘密キーのパスワード。 パスワードを [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) に保存し、パスワードを[変数グループ](/azure/devops/pipelines/library/variable-groups)にリンクすることをお勧めします。 この引数に変数を渡すことができます。 |

### <a name="configure-the-build"></a>ビルドを構成する

コマンド ラインを使って、つまり、他のビルド システムを使って、ソリューションをビルドする場合は、これらの引数を指定して MSBuild を実行します。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>パッケージ署名の構成

MSIX (または .appx) パッケージに署名するには、パイプラインで署名証明書を取得する必要があります。 これを行うには、VSBuild タスクの前に DownloadSecureFile タスクを追加します。
これにより、```signingCert``` 経由で署名証明書にアクセスできるようになります。

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

次に、署名証明書を参照するように VSBuild タスクを更新します。

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
> PackageCertificateThumbprint 引数は、予防措置として、意図的に空の文字列に設定されます。 拇印がプロジェクトで設定されていても、署名証明書と一致しない場合、ビルドは失敗し、次のエラーが表示されます。`Certificate does not match supplied signing thumbprint`

### <a name="review-parameters"></a>パラメーターの確認

`$()` 構文で定義されたパラメーターは、ビルド定義で定義される変数で、他のビルド システムでは変更されます。

![既定の変数](images/building-screen5.png)

すべての定義済みの変数を表示するには、「[定義済みのビルド変数](/azure/devops/pipelines/build/variables)」をご覧ください。

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

生成された成果物は、ビルド結果ページの **[成果物]** オプションで確認できます。

![成果物](images/building-screen6.png)

ここでは、`UapAppxPackageBuildMode` 引数を `StoreUpload` に設定しているため、成果物フォルダーには、ストアに提出するためのパッケージ (.msixupload と .appxupload) が含まれます。 通常のアプリ パッケージ (.msix と .appx) やアプリ バンドル (.msixbundle と .appxbundle) もストアに提出できることに注意してください。 この資料の目的上、.appxupload ファイルを使います。

## <a name="address-bundle-errors"></a>バンドル エラーに対処する

ソリューションに複数の UWP プロジェクトを追加してから、バンドルを作成しようとすると、このようなエラーが表示される場合があります。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

このエラーが表示されるのは、ソリューション レベルで、バンドルに含めるアプリが明確ではないためです。 この問題を解決するには、各プロジェクト ファイルを開き、最初の `<PropertyGroup>` 要素の末尾に以下のプロパティを追加します。

|**プロジェクト**|**[プロパティ]**|
|-------|----------|
|アプリ|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

その後、ビルド ステップから MSBuild の `AppxBundle` 引数を削除します。

## <a name="related-topics"></a>関連トピック

- [Windows 用の .NET アプリを構築する](/vsts/build-release/get-started/dot-net)
- [UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)
- [Windows 10 での LOB アプリのサイドローディング](/windows/deploy/sideload-apps-in-windows-10)
- [パッケージ署名用証明書を作成する](/windows/msix/package/create-certificate-package-signing)
