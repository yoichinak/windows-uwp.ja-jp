---
description: Windows アプリと、プラットフォームテキストのスケーリングをサポートするカスタム/テンプレートコントロールをビルドします。
title: テキストの拡大縮小
label: Text scaling
template: detail.hbs
keywords: UWP、テキスト、スケーリング、アクセシビリティ、"コンピューターの簡単操作"、表示、"テキストを大きくする"、ユーザーの操作、入力
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d47523ca69f8088d5e13ab944c5dd2be2d1d8ba
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860168"
---
# <a name="text-scaling"></a>テキストの拡大縮小

![100 ~ 225% のテキストスケーリングの例を示すヒーローの画像。](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 でのテキストスケーリングの例 (100% から225%)*

## <a name="overview"></a>概要

コンピューター画面上のテキスト (モバイルデバイスからノート pc、デスクトップモニター、Surface Hub のジャイアント画面) を読むのは、多くの方にとって困難な場合があります。 逆に、アプリや web サイトで使用されているフォントサイズが必要以上に大きくなるユーザーもいます。

広範なユーザーにとってテキストをできるだけ読みやすくするために、Windows では、OS と個々のアプリケーションの間で相対的なフォントサイズを変更することができます。 拡大鏡アプリ (これは通常、画面の領域内のすべてを拡大するだけであり、独自のユーザビリティの問題が発生する) を使用したり、ディスプレイの解像度を変更したり、DPI スケール (これは、ディスプレイと標準的な表示距離に基づいてすべてのサイズを変更する) に依存したりする代わりに、ユーザーはテキストだけを 100% (既定のサイズ) から最大 225% までの範囲でサイズ変更するための設定にすばやくアクセスできます。

## <a name="support"></a>サポート

ユニバーサル Windows アプリケーション (標準と PWA の両方) では、既定でテキストのスケーリングがサポートされています。

Windows アプリケーションにカスタムコントロール、カスタムテキストサーフェイス、ハードコーディングされたコントロールの高さ、古いフレームワーク、サードパーティのフレームワークが含まれている場合は、ユーザーにとって一貫性のある便利なエクスペリエンスを確保するために更新を行うことが必要になる場合があります。  

DirectWrite、GDI、および XAML SwapChainPanels は、テキストのスケーリングをネイティブでサポートしていませんが、Win32 サポートはメニュー、アイコン、およびツールバーに限定されています。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>ユーザー エクスペリエンス

ユーザーは、[設定] の [画面のサイズを大きくする] スライダーを使用してテキストのスケールを調整できます。 > > の視覚/表示画面が表示されます。

![[コンピューターの簡単操作のビジョン/表示設定] ページのスクリーンショット。 [テキストの拡大] スライダーが表示されます。](images/coretext/text-scaling-settings-100-small.png)  
*[設定] からのテキストスケールの設定-> 簡単なアクセス > ビジョン/表示画面*

## <a name="ux-guidance"></a>UX ガイダンス

テキストのサイズが変更されると、テキストとその新しいレイアウトに合わせて、コントロールとコンテナーのサイズも変更する必要があります。 前述のように、アプリ、フレームワーク、プラットフォームによっては、この作業の大部分が実行されます。 次の UX ガイダンスでは、そうでない場合について説明します。

### <a name="use-the-platform-controls"></a>プラットフォームコントロールの使用

これは既に言いましたか。 繰り返します。可能であれば、さまざまな Windows アプリフレームワークに用意されている組み込みのコントロールを使用して、最小限の労力で可能な最も包括的なユーザーエクスペリエンスを実現します。

たとえば、すべての UWP テキストコントロールは、カスタマイズやテンプレートを使用せずにフルテキストスケーリングエクスペリエンスをサポートします。

いくつかの標準のテキストコントロールを含む基本的な UWP アプリのスニペットを次に示します。

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![テキストを100% から225% にスケーリングするアニメーション。](images/coretext/text-scaling.gif)  
*アニメーション化テキストの拡大縮小*

### <a name="use-auto-sizing"></a>自動サイズ調整を使用する

コントロールに絶対サイズを指定しないでください。 可能な場合は常に、ユーザーとデバイスの設定に基づいて、プラットフォームのサイズを自動的に変更します。  

前の例のこのスニペットでは、一連の `Auto` `*` グリッド列にと幅の値を使用して、グリッド内に含まれる要素のサイズに基づいて、プラットフォームでアプリのレイアウトを調整できるようにします。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>テキストの折り返しを使用する

アプリのレイアウトを可能な限り柔軟に使用できるようにするには、テキストを含むコントロールでテキストの折り返しを有効にします (多くのコントロールでは、既定でテキストの折り返しがサポートされていません)。

テキストの折り返しを指定しない場合は、他の方法でクリップ (前の例を参照) を含むレイアウトが調整されます。

ここでは、とのテキストボックスプロパティを使用して、 `AcceptsReturn` `TextWrapping` レイアウトが可能な限り柔軟になるようにします。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![テキストの折り返しで100% から225% にテキストをスケーリングするアニメーション。](images/coretext/text-scaling-textwrap.gif)  
*テキストの折り返しによるアニメーション化されるテキストの拡大縮小*

### <a name="specify-text-trimming-behavior"></a>テキストのトリミング動作を指定する

テキストの折り返しが優先されない場合は、ほとんどのテキストコントロールを使用して、テキストをクリップするか、テキストのトリミング動作に省略記号を指定できます。 省略記号はスペース自体を占有するので、省略記号を使用することをお勧めします。

> [!NOTE]
> テキストをクリップする必要がある場合は、先頭ではなく文字列の末尾をクリップします。

この例では、 [Texttrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) プロパティを使用して TextBlock 内のテキストをクリップする方法を示します。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![テキスト領域で100% から225% に拡大縮小するテキストのスクリーンショット。](images/coretext/text-scaling-clipping-small.png)  
*テキスト領域を使用したテキストの拡大縮小*

### <a name="use-a-tooltip"></a>ツールヒントを使用する

テキストをクリップする場合は、ツールヒントを使用してフルテキストをユーザーに提供します。

ここでは、テキストの折り返しをサポートしていない TextBlock にツールヒントを追加します。

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>フォントベースのアイコンまたはシンボルをスケーリングしない

フォントベースのアイコンを強調または装飾に使用する場合は、これらの文字の拡大/縮小を無効にします。

ほとんどの XAML コントロールで、 [Istextscalefactorenabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) プロパティをに設定し `false` ます。

### <a name="support-text-scaling-natively"></a>テキストスケーリングのネイティブサポート

カスタムフレームワークとコントロールで [Textscalefactorchanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) uisettings システムイベントを処理します。 このイベントは、ユーザーがシステムでテキストの拡大率を設定するたびに発生します。

## <a name="summary"></a>まとめ

このトピックでは、Windows でのテキストスケーリングのサポートの概要について説明します。また、ユーザーエクスペリエンスをカスタマイズする方法に関する UX と開発者向けのガイダンスも紹介します。

## <a name="related-articles"></a>関連記事

### <a name="api-reference"></a>API リファレンス

- [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
