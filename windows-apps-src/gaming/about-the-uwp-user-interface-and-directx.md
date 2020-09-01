---
title: アプリ オブジェクトと DirectX
description: DirectX を使ったユニバーサル Windows プラットフォーム (UWP) ゲームでは、Windows UI ユーザー インターフェイスの要素とオブジェクトの多くが使われません。
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10、UWP、DirectX、アプリ オブジェクト
ms.localizationpriority: medium
ms.openlocfilehash: 29eaba70a7114624474275b8f98ec77f8038b2b0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163166"
---
# <a name="the-app-object-and-directx"></a>アプリ オブジェクトと DirectX

DirectX を使ったユニバーサル Windows プラットフォーム (UWP) ゲームでは、Windows UI ユーザー インターフェイスの要素とオブジェクトの多くが使われません。 逆に、それらのアプリは Windows ランタイム スタックの下位レベルで実行されることから、アプリ オブジェクトに直接アクセスして相互運用するという基本的な方法でユーザー インターフェイス フレームワークと相互運用する必要があります。 ここでは、この相互運用をいつどのように行うかと、DirectX 開発者が UWP アプリの開発でこのモデルを効果的に使う方法を説明します。

読み取り中に発生した未知のグラフィックス用語や概念については、「 [Direct3D グラフィックス用語集](../graphics-concepts/index.md) 」を参照してください。

## <a name="the-important-core-user-interface-namespaces"></a>重要なコア ユーザー インターフェイスの名前空間

最初に、UWP アプリに (**using** を使って) 必要な Windows ランタイムの名前空間を示します。 後で詳しく説明します。

-   [**Windows. ApplicationModel. コア**](/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows. ApplicationModel. Activation**](/uwp/api/Windows.ApplicationModel.Activation)
-   [**Windows. UI. コア**](/uwp/api/Windows.UI.Core)
-   [**Windows.System**](/uwp/api/Windows.System)
-   [**Windows.Foundation**](/uwp/api/Windows.Foundation)

> [!NOTE]
> UWP アプリを開発していない場合は、これらの名前空間で提供される型ではなく、JavaScript または XAML 固有のライブラリおよび名前空間で提供されるユーザーインターフェイスコンポーネントを使用します。

## <a name="the-windows-runtime-app-object"></a>Windows ランタイム アプリ オブジェクト

UWP アプリでは、ウィンドウとビュー プロバイダーを取得します。これらはビューの取得元となり、スワップ チェーン (表示バッファー) の接続先となります。 このビューは、実行中のアプリでウィンドウ固有のイベントにフックすることもできます。 [**Corewindow**](/uwp/api/Windows.UI.Core.CoreWindow)型によって定義されたアプリオブジェクトの親ウィンドウを取得するには、 [**Iframeworkviewsource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource)を実装する型を作成します。 **Iframeworkviewsource**を実装する方法を示す[C++/WinRT](../cpp-and-winrt-apis/index.md)コード例については、「[コンポジションと DirectX および Direct2D とのネイティブ相互運用](../composition/composition-native-interop.md)」を参照してください。

コアユーザーインターフェイスフレームワークを使用してウィンドウを取得するための基本的な手順を次に示します。

1.  [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) を実装する型を作成します。 これがビューになります。

    この型で次のメソッドを定義します。

    -   [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) メソッド。[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) のインスタンスをパラメーターとして使います。 この型のインスタンスを取得するには、[**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) を呼び出します。 アプリ オブジェクトは、アプリ起動時にこのメソッドを呼び出します。
    -   [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) メソッド。[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) のインスタンスをパラメーターとして使います。 この型のインスタンスを取得するには、新しい [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) インスタンスの [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) プロパティにアクセスします。
    -   [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) メソッド。エントリ ポイントの文字列を唯一のパラメーターとして使います。 このメソッドを呼び出すと、アプリ オブジェクトからエントリ ポイントの文字列が提供されます。 ここでリソースをセットアップします。 ここでデバイス リソースを作成します。 アプリ オブジェクトは、アプリ起動時にこのメソッドを呼び出します。
    -   [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) メソッド。[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) オブジェクトをアクティブ化し、ウィンドウ イベント ディスパッチャーを開始します。 アプリ オブジェクトは、アプリのプロセスが開始されたときにこのメソッドを呼び出します。
    -   [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) メソッド。[**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)  の呼び出しでセットアップされたリソースをクリーンアップします。 アプリ オブジェクトは、アプリ終了時にこのメソッドを呼び出します。

