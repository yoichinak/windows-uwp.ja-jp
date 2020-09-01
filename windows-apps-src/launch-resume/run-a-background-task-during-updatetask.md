---
title: UWP アプリが更新された際のバックグラウンド タスクの実行
description: ユニバーサル Windows プラットフォーム (UWP) アプリが更新された際に実行されるバックグラウンド タスクの作成方法を説明します。
ms.date: 04/21/2017
ms.topic: article
keywords: windows 10、uwp、更新、バックグラウンドタスク、updatetask、バックグラウンドタスク
ms.localizationpriority: medium
ms.openlocfilehash: ff4dd5487357728b6a79f4c4d31a437075fcd006
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164806"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>UWP アプリが更新された際のバックグラウンド タスクの実行

ユニバーサル Windows プラットフォーム (UWP) アプリが更新された後に実行されるバックグラウンド タスクの作成方法を説明します。

更新タスク バックグラウンド タスクは、ユーザーがデバイスにインストールされるアプリに更新プログラムをインストールした後、オペレーティング システムによって呼び出されます。 これによって、更新されたアプリをユーザーが起動する前に、アプリで新しいプッシュ通知チャネルの初期化やデータベース スキーマの更新などの初期化タスクを実行できます。

更新タスクは、[ServicingComplete](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) トリガーを使用してバックグラウンド タスクを起動することとは異なります。それは、トリガーを使用する場合、アプリが更新される前にアプリが少なくとも 1 回は実行されて、**ServicingComplete** トリガーによってアクティブ化されるバックグラウンド タスクが登録されている必要があるからです。  更新タスクは登録されないので、1 回も実行されずにアップグレードされたアプリでも更新タスクはトリガーされます。

## <a name="step-1-create-the-background-task-class"></a>手順 1: バックグラウンド タスク クラスを作る

他の種類のバックグラウンド タスクの場合と同様に、Windows ランタイム コンポーネントとして更新タスク バックグラウンド タスクを実装します。 このコンポーネントを作成するには、「[アウトプロセス バックグラウンド タスクの作成と登録](./create-and-register-a-background-task.md)」の「**バックグラウンド タスク クラスを作る**」セクションの手順に従ってください。 これには次の手順が含まれます。

- Windows ランタイムコンポーネントプロジェクトをソリューションに追加しています。
- アプリからコンポーネントへの参照を作成する。
- [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) を実装するコンポーネント内の public sealed クラスを作成する。
- 更新タスクの実行時に呼び出される必要なエントリ ポイントである [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) メソッドを実装する。 バックグラウンド タスクからの非同期呼び出しを作成する場合は、「[アウトプロセス バックグラウンド タスクの作成と登録](./create-and-register-a-background-task.md)」で **Run** メソッドでの遅延の使用方法を説明しています。

更新タスクを使用するうえで、このバックグラウンド タスクの登録 (**「アウトプロセス バックグラウンド タスクの作成と登録」** トピックの「実行するバックグラウンド タスクを登録する」セクション) は不要です。 更新タスクを使用する主な理由は、タスクを登録するためにコードをアプリに追加する必要がないこと、およびアプリの更新前にアプリを少なくとも 1 回実行してバックグラウンド タスクを登録する必要がないことです。

次のサンプル コードは、C# の更新タスク バックグラウンド タスク クラスの基本的な開始点を示しています。 バックグラウンド タスク クラス自体と、バックグラウンド タスク プロジェクト内のその他すべてのクラスは、**public****sealed** クラスである必要があります。 バックグラウンド タスク クラスは **IBackgroundTask** から派生する必要があり、以下に示すシグネチャを持つパブリック **Run()** メソッドが含まれている必要があります。

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>手順 2: パッケージ マニフェストでバックグラウンド タスクを宣言する

Visual Studio のソリューション エクスプローラーで、**Package.appxmanifest** を右クリックし、**[コードの表示]** をクリックして、パッケージ マニフェストを表示します。 次の `<Extensions>` XML を追加して更新タスクを宣言します。

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

上記の XML で、`EntryPoint` 属性が更新タスク クラスの <名前空間>.<クラス> 名となるように設定します。 名前の大文字と小文字は区別されます。

## <a name="step-3-debugtest-your-update-task"></a>手順 3: 更新タスクをデバッグ/テストする

アプリを PC に展開して、更新対象を用意します。

バックグラウンド タスクの Run() メソッドにブレークポイントを設定します。

![ブレークポイントの設定](images/run-func-breakpoint.png)

次に、ソリューション エクスプローラーで、アプリのプロジェクト (バックグラウンド タスクのプロジェクトではない) を右クリックし、**[プロパティ]** をクリックします。 アプリケーションの [プロパティ] ウィンドウで、左側の **[デバッグ]** をクリックし、**[起動しないが、開始時にコードをデバッグ]** を選択します。

![デバッグ設定の設定](images/do-not-launch-but-debug.png)

次に、UpdateTask がトリガーされるように、パッケージのバージョン番号を増やします。 ソリューション エクスプローラーで、アプリの **Package.appxmanifest** ファイルをダブルクリックして、パッケージ デザイナーを開き、**[ビルド]** 番号を更新します。

![バージョンの更新](images/bump-version.png)

これで、Visual Studio 2019 で F5 キーを押すと、アプリが更新され、バックグラウンドで UpdateTask コンポーネントがアクティブ化されます。 デバッガーは自動的にバックグラウンド プロセスにアタッチします。 ブレークポイントに到達すると、更新コード ロジックをステップ実行できます。

バックグラウンド タスクが完了したら、同じデバッグ セッション内で Windows のスタート メニューからフォアグラウンド アプリを起動できます。 デバッガーが今回はフォアグラウンド プロセスに自動的にアタッチし、アプリのロジックをステップ実行できます。

> [!NOTE]
> Visual Studio 2015 ユーザー: 上記の手順は、Visual Studio 2017 または Visual Studio 2019 に適用されます。 Visual Studio 2015 を使用している場合、同じ手法を使用して UpdateTask をトリガーしてテストできますが、Visual Studio によってデバッガーがアタッチされることはありません。 VS 2015 での代替の手順は、UpdateTask をエントリ ポイントとして設定する [ApplicationTrigger](./trigger-background-task-from-app.md) をセットアップし、フォアグラウンド アプリから直接実行をトリガーすることです。

## <a name="see-also"></a>関連項目

[アウトプロセス バックグラウンド タスクの作成と登録](./create-and-register-a-background-task.md)