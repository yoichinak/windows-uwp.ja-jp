---
title: プログラム互換性のトラブルシューティング ツール (ARM)
author: msatranjr
description: アプリが ARM で正しく動作しない場合に互換性の設定を調整するためのガイダンス
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 s, 常時接続, 互換性トラブルシューティング ツール, ARM 版 Windows
ms.localizationpriority: medium
ms.openlocfilehash: d517b2e455f6ad75d3158c7899ad70a774cabc46
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595163"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>プログラム互換性のトラブルシューティング ツール (ARM)
x86 アプリが ARM64 上の Windows 10 用に作成された新しい機能をサポートするようにエミュレーションします。 エミュレーションは、最適なエクスペリエンスの結果の最適化を実行することがあります。 プログラム互換性のトラブルシューティング ツールを使って x86 アプリのエミュレーション設定を切り替えることができるため、既定の最適化が減少し、互換性が強化される可能性があります。

## <a name="start-the-program-compatibility-troubleshooter"></a>プログラム互換性のトラブルシューティング ツールの起動
[プログラム互換性のトラブルシューティング ツール](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible)は、どの Windows 10 PC でも同じ方法によって手動で起動できます。実行可能ファイル (.exe) を右クリックし、**[互換性のトラブルシューティング]** を選びます。 この画面が表示されます。

![トラブルシューティングの互換性オプションのスクリーンショット](images/arm/Capture4.png)

**[問題のトラブルシューティング]** をクリックすると、次のオプションが表示されます。

![トラブルシューティングの互換性オプションのスクリーンショット](images/arm/Capture5.png)

すべてのオプションによって、すべての Windows 10 デスクトップ PC に適用可能または適用される設定が有効になります。 さらに、1 番目、2 番目、4 番目のオプションは、[アプリケーション キャッシュの無効化](#disable-app-cache)および[ハイブリッド実行モードも無効化](#disable-hybrid-exec-mode)エミュレーション設定を適用します。

## <a name="toggling-emulation-settings"></a>エミュレーション設定の切り替え
> [!WARNING]
> エミュレーション設定を変更すると、アプリケーションが予期せずクラッシュしたり、まったく起動しない可能性があります。

エミュレーション設定は、実行可能ファイルを右クリックして **[プロパティ]** を選ぶことで切り替えることができます。

ARM では、**[Windows 10 on ARM]** (ARM 版 Windows 10) というセクションが **[互換性]** タブに表示されます。**[Change emulation settings]** (エミュレーション設定の変更) をクリックし、ここに示すように 2 番目のウィンドウを開きます。

![エミュレーション設定の変更のスクリーンショット](images/arm/Capture.png)

このウィンドウでは、2 つの方法でエミュレーション設定を変更できます。 エミュレーション設定の定義済みグループを選ぶか、**[Use advanced settings]** (詳細設定の使用) オプションをクリックして個々の設定の選択を有効にできます。

グループ化されたエミュレーション設定により、品質を優先してパフォーマンスの最適化が低減されます。 選択できるグループ化された設定をいくつか以下に示します。

![エミュレーション設定の変更のスクリーンショット 2](images/arm/Capture2.png)

**[Use advanced settings]** (詳細設定の使用) を選び、この表で説明されているように個々の設定を選びます。

| エミュレーション設定 | 結果 |
| ----------------- | ----------- |
| <p id="disable-app-cache">アプリケーション キャッシュの無効化</p> | オペレーティング システムがコードのコンパイルされたブロックをキャッシュし、後続の実行のオーバーヘッドを減らします。 この設定では、エミュレーターが実行時にすべてのアプリ コードを再コンパイルする必要があります。 |
| <p id="disable-hybrid-exec-mode">ハイブリッド実行モードの無効化</p> | Compiled Hybrid Portable Executable (CHPE) バイナリは、パフォーマンスを高めるためにネイティブ ARM64 コードが含まれる x86 互換バイナリですが、一部のアプリとは互換性がない可能性があります。 この設定では、x86 のみのバイナリの使用が強制されます。 |
| 厳密な自己修正コードのサポート | 自己修正コードがエミュレーションで正しくサポートされるようにするには、これを有効にします。 最も一般的な自己修正コード シナリオは、既定のエミュレーター動作でカバーされます。 このオプションを有効にすると、実行時に自己修正コードのパフォーマンスが大幅に低下します。 |
| RWX ページ パフォーマンスの最適化の無効化 | この最適化によって、読み取り可能、書き込み可能、実行可能な (RWX) ページのコードのパフォーマンスが向上しますが、一部のアプリとの互換性がない可能性があります。 |

次のように、マルチコア設定を選択することもできます。

![マルチコア設定のスクリーン ショット](images/arm/Capture3.png)

これらの設定によって、エミュレーション中にアプリのコア間のメモリー アクセスを同期するために使用されるメモリ バリアの数が変更されます。 **[Fast]** が既定のモードですが、**[strict]** (厳格) オプションと **[very strict]** (非常に厳格) オプションではバリアの数が増加します。 これによりアプリが遅くなりますが、アプリ エラーのリスクが軽減されます。 **[single-core]** (シングル コア) オプションでは、すべてのバリアがなくなりますが、すべてのアプリ スレッドが強制的にシングル コアで実行されます。

特定の設定を変更して問題が解決した場合、詳細を記載したメールを *woafeedback@microsoft.com* にお送りいただければフィードバックが組み込まれます。