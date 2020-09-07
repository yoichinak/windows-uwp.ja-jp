---
description: xBind マークアップ拡張機能のデータ バインディング パスのリース ステップとして関数を使用する方法について説明します。
title: x:Bind の関数
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 4d677767f7eb73bf46784b3f256b511e54013548
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170046"
---
# <a name="functions-in-xbind"></a>x:Bind の関数

> [!NOTE]
> **{x:Bind}** によりアプリでデータ バインディングを使う方法に関する一般的な情報 (および **{x:Bind}** と **{Binding}** の全体的な比較) については、「[データ バインディングの詳細](data-binding-in-depth.md)」および「[{x:Bind} マークアップ拡張](../xaml-platform/x-bind-markup-extension.md)」をご覧ください。

Windows 10 バージョン 1607 以降、 **{x:Bind}** はバインド パスのリーフ ステップとしての関数の使用をサポートします。 これにより次のことが可能になります。

- 値の変換を実現するためのより簡単な方法
- 複数のパラメーターに依存するようにバインディングする方法

> [!NOTE]
> **{x:Bind}** で関数を使うには、アプリの対象が SDK バージョン 14393 以降である必要があります。 アプリがそれよりも前のバージョンの Windows 10 を対象としている場合は、関数を使えません。 ターゲット バージョンについて詳しくは、「[バージョン アダプティブ コード](../debug-test-perf/version-adaptive-code.md)」をご覧ください。

次の例では、項目の背景と前景が、Color パラメーターに基づいて変換を行うために関数にバインドされています。

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>XAML 属性の使用方法

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>関数へのパス

[関数へのパス](../xaml-platform/x-bind-markup-extension.md#property-path)は、他のプロパティ パスと同じように指定され、関数を見つけるために[ドット (.)](../xaml-platform/x-bind-markup-extension.md#property-path-resolution)、[インデクサー](../xaml-platform/x-bind-markup-extension.md#collections)、または[キャスト](../xaml-platform/x-bind-markup-extension.md#casting)を含めることができます。

静的関数は、XMLNamespace:ClassName.MethodName 構文を使って指定できます。 たとえば、分離コードで静的関数にバインドする場合は、次の構文を使用します。

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

また、マークアップでシステム関数を直接使用して、日付の書式設定、テキストの書式設定、テキストの連結など、シンプルなシナリオを実現することもできます。たとえば、次のようになります。

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

モードが OneWay/TwoWay である場合、関数のパスでは変更の検出が実行され、それらのオブジェクトに変更があるとバインディングが再評価されます。

バインディングされる関数には以下のことが要求されます。

- コードとメタデータにアクセスできる必要があります。したがって、C# では internal/private を使用できますが、C++/CX ではメソッドをパブリック WinRT メソッドにする必要があります。
- オーバーロードは引数の型ではなく数に基づき、同じ数の引数を持つ最初のオーバーロードとの一致が試みられます。
- 引数の型は渡されるデータと一致する必要があります。縮小変換は行われません。
- 関数の戻り値の型は、バインディングを使用しているプロパティの型と一致する必要があります。

バインディング エンジンでは、関数名で起動されたプロパティ変更通知に反応し、必要に応じてバインディングが再評価されます。 たとえば、次のようになります。

```xaml
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```

```csharp
public class Person : INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> x:Bind で関数を使用すると、WPF でコンバーターや MultiBinding によってサポートされていた同じシナリオを実現できます。

## <a name="function-arguments"></a>関数の引数

複数の関数引数をコンマ (,) で区切って指定できます。

- バインディング パス – そのオブジェクトにバインドする場合と同じ構文です。
  - モードが OneWay/TwoWay の場合は、変更検出が実行されて、オブジェクトが変化するとバインディングが再評価されます。
- 引用符で囲まれた定数文字列 – 文字列として指定するには引用符が必要です。 文字列で引用符をエスケープするにはハット (^) を使用できます
- 定数 - たとえば -123.456
- ブール値 – "x:True" または "x:False" と指定します

### <a name="two-way-function-bindings"></a>双方向の関数バインド

双方向のバインディング シナリオでは、逆方向のバインドのために第 2 の関数を指定する必要があります。 これを行うには **BindBack** バインディング プロパティを使用します。 次の例で、関数が受け取る必要のある引数は 1 つで、モデルにプッシュバックする必要のある値です。

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

## <a name="see-also"></a>関連項目
* [{x:Bind} マークアップ拡張](../xaml-platform/x-bind-markup-extension.md)