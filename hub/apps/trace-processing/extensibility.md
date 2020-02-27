---
title: 機能拡張-.NET TraceProcessing
description: このチュートリアルでは、.NET のトレース処理を拡張する方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629143"
---
# <a name="extend-traceprocessor"></a>TraceProcessor の拡張

多くの種類のトレースデータには[Traceprocessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor)が組み込まれていますが、他のプロバイダー (独自のカスタムプロバイダーを含む) を分析する必要がある場合は、処理の実行中にトレースからデータを取得することもできます。

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

## <a name="next-steps"></a>次のステップ:

このチュートリアルでは、TraceProcessor を拡張する方法について学習しました。

次の手順では、トレースの[シンボルを読み込む](symbols.md)方法について説明します。
