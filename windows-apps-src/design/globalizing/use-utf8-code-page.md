---
Description: UTF-8 文字エンコードを使用して、web アプリとその他の \*nix ベースのプラットフォーム (Unix、Linux、およびバリアント) 間の最適な互換性を確保し、ローカリゼーションのバグを最小限に抑え、テストのオーバーヘッドを削減します。
title: Windows UTF-8 コードページを使用する
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, グローバリゼーション, ローカライズの可否, ローカライズ
ms.localizationpriority: medium
ms.openlocfilehash: 4b4050dfea1589fbe79db08061bcc56e392173f1
ms.sourcegitcommit: 13ce25364201223e21e2e5e89f99bc7aa4d93f56
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847596"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8 コード ページの使用

[Utf-8](http://www.utf-8.com/)文字エンコードを使用して、web アプリとその他の \*nix ベースのプラットフォーム (Unix、Linux、およびバリアント) 間の最適な互換性を確保し、ローカリゼーションのバグを最小限に抑え、テストのオーバーヘッドを削減します。

UTF-8 は国際化のためのユニバーサルコードページであり、Unicode 文字セット全体をエンコードできます。 これは web で pervasively に使用され、* nix ベースのプラットフォームの既定値です。

> [!NOTE]
> エンコードされた文字は、1 ~ 4 バイトになります。 UTF-8 エンコーディングでは、最大6バイトのより長いバイトシーケンスがサポートされますが、Unicode 6.0 (U + 10FFFF) の最大のコードポイントは4バイトしか必要としません。

## <a name="-a-vs--w-apis"></a>-A vs.-W Api
  
Win32 Api は、多くの場合、-A と-W の両方のバリアントをサポートします。

-A バリアントは、システム上で構成された ANSI コードページを認識し、`char*`をサポートします。また、-W バリアントは UTF-16 で動作し、`WCHAR`をサポートします。

最近まで、Windows では "Unicode" -W のバリアントを -A API よりも強調してきました。 ただし、最近のリリースでは、アプリに UTF-8 のサポートを導入する手段として ANSI コードページと-Api が使用されています。 ANSI コードページが UTF-8 用に構成されている場合、Api は UTF-8 で動作します。 このモデルには、コードを変更することなく、-A Api でビルドされた既存のコードをサポートするという利点があります。

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
> コマンドラインから既存の実行可能ファイルにマニフェストを追加 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>コードページの変換

Windows が UTF-16 (`WCHAR`) でネイティブに動作するため、Windows Api と相互運用するには、UTF-8 データを UTF-16 (またはその逆) に変換する必要がある場合があります。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)と[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte)を使用すると、utf-8 と utf-16 (`WCHAR`) (およびその他のコードページ) 間で変換を行うことができます。 これは、レガシ Win32 API が `WCHAR`を理解する必要がある場合に特に便利です。 これらの関数を使用すると、UTF-8 入力を `WCHAR` に変換して-W API に渡し、必要に応じて結果を変換できます。
これらの関数を `CodePage` を `CP_UTF8`に設定して使用する場合は、`0` または `MB_ERR_INVALID_CHARS`の `dwFlags` を使用します。それ以外の場合は、`ERROR_INVALID_FLAGS` が発生します。

> [!NOTE]
> `CP_ACP` は、Windows バージョン 1903 (2019 更新プログラム) 以降で実行されていて、上記の ActiveCodePage プロパティが UTF-8 に設定されている場合にのみ `CP_UTF8` に相当します。 それ以外の場合は、従来のシステムコードページが受け入れられます。 `CP_UTF8` を明示的に使用することをお勧めします。

## <a name="related-topics"></a>関連トピック

- [コードページ](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [コードページ識別子](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
