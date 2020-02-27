---
title: 'クイックスタート: トレースの処理-.NET TraceProcessing'
description: このクイックスタートでは、ETW トレースのデータにアクセスする方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629104"
---
# <a name="quickstart-process-your-first-trace"></a>クイックスタート: 最初のトレースを処理する

Windows イベントトレーシング (ETW) トレースのデータにアクセスするには、TraceProcessor を試してみてください。 TraceProcessor を使用すると、.NET オブジェクトとして ETW トレースデータにアクセスできます。

このクイックスタートでは、次の方法について説明します。

1. TraceProcessing NuGet パッケージをインストールします。
2. TraceProcessor を作成します。
3. トレースに含まれているプロセスのコマンドラインにアクセスするには、TraceProcessor を使用します。

## <a name="prerequisites"></a>前提条件

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>TraceProcessing NuGet パッケージをインストールする

.NET TraceProcessing は、次のパッケージ ID を持つ[NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All)から使用できます。

Microsoft. Windows. EventTracing. All

このパッケージをコンソールアプリで使用すると、ETW トレース (.etl ファイル) に含まれているプロセスコマンドラインを一覧表示できます。

1. 新しい .NET Core コンソールアプリを作成します。 Visual Studio で、[ファイル]、[新規作成]、[プロジェクト...] の順に選択し、 C#の [コンソールアプリ (.net Core)] テンプレートを選択します。

    プロジェクト名を入力し (たとえば、TraceProcessorQuickstart スタート)、[作成] を選択します。

2. ソリューションエクスプローラーで、[依存関係] を右クリックし、[NuGet パッケージの管理...] を選択します。を選択し、[参照] タブに切り替えます。

3. 検索ボックスに、「Microsoft. Windows. すべて」と入力して検索します。

    その名前の NuGet パッケージで [インストール] を選択し、NuGet ウィンドウを閉じます。

## <a name="create-a-traceprocessor"></a>TraceProcessor の作成

1. Program.cs を次の内容に変更します。

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. プロジェクトの実行時に使用するトレース名を指定します。

    ソリューションエクスプローラーで、プロジェクトを右クリックし、[プロパティ] を選択します。 [デバッグ] タブに切り替え、[アプリケーション引数] にトレース (.etl ファイル) のパスを入力します。

    まだトレースファイルがない場合は、 [Windows パフォーマンスレコーダー](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording)を使用して作成できます。

3. アプリケーションを実行します。

    [デバッグ]、[デバッグなしで開始] の順に選択して、コードを実行します。

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>トレースに含まれるプロセスのコマンドラインにアクセスするには TraceProcessor を使用します

1. Program.cs を次の内容に変更します。

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

                trace.Process();

                IProcessDataSource processData = pendingProcessData.Result;

                foreach (IProcess process in processData.Processes)
                {
                    Console.WriteLine(process.CommandLine);
                }
            }
        }
    }
    ```

2. アプリケーションを再実行します。

    このとき、トレースの記録中に実行されていたすべてのプロセスからのリストコマンドラインが表示されます。

## <a name="next-steps"></a>次の手順

このクイックスタートでは、コンソールアプリケーションを作成し、TraceProcessor をインストールし、それを使用して ETW トレースからプロセスコマンドラインにアクセスしました。 これで、トレースデータにアクセスするアプリケーションが完成しました。

プロセス情報は、アプリケーションがアクセスできる ETW トレースに格納されているさまざまな種類のデータのうちの1つにすぎません。

次の手順では、TraceProcessor とそれがアクセスできるその他のデータソースについて[詳しく見ていき](tutorial.md)ます。
