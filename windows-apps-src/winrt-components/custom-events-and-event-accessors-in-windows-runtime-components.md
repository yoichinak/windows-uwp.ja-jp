---
title: Windows ランタイム コンポーネントのカスタム イベントおよびイベント アクセサー
description: .NET による Windows ランタイムコンポーネントのサポートにより、ユニバーサル Windows プラットフォーム (UWP) イベントパターンと .NET イベントパターンの違いを非表示にすることで、イベントコンポーネントを簡単に宣言できるようになります。
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b1666d938a83f8c8725523d3e5a1b14e416ca4e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174276"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Windows ランタイム コンポーネントのカスタム イベントおよびイベント アクセサー

.NET による Windows ランタイムコンポーネントのサポートにより、ユニバーサル Windows プラットフォーム (UWP) イベントパターンと .NET イベントパターンの違いを非表示にすることで、イベントコンポーネントを簡単に宣言できるようになります。 ただし、Windows ランタイムコンポーネントでカスタムイベントアクセサーを宣言する場合は、UWP で使用されるパターンに従う必要があります。

## <a name="registering-events"></a>イベントの登録

UWP のイベントを処理するための登録を行うと、add アクセサーはトークンを返します。 登録を解除するには、このトークンを remove アクセサーに渡します。 これは、UWP イベントの add と remove アクセサーが、これまで使ってきたアクセサーとは異なるシグニチャを持つことを意味します。

幸いにも、Visual Basic と C# コンパイラは、このプロセスを簡略化しています。 Windows ランタイムコンポーネントでカスタムアクセサーを持つイベントを宣言すると、コンパイラは UWP パターンを自動的に使用します。 たとえば、add アクセサーがトークンを返さない場合、コンパイル エラーが発生します。 .NET には、実装をサポートする2つの型が用意されています。

-   [EventRegistrationToken](/uwp/api/windows.foundation.eventregistrationtoken) 構造体はトークンを表します。
-   [EventRegistrationTokenTable&lt;T&gt;](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) クラスはトークンを作成し、トークンとイベント ハンドラーの間の対応付けを保持します。 ジェネリック型引数は、イベント引数の型です。 イベント ハンドラーがイベントに対して最初に登録されたときに、イベントごとにこのクラスのインスタンスを作成します。

NumberChanged イベントの次のコードは、UWP イベントの基本パターンを示しています。 この例では、イベント引数オブジェクトのコンストラクターである NumberChangedEventArgs は、変更された数値を表す単一の整数パラメーターを受け取ります。

> **メモ**   これは、Windows ランタイムコンポーネントで宣言する通常のイベントにコンパイラが使用するパターンと同じです。

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

static (Visual Basic では Shared) GetOrCreateEventRegistrationTokenTable メソッドは、イベントの EventRegistrationTokenTable&lt;T&gt; オブジェクトのインスタンスを限定的に作成します。 トークン テーブルのインスタンスを保持するクラス レベルのフィールドを、このメソッドに渡します。 フィールドが空の場合、メソッドはテーブルを作成し、テーブルへの参照をフィールドに格納し、テーブルへの参照を返します。 フィールドにトークン テーブルへの参照が既に含まれている場合、このメソッドはその参照を返します。

> **重要**   スレッドセーフを確保するには、EventRegistrationTokenTable T のイベントのインスタンスを保持するフィールド &lt; &gt; がクラスレベルのフィールドである必要があります。 クラス レベルのフィールドである場合、GetOrCreateEventRegistrationTokenTable メソッドでは、複数のスレッドがトークン テーブルの作成を試みるときに、すべてのスレッドでテーブルの同じインスタンスが取得されます。 特定のイベントでは、GetOrCreateEventRegistrationTokenTable メソッドのすべての呼び出しは、同じクラス レベルのフィールドを使う必要があります。

remove アクセサーや [RaiseEvent](/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) メソッド (C# では OnRaiseEvent メソッド) で GetOrCreateEventRegistrationTokenTable メソッドを呼び出すことによって、イベント ハンドラー デリゲートが追加される前にこれらのメソッドを呼び出した場合、例外は発生しません。

UWP イベント パターンで使われる EventRegistrationTokenTable&lt;T&gt; クラスの他のメンバーには、次のものがあります。

-   [AddEventHandler](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) メソッドは、イベント ハンドラー デリゲートのトークンを生成し、デリゲートをテーブルに保存し、デリゲートを呼び出しリストに追加して、トークンを返します。
-   [RemoveEventHandler(EventRegistrationToken)](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) メソッド オーバーロードは、テーブルと呼び出しリストからデリゲートを削除します。

    >**メモ**   AddEventHandler メソッドと RemoveEventHandler (EventRegistrationToken) メソッドは、スレッドセーフを確保するためにテーブルをロックします。

-   [InvocationList](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) プロパティは、イベントを処理するために現在登録されているすべてのイベント ハンドラーを含むデリゲートを返します。 このデリゲートを使ってイベントを発生させるか、Delegate クラスのメソッドを使ってハンドラーを個別に呼び出します。

    >**メモ**   この記事で前に示した例に示されているパターンに従い、デリゲートを呼び出し前に一時変数にコピーすることをお勧めします。 これにより、あるスレッドが最後のハンドラーを削除して、別のスレッドがデリゲートを呼び出す直前にデリゲートが null となる競合状態を回避できます。 デリゲートは変更できないため、コピーは引き続き有効です。

必要に応じて、独自のコードをアクセサーに配置します。 スレッド セーフが問題の場合、独自のロックをコードに提供する必要があります。

C# ユーザー: UWP イベント パターンでカスタム イベント アクセサーを作成すると、コンパイラは通常の構文のショートカットを提供しません。 コードでイベント名を使うとエラーが発生します。

Visual Basic ユーザー: .NET では、イベントは、登録されているすべてのイベントハンドラーを表すマルチキャストデリゲートにすぎません。 イベントを発生させることは、デリゲートを呼び出すことを意味します。 一般に、Visual Basic の構文はデリゲートとの対話を非表示にします。また、スレッド セーフに関するメモに説明されているように、コンパイラはデリゲートを呼び出す前にデリゲートをコピーします。 Windows ランタイムコンポーネントでカスタムイベントを作成する場合は、デリゲートを直接処理する必要があります。 これは、たとえばハンドラーを個別に呼び出す場合、[MulticastDelegate.GetInvocationList](/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) メソッドを使って、イベント ハンドラーごとに個別のデリゲートが含まれる配列を取得できることも意味します。

## <a name="related-topics"></a>関連トピック

* [イベント (Visual Basic)](/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [イベント (C# プログラミング ガイド)](/dotnet/articles/csharp/programming-guide/events/index)
* [UWP アプリ用 .NET の概要](/previous-versions/windows/apps/br230302(v=vs.140))
* [UWP アプリ用 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)
* [C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)