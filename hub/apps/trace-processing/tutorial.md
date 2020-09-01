---
title: トレースデータへのアクセス-.NET TraceProcessing
description: このチュートリアルでは、TraceProcessor を使用してトレースデータにアクセスする方法について説明します。
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: ef4d3df6e5a5dd93dbcb2caadc8e3f299aad581c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173696"
---
# <a name="access-trace-data"></a>トレースデータへのアクセス

.NET TraceProcessing は、次のパッケージ ID を持つ [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) から使用できます。

Microsoft. Windows. EventTracing. All

このパッケージを使用すると、トレースファイル内のデータにアクセスできます。 まだトレースファイルがない場合は、 [Windows パフォーマンスレコーダー](/windows-hardware/test/wpt/start-a-recording) を使用して作成できます。

次のコンソールアプリの例は、トレースに含まれるすべてのプロセスのコマンドラインにアクセスする方法を示しています。

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

## <a name="using-traceprocessor"></a>TraceProcessor の使用

トレースを処理するには、 [Traceprocessor. Create](/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create)を呼び出します。 コアインターフェイスは [ITraceProcessor](/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)であり、このインターフェイスを使用すると、次のようなパターンがあります。

1. 最初に、トレースから使用するデータをプロセッサに伝えます。
2. 次に、トレースを処理します。そして
3. 最後に、結果にアクセスします。

プロセッサに対して、必要なデータの種類を指定することで、すべての種類のトレースデータを大量に処理するのに時間を費やす必要がなくなります。 代わりに、 [Traceprocessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) は、要求した特定の種類のデータを提供するために必要な作業のみを実行します。

## <a name="recommended-project-settings"></a>推奨されるプロジェクト設定

TraceProcessor では、次の2つのプロジェクト設定を使用することをお勧めします。

1. Exe は64ビットとして実行することをお勧めします。

    新しい C# .NET Framework コンソールアプリケーションの Visual Studio の既定では、32ビットの優先がオンになっている任意の CPU が使用されています。 .NET Core の既定値には、既に推奨設定が含まれている場合があります。

    トレース処理は、特に大規模なトレースでメモリを集中的に使用することができます。また、TraceProcessor を使用する exe で、プラットフォームターゲットを x64 に変更する (または、32ビットを優先する) ことをお勧めします。 これらの設定を変更するには、プロジェクトの [プロパティ] の [ビルド] タブを参照してください。 すべての構成に対してこれらの設定を変更するには、[構成] ドロップダウンが、現在の構成の既定値ではなく、[すべての構成] に設定されていることを確認します。

2. 古い packages.config モードではなく、新しいスタイルの PackageReference モードで NuGet を使用することをお勧めします。

    新しいプロジェクトの既定値を変更するには、「ツール」、「NuGet パッケージマネージャー」、「パッケージマネージャーの設定」、Package Management 「既定のパッケージ管理形式」を参照してください。

## <a name="built-in-data-sources"></a>組み込みデータ ソース

.Etl ファイルは、トレース内のさまざまな種類のデータをキャプチャできます。 .Etl ファイルに含まれるデータは、トレースのキャプチャ時に有効にされたプロバイダーによって異なります。 次の一覧は、TraceProcessor から使用できるトレースデータの種類を示しています。

