---
title: Marble Maze サンプルの基礎
description: このドキュメントでは、Marble Maze プロジェクトの基本的な特性について説明します。たとえば、Windows ランタイム環境で Visual C++ をどのように使うか、どのように作られ、構成され、ビルドされるかなどです。
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, サンプル, DirectX, 基礎
ms.localizationpriority: medium
ms.openlocfilehash: 714641be3c5ae6e202f0d6b7da5a1a4c1fc93d86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165326"
---
# <a name="marble-maze-sample-fundamentals"></a>Marble Maze サンプルの基礎




このトピックでは、 &mdash; Windows ランタイム環境での Visual C++ の使用方法、作成方法と構成方法、ビルド方法など、大理石の迷路プロジェクトの基本的な特性について説明します。 また、コードで使われるいくつかの規則についても説明します。

> [!NOTE]
> このドキュメントに対応するサンプル コードは、[DirectX Marble Maze ゲームのサンプルに関するページ](https://github.com/microsoft/Windows-appsample-marble-maze)にあります。

このドキュメントでは、ユニバーサル Windows プラットフォーム (UWP) ゲームの計画と開発の際に重要となるいくつかの事柄について説明します。取り上げる内容は次のとおりです。

-   Directx UWP ゲームを作成するには、Visual Studio の **directx 11 アプリ (ユニバーサル Windows-C++/cx)** テンプレートを使用します。
-   より新しい、オブジェクト指向に沿った方法で UWP アプリを開発できるようなクラスとインターフェイスが、Windows ランタイムには用意されています。
-   オブジェクト参照を hat (^) Windows ランタイム記号と共に使用して、COM オブジェクトの有効期間を管理するための、 [Microsoft:: WRL:: ComPtr](/cpp/windows/comptr-class) 、および [std:: shared \_ ptr](/cpp/standard-library/shared-ptr-class) または [std:: unique \_ ptr](/cpp/standard-library/unique-ptr-class) を使用して、ヒープに割り当てられた他のすべての C++ オブジェクトの有効期間を管理します。
-   ほとんどの場合、予期しないエラーを処理するには、結果コードではなく例外処理を使います。
-   [SAL 注釈](/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects)をコード分析ツールと共に使用すると、アプリのエラーを検出するのに役立ちます。

## <a name="creating-the-visual-studio-project"></a>Visual Studio プロジェクトの作成


サンプルをダウンロードおよび展開してある場合は、Visual Studio で **MarbleMaze_VS2017.sln** ファイル (**C++** フォルダー内) を開くと、コードが表示されます。

Marble Maze の Visual Studio プロジェクトを作ったときには、既にあるプロジェクトを利用しました。 ただし、DirectX UWP ゲームで必要とされる基本的な機能を提供する既存のプロジェクトがない場合は、Visual Studio **directx 11 アプリ (ユニバーサル Windows-c + +/cx)** テンプレートに基づいてプロジェクトを作成することをお勧めします。これは、基本的な動作する3d アプリケーションを提供するためです。 そのためには、次の手順に従います。

1. Visual Studio 2019 で、[**ファイル > 新しい > プロジェクト**] を選択します。

2. [ **新しいプロジェクトの作成** ] ウィンドウで、[ **DirectX 11 アプリ (ユニバーサル Windows-C++/cx)**] を選択します。 このオプションが表示されない場合は、必要なコンポーネントがインストールされていない可能性があり &mdash; ます。追加のコンポーネントをインストールする方法については、「 [ワークロードとコンポーネントを追加または削除して Visual Studio 2019 を変更](/visualstudio/install/modify-visual-studio) する」を参照してください。

![[新しいプロジェクト]](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. [ **次へ**] を選択し、 **プロジェクト名**、格納するファイルの **場所** 、および **ソリューション名**を入力して、[ **作成**] を選択します。



**DirectX 11 アプリ (ユニバーサル Windows-c + +/cx)** テンプレートの重要なプロジェクト設定の1つに、 **/ZW**オプションがあります。これにより、プログラムで Windows ランタイム言語拡張機能を使用できるようになります。 Visual Studio テンプレートを使う場合、このオプションは既定で有効になっています。 Visual Studio でコンパイラ オプションを設定する方法について詳しくは、「[コンパイラ オプションの設定](/cpp/build/reference/setting-compiler-options)」をご覧ください。

> **注意**    **/ZW**オプションは、 **/clr**などのオプションと互換性がありません。 **/clr** の場合は、同じ Visual C++ プロジェクトで .NET Framework と Windows ランタイムの両方をターゲットにすることはできないことを意味します。

 

Microsoft Store から取得したすべての UWP アプリは、アプリケーションパッケージの形式になります。 アプリ パッケージには、アプリについての情報が記載されたパッケージ マニフェストが含まれています。 たとえば、アプリの機能 (つまり、保護されたシステム リソースやユーザー データへの必要なアクセス) を指定できます。 アプリで特定の機能が必須であると決めた場合は、パッケージ マニフェストを使って、必要な機能を宣言します。 マニフェストでは、サポートされているデバイスの回転、タイル画像、スプラッシュ画面など、プロジェクト プロパティを指定することもできます。 プロジェクトで **Package.appxmanifest** を開いて、マニフェストを編集することができます。 アプリ パッケージについて詳しくは、「[アプリのパッケージ化](../packaging/index.md)」をご覧ください。

##  <a name="building-deploying-and-running-the-game"></a>ゲームのビルド、展開、実行

Visual Studio の上部の、緑色の再生ボタンの左のドロップダウン リストで、展開構成を選択します。 デバイスのアーキテクチャ (32 ビットでは **x86**、64 ビットでは **x64**) による **ローカル コンピューター** をターゲットとする**デバッグ**として設定することをお勧めします。 **リモート コンピューター**でテストすることも、または USB で接続された**デバイス**を対象とすることも、可能です。 次に、緑色の再生ボタンをクリックしてビルドし、デバイスに展開します。

![デバッグ、x64、ローカル コンピューター](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>ゲームの制御

Marble Maze の制御には、タッチ、加速度計、Xbox One コントローラー、マウスを使うことができます。

-   アクティブなメニュー項目を変更するには、コントローラーの方向パッドを使います。
-   メニュー項目を選択するには、タッチ、コントローラーの A ボタンまたはスタート ボタン、マウスを使います。
-   迷路を傾けるには、タッチ、加速度計、左スティック、マウスを使います。
-   ハイ スコア表などのメニューを閉じるには、タッチ、コントローラーの A ボタンまたはスタート ボタン、マウスを使います。
-   ゲームを一時停止または再開するには、コントローラーのスタート ボタンまたはキーボードの P キーを使います。
-   ゲームを再開始するには、コントローラーの [戻る] ボタンやキーボードの Home キーを使います。
-   ハイ スコア表が表示されている場合に、すべてのスコアをクリアするには、コントローラーの [戻る] ボタンまたはキーボードの Home キーを使います。

##  <a name="code-conventions"></a>コードの規則


Windows ランタイムは、特別なアプリケーション環境だけで実行される UWP アプリの作成に使うプログラミング インターフェイスです。 このようなアプリは、承認された機能、データ型、およびデバイスを使用し、Microsoft Store から配布されます。 Windows ランタイムの最も基本となる部分を構成しているのは、アプリケーション バイナリ インターフェイス (ABI) です。 ABI は、JavaScript、.NET 言語、Visual C++ など、複数のプログラミング言語から Windows ランタイム API にアクセスできるようにするための基礎となるバイナリ コントラクトです。

Windows ランタイム API を JavaScript や .NET から呼び出すには、各言語環境に固有のプロジェクションが必要となります。 Windows ランタイム API を JavaScript または .NET から呼び出すとき、実際にはプロジェクションを呼び出し、そこからさらに、基になる ABI 関数を呼び出すことになります。 ABI 関数は C++ から直接呼び出すことができますが、Microsoft は、C++ 用のプロジェクションも併せて提供しています。そのようにすることで、Windows ランタイム API の扱いがシンプルになると共に、高いパフォーマンスを維持できるためです。 また、実際に Windows ランタイムのプロジェクションをサポートする、Visual C++ の言語拡張機能も Microsoft から提供されています。 こうした言語拡張機能の多くは、C++/CLI 言語の構文と似ています。 ただし、ネイティブ アプリはこの構文を使って、共通言語ランタイム (CLR) をターゲットにするのではなく、Windows ランタイムをターゲットにします。 オブジェクト参照、またはハット (^) 修飾子は、この新しい構文の重要な要素です。これによって、参照カウントに基づくランタイム オブジェクトの自動削除が可能になるためです。 Windows ランタイム オブジェクトの有効期間を管理するために [AddRef](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) や [Release](/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) などのメソッドを呼び出さなくても、他のコンポーネントがオブジェクトを参照していないときに (たとえばオブジェクトのスコープが終わったり、すべての参照が **nullptr** に設定されたりしたときに)、ランタイムがオブジェクトを削除します。 Visual C++ を使った UWP アプリの作成に関して、もう 1 つの重要な要素は **ref new** キーワードです。 参照カウントで管理される Windows ランタイム オブジェクトを作成するには、**new** ではなく **ref new** を使います。 詳しくは、「[型システム (C++/CX)](/cpp/cppcx/type-system-c-cx)」をご覧ください。

> [!IMPORTANT]
> **^** Windows ランタイムオブジェクトを作成する場合、または Windows ランタイムコンポーネントを作成する場合にのみ、と**ref new**を使用する必要があります。 Windows ランタイムを使わないコア アプリケーション コードを作成する際は、標準の C++ 構文を使うことができます。

大理石迷路は、 **^** **Microsoft:: wrl:: comptr** と共に使用して、ヒープに割り当てられたオブジェクトを管理し、メモリリークを最小化します。 (DirectX を使用する場合など) COM 変数の有効期間を管理するには、Windows ランタイム ^ を **使用し、** ヒープに割り当てられた他のすべての C++ オブジェクトの有効期間を管理するには、 **std:: shared \_ ptr** または **std:: unique \_ ptr** を使用することをお勧めします。

 

C++ UWP アプリで使える言語拡張機能について詳しくは、「[Visual C++ 言語のリファレンス (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx)」をご覧ください。

###  <a name="error-handling"></a>エラー処理

Marble Maze では、予期しないエラーに対応する主な方法として例外処理を使います。 ゲーム コードでは従来、エラーを示すためにログやエラー コード (**HRESULT** 値など) を使ってきましたが、例外処理には 2 つの主な利点があります。 1 つ目は、コードが読みやすく、保守しやすいことです。 コードの観点からは、例外処理はエラー処理ルーティンに効率よくエラーを伝達することができます。 エラー コードを使った場合は通常、個々の関数で明示的にエラーを伝達する必要があります。 2 つ目の利点は、例外が発生したところで中断するように Visual Studio デバッガーを構成すれば、エラーが発生したときに、その位置やコンテキストですぐに止めることができることです。 例外処理は、Windows ランタイムでも広く使われています。 したがって、自分のコード内で例外処理を使うことにより、あらゆるエラー処理を 1 つのモデルに統合することができます。

エラー処理モデルでは、次の規則を使うことをお勧めします。

-   例外は、予期しないエラーを知らせるために使います。
-   コードのフローを制御するためには、例外を使わないでください。
-   キャッチする例外は安全に処理、回復できるものだけにしてください。 それ以外の例外はキャッチせず、アプリを強制終了させます。
-   **HRESULT** を返す DirectX ルーチンを呼び出す場合は、**DX::ThrowIfFailed** 関数を使います。 この関数は、[DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h) 内で定義されています。 **HRESULT** がエラー コードであれば、**ThrowIfFailed** から例外がスローされます。 たとえば、 **E \_ ポインター** を使用すると、 **ThrowIfFailed** は [Platform:: NullReferenceException](/cpp/cppcx/platform-nullreferenceexception-class)をスローします。

    **ThrowIfFailed** を使うときは、次の例に示すように DirectX 呼び出しを別の行に記述して、コードが読みやすくなるようにします。

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   予期しないエラーのために **HRESULT** を使うことは避けるようにお勧めしますが、より重要なのは、コードのフローを制御するために例外処理を使わないようにすることです。 そのため、コードのフローを制御するために必要な場合は、**HRESULT** 戻り値を使う方が適切です。

###  <a name="sal-annotations"></a>SAL 注釈

アプリのエラーの発見に役立てるために、コード分析ツールと共に SAL 注釈を使います。

Microsoft Source-code Annotation Language (SAL) を使うと、関数がパラメーターをどのように使うかを説明する注を付けることができます。 SAL 注釈は、戻り値についても説明します。 SAL 注釈を C/C++ コード分析ツールと共に使うと、C や C++ ソース コードの潜在的な不具合を検出できます。 このツールで報告される一般的なコーディング エラーとして、バッファー オーバーラン、初期化されていないメモリ、Null ポインターの逆参照、メモリ リーク、リソース リークなどがあります。

[BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h) で宣言されている **BasicLoader::LoadMesh** メソッドを検討してください。 このメソッドは、`_In_` を使って *filename* が入力パラメーターである (つまり、読み取られるだけである) ことを指定します。また、`_Out_` を使って *vertexBuffer* と *indexBuffer* が出力パラメーターである (つまり、書き込まれるだけである) ことを指定します。さらに、`_Out_opt_` を使って *vertexCount* と *indexCount* が省略可能な出力パラメーターである (書き込まれる場合がある) ことを指定します。 *vertexCount* と *indexCount* は省略可能な出力パラメーターであるため、**nullptr** にすることができます。 C/C++ コード分析ツールは、このメソッドの呼び出しを調べて、渡されるパラメーターが条件を満たしていることを確認します。

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

アプリのコード分析を行うには、メニュー バーで **[ビルド] > [ソリューションでコード分析を実行]** の順に選択します。 コード分析について詳しくは、「[コード分析による C/C++ コード品質の分析](/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis)」をご覧ください。

利用できる注釈の完全なリストは、sal.h で定義されています。 詳しくは、「[SAL 注釈](/cpp/c-runtime-library/sal-annotations)」をご覧ください。

## <a name="next-steps"></a>次の手順


Marble Maze アプリケーション コードの構造と、DirectX UWP アプリの構造と従来のデスクトップ アプリケーションの構造の違いについての情報を、「[Marble Maze のアプリケーション構造](marble-maze-application-structure.md)」で読みます。

## <a name="related-topics"></a>関連トピック


* [Marble Maze のアプリケーション構造](marble-maze-application-structure.md)
* [Marble Maze、C++ と DirectX での UWP ゲームの開発](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 