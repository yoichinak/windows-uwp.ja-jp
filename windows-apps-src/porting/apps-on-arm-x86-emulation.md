---
title: ARM での x86 および ARM32 エミュレーションのしくみ
description: ARM での x86 アプリのエミュレーションの概要。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 常時接続, ARM での x86 エミュレーション
ms.localizationpriority: medium
ms.openlocfilehash: 8af6ba39468dd16a043040b797a03c862d6ce7db
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319658"
---
# <a name="how-x86-emulation-works-on-arm"></a>ARM での x86 エミュレーションのしくみ
x86 アプリのエミュレーションにより、Win32 アプリのリッチなエコシステムが ARM で利用できるようになります。 これにより、アプリに変更を加えなくても、ユーザーが既存の x86 win32 アプリを問題なく実行できるようになります。 アプリは、固有の API ([IsWoW64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)) を呼び出すことを除いて、ARM PC の Windows で実行されていることを認識することもありません。

Windows 10 の [WOW64](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications) レイヤーを使うと、ARM64 バージョンの Windows 10 で x86 コードを実行できます。 x86 エミュレーションは、パフォーマンスを高める最適化によって x86 命令のブロックを ARM64 命令にコンパイルすることで機能します。 サービスは、変換されたこれらのコード ブロックをキャッシュして命令変換のオーバーヘッドを減らし、コードをもう一度実行するときの最適化を実現します。 キャッシュはモジュールごとに生成されるため、他のアプリが初回起動時にそれらを使うことができます。 

これらのテクノロジについて詳しくは、[ARM 版 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) Channel9 ビデオをご覧ください。 