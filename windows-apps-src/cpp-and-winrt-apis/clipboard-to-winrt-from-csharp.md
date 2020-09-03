---
title: C# から C++/WinRT への Clipboard サンプルの移植 (ケース スタディ)
description: このトピックでは、[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/microsoft/Windows-universal-samples)のいずれかを [C#](/visualstudio/get-started/csharp) から [C++/WinRT](./intro-to-using-cpp-with-winrt.md) へ移植するケース スタディについて説明します。
ms.date: 04/13/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 移植, 移行, C#, サンプル, クリップボード, ケース, スタディ
ms.localizationpriority: medium
ms.openlocfilehash: 5a7ec46b28a8ddf0b4accadb37b40e786ac8c47a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170416"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>C# から C++/WinRT への Clipboard サンプルの移植 &mdash; ケース スタディ

このトピックでは、[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/microsoft/Windows-universal-samples)のいずれかを [C#](/visualstudio/get-started/csharp) から [C++/WinRT](./intro-to-using-cpp-with-winrt.md) へ移植するケース スタディについて説明します。 チュートリアルの手順を実行し、サンプルを自分で移植することにより、移植を練習し経験を得ることができます。

C# から C++/WinRT への移植に関する技術的な詳細の総合的な分類については、関連トピック「[C# から C++/WinRT への移行](./move-to-winrt-from-csharp.md)」を参照してください。

## <a name="a-brief-preface-about-c-and-c-source-code-files"></a>C# と C++ のソース コード ファイルに関する簡単な概要

C# プロジェクトでは、ソース コード ファイルは主に `.cs` ファイルです。 C++ に移動すると、使用するソース コード ファイルの種類が多いことがわかります。 その理由は、コンパイラ間の差異、C++ ソース コードの再利用方法、型とその関数 (メソッド) の "*宣言*" および "*定義*" の概念に関係しています。

関数の "*宣言*" では、関数の "*シグネチャ*" (戻り値の型、その名前、パラメーターの型および名前) だけを記述します。 関数の "*定義*" には、関数の "*本文*" (その実装) が含まれています。

型に関しては、事情が少し異なります。 型を "*定義*" するには、その名前を指定して、(少なくとも) そのメンバー関数 (およびその他のメンバー) をすべて "*宣言*" するだけでかまいません。 つまり、メンバー関数を定義していない場合でも、型を "*定義*" できます。

- 一般的な C++ ソース コード ファイルは `.h` および `.cpp` ファイルです。 `.h` ファイルは "*ヘッダー*" ファイルです。ここでは、1 つまたは複数の型を定義します。 ヘッダーでメンバー関数を定義することも "*できます*" が、通常は `cpp` ファイルを使用します。 そのため、たとえば C++ の型 **MyClass** の場合、`MyClass.h` で **MyClass** を定義し、`MyClass.cpp` でそのメンバー関数を定義します。 他の開発者がクラスを再利用できるようにするには、`.h` ファイルとオブジェクト コードだけを共有します。 実装は知的財産で構成されているため、`.cpp` ファイルは秘密にしておきます。
- プリコンパイル済みヘッダー (`pch.h`)。 通常、アプリケーションには一連のヘッダー ファイルが含まれていて、それらのセットが変更されることはほとんどありません。 そのため、コンパイルするたびにその一連のヘッダーの内容を処理するのではなく、これらのヘッダーを 1 つに集約し、それを一度コンパイルしてから、ビルドするたびにそのプリコンパイル手順の出力を使用できます。 これは、"*プリコンパイル済みヘッダー*" ファイル (通常、名前は `pch.h`) を使用して実行します。
- `.idl` ファイル。 これらのファイルには、インターフェイス定義言語 (IDL) が含まれています。 IDL は Windows ランタイム型のヘッダー ファイルと考えることができます。 IDL の詳細については、「[**Mainpage** 型の IDL](#idl-for-the-mainpage-type)」セクションを参照してください。

## <a name="download-and-test-the-clipboard-sample"></a>Clipboard サンプルをダウンロードしてテストする

[Clipboard サンプル](/samples/microsoft/windows-universal-samples/clipboard/)の Web ページにアクセスし、「**Download ZIP (ZIP のダウンロード)** 」をクリックします。 ダウンロードしたファイルを解凍し、フォルダー構造を確認します。

- C# バージョンのサンプル ソース コードは、`cs` という名前のフォルダーに格納されています。
- C++/WinRT バージョンのサンプル ソース コードは、`cppwinrt` という名前のフォルダーに格納されています。
- 他のファイル &mdash; C# バージョンと C++/WinRT バージョンの両方で使用される他のファイル &mdash; は、`shared` および `SharedContent` フォルダーにあります。

このトピックのチュートリアルでは、C# のソース コードから移植することにより、Clipboard サンプルの C++/WinRT バージョンを再作成する方法について説明します。 これにより、独自の C# プロジェクトを C++/WinRT に移植する方法を確認できます。

このサンプルの動作を理解するには、C# ソリューション (`\Clipboard_sample\cs\Clipboard.sln`) を開き、必要に応じて構成を変更し (おそらく *x64* に)、ビルドして、実行します。 サンプルの独自のユーザー インターフェイス (UI) では、さまざまな機能について順を追って説明します。

## <a name="create-a-blank-app-cwinrt-named-clipboard"></a>Clipboard という名前の空のアプリ (C++/WinRT) を作成する

> [!NOTE]
> C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

Microsoft Visual Studio で新しい C++/WinRT プロジェクトを作成することにより、移植プロセスを始めます。 **[空のアプリ (C++WinRT)]** プロジェクト テンプレートを使用して新しいプロジェクトを作成します。 その名前を *Clipboard* にして、(フォルダー構造がチュートリアルと一致するように) **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。

ベースラインを確立することだけを目的として、この新しい空のプロジェクトをビルドして実行します。

## <a name="packageappxmanifest-and-asset-files"></a>Package.appxmanifest、資産ファイル

C# バージョンと C++/WinRT バージョンのサンプルを同じコンピューターにサイド バイ サイドでインストールする必要がない場合は、2 つのプロジェクトのアプリケーション パッケージ マニフェスト ソース ファイル (`Package.appxmanifest`) を同じにすることができます。 その場合は、C# プロジェクトから C++/WinRT プロジェクトに `Package.appxmanifest` をコピーするだけで、作業は終了です。

2 つのバージョンのサンプルを共存させる場合は、異なる識別子が必要です。 その場合は、C++/WinRT プロジェクトで、`Package.appxmanifest` ファイルを XML エディターで開き、次の 3 つの値を記録します。

- **/Package/Identity** 要素内で、**Name** 属性の値を記録します。 これは "*パッケージ名*" です。 新しく作成されたプロジェクトでは、プロジェクトによって一意の GUID の初期値に設定されます。
- **/Package/Applications/Application** 要素内で、**Id** 属性の値を記録します。 これは "*アプリケーション ID*" です。
- **/Package/mp:PhoneIdentity** 要素内で、**PhoneProductId** 属性の値を記録します。 やはり、新しく作成されたプロジェクトでは、パッケージ名と同じ GUID に設定されます。

次に、C# プロジェクトから C++/WinRT プロジェクに `Package.appxmanifest` をコピーします。 最後に、記録した 3 つの値を復元できます。 または、コピーした値を編集し、(新しいプロジェクトでは通常そうするように) アプリケーションや組織に対して一意または適切な値にすることができます。 たとえば、この例では、パッケージ名の値を元に戻すのではなく、*Microsoft.SDKSamples.Clipboard.CS* からコピーした値を *Microsoft.SDKSamples.Clipboard.CppWinRT* に単に変更できます。 アプリケーション ID は *App* のままにしてかまいません。 パッケージ名 "*または*" アプリケーション ID が異なっている場合、2 つのアプリケーションのアプリケーション ユーザー モデル ID (AUMID) は異なる値になります。

このチュートリアルでは、`Package.appxmanifest` で他にもいくつか変更を行います。 *Clipboard C# Sample* という文字列が 3 箇所に出現します。 それを *Clipboard C++/WinRT Sample* に変更します。

C++/WinRT プロジェクトでは、`Package.appxmanifest` ファイルとプロジェクトは、参照している資産ファイルに関して同期されなくなります。 これを解決するには、まず、(Visual Studio のソリューション エクスプローラーで) `Assets` フォルダー内のすべてのファイルを選択し、それらを削除する (ダイアログで **[削除]** を選択する) ことにより、C++/WinRT プロジェクトから資産を削除します。

C# プロジェクトでは、共有フォルダーから資産ファイルが参照されます。 C++/WinRT プロジェクトでも同じようにするのでも、このチュートリアルで行うようにファイルをコピーするのでもかまいません。

`\Clipboard_sample\SharedContent\media` フォルダーに移動します。 C# プロジェクトに含まれる 7 つのファイル (`microsoft-sdk.png` から `windows-sdk.png` まで) を選択し、それらをコピーして、新しいプロジェクトの `\Clipboard\Clipboard\Assets` フォルダーに貼り付けます。

(ソリューション エクスプローラーの C++/WinRT プロジェクトで) `Assets` フォルダーを右クリックし、 **[追加]**  >  **[既存の項目...]** を選択して、`\Clipboard\Clipboard\Assets` に移動します。 ファイル ピッカーで 7 つのファイルを選択し、 **[追加]** をクリックします。

`Package.appxmanifest` が、プロジェクトの資産ファイルと同期されるようになります。

## <a name="mainpage-including-the-functionality-that-configures-the-sample"></a>サンプルを構成するための機能を含む **MainPage**

Clipboard サンプルも、すべての[ユニバーサル Windows プラットフォーム (UWP) アプリ サンプル](https://github.com/microsoft/Windows-universal-samples)と同様に、ユーザーが一度に 1 つずつ進めることができるシナリオのコレクションで構成されています。 特定のサンプルのシナリオのコレクションは、サンプルのソース コードで構成されています。 コレクション内の各シナリオは、タイトルを格納するデータ項目と、シナリオを実装するプロジェクトのクラスの型です。

サンプル C# バージョンでは、`SampleConfiguration.cs` ソース コード ファイルを調べると、2 つのクラスがあります。 ほとんどの構成ロジックは、**MainPage** クラスに含まれています。これは部分クラスです (`MainPage.xaml` のマークアップおよび `MainPage.xaml.cs` の命令型コードと組み合わされると、完全なクラスになります)。 このソース コード ファイルのもう 1 つのクラスは、**Scenario** とその **Title** および **ClassType** プロパティです。

次のいくつかのサブセクションでは、**MainPage** と **Scenario** を移植する方法について説明します。

### <a name="idl-for-the-mainpage-type"></a>**MainPage** 型の IDL

このセクションでは最初に、インターフェイス定義言語 (IDL: Interface Definition Language) と、それが C++/WinRT のプログラミング時にどのように役立つかについて簡単に説明します。 IDL は、Windows ランタイム型の呼び出し可能な領域を記述するソース コードの種類です。 型の呼び出し可能 (パブリック) な領域が "*投影*"、公開されて、その型が使用できるようになります。 型の "*投影*" されるこの部分は、型の実際の内部実装 (当然呼び出し不可能で、パブリックでない部分) と対照を成します。 投影されるのは、IDL で定義する部分のみになります。

(`.idl` ファイル内で) IDL ソース コードを作成したら、その IDL をマシンが読み取り可能なメタデータ ファイル (Windows メタデータとも呼ばれます) にコンパイルできます。 これらのメタデータには拡張子 `.winmd` が付いています。その用途をいくつか以下に示します。

- `.winmd` では、コンポーネントの Windows ランタイム型を記述します。 アプリケーション プロジェクトから Windows ランタイム コンポーネント (WRC) を参照すると、WRC に属する Windows メタデータ (このメタデータは別のファイル内にあるか、WRC そのものとして同じファイルにパッケージ化されています) がアプリケーション プロジェクトによって読み取られ、アプリケーション内から WRC の型を使用できるようになります。
- `.winmd` では、アプリケーションのある部分の Windows ランタイム型を記述して、それらを同じアプリケーションの別の部分でも使用できるようにできます。 たとえば、同じアプリの XAML ページで使用される Windows ランタイム型などです。
- Windows ランタイム型を (組み込みまたはサード パーティで) 簡単に使用できるように、C++/WinRT のビルド システムでは、`.winmd` ファイルを使用して、それらの Windows ランタイム型の投影される部分を表すラッパー型を生成します。
- 独自の Windows ランタイム型を簡単に実装できるように、C++/WinRT のビルド システムでは、IDL を `.winmd` ファイルに変換してから、それを使用して投影用のラッパーと、実装のベースとなるスタブを生成します (これらのスタブについては、このトピックで後ほど説明します)。

ここで C++/WinRT と共に使用する IDL の特定のバージョンは、[Microsoft インターフェイス定義言語 3.0](/uwp/midl-3/intro) です。 このセクションの残りの部分では、C# の **MainPage** 型について詳しく説明します。 C++/WinRT の **MainPage** 型の "*投影*" で必要な部分 (つまり、呼び出し可能またはパブリックな領域) と、実装に含めるだけでよい部分を決定します。 この区別が重要なのは、IDL を作成する際に (これはこの後のセクションで実行します)、その中で呼び出し可能な部分だけを定義することになるためです。

**MainPage** 型を実装している C# のソース コード ファイルは、`MainPage.xaml` (この後すぐ、コピーすることによって移植します)、`MainPage.xaml.cs`、`SampleConfiguration.cs` です。

C++/WinRT バージョンでは、同様の方法で **MainPage** 型をソース コード ファイルにファクタリングします。 `MainPage.xaml.cs` のロジックを取得し、その大部分を `MainPage.h` と `MainPage.cpp` に変換します。 `SampleConfiguration.cs` のロジックについては、`SampleConfiguration.h` と `SampleConfiguration.cpp` に変換します。

C# ユニバーサル Windows プラットフォーム (UWP) アプリケーションのクラスは、もちろん Windows ランタイム型です。 ただし、C++/WinRT アプリケーションで型を作成するときは、その型が Windows ランタイム型であるか、または通常の C++ クラス、構造体、列挙であるかを選択できます。

プロジェクトの XAML ページが Windows ランタイム型である必要があるため、**MainPage** は Windows ランタイム型にする必要があります。 C++/WinRT プロジェクトでは、**MainPage** は既に Windows ランタイム型であるため、その部分を変更する必要はありません。 具体的には、それは "*ランタイム クラス*" です。

- 指定した型に対してランタイム クラスを作成するかどうかについて詳しくは、「[C++/WinRT での API の作成](./author-apis.md)」をご覧ください。
- C++/WinRT では、ランタイム クラスの内部実装と、その投影される (パブリックな) 部分が、2 つの異なるクラスの形式で存在します。 これらは、"*実装型*" および "*投影型*" と呼ばれます。 これらの詳細については、上の箇条書きで説明したトピックのほか、「[C++/WinRT での API の使用](./consume-apis.md)」を参照してください。
- ランタイム クラスと IDL (`.idl` ファイル) の間の接続に関する情報については、「[XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)」トピックを参照して従ってください。 そのトピックでは、新しいランタイム クラスを作成する手順が示されています。その最初のステップでは、新しい **Midl ファイル (.idl)** 項目をプロジェクトに追加します。

**MainPage** の場合、実際は必要な `MainPage.idl` ファイルが C++/WinRT プロジェクトに既に存在しています。 これは、プロジェクト テンプレートによって作成されたためです。 ただし、このチュートリアルでは、後でさらに `.idl` ファイルをプロジェクトに追加します。

既存の `MainPage.idl` ファイルに追加する必要がある IDL の正確な一覧は、こので後すぐに示します。 その前に、IDL に含める必要があるものと、必要がないものについての理由を示します。

`MainPage.idl` で宣言する必要がある **MainPage** のメンバーと (**MainPage** ランタイム クラスの一部にするため)、単に **MainPage** 実装型のメンバーにできるものを決定するため、C# の **MainPage** クラスのメンバーの一覧を作成してみます。 それらのメンバーを探すには、`MainPage.xaml.cs` と `SampleConfiguration.cs` を調べます。

全部で 12 個の `protected` および `private` のフィールドとメソッドが見つかります。 そして、次の `public` メンバーが見つかります。

- 既定のコンストラクター `MainPage()`。
- 静的フィールド **Current** と **FEATURE_NAME**。
- プロパティ **IsClipboardContentChangedEnabled** と **Scenarios**。
- メソッド **BuildClipboardFormatsOutputString**、**DisplayToast**、**EnableClipboardContentChangedNotifications**、**NotifyUser**。

`MainPage.idl` で宣言する候補となるのは、それらの `public` メンバーです。 それぞれを調べて、**MainPage** ランタイム クラスに含める必要があるか、または単に実装の一部にする必要があるかを確認します。

- 既定のコンストラクター `MainPage()`。 XAML の**ページ**の場合、IDL 内で既定のコンストラクターを宣言するのが普通です。 それにより、XAML UI フレームワークで型をアクティブ化できます。
- 静的フィールド **Current** は、アプリケーションの **MainPage** のインスタンスにアクセスするため、シナリオの個々の XAML ページ内から使用されます。 **Current** は XAML フレームワークとの相互運用には使用されないため (コンパイル単位間でも使用されません)、単に実装型のメンバーにすることができます。 実際のプロジェクトでも、このような場合は、同じようにできます。 ただし、このフィールドは投影型のインスタンスなので、IDL で宣言することが論理的であると考えられます。 そのため、ここではそのようにします (これにより、コードが若干簡潔になります)。
- 静的な **FEATURE_NAME** フィールドの場合も似ており、**Mainpage.xaml** 型内でアクセスされます。 やはり、IDL 内で宣言するようにすると、コードが多少簡潔になります。
- プロパティ **IsClipboardContentChangedEnabled** は、**OtherScenarios** クラスでのみ使用されます。 そのため、移植では少し単純化し、**OtherScenarios** ランタイム クラスのプライベート フィールドにします。 したがって、それは IDL に入れません。
- プロパティ **Scenarios** は **Scenario** 型 (前に説明した型) のオブジェクトのコレクションです。 **Scenario** については次のサブセクションで説明するので、**Scenarios** プロパティについてもそれまでこのままにします。
- メソッド **BuildClipboardFormatsOutputString**、**DisplayToast**、**EnableClipboardContentChangedNotifications** は、メイン ページというよりサンプルの一般的な状態に関係するユーティリティ関数です。 そのため、移植では、これら 3 つのメソッドを **SampleState** という名前の新しいユーティリティ型にリファクタリングします (これは、Windows ランタイム型である必要はありません)。 したがって、これら 3 つのメソッドは IDL には移動されません。
- **NotifyUser** メソッドは、静的 *Current* フィールドから返される **MainPage** のインスタンス上の個々のシナリオ XAML ページ内から呼び出されます。 (既に説明したように) **Current** は投影型のインスタンスであるため、IDL で **NotifyUser** を宣言する必要があります。 **NotifyUser** は、**NotifyType** 型のパラメーターを受け取ります。 それについては、次のサブセクションで説明します。

データ バインドするメンバーも IDL で宣言する必要があります (`{x:Bind}` または `{Binding}` のどちらを使用しているかに関係なく)。 詳しくは、「[データ バインディング](../data-binding/index.md)」をご覧ください。

作業は進んでいます。`MainPage.idl` ファイルに追加するメンバーと追加しないメンバーの一覧を作成しています。 ただし、**Scenarios** プロパティと、**NotifyType** 型についてまだ検討する必要があります。 そこで、次にそれを行います。

### <a name="idl-for-the-scenario-and-notifytype-types"></a>**Scenario** 型と **NotifyType** 型の IDL

**Scenario** クラスは `SampleConfiguration.cs` で定義されています。 そのクラスを C++/WinRT に移植する方法を決定する必要があります。 既定では、おそらく通常の C++ の `struct` にします。 ただし、**Scenario** がバイナリ間で使用されている場合、または XAML フレームワークと相互運用する場合は、IDL で Windows ランタイム型として宣言する必要があります。

C# のソース コードを調べると、このコンテキストで **Scenario** が使用されていることがわかります。

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

**Scenario** オブジェクトのコレクションは、**ListBox** (項目コントロール) の [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティに割り当てられます。 **Scenario** は XAML と相互運用する必要が "*ある*" ため、Windows ランタイム型にする必要があります。 そのため、IDL で定義する必要があります。 **Scenario** 型を IDL で定義すると、C++/WinRT のビルド システムによって、**Scenario** のソース コード定義がバックグラウンドのヘッダー ファイル (このチュートリアルでは、その名前と場所は重要ではありません) に自動的に生成されます。

また、**MainPage.Scenarios** は、IDL にする必要がある **Scenario** オブジェクトのコレクションであることを思い出してください。 そのため、**MainPage.Scenarios** も IDL で宣言する必要があります。

**NotifyType** は、C# の `MainPage.xaml.cs` で宣言されている `enum` です。 **MainPage** ランタイム クラスに属するメソッドに **NotifyType** を渡すため、**NotifyType** も Windows ランタイム型である必要があり、`MainPage.idl` で定義する必要があります。

次に、IDL で宣言することに決めた **Mainpage** の新しい型と新しいメンバーを `MainPage.idl` ファイルに追加します。 同時に、Visual Studio のプロジェクト テンプレートによって提供されていた **Mainpage** のプレースホルダー メンバーを、IDL から削除します。

そのため、C++/WinRT プロジェクトで `MainPage.idl` を開き、次のリストのように編集します。 編集の 1 つで、名前空間の名前を **Clipboard** から **SDKTemplate** に変更していることに注意してください。 必要に応じて、`MainPage.idl` の内容全体を次のコードで置き換えることもできます。 もう 1 つの注意点は、**Scenario::ClassType** の名前を **Scenario::ClassName** に変更していることです。

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> C++/WinRT プロジェクトでの `.idl` ファイルの内容について詳しくは、[Microsoft インターフェイス定義言語 3.0](/uwp/midl-3/) に関する記事をご覧ください。

実際の移植作業では、ここで行っているような名前空間の名前の変更は、望ましくなく、必要もない場合があります。 ここでそれを行っているのは、移植している C# プロジェクトの既定の名前空間が **SDKTemplate** であるためです。一方で、プロジェクトとアセンブリの名前は **Clipboard** です。

ただし、このチュートリアルで移植を進めていく間に、ソース コードに出現するすべての **Clipboard** という名前空間名を、**SDKTemplate** に変更します。 また、C++/WinRT プロジェクトのプロパティにも **Clipboard** という名前空間名が使用されている場所があるので、この機会にそれを変更します。

Visual Studio の C++/WinRT プロジェクトで、プロジェクトのプロパティ **[共通プロパティ]**  \> **[C++/WinRT]**  \> **[ルート名前空間]** の値を *SDKTemplate* に設定します。

### <a name="save-the-idl-and-re-generate-stub-files"></a>IDL を保存してスタブ ファイルを再生成する

「[XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)」トピックでは、"*スタブ ファイル*" の概念を紹介し、それらの動作について説明しています。 このトピックでも、C++/WinRT のビルド システムでは `.idl` ファイルの内容を Windows メタデータに変換し、そのメタデータから実装のベースとなるスタブが `cppwinrt.exe` というツールによって生成されることを説明した前のところで、スタブについて説明しました。

IDL の内容を追加、削除、または変更し、ビルドを行うたびに、これらのスタブ ファイルのスタブ実装はビルド システムによって更新されます。 そのため、IDL を変更してビルドを行うたびに、これらのスタブ ファイルを確認し、変更されたシグネチャをコピーして、それらをご自分のプロジェクトに貼り付けることをお勧めします。 これを実行する正確な方法の詳細と例については、後で説明します。 ただし、これを実行するメリットは、実装型のあるべき形とそのメソッドの必要なシグネチャをいつでも間違いなく把握できることです。

ここまでで、`MainPage.idl` ファイルの編集はひとまず完了したので、保存する必要があります。 現時点ではプロジェクトのビルドは完了しませんが、ここでビルドを実行しておくと、**MainPage** 用のスタブ ファイルを再生成されるため、役に立ちます。

この C++/WinRT プロジェクトでは、スタブ ファイルは `\Clipboard\Clipboard\Generated Files\sources` フォルダーに生成されます。 部分的なビルドが完了すると、そこでスタブ ファイルが見つかります (やはり、予想どおり、ビルドは成功しません。 ただし、関心のあるスタブ生成ステップは成功して "*います*")。 重要なファイルは `MainPage.h` と `MainPage.cpp` です。

それら 2 つのスタブ ファイルには、IDL に追加した **MainPage** のメンバーの新しいスタブ実装が含まれます (たとえば、**Current** や **FEATURE_NAME**)。 それらのスタブ実装を、プロジェクトに既に存在する `MainPage.h` ファイルと `MainPage.cpp` ファイルにコピーします。 同時に、IDL で行ったのと同じように、それらの既存ファイルから、Visual Studio のプロジェクト テンプレートによって自動的に生成された **Mainpage** のプレースホルダー メンバーを削除します (**MyProperty** という名前のダミー プロパティ、および **ClickHandler** という名前のイベント ハンドラー)。

実際、現在のバージョンの **MainPage** のメンバーで残しておく必要があるものは、コンストラクターだけです。

スタブ ファイルから新しいメンバーをコピーし、不要なメンバーを削除して、名前空間を更新すると、プロジェクトの `MainPage.h` ファイルと `MainPage.cpp` ファイルは以下のコード リストのようになります。 2 種類の **MainPage** があります。 1 つは**implementation** 名前空間、2 つ目は **factory_implementation** 名前空間にあります。 **factory_implementation** に対する唯一の変更点は、**SDKTemplate** をその名前空間に追加することだけです。

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

文字列の場合、C# では **System.String** が使用されます。 例については、**MainPage.NotifyUser** メソッドを参照してください。 この IDL では、文字列は **String** で宣言されており、`cppwinrt.exe` ツールによる C++/WinRT のコードの自動生成では、[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring) 型が使用されます。 C# のコードで文字列が使用されている場合は常に、それを **winrt::hstring** に移植します。 詳しくは、「[C++/WinRT での文字列の処理](./strings.md)」をご覧ください。

メソッドのシグネチャでの `const&` パラメーターについては、「[パラメーターの引き渡し](./concurrency.md#parameter-passing)」をご覧ください。

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>残りのすべての名前空間の宣言と参照を更新してビルドする

C++/WinRT プロジェクトをビルドする前に、**Clipboard** 名前空間を宣言 (および参照) している箇所をすべて検索し、**SDKTemplate** に変更します。

- `MainPage.xaml` と `App.xaml`。 名前空間は、`x:Class` 属性と `xmlns:local` 属性の値に含まれます。
- `App.idl` の順にクリックします。
- `App.h` の順にクリックします。
- `App.cpp` の順にクリックします。 2 つの `using namespace` ディレクティブ (部分文字列 `using namespace Clipboard` を検索) と、**MainPage** 型の 2 つの修飾 (`Clipboard::MainPage` を検索)。 それらの変更が必要です。

**MainPage** からイベント ハンドラーを削除したので、`MainPage.xaml` に移動し、マークアップから **Button** 要素も削除します。

すべてのファイルを保存します。 ソリューションをクリーンアップし ( **[ビルド]**  >  **[Clipboard のクリーン]** )、ビルドを行います。 これまでに行った変更が有効であれば、ビルドは成功します。

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>IDL で宣言した **MainPage** のメンバーを実装する

#### <a name="the-constructor-current-and-feature_name"></a>コンストラクター、**Current**、**FEATURE_NAME**

次に示すのは、移植する必要のある関連するコード (C# プロジェクトから) です。

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

後で、`MainPage.xaml` を (コピーして) そのまま再利用します。 ここでは、**TextBlock** 要素を、適切な名前を付けて、C++/WinRT プロジェクトの `MainPage.xaml` に一時的に追加します。

**FEATURE_NAME** は、**MainPage** の静的フィールドであり (C# の `const` フィールドの動作は基本的に静的です)、`SampleConfiguration.cs` で定義されています。 C++/WinRT では、(静的) フィールドではなく、(静的) 読み取り専用プロパティの C++/WinRT 式にします。 C++/WinRT でプロパティ ゲッターを表す方法は、パラメーター (アクセサー) を受け取らずにプロパティの値を返す関数です。 したがって、C# の **FEATURE_NAME** 静的フィールドは、C++/WinRT では **FEATURE_NAME** 静的アクセサー関数になります (この場合は、文字列リテラルを返します)。

ちなみに、C# の読み取り専用プロパティを移植する場合も同じようにします。 C# の書き込み可能プロパティの場合、C++/WinRT でプロパティ セッターを表す方法は、パラメーター (ミューテーター) としてプロパティの値を受け取る `void` 関数です。 どちらの場合も、C# のフィールドまたはプロパティが静的である場合は、C++/WinRT のアクセサーまたはミューテーターも静的になります。

**Current** は、**MainPage** の静的 (定数ではない) フィールドです。 ここでも、それを読み取り専用プロパティ (の C++/WinRT 表現) にし、やはり静的にします。 **FEATURE_NAME** は定数ですが、**Current** はそうではありません。 そのため、C++/WinRT では、バッキング フィールドが必要であり、アクセサーではそれが返されます。 したがって、C++/WinRT プロジェクトでは、**current** という名前のプライベート静的フィールドを `MainPage.h` で宣言し、`MainPage.cpp` で **current** を定義および初期化して (静的ストレージの存続期間があるため)、**Current** という名前のパブリック静的アクセサー関数を使用してアクセスします。

コンストラクター自体でいくつかの割り当てが実行されますが、これは簡単に移植できます。

C++/WinRT プロジェクトでは、 **[Visual C++]**  >  **[コード]**  >  **[C++ファイル (.cpp)]** で、`SampleConfiguration.cpp` という名前の新しい項目を追加します。

以下のリストと一致するように、`MainPage.xaml`、`MainPage.h`、`MainPage.cpp`、`SampleConfiguration.cpp` を編集します。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

また、**MainPage::Current()** および **MainPage::FEATURE_NAME()** の `MainPage.cpp` から、既存の関数本体を削除します。これらのメソッドは他の場所で定義されているためです。

ご覧のように、**MainPage::current** は **SDKTemplate::MainPage** 型として宣言されており、これは投影型です。 実装型である **SDKTemplate::implementation::MainPage** 型ではありません。 投影型は、XAML 相互運用のためにプロジェクト内で、またはバイナリ全体で、使用するように設計されています。 実装型は、投影型で公開した機能を実装するために使用する型です。 **MainPage::current** の宣言 (`MainPage.h` 内) は、実装名前空間 (**winrt::SDKTemplate::implementation**) 内にあるため、修飾されていない **MainPage** では実装型が参照されます。 したがって、**MainPage::current** が投影型 **winrt::SDKTemplate::MainPage** のインスタンスであることを明確にするため、**SDKTemplate::** で修飾します。

コンストラクターには、`MainPage::current = *this;` に関連して説明が必要な点がいくつかあります。
- 実装型のメンバー内で `this` ポインターを使用するときは、当然、`this` ポインターは "*実装型へのポインターです*"。
- `this` ポインターを対応する投影型に変換するには、それを逆参照します。 (ここで行ったように) IDL から実装型を生成すると、実装型にはそれを投影型に変換する変換演算子があります。 そのため、ここでは割り当てが機能します。

それらについて詳しくは、「[実装型とインターフェイスをインスタンス化して返す](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces)」をご覧ください。

また、コンストラクターでも `SampleTitle().Text(FEATURE_NAME());` です。 `SampleTitle()` の部分は、**SampleTitle** という名前の簡単なアクセサー関数の呼び出しであり、XAML に追加した **TextBlock** が返されます。 XAML 要素 `x:Name` を使用するたびに、XAML コンパイラによってその要素の名前が付けられたアクセサーが自動的に生成されます。 `.Text(...)` の部分では、**SampleTitle** アクセサーから返された **TextBlock** オブジェクトに対して、**Text** ミューテーター関数を呼び出します。 `FEATURE_NAME()` では、静的な **MainPage::FEATURE_NAME** アクセサー関数が呼び出されて、文字列リテラルが返されます。 全体として、そのコード行では、*SampleTitle* という名前の **TextBlock** の **Text** プロパティが設定されます。

Windows ランタイムでは文字列はワイドであるため、文字列リテラルを移植するには、その前にワイド文字エンコード プレフィックス `L` を付けてあることに注意してください。 たとえば、"a string literal" は L"a string literal" に変更します。 「[ワイド文字列リテラル](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals)」も参照してください。

#### <a name="scenarios"></a>**シナリオ**

関連する C# コードで移植する必要があるものを次に示します。

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

以前の調査で、この **Scenario** オブジェクトのコレクションは **ListBox** に表示されることがわかっています。 C++/WinRT では、アイテム コントロールの **ItemsSource** プロパティに割り当てることができる "*コレクションの種類*" には制限があります。 コレクションはベクターまたは監視可能なベクターである必要があり、その要素は次のいずれかである必要があります。

- ランタイム クラス
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)

**IInspectable** のケースでは、要素自体がランタイム クラスでない場合、それらの要素は [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) との間でボックス化およびボックス化解除できる種類である必要があります。 これは、それらが Windows ランタイム型である必要があることを意味します (「[IInspectable へのスカラー値のボックス化とボックス化解除](./boxing.md)」を参照)。

このケース スタディでは、**Scenario** をランタイム クラスにしませんでした。 ただし、それでも適切なオプションです。 また、実際の移植作業では、ランタイム クラスを使用する必要がある場合もあります。 たとえば、要素の型を "*監視可能*" にする必要がある場合 (「[XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md)」を参照)、または他の理由によって要素がメソッドを持つ必要があり、それが単なるデータ メンバーのセットではない場合などです。

このチュートリアルでは、**Scenario** 型に対してランタイム クラスを使用 "*しない*" ため、ボックス化について考える必要があります。 **Scenario** を C++ の通常の `struct` にした場合は、それをボックス化することはできません。 しかし、**Scenario** を IDL で `struct` として宣言したので、それをボックス化 "*できます*"。

**Scenario** を事前にボックス化するか、または **ItemsSource** に割り当てるまで待って、ジャストインタイムでボックス化するかを、選択できます。 ここでは、これら 2 つのオプションに関する考慮事項について説明します。

- 事前のボックス化。 このオプションでは、データ メンバーは **IInspectable** のコレクションであり、UI に割り当てる準備ができています。 初期化時に、**Scenario** をそのデータ メンバーにボックス化します。 そのコレクションのコピーは 1 つしか必要ありませんが、フィールドの読み取りが必要になるたびに要素のボックス化を解除する必要があります。
- ジャストインタイムのボックス化。 このオプションでは、データ メンバーは **Scenario** のコレクションです。 UI に割り当てる時点で、**Scenario** オブジェクトをデータ メンバーから **IInspectable** の新しいコレクションにボックス化します。 ボックス化を解除しなくてもデータ メンバーの要素のフィールドを読み取ることができますが、コレクションのコピーが 2 つ必要です。

ご覧のように、このような小さなコレクションでは、長所と短所はほとんど差し引きゼロになります。 そのため、このケース スタディでは、ジャストインタイムのオプションを使用します。

**scenarios** メンバーは **MainPage** のフィールドであり、`SampleConfiguration.cs` において定義および初期化されています。 また、**Scenarios** は **MainPage** の読み取り専用プロパティであり、`MainPage.xaml.cs` において定義されています (そして、単に **scenarios** フィールドを返すように実装されています)。 C++/WinRT プロジェクトでも似たようなことを行います。ただし、2 つのメンバーを静的にします (アプリケーション全体で 1 つのインスタンスのみが必要であり、クラス インスタンスを必要とせずにアクセスできるようにするため)。 それらに、それぞれ *scenariosInner* および *scenarios* という名前を付けます。 `MainPage.h` で *scenariosInner* を宣言します。 また、静的ストレージ存続期間があるため、`.cpp` ファイル (この場合は `SampleConfiguration.cpp`) の中で定義および初期化します。

以下のリストと一致するように、`MainPage.h` および `SampleConfiguration.cpp` を編集します。

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

また、そのメソッドはヘッダー ファイルで定義されているので、**MainPage::scenarios()** に対する `MainPage.cpp` から既存の関数本体を削除します。

ご覧のように、`SampleConfiguration.cpp` では、[winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) という名前の C++/WinRT ヘルパー関数を呼び出すことによって、静的データ メンバー *scenariosInner* を初期化します。 その関数では、新しい Windows ランタイム コレクション オブジェクトが自動的に作成されて、[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) インターフェイスとして返されます。 このサンプルでは、コレクションは "*監視可能*" ではないため (初期化後に要素の追加や削除を行わないので、必要ありません)、代わりに [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector) を呼び出すことができます。 その関数からは、[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) インターフェイスとしてコレクションが返されます。

コレクションおよびそれらへのバインドの詳細については、「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](./binding-collection.md)」および「[C++/WinRT でのコレクション](./collections.md)」を参照してください。

追加した初期化コードは、まだプロジェクトにない型が参照されています (**winrt::SDKTemplate::CopyText** など)。 それを解決するため、プロジェクトに新しい空の XAML ページを 5 つ追加してみましょう。

#### <a name="add-five-new-blank-xaml-pages"></a>5 つの新しい空の XAML ページを追加する

**[XAML]**  >  **[空白のページ (C++/WinRT)]**  で新しい項目をプロジェクトに追加します ( **[空白のページ (C++/WinRT)]** 項目テンプレートであり、 **[空白のページ]** ではないことに注意してください)。 名前を `CopyText` に設定します。 新しい XAML ページは、**SDKTemplate** 名前空間内で定義されています。それが適切です。

上記のプロセスを 4 回繰り返し、XAML ページに `CopyImage`、`CopyFiles`、`HistoryAndRoaming`、`OtherScenarios` という名前を付けます。

必要であれば、もう一度ビルドできるようになります。

#### <a name="notifyuser"></a>**NotifyUser**

この C# プロジェクトには、`MainPage.xaml.cs` での **MainPage.NotifyUser** メソッドの実装が含まれます。 **MainPage.NotifyUser** は **MainPage.UpdateStatus** に依存しており、MainPage.UpdateStatus メソッドにはまだ移植されていない XAML 要素に対する依存関係があります。 ここでは、C++/WinRT プロジェクトの **UpdateStatus** メソッドをスタブにしておき、後でそれを移植します。

関連する C# コードで移植する必要があるものを次に示します。

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** では、[**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority) 列挙型が使用されます。 C++/WinRT では、Windows 名前空間の型を使用する場合は常に、対応する C++/WinRT Windows 名前空間ヘッダー ファイルを含める必要があります (詳細については、「[C++/WinRT の使用を開始する](./get-started.md)」を参照)。 この例では、次のコード リストに示すように、ヘッダーは `winrt/Windows.UI.Core.h` であり、それを `pch.h` にインクルードします。

**UpdateStatus** はプライベートです。 そのため、それを **Mainpage.xaml** 実装型でプライベート メソッドにします。 **UpdateStatus** はランタイム クラスで呼び出されないため、IDL では宣言しません。

**MainPage.NotifyUser** を移植し、**MainPage.UpdateStatus** をスタブ化した後、C++/WinRT は次のようになります。 このコード リストの後で、詳細をいくつか確認します。

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

C# では、*dot into* の入れ子になったプロパティにドット表記を使用することもできます。 したがって、C# の **MainPage** 型では、それ自体の **Dispatcher** プロパティに、`Dispatcher` という構文を使用してアクセスできます。 さらに、C# では、`Dispatcher.HasThreadAccess` のような構文を使用して、その値に "*ドットを付ける*" ことができます。 C++/WinRT では、プロパティはアクセサー関数として実装されるため、構文が異なるのは、各関数呼び出しにかっこを追加する点だけです。

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

C# バージョンの **NotifyUser** で [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) を呼び出すと、非同期コールバック デリゲートがラムダ関数として実装されます。 C++/WinRT バージョンでも同じことが行われますが、構文は少し異なります。 C++/WinRT では、後で使用する 2 つのパラメーターと `this` ポインター (メンバー関数を呼び出すため) を " *キャプチャ*" します。 ラムダとしてのデリゲートの実装に関する詳細とコード例については、「[C++/WinRT でのデリゲートを使用したイベントの処理](./handle-events.md)」を参照してください。 また、この特定のケースでは、`var task =` の部分を無視することもできます。 非同期で返されるオブジェクトを待機しないため、保存する必要はありません。 

### <a name="implement-the-remaining-mainpage-members"></a>**MainPage** の残りのメンバーを実装する

これまでに移植したものと、まだ移植していないものを確認できるように、**MainPage** のメンバーの完全な一覧を示します (`MainPage.xaml.cs` と `SampleConfiguration.cs` で実装されているもの)。

|メンバー|アクセス権|状態|
|-|-|-|
|**MainPage** コンストラクター|`public`|移植済み|
|**Current** プロパティ|`public`|移植済み|
|**FEATURE_NAME** プロパティ|`public`|移植済み|
|**IsClipboardContentChangedEnabled** プロパティ|`public`|開始前|
|**Scenarios** プロパティ|`public`|移植済み|
|**BuildClipboardFormatsOutputString** メソッド|`public`|開始前|
|**DisplayToast** メソッド|`public`|開始前|
|**EnableClipboardContentChangedNotifications** メソッド|`public`|開始前|
|**NotifyUser** メソッド|`public`|移植済み|
|**OnNavigatedTo** メソッド|`protected`|開始前|
|**isApplicationWindowActive** フィールド|`private`|開始前|
|**needToPrintClipboardFormat** フィールド|`private`|開始前|
|**scenarios** フィールド|`private`|移植済み|
|**Button_Click** メソッド|`private`|開始前|
|**DisplayChangedFormats** メソッド|`private`|開始前|
|**Footer_Click** メソッド|`private`|開始前|
|**HandleClipboardChanged** メソッド|`private`|開始前|
|**OnClipboardChanged** メソッド|`private`|開始前|
|**OnWindowActivated** メソッド|`private`|開始前|
|**ScenarioControl_SelectionChanged** メソッド|`private`|開始前|
|**UpdateStatus** メソッド|`private`|スタブ化|

次のいくつかのサブセクションでは、まだ移植されていないメンバーについて説明します。

> [!NOTE]
> ソース コードでは、XAML マークアップ (`MainPage.xaml`) 内の UI 要素が参照されていることがあります。 そのような参照の箇所では、簡単なプレースホルダー要素を XAML に追加することによって一時的に回避します。 そうすることで、各サブセクションの後でプロジェクトを引き続きビルドできます。 別の方法として、ここで C# プロジェクトから `MainPage.xaml` の内容 "*全体*" を C++/WinRT プロジェクトにコピーして、参照を解決することもできます。 しかし、そのようにすると、再びビルドできるようになるまでに長い時間がかかります (そのため、途中で発生した入力ミスやその他のエラーが隠される可能性があります)。
>
> **MainPage** クラスの命令型コードの移植が完了したら、"*その後で*"、XAML ファイルの内容をコピーし、プロジェクトを引き続きビルドできるようにします。

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

これは、取得および設定できる C# プロパティであり、既定では `false` です。 **MainPage** のメンバーであり、`SampleConfiguration.cs` において定義されています。

C++/WinRT では、フィールドとして、アクセサー関数、ミューテーター関数、およびバッキング データ メンバーが必要になります。 **IsClipboardContentChangedEnabled** では、**MainPage** 自体の状態ではなく、サンプル内のいずれかのシナリオの状態が表されているため、**SampleState** という名前の新しいユーティリティ型で新しいメンバーを作成します。 そして、それを `SampleConfiguration.cpp` ソース コード ファイルで実装し、メンバーを `static` にします (アプリケーション全体で 1 つのインスタンスのみが必要であり、クラス インスタンスを必要とせずにアクセスできるようにするため)。

`SampleConfiguration.cpp` を C++/WinRT プロジェクトに追加するため、 **[Visual C++]**  >  **[コード]**  >  **[ヘッダー ファイル (.h)]** で、`SampleConfiguration.h` という名前の新しい項目を追加します。 以下のリストと一致するように、`SampleConfiguration.h` および `SampleConfiguration.cpp` を編集します。

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

ここでも、`static` ストレージ (**SampleState::isClipboardContentChangedEnabled** など) のフィールドを、アプリケーションで 1 回定義する必要があります。また、`.cpp` ファイルは、そのための適切な場所です (この例では `SampleConfiguration.cpp`)。

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

このメソッドは **MainPage** のパブリック メンバーであり、`SampleConfiguration.cs` において定義されています。

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

C++/WinRT では、**BuildClipboardFormatsOutputString** を **SampleState** のパブリック静的メソッドにします。 どのインスタンス メンバーにもアクセスしないため、`static` にできます。

C++/WinRT で **Clipboard** 型と **DataPackageView** 型を使用するには、C++/WinRT の Windows 名前空間ヘッダー ファイル `winrt/Windows.ApplicationModel.DataTransfer.h` をインクルードする必要があります。

C# では、**DataPackageView.AvailableFormats** プロパティは **IReadOnlyList** であるため、その **Count** プロパティにアクセスできます。 C++/WinRT では、**DataPackageView::AvailableFormats** アクセサー関数から返される **IVectorView** に、呼び出すことができる **Size** アクセサー関数が含まれます。

C# の **System.Text.StringBuilder** 型の使用を移植するには、C++ の標準型 [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream) を使用します。 その型は、ワイド文字列の出力ストリームです (それを使用するには、`sstream` ヘッダー ファイルをインクルードする必要があります)。 **StringBuilder** で行うように **Append** メソッドを使用するのではなく、**wostringstream** などの出力ストリームでは[挿入演算子](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) を使用します。 詳細については、「[iostream プログラミング](/cpp/standard-library/iostream-programming)」および [C++/WinRT の文字列の書式設定](./strings.md#formatting-strings)に関する記事を参照してください。

C# のコードでは、`new` キーワードを使用して **StringBuilder** を構築します。 C# では、オブジェクトは既定では参照型であり、`new` を使用してヒープで宣言されます。 最新の標準 C++ では、オブジェクトは既定では値型であり、(`new` を使用せずに) スタックで宣言されています。 そのため、`StringBuilder output = new StringBuilder();` は、単純な `std::wostringstream output;` として C++/WinRT に移植します。

C# の `var` キーワードは、コンパイラに型を推定するように要求します。 `var` は、C++/WinRT では `auto` に移植します。 ただし、C++/WinRT では、(コピーを回避するために) 推定 (推測) された型を "*参照*" し、`auto&` で推論された型への lvalue 参照を表します。 また、*lvalue* または *rvalue* で初期化されるかどうかにかかわらず、適切にバインドされる特殊な参照が必要な場合もあります。 それは、`auto&&` で表します。 次に示す移植されたコードの `for` ループで、この形式が使用されています。 *lvalues* と *rvalues* の概要については、「[値のカテゴリと、その参照](./cpp-value-categories.md)」を参照してください。

以下のリストと一致するように、`pch.h`、`SampleConfiguration.h`、`SampleConfiguration.cpp` を編集します。

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> コード行 `DataPackageView clipboardContent{ Clipboard::GetContent() };` の構文では、"*均一な初期化*" と呼ばれる最新の C++ 標準の機能が使用されており、`=` 符号ではなく中かっこの特徴的な使用が示されています。 この構文を使用すると、代入ではなく初期化が行われることが明確にわかります。 代入のように "*見える*" (実際にはそうではありません) 構文を使用する方がよい場合は、上記の構文を同等の `DataPackageView clipboardContent = Clipboard::GetContent();` に置き換えることができます。 ただし、目にするコードではどちらも頻繁に使用されている可能性があるため、両方の初期化表現方法に慣れておくことをお勧めします。

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** は C# の **MainPage** クラスのパブリック静的メソッドであり、`SampleConfiguration.cs` で定義されています。 C++/WinRT では、それを **SampleState** のパブリック静的メソッドにします。

このメソッドの移植に関する詳細と手法のほとんどについては、既に説明してあります。 1 つ新しく注意する必要があることとして、C# の逐語的文字列リテラル (`@`) は、C++ の標準の[未加工文字列リテラル](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11) (`LR`) に移植します。

また、C++/WinRT で [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) 型と [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) を参照するときは、名前空間名でそれらを修飾するか、`SampleConfiguration.cpp` を編集して次の例のような `using namespace` ディレクティブを追加することができます。

```cppwinrt
using namespace Windows::UI::Notifications;
```

[**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) 型を参照するとき、および他の任意の Windows ランタイム型を参照するときにも常に、同じ選択肢があります。

それらの項目以外については、前に説明したのと同じガイダンスに従って、次の手順のようにします。

- `SampleConfiguration.h` でメソッドを宣言し、`SampleConfiguration.cpp` で定義します。
- `pch.h` を編集して、必要なすべての C++/WinRT Windows 名前空間ヘッダー ファイルをインクルードします。
- ヒープではなく、スタックで C++/WinRT オブジェクトを構築します。
- プロパティの get アクセサーの呼び出しを、関数呼び出しの構文 (`()`) に置き換えます。

コンパイラやリンカーのエラーのよくある原因は、必要な C++/WinRT Windows 名前空間ヘッダー ファイルのインクルードを忘れることです。 可能性のあるエラーについて詳しくは、「[リンカーで "LNK2019: 外部シンボルは未解決です" というエラーが発生するのはなぜですか?](./faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)」を参照してください。

チュートリアルに従って **DisplayToast** を自分で移植したい場合は、自分の結果と、ダウンロードした [Clipboard サンプル](/samples/microsoft/windows-universal-samples/clipboard/) ZIP に含まれる C++/WinRT バージョンのソース コードを比較できます。

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** は C# の **MainPage** クラスのパブリック静的メソッドであり、`SampleConfiguration.cs` において定義されています。

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

C++/WinRT では、それを **SampleState** のパブリック静的メソッドにします。

C# では、`+=` と `-=` の演算子構文を使用して、イベント処理デリゲートの登録と取り消しを行います。 C++/WinRT では、「[C++/WinRT でのデリゲートを使用したイベントの処理](./handle-events.md)」で説明されているように、デリゲートの登録と取り消しを行うための構文オプションがいくつかあります。 ただし、一般的な形式では、イベントの名前が付いた関数のペアを呼び出すことによって、登録と取り消しを行います。 登録するには、デリゲートを登録関数に渡し、返される取り消しトークンを取得します ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token))。 取り消すには、そのトークンを取り消し関数に渡します。 この場合、ハンドラーは静的であり (次のコード リストを参照)、関数の呼び出し構文は簡単です。

同様のトークンは、実際には C# の内側で*使用されています*。 しかし、言語によってその詳細が暗黙になります。 C++/WinRT では、暗黙になります。

C# のイベント ハンドラーのシグネチャには、**object** 型が含まれます。 C# 言語では、**object** は、.NET の [**System.Object**](/dotnet/api/system.object) 型に対する[エイリアス](/dotnet/csharp/language-reference/builtin-types/reference-types)です。 C++/WinRT でそれに相当するのは、[**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) です。 そのため、C++/WinRT のイベント ハンドラーには **IInspectable** が含まれます。

以下のリストと一致するように、`SampleConfiguration.h` および `SampleConfiguration.cpp` を編集します。

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

今のところ、イベント処理デリゲート自体 (**OnClipboardChanged** と **OnWindowActivated**) はスタブのままにしておきます。 これらは既に移植するメンバーのリストに含まれているので、後のサブセクションで説明します。

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** は、C# の **MainPage** クラスの保護されたメソッドであり、`MainPage.xaml.cs` において定義されています。 以下では、それで参照されている XAML の **ListBox** と共に示されています。

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

これは、**Scenario** オブジェクトのコレクションが UI に割り当てられる場所であり、重要で興味深いメソッドです。 C# のコードでは、**Scenario** オブジェクトの [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1) が構築され、それが **ListBox** (項目コントロール) の [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティに割り当てられます。 また、C# では、[文字列補間](/dotnet/csharp/language-reference/tokens/interpolated)を使用して、各 **Scenario** オブジェクトのタイトルが作成されています (特殊文字 `$` の使用に注意)。

C++/WinRT では、**OnNavigatedTo** を **MainPage** のパブリック メソッドにします。 そして、ビルドが成功するように、スタブの **ListBox** 要素を XAML に追加します。 コード リストの後で、詳細をいくつか確認します。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

やはり [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 関数を呼び出しますが、ここでは [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) のコレクションを作成します。 それは、ジャストインタイムで **Scenario** オブジェクトのボックス化を実行するために行った決定の一部でした。

また、ここでは C# の[文字列補間](/dotnet/csharp/language-reference/tokens/interpolated)を使用する代わりに、[**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 関数と、**winrt::hstring** の[連結演算子](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)の組み合わせを使用します。

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

C# では、**isApplicationWindowActive** は **MainPage** クラスに属する単純なプライベート `bool` フィールドであり、`SampleConfiguration.cs` において定義されています。 既定値は `false` です。 C++/WinRT では、`SampleConfiguration.h` および `SampleConfiguration.cpp` ファイルで **SampleState** のパブリック静的フィールドにします (既に説明した理由のため)。既定値は同じです。

静的フィールドを宣言、定義、初期化する方法については既に説明しました。 見直す場合は、**isClipboardContentChangedEnabled** フィールドで行ったことを再確認し、**isApplicationWindowActive** でも同じことを行います。

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

**isApplicationWindowActive** と同じパターンです (1 つ前の項目を参照)。

#### <a name="button_click"></a>**Button_Click**

**Button_Click** は、C# の **MainPage** クラスのプライベート (イベント処理) メソッドであり、`MainPage.xaml.cs` において定義されています。 以下では、それで参照されている XAML の **SplitView** と、それを登録している **ToggleButton** と共に示されています。

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
<ToggleButton Click="Button_Click" .../>
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

また、C++/WinRT に移植された同等のコードがこれです。 C++/WinRT バージョンでは、イベント ハンドラーが `public` であることに注意してください (ご覧のように、`private:` 宣言の "*前*" で宣言します)。 これは、XAML マークアップがアクセスするために、このような XAML マークアップに登録されているイベント ハンドラーを C++/WinRT で `public` にする必要があるためです。 命令型コードでイベント ハンドラーを (以前の **MainPage::EnableClipboardContentChangedNotifications** と同様に) 登録する場合、イベント ハンドラーを `public` にする必要はありません。

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

C# では、**DisplayChangedFormats** は **MainPage** クラスに属するプライベート メソッドであり、`SampleConfiguration.cs` において定義されています。

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

C++/WinRT では、`SampleConfiguration.h` および `SampleConfiguration.cpp` ファイルで、**SampleState** のプライベート静的フィールドにします (どのインスタンス メンバーにもアクセスしません)。 このメソッドの C# コードでは、**System.Text.StringBuilder** は使用されていません。ただし、それを使用すると C++/WinRT バージョンに対する十分な文字列書式設定が行われ、これは **std::wostringstream** を使用するもう 1 つの適切な場所です。

C# コードで使用されている静的な [**System.Environment.NewLine**](/dotnet/api/system.environment.newline) プロパティの代わりに、標準の C++ `std::endl` (改行文字) を出力ストリームに挿入します。

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

上の C++/WinRT バージョンの設計は、若干非効率的です。 最初に、**std::wostringstream** を作成しています。 しかし、**BuildClipboardFormatsOutputString** メソッド (前に移植したもの) も呼び出します。 そのメソッドでは、独自の **std::wostringstream** が作成されます。 そして、そのストリームを **winrt::hstring** に変換して、それを返します。 [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 関数を呼び出して、返された **hstring** を C スタイルの文字列に戻し、ストリームに挿入します。 **std::wostringstream** を 1 つだけ作成し、それ (への参照) を受け渡して、メソッドが文字列を直接挿入できるようにすると、さらに効率的になります。

(ダウンロードした ZIP に含まれる) [Clipboard サンプル](/samples/microsoft/windows-universal-samples/clipboard/)の C++/WinRT バージョンのソース コードでは、それが行われています。 そのソース コードに含まれる **SampleState::AddClipboardFormatsOutputString** という名前の新しいプライベート静的メソッドは、出力ストリームへの参照を受け取って操作します。 そして、メソッド **SampleState::DisplayChangedFormats** と **SampleState::BuildClipboardFormatsOutputString** が、その新しいメソッドを呼び出すようにリファクタリングされています。 これは、このトピックのコード リストと機能的には同等ですが、より効率的です。

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** は、C# の **MainPage** クラスに属している非同期イベント ハンドラーであり、`MainPage.xaml.cs` において定義されています。 以下のコード リストは、ダウンロードしたソース コードのメソッドと機能的には同等です。 ただし、ここでは、1 行から 4 行に展開してあり、処理内容の確認と、それに基づく移植方法の決定を、簡単に行うことができます。

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

技術的には、メソッドは非同期であり、`await` の後は何も行われないため、`await` (または `async` キーワード) は必要ありません。 Visual Studio で IntelliSense メッセージが表示されないようにするために使用されることがよくあります。

C++/WinRT の同等のメソッドも非同期です ([**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) を呼び出すため)。 ただし、`co_await` を行ったり、非同期オブジェクトを返したりする必要はありません。 `co_await` および非同期オブジェクトについては、「[C++/WinRT を使用した同時実行操作と非同期操作](./concurrency.md)」を参照してください。

次に、メソッドの動作について説明します。 これは、**HyperlinkButton** の **Click** イベントに対するイベント ハンドラーであるため、*sender* という名前のオブジェクトは実際には **HyperlinkButton** です。 そのため、型変換は安全です (代わりに、この変換を `sender as HyperlinkButton` と表すこともできます)。 次に、**Tag** プロパティの値を取得します (C# プロジェクトの XAML マークアップを確認すると、これが Web URL を表す文字列に設定されていることがわかります)。 **FrameworkElement.Tag** プロパティ (**HyperlinkButton** は **FrameworkElement** です) は **object** 型ですが、C# では [**Object.ToString**](/dotnet/api/system.object.tostring) を使用してこれを文字列化できます。 結果の文字列から、**Uri** オブジェクトを構築します。 そして最後に、(シェルの助けを借りて) ブラウザーを起動し、URL に移動します。

C++/WinRT に移植されたメソッドを次に示します (やはり、わかりやすくするために展開されています)。その後で、詳しく説明します。

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

いつもと同じように、イベント ハンドラーは `public` にします。 *sender* オブジェクトで [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 関数を使用して、それを **HyperlinkButton** に変換します。 C++/WinRT では、**Tag** プロパティは [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) です ([**Object**](/dotnet/api/system.object) と同等)。 ただし、**IInspectable** には **Tostring** はありません。 代わりに、**IInspectable** をスカラー値 (この場合は文字列) にボックス化解除する必要があります。 ここでも、ボックス化とボックス化解除の詳細については、「[IInspectable へのスカラー値のボックス化とボックス化解除](./boxing.md)」を参照してください。

最後の 2 行では前に見た移植パターンが繰り返されており、C# バージョンがほぼ反映されています。

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

このメソッドの移植に関しては、新しいことは何もありません。 ダウンロードした [Clipboard サンプル](/samples/microsoft/windows-universal-samples/clipboard/) ZIP に含まれる C# および C++/WinRT バージョンのソース コードを比較できます。

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** と **OnWindowActivated**

ここまで、これら 2 つのイベント ハンドラーに対しては空のスタブしかありません。 しかし、移植するのは簡単であり、新しく説明することは何もありません。

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

これは、C# の **MainPage** クラスに属しているもう 1 つのプライベート イベント ハンドラーであり、`MainPage.xaml.cs` において定義されています。 C++/WinRT では、パブリックにして、`MainPage.h` と `MainPage.cpp` で実装します。

このメソッドでは、プライベート ブール型フィールドである **MainPage::navigating** を `false` に初期化する必要があります。 また、`MainPage.xaml` に *ScenarioFrame* という名前の **Frame** が必要になります。 ただし、それらの詳細を除けば、このメソッドの移植に関して新しい手法はありません。

#### <a name="updatestatus"></a>**UpdateStatus**

**MainPage.UpdateStatus** については、ここまでスタブしかありません。 やはり、その実装の移植は、ほとんどが既出のものです。 1 つ注意の必要な新しい点は、C# では **string** と **String.Empty** を比較できますが、C++/WinRT では代わりに [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function) 関数を呼び出します。 もう 1 つ、C# の `null` に相当する標準の C++ は `nullptr` です。

移植の残りの部分は、既に説明した手法で実行できます。 このメソッドの移植されたバージョンをコンパイルする前に実行する必要があることの一覧を次に示します。

- `MainPage.xaml` に対して、*StatusBorder* という名前の **Border** を追加します。
- `MainPage.xaml` に対して、*StatusBlock* という名前の **TextBlock** を追加します。
- `MainPage.xaml` に対して、*StatusPanel* という名前の **StackPanel** を追加します。
- `pch.h` に対して、`#include "winrt/Windows.UI.Xaml.Media.h"` を追加します。
- `pch.h` に対して、`#include "winrt/Windows.UI.Xaml.Automation.Peers.h"` を追加します。
- `MainPage.cpp` に対して、`using namespace winrt::Windows::UI::Xaml::Media;` を追加します。
- `MainPage.cpp` に対して、`using namespace winrt::Windows::UI::Xaml::Automation::Peers;` を追加します。

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>**MainPage** の移植を完了するために必要な XAML とスタイルをコピーする

XAML の理想的なケースは、C# プロジェクトと C++/WinRT プロジェクトで "*同じ*" XAML マークアップを使用できる場合です。 そして、Clipboard サンプルはそのようなケースの 1 つです。

Clipboard サンプルの `Styles.xaml` ファイルに含まれる XAML の **ResourceDictionary** というスタイルが、アプリケーションの UI 全体のボタン、メニュー、その他の UI 要素に適用されます。 `Styles.xaml` ページは `App.xaml` にマージされます。 そして、UI に対しては標準の `MainPage.xaml` 開始ポイントがあります。これについては、既に簡単に説明しました。 C++/WinRT バージョンのプロジェクトでは、それら 3 つの `.xaml` ファイルを、変更なしに再利用できます。

資産ファイルと同様に、アプリケーションの複数のバージョンから、同じ共有 XAML ファイルを参照できます。 このチュートリアルでは、わかりやすくするためだけに、ファイルを C++/WinRT プロジェクトにコピーして、そのように追加します。

`\Clipboard_sample\SharedContent\xaml` フォルダーに移動し、`App.xaml` と `MainPage.xaml` を選択してコピーした後、それら 2 つのファイルを C++/WinRT プロジェクトの `\Clipboard\Clipboard` フォルダーに貼り付けます。メッセージが表示されたらファイルの置換を選択します。

C++/WinRT プロジェクトのプロジェクト ノードのすぐ下に、`Styles` という名前の新しいフォルダーを追加します。 `\Clipboard_sample\SharedContent\xaml` フォルダーに移動し、`Styles.xaml` を選択してコピーし、C++/WinRT プロジェクトの `\Clipboard\Clipboard\Styles` フォルダーにそれを貼り付けます。 (ソリューション エクスプローラーの C++/WinRT プロジェクトで) `Styles` フォルダーを右クリックし、 **[追加]**  >  **[既存の項目...]** を選択して、`\Clipboard\Clipboard\Styles` に移動します。 ファイル ピッカーで `Styles` を選択し、 **[追加]** をクリックします。

これで **MainPage** の移植が完了しました。手順に従って作業を行った場合、C++/WinRT プロジェクトをビルドして実行できます。

## <a name="consolidate-your-idl-files"></a>`.idl` ファイルを統合する

Clipboard サンプルには、UI に対する標準の `MainPage.xaml` 開始ポイントに加えて、他に 5 つのシナリオ固有の XAML ページと、対応する分離コード ファイルがあります。 プロジェクトの C++/WinRT バージョンでは、これらすべてのページの実際の XAML マークアップを、変更しないで再利用します。 ここでは、以下のいくつかの大きなセクションでは、分離コードを移植する方法について説明します。 ただし、その前に、IDL について説明しましょう。

ランタイム クラスを 1 つの IDL ファイルにまとめるとメリットがあります (「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」を参照)。 そこで次に、`CopyFiles.idl`、`CopyImage.idl`、`CopyText.idl`、`HistoryAndRoaming.idl`、`OtherScenarios.idl` の内容を、その IDL を `Project.idl` という名前の 1 つのファイルに移動することによって (そして、元のファイルを削除して) 統合します。

それを行う間に、それらの 5 つの XAML ページ型から自動生成されたダミー プロパティ (`Int32 MyProperty;` とその実装) も削除してみましょう。

最初に、新しい **Midl ファイル (.idl)** 項目を C++/WinRT プロジェクトに追加します。 これに `Project.idl` という名前をつけます。 `Project.idl` の内容全体を次のコードで置き換えます。

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

ご覧のように、これは個々の `.idl` ファイルの内容のコピーを 1 つの名前空間に収めて、各ランタイム クラスから `MyProperty` を削除したものです。

Visual Studio のソリューション エクスプローラーで、元のすべての IDL ファイル (`CopyFiles.idl`、`CopyImage.idl`、`CopyText.idl`、`HistoryAndRoaming.idl`、`OtherScenarios.idl`) を複数選択し、 **[編集]**  >  **[削除]** を選択します (ダイアログで **[削除]** を選択)。

最後に、`MyProperty` の削除を完了するため、5 つの各 XAML ページ型に対する `.h` および `.cpp` ファイルで、`int32_t MyProperty()` アクセサー関数と `void MyProperty(int32_t)` ミューテーター関数の宣言と定義を削除します。

ちなみに、XAML ファイルの名前は、それが表すクラスの名前と一致させることをお勧めします。 たとえば、XAML マークアップ ファイルに `x:Class="MyNamespace.MyPage"` がある場合、そのファイルに `MyPage.xaml` という名前を付ける必要があります。 これは技術要件ではありませんが、同じ成果物にさまざまな名前を付けて苦労することがなくなるため、プロジェクトの理解、保守、操作が容易になります。

## <a name="copyfiles"></a>**CopyFiles**

C# プロジェクトでは、**CopyFiles** XAML ページ型は、`CopyFiles.xaml` および `CopyFiles.xaml.cs` ソース コード ファイルで実装されています。 **CopyFiles** の各メンバーを順番に見ていきましょう。

### <a name="rootpage"></a>**rootPage**

これはプライベート フィールドです。

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

C++/WinRT では、次のように定義し、初期化することができます。

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

やはり (**MainPage::current** の場合と同様に)、**CopyFiles::rootPage** は **SDKTemplate::MainPage** 型として宣言されています。これは、投影型であり、実装型ではありません。

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (コンストラクター)

C++/WinRT プロジェクトの **CopyFiles** 型には、必要なコードが含まれるコンストラクターが既に存在します (**InitializeComponent** を呼び出すだけです)。

### <a name="copybutton_click"></a>**CopyButton_Click**

C# の **CopyButton_Click** メソッドはイベント ハンドラーであり、シグネチャの `async` キーワードから、メソッドが非同期で動作することがわかります。 C++/WinRT では、非同期メソッドを "*コルーチン*" として実装します。 C++/WinRT での同時実行の概要と、"*コルーチン*" の説明については、「[C++/WinRT を使用した同時実行操作と非同期操作](./concurrency.md)」を参照してください。

コルーチンが完了した後で他の作業をスケジュールするのが一般的であり、そのような場合は、コルーチンで待機可能な非同期オブジェクト型を返し、必要に応じて進行状況を報告します。 ただし、これらの考慮事項は、通常、イベント ハンドラーには適用されません。 したがって、非同期操作を実行するイベント ハンドラーがある場合は、**winrt::fire_and_forget** を返すコルーチンとして実装できます。 詳細については、「[ファイア アンド フォーゲット](./concurrency-2.md#fire-and-forget)」を参照してください。

ただし、ファイア アンド フォーゲット コルーチンのアイデアは、その完了を気にする必要がなく、処理はバックグラウンドで続行されている (または、中断されて再開を待機している) ということです。 C# の実装からは、**CopyButton_Click** が `this` ポインターに依存していることがわかります (インスタンス データ メンバー `rootPage` にアクセスします)。 したがって、`this` ポインター (**CopyFiles** オブジェクトへのポインター) が **CopyButton_Click** コルーチンより長く存在していることを確認する必要があります。 このサンプル アプリケーションのような、ユーザーが UI ページ間を移動する状況では、それらのページの有効期間を直接制御することはできません。 **CopyButton_Click** がバックグラウンド スレッドでまだ実行している間に、**CopyFiles** ページを (他の場所に移動することによって) 破棄した場合、`rootPage` へのアクセスが安全ではなくなります。 コルーチンを正しくするには、`this` ポインターへの強参照を取得し、コルーチンが継続している間、その参照を保持する必要があります。 詳しくは、「[C++/WinRT の強参照と弱参照](./weak-references.md)」をご覧ください。

サンプルの C++/WinRT バージョンを見ると、**CopyFiles::CopyButton_Click** において、それがスタックの単純な宣言を使用して実行されていることがわかります。

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

移植されたコードの他の注目すべき点を見てみましょう。

コードでは、[**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) オブジェクトがインスタンス化されており、その 2 行後で、オブジェクトの [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) プロパティにアクセスしています。 そのプロパティの戻り値の型では、文字列の **IVector** が実装されています。 そして、その **IVector** では、[IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall) メソッドが呼び出されています。 興味深いのは、そのメソッドに渡されている値であり、そこでは配列が想定されています。 そのコード行を次に示します。

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

渡している値 (`{ L"*" }`) は、C++ の標準の "*初期化子リスト*" です。 この例ではそれには 1 つのオブジェクトが含まれていますが、初期化子リストには任意の数のコンマ区切りオブジェクトを含めることができます。 このようなメソッドに初期化子リストを渡すことができる C++/WinRT の便利な機能については、「[標準的な初期化子リスト](./std-cpp-data-types.md#standard-initializer-lists)」で説明されています。

C# の `await` キーワードは、C++/WinRT では `co_await` に移植します。 コードの例を次に示します。

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

次に、この C# コード行について考えます。

```csharp
dataPackage.SetStorageItems(storageItems);
```

C# では、*storageItems* によって表される **IReadOnlyList<StorageFile>** を、[**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems) で必要な **IEnumerable<IStorageItem>** に、暗黙的に変換できます。 しかし、C++/WinRT では、**IVectorView<StorageFile>** から **IIterable<IStorageItem>** に明示的に変換する必要があります。 そこで、[**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 関数の動作に関するもう 1 つの例を示します。

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

C# では `null` キーワードを使用する場所で (例: `Clipboard.SetContentWithOptions(dataPackage, null)`)、C++/WinRT では `nullptr` を使用します (例: `Clipboard::SetContentWithOptions(dataPackage, nullptr)`)。

### <a name="pastebutton_click"></a>**PasteButton_Click**

これは、ファイア アンド フォーゲット コルーチンの形式になっているもう 1 つのイベント ハンドラーです。 移植されたコードの注目すべき点を見てみましょう。

サンプルの C# バージョンでは、`catch (Exception ex)` で例外をキャッチしています。 移植された C++/WinRT のコードでは、`catch (winrt::hresult_error const& ex)` という式になっています。 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) およびその使用方法の詳細については、「[C++/WinRT でのエラー処理](./error-handling.md)」を参照してください。

C# オブジェクトが `null` かどうかをテストする例は、`if (storageItems != null)` です。 C++/WinRT では、`bool` への変換演算子を使用できます。そこでは、内部的に `nullptr` のテストが行われます。

サンプルの移植された C++/WinRT バージョンのコード フラグメントを少し簡略化したバージョンを次に示します。

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

このような **winrt::hstring** からの **std::wstring_view** の構築は、[**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) 関数の呼び出し (**winrt::hstring** を C スタイルの文字列に変換するため) に対する代替手段を示します。 この代替手段は、**hstring** の [**std::wstring_view** への変換演算子](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view)によって機能します。

次のような C# のフラグメントについて考えます。

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

C# の `as` キーワードを C++/WinRT に移植するため、これまで [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 関数が使用されているのを 2 回見ました。 その関数では、型変換が失敗すると例外がスローされます。 しかし、変換が失敗した場合に `nullptr` を返す場合は (コード内でその状態を処理できるようにするため)、代わりに [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 関数を使用します。

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>**CopyFiles** の移植を完了するために必要な XAML をコピーする

これで、C#プロジェクトの `CopyFiles.xaml` ファイルの内容全体を選択し、それを C++/WinRT プロジェクトの `CopyFiles.xaml` ファイルに貼り付けることができます (C++/WinRT プロジェクトでのそのファイルの既存の内容を置き換えます)。

最後に、対応する XAML マークアップを上書きしたので、`CopyFiles.h` と `.cpp` を編集して、ダミーの **ClickHandler** 関数を削除します。

これで **CopyFiles** の移植が完了しました。手順に従って作業を行った場合、C++/WinRT プロジェクトをビルドして実行でき、**CopyFiles** シナリオが機能するようになります。

## <a name="copyimage"></a>**CopyImage**

**CopyImage** XAML ページ型を移植するには、**CopyFiles** と同じ手順に従います。 **CopyImage** の移植には、C# の "[*using ステートメント*](/dotnet/csharp/language-reference/keywords/using-statement)" が使用されている箇所があります。これにより、[**IDisposable**](/dotnet/api/system.idisposable) インターフェイスを実装しているオブジェクトが、正しく破棄されるようになります。

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

C++/WinRT でこれに相当するインターフェイスは、[**IClosable**](/uwp/api/windows.foundation.iclosable) とその単一の **Close** メソッドです。 上記の C# コードに相当する C++/WinRT を次に示します。

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

C++/WinRT のオブジェクトでは、主として確定的な終了処理を持たない言語のメリットのために、**IClosable** が実装されています。 C++/WinRT には確定的な終了処理があるため、C++/WinRT を記述するときは、**IClosable::Close** を呼び出す必要がないことがよくあります。 ただし、それを呼び出すことが適切な場合もあり、これはそのような場合の 1 つです。 ここで、*imageStream* 識別子は、基になる Windows ランタイム オブジェクト (この場合は、[**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype) を実装するオブジェクト) に対する参照カウント ラッパーです。 *imageStream* (そのデストラクター) のファイナライザーが外側のスコープの最後 (中かっこ) に実行されることは確認できますが、そのファイナライザーで **Close** が呼び出されることは確実ではありません。 これは、*imageStream* を他の API に渡しても、基になる Windows ランタイム オブジェクトの参照カウントに加算される可能性があるためです。 そのため、このような場合は、**Close** を明示的に呼び出すことをお勧めします。 詳しくは、「[使用するランタイム クラスで IClosable::Close を読み出す必要性](./faq.md#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume)」をご覧ください。

次に、C# の式 `(uint)(imageDecoder.OrientedPixelWidth * 0.5)` について考えます。これは、**OnDeferredImageRequestedHandler** イベント ハンドラーにあります。 その式では、`uint` と `double` を乗算し、`double` を得ています。 その後、それを `uint` にキャストします。 C++/WinRT では、同様の C スタイルのキャスト (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`) を使用することも "*できます*" が、意図するキャストの種類を正確に明示することをお勧めします。この例では `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)` を使用します。

C# バージョンの **CopyImage.OnDeferredImageRequestedHandler** には、`finally` 句はありますが、`catch` 句はありません。 C++/WinRT バージョンではもう少し進めて、遅延レンダリングが成功したかどうかを報告できるように `catch` 句を実装しました。

この XAML ページの残りの部分の移植については、新たに説明することはありません。 ダミーの **ClickHandler** 関数を削除することを忘れないでください。 また、**CopyFiles** と同様に、移植の最後のステップでは、`CopyImage.xaml` の内容全体を選択し、それを C++/WinRT プロジェクトの同じファイルに貼り付けます。

## <a name="copytext"></a>**CopyText**

既に説明した手法を使用して、`CopyText.xaml` と `CopyText.xaml.cs` を移植することができます。

## <a name="historyandroaming"></a>**HistoryAndRoaming**

**HistoryAndRoaming** XAML ページ型の移植では、いくつか重要な点が発生します。

まず、C# のソース コードを見て、**OnNavigatedTo** から **OnHistoryEnabledChanged** イベント ハンドラーを経て最後に非同期関数 **CheckHistoryAndRoaming** (これは待機しないため、基本的にファイア アンド フォーゲットです) に至る、制御の流れを辿ってください。 **CheckHistoryAndRoaming** は非同期であるため、C++/WinRT では `this` ポインターの有効期間について注意する必要があります。 `HistoryAndRoaming.cpp` ソース コード ファイルの実装を見ると、結果を確認できます。 まず、**Clipboard::HistoryEnabledChanged** イベントと **Clipboard::RoamingEnabledChanged** イベントにデリゲートをアタッチするときは、**HistoryAndRoaming** ページ オブジェクトへの弱参照のみを取得します。 それを行うには、`this` ポインターへの依存関係ではなく、[**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) から返された値に対する依存関係でデリゲートを作成します。 つまり、最終的に非同期コードを呼び出すデリゲート自体では、**HistoryAndRoaming** ページから余所に移動するときに、ページは保持されません。

2 番目として、最終的にファイア アンド フォーゲットの **CheckHistoryAndRoaming** コルーチンに達したときに、最初に行うことは、少なくともコルーチンが最後に完了するまで **HistoryAndRoaming** ページが確実に存在するように、`this` への強参照を取得することです。 ここで説明した両方の点の詳細については、「[C++/WinRT の強参照と弱参照](./weak-references.md)」を参照してください。

**CheckHistoryAndRoaming** の移植には、もう 1 つ興味深い点があります。 それには UI を更新するためのコードが含まれるので、それがメイン UI スレッドで実行されていることを確認する必要があります。 最初にイベント ハンドラーを呼び出すスレッドは、メイン UI スレッドです。 ただし、通常、非同期メソッドは任意のスレッドで実行または再開できます。 C# でそれを解決するには、[**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) を呼び出し、ラムダ関数内から UI を更新します。 C++/WinRT では、[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 関数を `this` ポインターの [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) と共に使用して、コルーチンを中断し、メイン UI スレッドですぐに再開することができます。

関連する式は `co_await winrt::resume_foreground(Dispatcher());` です。 または、わかりにくくなりますが、単に `co_await Dispatcher();` と表現することもできます。 より短いバージョンは、C++/WinRT によって提供される変換演算子によって実現されます。

この XAML ページの残りの部分の移植については、新たに説明することはありません。 ダミーの **ClickHandler** 関数を削除し、XAML マークアップを上書きコピーすることを忘れないでください。

## <a name="otherscenarios"></a>**OtherScenarios**

既に説明した手法を使用して、`OtherScenarios.xaml` と `OtherScenarios.xaml.cs` を移植することができます。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、移植に関する十分な情報と手法を説明したので、今度はご自分の C# アプリケーションを C++/WinRT に移植できるはずです。 復習するには、引き続き Cliboard サンプルの移植前 (C#) と移植後 (C++/WinRT) のバージョンのソース コードを並べて比較し、対応を確認することができます。

## <a name="related-topics"></a>関連トピック
* [C# から C++/WinRT への移行](./move-to-winrt-from-csharp.md)