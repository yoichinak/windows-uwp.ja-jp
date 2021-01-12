---
description: このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) プロジェクト内のソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植することに関する技術的な詳細を取り上げています。
title: C++/CX から C++/WinRT への移行
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 移植, 移行, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 035003be1c9b8ef84d0563af6be9f5b3a01978c7
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860140"
---
# <a name="move-to-cwinrt-from-ccx"></a>C++/CX から C++/WinRT への移行

このトピックは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) プロジェクトのソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植する方法について説明する一連のトピックの最初のものです。

プロジェクトで [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 型も使用している場合は、「[WRL から C++/WinRT への移行](move-to-winrt-from-wrl.md)」を参照してください。

## <a name="strategies-for-porting"></a>移植の方法

C++/CX から C++/WinRT への移植は一般的に簡単ですが、[並列パターン ライブラリ (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) タスクのコルーチンへの移動は 1 つの例外であるということを知っておいてください。 モデルはそれぞれ異なります。 PPL タスクからコルーチンへの自然な 1 対 1 のマッピングはなく、すべてのケースで動作するコードを機械的に移植するための簡単な方法はありません。 移植のこの特定の側面と、2 つのモデル間の相互運用のためのオプションについては、「[非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)」を参照してください。

開発チームは常に、非同期コードを移植するハードルを超えれば、残りの移植作業は主に機械的であるということを報告しています。

### <a name="porting-in-one-pass"></a>1 回の動作での移植

プロジェクト全体を 1 回の動作で移植できる場合、必要な情報はこのトピックだけになります (この後に続く *相互運用* に関するトピックは不要です)。 まず、いずれかの C++/WinRT プロジェクト テンプレートを使用して、Visual Studio で新しいプロジェクトを作成することをお勧めします ([C++/WinRT の Visual Studio のサポート](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください)。 その後、ソース コード ファイルを新しいプロジェクトに移動し、その際にすべての C++/CX ソース コードを C++/WinRT に移植します。

または、既存の C++/CX プロジェクト内で移植作業を行う場合は、それに C++/WinRT サポートを追加する必要があります。 これを実行する手順については、「[C++/CX プロジェクトを取得して C++/WinRT サポートを追加する](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)」を参照してください。 移植が完了するまでには、純粋な C++/CX プロジェクトが純粋なプロジェクト C++/WinRT に変更されます。

> [!NOTE]
> Windows ランタイム コンポーネント プロジェクトがある場合は、1 回の動作での移植が唯一のオプションです。 C++ で記述された Windows ランタイム コンポーネント プロジェクトには、すべて C++/CX のソース コードまたはすべて C++/WinRT のソース コードのいずれかが含まれている必要があります。 このプロジェクトの種類でこれらを共存させることはできません。

### <a name="porting-a-project-gradually"></a>プロジェクトの段階的な移植

前のセクションで説明したように、Windows ランタイム コンポーネント プロジェクトを除き、コードベースのサイズまたは複雑さによってプロジェクトを段階的に移植する必要がある場合は、C++/CX と C++/WinRT のコードがしばらくの間同じプロジェクト内に共存する移植プロセスが必要になります。 このトピックの参照に加えて、「[C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md)」および「[非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)」も参照してください。 これらのトピックでは、2 つの言語プロジェクションを相互運用する方法を示す情報とコード例を提供します。

段階的な移植プロセス用にプロジェクトを準備するには、1 つのオプションとして C++/CX プロジェクトに C++/WinRT サポートを追加する方法があります。 これを実行する手順については、「[C++/CX プロジェクトを取得して C++/WinRT サポートを追加する](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)」を参照してください。 その後、段階的に移植できます。

別の方法として、いずれかの C++/WinRT プロジェクト テンプレートを使用して、Visual Studio で新しいプロジェクトを作成します ([C++/WinRT の Visual Studio のサポート](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください)。 その後、 そのプロジェクトに C++/CX サポートを追加します。 これを実行する手順については、「[C++/WinRT プロジェクトを取得して C++/CX サポートを追加する](./interop-winrt-cx.md#taking-a-cwinrt-project-and-adding-ccx-support)」を参照してください。 その後、ソース コードのそこへの移動を開始し、その際に "*一部の*" C++/CX ソース コードを C++/WinRT に移植します。

どちらの場合も、C++/WinRT コードとまだ移植されていない C++/CX コードの間で (両方向で) 相互運用することになります。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) と Windows SDK の両方で、ルート名前空間 **Windows** で型を宣言します。 C++/WinRT に投影された Windows 型は Windows 型と同じ完全修飾名を持ちますが、 C++ **winrt** 名前空間に配置されます。 これらの異なる名前空間では、独自のペースで C++/CX から C++/WinRT へ移植できます。

#### <a name="porting-a-xaml-project-gradually"></a>XAML プロジェクトの段階的な移植

> [!IMPORTANT]
> XAML を使用するプロジェクトでは、XAML ページのすべての種類は、完全に C++/CX であるか、または完全に C++/WinRT である必要があります。 それでも、(model や viewmodel などの) 同じプロジェクト内の XAML ページの種類の "*外部*" で、C++/CX と C++/WinRT を混在させることができます。

このシナリオで推奨されるワークフローは、新しい C++/WinRT プロジェクトを作成し、ソース コードとマークアップを C++/CX プロジェクトからコピーすることです。 すべての XAML ページの種類が C++/WinRT である限り、 **[プロジェクト]** \> **[新しい項目の追加...]** を使用して新しい XAML ページを追加できます。\> **[Visual C++]**  >  **[空白のページ] (C++/WinRT)** を選択します。

または、Windows ランタイム コンポーネント (WRC) を使用して、移植時に XAML C++/CX プロジェクトからコードを除外することもできます。

- 新しい C++/CX WRC プロジェクトを作成し、そのプロジェクトにできるだけ多くの C++/CX コードを移動して、XAML プロジェクトを C++/WinRT に変更することができます。
- または、新しい C++/WinRT の WRC プロジェクトを作成し、XAML プロジェクトは C++/CX のままとし、C++/CX から C++/WinRT への移植と、XAML プロジェクトからコンポーネント プロジェクトへの結果コードの移動を開始できます。
- また、同じソリューション内に C++/CX コンポーネント プロジェクトと C++/WinRT コンポーネント プロジェクトを一緒に用意し、ご利用のアプリケーション プロジェクトから両方を参照し、一方からもう一方に徐々に移植することもできます。 ここでも、同じプロジェクト内で 2 つの言語プロジェクションを使用する方法の詳細については、「[C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md)」を参照してください。

## <a name="first-steps-in-porting-a-ccx-project-to-cwinrt"></a>C++/CX プロジェクトを C++/WinRT に移植するための最初の手順

移植方法 (1 回の動作での移植または段階的な移植) に関係なく、最初の手順は、移植のためにプロジェクトを準備することです。 ここでは、開始するプロジェクトの種類とその設定方法について、「[移植の方法](#strategies-for-porting)」で説明した内容を要約します。

- **1 回の動作での移植**。 いずれかの C++/WinRT プロジェクト テンプレートを使用して Visual Studio で新しいプロジェクトを作成します。 C++/CX プロジェクトからその新しいプロジェクトにファイルを移動し、C++/CX ソース コードを移植します。
- **XAML 以外のプロジェクトの段階的な移植**。 C++/CX プロジェクトに C++/WinRT サポートを追加することを選択し (「[C++/CX プロジェクトを取得して C++/WinRT サポートを追加する](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)」を参照)、段階的に移植できます。 または、新しい C++/WinRT プロジェクトを作成し、それに C++/CX サポートを追加し (「[C++/WinRT プロジェクトを取得して C++/CX サポートを追加する](./interop-winrt-cx.md#taking-a-cwinrt-project-and-adding-ccx-support)」を参照)、ファイルを移動して、段階的に移植することもできます。
- **XAML プロジェクトの段階的な移植**。 新しい C++/WinRT プロジェクトを作成し、ファイルを移動して、段階的に移植します。 XAML ページの種類は常に、すべて C++/WinRT "*または*" すべて C++/CX の "*いずれか*" である必要があります。

このトピックの残りの部分は、選択する移植方法に関係なく適用されます。 これには、ソース コードを C++/CX から C++/WinRT に移植するために必要な技術情報のカタログが含まれています。 段階的に移植する場合は、「[C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md)」および「[非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)」も参照してください。

## <a name="file-naming-conventions"></a>ファイルの名前付け規則

### <a name="xaml-markup-files"></a>XAML マークアップ ファイル

| ファイルの発生元 | C++/CX | C++/WinRT |
| - | - | - |
| **開発者の XAML ファイル** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (下記を参照) |
| **生成された XAML ファイル** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

C++/WinRT では `*.h` と `*.cpp` のファイル名から `.xaml` が削除されることに注意してください。

C++/WinRT では、追加の開発者ファイルである **Midl ファイル (.idl)** が追加されます。 C++/CX ではこのファイルが内部的に自動生成されて、パブリック メンバーと保護されたメンバーのすべてに追加されます。 C++/WinRT では、自分でファイルを追加して作成します。 IDL の使用に関する詳細、コード、およびチュートリアルについては、「[XAML コントロール、C++/WinRT プロパティへのバインド](./binding-property.md)」をご覧ください。

「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」も参照してください

### <a name="runtime-classes"></a>ランタイム クラス

C++/CX ではヘッダー ファイルの名前に制限は適用されません。特に小規模なクラスの場合は、複数のランタイムクラス定義を 1 つのヘッダー ファイルに組み入れるのが一般的です。 ただし、C++/WinRT では、各ランタイム クラスに、クラス名に基づいて名前が付けられた独自のヘッダー ファイルがある必要があります。 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

C++/CX ではあまり一般的ではない (ただし合法的ではある) こととして、XAML カスタム コントロールには異なる名前のヘッダー ファイルを使用することがあります。 これらのヘッダー ファイルの名前は、クラス名と一致するように変更する必要があります。

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>ヘッダー ファイルの要件

C++/CX では、ヘッダー ファイルが `.winmd` ファイルから内部的に自動生成されるため、特別なヘッダー ファイルを含める必要はありません。 C++/CX では、名前によって使用する名前空間に `using` ディレクティブを使用するのが一般的です。

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

`using namespace Windows::Media::Playback` ディレクティブを使用すると、名前空間プレフィックスなしで `MediaPlaybackItem` を書き込むことができます。 `item->VideoTracks->GetAt(0)` は **Windows.Media.Core.VideoTrack** を返すため、`Windows.Media.Core` 名前空間についても触れました。 ただし、**VideoTrack** という名前を入力する必要がなかったため、`using Windows.Media.Core` ディレクティブは必要ありませんでした。

しかし、C++/WinRT では、名前を付けない場合でも、使用する各名前空間に対応するヘッダー ファイルをインクルードする必要があります。

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

一方、**MediaPlaybackItem.AudioTracksChanged** イベントの型が **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** であっても、そのイベントは使用しなかったため、`winrt/Windows.Foundation.Collections.h` をインクルードする必要はありませんでした。

また、C++/WinRT では、XAML マークアップによって使用される名前空間のヘッダー ファイルをインクルードする必要もあります。

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

**Rectangle** クラスを使用することは、このインクルードを追加する必要があることを意味します。

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

ヘッダー ファイルを忘れた場合は、すべての操作が正常にコンパイルされますが、`consume_` クラスが見つからないためにリンカー エラーが発生します。

## <a name="parameter-passing"></a>パラメーターの引き渡し

C++/CX ソース コードを記述するときは、ハット (\^) 参照のように C++/CX 型を関数パラメーターとして渡します。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT では、同期関数に既定で `const&` パラメーターを使用する必要があります。 これにより、コピーとインター ロック オーバーヘッドが回避されます。 ただし、コルーチンが値による呼び出しをして有効期間の問題を回避するように、値渡しを使用する必要があります (詳細については、「[C++/WinRT を使用した同時実行と非同期操作](concurrency.md)」を参照してください)。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT オブジェクトは、基本的に Windows ランタイムのバッキング オブジェクトへのインターフェイス ポインターを保持する値です。 C++/WinRT オブジェクトをコピーすると、コンパイラはカプセル化されたインターフェイス ポインターをコピーし、その参照カウントをインクリメントします。 コピーの最終的なデストラクションには、参照カウントのデクリメントも含まれます。 そのため、必要な場合にのみ、コピーのオーバーヘッドが発生します。

## <a name="variable-and-field-references"></a>変数とフィールドの参照

C++/CX ソース コードを記述するときに、ハット (\^) 変数を使用して Windows ランタイム オブジェクトを参照し、矢印 (-&gt;) 演算子を使用してハット変数を逆参照します。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

同等の C++/WinRT コードに移植する場合は、ハットを取り除き、矢印演算子 (-&gt;) をドット演算子 (.) に変更することにより、開始できるようになります。 C++/WinRT のプロジェクションが実行された型は値であり、ポインターではありません。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

C++/CX ハット参照用の既定のコンストラクターにより、それを null に初期化します。 次に示したのは、正しい型の変数/フィールドを作成するための C++/CX コード例でが、初期化は行われません。 つまり、それによって最初は **TextBlock** は参照されません。後で参照を割り当てるつもりです。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

C++/WinRT での同等のものについては、「[初期化の遅延](consume-apis.md#delayed-initialization)」を参照してください。

## <a name="properties"></a>プロパティ

C++/CX 言語拡張機能には、プロパティの概念が含まれています。 C++/CX ソース コードを記述するときに、フィールドと同様にプロパティにアクセスできます。 標準 C++ にはプロパティの概念がないため、C++/WinRT では、Get 関数と Set 関数を呼び出します。

次の例では、**XboxUserId**、**UserState**、**PresenceDeviceRecords**、**Size** はすべてプロパティです。

### <a name="retrieving-a-value-from-a-property"></a>プロパティからの値の取得

C++/CX でプロパティの値を取得する方法は次のとおりです。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

同等の C++/WinRT ソース コードは、プロパティと同じ名前を持ち、パラメーターのない関数を呼び出します。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

**PresenceDeviceRecords** 関数は、それ自体が **Size** 関数を持つ Windows ランタイム オブジェクトを返します。 返されたオブジェクトは C++/WinRT の投影された型でもあるため、ドット演算子を使用して逆参照し、**Size** を呼び出します。

### <a name="setting-a-property-to-a-new-value"></a>新しい値へのプロパティの設定

新しい値にプロパティを設定するには、同様のパターンに従います。 まず、C++/CX で次の操作を行います。

```cppcx
record->UserState = newValue;
```

C++/WinRT で対応する操作を行うには、プロパティと同じ名前の関数を呼び出し、引数を渡します。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>クラスのインスタンスの作成

C++/CX オブジェクトは、それに対するハンドル (通常はハット (\^) 参照と呼ばれます) を介して操作します。 `ref new` キーワードにより新しいオブジェクトを作成します。これにより、[**RoActivateInstance**](/windows/desktop/api/roapi/nf-roapi-roactivateinstance) が呼び出され、ランタイム クラスの新しいインスタンスがアクティブ化されます。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT オブジェクトは値であるため、スタックに割り当てるか、オブジェクトのフィールドとして割り当てることができます。 C++/WinRT オブジェクトを割り当てるために、`ref new` (または `new`) は使用 *しません*。 バックグラウンドで **RoActivateInstance** は引き続き呼び出されています。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

リソースを初期化するコストが高い場合は、実際に必要になるまで初期化を遅延するのが一般的です。 既に説明したように、C++/CX ハット参照用の既定のコンストラクターにより、それを null に初期化します。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

同じコードが C++/WinRT に移植されました。 **std::nullptr_t** コンストラクターの使用に注意してください。 コンストラクターの詳細については、「[初期化の遅延](consume-apis.md#delayed-initialization)」を参照してください。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="how-the-default-constructor-affects-collections"></a>既定のコンストラクターによるコレクションへの影響

C++ コレクション型では既定のコンストラクターが使用されるため、意図しないオブジェクトの構築が発生する可能性があります。

| シナリオ | C++/CX | C++/WinRT (正しくない) | C++/WinRT (正しい) |
| - | - | - | - |
| ローカル変数 (最初は空) | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| メンバー変数 (最初は空) | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| グローバル変数 (最初は空) | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| 空の参照のベクトル | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| マップに値を設定する | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| 空の参照の配列 | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| ペアリング | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>空の参照のコレクションの詳細

C++/CX (実際には隣接したコンテナー) 内に **Platform::Array\^** ([Port **Platform::Array\^**](#port-platformarray) を参照) がある場合はいつでも、それを配列のままにしておくのではなく、C++/WinRT の **std::vector** に移植するかどうかを選択できます。 **std::vector** を選択することには利点があります。

たとえば、空の参照の固定サイズ ベクトルを作成するための簡単な方法はありますが (上記の表を参照)、空の参照の "*配列*" を作成するための簡単な方法はありません。 配列内の要素ごとに、`nullptr` を繰り返す必要があります。 値が少なすぎる場合は、追加分が既定で構築されます。

ベクトルの場合は、初期化時に空の参照を入力できます (上の表を参照)。または、初期化後にこのようなコードを使用して空の参照を入力することもできます。

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>**std::map** の例に関する詳細

**std::map** の `[]` 添字演算子は、次のように動作します。

- キーがマップ内にある場合は、既存の値への参照を返します (これは上書きできます)。
- キーがマップ内にない場合は、キー (移動できる場合は移動済み) と *既定で構築された値* で構成される新しいエントリをマップ内に作成し、値への参照を返します (これは後で上書きできます)。

つまり、`[]` 演算子は常にマップ内にエントリを作成します。 これは C#、Java、および JavaScript とは異なります。

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>基本ランタイム クラスから派生クラスへの変換

派生型のオブジェクトを参照する既知の reference-to-base を使用することは一般的です。 C++/CX では、`dynamic_cast` を使用して、reference-to-base を reference-to-derived に "*キャスト*" します。 `dynamic_cast` は、実際には [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) の隠された呼び出しです。 次に示すのは典型的な例です。依存関係プロパティの変更イベントを処理しており、**DependencyObject** から、依存関係プロパティを所有する実際の型にキャストし直す必要があります。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

同等の C++/WinRT コードでは、`dynamic_cast` が [**IUnknown :: try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 関数 (**QueryInterface** をカプセル化する) の呼び出しによって置き換えられます。 代わりに [**IUnknown :: as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) を呼び出すオプションもあります。このオプションでは、必須のインターフェイス (要求している型の既定のインターフェイス) に対するクエリが返されない場合に例外がスローされます。 次に C++/WinRT コード例を示します。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>派生クラス

ランタイム クラスから派生するためには、基底クラスが *構成可能* である必要があります。 C++/CX ではクラスを構成可能にするために特別な手順を実行する必要はありませんが、C++/WinRT では必要です。 クラスを基底クラスとして使用できるようにすることを示すには、[シールされていないキーワード](/uwp/midl-3/intro#base-classes)を使用します。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

実装ヘッダー クラスでは、派生クラスの自動生成されたヘッダーをインクルードする前に、基底クラスのヘッダー ファイルをインクルードする必要があります。 そうしないと、"この型は式として不適切に使用されています" などのエラーが表示されます。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>デリゲートを使用したイベント処理

この場合はデリゲートとしてラムダ関数を使用して、C++/CX でイベントを処理する典型的な例を次に示します。

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

C++/WinRT で対応するコードは次のとおりです。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

ラムダ関数の代わりに、デリゲートを自由関数として実装するか、またはメンバー関数へのポインターとして実装するかを選択できます。 詳細については、「[C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)」を参照してください。

イベントとデリゲートが内部的に使用される C++/CX コードベース (バイナリではなく) から移植する場合は、[**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) を使用すると、C++/WinRT でそのパターンを複製できます。 「[パラメーター化されたデリゲート、単純なシグナル、およびプロジェクト内でのコールバック](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)」も参照してください。

## <a name="revoking-a-delegate"></a>デリゲートの取り消し

C++/CX では、`-=` 演算子を使用して前のイベント登録を取り消します。

```cppcx
myButton->Click -= token;
```

C++/WinRT で対応するコードは次のとおりです。

```cppwinrt
myButton().Click(token);
```

詳細とオプションについては、「[登録済みデリゲートの取り消し](handle-events.md#revoke-a-registered-delegate)」を参照してください。

## <a name="boxing-and-unboxing"></a>ボックス化とボックス化解除

C++/CX では、スカラーが自動的にオブジェクト内にボックス化されます。 C++/Winrt では、[**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 関数を明示的に呼び出す必要があります。 どちらの言語でも、明示的にボックス化解除する必要があります。 [C++/WinRT を使用したボックス化とボックス化解除](./boxing.md)に関するページを参照してください。

次の表では、これらの定義を使用します。

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| 操作 | C++/CX | C++/WinRT|
|-|-|-|-|
| ボックス化 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| ボックス化解除 | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX と C# では、Null ポインターを値型にボックス化解除しようとすると例外が発生します。 C++/WinRT はこれをプログラミング エラーと見なし、クラッシュします。 C++/Winrt では、オブジェクトが想定した型ではないケースに対処したい場合は [**winrt:: unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 関数を使用します。

| シナリオ | C++/CX | C++/WinRT|
|-|-|-|-|
| 既知の整数をボックス化解除する | `i = (int)o;` | `i = unbox_value<int>(o);` |
| o が null の場合 | `Platform::NullReferenceException` | クラッシュ |
| o がボックス化された int でない場合 | `Platform::InvalidCastException` | クラッシュ |
| int をボックス化解除し、null の場合はフォールバックを使用する。それ以外の場合はクラッシュ | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 可能であれば int をボックス化解除する。それ以外の場合はフォールバックを使用する | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>文字列のボックス化とボックス化解除

文字列は、ある点では値型であり、別の点では参照型です。 C++/CX と C++/WinRT では文字列の扱いが異なります。

ABI 型 [**HSTRING**](/windows/win32/winrt/hstring) は、参照カウント文字列へのポインターです。 しかし、[**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) から派生したものではないため、厳密に言えば *オブジェクト* ではありません。 さらに、null の **HSTRING** は空の文字列を表します。 **IInspectable** から派生したもの以外のボックス化は、[**IReference \<T\>**](/uwp/api/windows.foundation.ireference_t_) 内にラップすることによって行われ、Windows ランタイムでは標準の実装が [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) オブジェクトの形式で行われます (カスタム型は [**PropertyType::Othertype**](/uwp/api/windows.foundation.propertytype) として報告されます)。

C++/CX は Windows ランタイム文字列を参照型として表しますが、C++/WinRT は文字列を値型として投影します。 つまり、ボックス化された null 文字列は、どのように表されるかによって異なる表現を持つことができます。

さらに、C++/CS を使用すると、null **String^** を逆参照できます。この場合は、文字列 `""` のように動作します。

| 動作 | C++/CX | C++/WinRT|
|-|-|-|
| 宣言 | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| 文字列型のカテゴリ | 参照型 | 値の種類 |
| null **HSTRING** による投影 | `(String^)nullptr` | `hstring{}` |
| null と `""` が同一かどうか | はい | はい |
| null の有効性 | `s = nullptr;`<br>`s->Length == 0` (有効) | `s = hstring{};`<br>`s.size() == 0` (有効) |
| オブジェクトに null 文字列を割り当てる場合 | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| オブジェクトに `""` を割り当てる場合 | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本的なボックス化とボックス化解除。

| 操作 | C++/CX | C++/WinRT|
|-|-|-|
| 文字列をボックス化する | `o = s;`<br>空の文字列は nullptr になります。 | `o = box_value(s);`<br>空の文字列は非 null オブジェクトになります。 |
| 既知の文字列をボックス化解除する | `s = (String^)o;`<br>null オブジェクトは空の文字列になります。<br>文字列でない場合は InvalidCastException。 | `s = unbox_value<hstring>(o);`<br>null オブジェクトはクラッシュします。<br>文字列でない場合はクラッシュします。 |
| 使用可能な文字列をボックス化解除する | `s = dynamic_cast<String^>(o);`<br>null オブジェクトまたは文字列以外は空の文字列になります。 | `s = unbox_value_or<hstring>(o, fallback);`<br>null または文字列以外はフォールバックになります。<br>空の文字列は保持されます。 |

## <a name="concurrency-and-asynchronous-operations"></a>同時実行操作と非同期操作

並列パターン ライブラリ (PPL) ([**concurrency::task**](/cpp/parallel/concrt/reference/task-class) など) は、C++/CS ハット参照をサポートするように更新されました。

C++/WinRT の場合は、代わりにコルーチンと `co_await` を使用する必要があります。 詳細とコード例については、「[C++/WinRT を使用した同時開催操作と非同期操作](./concurrency.md)」を参照してください。

## <a name="consuming-objects-from-xaml-markup"></a>XAML マークアップからのオブジェクトの使用

C++/CX プロジェクトでは、XAML マークアップからプライベート メンバーと名前付き要素を使用できます。 ただし、C++/WinRT では、XAML [ **{x:Bind} マークアップ拡張**](../xaml-platform/x-bind-markup-extension.md)の使用によって使用されるすべてのエンティティが、IDL で公開されている必要があります。

また、ブール値へのバインドにより C++/CX では `true` または `false` が表示されますが、C++/WinRT では **Windows.Foundation.IReference`1\<Boolean\>** が表示されます。

詳細とコード例については、[マークアップからのオブジェクトの使用](./binding-property.md#consuming-objects-from-xaml-markup)に関するページを参照してください。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>C++/WinRT 型への C++/CX **Platform** 型のマッピング

C++/CX は **Platform** 名前空間でいくつかのデータ型を提供します。 これらの型は標準 C++ ではないため、Windows ランタイム言語拡張機能を有効にしたとき (Visual Studio プロジェクトのプロパティの **[C/C++]**  >  **[全般]**  >  **[Windows ランタイム拡張機能の使用]**  >  **[はい (/ZW)]** ) にのみ使用できます。 下の表は、**Platform** 型を C++/WinRT の同等の型に移植するときに役立ちます。 完了したら、C++/WinRT は標準 C++ であるため、`/ZW` オプションをオフにすることができます。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | 「[**Platform::Array\^** を移植する](#port-platformarray)」を参照してください。 |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>**Platform::Agile\^** を **winrt::agile_ref** に移植する

C++/CX の **Platform::agile\^** 型は、任意のスレッドからアクセスできる Windows ランタイム クラスとなります。 C++/WinRT の同等の型は [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) です。

C++/CX で、次の操作を行います。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

C++/WinRT で、次の操作を行います。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>**Platform::Array\^** を移植する

C++/CX で配列を使用する必要がある場合、C++/WinRT では任意の隣接したコンテナーを使用できます。 **std::vector** の使用が適切である理由については、「[既定のコンストラクターによるコレクションへの影響](#how-the-default-constructor-affects-collections)」を参照してください。

このため、C++/CX の **Platform::Array\^** がある場合は常に、移植オプションに、初期化子リスト、**std::array**、または **std::vector** の使用を含めます。 詳細およびコード例については、「[標準的な初期化子リスト](./std-cpp-data-types.md#standard-initializer-lists)」および「[標準的な配列とベクトル](./std-cpp-data-types.md#standard-arrays-and-vectors)」を参照してください。

### <a name="port-platformexception-to-winrthresult_error"></a>**Platform::Exception\^** を **winrt::hresult_error** に移植する

Windows ランタイム API から S\_OK HRESULT 以外が返された場合、**Platform::Exception\^** 型が C++/CX で生成されます。 C++/WinRT の同等の型は [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) です。

C++/WinRT に移植するには、**Platform::Exception\^** を使用するすべてのコードを、**winrt::hresult_error** を使用するように変更します。

C++/CX で、次の操作を行います。

```cppcx
catch (Platform::Exception^ ex)
```

C++/WinRT で、次の操作を行います。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT には、これらの例外クラスが用意されています。

| 例外の種類 | 基本クラス | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | [  **hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 呼び出し |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

各クラスは (**hresult_error** 基本クラスを介して)、エラーの HRESULT を返す [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 関数を指定し、その HRESULT の文字列表現を返す [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 関数を指定します。

C++/CX で例外をスローする例を次に示します。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

また、C++/WinRT での同等のコードは次のとおりです。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>**Platform::Object\^** を **winrt::Windows::Foundation::IInspectable** に移植する

すべての C++/WinRT 型と同様に、**winrt::Windows::Foundation::IInspectable** は値の型です。 その型の変数を null に初期化する方法は次のとおりです。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>**Platform::String\^** を **winrt::hstring** に移植する

**Platform::String\^\^** は Windows ランタイム HSTRING ABI 型と同等です。 C++/WinRT では、同等の型は [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) です。 ただし、C++/WinRT では、**std::wstring** などの C++ 標準ライブラリのワイド文字列型、およびワイド文字列リテラルを使用して Windows ランタイム API を呼び出すことができます。 詳細とコード例については、「[C++/WinRT での文字列の処理](strings.md)」を参照してください。

C++/CX では、[**Platform::String::Data**](/cpp/cppcx/platform-string-class#data) プロパティにアクセスして、C スタイルの **const wchar_t\* *_ 配列として文字列を取得できます (たとえば、それを _* std::wcout** に渡すために)。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

C++/WinRT で同じ操作を行うには、[**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 関数を使用して null で終了する C スタイルの文字列バージョンを取得します。これは **std::wstring** から取得する場合と同様です。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

文字列を取るかまたは文字列を返す API の実装に関しては、通常、**Platform::String\^** を使用する C++/CX コードを変更して、代わりに **winrt::hstring** を使用します。

文字列を取る C++/CX API の例を次に示します。

```cppcx
void LogWrapLine(Platform::String^ str);
```

C++/WinRT では、次のようにその API を [MIDL 3.0](/uwp/midl-3) で宣言できます。

```idl
// LogType.idl
void LogWrapLine(String str);
```

次に C++/WinRT ツール チェーンは次のようなソース コードを生成します。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX 型には [Object::ToString](/cpp/cppcx/platform-object-class#tostring) メソッドが用意されています。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT ではこの機能は直接提供されていませんが、代替機能があります。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

また、C++/WinRT では、限られた数の型のための [**winrt:: to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) もサポートしています。 stringify が必要なその他の型のオーバーロードを追加する必要があります。

| Language | Stringify int | Stringify enum |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

列挙型の stringify の場合は、**winrt::to_hstring** の実装を指定する必要があります。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

このような文字列化は、多くの場合、データ バインディングによって暗黙的に使用されます。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

これらのバインディングは、バインドされたプロパティの **winrt::to_hstring** を実行します。 2 番目の例 **(statusenum**) の場合は、**winrt::to_hstring** の独自のオーバーロードを指定する必要があります。そうしないと、コンパイラ エラーが発生します。

#### <a name="string-building"></a>文字列の作成

C++/CX と C++/WinRT は、文字列の作成には標準 **std::wstringstream** に従います。

| 操作 | C++/CX | C++/WinRT |
|-|-|-|
| 文字列を追加し、null 値を保持する | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| 文字列を追加し、最初の null で停止する | `stream << s->Data();` | `stream << s.c_str();` |
| 結果を抽出する | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>その他の例

次の例では *ws* は **std::wstring** 型の変数です。 また、C++/CX では 8 ビット文字列から **Platform::String** を構築できますが、C++/WinRT ではこれは実行されません。

| 操作 | C++/CX | C++/WinRT |
| - | - | - |
| リテラルから文字列を構築する | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| **std::wstring** から変換し、null 値を保持する | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| **std::wstring** から変換し、最初の null で停止する | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| **std::wstring** に変換し、null 値を保持する | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| **std::wstring** に変換し、最初の null で停止する | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| メソッドにリテラルを渡す | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| **std::wstring** をメソッドに渡す | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>重要な API

* [winrt::delegate 構造体テンプレート](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 構造体](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 構造体](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>関連トピック

* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT でのイベントの作成](./author-events.md)
* [C++/WinRT を使用した同時開催操作と非同期操作](./concurrency.md)
* [C++/WinRT で API を使用する](./consume-apis.md)
* [C++/WinRT でのデリゲートを使用したイベントの処理](./handle-events.md)
* [C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md)
* [非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md)
* [Microsoft インターフェイス定義言語 3.0 リファレンス](/uwp/midl-3)
* [WRL から C++/WinRT への移行](./move-to-winrt-from-wrl.md)
* [C++/WinRT での文字列の処理](./strings.md)
