---
title: ストリーミングを使用する-.NET TraceProcessing
description: このチュートリアルでは、ストリーミングを使用して、すぐにトレースデータにアクセスし、より少ないメモリを使用する方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 6ad0f5977ed4d739ce3133c9e67c0eefc6e0cbd9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168826"
---
# <a name="use-streaming-with-traceprocessor"></a>TraceProcessor でのストリーミングの使用

既定では、TraceProcessor はトレースが処理されるときにメモリに読み込むことによってデータにアクセスします。 このバッファリングアプローチは簡単に使用できますが、メモリ使用量の観点からはコストが高くなる可能性があります。

TraceProcessor はトレースも提供します。UseStreaming ()。ストリーミング方式での複数の種類のトレースデータへのアクセスをサポートします (データをメモリ内にバッファリングするのではなく、トレースファイルから読み取ったデータを処理します)。 たとえば、syscall トレースは非常に大きくなる可能性があり、トレース内の syscall のリスト全体をバッファリングすると非常に負荷が大きくなる可能性があります。

## <a name="accessing-buffered-data"></a>バッファーデータへのアクセス

次のコードは、トレースによって通常のバッファリングされた方法で syscall データにアクセスする方法を示しています。UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>ストリーミングデータへのアクセス

大規模な syscall トレースでは、メモリ内の syscall データをバッファーに格納しようとすると、非常に負荷がかかることがあります。また、不可能な場合もあります。 次のコードは、同じ syscall データにストリーミング方式でアクセスする方法を示しています。トレースを置き換えます。UseSyscalls () と trace。UseStreaming().UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>ストリーミングのしくみ

既定では、すべてのストリーミングデータはトレースの最初のパス中に提供され、他のソースからのバッファーデータは使用できません。 上の例では、ストリーミングをバッファリングと組み合わせる方法を示しています。 syscall データがストリーミングされる前に、スレッドデータがバッファリングされます。 その結果、トレースは、バッファーされたスレッドデータを取得するために1回、バッファーされたスレッドデータを使用できるようになったストリーミング syscall データに2回目にアクセスする必要があります。 ストリーミングとバッファリングをこの方法で結合するために、この例では ConsumerSchedule パスをトレースに渡しています。UseStreaming().UseSyscalls ()。これにより、トレースの2回目のパスで syscall 処理が発生します。 2番目のパスでを実行することで、syscall コールバックはトレースから保留中の結果にアクセスできます。UseThreads () は、各 syscall を処理します。 この省略可能な引数を指定しない場合、syscall ストリーミングはトレースの最初のパス (1 つのパスのみ)、およびトレースからの保留中の結果として実行されます。UseThreads () はまだ使用できません。 この場合、コールバックは、syscall から ThreadId にアクセスできますが、スレッドのプロセスにアクセスすることはできません (リンクデータを処理するスレッドは、まだ処理されていない可能性がある他のイベントによって提供されるため)。

バッファリングとストリーミングの使用には、次のような重要な違いがあります。

1. バッファリングは[IPendingResult &lt; T &gt; ](/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)を返し、保持される結果はトレースが処理される前にのみ使用できます。 トレースが処理された後、foreach や LINQ などの手法を使用して結果を列挙できます。
2. Streaming は void を返し、代わりにコールバック引数を受け取ります。 各項目が使用可能になると、コールバックが1回呼び出されます。 データはバッファリングされないため、foreach または LINQ で列挙する結果の一覧はありません。ストリーミングコールバックは、処理が完了した後に使用するために保存するデータのすべての部分をバッファーする必要があります。
3. バッファリングされたデータを処理するためのコードは、trace の呼び出しの後に表示されます。処理 ()。保留中の結果を使用できます。
4. ストリーミングデータを処理するためのコードは、トレースの呼び出しの前に表示されます。プロセス () をトレースへのコールバックとして処理します。UseStreaming...() メソッド。
5. ストリーミングコンシューマーは、コンテキストを呼び出すことによって、ストリームの一部だけを処理し、将来のコールバックをキャンセルすることを選択できます。キャンセル ()。 バッファリングコンシューマーは常に、バッファー内の完全なリストを提供します。

## <a name="correlated-streaming-data"></a>相関ストリーミングデータ

トレースデータには一連のイベントが含まれる場合があります。たとえば、syscall は個別の enter イベントと exit イベントを使用してログに記録されますが、両方のイベントの結合データはより便利な場合があります。 メソッドトレース。UseStreaming().UseSyscalls () は、これらのイベントの両方のデータを関連付け、ペアが使用可能になったときにそれを提供します。 いくつかの種類の相関データは、trace を介して使用できます。UseStreaming():

