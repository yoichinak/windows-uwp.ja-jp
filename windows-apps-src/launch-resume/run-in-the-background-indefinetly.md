---
title: バックグラウンドで無期限に実行する
description: extendedExecutionUnconstrained 機能を使用すると、バックグラウンドで無期限にバックグラウンド タスクまたは延長実行セッションを実行できます。
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: バックグラウンドタスク、拡張実行、リソース、制限、バックグラウンドタスク
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f843c23a4a1e0738cfc05e96009b2597f4919809
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217655"
---
# <a name="run-in-the-background-indefinitely"></a>バックグラウンドで無期限に実行する

ユーザーに最適なエクスペリエンスを提供するために、Windows によりユニバーサル Windows プラットフォーム (UWP) アプリにリソース制限が適用されます。 フォアグラウンド アプリに最も多くのメモリ量と実行時間が割り当てられ、バックグラウンド アプリへの割り当ては少なくなります。 これによってユーザーは、フォアグラウンド アプリのパフォーマンスを維持し、大量のバッテリ消費を回避できます。

ただし、個人利用を目的とした UWP アプリ (Microsoft Store で公開しないサイドローディング アプリ) を作成する場合や、エンタープライズ UWP アプリを作成する場合は、バックグラウンド実行や延長実行の調整を指定せずに、すべてのリソースをデバイスで利用可能にしておくこともできます。 基幹アプリケーションや個人用の UWP アプリケーションでは、Windows Creators Update (Version 1703) の API を使用して、調整をオフにすることができます。 ただし、これらの API を使用している場合はアプリを Microsoft Store に公開できないため、注意が必要です。

## <a name="run-while-minimized"></a>最小化状態での実行

フォアグラウンドで実行されていない UWP アプリは一時停止状態になります。 デスクトップでは、ユーザーがアプリを最小化すると、この状態になります。 最小化状態でもアプリの実行を継続するには、延長実行セッションを使用します。 Microsoft Store で承認されている延長実行 API については、「[延長実行を使ってアプリの中断を延期する](./run-minimized-with-extended-execution.md)」をご覧ください。

開発中のアプリを Microsoft Store に提出する予定がない場合は、制限された機能 `extendedExecutionUnconstrained` を指定して [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) を使用できます。これにより、アプリはデバイスの電力状態を問わず、最小化状態でも実行を継続します。  

`extendedExecutionUnconstrained` 機能は、制限された機能としてアプリのマニフェストに追加されます。 制限された機能について詳しくは、「[アプリ機能の宣言](../packaging/app-capability-declarations.md)」をご覧ください。

> [!NOTE]
> *Xmlns: rescap* XML 名前空間宣言を追加し、 *rescap*プレフィックスを使用して機能を宣言します。
>
> 詳細については、「 [アプリ機能の宣言](../packaging/app-capability-declarations.md)」の「制限された機能」を参照してください。
>

_Package.appxmanifest_

```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

`extendedExecutionUnconstrained` 機能を使用する場合は、[ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) と [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) ではなく [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) と [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) が使用されます。 この場合も、セッションの作成、メンバーの設定、非同期的な延長要求は同じパターンになります。 

```cs
var newSession = new ExtendedExecutionForegroundSession();
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;
newSession.Description = "Long Running Processing";
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();
switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```

アプリがフォアグラウンドになるとすぐに、この延長実行セッションを要求できます。 制限のない延長実行セッションは、割り当て電力やオペレーティング システムのバッテリー節約機能によって制限されません。 セッション オブジェクトへの参照が存在する限り、アプリは実行状態を維持し、中断状態に移行しません。 ユーザーによってアプリが閉じられると、セッションは失効します。

**Revoked** イベントに登録すると、アプリは必要なクリーンアップ作業を行うことができます。 中断状態では、[ExtendedExecutionReason.SavingData](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) を使用して延長実行セッションを作成することにより、アプリが中断されメモリから削除される前にユーザー データを保存できます。

## <a name="run-background-tasks-indefinitely"></a>バックグラウンド タスクを無期限に実行する

ユニバーサル Windows プラットフォームのバックグラウンド タスクは、どのような形のユーザー インターフェイスも持たない、バックグラウンドで実行されるプロセスです。 一般的に、バックグラウンド タスクは取り消されるまで最長で 25 秒間実行できます。 長時間実行されるタスクについても、バックグラウンド タスクのアイドル状態やメモリ使用について確認が行われます。 Windows Creators Update (Version 1703) では、制限された機能 [extendedBackgroundTaskTime](../packaging/app-capability-declarations.md) が導入され、これらの制限が解消されました。 **extendedBackgroundTaskTime** 機能は、制限された機能としてアプリのマニフェスト ファイルに追加されます。

> [!NOTE]
> *Xmlns: rescap* XML 名前空間宣言を追加し、 *rescap*プレフィックスを使用して機能を宣言します。
>
> 詳細については、「 [アプリ機能の宣言](../packaging/app-capability-declarations.md)」の「制限された機能」を参照してください。
>

_Package.appxmanifest_

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

この機能を使用すると、実行時間の制限とアイドル タスクに対するウォッチドッグが解除されます。 トリガーまたはアプリ サービスの呼び出しによってバックグラウンド タスクが開始され、**Run** メソッドで指定された [BackgroundTaskInstance](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) によって延期されると、そのバックグラウンド タスクは無期限に実行されます。 アプリが **[Windows で管理]** に設定されている場合は、割り当て電力が指定されていることがあり、バッテリー節約機能が有効であればバックグラウンド タスクがアクティブになりません。この動作は OS の設定で変更できます。 詳しくは、「[バックグラウンド アクティビティの最適化](../debug-test-perf/optimize-background-activity.md)」をご覧ください。

ユニバーサル Windows プラットフォームでは、バッテリ寿命を維持し、フォアグラウンド アプリで最適なエクスペリエンスを提供するために、バックグラウンド タスクの実行が監視されます。 ただし、個人用アプリや企業の基幹業務アプリでは、延長実行と **extendedBackgroundTaskTime** 機能を使用して、デバイス リソースの可用性にかかわらず、必要な限り実行されるアプリを作成できます。

ただし、**extendedExecutionUnconstrained** 機能と **extendedBackgroundTaskTime** 機能では UWP アプリの既定ポリシーが上書きされ、大幅にバッテリが消耗することがあるため、注意が必要です。 これらの機能を使用する前に、まず延長実行およびバックグラウンド タスクの時間に関する既定のポリシーがニーズに合っていないことを確認し、バッテリに制約のある状態でテストを実行して、アプリがデバイスに与える影響を把握してください。

## <a name="see-also"></a>関連項目

[バックグラウンド タスクのリソース制限を解除する](/windows/application-management/enterprise-background-activity-controls)