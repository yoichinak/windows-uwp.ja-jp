---
description: アジャイル オブジェクトは、いずれかのスレッドからアクセスできます。 C#/WinRT では、安全な方法でアパートメント間で非アジャイルオブジェクトをマーシャリングする必要がある場合に、アジャイル参照がサポートされます。
title: C#/WinRT を使用したアジャイルオブジェクト
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355233"
---
# <a name="agile-objects-in-cwinrt"></a>C#/WinRT のアジャイルオブジェクト

ほとんどの Windows ランタイムクラスは *アジャイル* です。つまり、さまざまなアパートメント間の任意のスレッドからアクセスできます。 作成した C#/WinRT 型は既定でアジャイルであるため、これらの型に対してこの動作を無効にすることはできません。

ただし、予測される C#/WinRT 型 (Windows SDK と WinUI ライブラリによって提供される Windows ランタイムの型を含む) は、アジャイルである場合もあれば、そうでない場合もあります。 たとえば、UI オブジェクトを表す多くの型はアジャイルではありません。 非アジャイル型を使用する場合は、スレッドモデルとマーシャリング動作を考慮に入れる必要があります。 C#/WinRT では、安全な方法でアパートメント間で非アジャイルオブジェクトをマーシャリングする必要がある場合に、アジャイル参照がサポートされます。

> [!NOTE]
> Windows ランタイムは COM に基づいています。 COM の用語では、アジャイル クラスは `ThreadingModel` = *両方* に登録されています。 COM スレッド モデルとアパートメントの詳細については、「[COM スレッド モデルの理解と使用](/previous-versions/ms809971(v=msdn.10))」をご覧ください。

## <a name="check-for-agile-support"></a>アジャイルサポートを確認する

Windows ランタイムオブジェクトがアジャイルであるかどうかを確認するには、次のコードを使用して、オブジェクトが [Iagileobject](/windows/desktop/api/objidl/nn-objidl-iagileobject) インターフェイスをサポートしているかどうかを確認します。

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>アジャイル参照を作成する

非アジャイルオブジェクトのアジャイル参照を作成するには、拡張メソッドを使用でき `AsAgile` ます。 `AsAgile` は、射影された任意の C#/WinRT 型に適用できる汎用的な拡張メソッドです。 型が射影された型でない場合は、例外がスローされます。 Windows SDK の非アジャイル型である [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) オブジェクトの使用例を次に示します。

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

これで `agileReference` 、別のアパートメントのスレッドにを渡して、そこで使用できるようになりました。

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```
