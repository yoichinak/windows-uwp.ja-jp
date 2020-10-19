---
description: この記事では、C# で WinUI 3 の XAML テンプレート コントロールを作成する手順について説明します。
title: C# を使用した WinUI 3 アプリ用のテンプレート化された XAML コントロール
ms.date: 09/11/2020
ms.topic: article
keywords: Windows 10, UWP, カスタム コントロール, テンプレート コントロール, WinUI
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 618bfc5a9937d29c546dc9420a1cba1c25fcc0ea
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061700"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>C# を使用した WinUI 3 アプリ用のテンプレート化された XAML コントロール

この記事では、C# で WinUI 3 のテンプレート化された XAML コントロールを作成する手順について説明します。 テンプレート化されたコントロールは **Microsoft.UI.Xaml.Controls.Control** から継承され、XAML コントロール テンプレートを使用してカスタマイズできる視覚的な構造と視覚的な動作を持っています。

この記事の手順を実行する前に、開発環境が WinUI 3 アプリを作成するように構成されていることを確認する必要があります。 セットアップ情報については、「[デスクトップ アプリ用の WinUI 3 の概要](./get-started-winui3-for-desktop.md)」を参照してください。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>空のアプリを作成する (BgLabelControlApp)

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **[新しいプロジェクトの作成]** ダイアログで、 **[空のアプリ (UWP 内の WinUI)]** プロジェクト テンプレートを選択します。このとき、必ず C# 言語バージョンを選択します。 プロジェクト名を "BgLabelControlApp" に設定して、ファイル名が次の例のコードと一致するようにします。 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定します。 このチュートリアルは、 **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (デスクトップの WinUI)\)** プロジェクト テンプレートを使用して作成するデスクトップ アプリでも有効です。**BgLabelControlApp (デスクトップ)** のすべての手順は必ず実行してください。

![空のアプリ プロジェクト テンプレート](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>テンプレート コントロールをアプリに追加する

テンプレート コントロールを追加するには、ツールバーの **[プロジェクト]** メニューをクリックするか、**ソリューション エクスプローラー**でプロジェクトを右クリックし、 **[新しい項目の追加]** を選択します。 **[Visual C#] -> [WinUI]** の下で、 **[カスタム コントロール (WinUI)]** テンプレートを選択します。 新しいコントロールに "BgLabelControl" という名前を指定し、 *[追加]* をクリックします。 これにより、2 つの新しいファイルがプロジェクトに追加されます。 `BgLabelControl.cs` には、コントロールのコードビハインドが含まれます。 

## <a name="update-the-code-behind-file"></a>コードビハインド ファイルを更新する

コード ビハンド ファイル BgLabelControl.xaml.cs では、コンストラクターによってコントロールの **DefaultStyleKey** プロパティが定義されていることに注意してください。 このキーでは、コントロールのコンシューマーがテンプレートを明示的に指定していない場合に使用される既定のテンプレートを識別します。 今作成しているコントロールの場合、キーの値は "*type*" です。 このキーは、後で汎用テンプレート ファイルを実装するときに使用します。

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

今作成しているテンプレート コントロールには、コード内か XAML 内で、またはデータ バインディングを使用してプログラムから設定できるテキスト ラベルがあります。 システムでコントロールのラベルのテキストを最新の状態に保つためには、それを [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty) として実装する必要があります。 これを行うには、まず文字列プロパティを宣言して **Label** という名前を付けます。 バッキング変数を使用する代わりに、[GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) と [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) を呼び出すことによって依存関係プロパティの値の設定と取得を行います。 これらのメソッドは、**Microsoft.UI.Xaml.Controls.Control** が継承する [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject) によって提供されます。

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
次に、依存関係プロパティを宣言し、それをシステムに登録するために、[DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register) を呼び出します。 このメソッドでは、**Label** プロパティの名前と型、プロパティの所有者の型 (**BgLabelControl** クラス)、およびプロパティの既定値を指定します。

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string)));
```

依存関係プロパティを実装するために必要な手順はこの 2 つだけですが、この例では、**OnLabelChanged** イベントに対する省略可能なハンドラーを追加します。 このイベントは、プロパティ値が更新されるたびにシステムによって発生させられます。 この例では、新しいラベルのテキストが空の文字列であるかどうかを確認し、それに応じてクラス変数を更新します。

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    {
        BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
        String s = e.NewValue as String; //null checks omitted
        if (s == String.Empty)
        {
            labelControl.HasLabelValue = false;
        }
        else
        {
            labelControl.HasLabelValue = true;
        }
    }
}
```
依存関係プロパティのしくみについて詳しくは、「[依存関係プロパティの概要](/windows/uwp/xaml-platform/dependency-properties-overview)」をご覧ください。

## <a name="define-the-default-style-for-bglabelcontrol"></a>BgLabelControl の既定のスタイルを定義する
テンプレート コントロールでは、コントロールのユーザーが明示的にスタイルを設定しない場合に使用される既定のスタイル テンプレートを提供する必要があります。 このステップでは、コントロールの汎用テンプレート ファイルを作成します。

(**ソリューション エクスプローラー**で) **[すべてのファイルを表示]** がまだオンになっていることを確認します。 プロジェクト ノードで、新しいフォルダーを作成し、"Themes" という名前を付けます。 `Themes` の下に、種類が **[Visual C#] > [XAML] > [リソース ディクショナリ (WinUI)]** である新しい項目を追加し、"Generic.xaml" という名前を付けます。 XAML フレームワークでテンプレート コントロールの既定のスタイルを検出するために、フォルダーとファイルの名前をこのようにする必要があります。 Generic.xaml の既定のコンテンツを削除し、以下のマークアップを貼り付けます。



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

この例では、**Style** 要素の **TargetType** 属性が **BgLabelControlApp** 名前空間内の **BgLabelControl** 型に設定されていることがわかります。 この型は、上記のコントロールのコンストラクターで **DefaultStyleKey** プロパティに対して指定したものと同じ値です。これにより、これがコントロールの既定のスタイルとして指定されます。

コントロール テンプレート内の **TextBlock** の **Text** プロパティは、このコントロールの **Label** 依存関係プロパティにバインドされています。 このプロパティにバインドするには、[TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension) マークアップ拡張機能を使用します。 また、この例では、**Grid** の背景を、**Control** クラスから継承されている **Background** 依存関係プロパティにバインドしています。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>メイン UI ページに BgLabelControl のインスタンスを追加する

メイン UI ページの XAML マークアップが含まれている `MainPage.xaml` を開きます。 **Button** 要素の直後 (**StackPanel** 内) に次のマークアップを追加します。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

アプリをビルドして実行すると、指定した背景色とラベルを持つテンプレート コントロールが表示されます。

![テンプレート コントロールの結果](images/winui-templated-control-result.png)


