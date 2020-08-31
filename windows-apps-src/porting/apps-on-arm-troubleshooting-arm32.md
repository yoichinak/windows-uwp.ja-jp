---
title: ARM32 UWP アプリのトラブルシューティング
description: ARM で実行する際の ARM32 アプリの一般的な問題とその解決方法。
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, 常時接続, ARM での ARM32 アプリ, ARM 版 windows 10, トラブルシューティング
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171226"
---
# <a name="troubleshooting-arm-uwp-apps"></a>ARM UWP アプリのトラブルシューティング

ARM32 または ARM64 UWP アプリが ARM で正常に動作していない場合は、次のガイダンスを参考にしてください。

>[!NOTE]
> ARM64 プラットフォームをネイティブでターゲットとする UWP アプリケーションをビルドするには、Visual Studio 2017 バージョン15.9 以降、または Visual Studio 2019 が必要です。 詳細については、[このブログ投稿](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)を参照してください。


## <a name="common-issues"></a>一般的な問題
ARM32 と ARM64 アプリのトラブルシューティングを行う際に注意すべき一般的な問題を次に示します。

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>ARM ベースのプロセッサでの Windows 10 Mobile 専用 API の使用
ARM アプリは、モバイル専用 Api (たとえば、 **ハードウェアボタン**) を使用しているときに問題が発生する可能性があります。 これを軽減するには、それらの API を呼び出す前に Windows 10 Mobile でアプリが実行されているかどうかを動的に検出します。 [API コントラクトを使った機能の動的な検出](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)に関するブログ投稿のガイダンスに従います。

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>UWP アプリによりサポートされていない依存関係の追加
Visual Studio と UWP SDK で正しくビルドされていないユニバーサル Windows プラットフォーム (UWP) アプリは、ARM64 システムで実行されている ARM アプリで使用できない OS コンポーネントに依存している可能性があります。 そのような依存関係の例は、次のとおりです。

- .NET Framework の一部が使用可能であることが期待される。
- UWP と互換性のないサード パーティの .NET コンポーネントを参照している。

これらの問題は、最新の Microsoft Visual Studio および UWP SDK バージョンを使用して、使用できない依存関係を削除し、アプリを再構築することによって解決できます。または、最後の手段として、Microsoft Store から ARM アプリを削除して、アプリの x86 バージョン (利用可能な場合) がユーザーの Pc にダウンロードされるようにします。

UWP アプリに使用可能な .NET API について詳しくは、「[UWP アプリの .NET](/dotnet/api/index?view=dotnet-uwp-10.0)」をご覧ください。

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>以前のバージョンの Visual Studio と SDK を使ったアプリのコンパイル
問題が発生した場合、最新バージョンの Microsoft Visual Studio と Windows SDK を使ってアプリをコンパイルしていることを確認します。 以前のバージョンの Visual Studio と SDK でコンパイルされたアプリでは、以降のバージョンで修正された問題が発生する可能性があります。

## <a name="debugging"></a>デバッグ
ARM プラットフォーム用のアプリを開発するために、既存のツールを使用できます。 便利なリソースを次に示します。

- Visual Studio 15.5 Preview 1 以降では、ユニバーサル認証モードを使った ARM32 アプリの実行がサポートされます。 これにより、必要なリモート デバッグ ツールが自動的にブートストラップされます。
- ARM でデバッグを行うためのツールと戦略について詳しくは、[ARM64 でのデバッグに関するページ](/windows-hardware/drivers/debugger/debugging-arm64)をご覧ください。