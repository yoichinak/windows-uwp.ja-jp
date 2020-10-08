---
description: Windows アプリでは、windows デバイスでサポートされているテキストサービスからテキスト入力を受け取ることができます。コア名前空間のコアテキスト Api を使用できます。
title: カスタム テキスト入力の概要
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: キーボード, テキスト, 基本的なテキスト, カスタム テキスト, テキスト サービス フレームワーク, 入力, ユーザー操作
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 95dbd6de78cb6670ea7e904252bbc1f9f14edb77
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829632"
---
# <a name="custom-text-input"></a>カスタム テキスト入力



Windows アプリでは、windows デバイスでサポートされているテキストサービスからテキスト入力を受け取ることができます。 [**コア名前空間**](/uwp/api/Windows.UI.Text.Core) のコアテキスト api を使用できます。 この API は、アプリがテキスト サービスの詳細を認識している必要がないという点で、[テキスト サービス フレームワーク](/windows/desktop/TSF/text-services-framework) API に似ています。 これにより、アプリは、任意の言語で、キーボード、音声、ペンなどの任意の入力の種類からテキストを受け取ることができます。

> **重要な API**: [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core)、[**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>基本的なテキスト API を使う理由


多くのアプリでは、テキストの入力や編集には XAML や HTML のテキスト ボックス コントロールで十分です。 ただし、ワード プロセッシング アプリなど、アプリでテキストの複雑なシナリオを処理する場合は、柔軟なカスタム テキスト編集コントロールが必要になる可能性があります。 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) キーボード API を使ってテキスト編集コントロールを作成できますが、これらは、東アジアの言語をサポートするために必要なコンポジション ベースのテキスト入力を受け取る方法を提供しません。

カスタム テキスト編集コントロールを作成する必要がある場合は、代わりに [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core) API を使います。 これらの API は、任意の言語でのテキスト入力の処理において高い柔軟性を実現するように設計されており、アプリに最適なテキスト エクスペリエンスを提供できます。 基本的なテキスト API に組み込まれているテキスト入力および編集コントロールは、Windows デバイスでの既存のすべてのテキスト入力方式からテキスト入力を受け取ることができます。これには、[テキスト サービス フレームワーク](/windows/desktop/TSF/text-services-framework) ベースの入力方式エディター (IME) や PC での手書き入力、モバイル デバイスでの WordFlow キーボード (自動修正、予測入力、ディクテーションを提供する) が含まれます。

## <a name="architecture"></a>Architecture


テキスト入力システムの単純な例を次に示します。

-   "アプリケーション" は、コアテキスト Api を使用して構築されたカスタムエディットコントロールをホストする Windows アプリを表します。
-   [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core) API は、Windows 経由でのテキスト サービスとの通信を容易にします。 テキスト編集コントロールとテキスト サービス間の通信は、主に [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) オブジェクトによって処理されます。このオブジェクトが通信を容易にするメソッドとイベントを提供します。

