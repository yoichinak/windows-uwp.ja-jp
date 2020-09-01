---
title: Winmain バックグラウンドタスクの作成と登録
description: メインプロセスで実行できる COM バックグラウンドタスクを作成します。または、パッケージ化された winmain アプリが実行されていない場合はアウトプロセスで実行できます。
ms.assetid: 8CBD4986-6E65-4374-BC7C-C38908E417E1
ms.date: 03/27/2020
ms.topic: article
keywords: windows 10、デスクトップブリッジ、スパース署名されたパッケージ、winmain、バックグラウンドタスク
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 72b6196f0b4607f2414eb94220dd31190ef93245
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156016"
---
# <a name="create-and-register-a-winmain-com-background-task"></a>winmain COM バックグラウンド タスクの作成と登録

> [!TIP]
> SetTaskEntryPointClsid メソッドは、Windows 10 バージョン2004以降で使用できます。

> [!NOTE]
> このシナリオは、パッケージ化された WinMain アプリにのみ適用できます。 UWP アプリケーションでは、このシナリオを実装しようとするとエラーが発生します。

**重要な API**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

COM バックグラウンドタスククラスを作成し、トリガーへの応答として完全信頼パッケージの winmain アプリで実行されるように登録します。 バックグラウンド タスクを使えば、アプリが中断されている、または実行されていないときに機能を提供できます。 このトピックでは、フォアグラウンドアプリプロセスまたは別のプロセスで実行できるバックグラウンドタスクを作成し、登録する方法について説明します。

## <a name="create-the-background-task-class"></a>バックグラウンド タスク クラスを作る

[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) インターフェイスを実装するクラスを作ると、コードをバックグラウンドで実行できます。 このコードは、 [**Systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) や [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)などを使用して特定のイベントがトリガーされたときに実行されます。

次の手順では、 [**Ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) インターフェイスを実装する新しいクラスを作成し、それをメインプロセスに追加する方法を示します。

