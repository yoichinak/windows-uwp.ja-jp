---
Description: Web アプリ間での最適な互換性のためのエンコード、およびその他の utf-8 文字を使用して、* nix ベースのプラットフォーム (Unix、Linux、およびバリアント) のローカリゼーションのバグを最小限に抑えるし、テストのオーバーヘッドを削減します。
title: Windows utf-8 コード ページを使用します。
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, グローバリゼーション, ローカライズの可否, ローカライズ
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141806"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8 コード ページの使用

使用[utf-8](http://www.utf-8.com/) web アプリおよびその他の間の最適な互換性のためのエンコード文字 * nix ベースのプラットフォーム (Unix、Linux、およびバリアント) のローカリゼーションのバグを最小限に抑えるし、テストのオーバーヘッドを削減します。

Utf-8 では、国際化対応のユニバーサル コード ページは、し、1 ~ 4 バイトの可変幅のエンコーディングを使用してすべての Unicode コード ポイントをサポートします。 Web テクノロジーを広範囲に使用されるの既定値は、* nix ベースのプラットフォームです。

## <a name="-a-vs--w-apis"></a>-W Api とは
  
Win32 Api は、多くの場合- と -W のバリエーションをサポートします。

は、バリアントは、システムとサポートで構成されている ANSI コード ページを認識`char*`-w バリアントが utf-16 とサポートで動作中に、`WCHAR`します。

最近まで、Windows が強調表示される"Unicode"-w バリアント-a Api 経由します。 ただし、最近のリリースは、ANSI コード ページを使用して、-、utf-8 を導入するための手段として Api アプリをサポートします。 Utf-8 の ANSI コード ページを構成する場合-Api 動作を utf-8 でします。 このモデルでは、-a Api コード変更なしで構築された既存のコードをサポートするという利点があります。

## <a name="set-a-process-code-page-to-utf-8"></a>プロセスのコード ページを utf-8 に設定します。

Windows バージョン 1903 (2019 の更新の可能性があります)、使えば ActiveCodePage プロパティ、appxmanifest でパッケージ化されたアプリの場合、またはアプリのパッケージ化されていない場合、fusion マニフェストとしてプロセスのコード ページ utf-8 を使用するプロセスを強制的にできます。

このプロパティを宣言することができ、ビルドを以前の Windows では、ターゲット/実行が通常どおりレガシ コード ページの検出と変換を処理する必要があります。 Windows バージョン 1903 の最小ターゲット バージョンでは、レガシ コード ページの検出と変換を回避できるように、プロセスのコード ページは utf-8 を必ずします。

## <a name="examples"></a>使用例

**パッケージ アプリの Appx マニフェスト:**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Fusion のマニフェストをパッケージ化されていない Win32 アプリ:**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> 既存の実行可能ファイルを使用してコマンドラインからマニフェストを追加します。 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>コード ページ変換

Utf-16 でネイティブ Windows よう (`WCHAR`)、Windows Api との相互運用に utf-16 (またはその逆) に、utf-8 データを変換する必要があります。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)と[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) let utf-8 と utf-16 との間で変換する (`WCHAR`) (およびその他のコード ページ)。 これは特に便利です、従来の Win32 API が認識のみ`WCHAR`します。 これらの関数では、utf-8 入力に変換できます。 `WCHAR` 、-w API に渡すと、必要な場合に戻り、結果を変換します。
これらの関数を使用する場合`CodePage`に設定`CP_UTF8`を使用して、`dwFlags`いずれかの`0`または`MB_ERR_INVALID_CHARS`、それ以外の場合、`ERROR_INVALID_FLAGS`に発生します。

注:`CP_ACP`に割り当てられる総合`CP_UTF8`または上記の Windows バージョンの 1903 (2019 の更新の可能性があります) で実行されているし、上記で説明した ActiveCodePage プロパティが utf-8 に設定されている場合のみです。 それ以外の場合、レガシ システムのコード ページが考慮されます。 使用することをお勧めします。`CP_UTF8`明示的にします。

## <a name="related-topics"></a>関連トピック

- [コード ページ](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [コード ページの識別子](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