2.  [**IFrameworkViewSource**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkViewSource) を実装する型を作成します。 これがビュー プロバイダーになります。

    この型で次のメソッドを定義します。

    -   [**CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview) メソッド。手順 1. で作成した [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 実装のインスタンスを返します。

3.  ビュー プロバイダーのインスタンスを、**main** から [**CoreApplication.Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) に渡します。

上記の基本事項を踏まえて、このアプローチを拡張するその他のオプションを説明します。

## <a name="core-user-interface-types"></a>コア ユーザー インターフェイスの型

Windows ランタイムには、他にも次のような便利なコア ユーザー インターフェイスの型があります。

-   [**Windows. ApplicationModel. CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
-   [**Windows.UI.Core.CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows.UI.Core.CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)

これらの型を使って、アプリのビュー、具体的にはアプリの親ウィンドウのコンテンツを描画するビットにアクセスし、そのウィンドウで発生したイベントを処理できます。 アプリ ウィンドウのプロセスは、すべてのコールバックを処理する独立した *アプリケーション シングルスレッド アパートメント* (ASTA) です。

アプリのビューは、アプリ ウィンドウのビュー プロバイダーによって生成され、ほとんどの場合、特定のフレームワーク パッケージまたはシステム自体によって実装されるため、手動で実装する必要はありません。 DirectX の場合は、既に説明したように手動でシン ビュー プロバイダーを実装する必要があります。 次のコンポーネントと動作の間には、明確な一対一の関係があります。

-   アプリのビュー。[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) 型で表され、ウィンドウを更新するためのメソッドを定義します。
-   ASTA。その属性で、アプリのスレッド動作を定義します。 ASTA で COM STA 属性型のインスタンスを作成することはできません。
-   ビュー プロバイダー。これはアプリがシステムから取得するか、手動で実装します。
-   親ウィンドウ。[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 型で表されます。
-   すべてのアクティブ化イベントのソースです。 ビューとウィンドウの両方に、独立したアクティブ化イベントがあります。

要するに、アプリ オブジェクトがビュー プロバイダー ファクトリを提供します。 このファクトリがビュー プロバイダーを作成し、アプリの親ウィンドウをインスタンス化します。 ビュー プロバイダーが、アプリの親ウィンドウに対してアプリのビューを定義します。 次に、ビューと親ウィンドウについて詳しく説明します。

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView の動作とプロパティ

[**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) は、現在のアプリ ビューを表します。 アプリ ビューは、初期化時にアプリ シングルトンによって作成されますが、アクティブ化されるまでアクティブになりません。 ビューを表示する [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) を取得するには、そのビューの [**CoreApplicationView.CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) プロパティにアクセスします。また、ビューのアクティブ化イベントと非アクティブ化イベントを処理するには、[**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) イベントにデリゲートを登録します。

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow の動作とプロパティ

アプリ オブジェクトが初期化されるときに、[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) インスタンスである親ウィンドウが作成され、ビュー プロバイダーに渡されます。 アプリに表示するウィンドウがある場合はそれが表示され、そうでない場合はビューの初期化のみが行われます。

[**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) では、入力および基本のウィンドウ動作専用のイベントを多数提供しています。 これらのイベントを処理するには、イベントに自分専用のデリゲートを登録します。

また、 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)のインスタンスを提供する[**corewindow. dispatcher**](/uwp/api/windows.ui.core.corewindow.dispatcher)プロパティにアクセスして、ウィンドウのウィンドウイベントディスパッチャーを取得することもできます。

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher の動作とプロパティ

[**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 型のウィンドウに使うイベント ディスパッチのスレッド動作を決定できます。 この型には、ウィンドウ イベント処理を開始する特に重要なメソッド [**CoreDispatcher.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) があります。 アプリで間違ったオプションを指定してこのメソッドを呼び出すと、さまざまな予期しないイベント処理動作が発生する可能性があります。

| CoreProcessEventsOption のオプション                                                           | 説明                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) | 現在キューに入っているすべてのイベントをディスパッチします。 保留中のイベントがない場合は、次の新しいイベントを待ちます。                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | キュー内で保留中のイベントを 1 つだけディスパッチします。 保留中のイベントがない場合は、新しいイベントの発生を待たずに直ちに制御を返します。                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)        | 新しいイベントを待ち、発生したイベントをすべてディスパッチします。 この動作を、ウィンドウが閉じられるか、アプリが [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) インスタンスで [**Close**](/uwp/api/windows.ui.core.corewindow.close) メソッドを呼び出すまで継続します。 |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption)     | 現在キューに入っているすべてのイベントをディスパッチします。 保留中のイベントがない場合は、直ちに制御を返します。                                                                                                                                          |
DirectX を使った UWP アプリでは、[**CoreProcessEventsOption.ProcessAllIfPresent**](/uwp/api/Windows.UI.Core.CoreProcessEventsOption) オプションを使って、グラフィックスの更新を中断する可能性があるブロック動作を防ぐ必要があります。

