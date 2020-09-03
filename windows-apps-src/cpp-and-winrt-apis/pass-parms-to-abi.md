---
description: C++/WinRT では、一般的なケースの自動変換を提供することによって、ABI 境界へのパラメーターの受け渡しが簡略化されます。
title: ABI 境界へのパラメーターの受け渡し
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 受け渡し, パラメーター, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 05a627349ad2c4fda890a4f5280f5d33454ea910
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154456"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>ABI 境界へのパラメーターの受け渡し

C++/WinRT の **winrt::param** 名前空間の型では、一般的なケースの自動変換が提供されることにより、ABI 境界へのパラメーターの受け渡しが簡略化されます。 詳細およびコード例については、「[文字列の処理](./strings.md)」および「[標準的な C++ のデータ型と C++/WinRT](./std-cpp-data-types.md)」をご覧ください。

> [!IMPORTANT]
> **winrt::param** 名前空間内の型を自分では使用しないでください。 それらはプロジェクションのためのものです。

多くの型には、同期バージョンと非同期バージョンの両方があります。 C++/WinRT では、同期メソッドにパラメーターを渡すときは、同期バージョンが使われます。非同期メソッドにパラメーターを渡すときは、非同期バージョンが使われます。 非同期バージョンでは、操作が完了する前に呼び出し元によってコレクションが変更されるのを難しくするため、余分なステップが実行されます。 ただし、どのバリエーションでも、別のスレッドからのコレクションの変更は保護されないことに注意してください。 その防止は、開発者が行う必要があります。

## <a name="string-parameters"></a>文字列パラメーター

**winrt::param::hstring** では、**HSTRING** を受け取る API へのパラメーターの引き渡しが簡略化されます。

|渡すことができる型|メモ|
|-|-|
|`{}`|空の文字列を渡します。|
|**winrt::hstring**||
|**std::wstring_view**|リテラルの場合、`L"Name"sv` と記述することができます。 ビューには、末尾の後に null 終端文字が必要です。|
|**std::wstring**|-|
|**wchar_t const\***|null で終わる文字列。|

`nullptr` は許可されません。 代わりに `{}` を使います。

コンパイラでは、コンパイル時に文字列リテラルで `wcslen` を評価する方法が認識されています。 したがって、リテラルの場合、`L"Name"sv` と `L"Name"` は同等です。

**std::wstring_view** オブジェクトは null で終了しませんが、C++/WinRT では、文字列の末尾の後の文字が null である必要があります。 null で終了していない **std::wstring_view** を渡した場合、プロセスは終了します。

## <a name="iterable-parameters"></a>反復可能なパラメーター

**winrt::param::iterable\<T\>** と **winrt::param::async_iterable\<T\>** では、**IIterable\<T\>** を受け取る API へのパラメーターの引き渡しが簡略化されます。

Windows ランタイム コレクションは、既に **IIterable** です。

|渡すことができる型|同期|Async|メモ|
|-|-|-|-|
| `nullptr` | はい | はい | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IIterable\<T\>** | はい | はい | または、それに変換可能なもの。|
| **std::vector\<T\> const&** | はい | いいえ ||
| **std::vector\<T\>&&** | はい | はい | 変更を防ぐため、内容は反復子に移動されます。|
| **std::initializer_list\<T\>** | はい | はい | 非同期バージョンでは、項目がコピーされます。|
| **std::initializer_list\<U\>** | はい | いいえ | **U** は **T** に変換できる必要があります。|
| `{ ForwardIt begin, ForwardIt end }` | はい | いいえ | `*begin` は **T** に変換できる必要があります。|

**U** が **T** に変換可能であっても、**IIterable\<U\>** と **std::vector\<U\>** は許可されないことに注意してください。**std::vector\<U\>** の場合は、二重反復子バージョン (詳細は後述) を使用できます。

場合によっては、お使いのオブジェクトで必要な **IIterable** が実際に実装されている可能性があります。 たとえば、[**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) によって生成される **IVectorView\<StorageFile\>** では、**IIterable\<StorageFile\>** が実装されています。 ただし、**IIterable\<IStorageItem\>** も実装されています。それを明示的に要求する必要があります。

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

それ以外の場合は、二重反復子バージョンを使用できます。

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

上記のどのシナリオにも適合しないコレクションがある場合、コレクションを反復処理して **T** に変換できるものを生成できるなら、二重反復子でさらに一般的に処理できます。上では、派生型のベクトルを反復処理するためにそれを使用しました。 以下では、派生型の非ベクトルを反復処理するためにそれを使います。

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

反復子が `RandomAcessIt` である場合、[**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) の実装はさらに効率的になります。 それ以外の場合は、範囲に対して複数のパスができます。

|渡すことができる型|同期|Async|メモ|
|-|-|-|-|
| `nullptr` | はい | はい | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IIterable\<IKeyValuePair\<K, V\>\>** | はい | はい | または、それに変換可能なもの。|
| **std::map\<K, V\> const&** | はい | いいえ ||
| **std::map\<K, V\>&&** | はい | はい | 変更を防ぐため、内容は反復子に移動されます。|
| **std::unordered_map\<K, V\> const&** | はい | いいえ ||
| **std::unordered_map\<K, V\>&&** | はい | はい | 変更を防ぐため、内容は反復子に移動されます。|
| **std::initializer_list\<std::pair\<K, V\>\>** | はい | はい | 型 **K** と **V** は、厳密に一致する必要があります。 キーが重複することはできません。 非同期バージョンでは、項目がコピーされます。|
| `{ ForwardIt begin, ForwardIt end }` | はい | いいえ | `begin->first` および `begin->second` はそれぞれ、**K** および **V** に変換できる必要があります。|

