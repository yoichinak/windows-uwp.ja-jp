---
title: DirectX の移植の計画
description: DirectX 9 から DirectX 11 とユニバーサル Windows プラットフォーム (UWP) へのゲーム移植プロジェクトを計画してください。グラフィックス コードのアップグレードと、Windows ランタイム環境へのゲームの配置が必要です。
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, DirectX, 移植
ms.localizationpriority: medium
ms.openlocfilehash: 032eeaf2a17ef244287e25e6d9ff32a12c61e137
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258468"
---
# <a name="plan-your-directx-port"></a>DirectX の移植の計画



**要約**

-   DirectX の移植の計画
-   [Important changes from Direct3D 9 to Direct3D 11](understand-direct3d-11-1-concepts.md)
-   [Feature mapping](feature-mapping.md)


DirectX 9 から DirectX 11 とユニバーサル Windows プラットフォーム (UWP) へのゲーム移植プロジェクトを計画してください。グラフィックス コードのアップグレードと、Windows ランタイム環境へのゲームの配置が必要です。

## <a name="plan-to-port-graphics-code"></a>グラフィックス コードの移植の計画


UWP へのゲームの移植を開始する前に、ゲームに Direct3D 8 の要素が残っていない状態にすることが重要です。 ゲームに固定関数パイプラインが残っていないことを確かめてください。 固定パイプライン機能など、推奨されなくなった機能の全一覧については、「[推奨されなくなった機能](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated)」をご覧ください。

Direct3D 9 から Direct3D 11 へのアップグレードは、変更箇所を探して置き換える作業にとどまりません。 Direct3D デバイス、デバイス コンテキスト、グラフィックス インフラストラクチャの違いを把握し、Direct3D 9 以降のその他の重要な変更について理解する必要があります。 このセクションの他のトピックを読むことで、このプロセスを開始できます。

D3DX と DXUT のヘルパー ライブラリは、独自のヘルパー ライブラリか、コミュニティ ツールに置き換える必要があります。 詳しくは、「[DirectX 11 API への DirectX 9 の機能のマッピング](feature-mapping.md)」をご覧ください。

> **Note**   You can use the [DirectX Tool Kit](https://github.com/Microsoft/DirectXTK) or [DirectXTex](https://github.com/Microsoft/DirectXTex) to replace some functionality that was formerly provided by D3DX and DXUT.

 

Shaders written in assembly language should be upgraded to HLSL using shader model 4 level 9\_1 or 9\_3 functionality, and shaders written for the Effects library will need to be updated to a more recent version of HLSL syntax. 詳しくは、「[DirectX 11 API への DirectX 9 の機能のマッピング](feature-mapping.md)」をご覧ください。

さまざまな [Direct3D 機能レベル](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)について確かめてください。 機能レベルは、既知の機能のセットを定義することで、幅広いビデオ ハードウェアを分類するものです。 各セットは 9.1 ～ 11.2 のバージョンの Direct3D にほぼ対応しています。 すべての機能レベルで DirectX 11 API を使います。

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>CoreWindow への Win32 UI コードの移植の計画


UWP アプリは、[**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) と呼ばれる、アプリ コンテナーに対して作成されるウィンドウで実行されます。 ゲームでは、デスクトップのウィンドウよりも必要な実装の詳細が少ない [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) からの継承によってウィンドウを制御します。 ゲームのメイン ループは [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run) メソッドにあります。

UWP アプリのライフサイクルはデスクトップ アプリとは大きく異なります。 ゲームの保存は頻繁に行う必要があります。中断イベントが発生したときに、アプリで実行中のコードを停止できる時間が限られているうえ、アプリの再開時には、プレーヤーが中断時の状態からすぐにゲームを再開できるようにする必要があるためです。 ゲームの保存は、再開時に連続したゲームプレイ エクスペリエンスを維持できるだけの頻度で行う必要があります。ただし、フレームレートに影響したり、ゲームに引っかかりをもたらしたりしない程度にしてください。 ゲームは、ゲームが終了状態から再開されたときに、必要に応じてゲームの状態を読み込む必要があります。

[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-progguide) は、D3DXMath と XNAMath の代わりになり、数学演算ライブラリが必要になったときに役立ちます。 DirectXMath には、高速かつ移植可能なデータ型と、シェーダーで利用できるように整列およびパッキングされた型があります。

[インタロック API](https://docs.microsoft.com/windows/desktop/Sync/what-s-new-in-synchronization) などのネイティブ ライブラリは、ARM の組み込みメソッドをサポートするために拡張されました。 ゲームでインタロック API を使う場合は、DirectX 11 と UWP でも使い続けることができます。

Microsoft のテンプレートとコード サンプルでは新しい C++ 機能が使われており、馴染みがない可能性があります。 たとえば、UI スレッドをブロックすることなく Direct3D リソースを読み込むために、非同期メソッドが[**ラムダ式**](https://docs.microsoft.com/cpp/cpp/lambda-expressions-in-cpp)と共に使われます。

頻繁に使う 2 つの概念があります。

-   マネージ リファレンス ([ **^ 演算子**](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)) と [**マネージ クラス**](https://docs.microsoft.com/cpp/windows/classes-and-structs-cpp-component-extensions) (ref クラス) は、Windows ランタイムの基本となる部分です。 Windows ランタイム コンポーネントとのインターフェイスとして機能するマネージ ref クラスを使う必要があります。具体的には、[**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) などです。詳しくはチュートリアルをご覧ください。
-   Direct3D 11 の COM インターフェイスを操作する場合は、[**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) テンプレート型を使うと COM ポインターが使いやすくなります。

 

 