![CoreText アーキテクチャの図](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>テキスト範囲と選択範囲


編集コントロールはテキスト入力用の領域を提供し、ユーザーはこの領域の任意の場所でテキストを編集できることを期待しています。 ここでは、基本的なテキスト API で使用されるテキスト配置システムについて説明します。また、範囲と選択範囲がこのシステムでどのように表現されるかについても説明します。

### <a name="application-caret-position"></a>アプリケーションのカーソル位置

基本的なテキスト API で使用されるテキスト範囲は、カーソル位置の観点で表されます。 "アプリケーション カーソル位置 (ACP)" は、次に示すように、カーソルの直前にあるテキスト ストリームの先頭からの文字数を示す 0 から始まる数値です。

![アプリケーションのキャレット位置 (ACP) の文字数を示すスクリーンショット](images/coretext/stream-1.png)

### <a name="text-ranges-and-selection"></a>テキスト範囲と選択範囲

テキスト範囲と選択範囲は、次の 2 つのフィールドが含まれる [**CoreTextRange**](/uwp/api/Windows.UI.Text.Core.CoreTextRange) 構造体で表されます。

| フィールド                  | データ型                                                                 | 説明                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **番号** \[Java\] | **System.Int32** System.string \[ .net\] | **int32** \[C++\] | 範囲の開始位置は、最初の文字の直前の ACP です。 |
| **EndCaretPosition**   | **番号** \[Java\] | **System.Int32** System.string \[ .net\] | **int32** \[C++\] | 範囲の終了位置は、最後の文字の直後の ACP です。     |

 

たとえば、前に示したテキスト範囲では、0 ~ 5 の範囲で \[ \] "Hello" という語が指定されています。 **StartCaretPosition** は、常に **EndCaretPosition** 以下である必要があります。 範囲 \[ 5、0 \] は無効です。

### <a name="insertion-point"></a>挿入ポイント

現在のカーソル位置は挿入ポイントとも呼ばれ、**StartCaretPosition** を **EndCaretPosition** と等しくなるように設定することによって表されます。

### <a name="noncontiguous-selection"></a>連続しない選択範囲

一部の編集コントロールでは、連続しない選択範囲がサポートされます。 たとえば、Microsoft Office アプリでは任意の複数の選択範囲がサポートされ、多くのソース コード エディターでは列の選択がサポートされています。 ただし、コアテキスト Api では、連続していない選択はサポートされません。 編集コントロールは、1 つ連続した選択範囲のみをレポートする必要があります。これは通常、連続しない選択範囲のアクティブな下位範囲です。

たとえば、次の図は、2つの連続していない選択を含むテキストストリームを示しています。 \[ 0、1、 \] 6、11です。この場合、 \[ \] エディットコントロールは1つだけをレポートする必要があります ( \[ 0、1、 \] または \[ 6、11 \] )。

![連続していないテキスト選択を示すスクリーンショット。最初の文字と最後の5文字が選択されます。](images/coretext/stream-2.png)

## <a name="working-with-text"></a>テキストの操作


[**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) クラスの [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベント、[**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベント、[**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) メソッドによって、Windows と編集コントロールとの間でのテキスト フローを実現できます。

編集コントロールは、ユーザーがキーボード、音声、IME などのテキスト入力方式を操作したときに生成される [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベントによってテキストを受け取ります。

テキストをコントロールに貼り付けるなどの方法で、編集コントロールでテキストを変更する場合、[**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) を呼び出して Windows に通知する必要があります。

テキスト サービスが新しいテキストを必要とする場合、[**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベントが発生します。 **TextRequested** イベント ハンドラーで、新しいテキストを提供する必要があります。

### <a name="accepting-text-updates"></a>テキストの更新の受け付け

編集コントロールでは、通常、テキストの更新要求を受け付ける必要があります。これらは、ユーザーが入力するテキストを表しているためです。 [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベント ハンドラーでは、編集コントロールの次の操作が想定されています。

1.  [**CoreTextTextUpdatingEventArgs.Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) で指定されたテキストを、[**CoreTextTextUpdatingEventArgs.Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range) で指定された位置に挿入します。
2.  [**CoreTextTextUpdatingEventArgs.NewSelection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection) で指定された位置に選択範囲を配置します。
3.  [**Coretexttextupdatingeventargs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result)を設定して、更新が成功したことをシステムに通知します。結果は[**Coretexttextupdatingeventargs. succeeded**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult)になります。

たとえば、これは、ユーザーが "d" を入力する前の編集コントロールの状態です。 挿入ポイントは \[ 10, 10 \] です。

![挿入位置を示すテキストストリームダイアグラムのスクリーンショット ( \[ 10, 10 \] , 挿入前)](images/coretext/stream-3.png)

ユーザーが "d" を入力すると、次の [**CoreTextTextUpdatingEventArgs**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs) データを使って、[**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベントが発生します。

-   [**Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)  =  範囲 \[10、10\]
-   [**Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**Newselection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)  =  \[11、11\]

編集コントロールで、指定された変更を適用し、[**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) を **Succeeded** に設定します。 変更が適用された後のコントロールの状態は、次のようになります。

:::image type="content" source="images/coretext/stream-4.png" alt-text="挿入位置を示すテキストストリームダイアグラムのスクリーンショット (11 \[ , 11 \] )":::

### <a name="rejecting-text-updates"></a>テキストの更新の拒否

要求された範囲が編集コントロールの変更してはいけない領域にある場合、テキストの更新を適用できないことがあります。 この場合、変更を適用しないでください。 代わりに、 [**Coretexttextupdatingeventargs**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) を設定して、更新が失敗したことをシステムに通知します。結果は [**Coretexttextupdatingeventargs. failed**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

たとえば、電子メール アドレスのみを受け付ける編集コントロールがあるとします。 電子メール アドレスにスペースを含めることはできないため、スペースは拒否する必要があります。そのため、Space キーについて [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベントが発生した場合は、編集コントロールで単に [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) を **Failed** に設定する必要があります。

### <a name="notifying-text-changes"></a>テキストの変更の通知

テキストの貼り付けや自動修正などが行われた場合に、編集コントロールのテキストが変更されることがあります。 このような場合、[**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) を呼び出すことによって、これらの変更をテキスト サービスに通知する必要があります。

たとえば、これは、ユーザーが "World" を貼り付ける前の編集コントロールの状態です。 挿入ポイントは \[ 6、6 \] です。

![\[挿入前の6、6、挿入位置を示すテキストストリームダイアグラムのスクリーンショット \]](images/coretext/stream-5.png)

ユーザーは、変更が適用された後、貼り付け操作と編集コントロールを実行します。

:::image type="content" source="images/coretext/stream-4.png" alt-text="挿入位置を示すテキストストリームダイアグラムのスクリーンショット (11 \[ , 11 \] )":::

この場合、次の引数を使って、[**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) を呼び出す必要があります。

-   *modifiedRange*  =  modifiedRange \[6、6\]
-   *newLength* = 5
-   *Newselection*  =  \[11、11\]

1 つまたは複数の [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベントが発生し、これを処理することによって、テキスト サービスが操作しているテキストを更新します。

### <a name="overriding-text-updates"></a>テキストの更新の上書き

編集コントロールで、自動修正機能を提供するために、テキストの更新を上書きすることが必要になる場合があります。

たとえば、短縮形を正式なつづりにする修正機能を提供する編集コントロールがあるとします。 ユーザーが Space キーを押して修正機能をトリガーする前の編集コントロールの状態は、次のようになっています。 挿入ポイントは \[ 3、3 \] です。

![挿入位置を示すテキストストリームダイアグラムのスクリーンショット ( \[ 3, 3 \] )](images/coretext/stream-6.png)

ユーザーが Space キーを押すと、対応する [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベントが発生します。 編集コントロールは、テキストの更新を受け付けます。 修正が完了する直前の編集コントロールの状態は、次のようになります。 挿入ポイントは \[ 4, 4 \] です。

![挿入位置を示すテキストストリームダイアグラムのスクリーンショット ( \[ 4, 4 \] )](images/coretext/stream-7.png)

[**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) イベント ハンドラーの外部で、編集コントロールは次の修正を実行します。 修正が完了した後の編集コントロールの状態は、次のようになります。 挿入ポイントは \[ 5、5 \] です。

![5, 5 の挿入ポイントを示すテキストストリームダイアグラムのスクリーンショット \[\]](images/coretext/stream-8.png)

この場合、次の引数を使って、[**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) を呼び出す必要があります。

-   *modifiedRange*  =  modifiedRange \[1、2\]
-   *newLength* = 2
-   *Newselection*  =  \[5、5\]

1 つまたは複数の [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベントが発生し、これを処理することによって、テキスト サービスが操作しているテキストを更新します。

### <a name="providing-requested-text"></a>要求されたテキストの提供

テキスト サービスでは、自動修正や予測入力などの機能を提供する場合、正しいテキストがあることが重要です。特に、ドキュメントの読み込みなどから編集コントロールに既に存在しているテキストや、前のセクションで説明したように編集コントロールによって挿入されたテキストについて重要です。 そのため、[**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベントが発生するたびに、現在編集コントロール内にあり、指定された範囲のテキストを提供する必要があります。

[**CoreTextTextRequest**](/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) 内の [**Range**](/uwp/api/windows.ui.text.core.coretexttextrequest.range) が、編集コントロールでそのまま格納できない範囲を指定する場合があります。 たとえば、[**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) イベントの発生時に **Range** が編集コントロールのサイズよりも大きい場合や、**Range** の最後が範囲外である場合です。 このような場合は、何か適切な範囲を返す必要があります。通常、これは要求された範囲のサブセットです。

## <a name="related-articles"></a>関連記事

### <a name="samples"></a>サンプル

- [カスタム編集コントロールのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [XAML テキスト編集のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