## <a name="vector-view-parameters"></a>ベクトル ビューのパラメーター

**winrt::param::vector_view\<T\>** と **winrt::param::async_vector_view\<T\>** では、**IVectorView\<T\>** を受け取る API へのパラメーターの引き渡しが簡略化されます。

[**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) を使って、**IVector** から **IVectorView** を取得できます。

|渡すことができる型|同期|Async|メモ|
|-|-|-|-|
| `nullptr` | はい | はい | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IVectorView\<T\>** | はい | はい | または、それに変換可能なもの。|
| **std::vector\<T\>const&** | はい | いいえ ||
| **std::vector\<T\>&&** | はい | はい | 変更を防ぐため、内容はビューに移動されます。|
| **std::initializer_list\<T\>** | はい | はい | 型は正確に一致する必要があります。 非同期バージョンでは、項目がコピーされます。|
| `{ ForwardIt begin, ForwardIt end }` | はい | いいえ | `*begin` は **T** に変換できる必要があります。|

二重反復子バージョンを使って、直接引き渡す場合の要件に適合しないものから、ベクトル ビューを作成できます。 ただし、ベクトルではランダム アクセスがサポートされているため、`RandomAcessIter` を渡すことをお勧めします。

## <a name="map-view-parameters"></a>マップ ビューのパラメーター

**winrt::param::map_view\<T\>** と **winrt::param::async_map_view\<T\>** では、**IMapView\<T\>** を受け取る API へのパラメーターの引き渡しが簡略化されます。

**IMap::GetView** を使って、**IMap** から **IMapView** を取得できます。

|渡すことができる型|同期|Async|メモ|
|-|-|-|-|
| `nullptr` | はい | はい | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IMapView\<K, V\>** | はい | はい | または、それに変換可能なもの。|
| **std::map\<K, V\> const&** | はい | いいえ ||
| **std::map\<K, V\>&&** | はい | はい | 変更を防ぐため、内容はビューに移動されます。|
| **std::unordered_map\<K, V\> const&**  | はい | いいえ ||
| **std::unordered_map\<K, V\>&&** | はい | はい | 変更を防ぐため、内容はビューに移動されます。|
| **std::initializer_list\<std::pair\<K, V\>\>** | はい | はい | 同期バージョンと非同期バージョンの両方で、項目がコピーされます。 キーが重複してはなりません。|

## <a name="vector-parameters"></a>ベクトルのパラメーター

**winrt::param::vector\<T\>** では、**IVector\<T\>** を受け取る API へのパラメーターの引き渡しが簡略化されます。

|渡すことができる型|メモ|
|-|-|
| `nullptr` | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IVector\<T\>** | または、それに変換可能なもの。|
| **std::vector\<T\>&&** | 変更を防ぐため、内容はパラメーターに移動されます。 結果が元に戻されることはありません。|
| **std::initializer_list\<T\>** | 変更を防ぐため、内容はパラメーターにコピーされます。|

メソッドでベクトルが変更される場合、変更を確認するには、**IVector** を直接渡すしかありません。 **std::vector** を渡す場合、メソッドではコピーが変更され、オリジナルは変更されません。

## <a name="map-parameters"></a>マップのパラメーター

**winrt::param::map\<T\>** では、**IMap\<T\>** を受け取る API へのパラメーターの引き渡しが簡略化されます。

|渡すことができる型|メモ|
|-|-|
| `nullptr` | 基になるメソッドで `nullptr` がサポートされていることを確認する必要があります。|
| **IMap\<T\>** | または、それに変換可能なもの。|
| **std::map\<K, V\>&&** | 変更を防ぐため、内容はパラメーターに移動されます。 結果が元に戻されることはありません。|
| **std::unordered_map\<K, V\>&&** | 変更を防ぐため、内容はパラメーターに移動されます。 結果が元に戻されることはありません。|
| **std::initializer_list\<std::pair\<K, V\>\>** | 変更を防ぐため、内容はパラメーターにコピーされます。|

メソッドでマップが変更される場合、変更を確認するには、**IMap** を直接渡すしかありません。 **std::map** または **std::unordered_map** を渡す場合、メソッドではコピーが変更され、オリジナルは変更されません。

## <a name="array-parameters"></a>配列のパラメーター

**winrt::array_view\<T\>** は **winrt::param** 名前空間内にありませんが、C スタイルの配列&mdash; ("*準拠する配列*" とも呼ばれます) であるパラメーターに使用されます。

|渡すことができる型|メモ|
|-|-|
| `{}` | 空の配列。|
| **array** | C の準拠する配列 (つまり `C array[N];`)。**C** は **T** に変換でき、`sizeof(C) == sizeof(T)` です。 |
| **std::array<C, N>** | **C** の C++ **std::array**。**C** は **T** に変換でき、`sizeof(C) == sizeof(T)` です。 |
| **std::vector<C>** | **C** の C++ **std::vector**。**C** は **T** に変換でき、`sizeof(C) == sizeof(T)` です。 |
| `{ T*, T* }` | ポインターのペアは、範囲 [開始、終了) を表します。|
| **std::initializer_list\<T\>** ||

また、ブログ記事「[Windows ランタイム ABI 境界全体で C スタイルの配列を渡すためのさまざまなパターン](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398)」も参照してください。