1.  パッケージ化された WinMain アプリケーションソリューションで WinRT Api を参照するには、[**次の手順を参照してください**](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。 これは、IBackgroundTask と関連する Api を使用するために必要です。
2.  その新しいクラスで、 [**Ibackgroundtask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) インターフェイスを実装します。 [**Ibackgroundtask. Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)メソッドは、指定されたイベントがトリガーされたときに呼び出される必須のエントリポイントです。このメソッドは、すべてのバックグラウンドタスクで必要です。

> [!NOTE]
> バックグラウンドタスククラス自体 &mdash; およびバックグラウンドタスクプロジェクト内の他のすべてのクラスは、 &mdash; **パブリック**である必要があります。

次のサンプルコードは、primes をカウントし、キャンセルが要求されるまでファイルに書き込む基本的なバックグラウンドタスククラスを示しています。

C++/WinRT の例では、 [**COM コクラス**](../cpp-and-winrt-apis/author-coclasses.md#implement-the-coclass-and-class-factory)としてバックグラウンドタスククラスを実装しています。


<details>
<summary>バックグラウンド タスクのコード サンプル</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample {

    // Note insert unique UUID.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int MaximumPotentialPrime = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < MaximumPotentialPrime))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != NULL) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };
}

```

</p>
</details>


## <a name="add-the-support-code-to-instantiate-the-com-class"></a>COM クラスをインスタンス化するためのサポートコードを追加する

バックグラウンドタスクを完全信頼 winmain アプリケーションでアクティブにするには、バックグラウンドタスククラスにサポートコードが必要です。これは、実行されていない場合にアプリケーションプロセスを開始する方法を理解し、そのバックグラウンドタスクの新しいアクティブ化を処理するために現在どのプロセスのインスタンスがサーバーであるかを理解するためです。

1.  アプリケーションがまだ実行されていない場合、COM はアプリプロセスを起動する方法を理解する必要があります。 バックグラウンドタスクコードをホストするアプリプロセスは、パッケージマニフェストで宣言されている必要があります。 次のサンプルコードは、 **SampleBackgroundApp.exe**内で**sampletask**がどのようにホストされるかを示しています。 プロセスが実行されていないときにバックグラウンドタスクを起動すると、プロセス引数 **"-startsampletaskserver"** を使用して**SampleBackgroundApp.exe**が起動されます。

```xml

<Extensions>
  <com:Extension Category="windows.comServer">
    <com:ComServer>
      <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
        <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
      </com:ExeServer>
    </com:ComServer>
  </com:Extension>
</Extensions>

```

2.  正しい引数を使用してプロセスを開始すると、そのプロセスが SampleTask の新しいインスタンスの現在の COM サーバーであることが COM に通知されます。 次のサンプルコードは、アプリケーションプロセスがそれ自体を COM に登録する方法を示しています。 注これらのサンプルでは、終了する前に少なくとも1つのインスタンスが完了するように、プロセスがその COM サーバーを SampleTask に対して宣言する方法を示しています。 これはオプションです。バックグラウンドタスクを処理すると、主要なプロセス関数が開始される場合があります。

```csharp

class SampleTaskServer
{
    SampleTaskServer()
    {
        comRegistrationToken = 0;
        waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    public void Start()
    {
        RegistrationServices registrationServices = new RegistrationServices();
        comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

        // Either have the background task signal this handle when it completes, or never signal this handle to keep this
        // process as the COM server until the process is closed.
        waitHandle.WaitOne();
    }

    public void Stop()
    {
        if (comRegistrationToken != 0)
        {
            RegistrationServices registrationServices = new RegistrationServices();
            registrationServices.UnregisterTypeForComClients(registrationCookie);
        }

        waitHandle.Set();
    }

    private int comRegistrationToken;
    private EventWaitHandle waitHandle;
}

var sampleTaskServer = new SampleTaskServer();
sampleTaskServer.Start();

```

```cppwinrt

class SampleTaskServer
{
public:
    SampleTaskServer()
    {
        waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
        comRegistrationToken = 0;
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    void Start()
    {
        try
        {
            com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

            winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                       taskFactory.get(),
                                                       CLSCTX_LOCAL_SERVER,
                                                       REGCLS_MULTIPLEUSE,
                                                       &comRegistrationToken));

            // Either have the background task signal this handle when it completes, or never signal this handle to
            // keep this process as the COM server until the process is closed.
            waitHandle.WaitOne();

        }
        catch (...)
        {
            // Indicate an error has been encountered.
        }
    }

    void Stop()
    {
        if (comRegistrationToken != 0)
        {
            CoRevokeClassObject(comRegistrationToken);
        }

        waitHandle.Set();
    }

private:
    DWORD comRegistrationToken;
    EventWaitHandle waitHandle;
};

SampleTaskServer sampleTaskServer;
sampleTaskServer.Start();

```


## <a name="register-the-background-task-to-run"></a>実行するバックグラウンド タスクを登録する

1.  バックグラウンドタスクが既に登録されているかどうかを確認するには、 [**Backgroundtaskregistration. AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) プロパティを繰り返します。 *この手順は重要*です。アプリが既存のバックグラウンドタスクの登録を確認しない場合は、タスクを複数回登録すると、パフォーマンスに関する問題が発生し、作業が完了するまでタスクの利用可能な CPU 時間が不足する可能性があります。 アプリケーションでは、同じエントリポイントを使用してすべてのバックグラウンドタスクを処理し、 [**Backgroundtaskregistration**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration)に割り当てられた[**名前**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.name#Windows_ApplicationModel_Background_BackgroundTaskRegistration_Name)や[**TaskId**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.taskid#Windows_ApplicationModel_Background_BackgroundTaskRegistration_TaskId)などの他のプロパティを使用して、どのような作業を行う必要があるかを判断できます。

次の例では、 **Alltasks** プロパティを反復処理し、タスクが既に登録されている場合はフラグ変数を true に設定します。

```csharp

var taskRegistered = false;
var sampleTaskName = "SampleTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

```cppwinrt

bool taskRegistered = false;
std::wstring sampleTaskName = L"SampleTask";
auto allTasks = BackgroundTaskRegistration::AllTasks();

for (auto const& task : allTasks)
{
    if (task.Value().Name() == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

1.  バックグラウンド タスクがまだ登録されていない場合は、[**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) を使ってバックグラウンド タスクのインスタンスを作成します。 タスクのエントリ ポイントには、名前空間を含めてバックグラウンド タスク クラスの名前を指定します。

バックグラウンド タスク トリガーは、バックグラウンド タスクが実行されるタイミングを制御します。 考えられるトリガーの一覧については、「 [**Windows アプリケーション**](/uwp/api/windows.applicationmodel.background) の名前空間」を参照してください。

> [!NOTE]
> パッケージ化された winmain バックグラウンドタスクでは、トリガーのサブセットのみがサポートされます。

たとえば、次のコードでは、新しいバックグラウンドタスクを作成し、15分間の定期的な [**TimeTrigger**]()で実行するように設定しています。

```csharp

if (!taskRegistered)
{
    var builder = new BackgroundTaskBuilder();

    builder.Name = sampleTaskName;
    builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
    builder.SetTrigger(new TimeTrigger(15, false));
}

// The code in the next step goes here.

```

```cppwinrt

if (!taskRegistered)
{
    BackgroundTaskBuilder builder;

    builder.Name(sampleTaskName);
    builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
    builder.SetTrigger(TimeTrigger(15, false));
}

// The code in the next step goes here.

```

1.  トリガー イベントの発生後、どのようなときにタスクを実行するかという条件を追加することもできます (省略可能)。 たとえば、インターネットが使用可能になるまでタスクを実行したくない場合は、 **Internetavailable**という条件を使用します。 指定できる条件の一覧については、「[**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)」をご覧ください。

次のサンプル コードでは、"ユーザーの存在" を条件として割り当てています。

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.InternetAvailable));
// The code in the next step goes here.
```

```cppwinrt
builder.AddCondition(SystemCondition{ SystemConditionType::InternetAvailable });
// The code in the next step goes here.
```

4.  [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) オブジェクトで Register メソッドを呼び出してバックグラウンド タスクを登録します。 [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) の結果は、次の手順に備えて保存します。 Register 関数は、例外の形式でエラーを返す場合があることに注意してください。 必ず、try catch で Register を呼び出してください。

バックグラウンド タスクを登録し、その結果を保存するコードを次に示します。

```csharp

try
{
    var task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

```cppwinrt

try
{
    auto task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

## <a name="bringing-it-all-together"></a>まとめ

次のコードサンプルは、COM winmain バックグラウンドタスクを実行して登録するために必要な完全なコードを示しています。

<details>
<summary>完全な winmain アプリパッケージマニフェスト</summary>
<p>

```xml

<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  IgnorableNamespaces="uap rescap com">

  <Identity
    Name="SamplePackagedWinMainBackgroundApp"
    Publisher="CN=Contoso"
    Version="1.0.0.0" />

  <Properties>
    <DisplayName>SamplePackagedWinMainBackgroundApp</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Images\StoreLogo.png</Logo>
  </Properties>

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
  </Dependencies>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Applications>
    <Application Id="App"
                 Executable="SampleBackgroundApp\$targetnametoken$.exe"
                 EntryPoint="$targetentrypoint$">

      <uap:VisualElements
        DisplayName="SampleBackgroundApp"
        Description="SampleBackgroundApp"
        BackgroundColor="transparent"
        Square150x150Logo="Images\Square150x150Logo.png"
        Square44x44Logo="Images\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Images\Wide310x150Logo.png" />
        <uap:SplashScreen Image="Images\SplashScreen.png" />
      </uap:VisualElements>

      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
              <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>
      </Extensions>
    </Application>
  </Applications>

  <Capabilities>
  <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>

```

</p>
</details>


<details>
<summary>バックグラウンドタスクの完了コードのサンプル</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }


    // COM server startup code.
    class SampleTaskServer
    {
        SampleTaskServer()
        {
            comRegistrationToken = 0;
            waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        public void Start()
        {
            RegistrationServices registrationServices = new RegistrationServices();
            comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

            // Either have the background task signal this handle when it completes, or never signal this handle to keep this
            // process as the COM server until the process is closed.
            waitHandle.WaitOne();
        }

        public void Stop()
        {
            if (comRegistrationToken != 0)
            {
                RegistrationServices registrationServices = new RegistrationServices();
                registrationServices.UnregisterTypeForComClients(registrationCookie);
            }

            waitHandle.Set();
        }

        private int comRegistrationToken;
        private EventWaitHandle waitHandle;
    }


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            var taskRegistered = false;
            var sampleTaskName = "SampleTask";

            foreach (var task in BackgroundTaskRegistration.AllTasks)
            {
                if (task.Value.Name == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                var builder = new BackgroundTaskBuilder();

                builder.Name = sampleTaskName;
                builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
                builder.SetTrigger(new TimeTrigger(15, false));
            }

            try
            {
                var task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }


    // Application entry point.
    static class Program
    {
        [MTAThread]
        static void Main()
        {
            string[] commandLineArgs = Environment.GetCommandLineArgs();
            if (commandLineArgs.Length < 2)
            {
                // Open the WPF UI when no arguments are specified.
            }
            else
            {
                if (commandLineArgs.Contains("-RegisterSampleTask", StringComparer.InvariantCultureIgnoreCase))
                {
                    SampleTaskRegistrar.Register();
                }

                if (commandLineArgs.Contains("-StartSampleTaskServer", StringComparer.InvariantCultureIgnoreCase))
                {
                    var sampleTaskServer = new SampleTaskServer();
                    sampleTaskServer.Start();
                }
            }

            return;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int maxPrimeNumber = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < maxPrimeNumber))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != nullptr) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };


    // COM server startup code.
    class SampleTaskServer
    {
    public:
        SampleTaskServer()
        {
            waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
            comRegistrationToken = 0;
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        void Start()
        {
            try
            {
                com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

                winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                           taskFactory.get(),
                                                           CLSCTX_LOCAL_SERVER,
                                                           REGCLS_MULTIPLEUSE,
                                                           &comRegistrationToken));

                // Either have the background task signal this handle when it completes, or never signal this handle to
                // keep this process as the COM server until the process is closed.
                waitHandle.WaitOne();

            }
            catch (...)
            {
                // Indicate an error has been encountered.
            }
        }

        void Stop()
        {
            if (comRegistrationToken != 0)
            {
                CoRevokeClassObject(comRegistrationToken);
            }

            waitHandle.Set();
        }

    private:
        DWORD comRegistrationToken;
        EventWaitHandle waitHandle;
    };


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            bool taskRegistered = false;
            std::wstring sampleTaskName = L"SampleTask";
            auto allTasks = BackgroundTaskRegistration::AllTasks();

            for (auto const& task : allTasks)
            {
                if (task.Value().Name() == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                BackgroundTaskBuilder builder;

                builder.Name(sampleTaskName);
                builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
                builder.SetTrigger(TimeTrigger(15, false));
            }

            try
            {
                auto task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }

}

using namespace PackagedWinMainBackgroundTaskSample;

// Application entry point.
int wmain(_In_ int argc, _In_reads_(argc) const wchar** argv)
{
    unsigned int argumentIndex;

    winrt::init_apartment();

    if (argc <= 1)
    {
        return E_INVALIDARG;
    }

    for (argumentIndex = 0; argumentIndex < argc ; argumentIndex += 1)
    {
        if (_wcsnicmp(L"RegisterSampleTask",
                      argv[argumentIndex],
                      wcslen(L"RegisterSampleTask")) == 0)
        {
            SampleTaskRegistrar::Register();
        }

        if (_wcsnicmp(L"StartSampleTaskServer",
                      argv[argumentIndex],
                      wcslen(L"StartSampleTaskServer")) == 0)
        {
            SampleTaskServer sampleTaskServer;
            sampleTaskServer.Start();
        }
    }

    return S_OK;
}

```

</p>
</details>


## <a name="remarks"></a>注釈

モダンスタンバイでバックグラウンドタスクを実行できる UWP アプリとは異なり、WinMain アプリは最新のスタンバイの低電力フェーズからコードを実行できません。 詳細については、「 [最新のスタンバイ](/windows-hardware/design/device-experiences/modern-standby) 」を参照してください。

API リファレンス、バックグラウンド タスクの概念的ガイダンス、バックグラウンド タスクを使ったアプリの作成に関する詳しい説明については、次の関連するトピックをご覧ください。

## <a name="related-topics"></a>関連トピック

* [バックグラウンド タスクによるシステム イベントへの応答](respond-to-system-events-with-background-tasks.md)
* [バックグラウンド タスクの登録](register-a-background-task.md)
* [バックグラウンド タスクを実行するための条件の設定](set-conditions-for-running-a-background-task.md)
* [メンテナンス トリガーの使用](use-a-maintenance-trigger.md)
* [取り消されたバックグラウンド タスクの処理](handle-a-cancelled-background-task.md)
* [バックグラウンド タスクの進捗状況と完了の監視](monitor-background-task-progress-and-completion.md)
* [タイマーでのバックグラウンド タスクの実行](run-a-background-task-on-a-timer-.md)
* [インプロセスバックグラウンドタスクを作成して登録](create-and-register-an-inproc-background-task.md)します。
* [アウトプロセスのバックグラウンド タスクをインプロセスのバックグラウンド タスクへ変換](convert-out-of-process-background-task.md)

**バックグラウンド タスクのガイダンス**

* [バックグラウンド タスクのガイドライン](guidelines-for-background-tasks.md)
* [バックグラウンド タスクのデバッグ](debug-a-background-task.md)
* [UWP アプリで一時停止イベント、再開イベント、バックグラウンド イベントをトリガーする方法 (デバッグ時)](/previous-versions/hh974425(v=vs.110))

**バックグラウンド タスクの API リファレンス**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)