---
title: コンポジションの調整
description: コンポジション Api を使用して、UI を調整し、パフォーマンスを最適化し、ユーザー設定とデバイスの特性に対応します。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d4aa82f70e9bad7a60a97b6b28f28f3dfd008c9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166406"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Windows UI を使用した効果 & エクスペリエンスの調整

Windows UI には、さまざまな視覚効果、アニメーション、および区別のための手段が用意されています。 ただし、パフォーマンスとカスタマイズ可能性に対するユーザーの期待を満たすことは、アプリケーションの正常な作成においても必要となります。 ユニバーサル Windows プラットフォームは、さまざまな機能と機能を備えた大規模で多様なデバイスファミリをサポートしています。 すべてのユーザーに包括的なエクスペリエンスを提供するには、アプリケーションをデバイス間で拡張し、ユーザー設定を考慮する必要があります。 UI の調整により、デバイスの機能を活用し、快適で包括的なユーザーエクスペリエンスを実現するための効率的な方法が提供されます。

UI の調整は、次の領域に関する高性能で美しい UI の作業を含む広範なカテゴリです。

- ユーザー設定を適用して効果を重視する
- アニメーションのユーザー設定の対応
- 特定のハードウェア機能用の UI の最適化

ここでは、上の領域でビジュアルレイヤーを使用して効果とアニメーションを調整する方法について説明しますが、優れたエンドユーザーエクスペリエンスを実現するためにアプリケーションを調整する方法は他にも多数あります。 さまざまなデバイス用の UI を [調整](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) し、応答性の高い [ui を作成](../design/layout/responsive-design.md)する方法に関するガイダンスドキュメントをご利用いただけます。

## <a name="user-effects-settings"></a>ユーザー効果の設定

ユーザーは、さまざまな理由で Windows エクスペリエンスをカスタマイズすることができます。これは、アプリケーションを尊重し、に適合させる必要があります。 エンドユーザーが制御できる領域の1つは、システム全体で使用される効果の種類を変更することです。

### <a name="transparency-effects-settings"></a>透明度効果の設定

ユーザーがカスタマイズできるエフェクト設定の1つは、透明度の効果をオン/オフにすることです。 これは、設定アプリの [パーソナル化 > の色]、または [設定アプリ] > [アクセスの容易さ > 表示] の順に表示されます。

![設定の透明度オプション](images/tailoring-transparency-setting.png)

有効にすると、透明度を使用するすべての効果が想定どおりに表示されます。 これは、Acrylic、HostBackdropBrush、または完全に不透明でない任意のカスタム効果グラフに適用されます。

オフにすると、既定では、XAML の acrylic brush がこのイベントをリッスンしているため、acrylic マテリアルは自動的に純色に戻ります。 ここでは、透明度効果が有効になっていない場合に、電卓アプリが適切に単色に戻ります。

![Acrylic 計算を使用した電卓と ](images/tailoring-acrylic.png)
 ![ Acrylic の透明度設定への応答](images/tailoring-acrylic-fallback.png)

ただし、カスタム効果の場合、アプリケーションは [AdvancedEffectsEnabled](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) プロパティまたは [AdvancedEffectsEnabledChanged](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) イベントに応答し、透明度のない効果を使用するように効果/効果グラフを切り替える必要があります。 この例を次に示します。

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>アニメーションの設定

同様に、設定のユーザー設定に基づいてアニメーションがオンまたはオフになっていることを確認するために、 [Uisettings.](/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) のプロパティをリッスンして応答する必要があります。これにより、アクセス > 表示 > 簡単になります。

![[設定] の [アニメーション] オプション](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Capabilities API の活用

[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) api を利用することにより、特定のハードウェアで利用可能なコンポジション機能とパフォーマンスが向上したことを検出し、設計を調整して、エンドユーザーがどのデバイスでも高性能で美しいエクスペリエンスを得られるようにすることができます。 これらの Api は、さまざまなフォームファクターに対して優れた効果のスケーリングを実装するためにハードウェアシステムの機能を確認する手段を提供します。 これにより、アプリケーションを適切に調整して、美しいシームレスなエンドユーザーエクスペリエンスを作成することが簡単になります。

この API には、アプリケーション UI のスケーリングの決定に影響を与えるために使用できるメソッドとイベントリスナーが用意されています。 この機能は、複雑な合成操作やレンダリング操作をどの程度の方法で処理できるかを検出し、開発者が使用するための使いやすいモデルで情報を返します。

### <a name="using-composition-capabilities"></a>コンポジション機能の使用

CompositionCapabilities 機能は、Acrylic 資料などの機能に既に利用されています。この場合、シナリオとハードウェアによっては、よりパフォーマンスに優れた効果が適用されます。

この API は、いくつかの簡単な手順で既存のコードに追加できます。

1. アプリケーションのコンストラクターで capabilities オブジェクトを取得します。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. アプリの機能が変更されたイベントリスナーを登録します。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. さまざまな機能レベルを処理するために、イベントコールバックメソッドにコンテンツを追加します。 これは、次の手順のようになります。
1. 効果を使用する場合は、まず capabilities オブジェクトを確認します。 効果をどのように調整するかに応じて、条件チェックや switch 制御ステートメントを使用することを検討してください。

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

完全なコード例については、 [WINDOWS UI Github リポジトリ](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)を参照してください。

## <a name="fast-vs-slow-effects"></a>高速効果と低速効果

アプリケーションでは、CompositionCapabilities API で提供され [ている、サポート](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) されている Areの [高速](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) メソッドからのフィードバックに基づいて、デバイス用に最適化されたその他の効果に対して、高価またはサポートされていない影響をスワップすることができます。 一部の効果は、常に他のリソースよりも多くのリソースを消費することがわかっているため、控えめに使用する必要があり、その他の効果はより自由に使用できます。 ただし、すべての効果については、チェーンとアニメーション化を使用して、いくつかのシナリオや組み合わせが効果グラフのパフォーマンス特性を変更することがあります。 次に、個々の効果の thumb パフォーマンス特性の規則をいくつか示します。

- パフォーマンスに大きな影響があるとわかっている効果は、ブラー (ガウス)、シャドウマスク、BackDropBrush、HostBackDropBrush、およびレイヤービジュアルのようになります。 これらは、低エンドデバイス [(機能レベル 9.1-9.3)](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)には推奨されません。ハイエンドデバイスでは慎重に使用する必要があります。
- パフォーマンスに大きな影響を与える効果には、カラーマトリックス、特定の Blend 効果の BlendModes (輝度、色、鮮やかさ、色合い)、スポットライト、SceneLightingEffect、および (シナリオによっては) BorderEffect があります。 これらの効果は、低レベルのデバイスで特定のシナリオで動作する場合がありますが、チェーンとアニメーション化には注意が必要です。 使用を2以下に制限することをお勧めします。また、遷移のみをアニメーション化します。
- 他のすべての効果は、アニメーション化とチェーン化を行う際に、パフォーマンスへの影響が少なく、すべての適切なシナリオで動作します。

## <a name="related-articles"></a>関連記事

- [UWP の応答性の高い設計手法](../design/layout/responsive-design.md)
- [UWP デバイスの調整](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)