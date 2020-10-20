---
title: DirectX 11 の移植に関する FAQ
description: ユニバーサル Windows プラットフォーム (UWP) へのゲームの移植についてよく寄せられる質問に対してお答えします。
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: bc6e1c2053b6ab3afe6eb42ed8b5223feb8ffb65
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192992"
---
# <a name="directx-11-porting-faq"></a>DirectX 11 の移植に関する FAQ




ユニバーサル Windows プラットフォーム (UWP) へのゲームの移植についてよく寄せられる質問に対してお答えします。

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>ゲームの移植作業は、API メソッドを検索して置き換える作業になりますか。それとも、より慎重な移植プロセスを計画する必要がありますか。


Direct3D 11 は Direct3D 9 からの大幅なアップグレードです。 仮想化されたグラフィックス アダプターとそのコンテキストのための独立した API や、デバイス リソースのポリモーフィズムの新しいレイヤーなど、いくつかのパラダイム シフトがあります。 ゲームでは、グラフィックス ハードウェアを実質的に同じ方法で使い続けることができますが、新しい Direct3D 11 API のアーキテクチャについて学び、正しい API コンポーネントが使われるようにグラフィックス コードの各部分を更新する必要があります。 「[DirectX 9 からの DirectX 11 と Windows ストアへの移行](porting-considerations.md)」をご覧ください。

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>新しいデバイス コンテキストの用途は何ですか。 自分の Direct3D 9 デバイスを Direct3D 11 デバイスに置き換えたり、Direct3D 9 デバイス コンテキストを Direct3D 11 デバイス コンテキストに置き換えたりする必要はありますか。その両方が必要でしょうか。


Direct3D デバイスは、ビデオ メモリにリソースを作成するために使われます。一方、デバイス コンテキストは、パイプラインの状態を設定し、レンダリング コマンドを生成するために使われます。 詳しくは、「[Direct3D 9 と Direct3D 11 の間の重要な変更点](understand-direct3d-11-1-concepts.md)」をご覧ください。

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>UWP 向けのゲーム タイマーを更新する必要はありますか。


[**QueryPerformanceCounter**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter) と [**QueryPerformanceFrequency**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancefrequency) は、引き続き UWP アプリのゲーム タイマーを実装する最適な手段です。

タイマーと UWP アプリのライフサイクルのニュアンスに注意する必要があります。 中断と再開は、プレーヤーによるデスクトップ ゲームの再起動とは異なります。ゲームでは、最後にプレイされていた時点のスナップショットを再開します。 数週間など、長時間経過した場合は、ゲーム タイマーの実装は適切に動作しない可能性があります。 ゲームの再開時にアプリのライフサイクル イベントを使ってタイマーをリセットできます。

まだ RDTSC 命令を使っているゲームはアップグレードする必要があります。 「[ゲームのタイミングとマルチコア プロセッサ](/windows/desktop/DxTechArts/game-timing-and-multicore-processors)」をご覧ください。

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>自分のゲーム コードは D3DX と DXUT に基づいています。 コードの移植に役立つものはありますか。


