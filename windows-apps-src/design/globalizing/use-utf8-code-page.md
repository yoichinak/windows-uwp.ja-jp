---
Description: Web アプリ間での最適な互換性のためのエンコード、およびその他の utf-8 文字を使用して、* nix ベースのプラットフォーム (Unix、Linux、およびバリアント) のローカリゼーションのバグを最小限に抑えるし、テストのオーバーヘッドを削減します。
title: Windows utf-8 コード ページを使用します。
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, グローバリゼーション, ローカライズの可否, ローカライズ
ms.localizationpriority: medium
ms.openlocfilehash: 453d58b0d52aaa24461784b6f393b26b93e572a1
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827378"
---
# <a name="use-the-utf-8-code-page"></a>Utf-8 コード ページを使用します。

使用[utf-8](http://www.utf-8.com/) web アプリおよびその他の間の最適な互換性のためのエンコード文字 * nix ベースのプラットフォーム (Unix、Linux、およびバリアント) のローカリゼーションのバグを最小限に抑えるし、テストのオーバーヘッドを削減します。

Utf-8 では、国際化対応のユニバーサル コード ページは、し、1 ~ 4 バイトの可変幅のエンコーディングを使用してすべての Unicode コード ポイントをサポートします。 Web テクノロジーを広範囲に使用されるの既定値は、* nix ベースのプラットフォームです。

## <a name="-a-vs--w-apis"></a>-W Api とは
  
Win32 Api は、多くの場合- と -W のバリエーションをサポートします。

は、バリアント-w バリアントが utf-16 とサポートで動作中に、システムおよびサポート char * で構成されている ANSI コード ページを認識する`WCHAR`します。

最近まで、Windows が強調表示される"Unicode"-w バリアント-a Api 経由します。 ただし、最近のリリースは、ANSI コード ページを使用して、-、utf-8 を導入するための手段として Api アプリをサポートします。 Utf-8 の ANSI コード ページを構成する場合-Api 動作を utf-8 でします。 このモデルでは、-a Api コード変更なしで構築された既存のコードをサポートするという利点があります。

## <a name="set-a-process-code-page-to-utf-8"></a>プロセスのコード ページを utf-8 に設定します。

パッケージのアプリ用の appxmanifest ファイルまたは ActiveCodePage プロパティを使用してパッケージ化されていないアプリの fusion マニフェストを介してプロセス コード ページ utf-8 を使用するプロセスを強制することができます。

このプロパティを宣言することができ、ビルドを以前の Windows では、ターゲット/実行が通常どおりレガシ コード ページの検出と変換を処理する必要があります (19 時間 1 の最小ターゲット バージョンでプロセスのコード ページが必ず utf-8)。

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

Windows の動作で utf-16 (WCHAR) ネイティブ、Windows Api との相互運用に utf-16 (またはその逆) に、utf-8 データを変換することが必要になります。

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar)と[WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) utf-8 と utf-16 (WCHAR) (およびその他のコード ページ) の間で変換するようにします。 これは、従来の Win32 API 可能性がありますのみ WCHAR を理解できる場合に特に便利です。 これらの関数を使用すると、utf-8 入力、-w API に渡すし、必要な場合に戻り、結果を変換する WCHAR に変換できます。
コードページ CP_UTF8、0 または MB_ERR_INVALID_CHARS のいずれかの使用 dwFlags での Windows でこれらの関数を使用する場合はそれ以外の場合、ERROR_INVALID_FLAGS が発生します。

注:Windows のバージョン 1903 (2019 の更新の可能性があります) で実行されている場合にのみに、CP_ACP が CP_UTF8 に相当し、上記で説明した ActiveCodePage プロパティが utf-8 に設定します。 それ以外の場合、レガシ システムのコード ページが考慮されます。 CP_UTF8 を明示的に使用することをお勧めします。

## <a name="related-topics"></a>関連トピック

- [コード ページ](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [コード ページの識別子](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
