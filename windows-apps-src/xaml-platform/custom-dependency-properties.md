---
description: C++、C#、または Visual Basic を使った Windows ランタイム アプリでカスタム依存関係プロパティを定義および実装する方法を説明します。
title: カスタム依存関係プロパティ
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: e7a14383f127f12b5ba13e67e1690c0d7a936c82
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174216"
---
# <a name="custom-dependency-properties"></a>カスタム依存関係プロパティ

ここでは、C++、C#、または Visual Basic を使った Windows ランタイム アプリで固有の依存関係プロパティを定義および実装する方法を説明します。 アプリ開発者とコンポーネント作成者がカスタム依存関係プロパティを作成する理由の一覧を示します。 カスタム依存関係プロパティの実装手順と、依存関係プロパティのパフォーマンス、操作性、または汎用性を向上させることのできるいくつかのヒントについて説明します。

## <a name="prerequisites"></a>前提条件

「[依存関係プロパティの概要](dependency-properties-overview.md)」を読み、依存関係プロパティを既にある依存関係プロパティのユーザーの観点から理解していることを前提としています。 このトピックの例を参考にするには、XAML について理解し、C++、C#、または Visual Basic を使った基本的な Windows ランタイム アプリを作る方法を理解している必要もあります。

## <a name="what-is-a-dependency-property"></a>依存関係プロパティとは

プロパティのスタイル設定、データ バインディング、アニメーション、既定値をサポートするには、依存関係プロパティとして実装する必要があります。 依存関係プロパティの値はフィールドとしてクラスに格納されるのではなく、xaml フレームワークによって格納され、キーを使って参照されます。キーは、[**DependencyProperty.Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register)メソッドを呼び出すことにより、プロパティが Windows ランタイム プロパティ システムに登録されるときに取得されます。   依存関係プロパティは、[**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) から派生した型でのみ使うことができます。 ただし、**DependencyObject** はクラス階層のかなり上位にあるため、UI とプレゼンテーションのサポートを目的とするクラスの大半は、依存関係プロパティをサポートできます。 依存関係プロパティと、このドキュメントでそれらを説明するために使っている用語と表記規則の一部については、「[依存関係プロパティの概要](dependency-properties-overview.md)」を参照してください。

