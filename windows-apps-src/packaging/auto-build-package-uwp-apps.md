---
title: UWP アプリの自動ビルドを設定する
description: サイドロード パッケージやストア パッケージを生成する自動ビルドを構成する方法について説明します。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: cb21573dac0c4cc4fc2d6aa2e2345c56631fde87
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372760"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>UWP アプリの自動ビルドを設定する

Azure のパイプラインを使用すると、UWP プロジェクトに対して自動ビルドを作成します。 この記事では、これを行うさまざまな方法を紹介します。 紹介しますが、他のビルド システムと統合できるように、コマンドラインを使用して、これらのタスクを実行する方法。

## <a name="create-a-new-azure-pipeline"></a>新しい Azure パイプラインを作成します。

まず[パイプラインを Azure にサインアップ](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up)をまだ行っていない場合。

次に、ソース コードをビルドするのに使用できるパイプラインを作成します。 GitHub リポジトリを構築するためのパイプラインの構築に関するチュートリアルについては、次を参照してください。[最初のパイプラインを作成する](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml)します。 Azure のパイプラインが表示されているリポジトリの種類をサポートしています[今回](https://docs.microsoft.com/azure/devops/pipelines/repos)します。

## <a name="set-up-an-automated-build"></a>自動ビルドを設定する

まず、既定値は、UWP は、Azure の開発運用で使用可能な定義を作成し、パイプラインを構成する方法を説明します。

ビルド定義テンプレートの一覧で、 **[ユニバーサル Windows プラットフォーム]** テンプレートを選択します。

![UWP テンプレートを選択します。](images/select-yaml-template.png)

このテンプレートには、UWP プロジェクトをビルドする基本的な構成が含まれています。

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

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

既定のテンプレートは、.csproj ファイルで指定された証明書でパッケージに署名しようとします。 ビルド中に、パッケージに署名する場合は、秘密キーへのアクセスが必要です。 署名パラメーターを追加して無効にした場合は、`/p:AppxPackageSigningEnabled=false`を`msbuildArgs`YAML ファイルのセクション。

## <a name="add-your-project-certificate-to-a-repository"></a>リポジトリに、プロジェクトの証明書を追加します。

パイプラインは、Azure リポジトリの Git と TFVC の両方のリポジトリで動作します。 Git リポジトリを使用する場合は、ビルド エージェントがアプリ パッケージに署名できるように、リポジトリにプロジェクトの証明書ファイルを追加します。 これを行わない場合、Git リポジトリは証明書ファイルを無視します。 証明書ファイルを右クリックし、リポジトリには、証明書ファイルを追加するに**ソリューション エクスプ ローラー**、ショートカット メニューの 、**無視ファイルをソース管理に追加**コマンド。

![証明書を含める方法](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>ソリューションのビルドのビルド タスクを構成する

このタスクは、バイナリを作業フォルダーにあり、出力アプリのパッケージ ファイルを生成するソリューションをコンパイルします。
このタスクは、MSBuild 引数を使用します。 これらの引数の値を指定する必要があります。 次の表をガイドとして使用してください。

|**MSBuild 引数**|**値**|**説明**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | 生成された成果物を格納するフォルダーを定義します。 |
| AppxBundlePlatforms | $(Build.BuildPlatform) | バンドルに含めるプラットフォームを定義できます。 |
| AppxBundle | 常に  | 指定されたプラットフォームの.msix/.appx ファイルで、.msixbundle/.appxbundle を作成します。 |
| UapAppxPackageBuildMode | StoreUpload | .Msixupload/.appxupload ファイルを生成し、**テスト (_t)** サイドローディング用のフォルダー。 |
| UapAppxPackageBuildMode | CI | .Msixupload/.appxupload ファイルのみを生成します。 |
| UapAppxPackageBuildMode | SideloadOnly | 生成、**テスト (_t)** のみサイドローディング用のフォルダー |

またはその他のビルド システムを使用して、コマンドラインを使用して、ソリューションをビルドする場合は、これらの引数で MSBuild を実行します。

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

定義されているパラメーター、`$()`構文は、ビルド定義で定義された変数とその他の変更がシステムを構築します。

![既定の変数](images/building-screen5.png)

定義済みのすべての変数を表示するを参照してください。[ビルド変数の定義済み](https://docs.microsoft.com/azure/devops/pipelines/build/variables)します。

## <a name="configure-the-publish-build-artifacts-task"></a>ビルド成果物の発行タスクを構成します。

既定の UWP パイプラインは、生成された成果物を保存できません。 YAML 定義には、発行機能を追加するには、次のタスクを追加します。

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

生成された成果物を表示できます、**成果物**オプション、ビルドの結果 ページ。

![成果物](images/building-screen6.png)

ご用意しましたので、`UapAppxPackageBuildMode`引数`StoreUpload`、成果物フォルダーには (.msixupload/.appxupload) ストアに送信するパッケージが含まれています。 送信する通常のアプリ パッケージ (.msix/.appx) またはアプリ バンドル (.msixbundle/.appxbundle/) ストアに注意してください。 この資料の目的上、.appxupload ファイルを使います。

## <a name="address-bundle-errors"></a>アドレスのバンドルのエラー

1 つ以上の UWP プロジェクトをソリューションに追加してバンドルを作成しようとしは、次のようなエラーが発生します。

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

このエラーが表示されるのは、ソリューション レベルで、バンドルに含めるアプリが明確ではないためです。 この問題を解決するには、各プロジェクト ファイルを開くし、次のプロパティを 1 つ目の末尾に追加`<PropertyGroup>`要素。

|**プロジェクト**|**[プロパティ]**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

次に、削除、`AppxBundle`ビルド ステップの MSBuild 引数。

## <a name="related-topics"></a>関連トピック

- [Windows の .NET アプリを構築します。](https://www.visualstudio.com/docs/build/get-started/dot-net)
- [UWP アプリのパッケージ化](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Windows 10 で LOB アプリのサイドロード](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
- [パッケージに署名するための証明書を作成します。](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
