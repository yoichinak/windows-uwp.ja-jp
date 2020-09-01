---
Description: UTF-8 文字エンコードを使用して、web アプリとその他の \* nix ベースプラットフォーム (Unix、Linux、およびバリアント) 間の最適な互換性を確保し、ローカリゼーションのバグを最小限に抑え、テストのオーバーヘッドを削減します。
title: Windows UTF-8 コードページを使用する
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, グローバリゼーション, ローカライズの可否, ローカライズ
ms.localizationpriority: medium
ms.openlocfilehash: 72e422ee3e1a911658b2fe4957967aeba116c353
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173466"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8 コード ページの使用

[Utf-8](http://www.utf-8.com/)文字エンコードを使用して、web アプリとその他の \* nix ベースプラットフォーム (Unix、Linux、およびバリアント) 間の最適な互換性を確保し、ローカリゼーションのバグを最小限に抑え、テストのオーバーヘッドを削減します。

UTF-8 は国際化のためのユニバーサルコードページであり、Unicode 文字セット全体をエンコードできます。 これは web で pervasively に使用され、* nix ベースのプラットフォームの既定値です。

> [!NOTE]
> エンコードされた文字は、1 ~ 4 バイトになります。 UTF-8 エンコーディングでは、最大6バイトのより長いバイトシーケンスがサポートされますが、Unicode 6.0 (U + 10FFFF) の最大のコードポイントは4バイトしか必要としません。

## <a name="-a-vs--w-apis"></a>-A vs.-W Api
  
Win32 Api は、多くの場合、-A と-W の両方のバリアントをサポートします。

-バリアントは、システムおよびサポートに構成されている ANSI コードページを認識し `char*` 、-W バリアントは utf-16 およびサポートで動作 `WCHAR` します。

最近までは、Windows は "Unicode"-W のバリエーションを Api 経由で強調してきました。 ただし、最近のリリースでは、アプリに UTF-8 のサポートを導入する手段として ANSI コードページと-Api が使用されています。 ANSI コードページが UTF-8 用に構成されている場合、Api は UTF-8 で動作します。 このモデルには、コードを変更することなく、-A Api でビルドされた既存のコードをサポートするという利点があります。

## <a name="set-a-process-code-page-to-utf-8"></a>プロセスコードページを UTF-8 に設定する

Windows バージョン 1903 (2019 更新プログラム) では、パッケージアプリの package.appxmanifest の ActiveCodePage プロパティまたはパッケージ化されていないアプリの fusion マニフェストを使用して、プロセスがプロセスコードページとして UTF-8 を使用するように強制することができます。

このプロパティを宣言し、以前の Windows ビルドでターゲット/実行できますが、従来のコードページの検出と変換は通常どおりに処理する必要があります。 Windows バージョン1903の最小ターゲットバージョンでは、プロセスコードページは常に UTF-8 になるため、レガシコードページの検出と変換を回避できます。

## <a name="examples"></a>例

**パッケージアプリの Appx マニフェスト:**

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

**アンパッケージ Win32 アプリの Fusion マニフェスト:**

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
> コマンドラインから既存の実行可能ファイルにマニフェストを追加します。 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>コードページの変換

Windows が UTF-16 () でネイティブに動作するため `WCHAR` 、Windows api と相互運用するには、utf-8 データを utf-16 (またはその逆) に変換する必要がある場合があります。

[MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) と [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) を使用すると、utf-8 と utf-16 ( `WCHAR` ) (およびその他のコードページ) 間で変換を行うことができます。 これは、レガシ Win32 API が理解するだけの場合に特に便利です `WCHAR` 。 これらの関数を使用すると、UTF-8 入力をに変換し `WCHAR` て-W API に渡し、必要に応じて結果を変換できます。
をに設定してこれらの関数を使用する場合 `CodePage` `CP_UTF8` は、 `dwFlags` またはのいずれかを使用し `0` ます。それ以外の場合は、 `MB_ERR_INVALID_CHARS` `ERROR_INVALID_FLAGS` が発生します。

> [!NOTE]
> `CP_ACP` は、 `CP_UTF8` Windows バージョン 1903 (2019 更新プログラム) 以降で実行されていて、上で説明した ActiveCodePage プロパティが utf-8 に設定されている場合にのみとなります。 それ以外の場合は、従来のシステムコードページが受け入れられます。 を明示的に使用することをお勧めし `CP_UTF8` ます。

## <a name="related-topics"></a>関連トピック

- [コード ページ](/windows/desktop/Intl/code-pages)
- [コードページ識別子](/windows/desktop/Intl/code-page-identifiers)