Windows ランタイムでの依存関係プロパティの例として、[**Control.Background**](/uwp/api/windows.ui.xaml.controls.control.background)、[**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、および [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text) があります。

規則として、クラスで公開されている各依存関係プロパティには、依存関係プロパティの識別子を提供する同じクラスで公開される [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 型の対応する **public static readonly** プロパティがあります。 識別子の名前は、依存関係プロパティの名前の終わりに "Property" という文字列を追加した名前です。 たとえば、**Control.Background** プロパティに対応する **DependencyProperty** 識別子は [**Control.BackgroundProperty**](/uwp/api/windows.ui.xaml.controls.control.backgroundproperty) です。 識別子を登録したときに依存関係プロパティに関する情報が格納され、[**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) の呼び出しなど、依存関係プロパティに関係する他の操作でその識別子を使うことができます。

## <a name="property-wrappers"></a>プロパティ ラッパー

通常、依存関係プロパティにはラッパー実装があります。 ラッパーがない場合は、依存関係プロパティのユーティリティ メソッド [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) および [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を使って識別子をパラメーターとして渡す方法でのみプロパティを取得または設定できます。 これは、明らかにプロパティであるものについては不自然な使用方法です。 しかし、ラッパーがあれば、お使いのコード、および依存関係プロパティを参照する他のコードで、使っている言語にとって自然な単純なオブジェクト プロパティ構文を使うことができます。

カスタム依存関係プロパティを自分で実装し、パブリックにして簡単に呼び出すには、プロパティ ラッパーも定義します。 プロパティ ラッパーは、依存関係プロパティに関する基本情報をリフレクション プロセスまたは静的分析プロセスにレポートする場合にも役立ちます。 具体的には、ラッパーは [**ContentPropertyAttribute**](/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute) などの属性を配置する場所です。

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>プロパティを依存関係プロパティとして実装する状況

クラスにパブリック読み取り/書き込みプロパティを実装する場合、クラスが [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) から派生する限り、プロパティを依存関係プロパティとして機能させるオプションがあります。 場合によっては、プライベート フィールドでプロパティをバッキングする標準的な手法で十分です。 カスタム プロパティを依存関係プロパティとして定義することは、必ずしも必須または適切ではありません。 どれを選ぶかは、プロパティでサポートするシナリオによって決まります。

Windows ランタイムまたは Windows ランタイム アプリの次の機能の 1 つ以上をサポートする場合は、プロパティを依存関係プロパティとして実装することを検討してください。

- [**Style**](/uwp/api/Windows.UI.Xaml.Style) によるプロパティの設定
- [**{Binding}**](binding-markup-extension.md) によるデータ バインディングの有効なターゲット プロパティとしての機能
- [**Storyboard**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) によるアニメーション化された値のサポート
- プロパティの値が次のものによって変更された場合のレポート:
  - プロパティ システム自体によって実行されたアクション
  - 環境
  - ユーザー操作
  - スタイルの読み取りと書き込み

## <a name="checklist-for-defining-a-dependency-property"></a>依存関係プロパティの定義のチェック リスト

依存関係プロパティの定義は、一連の概念と考えることができます。 いくつかの概念は実装の単一コード行で処理できるため、これらの概念は必ずしも手順ではありません。 このリストでは、概要のみ示します。 各概念については後でこのトピックで詳しく説明し、複数の言語でのコード例を示します。

- プロパティ名をプロパティ システムに登録して ([**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) を呼び出す)、所有者型とプロパティ値の型を指定します。
  - [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) にはプロパティ メタデータを要求する必須パラメーターがあります。 これに **null** を指定するか、プロパティ変更動作や、[**ClearValue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) を呼び出すことによって復元できるメタデータ ベースの既定値が必要な場合は、[**PropertyMetadata**](/uwp/api/windows.ui.xaml.propertymetadata) のインスタンスを指定します。
- [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) 識別子を所有者型の **public static readonly** プロパティ メンバーとして定義します。
- 実装する言語で使うプロパティ アクセサー モデルに従ってラッパー プロパティを定義します。 ラッパー プロパティ名は、[**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) で使った *name* 文字列と一致する必要があります。 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) と [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を呼び出し、独自のプロパティの識別子をパラメーターとして渡すことで、**get** アクセサーと **set** アクセサーを実装して、ラップする依存関係プロパティにラッパーを関連付けます。
- (省略可能) ラッパーに [**ContentPropertyAttribute**](/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute) などの属性を配置します。

> [!NOTE]
> カスタム添付プロパティを定義する場合は、通常、ラッパーを省略します。 代わりに、XAML プロセッサで使うことのできる別のスタイルのアクセサーを作ります。 詳しくは、「[カスタム添付プロパティ](custom-attached-properties.md)」をご覧ください。 

## <a name="registering-the-property"></a>プロパティの登録

プロパティを依存関係プロパティにするには、Windows ランタイム プロパティ システムでメンテナンスされるプロパティ ストアにプロパティを登録する必要があります。  プロパティを登録するには、[**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) メソッドを呼び出します。

Microsoft .NET 言語 (C# と Microsoft Visual Basic) では、クラスの本文 (クラス内だがメンバー定義の外部) で [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) を呼び出します。 識別子は、[**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) メソッド呼び出しで戻り値として提供されます。 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 呼び出しは通常、静的コンストラクターとして、またはクラスの一部である型 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) の **public static readonly** プロパティ初期化の一部として行われます。 このプロパティは、依存関係プロパティの識別子を公開します。 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 呼び出しの例を次に示します。

> [!NOTE]
> 依存関係プロパティを識別子プロパティ定義の一部として登録することは、一般的な実装ですが、クラスの静的コンストラクターに依存関係プロパティを登録することもできます。 このアプローチは、依存関係プロパティの初期化に複数のコード行が必要な場合に意味を持つことがあります。

C++/CX では、ヘッダーとコードファイルの間で実装を分割する方法に関するオプションが用意されています。 一般的な分割では、**set** ではなく **get** 実装で、識別子自体をヘッダーで **public static** プロパティとして宣言します。 **get** 実装は、初期化されていない [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) インスタンスであるプライベート フィールドを参照します。 ラッパー、およびラッパーの **get** 実装と **set** 実装を宣言することもできます。 この場合、ヘッダーにはいくつかの最小限の実装が含まれます。 ラッパーに Windows ランタイム属性が必要な場合は、ヘッダーにも属性が必要です。 コード ファイルの最初にアプリが初期化するときにのみ実行されるヘルパー関数内に [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) の呼び出しを配置します。 **Register** の戻り値を使って、ヘッダーで宣言した初期化されていない静的識別子を入力します。これは実装ファイルのルート スコープで当初は **nullptr** に設定します。

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> C++/CX コードの場合、プライベートフィールドとパブリック読み取り専用プロパティがあり、その依存関係プロパティを使用する他の呼び出し元が、その識別子がパブリックであることを必要とするプロパティシステムユーティリティ Api も使用できるようにするために、 [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) が表示される理由。 識別子をプライベートのままにした場合、他のユーザーはこれらのユーティリティ API を使うことができません。 このような API とシナリオの例には、[**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue)、任意の [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)、[**ClearValue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue)、[**GetAnimationBaseValue**](/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue)、[**SetBinding**](/uwp/api/windows.ui.xaml.frameworkelement.setbinding)、および [**Setter.Property**](/uwp/api/windows.ui.xaml.setter.property) があります。 Windows ランタイム メタデータの規則ではパブリック フィールドが許可されないため、これにパブリック フィールドを使うことはできません。

## <a name="dependency-property-name-conventions"></a>依存関係プロパティの命名規則

依存関係プロパティには命名規則があります。例外的な状況を除き、これに従ってください。 依存関係プロパティ自体には、[**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) の最初のパラメーターとして与えられる基本的な名前 (前の例では "Label") があります。 名前は登録の種類ごとに一意である必要があり、一意性の要件は継承されるメンバーにも適用されます。 基本型を通じて継承された依存関係プロパティは、既に登録型の一部と見なされます。継承されたプロパティの名前を再び登録することはできません。

> [!WARNING]
> ここで指定する名前は、選択した言語のプログラミングで有効な文字列識別子にすることができますが、通常は、XAML でも依存関係プロパティを設定できるようにする必要があります。 XAML で設定するには、選ぶプロパティが有効な XAML 名である必要があります。 詳細については、「 [XAML の概要](xaml-overview.md)」を参照してください。

識別子プロパティを作る場合は、登録したプロパティの名前にサフィックス "Property" を結合します ("LabelProperty" など)。 このプロパティは依存関係プロパティの識別子であり、独自のプロパティ ラッパーで呼び出す [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) と [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) の入力として使われます。 プロパティ システムや、[**{x:bind}:**](x-bind-markup-extension.md) などの他の XAML プロセッサによっても使われます。

## <a name="implementing-the-wrapper"></a>ラッパーの実装

プロパティ ラッパーでは、**get** 実装の [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) と **set** 実装の [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を呼び出す必要があります。

> [!WARNING]
> すべての例外的な状況において、ラッパーの実装では、 [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) 操作と [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 演算のみを実行する必要があります。 そうしないと、プロパティは XAML で設定された場合とコードで設定された場合とで動作が異なります。 効率を高めるため、依存関係プロパティを設定するときに XAML パーサーはラッパーをバイパスし、**SetValue** 経由でバッキング ストアとやり取りします。

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>カスタム依存関係プロパティのプロパティ メタデータ

プロパティ メタデータが依存関係プロパティに割り当てられている場合、同じメタデータが、プロパティ所有者型のすべてのインスタンスまたはそのサブクラスのそのプロパティに適用されます。 プロパティ メタデータでは、次の 2 つの動作を指定できます。

- プロパティ システムがプロパティのすべてのケースに割り当てる既定値。
- プロパティ値の変更が検出されるたびにプロパティ システム内で自動的に呼び出される静的コールバック メソッド。

### <a name="calling-register-with-property-metadata"></a>プロパティ メタデータでの登録の呼び出し

前に示した [**DependencyProperty.Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) を呼び出す例では、*propertyMetadata* パラメーターに null 値を渡しました。 依存関係プロパティを有効にして、既定値を提供するかプロパティ変更コールバックを使うには、これらの機能のいずれかまたは両方を提供する、[**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) インスタンスを定義する必要があります。

通常は、 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata)のパラメーター内に、インラインで作成されたインスタンスとして指定[**します。**](/uwp/api/windows.ui.xaml.dependencyproperty.register)

> [!NOTE]
> [**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback)実装を定義する場合は、 [**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata)コンストラクターを呼び出して**PropertyMetadata**インスタンスを定義するのではなく、 [**PropertyMetadata**](/uwp/api/windows.ui.xaml.propertymetadata.create)ユーティリティメソッドを使用する必要があります。

次の例では、前に示した [**DependencyProperty.Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) の例を、[**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) インスタンスを [**PropertyChangedCallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) 値を使って参照することで変更します。 "OnLabelChanged" コールバックの実装については、このセクションの後半で説明します。

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>既定値

プロパティが設定されていないときには常に特定の既定値が返されるように、依存関係プロパティに既定値を指定することができます。 この値は、そのプロパティの型固有の既定値とは別の値にすることができます。

既定値が指定されていない場合、参照型の依存関係プロパティの既定値は null になるか、値型または言語プリミティブの型の既定になります (たとえば、整数の場合は 0、文字列の場合は空の文字列)。 既定値を設定する主な理由は、プロパティに対して [**ClearValue**](/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) を呼び出すときに、この値が復元されるためです。 プロパティごとに既定値を設定すると、値型の場合には特に、コンストラクターで既定値を設定するよりも便利な場合があります。 ただし、参照型の場合は、既定値を設定しても意図しないシングルトン パターンが作られないことを確認してください。 詳しくは、このトピックの「[ベスト プラクティス](#best-practices)」をご覧ください。

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> 既定値の [**Unsetvalue**](/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue)に登録しないでください。 登録すると、プロパティのユーザーが混乱し、プロパティ システム内で意図しない結果が発生します。

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

シナリオによっては、複数の UI スレッドで使われるオブジェクトの依存関係プロパティを定義します。 これは、複数のアプリで使われるデータ オブジェクト、または複数のアプリで使用するコントロールを定義している場合に当てはまることがあります。 プロパティを登録したスレッドと関連している、既定値インスタンスではなく [**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) の実装を提供することで、さまざまな UI スレッド間でオブジェクトの交換を有効にすることができます。 基本的に、[**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) は既定値のファクトリを定義します。 **CreateDefaultValueCallback** により返された値は、オブジェクトを使っている現在の UI **CreateDefaultValueCallback** スレッドと常に関連付けられています。

[**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) を指定するメタデータを定義するには、[**PropertyMetadata.Create**](/uwp/api/windows.ui.xaml.propertymetadata.create) を呼び出して、メタデータ インスタンスを返す必要があります。[**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) コンストラクターには **CreateDefaultValueCallback** パラメーターを含むシグネチャがありません。

[**CreateDefaultValueCallback**](/uwp/api/windows.ui.xaml.createdefaultvaluecallback) の一般的な実装パターンでは、新しい [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) クラスを作成し、**DependencyObject** の各プロパティの特定のプロパティ値を目的の既定値に設定してから、**CreateDefaultValueCallback** メソッドの戻り値によって新しいクラスを **Object** リファレンスとして返します。

### <a name="property-changed-callback-method"></a>プロパティ変更コールバック メソッド

プロパティ変更コールバック メソッドを定義して、プロパティと他の依存関係プロパティの対話を定義することや、プロパティが変更されるたびに内部プロパティまたはオブジェクトの状態を更新することができます。 コールバックが呼び出された場合、プロパティ システムは有効なプロパティ値の変更があるかどうかを判断しています。 コールバック メソッドは静的であるため、変更をレポートしたクラスのインスタンスを示すコールバックの *d* パラメーターは重要です。 標準的な実装では、通常は *d* として渡されるオブジェクトに対して他の変更を実行することで、イベント データの [**NewValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue) プロパティを使い、その値をいずれかの方法で処理します。 プロパティ変更に対する追加の応答では、**NewValue** でレポートされる値を拒否、[**OldValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue) を復元、または **NewValue** に適用されるプログラムの制約に値を設定します。

次の例では、[**PropertyChangedCallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) の実装を示しています。 これは、前の [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) の例で参照したメソッドを、[**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) の構成引数の一部として実装します。 このコールバックで処理するシナリオは、クラスに "HasLabelValue" という名前の計算された読み取り専用プロパティもある場合です (実装は示していません)。 "Label" プロパティが再評価されるたびに、このコールバック メソッドが呼び出され、コールバックは依存する計算された値が依存関係プロパティに対する変更との同期を保てるようにします。

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>構造体と列挙に対するプロパティ変更動作

[**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) の型が列挙または構造体である場合、構造体または列挙値の内部値が変更されなかった場合でも、コールバックが呼び出されることがあります。 これは、値が変更された場合にのみ呼び出される、文字列などのシステム プリミティブとは異なります。 これは、内部で実行されたこれらの値でのボックスとボックス解除操作による影響です。 値が列挙または構造体であるプロパティに [**PropertyChangedCallback**](/uwp/api/windows.ui.xaml.propertychangedcallback) メソッドがある場合、自分で値をキャストし、now-cast 値に使用可能なオーバーロードされた比較演算子を使用して、[**OldValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue) と [**NewValue**](/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue) を比較する必要があります。 または、このような演算子が使用できなければ (カスタム構造の場合など)、個別の値を比較する必要がある場合もあります。 値が変更されていないという結果の場合、通常は何もしないことを選びます。

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>ベスト プラクティス

カスタム依存関係プロパティを定義するときには、次の考慮事項をヒントとして念頭に置いてください。

### <a name="dependencyobject-and-threading"></a>DependencyObject とスレッド

すべての [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) インスタンスは、Windows ランタイム アプリによって表示される現在の [**Window**](/uwp/api/Windows.UI.Xaml.Window) と関連付けられている UI スレッド上で作成する必要があります。 それぞれの **DependencyObject** はメイン UI スレッド上で作成する必要がありますが、オブジェクトは、[**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) を呼び出すことにより、他のスレッドからディスパッチャー参照を使ってアクセスできます。

[**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) のスレッドの側面も問題となります。それは、通常、UI スレッド上で実行されるコードのみが依存関係プロパティの値を変更または読み取ることができることを意味するためです。 通常、**非同期**パターンとバックグラウンド ワーカー スレッドを適切に使う一般的な UI コードでは、スレッドの問題を回避できます。 独自に **DependencyObject** 型を定義し、それをデータ ソースや **DependencyObject** が必ずしも適切でないその他のシナリオで使おうとすると、通常は **DependencyObject** に関連するスレッドの問題が発生します。

### <a name="avoiding-unintentional-singletons"></a>意図しないシングルトンの回避

意図しないシングルトンは、参照型を受け取る依存関係プロパティを宣言し、その参照型のコンストラクターを、[**PropertyMetadata**](/uwp/api/Windows.UI.Xaml.PropertyMetadata) を設定するコードの一部として呼び出す場合に発生することがあります。 その場合、使用するすべての依存関係が **PropertyMetadata** の 1 つのインスタンスを共有し、したがって構築した単一の参照型を共有しようとします。 依存関係プロパティで設定するその値型のすべてのサブプロパティは、おそらく意図しない方法で他のオブジェクトに伝播されます。

クラス コンストラクターを使うと、null 以外の値が必要な場合に参照型の依存関係プロパティの初期値を設定できますが、[依存関係プロパティの概要](dependency-properties-overview.md)の趣旨に沿って、これはローカル値と見なされることに注意してください。 クラスでテンプレートがサポートされる場合は、この目的にテンプレートを使う方が適切な場合があります。 シングルトン パターンを回避すると同時に便利な既定値を提供する別の方法は、そのクラスの値に適した既定値を提供する静的プロパティを参照型で公開することです。

### <a name="collection-type-dependency-properties"></a>コレクション型の依存関係プロパティ

コレクション型依存関係プロパティには、考慮すべき実装問題が他にもいくつかあります。

コレクション型の依存関係プロパティは、Windows ランタイム API では比較的まれです。 ほとんどの場合、項目が [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) サブクラスであるコレクションを使うことができますが、コレクション プロパティ自体は従来の CLR または C++ プロパティとして実装されます。 これは、依存関係プロパティが関与するいくつかの典型的なシナリオにはコレクションが必ずしも適さないためです。 次に例を示します。

- 通常は、コレクションをアニメーション化しません。
- 通常、スタイルまたはテンプレートを持つコレクションには項目を事前設定しません。
- コレクションへのバインディングは主要なシナリオですが、コレクションはバインディング ソースとなる依存関係プロパティでなくてもかまいません。 バインディング ターゲットの場合は、[**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) または [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) のサブクラスを使ってコレクション項目をサポートするか、ビュー モデル パターンを使う方が一般的です。 コレクションとのバインドの詳細については、「 [データバインディングの詳細](../data-binding/data-binding-in-depth.md)」を参照してください。
- コレクションの変更の通知は、**INotifyPropertyChanged** や **INotifyCollectionChanged** などのインターフェイスを通じて処理するか、[**ObservableCollection&lt;T&gt;**](/dotnet/api/system.collections.objectmodel.observablecollection-1) からコレクション型を派生させることで処理する方が適切です。

それでも、コレクション型の依存関係プロパティのシナリオは存在します。 次の 3 つのセクションでは、コレクション型の依存関係プロパティを実装する方法に関するガイダンスを示します。

### <a name="initializing-the-collection"></a>コレクションの初期化

依存関係プロパティを作る場合は、依存関係プロパティ メタデータを使って既定値を設定できます。 ただし、シングルトン静的コレクションを既定値として使わないように注意してください。 代わりに、コレクション プロパティの所有者クラスのクラス コンストラクター ロジックの一部として、コレクション値を一意の (インスタンス) コレクションに意図的に設定する必要があります。

### <a name="change-notifications"></a>変更通知

コレクションを依存関係プロパティとして定義しても、"PropertyChanged" コールバック メソッドを呼び出すプロパティ システムによってコレクションの項目の変更通知が自動的に提供されるわけではありません。 たとえばデータ バインディング シナリオなどでコレクションまたはコレクション項目の通知が必要な場合は、**INotifyPropertyChanged** インターフェイスまたは **INotifyCollectionChanged** インターフェイスを実装します。 詳細については、「 [データバインディングの詳細](../data-binding/data-binding-in-depth.md)」を参照してください。

### <a name="dependency-property-security-considerations"></a>依存関係プロパティのセキュリティに関する考慮事項

依存関係プロパティはパブリック プロパティとして宣言します。 依存関係プロパティの識別子を **パブリックな静的読み取り専用** メンバーとして宣言します。 言語で許可されている他のアクセス レベル (**protected** など) を宣言しようとした場合でも、依存関係プロパティは常に、プロパティ システム API と組み合わせた識別子を使ってアクセスできます。 依存関係プロパティ識別子を内部またはプライベートして宣言すると、プロパティ システムは正常に動作できないため、このような宣言は機能しません。

ラッパー プロパティは便宜だけが目的であるため、ラッパーに適用されるセキュリティ メカニズムは [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) または [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を代わりに呼び出すことでバイパスできます。 したがって、ラッパー プロパティはパブリックのままにしてください。そうしないと、正当な呼び出し元がプロパティを使いにくくなるだけで、実際のセキュリティ上の利点はありません。

Windows ランタイムには、カスタム依存関係プロパティを読み取り専用として登録する方法は用意されていません。

### <a name="dependency-properties-and-class-constructors"></a>依存関係プロパティとクラス コンストラクター

クラス コンストラクターは仮想メソッドを呼び出してはならないという一般的な原則があります。 これは、コンストラクターは派生クラス コンストラクターの基本の初期化を実行するために呼び出すことができ、構築されるオブジェクト インスタンスの初期化がまだ完了していないときにコンストラクターから仮想メソッドに入ることがあるためです。 [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) から既に派生したクラスから派生する場合は、プロパティ システム自体がそのサービスの一部として仮想メソッドを内部的に呼び出し、公開することに注意してください。 実行時の初期化での潜在的な問題を回避するために、クラスのコンストラクター内では依存関係プロパティを設定しないでください。

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>C++/CX アプリの依存関係プロパティの登録

C++/CX のプロパティ登録の実装は、C# より込み入っています。それは、ヘッダー ファイルと実装ファイルに分かれているためと、実装ファイルのルート スコープでの初期化が好ましくないためです  (Visual C++ コンポーネント拡張機能 (C++/CX) はルート スコープの静的な初期化子コードを直接 **DllMain** に配置しますが、C# コンパイラは静的な初期化子をクラスに割り当てて、**DllMain** の読み込み時ロックの問題を回避しています)。 ここでのベスト プラクティスは、クラスごとに 1 つ、そのクラスの依存関係プロパティの登録をすべて実行するヘルパー関数を宣言することです。 続いて、アプリで使う各カスタム クラスについて、使う各カスタム クラスが公開したヘルパー登録関数を参照する必要があります。 の前に、 [**アプリケーションコンストラクター**](/uwp/api/windows.ui.xaml.application.-ctor) () の一部として、各ヘルパー登録関数を1回呼び出し `App::App()` `InitializeComponent` ます。 そのコンストラクターは、アプリが実際に初めて参照されたときにだけ実行され、たとえば中断されたアプリが再開された場合には実行されません。 また、前の C++ 登録の例に示すように、各 [**Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register) 呼び出し時の **nullptr** チェックが重要です。これによって、関数の呼び出し元がプロパティを 2 回登録できないことが保証されます。 このようなチェックをしないで 2 回、登録を呼び出すと、プロパティ名が重複するためアプリはおそらくクラッシュします。 [XAML ユーザー コントロールとカスタム コントロールのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20user%20and%20custom%20controls%20sample)でサンプルの C++/CX バージョンのコードを参照すると、この実装パターンを確認できます。

## <a name="related-topics"></a>関連トピック

- [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty.Register**](/uwp/api/windows.ui.xaml.dependencyproperty.register)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [XAML ユーザーとカスタム コントロールのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20user%20and%20custom%20controls%20sample)
 