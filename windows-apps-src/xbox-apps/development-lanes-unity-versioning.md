---
title: Unity - UWP プロジェクトのバージョン管理
description: UWP プロジェクトをバージョン管理します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: b98fba394fb326d60451f07938504e99a92d764d
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089488"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: UWP プロジェクトのバージョン管理

ユニバーサル Windows プラットフォーム (UWP) を使って Xbox 用の Unity ゲームをまだ構築していない場合には、  まず「[Xbox の UWP への Unity ゲームの移行](development-lanes-unity.md)」をご覧ください。

生成された UWP ディレクトリの一部をバージョン管理に追加することにはいくつかの理由があり、依存関係 (たとえば、Xbox Live SDK) の追加もその 1 つです。  このチュートリアルでは、このシナリオを例として使用しますが、プロジェクトの個別のニーズを解決する際の参考になれば幸いです。

***免責事項: バージョン管理ソリューションとして Git を使用します。 これらが異なる場合は、概念を翻訳する必要があります。***

サンプルのゲーム ***ScrapyardPhoenix*** のディレクトリが現在どのようになっているかを、確認のため以下に示します。

![ビルドの保存先フォルダー](images/build-destination.png)

また、UWP ディレクトリは次のようになっています。

![UWP VS ソリューション](images/uwp-vs-solution.png)

このディレクトリで、注目する必要があるのは ***ScrapyardPhoenix*** (お客様のゲームの名前をここに挿入) フォルダーのみです。  他の要素はバージョン管理ではすべて無視できます。

***ファイルの種類について理解していない場合は、 「 [.Gitignore](https://git-scm.com/docs/gitignore)を参照してください。***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

ここでは、**UWP/ScrapyardPhoenix** フォルダー内のいくつかのファイルやフォルダーを選択してバージョン管理に追加します。  最初に、フォルダーの内容全体を詳しく見てみましょう。

![UWP ビルド ディレクトリ](images/uwp-build-directory.png)  

## <a name="folders"></a>フォルダ  

`Assets` | を***含める***|Microsoft Store イメージを含む  
`Data`   | ***無視***|Unity はプロジェクトを (シーン、シェーダー、スクリプト、Prefabs など) にコンパイルします。  
`Dependencies` | を***含める***|このフォルダーは、すべての UWP 依存関係を保持するために作成したものです (たとえば、XboxLiveSDK)。  
`Properties` | を***含める***|開発者が変更できる詳細設定が含まれています  
`Unprocessed` | ***無視***|Unity `.dll` と `.pdb` ファイルが含まれています  

## <a name="files"></a>ファイル  

`App.cs` | を***含める***|UWP アプリケーションのエントリポイントです。これは、他のソースファイルで変更および拡張できます。  
`Package.appxmanifest` | を***含める***|. Msix または .appx パッケージのアプリケーションパッケージマニフェストソースファイル  
`project.json` | を***含める***|`*.csproj` が依存する NuGet パッケージについて説明します  
`ScrapyardPhoenix.csproj` | を***含める***|UWP ビルドターゲットについて説明します。UWP プロジェクトに依存関係を追加すると、この `*.csproj` ファイルにその情報が含まれます。  
`ScrapyardPhoenix.csproj.user` | ***無視***|このファイルにはローカルユーザー情報が含まれています

## <a name="resulting-gitignore"></a>結果として得られる .gitignore

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

これで、開発チームのメンバーは生成された最新の UWP プロジェクトを利用できるようになります。 新しいアセット、ソース、依存関係を、UWP プロジェクトに自由に追加できます。

UWP フォルダーのバージョン管理については、[これらの例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)もご覧ください。

## <a name="adding-dependencies-to-your-uwp-app"></a>UWP アプリへの依存関係の追加

依存関係を **Plugins** フォルダーの下の **Unity Assets** フォルダーに置くことにより、DLL と WINMD に依存関係を追加します。次に Inspector でそれを選択し、ターゲット プラットフォーム設定を適切に設定します。

![UWP ソリューション](images/uwp-solution.PNG)

***ScrapyardPhoenix (ユニバーサル Windows)*** が、Xbox Live SDK などへの参照を追加する対象のプロジェクトです。

## <a name="see-also"></a>参照
- [既存のゲームの Xbox への移行](development-lanes-landing.md)
- [Xbox One の UWP](index.md)
