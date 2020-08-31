---
title: .NET トレース処理とは-.NET TraceProcessing
description: この概要では、.NET のトレース処理について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 03f88d6bd1ac150d94f73f9f9177249c3b5ae78f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170556"
---
# <a name="process-etw-traces-in-net"></a>.NET での ETW トレースの処理

[Windows イベントトレーシング (ETW)](/windows/win32/etw/event-tracing-portal) は、Windows オペレーティングシステムに組み込まれている強力なトレースコレクションシステムです。 Windows には、ETW との緊密な統合があります。これには、コンテキストの切り替え、メモリの割り当て、プロセスの作成と終了などのイベントについて、システムの動作に関するデータが含まれます。 ETW から利用できるシステム全体のデータを使用すると、エンドツーエンドのパフォーマンス分析や、システム全体のさまざまなコンポーネント間の相互作用を調べる必要があるその他の質問に適したものになります。

テキストログとは異なり、ETW はデータ処理を自動化するように設計された構造化イベントを提供します。 Microsoft では、これらの構造化されたイベント ( [Windows Performance Analyzer (WPA)](/windows-hardware/test/wpt/windows-performance-analyzer)を含む) 上に強力なツールを構築しています。これは、ETW トレースファイル (.etl) でキャプチャされたトレースデータを視覚化および探索するためのグラフィカルインターフェイスを備えています。

Microsoft では、Windows の新しいビルドのパフォーマンスを測定するために、ETW トレースを頻繁に使用しています。 Windows エンジニアリングシステムを生成したデータの量によっては、自動分析が不可欠です。 Microsoft では、自動化されたトレース分析のために、C# と .NET を頻繁に使用しているため、さまざまな種類の ETW トレースデータにアクセスするための [.Net TraceProcessing API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) を作成しました。 このテクノロジは、Windows パフォーマンスアナライザー内で、いくつかのテーブルの機能を強化するためにも使用されます。

.NET TraceProcessing NuGet パッケージを使用すると、Microsoft が Windows の分析に使用するのと同じツールを使用して、独自のアプリケーションやシステムを分析することができます。

## <a name="next-steps"></a>次の手順

この概要では、.NET の TraceProcessing について学習しました。

次の手順では、 [最初のトレースを処理](quickstart.md)します。

## <a name="related-topics"></a>関連トピック

* [トレースデータへのアクセス](tutorial.md)
* [TraceProcessor の拡張](extensibility.md)
* [シンボルの読み込み](symbols.md)
* [ストリーミングの使用](streaming.md)
* [サンプル](https://github.com/microsoft/eventtracing-processing-samples)
* [API リファレンス](reference.md)