---
title: 機能拡張-.NET TraceProcessing
description: このチュートリアルでは、.NET のトレース処理を拡張する方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: f5680bdc6502c4b917667e5a59084286b445063c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166736"
---
# <a name="extend-traceprocessor"></a>TraceProcessor の拡張

多くの種類のトレースデータには [Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor)が組み込まれていますが、他のプロバイダー (独自のカスタムプロバイダーを含む) を分析する必要がある場合は、処理の実行中にトレースからデータを取得することもできます。

> [!NOTE]
> API のこの部分はプレビュー段階で、アクティブな開発中です。 将来のリリースで変更される可能性があります。

たとえば、トレース内のプロバイダー Id の一覧を取得する簡単な方法を次に示します。

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

次の例は、簡略化されたカスタムデータソースを示しています。

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>次の手順

このチュートリアルでは、TraceProcessor を拡張する方法について学習しました。

次の手順では、トレースの [シンボルを読み込む](symbols.md) 方法について説明します。