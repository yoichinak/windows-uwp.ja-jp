---
title: シンボルの読み込み-.NET TraceProcessing
description: このチュートリアルでは、トレースの処理時にシンボルを読み込む方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 72264b51edcc0b02aa335b8766100c196a0d5090
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168816"
---
# <a name="use-symbols-in-net-traceprocessing"></a>.NET トレース処理でシンボルを使用する

[Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) は、複数のデータソースからシンボルを読み込み、スタックを取得することをサポートしています。 次のコンソールアプリケーションは、CPU サンプルを調べ、特定の関数が実行されていた推定時間を出力します (CPU 使用率のトレースの統計サンプリングに基づいています)。

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

このプログラムを実行すると、次のような出力が生成されます。

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

(出力の詳細はトレースによって異なります)。

## <a name="symbols-format"></a>シンボルの形式

内部的には、TraceProcessor は、PDB に格納されている一部のデータのキャッシュである [symcache](/windows-hardware/test/wpt/loading-symbols#symcache-path) 形式を使用します。 シンボルを読み込むときに、TraceProcessor は、これらの SymCache ファイル (SymCache パス) に使用する場所を指定し、必要に応じて Pdb にアクセスするためのシンボルパスを指定する必要があります。 シンボルパスが指定されると、TraceProcessor は必要に応じて、PDB ファイルから SymCache ファイルを作成します。また、同じデータの後続の処理では、パフォーマンス向上のために SymCache ファイルを直接使用できます。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、トレースの処理時にシンボルを読み込む方法について学習しました。

次の手順では、ストリーミングを [使用](streaming.md) して、メモリ内のすべてのデータをバッファリングすることなく、トレースデータにアクセスする方法を学習します。