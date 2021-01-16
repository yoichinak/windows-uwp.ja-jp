---
title: Unity - UWP プロジェクトのバージョン管理
description: ユニバーサル Windows プラットフォーム (UWP) を使用して、Xbox 用 Unity ゲームでバージョン管理を使用する方法について説明します。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 67c4ec927d83ecba1257eb73fec451e3281333b2
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254167"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: UWP プロジェクトのバージョン管理

ユニバーサル Windows プラットフォーム (UWP) を使って Xbox 用の Unity ゲームをまだ構築していない場合には、  まず「[Xbox の UWP への Unity ゲームの移行](development-lanes-unity.md)」をご覧ください。

生成された UWP ディレクトリの一部をバージョン管理に追加することにはいくつかの理由があり、依存関係 (たとえば、Xbox Live SDK) の追加もその 1 つです。  このチュートリアルでは、このシナリオを例として使用しますが、プロジェクトの個別のニーズを解決する際の参考になれば幸いです。

***免責事項: バージョン管理ソリューションとして Git を使用します。 これらが異なる場合は、概念を翻訳する必要があります。** _

メモリを最新の状態に更新するために、次のようなゲームのディレクトリが _*_ScrapyardPhoenix_*_ ています。

![ビルドの保存先フォルダー](images/build-destination.png)

また、UWP ディレクトリは次のようになっています。

![UWP VS ソリューション](images/uwp-vs-solution.png)

このディレクトリでは、1つのフォルダー、 _*_ScrapyardPhoenix_*_ (ここにゲームの名前を挿入) フォルダーについてのみ説明しています。  他の要素はバージョン管理ではすべて無視できます。

_*_ファイルの種類について理解していない場合は、 「 [.Gitignore](https://git-scm.com/docs/gitignore)を参照してください。_*_

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep... (this line will be modified and removed further down)
!/UWP/ScrapyardPhoenix/
```

ここでは、**UWP/ScrapyardPhoenix** フォルダー内のいくつかのファイルやフォルダーを選択してバージョン管理に追加します。  最初に、フォルダーの内容全体を詳しく見てみましょう。

![UWP ビルド ディレクトリ](images/uwp-build-directory.png)  

## <a name="folders"></a>フォルダー  

| [フォルダー名] | 設定 | 説明 |
|-------------|---------|-------------|
| `Assets` | **_インクルード_* _ | Microsoft Store イメージを含む |
| `Data` | _*_Ignore_*_ | Unity はプロジェクトを (シーン、シェーダー、スクリプト、Prefabs など) にコンパイルします。 |
| `Dependencies` | _*_用意_*_ | このフォルダーは、すべての UWP 依存関係を保持するために作成したものです (たとえば、XboxLiveSDK.dll)。 |
| `Properties` | _*_用意_*_ | 開発者が変更できる詳細設定が含まれています |
| `Unprocessed` | _*_Ignore_*_ | Unity `.dll` と `.pdb` ファイルが含まれています |

## <a name="files"></a>ファイル  

| [フォルダー名] | 設定 | 説明 |
|-------------|---------|-------------|
| `App.cs` | _*_包含_*_ | UWP アプリケーションのエントリポイントです。これは、他のソースファイルで変更および拡張できます。 |
| `Package.appxmanifest` | _*_用意_*_ | . Msix または .appx パッケージのアプリケーションパッケージマニフェストソースファイル |
| `project.json` | _*_用意_*_ | が依存する NuGet パッケージについて説明します。 `_.csproj` |
| `ScrapyardPhoenix.csproj` | ***インクルード** _ | UWP ビルドターゲットについて説明します。UWP プロジェクトに依存関係を追加すると、この `_.csproj` ファイルにはその情報が含まれます。 |
| `ScrapyardPhoenix.csproj.user` | ***無視** _ | このファイルにはローカルユーザー情報が含まれています |

## <a name="resulting-gitignore"></a>結果として得られる .gitignore

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep...
!/UWP/ScrapyardPhoenix/Assets/*
!/UWP/ScrapyardPhoenix/Dependencies/*
!/UWP/ScrapyardPhoenix/Properties/*
!/UWP/ScrapyardPhoenix/App.cs
!/UWP/ScrapyardPhoenix/Package.appxmanifest
!/UWP/ScrapyardPhoenix/project.json
!/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj
```

これで、開発チームのメンバーは生成された最新の UWP プロジェクトを利用できるようになります。 新しいアセット、ソース、依存関係を、UWP プロジェクトに自由に追加できます。

UWP フォルダーのバージョン管理については、[これらの例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)もご覧ください。

## <a name="adding-dependencies-to-your-uwp-app"></a>UWP アプリへの依存関係の追加

依存関係を **Plugins** フォルダーの下の **Unity Assets** フォルダーに置くことにより、DLL と WINMD に依存関係を追加します。次に Inspector でそれを選択し、ターゲット プラットフォーム設定を適切に設定します。

![UWP ソリューション](images/uwp-solution.PNG)

**_ScrapyardPhoenix (ユニバーサル Windows)_** は、XBOX Live SDK などの参照を追加するプロジェクトです。

## <a name="see-also"></a>参照

- [既存のゲームの Xbox への移行](development-lanes-landing.md)
- [Xbox One の UWP](index.md)
