---
title: IWindowNative インターフェイス
description: XAML とネイティブ ウィンドウの間の相互運用を提供する WinUI COM インターフェイス。
ms.topic: reference
ms.date: 03/09/2021
keywords: WinUI、Windows UI ライブラリ
ms.openlocfilehash: e641ab73a87e993fddb3f6aa8b90a0149744b99f
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730660"
---
# <a name="iwindownative-interface-microsoftuixamlwindowh"></a>IWindowNative インターフェイス (microsoft.ui.xaml.window.h)

XAML とネイティブ ウィンドウの間の相互運用を可能にします。

このインターフェイスは [Window](/windows/winui/api/microsoft.ui.xaml.window) によって実装され、デスクトップ アプリでこれを使用することにより、そのウィンドウの基になる HWND を取得できます。

## <a name="inheritance"></a>継承

IWindowNative インターフェイスは IUnknown インターフェイスから継承します。 IWindowNative には、次の種類のメンバーも用意されています。

- [Properties](#properties)

## <a name="properties"></a>プロパティ

IWindowNative には、次のプロパティが用意されています。

| プロパティ | 説明 |
| --- | --- |
| [IWindowNative::WindowHandle](iwindownative-windowhandle.md) | 要求されたウィンドウの HWND を取得します。 |

## <a name="applies-to"></a>適用対象

| 製品 | バージョン |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5、3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>関連項目

[Windows UI ライブラリ (WinUI)](../index.md)