| コード                                        | 説明                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| トレース.UseStreaming().UseContextSwitchData() | 関連付けられたコンテキストの切り替えデータをストリームします (コンパクトで非コンパクトなイベントから、未加工の非圧縮イベントよりも正確な SwitchInThreadIds)。 |
| トレース.UseStreaming().UseScheduledTasks()    | 関連付けられたスケジュールされたタスクデータをストリームします。                                                                                                         |
| トレース.UseStreaming().UseSyscalls()          | 相関システム呼び出しデータをストリームします。                                                                                                            |
| トレース.UseStreaming().UseWindowInFocus()     | 関連付けられたウィンドウ内のデータをストリームします。                                                                                                        |

## <a name="standalone-streaming-events"></a>スタンドアロンストリーミングイベント

さらに、trace を行います。UseStreaming () は、さまざまなスタンドアロンイベントの種類に対して解析されたイベントを提供します。

| コード                                               | 説明                                     |
|----------------------------------------------------|-------------------------------------------------|
| トレース.UseStreaming().UseLastBranchRecordEvents()   | ストリームは、最後のブランチレコード (LBR) イベントを解析します。 |
| トレース.UseStreaming().UseReadyThreadEvents()        | ストリーム解析された準備完了スレッドイベント。             |
| トレース.UseStreaming().UseThreadCreateEvents ()       | ストリーム解析されたスレッドは、イベントを作成します。            |
| トレース.UseStreaming().UseThreadExitEvents()         | 解析されたスレッド終了イベントをストリームします。              |
| トレース.UseStreaming().UseThreadRundownStartEvents () | ストリーム解析されたスレッドランダウン開始イベント。     |
| トレース.UseStreaming().UseThreadRundownStopEvents ()  | ストリーム解析されたスレッドランダウンストップイベント。      |
| トレース.UseStreaming().UseThreadSetNameEvents()      | 解析されたスレッドセット名イベントをストリームします。          |

## <a name="underlying-streaming-events-for-correlated-data"></a>関連付けられるデータの基になるストリーミングイベント

最後に、trace を行います。UseStreaming () には、上記のリストのデータを関連付けるために使用される、基になるイベントも用意されています。 これらの基になるイベントは次のとおりです。

| コード                                                        | 説明                                                                                | このバージョンを含む製品                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| トレース.UseStreaming().UseCompactContextSwitchEvents()        | ストリーム解析コンパクトコンテキスト切り替えイベント。                                              | トレース.UseStreaming().UseContextSwitchData() |
| トレース.UseStreaming().UseContextSwitchEvents()               | ストリーム解析されたコンテキストの切り替えイベント。 SwitchInThreadIds は、場合によっては正確でない場合があります。 | トレース.UseStreaming().UseContextSwitchData() |
| トレース.UseStreaming().UseFocusChangeEvents()                 | 解析されたウィンドウフォーカス変更イベントをストリームします。                                                 | トレース.UseStreaming().UseWindowInFocus()     |
| トレース.UseStreaming().UseScheduledTaskStartEvents()          | スケジュールされたタスクの開始イベントを解析しました。                                                | トレース.UseStreaming().UseScheduledTasks()    |
| トレース.UseStreaming().UseScheduledTaskStopEvents()           | スケジュールされたスケジュールされたタスクの停止イベントをストリームします。                                                 | トレース.UseStreaming().UseScheduledTasks()    |
| トレース.UseStreaming().UseScheduledTaskTriggerEvents()        | トリガーされたスケジュールされたタスクトリガーイベントのストリーム。                                              | トレース.UseStreaming().UseScheduledTasks()    |
| トレース.UseStreaming().UseSessionLayerSetActiveWindowEvents() | ストリーム解析されたセッションレイヤーセットのアクティブウィンドウイベント。                                     | トレース.UseStreaming().UseWindowInFocus()     |
| トレース.UseStreaming().すべてのイベント ()                | ストリーム解析された syscall は、イベントを開始します。                                                       | トレース.UseStreaming().UseSyscalls()          |
| トレース.UseStreaming().すべてのイベント ()                 | 解析された syscall 終了イベントをストリームします。                                                        | トレース.UseStreaming().UseSyscalls()          |

## <a name="next-steps"></a>次の手順

このチュートリアルでは、ストリーミングを使用して、すぐにトレースデータにアクセスし、使用するメモリを少なくする方法を学習しました。

次の手順では、トレースから必要なデータにアクセスします。 いくつかのアイデアについては、 [サンプル](https://github.com/microsoft/eventtracing-processing-samples) を参照してください。 すべてのトレースにサポートされているすべての種類のデータが含まれているわけではありません。