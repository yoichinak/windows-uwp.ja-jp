---
description: XBind マークアップ拡張機能は、バインドに代わる高いパフォーマンスを備えています。 xBind-Windows 10 の新機能は、バインドよりも短時間で実行されるメモリが少なく、デバッグの向上にも対応しています。
title: xBind マークアップ拡張機能
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715858"
---
# <a name="xbind-markup-extension"></a>{x:Bind} マークアップ拡張機能

**注**  **{x:Bind}** を使用してアプリでデータバインディングを使用する方法に関する一般的な情報 (と **{x:Bind}** と **{binding}** のすべての比較) については、「[データバインディングの詳細](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)」を参照してください。

{ **X:Bind}** markup Extension (Windows 10 用の新機能) は、 **{Binding}** の代わりに使用できます。 **{x:Bind}** は、より短い時間で実行され、メモリが **{Binding}** より少ないため、デバッグがより適切にサポートされます。

XAML のコンパイル時に、 **{x:Bind}** は、データソースのプロパティから値を取得し、マークアップで指定されたプロパティに設定するコードに変換されます。 バインドオブジェクトは、必要に応じて、データソースプロパティの値の変更を監視し、それらの変更 (`Mode="OneWay"`) に基づいて更新するように構成できます。 また、必要に応じて、独自の値の変更を source プロパティ (`Mode="TwoWay"`) にプッシュするように構成することもできます。

