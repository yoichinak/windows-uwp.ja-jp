---
description: これは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) から [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) への段階的な移植に関する高度なトピックです。 ここでは、並列パターン ライブラリ (PPL) のタスクとコルーチンを同じプロジェクトに並べて配置できる方法について説明します。
title: 非同期性、および C++/WinRT と C++/CX 間の相互運用
ms.date: 08/06/2020
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、移植、移行、相互運用、C++/CX、PPL、タスク、コルーチン
ms.localizationpriority: medium
ms.openlocfilehash: daa263836e9024a0aaa55b239b1a0db9437f1cd5
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2020
ms.locfileid: "88181074"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>非同期性、および C++/WinRT と C++/CX 間の相互運用

> [!TIP]
> このトピックは最初からお読みになることをお勧めしますが、「[C++/CX 非同期から C++/WinRT への移植の概要](#overview-of-porting-ccx-async-to-cwinrt)」 セクションにある相互運用手法の概要に直接進んでも構いません。

これは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) から [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) への段階的な移植に関する高度なトピックです。 このトピックでは、「[C++/WinRT と C++/CX 間の相互運用](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)」のトピックが終了したところから説明します。

コードベースのサイズまたは複雑さのためにプロジェクトを段階的に移植する必要がある場合は、しばらくの間、C++/CX と C++/WinRT のコードが同じプロジェクトに共存するような移植プロセスが必要になります。 非同期コードを使用している場合は、ソース コードを段階的に移植する際に、並列パターン ライブラリ (PPL) タスク チェーンとコルーチンをプロジェクトに共存させることが必要な場合があります。 このトピックでは、非同期 C++/CX コードと非同期 C++/WinRT コードを相互運用する手法について説明します。 これらの手法は、個別に使用することも、組み合わせて使用することもできます。

このトピックを読む前に、「[C++/WinRT と C++/CX 間の相互運用](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)」を読むことをお勧めします。 このトピックでは、段階的な移植のためにプロジェクトを準備する方法について説明します。 また、C++/CX オブジェクトを C++/WinRT オブジェクトに (およびその逆に) 変換するために使用できる 2 つのヘルパー関数を紹介します。 非同期性に関するこのトピックでは、この情報に基づき、これらのヘルパー関数を使用します。

> [!NOTE]
> 段階的な移植にはいくつかの制限があります。 Windows ランタイム コンポーネント プロジェクトがある場合、段階的に移植することはできず、プロジェクトを 1 回のプロセスで移植する必要があります。 また、XAML プロジェクトの場合、XAML ページの種類は常に、すべて C++/WinRT であるか "*または*" すべて C++/CX であるかの "*いずれか*" である必要があります。 詳細については、「[C++/CX から C++/WinRT への移行](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)」を参照してください。

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>トピック全体が非同期コード相互運用に特化している理由

C++/CX から C++/WinRT への移植は、[並列パターン ライブラリ (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) タスクからコルーチンへの移行という 1 つの例外を除いて、一般的に簡単です。 モデルはそれぞれ異なります。 PPL タスクからコルーチンへの自然な 1 対 1 のマッピングはなく、すべてのケースで動作するコードを機械的に移植する簡単な方法もありません。

幸いなことに、タスクからコルーチンへの変換によって大幅な単純化が実現します。 また、開発チームは、非同期コードの移植というハードルを越えてしまえば、残りの移植作業の大部分は機械的なものになると、いつも報告しています。

多くの場合、アルゴリズムはもともと同期 API に適合するように作成されています。 そして、それがタスクや明示的な継続処理に変換された結果、基になるロジックが誤って難読化されることがよくありました。 たとえば、ループは再帰になり、if-else 分岐は入れ子になったタスクのツリー (チェーン) になり、共有変数は **shared_ptr** になります。 PPL ソース コードによくある不自然な構造を分解するには、最初に一歩引いて、元のコードの意図を理解する (つまり、元の同期バージョンを見つける) ことをお勧めします。 次に、適切な場所に `co_await` を挿入します。

そのため、移植を開始する非同期コードのバージョンが (C++/CX ではなく) C# である場合は、その方が時間がかからず、よりクリーンな移植ができます。 C# コードは `await` を使用します。 したがって、C# コードは基本的に、同期バージョンから始めて、適切な場所に `await` を挿入するという考え方に既に従っています。

## <a name="some-background-in-asynchronous-programming"></a>非同期プログラミングの背景

非同期プログラミングの概念と用語についてのしっかりとした参照の枠組みが用意できたので、次に Windows ランタイム非同期プログラミングの一般的な状況と、C++ の 2 つの言語プロジェクションがそれぞれ異なる方法でどのように階層化されているかについて、簡単に説明します。

使用するプロジェクトには、非同期的に動作するメソッドがあります。 多くの場合、他の作業を行う前に、現在の作業が完了するまで待つ必要があります。 非同期メソッドを待機できるように、そのメソッドは非同期操作オブジェクトを返します。 しかし、非同期に行われた作業の完了を待ちたくない、あるいは待つ必要がない場合もあります。 その場合、そのメソッドは非同期操作オブジェクトを返す必要はありません。 待機しない非同期メソッドは、*fire-and-forget* メソッドとして知られています。

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Windows ランタイム非同期オブジェクト (**IAsyncXxx**)

**Windows::Foundation** Windows ランタイムの名前空間には 4 種類の非同期操作オブジェクトが含まれます。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1)
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)

