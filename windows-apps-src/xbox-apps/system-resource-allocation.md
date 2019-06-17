---
title: Xbox One 上の UWP アプリとゲームのシステム リソース
description: Xbox 上の UWP のシステム リソース
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8cc6ca24453be83f5c10cc6c86c508a5a3f99c4c
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131928"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Xbox One 上の UWP アプリとゲームのシステム リソース

Xbox One で実行されている UWP アプリは、システムやその他のアプリとリソースを共有します。 Xbox One アプリの UWP で利用可能なリソースは、アプリとして提出するか、Xbox Live クリエーターズ プログラム ゲームとして提出するかによって異なります。

* フォアグラウンドで実行されているときに利用可能な最大メモリ
    * アプリ:1 GB
    * ゲーム:5 GB

バックグラウンドで実行されているアプリに利用可能なメモリは最大 128 MB です。 バックグラウンド モードは、バックグラウンド ミュージック プレーヤーなどの同時アプリケーションにのみ適用されます。  ゲームはバックグラウンドで一時停止および終了されます。

これらの制限を超えると、メモリ割り当てエラーが発生します。 メモリ使用量の監視について詳しくは、[MemoryManager クラス](https://docs.microsoft.com/uwp/api/windows.system.memorymanager)のリファレンスをご覧ください。

> [!NOTE]
> Visual Studio デバッガーからアプリやゲームを実行するときに、これらのメモリの制約は適用されません。 この制限は、デバッグ モードで実行されていないときにのみ適用されます。

* CPU
    * アプリ: システムで実行されているアプリとゲームの数に応じて、2 ～ 4 個の CPU コアを共有します。
    * ゲーム:排他的と 2 つの 4 CPU コアを共有します。

* GPU
    * アプリ: システムで実行されているアプリとゲームの数に応じて、GPU の 45% を共有します。
    * ゲーム: 利用可能な GPU サイクルへのフル アクセス。

* DirectX のサポート
    * アプリ:DirectX 11 の機能レベル 10 です。
    * ゲーム:DirectX 12、DirectX 11 の機能レベル 10.

* アプリおよびゲームを Xbox 向けに開発または Microsoft Store に提出するには、必ず x64 アーキテクチャをターゲットにする必要があります。  

**アプリケーション開発**の場合、利用可能なリソースは標準 PC と比べて限られることがあり、システムで実行されているアプリやゲームの数によって異なる場合があります。

**ゲーム開発**の場合、Xbox One は、他のゲーム コンソールと同様に、その潜在的な機能を最大限に利用するために特定のハードウェア ベースの開発キットを必要とする、特殊なハードウェアです。 Xbox One のハードウェアの機能を最大限に利用する必要があるゲームを開発している場合、[ID@Xbox](https://www.xbox.com/Developers/id) プログラムに登録して Xbox One 開発キットにアクセスすることを検討してください。

## <a name="see-also"></a>関連項目
- [Xbox One の UWP](index.md)
- [Xbox Live クリエーターズ プログラムを概要します。](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX および Xbox One での UWP](https://walbourn.github.io/)