[DirectX ツール キット (DirectXTK)](https://github.com/Microsoft/DirectXTK) コミュニティのプロジェクトには、Direct3D 11 で利用できるヘルパー クラスが用意されています。

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>デスクトップと Microsoft Store のコードパスを維持操作方法には、


Chuck Walbourn の記事シリーズでは、 [2 つのゲームのコーディング技法](https://blogs.msdn.com/b/chuckw/archive/2012/09/17/dual-use-coding-techniques-for-games.aspx) について説明しています。デスクトップと Microsoft Store コードパスの間でコードを共有するためのガイダンスを提供しています。

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>DirectX UWP アプリの画像リソースを読み込む方法を教えてください。


画像を読み込むための API パスは 2 つあります。

-   コンテンツ パイプラインは Direct3D のテクスチャ リソースとして使われる DDS ファイルに画像を変換します。 「[ゲームまたはアプリケーションでの 3-D アセットの使用](/visualstudio/designers/using-3-d-assets-in-your-game-or-app)」をご覧ください。
-   [Windows Imaging Component](/windows/desktop/wic/-wic-lh) を使うと、さまざまな形式から画像を読み込むことができます。このコンポーネントは、Direct2D ビットマップや、Direct3D のテクスチャ リソースに使用できます。

[DirectXTK](https://github.com/Microsoft/DirectXTK) または [DirectXTex](https://github.com/Microsoft/DirectXTex) の DDSTextureLoader と WICTextureLoader を使うこともできます。

## <a name="where-is-the-directx-sdk"></a>DirectX SDK の場所


DirectX SDK は Windows SDK に同梱されています。 Windows SDK に同梱されていない最新の DirectX SDK は、2010 年 6 月のものです。 Direct3D サンプルは、他の Windows アプリ サンプルと共にコード ギャラリーにあります。

## <a name="what-about-directx-redistributables"></a>DirectX の再頒布可能パッケージについて教えてください。


Windows SDK の大半のコンポーネントは、サポートされているバージョンの OS に含まれていますが、DLL コンポーネントは含まれていません (DirectXMath など)。 UWP アプリで使うことができるすべての Direct3D API コンポーネントは、ゲームで既に使用できる状態です。再頒布する必要はありません。

Win32 デスクトップ アプリケーションは引き続き DirectSetup を使います。そのため、ゲームのデスクトップ バージョンをアップグレードする場合は、「[ゲーム開発者向けの Direct3D 11 の展開](/windows/desktop/direct3darticles/direct3d11-deployment)」をご覧ください。

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>Effects から離れる前にデスクトップ コードを DirectX 11 に更新する方法はありますか。


[Direct3D 11 向けの Effects の更新に関するページ](https://github.com/Microsoft/FX11)をご覧ください。 Effects 11 は、レガシ DirectX SDK ヘッダーへの依存を排除します。移植のサポート用に作成されたものであり、デスクトップ アプリでのみ利用できます。

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>UWP に DirectX 8 ゲームを移植するためのパスはありますか。


Yes:

-   「[Direct3D 9 への変換](/windows/desktop/direct3d9/converting-to-directx-9)」をご覧ください。
-   ゲームに固定パイプラインが残っていないことを確かめます。「[推奨されなくなった機能](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)」をご覧ください。
-   次に、DirectX 9 移植パスに従います。「[チュートリアル: DirectX 11 とユニバーサル Windows プラットフォーム (UWP) への簡単な Direct3D 9 アプリの移植](walkthrough--simple-port-from-direct3d-9-to-11-1.md)」をご覧ください。

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>UWP に DirectX 10 または 11 ゲームを移植することはできますか。


DirectX 10.x と 11 のデスクトップ ゲームは、UWP に簡単に移植できます。 「[Direct3D 11 への移行](/windows/desktop/direct3d11/d3d11-programming-guide-migrating)」をご覧ください。

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>マルチモニター システムで適切なディスプレイ デバイスを選ぶにはどうすればよいですか。


アプリを表示するモニターはユーザーが選びます。 最初のパラメーターを **nullptr** に設定して [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) を呼び出すことで、Windows が正しいアダプターを提供できるようにしてください。 次にデバイスの [**IDXGIDevice interface**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice) を取得し、[**GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) を呼び出して、DXGI アダプターを使ってスワップ チェーンを作成します。

## <a name="how-do-i-turn-on-antialiasing"></a>アンチエイリアシングをオンにするにはどうすればよいですか。


Direct3D デバイスを作成するとアンチエイリアシング (マルチサンプリング) が有効になります。 [**CheckMultisampleQualityLevels**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels)を呼び出してマルチサンプリングサポートを列挙し、次に、[] を呼び出すときに、 [**DXGI \_ サンプル \_ DESC 構造**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc)にマルチサンプリングオプションを[**設定します。**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-createsurface)

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>自分のゲームでは、マルチスレッドや遅延レンダリングを使ってレンダリングを行います。 Direct3D 11 向けに何を把握しておく必要がありますか。


まず、「[Direct3D 11 でのマルチスレッドの概要](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro)」をご覧ください。 主な違いの一覧については、「[Direct3D のバージョン間におけるスレッドの違い](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-differences)」をご覧ください。 遅延レンダリングでは*イミディエイト コンテキスト*ではなくデバイスの*遅延コンテキスト*が使われることに注意してください。

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>Direct3D 9 以降のプログラム可能なパイプラインに関する詳しい情報はどこにありますか。


次のトピックをご覧ください。

-   [HLSL 用プログラミング ガイド](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-pguide)
-   [Direct3D 10 のよく寄せられる質問](/windows/desktop/DxTechArts/direct3d10-frequently-asked-questions)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>モデルには .x ファイル形式の代わりに何を使えばよいですか。


.X ファイル形式を公式に置き換えることはできませんが、多くのサンプルでは SDKMesh 形式が使用されています。 Visual Studio には、一般的な形式を CMO ファイルにコンパイルする[コンテンツ パイプライン](/visualstudio/designers/using-3-d-assets-in-your-game-or-app)があります。CMO ファイルは、Visual Studio 3D スターター キットのコードか、[DirectXTK](https://github.com/Microsoft/DirectXTK) を使って読み込むことができます。

## <a name="how-do-i-debug-my-shaders"></a>シェーダーをデバッグするにはどうしたらよいですか。


Microsoft Visual Studio には、DirectX グラフィックス用の診断ツールが含まれています。 「[DirectX グラフィックスのデバッグ](/visualstudio/debugger/visual-studio-graphics-diagnostics)」をご覧ください。

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>*x* 関数に相当する Direct3D 11 の要素は何ですか。


「DirectX 11 API への DirectX 9 の機能のマッピング」の「[関数のマッピング](feature-mapping.md#function-mapping)」をご覧ください。

##  <a name="what-is-the-dxgi_format-equivalent-of-y-surface-format"></a>X 形式に相当する DXGI \_ 形式*y*とは


「DirectX 11 API への DirectX 9 の機能のマッピング」の「[サーフェス形式のマッピング](feature-mapping.md#surface-format-mapping)」をご覧ください。

 

 