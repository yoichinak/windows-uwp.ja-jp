---
description: このトピックでは、Windows のズームと要素のサイズ変更について説明し、アプリでこのような新しい対話式操作のメカニズムを使うときのユーザー エクスペリエンスのガイドラインを示します。
title: 光学式ズームとサイズ変更のガイドライン
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ea41bff05fc1fcb284be537b66884df337467579
ms.sourcegitcommit: 4fffc66fac18fc4c80281e2a4afa9c4f2e1f7551
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94513701"
---
# <a name="optical-zoom-and-resizing"></a>光学式ズームとサイズ変更



この記事では、Windows のズームとサイズ変更の要素について説明し、アプリでこのような対話式操作のメカニズムを使うためのユーザー エクスペリエンスのガイドラインを示します。

> **重要な API** : [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、 [**Input (XAML)**](/uwp/api/Windows.UI.Xaml.Input)

光学式ズームを使うと、ユーザーはコンテンツの表示を拡大できます (コンテンツ領域自体に対して実行されます)。一方、サイズ変更を使うと、コンテンツ領域の表示は変更せずに、1 つまたは複数のオブジェクトの相対的なサイズをユーザーが変更できます (コンテンツ領域内のオブジェクトに対して実行されます)。

光学式ズーム操作とセマンティック ズーム操作は両方とも、ピンチ ジェスチャとストレッチ ジェスチャ (指を広げて拡大、互いに近づけて縮小)、Ctrl キーを押しながらマウスのスクロール ホイールをスクロール、または Ctrl キーを (テンキーがない場合は Shift キーも同時に) 押しながらプラス (+) キーまたはマイナス (-) キーを押して実行します。

次の図にサイズ変更と光学式ズームの違いを示します。

**光学式ズーム** : ユーザーは領域を選び、領域全体を拡大します。

![コンテンツ領域上で指を互いに近づけて拡大し、広げて縮小する](images/areazoom.png)

**サイズ変更** : ユーザーは領域内のオブジェクトを選び、そのオブジェクトのサイズを変更します。

![指を互いに近づけてオブジェクトを縮小し、広げて拡大する](images/objectresize.png)

**注**  
光学式ズームと[セマンティック ズーム](../controls-and-patterns/semantic-zoom.md)を混同しないように気を付けてください。 どちらの操作でも同じジェスチャが使われますが、セマンティック ズームは、単一のビュー内で整理されたコンテンツを表示したりナビゲーションしたりする場合に使われます (コンピューターのフォルダー構造、ドキュメント ライブラリ、フォト アルバムなど)。

 

## <a name="dos-and-donts"></a>推奨と非推奨


サイズ変更または光学式ズームをサポートするアプリでは、次のガイドラインに従ってください。

-   最大サイズと最小サイズの制限または範囲が定義されている場合には、視覚的なフィードバックを使って、ユーザーがこの制限に達したことや超過したことを示します。
-   スナップ位置を使うと、論理的な操作停止位置を指定してズームとサイズ変更の動作を変更し、コンテンツの特定の部分がビューポートに表示されるようにできます。 一般的なズーム レベルまたは論理ビューに対してスナップ位置を設定して、ユーザーがこれらのレベルを簡単に選べるようにします。 たとえば、写真のアプリでは 100% の位置にサイズ変更用のスナップ位置を設定します。また、地図のアプリでスナップ位置を設定すると、市、県、国を表示する場合に便利です。

    スナップ位置があると、ユーザーの操作が正確でなくても意図された操作を実行できます。 XAML を使う場合は、 [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) のスナップ位置のプロパティをご覧ください。 JavaScript と HTML の場合は、 [**-ms-content-zoom-snap-points**](/previous-versions/hh771895(v=vs.85)) を使います。

    スナップ位置には次の 2 種類があります。

    -   近接: 指を離した後、スナップ位置の距離のしきい値の範囲内で慣性に従った動きが止まると、スナップ位置が選ばれます。 近接スナップ位置の場合は、ズームとサイズ変更をスナップ位置とスナップ位置の間で止めることができます。
    -   強制: 指を離す前に通過した最後のスナップ位置の直前または直後のスナップ位置が選ばれます (ジェスチャの方向と速度によって異なります)。 操作が必ず強制スナップ位置で止まるようにする必要があります。
-   慣性の物理法則を使います。 これらには、次のものが含まれます。
    -   減速: ユーザーが 2 本の指を互いに近づけたり、遠ざけたりしたときに発生します。 これは滑りやすい表面で滑っている状態から止まるまでの動きに似ています。
    -   バウンド: サイズの制限または範囲を超えると、わずかな跳ね返りの効果が発生します。
-   「[ターゲットの設定のガイドライン](guidelines-for-targeting.md)」に従った領域制御。
-   制限付きのサイズ変更のためにスケーリング ハンドルを提供します。 ハンドルが指定されない場合は、等角投影、つまり比が一定のサイズ変更が既定値です。
-   UI の操作またはアプリ内の追加コントロールの公開用にはズームを使わず、パン領域を使います。 パンについて詳しくは、「[パンのガイドライン](guidelines-for-panning.md)」をご覧ください。
-   サイズ変更できるコンテンツ領域内にサイズ変更できるオブジェクトを置かないようにします。 ただし、次のような例外があります。
    -   サイズ変更できるアイテムがサイズ変更できるキャンバスまたはアート ボードに表示される描画アプリケーション。
    -   地図などの埋め込みオブジェクトがある Web ページ。

    **注**  
    どのような場合でも、すべてのタッチ ポイントがサイズ変更できるオブジェクト内にある場合以外は、コンテンツ領域のサイズが変更されます。

## <a name="related-articles"></a>関連記事

### <a name="samples"></a>サンプル

- [基本的な入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [待機時間が短い入力のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [ユーザー操作モードのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [フォーカスの視覚効果のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>サンプルのアーカイブ

- [入力: XAML ユーザー入力イベントのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [入力: デバイス機能のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [入力: タッチのヒット テストのサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML のスクロール、パン、ズームのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [入力: 簡略化されたインクのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [入力: 操作とジェスチャのサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX タッチ入力のサンプル](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