**{X:Bind}** と **{binding}** によって作成されるバインドオブジェクトは、機能的には同等です。 ただし、 **{x:Bind}** は、コンパイル時に生成される特殊な目的のコードを実行し、 **{Binding}** は汎用のランタイムオブジェクト検査を使用します。 その結果、 **{x:Bind}** binding (多くの場合、コンパイルされたバインディングと呼ばれます) は優れたパフォーマンスを持ち、バインド式のコンパイル時の検証を行います。また、コードファイルにブレークポイントを設定してデバッグをサポートします。ページの部分クラスとして生成されます。 これらのファイルは、`obj` フォルダーにあり、(の場合C#)`<view name>.g.cs`のように名前が付けられています。

> [!TIP]
> **{x:Bind}** の既定のモードは**OneTime**ですが、既定のモードが**OneWay**の **{Binding}** とは異なります。 これはパフォーマンス上の理由から選択されました。これは、 **OneWay**を使用すると、変更の検出をフックして処理するためにより多くのコードが生成されるためです。 OneWay または TwoWay binding を使用するモードを明示的に指定できます。 [X:DefaultBindMode](x-defaultbindmode-attribute.md)を使用して、マークアップツリーの特定のセグメントに対する **{x:Bind}** の既定のモードを変更することもできます。 指定したモードは、その要素とその子の **{x:Bind}** 式に適用され、バインディングの一部として明示的にモードが指定されることはありません。

**{X:Bind} をデモンストレーションするサンプルアプリ**

-   [{x:Bind} サンプル](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [XAML UI の基本サンプル](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>XAML 属性の使用法

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| 期間 | 説明 |
|------|-------------|
| _propertyPath_ | バインディングのプロパティパスを指定する文字列。 詳細については、以下の「[プロパティパス](#property-path)」セクションを参照してください。 |
| _bindingProperties_ |
| _propName_=_値_\[、 _propName_=_値_\]* | 名前と値のペアの構文を使用して指定される1つ以上のバインドプロパティ。 |
| _propName_ | バインドオブジェクトに設定するプロパティの文字列名。 たとえば、"Converter" のようになります。 |
| _値_ | プロパティに設定する値。 引数の構文は、設定するプロパティによって異なります。 値がマークアップ拡張機能である場合の_propName_=_値_の使用例を次に示します。 `Converter={StaticResource myConverterClass}`。 詳細については、以下の「 [{x:Bind} を使用して設定できるプロパティ](#properties-that-you-can-set-with-xbind)」を参照してください。 |

## <a name="examples"></a>例

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

この XAML の例では、 **{x:Bind}** と**ListView**プロパティを使用します。 **X:DataType**値の宣言に注意してください。

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>プロパティのパス

*PropertyPath*は、 **{X:Bind}** 式の**パス**を設定します。 **Path**は、バインド先のプロパティ、サブプロパティ、フィールド、またはメソッドの値 (ソース) を指定するプロパティパスです。 **パス**プロパティの名前は明示的に記述できます。 `{x:Bind Path=...}`。 または、`{x:Bind ...}`を省略することもできます。

### <a name="property-path-resolution"></a>プロパティパスの解決

**{x:Bind}** は、既定のソースとして**DataContext**を使用しません。代わりに、ページまたはユーザーコントロール自体を使用します。 これにより、ページの分離コードや、プロパティ、フィールド、およびメソッドのユーザーコントロールが検索されます。 ビューモデルを **{x:Bind}** に公開するには、通常、ページまたはユーザーコントロールの分離コードに新しいフィールドまたはプロパティを追加します。 プロパティパスのステップは、ドット (.) で区切られており、複数の区切り記号を使用して連続するサブプロパティを走査できます。 バインドされるオブジェクトを実装するために使用されるプログラミング言語に関係なく、ドット区切り記号を使用します。

たとえば、ページで、 **Text = "{X:Bind employee. FirstName}"** と入力すると、ページの**employee**メンバーが検索され、 **employee**によって返されたオブジェクトの**FirstName**メンバーが検索されます。 アイテムコントロールを、従業員の依存関係を含むプロパティにバインドする場合、プロパティのパスは "Employee. 扶養者" である可能性があり、アイテムコントロールの項目テンプレートでは、アイテムが "依存" に表示されます。

/Cx C++の場合、 **{x:Bind}** はページまたはデータモデルのプライベートフィールドおよびプロパティにバインドできません。バインドできるようにするには、パブリックプロパティが必要です。 関連するメタデータを取得できるようにするには、バインドのセキュリティ領域を CX クラス/インターフェイスとして公開する必要があります。 **\[バインド**可能な\]属性は必要ありません。

**X:Bind**では、バインド式の一部として**ElementName = xxx**を使用する必要はありません。 代わりに、要素の名前をバインドのパスの最初の部分として使用できます。名前付き要素は、ルートバインディングソースを表すページ内のフィールドまたはユーザーコントロールになります。 


### <a name="collections"></a>コレクション

データソースがコレクションの場合、プロパティパスでは、位置またはインデックスを使用してコレクション内の項目を指定できます。 たとえば、"Teams\[0\]です。"\[\]" というリテラルは、ゼロから始まるコレクション内の最初の項目を要求する "0" を囲みます。

インデクサーを使用するには、モデルで、インデックスを作成するプロパティの型に対して**IList&lt;t&gt;** または**ivector&lt;t&gt;** を実装する必要があります。 (IReadOnlyList&lt;T&gt; と IVectorView&lt;T&gt; では、インデクサー構文はサポートされていません)。インデックス付きプロパティの型が**INotifyCollectionChanged**または**IObservableVector**をサポートし、バインドが OneWay または TwoWay の場合は、これらのインターフェイスに対する変更通知を登録してリッスンします。 変更検出ロジックは、特定のインデックス値に影響を与えない場合でも、すべてのコレクションの変更に基づいて更新されます。 これは、リッスンしているロジックがコレクションのすべてのインスタンスで共通であるためです。

データソースがディクショナリまたはマップの場合、プロパティパスでは、文字列名によってコレクション内の項目を指定できます。 たとえば **&lt;TextBlock Text = "{X:Bind Players\[' John smith '\]"/&gt;** は、"john smith" という名前の辞書内の項目を検索します。 名前は引用符で囲む必要があり、一重引用符または二重引用符を使用できます。 Hat (^) を使用して、文字列内の引用符をエスケープすることができます。 通常、XAML 属性で使用されているものとは別の引用符を使用するのが最も簡単です。 (Ireadonlydictionary<&lt;T&gt; と IMapView&lt;T&gt; では、インデクサー構文はサポートされていません)。

文字列インデクサーを使用するには、インデックスを作成するプロパティの型に対して、 **IDictionary&lt;string、t&gt;** または**IMap&lt;string、t&gt;** を実装する必要があります。 インデックス付きプロパティの型が**IObservableMap**をサポートしていて、バインドが OneWay または TwoWay の場合は、これらのインターフェイスに対する変更通知を登録してリッスンします。 変更検出ロジックは、特定のインデックス値に影響を与えない場合でも、すべてのコレクションの変更に基づいて更新されます。 これは、リッスンしているロジックがコレクションのすべてのインスタンスで共通であるためです。

### <a name="attached-properties"></a>添付プロパティ

[アタッチされたプロパティ](./attached-properties-overview.md)にバインドするには、クラスとプロパティ名をドットの後にかっこで囲んで指定する必要があります。 たとえば、 **Text = "{X:Bind Button22. (Grid. Row)} "** プロパティが Xaml 名前空間で宣言されていない場合は、xml 名前空間でプレフィックスを付ける必要があります。これは、ドキュメントの先頭にあるコードの名前空間にマップする必要があります。

### <a name="casting"></a>キャスト

コンパイル済みのバインディングは厳密に型指定され、パス内の各ステップの型を解決します。 返される型にメンバーがない場合、コンパイル時にエラーが発生します。 キャストを指定すると、オブジェクトの実際の型をバインドするように指示できます。 次の例では、 **obj**は object 型のプロパティですが、テキストボックスが含まれているので、 **text = "{X:Bind ((TextBox) obj" を使用できます。Text} "** または**text =" {x:Bind obj. (TextBox. Text)} "** 。
**Text = "{x:Bind ((data: SampleDataGroup) groups3\[0\]) の groups3 フィールド。Title} "** はオブジェクトの辞書であるため、 **Data: SampleDataGroup**にキャストする必要があります。 Xml**データ:** 名前空間プレフィックスを使用して、オブジェクトの種類を既定の XAML 名前空間の一部ではないコードの名前空間にマップすることに注意してください。

_注: スタイルC#のキャスト構文は、添付プロパティの構文よりも柔軟であり、今後推奨される構文です。_

## <a name="functions-in-binding-paths"></a>バインドパス内の関数

Windows 10 バージョン1607以降、 **{x:Bind}** は、バインドパスのリーフステップとして関数を使用することをサポートしています。 これは、マークアップでいくつかのシナリオを可能にするデータバインドのための強力な機能です。 詳細については、「[関数バインド](../data-binding/function-bindings.md)」を参照してください。

## <a name="event-binding"></a>イベントのバインド

イベントバインディングは、コンパイル済みバインディングの固有の機能です。 これにより、分離コードのメソッドではなく、バインディングを使用してイベントのハンドラーを指定できます。 例: **= "{X:Bind rootFrame. GoForward}" をクリックし**ます。

イベントの場合、ターゲットメソッドはオーバーロードされていない必要があります。また、次のものも必要です。

- イベントの署名と一致します。
- またはパラメーターがありません。
- または、イベントパラメーターの型から割り当て可能な型のパラメーターの数が同じです。

生成された分離コードでは、コンパイル済みバインディングはイベントを処理し、モデルのメソッドにルーティングして、イベントが発生したときにバインディング式のパスを評価します。 これは、プロパティバインドとは異なり、モデルの変更を追跡しないことを意味します。

プロパティパスの文字列構文の詳細については、「[プロパティ](property-path-syntax.md)パスの構文」を参照してください。「 **{x:Bind}** 」に記載されている相違点に注意してください。

## <a name="properties-that-you-can-set-with-xbind"></a>{X:Bind} を使用して設定できるプロパティ

**{x:Bind}** は、マークアップ拡張機能で設定できる複数の読み取り/書き込みプロパティがあるため、 *bindingproperties*プレースホルダー構文で示されています。 プロパティは、コンマ区切りの*propName*=*値*のペアを使用して任意の順序で設定できます。 バインド式に改行を含めることはできないことに注意してください。 一部のプロパティには型変換を持たない型が必要なため、 **{x:Bind}** 内で入れ子になっている独自のマークアップ拡張が必要です。

これらのプロパティは、[**バインディング**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)クラスのプロパティとほぼ同じ方法で動作します。

| プロパティ | 説明 |
|----------|-------------|
| **パス** | 前の「[プロパティのパス](#property-path)」セクションを参照してください。 |
| **修復** | バインディングエンジンによって呼び出されるコンバーターオブジェクトを指定します。 コンバーターは XAML で設定できますが、リソースディクショナリ内のそのオブジェクトへの[{StaticResource} マークアップ拡張機能](staticresource-markup-extension.md)の参照で割り当てられたオブジェクトインスタンスを参照する場合に限ります。 |
| **収束 Terlanguage** | コンバーターで使用するカルチャを指定します。 (変換**言語**を設定している場合は、**コンバーター**も設定する必要があります)。カルチャは、標準ベースの識別子として設定されます。 詳細については、「[**コンバーター Terlanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)」を参照してください。 |
| **ConverterParameter** | コンバーターのロジックで使用できるコンバーターパラメーターを指定します。 (変換**Terparameter**を設定している場合は、**コンバーター**も設定する必要があります)。ほとんどのコンバーターは、渡された値から必要なすべての情報を取得して変換する単純なロジックを使用します。また、変換**Terparameter**値は必要ありません。 変換**Terparameter**パラメーターは、複数のロジックを持つ、2つ**以上のロジック**を含む、より高度なコンバーターの実装用です。 文字列以外の値を使用するコンバーターを記述できますが、これは一般的ではありません。詳細については、「変換[**Terparameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) 」を参照してください。 |
| **FallbackValue** | ソースまたはパスを解決できない場合に表示する値を指定します。 |
| **モード** | バインドモードを、"OneTime"、"OneWay"、または "TwoWay" のいずれかの文字列として指定します。 既定値は "OneTime" です。 これは、ほとんどの場合、"OneWay" である **{Binding}** の既定値とは異なることに注意してください。 |
| **TargetNullValue** | ソース値が解決されるときに表示する値を指定しますが、明示的に**null**を指定します。 |
| **バインド** | 双方向のバインディングの逆方向に使用する関数を指定します。 |
| **System.windows.data.binding.updatesourcetrigger** | TwoWay 束縛で、コントロールからモデルに変更をプッシュするタイミングを指定します。 TextBox 以外のすべてのプロパティの既定値は PropertyChanged です。TextBox。テキストは LostFocus です。|

> [!NOTE]
> マークアップを **{Binding}** から **{x:Bind}** に変換する場合は、 **Mode**プロパティの既定値の違いに注意してください。
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute)は、マークアップツリーの特定のセグメントについて、x:Bind の既定のモードを変更するために使用できます。 選択したモードでは、バインドの一部としてモードを明示的に指定していない、その要素とその子に x:Bind 式がすべて適用されます。 OneTime は、OneWay を使用すると、変更の検出をフックして処理するためにより多くのコードが生成されるため、OneWay よりもパフォーマンスが向上します。

## <a name="remarks"></a>解説

**{X:Bind}** は、生成されたコードを使用してその利点を達成するため、コンパイル時に型情報を必要とします。 これは、型が事前にわからないプロパティにバインドできないことを意味します。 このため、**オブジェクト**型である**DataContext**プロパティと共に **{x:Bind}** を使用することはできません。また、実行時にも変更される可能性があります。

**{X:Bind}** とデータテンプレートを使用する場合は、「[例](#examples)」のセクションに示されているように、 **x:DataType**の値を設定して、バインドされる型を示す必要があります。 また、型をインターフェイスまたは基本クラスの型に設定し、必要に応じてキャストを使用して完全な式を作成することもできます。

コンパイル済みバインディングは、コード生成に依存します。 したがって、リソースディクショナリで **{x:Bind}** を使用する場合、リソースディクショナリには分離コードクラスが必要です。 コード例については、「 [{x:Bind} を使用したリソースディクショナリ](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind)」を参照してください。

コンパイル済みのバインドを含むページとユーザーコントロールには、生成されたコードに "Bindings" プロパティがあります。 これには、次のメソッドが含まれます。

- **Update ()** -コンパイルされたすべてのバインドの値を更新します。 すべての一方向/双方向のバインドでは、変更を検出するためにリスナーがフックされます。
- **Initialize ()** -バインドがまだ初期化されていない場合は、Update () を呼び出してバインドを初期化します。
- **Stoptracking ()** -一方向および双方向のバインド用に作成されたすべてのリスナーをアンフックします。 これらは、Update () メソッドを使用して再初期化できます。

> [!NOTE]
> Windows 10 バージョン1607以降では、XAML フレームワークには、可視性コンバーターに組み込みブール値が用意されています。 コンバーターは、**表示可能**な列挙値に**true**をマップし、コンバーターを作成せずに可視性プロパティをブール値にバインドできるように**false**を**折りたたみ**ます。 これは関数バインドの機能ではなく、プロパティバインドのみであることに注意してください。 組み込みのコンバーターを使用するには、アプリの最小ターゲット SDK バージョンが14393以降である必要があります。 アプリが以前のバージョンの Windows 10 を対象としている場合は、使用できません。 ターゲットバージョンの詳細については、「[バージョンアダプティブコード](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)」を参照してください。

**ヒント**  in [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path)や[**収束 terparameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)など、値に単一の中かっこを指定する必要がある場合は、その前に円記号 (`\{`) を付けてください。 または、`ConverterParameter='{Mix}'`など、2番目の引用符セットでエスケープが必要な中かっこが含まれている文字列全体を囲みます。

[**コンバーター**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)、変換[**Terlanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)および変換**terlanguage**は、バインディングソースの値または型を、バインディングターゲットプロパティと互換性のある型または値に変換するシナリオに関連しています。 詳細と例については、「[データバインディングの詳細](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)」の「データ変換」セクションを参照してください。

**{x:Bind}** はマークアップ拡張機能のみであり、プログラムによってこのようなバインディングを作成または操作することはできません。 マークアップ拡張機能の詳細については、「 [XAML の概要](xaml-overview.md)」を参照してください。

