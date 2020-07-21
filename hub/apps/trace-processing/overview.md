---
title: .NET トレース処理とは-.NET TraceProcessing
description: この概要では、.NET のトレース処理について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bc2ec3035e097a8cab15b556d1b1793385b8aeb9
ms.sourcegitcommit: 7f1b64f62bc3a82ebcd3807c809363df46919195
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77705757"
---
# <a name="process-etw-traces-in-net"></a>.NET での ETW トレースの処理

[Windows イベントトレーシング (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)は、Windows オペレーティングシステムに組み込まれている強力なトレースコレクションシステムです。 Windows には、ETW との緊密な統合があります。これには、コンテキストの切り替え、メモリの割り当て、プロセスの作成と終了などのイベントについて、システムの動作に関するデータが含まれます。 ETW から利用できるシステム全体のデータを使用すると、エンドツーエンドのパフォーマンス分析や、システム全体のさまざまなコンポーネント間の相互作用を調べる必要があるその他の質問に適したものになります。

テキストログとは異なり、ETW はデータ処理を自動化するように設計された構造化イベントを提供します。 Microsoft では、これらの構造化されたイベント ( [Windows Performance Analyzer (WPA)](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer)を含む) 上に強力なツールを構築しています。これは、ETW トレースファイル (.etl) でキャプチャされたトレースデータを視覚化および探索するためのグラフィカルインターフェイスを備えています。

Microsoft では、Windows の新しいビルドのパフォーマンスを測定するために、ETW トレースを頻繁に使用しています。 Windows エンジニアリングシステムを生成したデータの量によっては、自動分析が不可欠です。 自動化されたトレース分析では、 C#と .net が頻繁に使用されるため、さまざまな種類の ETW トレースデータにアクセスするための[.NET traceprocessing API](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)を作成しました。 このテクノロジは、Windows パフォーマンスアナライザー内で、いくつかのテーブルの機能を強化するためにも使用されます。

.NET TraceProcessing NuGet パッケージを使用すると、Microsoft が Windows の分析に使用するのと同じツールを使用して、独自のアプリケーションやシステムを分析することができます。

## <a name="next-steps"></a>次のステップ:

この概要では、.NET の TraceProcessing について学習しました。

次の手順では、[最初のトレースを処理](quickstart.md)します。

## <a name="related-topics"></a>関連トピック

* [トレースデータへのアクセス](tutorial.md)
* [TraceProcessor の拡張](extensibility.md)
* [シンボルの読み込み](symbols.md)
* [ストリーミングの使用](streaming.md)
* [サンプル](https://github.com/microsoft/eventtracing-processing-samples)
* [API リファレンス](reference.md)
