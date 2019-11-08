---
title: マネージ Windows ランタイムコンポーネントの配布
description: ファイルのコピーによって Windows ランタイムコンポーネントを配布できます。
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690386"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>マネージ Windows ランタイムコンポーネントの配布

ファイルのコピーによって Windows ランタイムコンポーネントを配布できます。 ただし、コンポーネントが多数のファイルで構成されている場合は、ユーザーにとってインストールが煩雑になることがあります。 また、ファイルの配置や参照の設定に失敗すると、エラーが発生することがあります。 複雑なコンポーネントを Visual Studio 拡張機能 SDK としてパッケージ化して、簡単にインストールして使用できるようにすることができます。 ユーザーは、パッケージ全体に対して1つの参照のみを設定する必要があります。 「 [Visual Studio 拡張機能の検索と使用](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)」で説明されているように、 **[拡張機能と更新プログラム]** ダイアログボックスを使用して、コンポーネントを簡単に見つけてインストールできます。

## <a name="planning-a-distributable-windows-runtime-component"></a>配布可能な Windows ランタイムコンポーネントの計画

Winmd ファイルなど、バイナリファイルの一意な名前を選択します。 一意性を確保するために、次の形式をお勧めします。

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

バイナリファイルは、他の開発者からのバイナリファイルなど、アプリケーションパッケージにインストールされます。 「[方法: ソフトウェア開発キットを作成](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)する」の「拡張機能 sdk」を参照してください。

コンポーネントを配布する方法を決定するには、どのように複雑になるかを検討してください。 次の場合には、拡張 SDK または類似のパッケージマネージャーをお勧めします。

-   コンポーネントは複数のファイルで構成されます。
-   複数のプラットフォーム (x86 と ARM など) に対応するコンポーネントのバージョンを提供します。
-   コンポーネントのデバッグバージョンとリリースバージョンの両方を提供します。
-   コンポーネントには、デザイン時にのみ使用されるファイルとアセンブリがあります。

拡張 SDK は、上記の2つ以上が当てはまる場合に特に便利です。

> **注**  複雑なコンポーネントの場合、NuGet パッケージ管理システムには拡張 sdk に代わるオープンソースが用意されています。 拡張 Sdk と同様に、NuGet を使用すると、複雑なコンポーネントのインストールを簡略化するパッケージを作成できます。 NuGet パッケージと Visual Studio 拡張機能 sdk の比較については、「 [nuget と拡張 Sdk を使用した参照の追加](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)」を参照してください。

## <a name="distribution-by-file-copy"></a>ファイルのコピーによる配布

コンポーネントが1つの winmd ファイル、つまり、winmd ファイルとリソースインデックス (pri) ファイルで構成されている場合は、ユーザーがコピーできるように、単に winmd ファイルを使用できます。 ユーザーは、プロジェクト内の任意の場所にファイルを配置できます。 **既存項目の追加** ダイアログボックスを使用して、winmd ファイルをプロジェクトに追加し、参照マネージャー ダイアログボックスを使用して参照を作成します。 .Pri ファイルまたは .xml ファイルを含める場合は、それらのファイルを winmd ファイルに配置するようにユーザーに指示します。

>   Visual Studio では、プロジェクトにリソースが含まれていない場合でも、Windows ランタイムコンポーネントをビルドすると常に .pri ファイルが生成される**ことに注意**してください。 コンポーネント用のテストアプリがある場合は、アプリケーションパッケージの内容を確認するために、bin\\デバッグ\\AppX フォルダー内のアプリケーションパッケージの内容を調べることによって、.pri ファイルが使用されているかどうかを判断できます。 コンポーネントからの .pri ファイルが表示されない場合は、配布する必要はありません。 または、 [Makepri .exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))ツールを使用して、Windows ランタイムコンポーネントプロジェクトからリソースファイルをダンプすることもできます。 たとえば、Visual Studio の [コマンドプロンプト] ウィンドウで、「makepri.exe dump/if MyComponent/of MyComponent」と入力します。[リソース管理システム (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))での .pri ファイルの詳細については、「」を参照してください。

## <a name="distribution-by-extension-sdk"></a>拡張 SDK による配布

通常、複雑なコンポーネントには Windows リソースが含まれますが、前のセクションで説明した空の pri ファイルの検出に関する注意事項を参照してください。

**拡張機能 SDK を作成するには**

1.  Visual Studio SDK がインストールされていることを確認します。 Visual studio SDK は、 [Visual studio のダウンロード](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs)ページからダウンロードできます。
2.  VSIX プロジェクトテンプレートを使用して、新しいプロジェクトを作成します。 このテンプレートは、[機能拡張C# ] カテゴリの [ビジュアル] または [Visual Basic] の下にあります。 このテンプレートは、Visual Studio SDK の一部としてインストールされます。 ([チュートリアル: または Visual Basic をC#使用した sdk の作成](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015)または[チュートリアルC++: を使用した sdk](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)の作成では、非常に簡単なシナリオでこのテンプレートを使用する方法を示します。 )
3.  SDK のフォルダー構造を決定します。 フォルダー構造は、**参照**、再**頒布**、および**デザイン時**フォルダーを使用して、VSIX プロジェクトのルートレベルで開始されます。

    -   **参照**は、ユーザーがプログラミングできるバイナリファイルの場所です。 拡張 SDK は、ユーザーの Visual Studio プロジェクトでこれらのファイルへの参照を作成します。
    -   **Redist**は、コンポーネントを使用して作成されたアプリで、バイナリファイルと共に配布する必要がある他のファイルの場所です。
    -   **デザイン時**は、開発者がコンポーネントを使用するアプリを作成する場合にのみ使用されるファイルの場所です。

    これらの各フォルダーで、構成フォルダーを作成できます。 許可される名前は、debug、retail、および CommonConfiguration です。 CommonConfiguration フォルダーは、リテールビルドとデバッグビルドのどちらで使用されているかにかかわらず、同じファイル用です。 コンポーネントのリテールビルドのみを配布する場合は、すべてを CommonConfiguration に配置し、他の2つのフォルダーを省略することができます。

    各構成フォルダーで、プラットフォーム固有のファイルのアーキテクチャフォルダーを提供できます。 すべてのプラットフォームで同じファイルを使用する場合は、ニュートラルという名前のフォルダーを1つ指定できます。 フォルダー構造の詳細については、「[方法: ソフトウェア開発キットを作成](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)する」を参照してください。 (この記事では、プラットフォーム Sdk と拡張 Sdk の両方について説明します。 混乱を避けるために、プラットフォーム Sdk についてのセクションを折りたたむと役立つ場合があります。 )

4.  SDK マニフェストファイルを作成します。 マニフェストでは、名前とバージョン情報、SDK がサポートするアーキテクチャ、.NET のバージョン、Visual Studio が SDK を使用する方法に関するその他の情報を指定します。 詳細と例については、 [「方法: ソフトウェア開発キットを作成](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)する」を参照してください。
5.  拡張機能 SDK をビルドして配布します。 VSIX パッケージのローカライズや署名などの詳細については、「 [vsix の配置](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)」を参照してください。

## <a name="related-topics"></a>関連トピック

* [ソフトウェア開発キットの作成](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet パッケージ管理システム](https://github.com/NuGet/Home)
* [リソース管理システム (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Visual Studio 拡張機能の検索と使用](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [MakePRI コマンドオプション](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