このトピックで、**IAsyncXxx** という簡略表記を使用した場合、これらの種類を集合的に参照しているか、あるいは、どれかを指定せずに 4 種類のいずれかを指しているかのいずれかです。

### <a name="ccx-async"></a>C++/CX 非同期

非同期 C++/CX コードでは、[並列パターン ライブラリ (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) タスクを使用します。 PPL タスクは [**concurrency::task**](/cpp/parallel/concrt/reference/task-class) クラスによって表されます。

通常、非同期 C++/CX メソッドは、ラムダ関数と [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) および [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then) を使用して、PPL タスクを連結します。 これらのラムダ関数はそれぞれタスクを返し、それが完了したときに値が生成され、それが次にタスクの "*継続処理*" のラムダに渡されます。

あるいは、**create_task** を呼び出してタスクを作成するのではなく、非同期 C++/CX メソッドは [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) を呼び出して **IAsyncXxx**\^ を作成できます。

したがって、非同期 C++/CX メソッドの戻り値の型は、PPL タスク、または **IAsyncXxx**\^ である場合があります。

どちらの場合も、メソッド自体は `return` キーワードを使用して非同期オブジェクトを返し、それが完了すると、呼び出し側が実際に必要とする値 (おそらく、ファイル、バイト配列、またはブール値) が生成されます。

> [!NOTE]
> 非同期 C++/CX メソッドが **IAsyncXxx**\^ を返す場合、**TResult** は Windows ランタイム型に制限されます。 たとえば、ブール値は Windows ランタイム型ですが、C++/CX 投影型 (たとえば、**Platform::Array<byte>** ^) はそうではありません。

### <a name="cwinrt-async"></a>C++/WinRT 非同期

C++/WinRT は、C++ コルーチンをプログラミング モデルに統合します。 コルーチンと `co_await` ステートメントは、結果を協調的に待機するための自然な方法を提供します。

それぞれの **IAsyncXxx** 型は、**winrt::Windows::Foundation** C++/WinRT の名前空間で対応する型に投影されます。 これらを **winrt::IAsyncXxx** と呼ぶことにします。 C++/WinRT コルーチンの戻り値の型は **winrt::IAsyncXxx** または [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) です。 また、コルーチンは、`return` キーワードを使用して非同期オブジェクトを返す代わりに、`co_return` キーワードを使用して、呼び出し元が実際に必要とする値 (おそらく、ファイル、バイト配列、またはブール値) を返します。

メソッドに少なくとも 1 つの `co_await` ステートメント (または少なくとも 1 つの `co_return` または `co_yield`) が含まれている場合、メソッドはその理由でコルーチンになります。

詳細については、「[C++/WinRT を使用した同時実行操作と非同期操作](/windows/uwp/cpp-and-winrt-apis/concurrency)」を参照してください。

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Direct3D ゲーム サンプル (**Simple3DGameDX**)

このトピックには、非同期コードを段階的に移植する方法を示すいくつかのチュートリアルが含まれています。 ケース スタディとして、[Direct3D ゲーム サンプル](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (**Simple3DGameDX** と呼ばれています) の C++/CX バージョンを使用します。 そのプロジェクトの元の C++/CX ソース コードを使用して、その非同期コードを C++/WinRT に段階的に移植する方法の例をいくつか紹介します。

- 上記のリンクから ZIP をダウンロードし、解凍します。
- Visual Studio で C++/CX プロジェクトを開きます (これは `cpp` という名前のフォルダーにあります)。
- その後、このプロジェクトに C++/WinRT サポートを追加する必要があります。 これを行うための手順については、「[C++/Cx プロジェクトを取得して C++/WinRT サポートを追加する](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx#taking-a-ccx-project-and-adding-cwinrt-support)」を参照してください。 `interop_helpers.h` ヘッダー ファイルをプロジェクトに追加する手順は特に重要です。このトピックではそれらのヘルパー関数に依存することになるためです。
- 最後に、`#include <pplawait.h>` を `pch.h` に追加します。 これにより、PPL のコルーチン サポートが提供されます (このサポートの詳細については次のセクションで説明します)。

ビルドすると **byte** があいまいであるというエラーが発生します。 これを解決する方法を次に示します。

- `BasicLoader.cpp` を開き、`using namespace std;` をコメントアウトします。
- 同じソース コード ファイルで、**shared_ptr** を **std::shared_ptr** として修飾する必要があります。 これは、そのファイルで検索と置換を使用して行うことができます。
- **vector** を **std::vector** として修飾し、**string** を **std::string** として修飾します。

これで、プロジェクトが再ビルドされ、C++/WinRT がサポートされ、**from_cx** および **to_cx** 相互運用ヘルパー関数が含まれるようになりました。

これで、このトピックのコード チュートリアルを進めるための **Simple3DGameDX** プロジェクトの準備が整いました。

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>C++/CX 非同期から C++/WinRT への移植の概要

簡単に述べると、移植の際に、PPL タスク チェーンを `co_await` の呼び出しに変更します。 PPL タスクからメソッドの戻り値を、C++/WinRT **IAsyncXxx** オブジェクトまたは **IAsyncXxx**\^ に変更します。 そして、**IAsyncXxx**\^ を C++/WinRT **IAsyncXxx** に変更します。

コルーチンは、`co_xxx` を呼び出す任意のメソッドであることを思い出してください。 C++/WinRT コルーチンは、`co_return` を使用してその値を返します。 PPL のコルーチン サポート (`pplawait.h` によって提供) のおかげで、`co_return` を使用して、コルーチンから PPL タスクを返すこともできます。 また、タスクと **IAsyncXxx** の両方を `co_await` することもできます。 ただし、`co_return` を使用して **IAsyncXxx**\^ を返すことはできません。 次の表では、さまざまな非同期手法間の相互運用に対するサポートについて説明します。

|Method|それを `co_await` できるか|それから `co_return` できるか|
|-|-|-|
|メソッドが task\<void\> を返す|はい|はい|
|メソッドが task\<T\> を返す|いいえ|はい|
|メソッドが IAsyncXxx\^ を返す|はい|いいえ。 ただし、`co_return` を使用するタスクに **create_async** をラップします。|
|メソッドが winrt::IAsyncXxx を返す|はい|はい|

この表を使用して、興味のある相互運用の手法に直接移動します。

|非同期相互運用の手法|このトピックのセクション|
|-|-|
|`co_await` を使用して fire-and-forget メソッド内から **task\<void\>** メソッドを待機します。|[fire-and-forget メソッド内で **task\<void\>** を待機する](#await-taskvoid-within-a-fire-and-forget-method)|
|`co_await` を使用して **task\<void\>** メソッド内から **task\<void\>** メソッドを待機します。|[**task\<void\>** メソッド内で **task\<void\>** を待機する](#await-taskvoid-within-a-taskvoid-method)|
|`co_await` を使用して **task\<T\>** メソッド内から **task\<void\>** メソッドを待機します。|[**task\<T\>** メソッド内で **task\<void\>** を待機する](#await-taskvoid-within-a-taskt-method)|
|`co_await` を使用して **IAsyncXxx**^ メソッドを待機します。|[**task** メソッドで **IAsyncXxx**^ を待機し、プロジェクトの残りの部分は変更しない](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|**task\<void\>** メソッド内で `co_return` を使用します。|[**task\<void\>** メソッド内で **task\<void\>** を待機する](#await-taskvoid-within-a-taskvoid-method)|
|**task\<T\>** メソッド内で `co_return` を使用します。|[**task** メソッドで **IAsyncXxx**^ を待機し、プロジェクトの残りの部分は変更しない](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|`co_return` を使用するタスクに **create_async** をラップします|[`co_return` を使用するタスクに **create_async** をラップする](#wrap-create_async-around-a-task-that-uses-co_return)|
|**concurrency::wait** を移植します。|[**concurrency::wait** を `co_await winrt::resume_after` に移植する](#port-concurrencywait-to-co_await-winrtresume_after)|
|**task\<void\>** ではなく **winrt::IAsyncXxx** を返します。|[**task\<void\>** の戻り値の型を **winrt::IAsyncXxx** に移植する](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|

ここでは、いくつかのサポートを示す簡単なコード例を紹介します。

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that.
    return concurrency::create_async([=]() -> task<bool> {
        co_return true;
    });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

これらの優れた相互運用のオプションがあっても、段階的な移植は、システムの他の部分に影響を与えないような、外科手術的に行える変更を選択できるかどうかにかかっています。 無作為の未解決事項を解決しようとしてプロジェクト全体を破綻させることを回避する必要があります。 そのためには、特定の順序で処理する必要があります。 このような非同期関連の移植の変更を行う例をいくつか見てみましょう。

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>**task\<void\>** メソッドを待機し、プロジェクトの残りの部分は変更しない

**task\<void\>** を返すメソッドは、非同期的に処理を実行しますが、最終的には値を生成しません。 このようなメソッドを `co_await` できます。

したがって、非同期コードの段階的な移植を行うには、まずこれらのメソッドを呼び出す場所を見つけることをお勧めします。 このような場所には、タスクの作成や返却、または各タスクからその継続処理に値が渡されないタスク チェーンが含まれます。 そのような場所では、後で説明するように、非同期コードを `co_await` ステートメントに単純に置き換えることができます。

いくつかの例を見てみましょう。 **Simple3DGameDX** プロジェクトを開きます (「[Direct3D ゲーム サンプル](#the-direct3d-game-sample-simple3dgamedx)」を参照してください)。

> [!IMPORTANT]
> 次の例では、変更されるメソッドの実装からわかるように、変更するメソッドの "*呼び出し元*" を変更する必要はないことに注意してください。 これらの変更は局所的で、プロジェクト全体にカスケードされません。

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>fire-and-forget メソッド内で **task\<void\>** を待機する

ここでは、最も単純なケースである "*fire-and-forget*" メソッド内での **task\<void\>** の待機から始めます。 これらは非同期的に動作するメソッドですが、メソッドの呼び出し元は、その動作が完了するまで待機しません。 メソッドは非同期的に完了しますが、それを呼び出したらそのままにして構いません。

プロジェクトの依存関係グラフのルートを参照して、**create_task** を含む `void` メソッドや、**task\<void\>** メソッドのみが呼び出されるタスク チェーンを探します。

**Simple3DGameDX** では、メソッド **GameMain::Update** の実装に、一致するものが見つかります。 これはソース コード ファイル `GameMain.cpp` にあります。

#### <a name="gamemainupdate"></a>**GameMain::Update**

これは、C++/CX バージョンのメソッドからの抜粋で、非同期に完了する 2 つの部分を示しています。

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

(PPL **task\<void\>** を返す) **Simple3DGame::LoadLevelAsync** メソッドの呼び出しを確認できます。 その後は、いくつかの同期処理を実行する "*継続処理*" です。 **LoadLevelAsync** は非同期ですが、値を返しません。 したがって、タスクから継続処理に値が渡されることはありません。

これらの 2 つの場所のコードは、同じ方法で変更できます。 コードについては、以下の一覧の後で説明します。 ここで、class-member コルーチンの *this* ポインターにアクセスする安全な方法について説明することもできます。 しかし、それは後のセクションで行います。今のところ、このコードが機能します。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

ご覧のように、**LoadLevelAsync** がタスクを返すため、それを `co_await` できます。 また、明示的な継続処理は必要ありません。`co_await` に続くコードは、**LoadLevelAsync** が完了したときにのみ実行されます。

`co_await` を導入すると、メソッドはコルーチンに変換されるため、`void` を返すことができなくなります。 これは fire-and-forget メソッドであるため、[**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) を返します。

また、`GameMain.h` の宣言で、**GameMain::Update** の戻り値の型を `void` から **winrt::fire_and_forget** に変更する必要があります。

この変更をプロジェクトのコピーに加えても、ゲームは同じようにビルドされて実行されます。 ソース コードは現在も基本的に C++/CX ですが、C++/WinRT と同じパターンが使用されるようになったので、残りのコードを機械的に移植できるように少しずつ近づいてきました。

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** はもう 1 つの fire-and-forget メソッドで、**LoadLevelAsync** も呼び出します。 したがって、そこで同じ変更を行うことができます。

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

**GameMain::OnDeviceRestored** では、無処理タスクを含む非同期コードの入れ子がより深くなるため、さらに興味深いものになります。 ここで、そのメソッドの非同期部分の概要を示します (あまり興味を引かない同期コードは省略記号で表します)。

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

最初に、`GameMain.h` および `.cpp` で **GameMain::OnDeviceRestored** の戻り値の型を `void` から **winrt::fire_and_forget** に変更します。 また、`DeviceResources.h` を開き、**IDeviceNotify::OnDeviceRestored** の戻り値の型に同じ変更を加える必要があります。

非同期コードを移植するには、**create_task** および **then** 呼び出しとそれらの中かっこをすべて削除し、メソッドをフラットな一連のステートメントに単純化します。

(タスクを返す) すべての `return` を `co_await` に変更します。 何も返さない `return` が 1 つ残るので、それを削除します。 完了すると、無処理タスクがなくなり、メソッドの非同期部分の概要は次のようになります。 繰り返しになりますが、あまり興味を引かない同期コードは省略記号で表しています。

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

ご覧のとおり、はるかにシンプルで読みやすくなっています。

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

**GameMain::GameMain** コンストラクターは処理を非同期的に実行し、プロジェクトのどの部分もその処理の完了を待機しません。 ここでも、非同期部分の概要を次に示します。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

ただし、コンストラクターは **winrt::fire_and_forget** を返すことができないため、非同期コードを新しい **GameMain::ConstructInBackground** fire-and-forget メソッドに移動し、コードを `co_await` ステートメントにフラット化し、コンストラクターから新しいメソッドを呼び出します。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

これにより、**GameMain** のすべての fire-and-forget メソッド (つまり、すべての非同期コード) がコルーチンに変換されました。 興味がある場合は、他のクラスで fire-and-forget メソッドを見つけ、同様の変更を行うことができます。

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>`co_await` および *this* ポインターに関して延期された説明

**GameMain::Update** に変更を加えたときに、*this* ポインターに関する説明を先延ばしにしました。 ここでその説明を行います。

これは、これまでに変更したすべてのメソッドに適用されます。また、fire-and-forget のものだけでなく、"*すべての*" コルーチンに適用されます。 メソッドに `co_await` を導入すると、"*中断ポイント*" が導入されます。 そのため、*this* ポインターに注意する必要があります。これはもちろん、クラス メンバーにアクセスするたびに、中断ポイントの "*後*" に使用します。

簡単に言えば、解決策は [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) を呼び出すことです。 ただし、この問題と解決策の詳細については、「[class-member コルーチンで *this* ポインターに安全にアクセスする](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)」を参照してください。

**implements::get_strong** は、[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) から派生したクラスでのみ使用できます。

#### <a name="derive-gamemain-from-winrtimplements"></a>**winrt::implements** から **GameMain** を派生させる

最初に行う必要がある変更は `GameMain.h` で行います。

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain** は引き続き **DX::IDeviceNotify** を実装しますが、[**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) から派生するようにそれを変更します。

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

次に、`App.cpp` で、このメソッドを見つけます。

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

ただし今回は、**GameMain** は **winrt::implements** から派生するため、別の方法で構築する必要があります。 この場合、[**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 関数テンプレートを使用します。 詳細については、「[実装型とインターフェイスをインスタンス化して返す](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces)」を参照してください。

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

その変更に関するループを閉じるには、*m_main* の型も変更する必要があります。 `App.h` で、このコードを見つけます。

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

*m_main* の宣言を下記に変更します。

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>**implements::get_strong** が呼び出し可能になった状態

**GameMain::Update** について、および `co_await` を追加したその他のすべてのメソッドについて、コルーチンの先頭で **get_strong** を呼び出して、コルーチンが完了するまで強参照が確実に維持されるようにする方法を以下に示します。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>**task\<void\>** メソッド内で **task\<void\>** を待機する

次に示す最も単純なケースは、それ自身が **task\<void\>** を返すメソッド内で **task\<void\>** を待機します。これは、ユーザーは **task\<void\>** を `co_await` でき、これから `co_return` することができるためです。

メソッド **Simple3DGame::LoadLevelAsync** の実装で非常にシンプルな例を紹介します。 これはソース コード ファイル `Simple3DGame.cpp` にあります。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

いくつかの同期コードがあり、その後に **GameRenderer::LoadLevelResourcesAsync** によって作成されたタスクが返されます。

そのタスクを返す代わりに、それを `co_await` し、その結果の `void` を `co_return` します。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

### <a name="await-taskvoid-within-a-taskt-method"></a>**task\<T\>** メソッド内で **task\<void\>** を待機する

**Simple3DGameDX** には適切な例がありませんが、パターンを示すためだけに仮定の例を考えてみます。

次のコード例は、**task\<void\>** の単純な `co_await` を単に示しています。 次に、**task\<T\>** の戻り値の型を満たすために、Windows ランタイム メソッドを co_await することによって **StorageFile\^** を非同期的に返し、その結果のファイルを `co_return` します。

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

ここでの次の手順は、このようにしてさらに多くのメソッドを C++/WinRT に移植することです。

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

この例では、*m_renderer* データ メンバーはまだ C++/CX のままです。

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>**task** メソッドで **IAsyncXxx**^ を待機し、プロジェクトの残りの部分は変更しない

これまでに、**task\<void\>** を `co_await` する方法について説明しました。 また、**IAsyncXxx** を返すメソッドを `co_await` することもできます (これがプロジェクト内のメソッドであるか、または非同期 Windows API (たとえば、[**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync)) であるかに関係ありません)。

このコード変更を実行できる場所の例については、**BasicReaderWriter::ReadDataAsync** を参照してください (これは `BasicReaderWriter.cpp` に実装されています)。 元の C++/CX バージョンを以下に示します。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Windows API を `co_await` できるだけでなく、メソッドが非同期的に返す値 (この場合はバイト配列) を `co_return` することもできます。 この最初のステップでは、これらの変更のみを行う方法について説明します。次のセクションでは、実際に C++/CX コードを C++/WinRT に移植します。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

ここでも、戻り値の型を変更していないので、変更しようとしているメソッドの "*呼び出し元*" を変更する必要はありません。

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>**ReadDataAsync** (ほとんどすべて) を C++/WinRT に移植し、プロジェクトの残りの部分は変更しない

さらに一歩進んで、プロジェクトの他の部分を変更せずに、メソッドを "*ほぼ完全に*" C++/WinRT に移植できます。

このメソッドがプロジェクトの残りの部分に対して持つ唯一の依存関係は、**BasicReaderWriter::m_location** データ メンバーで、これは C++/CX **StorageFolder**^ です。 そのデータ メンバーを変更しないままにし、かつパラメーターの型と戻り値の型を変更しないままにするには、2 つの変換を (1 つはメソッドの先頭で、もう 1 つは末尾で) 実行すれば十分です。 そのためには、**from_cx** および **to_cx** の相互運用ヘルパー関数を使用することができます。

ここでは、実装を主に C++/WinRT に移植した後に **BasicReaderWriter::ReadDataAsync** がどのようになるかを示します。 これは "*段階的な移植*" の良い例です。 またこのメソッドは、"*C++/WinRT のいくつかの手法を使用する C++/CX メソッド*" としてこれを見なす考えを変え、"*C++/CX と相互運用する C++/WinRT メソッド*" として見なす段階にあります。

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> **ReadDataAsync** では、新しい C++/CX 配列を構築して返しました。 もちろん、これを行ったのは、メソッドの戻り値の型を満たす (それにより、プロジェクトの残りの部分を変更する必要がないようにする) ためです。
>
> ご自身のプロジェクト内で、移植後にメソッドの末尾に到達し、C++/WinRT オブジェクトだけが残るという他の例に遭遇する可能性があります。 それを `co_return` するには、単に **to_cx** を呼び出してそれを変換します。 これは仮定の例です。

```cppcx
task<Windows::Storage::StorageFile^> RetrieveFileAsync()
{
    winrt::Windows::Storage::StorageFile storageFile = co_await ...
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>`co_return` を使用するタスクに **create_async** をラップします

**IAsyncXxx**\^ を直接 `co_return` することはできませんが、同様の処理を行うことができます。 `co_return` を使用するタスクがある場合は、それに [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) をラップすることができます。

**Simple3DGameDX** から使用できる例がないため、次に示すのは仮定の例です。

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ Simple3DGame::ReturnBooleanAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await BooleanMemberFunctionAsync();
            co_return result;
        });
}
```

ご覧のように、`co_await` できる任意のメソッドから戻り値を取得できます。

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>**concurrency::wait** を `co_await winrt::resume_after` に移植する

**Simple3DGameDX** が [**concurrency::wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) を使用して、スレッドを短時間停止する場所がいくつかあります。 次に例を示します。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

**concurrency::wait** の C++/WinRT バージョンは **winrt::resume_after** 構造体です。 その構造体を PPL タスク内で `co_await` することができます。 次にコード例を示します。

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

行う必要があった他の 2 つの変更点に注意してください。 **GameConstants::InitialLoadingDelay** の型を **std::chrono::duration** に変更し、ラムダ関数の戻り値の型を明示的に変更しました。これはコンパイラがそれを推測できなくなったためです。

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>**task\<void\>** の戻り値の型を **winrt::IAsyncXxx** に移植する

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

**Simple3DGameDX** での作業のこの段階では、**Simple3DGame::LoadLevelAsync** を呼び出すプロジェクト内のすべての場所で `co_await` を使用してそれを呼び出します。

つまり、そのメソッドの戻り値の型を **task\<void\>** から **winrt::Windows::Foundation::IAsyncAction** に変更できます。

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

この時点で、そのメソッドの残りの部分とその依存関係を C++/WinRT に移植することは、かなり機械的になるはずです。

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

**GameRenderer::LoadLevelResourcesAsync** の元の C++/CX バージョンを次に示します。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync** は、**GameRenderer::LoadLevelResourcesAsync** を呼び出すプロジェクト内の唯一の場所であり、それを呼び出すために既に `co_await` を使用しています。

そのため、**GameRenderer::LoadLevelResourcesAsync** はタスクを返す必要がなくなり、代わりに **winrt::Windows::Foundation::IAsyncAction** を返すことができます。 また、実装自体が単純なため、C++/WinRT に完全に移植できます。 これには、「[**concurrency::wait** を `co_await winrt::resume_after` に移植する](#port-concurrencywait-to-co_await-winrtresume_after)」で行ったのと同じ変更が含まれります。 また、プロジェクトの他の部分には、懸念すべき重要な依存関係はありません。

以下に、C++/WinRT への完全な移植後に、メソッドがどのように見えるかを示します。

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="basicreaderwriterreaddataasync"></a>**BasicReaderWriter::ReadDataAsync**

では、**BasicReaderWriter::ReadDataAsync** を C++/WinRT に完全に移植することによって、このチュートリアルを完了します。 前回これを見たとき (「[**ReadDataAsync** (ほとんどすべて) を C++/WinRT に移植し、プロジェクトの残りの部分は変更しない](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)」において) は、ほとんどすべてが C++/WinRT に移植されていました。 ただし、その場合も **Platform::Array\<byte\>** ^ のタスクを返していました。

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

タスクを返すのではなく、(呼び出しサイトでコードを変更することになりますが) C++/WinRT [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) オブジェクトを非同期的に返すようにそれを変更してみましょう。

ここでは、メソッドの実装とそのパラメーター、および *m_location* データ メンバーを移植して、C++/WinRT 構文およびオブジェクトを使用するようにした後に、メソッドがどのように見えるかを示します。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

ご覧のとおり、**BasicReaderWriter::ReadDataAsync** 自体ははるかに単純です。これは、バッファーからバイトを取得する同期ロジックを独自のメソッドに組み込んだためです。

しかし、ここでは、C++/CX のこの種類の構造から呼び出しサイトを移植する必要があります。

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

C++/WinRT のこのパターンに行います。

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>重要な API

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>関連トピック

* [C++/CX から C++/WinRT への移行](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [C++/WinRT と C++/CX 間の相互運用](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [C++/WinRT を使用した同時開催操作と非同期操作](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [C++/WinRT の強参照と弱参照](/windows/uwp/cpp-and-winrt-apis/weak-references)
* [C++/WinRT で API を作成する](/windows/uwp/cpp-and-winrt-apis/author-apis)
