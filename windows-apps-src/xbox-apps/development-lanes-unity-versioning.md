---
title: Unity - UWP プロジェクトのバージョン管理
description: ユニバーサル Windows プラットフォーム (UWP) を使用して、Xbox 用 Unity ゲームでバージョン管理を使用する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 3e4d98892b9bd738eca788d166ef79f81ea1b047
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173746"
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

## <a name="folders"></a>Folders  

`Assets` | ***Include*** |Microsoft Store イメージを含む  
`Data`   | ***無視*** |Unity はプロジェクトを (シーン、シェーダー、スクリプト、Prefabs など) にコンパイルします。  
`Dependencies` | ***Include*** |このフォルダーは、すべての UWP 依存関係を保持するために作成したものです (たとえば、XboxLiveSDK.dll)。  
`Properties` | ***Include*** |開発者が変更できる詳細設定が含まれています  
`Unprocessed` | ***無視*** |Unity `.dll` と `.pdb` ファイルが含まれています  

## <a name="files"></a>ファイル  

`App.cs` | ***Include*** |UWP アプリケーションのエントリポイントです。これは、他のソースファイルで変更および拡張できます。  
`Package.appxmanifest` | ***Include*** |. Msix または .appx パッケージのアプリケーションパッケージマニフェストソースファイル  
`project.json` | ***Include*** |が依存する NuGet パッケージについて説明します。 `*.csproj`  
`ScrapyardPhoenix.csproj` | ***Include*** |UWP ビルドターゲットについて説明します。UWP プロジェクトに依存関係を追加すると、この `*.csproj` ファイルにはその情報が含まれます。  
`ScrapyardPhoenix.csproj.user` | ***無視*** |このファイルにはローカルユーザー情報が含まれています

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

## <a name="see-also"></a>関連項目
- [既存のゲームの Xbox への移行](development-lanes-landing.md)
- [Xbox One の UWP](index.md)