## <a name="asta-considerations-for-directx-devs"></a>DirectX 開発者向けの ASTA の考慮事項

DirectX を使った UWP アプリの実行時の形式を定義するアプリ オブジェクトは、アプリケーション シングルスレッド アパートメント (ASTA) というスレッド モデルを使ってアプリの UI ビューをホストします。 DirectX を使った UWP アプリを開発している場合、そのアプリからディスパッチするすべてのスレッドは [**Windows::System::Threading**](/uwp/api/Windows.System.Threading) API または [**CoreWindow::CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) を使う必要があるので、ASTA のプロパティについては十分に理解していると考えられます  (アプリから [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) を呼び出すことで ASTA の [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) オブジェクトを取得できます)。

UWP DirectX アプリの開発者として知っておくべき最も重要な点は、 **main ()** に**Platform:: MTAThread**を設定することにより、アプリスレッドで MTA スレッドをディスパッチできるようにする必要があることです。

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

DirectX を使った UWP アプリのアプリ オブジェクトは、アクティブ化されると、UI ビューに使われる ASTA を作成します。 新しい ASTA スレッドは、ビュー プロバイダー ファクトリを呼び出して、アプリ オブジェクトのビュー プロバイダーを作成します。その結果、ビュー プロバイダー コードがこの ASTA スレッド上で実行されます。

また、ASTA から分離するスレッドは MTA にある必要があります。 分離する MTA スレッドには、引き続き再入の問題が発生して、デッドロックとなる可能性があることに注意してください。

ASTA スレッドで実行するために元のコードを移植している場合、これらの考慮事項に注意してください。

-   [**CoWaitForMultipleObjects**](/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultipleobjects) などの待機プリミティブは、STA 内と ASTA 内とで動作が異なります。
-   COM 呼び出しモーダル ループの動作は ASTA では異なります。 呼び出しの進行中は、無関係な呼び出しを受け取ることができなくなります。 たとえば、次の動作は ASTA からデッドロックを引き起こします (さらに、即座にアプリがクラッシュします)。
    1.  ASTA が MTA オブジェクトを呼び出し、インターフェイス ポインター P1 を渡します。
    2.  その後で、ASTA が同じ MTA オブジェクトを呼び出します。 MTA オブジェクトは、ASTA に制御を返す前に P1 を呼び出します。
    3.  P1 は、無関係な呼び出しがブロックされているため、ASTA に入ることができません。 しかし、MTA スレッドは P1 の呼び出しを試みるのでブロックされます。

    これは次の方法で解決できます。
    -   並列パターン ライブラリ (PPLTasks.h) で定義された **async** パターンを使います。
    -   できるだけ早くアプリの ASTA (アプリのメイン スレッド) から [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) を呼び出して、任意の呼び出しを許可します。

    しかし、アプリの ASTA への無関係な呼び出しの即時の配信に頼ることはできません。 非同期呼び出しの詳細については、「 [C++ での非同期プログラミング](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md)」を参照してください。

全体的に見れば、UWP アプリをデザインする場合、自分で MTA スレッドを作って管理しようとするのではなく、アプリの [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) の [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) と [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) を使ってすべての UI スレッドを処理します。 **CoreDispatcher** で処理できない個別のスレッドが必要な場合、非同期パターンを使い、再入の問題を回避するために前のガイドに従います。