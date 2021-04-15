---
title: IWindowNative.WindowHandle プロパティ
description: 要求されたウィンドウの HWND を取得する WinUI COM プロパティ。
ms.topic: reference
ms.date: 03/09/2021
keywords: WinUI、Windows UI ライブラリ
ms.openlocfilehash: 9c8278b5b19f4e7eeee447ed4a4261064f36194d
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730667"
---
# <a name="iwindownativewindowhandle-property-microsoftuixamlwindowh"></a>IWindowNative.WindowHandle プロパティ (microsoft.ui.xaml.window.h)

ウィンドウの要求されたハンドルを取得します。

## <a name="syntax"></a>構文

<!--
[
    object,
    uuid( EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB ),
    local,
    pointer_default(unique)
]
interface IWindowNative: IUnknown
{
    [propget] HRESULT WindowHandle([out, retval] HWND* hWnd);
};
-->

```cpp
HRESULT WindowHandle(
  HWND* hWnd
);
```

## <a name="parameters"></a>パラメーター

*hWnd* [out]

型: **HWND***

ウィンドウへのハンドル。

## <a name="return-value"></a>戻り値

型: HRESULT

このメソッドは、成功すると S_OK を返します。 成功しなかった場合は、HRESULT エラー コードを返します。

## <a name="remarks"></a>解説

## <a name="examples"></a>例

次の例を試す前に、次のトピックをご確認ください。

- デスクトップ用の WinUI 3 プロジェクト テンプレートを使用するには、開発用コンピューターを構成し、[開発環境を設定](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)します。
- 「[デスクトップ アプリ用の WinUI 3 の概要](../winui3/get-started-winui3-for-desktop.md)」で説明に従って初期テンプレート アプリを作成および実行することにより、開発環境が期待どおりに機能していることを確認します。

### <a name="customized-window-icon"></a>カスタマイズされたウィンドウ アイコン

次の例では、初期 **WinUI in Desktop C#/.NET 5** テンプレート コードについてまず説明し、**WindowHandle** を使用して、アプリ ウィンドウに使用されるアイコンをカスタマイズする方法について説明します。

#### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

1. まず、ディレクティブを使用して以下を追加します。

    - [System.Runtime.InteropServices](/dotnet/api/system.runtime.interopservices): COM 相互運用およびプラットフォーム呼び出しサービスのサポートを提供します。 この例では、PInvoke 機能に必要になります。
    - [WinRT](/uwp/cpp-ref-for-winrt/winrt): C++/WinRT に属するカスタム データ型を提供します。 この例では、IWindowNative COM インターフェイスに必要になります。

    ```csharp
    using System.Runtime.InteropServices;
    using WinRT;
    ```

1. 次に、.ico ファイルをプロジェクト ("Images/windowIcon.ico") に追加し、このファイルの [ビルド アクション] (ファイルを右クリックし、[プロパティ] を選択) を [コンテンツ] に設定します。

1. MainWindow メソッドで、前の手順で追加した .ico ファイルへの参照を使用して、`LoadIcon("Images/windowIcon.ico");` 関数への呼び出しを追加します (次の手順を参照)。

    ```csharp
    public MainWindow()
    {
        LoadIcon("Images/windowIcon.ico");
    
        this.InitializeComponent();
    }
    ```

1. 次に、アプリケーションへのハンドルを取得し、[LoadImage](/windows/win32/api/winuser/nf-winuser-loadimagew) や [SendMessage](/windows/win32/api/winuser/nf-winuser-sendmessage) などのさまざまな [PInvoke](/dotnet/standard/native-interop/pinvoke) 機能を使用してアプリケーション アイコンを設定する `LoadIcon(string iconName)` 関数を追加します。

    ```csharp
    private void LoadIcon(string iconName)
    {
        //Get the Window's HWND
        var hwnd = this.As<IWindowNative>().WindowHandle;
    
        IntPtr hIcon = PInvoke.User32.LoadImage(
            IntPtr.Zero, 
            iconName,
            PInvoke.User32.ImageType.IMAGE_ICON, 
            16, 16, 
            PInvoke.User32.LoadImageFlags.LR_LOADFROMFILE);
    
        PInvoke.User32.SendMessage(
            hwnd, 
            PInvoke.User32.WindowMessage.WM_SETICON, 
            (IntPtr)0, 
            hIcon);
    }    
    ```

1. 最後に、パブリック IWindowNative インターフェイスから型情報を埋め込み、ランタイム アセンブリからクラスのインスタンスを作成します。 詳細については、[マネージド アセンブリからの型の埋め込み](/dotnet/standard/assembly/embed-types-visual-studio)に関するページを参照してください。

    ```csharp
    [ComImport]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB")]
    internal interface IWindowNative
    {
        IntPtr WindowHandle { get; }
    }
    ```

1. 独自のアプリでこれらの手順に従っている場合は、そのアプリをビルドして実行します。 次のようなアプリケーション ウィンドウが、カスタム アプリ アイコンと共に表示されるはずです。

    :::image type="content" source="../winui3/images/build-basic/template-app-windowhandle.png" alt-text="カスタム アプリケーション アイコンを使用したテンプレート アプリ。":::<br/>*カスタム アプリケーション アイコンを使用したテンプレート アプリ。*

## <a name="applies-to"></a>適用対象

| 製品 | バージョン |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5、3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>関連項目

[IWindowNative インターフェイス](iwindownative.md)