| コード                                      | 説明                                                                                                                | 関連する WPA 項目                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| トレース.UseClassicEvents()                  | トレースからの従来の ETW イベントを提供します。これには、スキーマ情報は含まれません。                                         | 汎用イベントテーブル (イベントの種類が Classic または WPP の場合)             |
| トレース.UseConnectedStandbyData()           | コネクトスタンバイを開始または終了するシステムに関するトレースのデータを提供します。                                        | CS 概要テーブル                                                     |
| トレース.UseCpuIdleStates()                  | CPU C の状態に関するトレースのデータを提供します。                                                                             | CPU アイドル状態テーブル (型が実際の場合)                          |
| トレース.UseCpuSamplingData()                | 命令ポインターの定期的なサンプリングに基づいて、CPU 使用率に関するトレースのデータを提供します。                          | CPU 使用率 (サンプリング) テーブル                                            |
| トレース.UseCpuSchedulingData()              | コンテキストスイッチや準備完了スレッドイベントなど、CPU スレッドスケジューリングに関するトレースのデータを提供します。                | CPU 使用率 (正確) テーブル                                            |
| トレース.UseDevicePowerData ()                | デバイス D の状態に関するトレースのデータを提供します。                                                                          | デバイスの DState テーブル                                                  |
| トレース.UseDirectXData()                    | DirectX アクティビティに関するトレースのデータを提供します。                                                                         | GPU 使用率テーブル                                                |
| traceUseDiskIOData ()                      | ディスク i/o アクティビティに関するトレースのデータを提供します。                                                                        | ディスク使用量テーブル                                                     |
| トレース.UseEnergyEstimationData()           | エネルギー推定エンジンによる、推定されたプロセスごとのエネルギー使用量に関するトレースのデータを提供します。                         | エネルギー推定エンジンの概要 (プロセス別) テーブル                  |
| トレース.UseEnergyMeterData()                | エネルギーメーターインターフェイス (EMI) からの測定エネルギー使用量に関するトレースのデータを提供します。                                  | エネルギー推定エンジン (Emi) テーブル                              |
| トレース.UseFileIOData()                     | ファイル i/o アクティビティに関するトレースのデータを提供します。                                                                        | ファイル i/o テーブル                                                       |
| トレース.UseGenericEvents ()                  | トレースからのイベントおよび TraceLogging イベントを提供します。                                                                  | 汎用イベントテーブル (イベントの種類が "発生" または "TraceLogging" の場合) |
| トレース.UseHandles ()                        | アクティブなカーネルハンドルに関するトレースから部分的なデータを提供します。                                                            | テーブルの処理                                                        |
| トレース.Useハードフォールト ()                     | ハードページフォールトに関するトレースのデータを提供します。                                                                         | ハードフォールトテーブル                                                    |
| トレース.UseHeapSnapshots ()                  | プロセスヒープの使用状況に関するトレースのデータを提供します。                                                                       | ヒープスナップショットテーブル                                                  |
| トレース.UseHypercalls コールハイパー ()                     | トレース中に発生した Hyper-v ハイパースレッディングに関するデータを提供します。                                                        |                                                                      |
| トレース.UseImageSections()                  | 画像のセクションに関するトレースのデータを提供します。                                                                 | CPU 使用率 (サンプリングされた) テーブルのセクション名列                 |
| トレース.UseInterruptHandlingData()          | 割り込みサービスルーチン (ISR) および遅延プロシージャ呼び出し (DPC) アクティビティに関するトレースのデータを提供します。               | DPC/ISR テーブル                                                        |
| トレース.UseMarks()                          | トレースのマーク (タイムスタンプ) を提供します。                                                                      | マークテーブル                                                          |
| トレース.UseMemoryUtilizationData()          | システムメモリの合計使用率に関するトレースのデータを提供します。                                                          | メモリ使用率テーブル                                             |
| トレース.UseMetadata()                       | 追加の処理なしで使用可能なトレースメタデータを提供します。                                                              | システム構成、トレース、および全般                             |
| トレース.UsePlatformIdleStates()             | システムのターゲットと実際のプラットフォームアイドル状態に関するトレースのデータを提供します。                                   | プラットフォームのアイドル状態のテーブル                                            |
| トレース.UsePoolAllocations ()                | カーネルプールのメモリ使用量に関するトレースのデータを提供します。                                                                 | プールの概要テーブル                                                   |
| トレース.UsePowerConfigurationData()         | システムの電源構成に関するトレースのデータを提供します。                                                               | システム構成、電源設定                                 |
| トレース.UsePowerDependencyCoordinatorData() | アクティブな電源依存関係コーディネーターのフェーズに関するトレースのデータを提供します。                                               | 通知フェーズの概要テーブル                                     |
| トレース.UseProcesses ()                      | トレース中にアクティブになっているプロセスに関するデータと、そのイメージと Pdb を提供します。                                      | プロセステーブル。Images テーブル;シンボルハブ                           |
| トレース.UseProcessorCounters ()              | プロセッサカウンターモニター (PCM) からのプロセッサパフォーマンスカウンター値に関するトレースのデータを提供します。                |                                                                      |
| トレース.UseProcessorFrequencyData()         | プロセッサの実行頻度に関するトレースのデータを提供します。                                                    | プロセッサ頻度テーブル (型が実際の場合)                      |
| トレース.UseProcessorProfileData ()           | アクティブなプロセッサ電源プロファイルに関するトレースのデータを提供します。                                                       | プロセッサプロファイルテーブル                                             |
| トレース.Useprocessorparのデータ ()           | どのプロセッサが保留または保留解除されたかに関するデータをトレースから提供します。                                                 | プロセッサパーキング状態テーブル                                        |
| トレース.Useprocessorparの制限 ()         | 保留解除されたプロセッサの最大許容数に関するトレースのデータを提供します。                                        | コアパーキングキャップ状態テーブル                                         |
| トレース.UseProcessorQualityOfServiceData()  | 各プロセッサのサービスレベルの品質に関するトレースのデータを提供します。                                          | プロセッサ Qos クラステーブル                                            |
| トレース.UseProcessorThrottlingData()        | プロセッサの最大周波数調整に関するトレースのデータを提供します。                                                   | プロセッサの制約の表                                          |
| トレース.UseReadyBootData()                  | ブートプリフェッチアクティビティに関するトレースからのデータを、準備完了の状態で提供します。                                                | 準備完了の起動イベントテーブル                                              |
| トレース.Usereの Encesetdata ()               | 各プロセスで使用される仮想メモリのページに関するトレースのデータを提供します。                                             | 参照セットテーブル                                                  |
| トレース.UseRegionsOfInterest()              | Xml 構成ファイルで指定されているように、トレースから対象期間の名前付き領域を提供します。                       | 関心のあるテーブルの地域                                            |
| トレース.UseRegistryData()                   | トレース中のレジストリアクティビティに関するデータを提供します。                                                                      | レジストリテーブル                                                       |
| トレース.UseResidentSetData()                | 物理メモリに常駐していた各プロセスの仮想メモリのページに関するトレースのデータを提供します。       | 常駐セットテーブル                                                   |
| トレース.UseRundownData ()                    | トレースランダウンデータ収集が発生した間隔に関するトレースのデータを提供します。                            | グラフのタイムラインでの網掛けされた領域                                 |
| トレース.UseScheduledTasks()                 | トレース中に実行されたスケジュールされたタスクに関するデータを提供します。                                                               | スケジュールされたタスクの表                                                |
| トレース.UseServices ()                       | トレース中にアクティブだった、または状態がキャプチャされたサービスに関するデータを提供します。                                  | Services テーブル;システム構成、サービス                       |
| トレース.UseStacks ()                         | トレース中に記録されたスタックに関するデータを提供します。                                                                        |                                                                      |
| トレース.UseStackEvents 孔 ()                    | トレース中に記録されたスタックに関連付けられたイベントに関するデータを提供します。                                                 | スタックテーブル                                                         |
| トレース.UseStackTags ()                      | トレースのスタックを XML 構成ファイルで指定されているスタックタグにグループ化するマッパーを提供します。               | スタックタグやスタック (フレームタグ) などの列                     |
| トレース.UseSymbols()                        | トレースのシンボルを読み込む機能を提供します。                                                                          | シンボルパスを構成します。シンボルの読み込み                                 |
| トレース.UseSyscalls()                       | トレース中に発生した syscall に関するデータを提供します。                                                                 | Syscall テーブル                                                       |
| トレース.UseSystemMetadata()                 | トレースから、システム全体にわたる一般的なメタデータを提供します。                                                                       | システム構成                                                 |
| トレース.UseSystemPowerSourceData()          | アクティブなシステム電源 (AC および DC) に関するトレースのデータを提供します。                                                | システムの電源コードテーブル                                            |
| トレース.UseSystemSleepData()                | システム全体の電源状態に関するトレースのデータを提供します。                                                               | 電源切り替えテーブル                                               |
| トレース.UseTargetCpuIdleStates()            | ターゲット CPU C 状態に関するトレースのデータを提供します。                                                                      | CPU アイドル状態テーブル (種類がターゲットの場合)                          |
| トレース.UseTargetProcessorFrequencyData()   | ターゲットプロセッサの頻度に関するトレースのデータを提供します。                                                             | プロセッサ頻度テーブル (型がターゲットの場合)                      |
| トレース.UseThreads ()                        | トレース中にアクティブなスレッドに関するデータを提供します。                                                                         | スレッドの有効期間の表                                               |
| トレース.UseTraceStatistics()                | トレース内のイベントに関する統計情報を提供します。                                                                           | システム構成、トレース統計                               |
| トレース.UseUtcData()                        | ユニバーサルテレメトリクライアント (UTC) を使用して、Microsoft テレメトリアクティビティに関するトレースのデータを提供します。                      | Utc テーブル                                                            |
| トレース.UseWindowInFocus()                  | フォーカスされているアクティブな UI ウィンドウに対する変更に関するトレースのデータを提供します。                                                 | フォーカステーブル内のウィンドウ                                                |
| トレース.UseWindowsTracePreprocessorEvents() | トレースから Windows software trace プリプロセッサ (WPP) イベントを提供します。                                                    | WPP トレーステーブル;汎用イベントテーブル (イベントの種類が WPP の場合)       |
| トレース.UseWinINetData ()                    | Windows インターネット (WinINet) を介したインターネットアクティビティに関するトレースのデータを提供します。                                         | 詳細テーブルのダウンロード                                               |
| トレース.UseWorkingSetData()                 | 各プロセスまたはカーネルカテゴリのワーキングセットに含まれていた仮想メモリのページに関するトレースのデータを提供します。 | 仮想メモリスナップショットテーブル                                       |

使用可能なすべてのトレースデータに対する [Itracesource](/dotnet/api/microsoft.windows.eventtracing.itracesource) の拡張メソッドを参照するか、"trace" から使用可能なメソッドを確認してください。 IntelliSense によって表示されます。

## <a name="next-steps"></a>次の手順

この概要では、TraceProcessor とそれがアクセスできる組み込みのデータソースを使用して、トレースデータにアクセスする方法を学習しました。

次の手順では、traceprocessor を [拡張](extensibility.md) してカスタムトレースデータにアクセスする方法を学習します。