---
title: 3D の Babylon.js ゲームに WebVR サポートを追加する
description: このチュートリアルでは、WebVR 仮想現実のサポートを既存の 3D Babylon.js ゲームに追加する方法について説明します。
ms.date: 11/29/2017
ms.topic: article
keywords: WebVR、Edge、Web 開発、Babylon、Babylonjs、Babylon.js、JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: a01e459160025e9ed1b83fbe81da6d562340691e
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094549"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>3D の Babylon.js ゲームに WebVR サポートを追加する

Babylon.js を使って 3D ゲームを作成したことがあり、仮想現実 (VR) をサポートしてみたいと考えている場合には、次の簡単な手順に沿って、実現することができます。

このチュートリアルでは、3D ゲームを WebVR で実行できるようにするための、いくつかの手順について説明します。 [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) ヘッドセットを使って、Microsoft Edge に追加された WebVR サポートを利用します。 これらの変更をゲームに適用すると、WebVR をサポートする他のブラウザー/ヘッドセットの組み合わせでも動作します。



## <a name="prerequisites"></a>必要条件

- テキスト エディター ([Visual Studio Code](https://code.visualstudio.com/download) など)
- コンピューターに接続されている Xbox コントローラー
- Windows 10 Creators Update
- [Windows Mixed Reality を実行するための最小要件仕様](https://developer.microsoft.com/windows/mixed-reality/immersive_headset_setup)を満たすコンピューター
- Windows Mixed Reality デバイス (オプション) 



## <a name="getting-started"></a>はじめに

最も簡単に始める方法としては、[Windows-tutorials-web GitHub repo](https://github.com/Microsoft/Windows-tutorials-web) に移動して、緑色の **[Clone or download]** (複製またはダウンロード) ボタンを押し、 **[Open in Visual Studio]** (Visual Studio で開く) を選択します。

![[Clone or download] (複製またはダウンロード) ボタン](images/3dclone.png)

プロジェクトを複製しない場合は、zip ファイルとしてダウンロードすることもできます。
[[Before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)] と [[After](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)] という 2 つのフォルダーがあります。 [Before] フォルダーは VR 機能が追加される前のゲームで、[After] フォルダーは、VR サポートを追加して完成したゲームです。

[Before] と [After] のフォルダーには、次のファイルが含まれています。
-   **textures/** - ゲームで使用するイメージが含まれるフォルダー。
-   **css/** - ゲームの CSS が含まれるフォルダー。
-   **js/** - JavaScript ファイルが含まれるフォルダー。 main.js ファイルはゲームで、他のファイルは使用されるライブラリです。
-   **models/** - 3D モデルが含まれるフォルダー。 このゲームでは、恐竜の 1 つのモデルのみがあります。
-   **Index.html** - ゲームのレンダラーをホストする Web ページ。 Microsoft Edge でこのページを開くと、ゲームが起動します。

Microsoft Edge でそれぞれの index.html ファイルを開くと、ゲームの両方のバージョンをテストできます。



## <a name="the-mixed-reality-portal"></a>Mixed Reality ポータル

Windows Mixed Reality をまだお使いでなく、Windows 10 Creators Update がコンピューターにインストールされており、Windows Mixed Reality に対応しているグラフィックス カードが備えられている場合は、Windows 10 のスタート メニューから **[Mixed Reality ポータル]** アプリを開いてみてください。

![Mixed Reality ポータルの検索](images/mixed-reality-portal.png)

すべての要件が満たされいる場合、開発者向け機能をオンにすると、コンピューターに接続された Windows Mixed Reality ヘッドセットをシミュレートできます。 また既に実際のヘッドセットをお持ちの場合は、接続してセットアップを実行します。

> [!IMPORTANT]
> Mixed Reality ポータルは、このチュートリアルの間、常に開いておく必要があります。

これで Microsoft Edge を使用して WebVR を試す準備ができました。

## <a name="2d-ui-in-a-virtual-world"></a>仮想世界の 2D UI

>[!NOTE]
> スターター サンプルは [[**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)] フォルダーにあります。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) は、VR 向けのライブラリです。これを使用して、VR 表示および非 VR 表示に適したシンプルな対話型のユーザー インターフェイスを作成できます。
2D 要素を作成するために、Babylon.js の拡張機能である `GUI` ライブラリがサンプル全体で使用されています。


調整する属性の数に基づいて、数行で 2D テキスト `GUI` 要素を作成できます。
次のコード スニペットは [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) サンプルに既に含まれていますが、何が行われているか見ていきましょう。
最初に、GUI の対象を設定するために、[`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) オブジェクトを作成します。 このサンプルでは、これを `CreateFullScreenUI()` に設定しています。したがって、UI の対象は画面全体となります。 `AdvancedDynamicTexture` を作成した後、`GUI.Rectanlge()` および `GUI.TextBlock()` を使用して、ゲームの開始時に表示される 2D テキスト ボックスを作成します。


このコードは、[**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168) 内に追加されています。
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


この UI は一度作成すると表示されますが、`isVisible` を使用して、ゲームで行われていることに基づいてオンとオフを切り替えることができます。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>ヘッドセットを検出する

VR アプリケーションで 2 種類のカメラを備えて、複数のシナリオをサポートすることは、推奨されるプラクティスです。 このゲームでは、ヘッドセットの接続を必要とする 1 つのカメラと、ヘッドセットを使用しないもう 1 つのカメラをサポートします。 ゲームでどちらを使用するかを判断するため、最初にヘッドセットが検出されたかどうかをチェックする必要があります。 これを行うために、[`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays) を使用します。


次のコードを、**main.js** の `window.addEventListener('DOMContentLoaded')` の前に追加します。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

`headset` 変数に保存されている情報を使って、ユーザーに適切なカメラを選択することができます。


## <a name="creating-and-selecting-the-initial-camera"></a>初期のカメラを作成および選択する

Babylon.js では、[`WebVRFreeCamera`](https://doc.babylonjs.com/api/classes/babylon.webvrfreecamera) を使用して、WebVR をすばやく追加できます。 このカメラはキーボード入力を受け取ることができ、VR ヘッドセットを使用して、"ヘッド" の回転を制御することができます。


### <a name="step-1-checking-for-headsets"></a>手順 1: ヘッドセットを確認する

フォールバック カメラには、元のゲームで現在使用されている [`UniversalCamera`](https://doc.babylonjs.com/api/classes/babylon.universalcamera) を使用します。

`headset` 変数を確認して、`WebVRFreeCamera` カメラを使用できるかどうかを判断します。

`camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` を次のコードに置き換えます。
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>手順 2: WebVRFreeCamera をアクティブ化する
ほとんどのブラウザーでは、このカメラをアクティブ化するため、仮想操作を要求するいくつかの操作を実行する必要があります。
この機能をマウス クリックにフックします。


`createScene()` 関数内のコードを `camera.applyGravity = true;` の後に貼り付けます。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

ゲームでクリックすると、次のようなプロンプトが表示されます。また、ユーザーが以前にプロンプトを受け入れている場合、ただちにヘッドセットにゲームが表示されます。

![イマーシブのプロンプト](images/immersiveview.png)

また、`WebVRFreeCamera` に切り替える前に `UniversalCamera` ビューを表示するコード断片を追加できます。これにより、ユーザーには、青いウィンドウの代わりにゲームが表示されます。 

`engine.runRenderLoop(function () {` の後に以下を追加します。
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>手順 3: ゲームパッドのサポートを追加する

`WebVRFreeCamera` は、初期状態ではゲームパッドをサポートしないため、ゲームパッドのボタンをキーボードの方向キーにマッピングします。 カメラの `inputs` プロパティを詳細に設定することで、これを行います。 左のアナログ スティックの上、下、左、右を方向キーに対応させるコードを追加します。これでゲームパッドを使えるようになります。


`scene.onPointerDown = function() {...}` 呼び出しの下にこのコードを追加します。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>手順 4: 試してみる

ヘッドセットとゲーム コントローラーを接続して **index.html** を開き、青いゲーム ウィンドウで左クリックすると、ゲームが VR モードに切り替わります。 ヘッドセットを装着して、結果を確認します。 



## <a name="conclusion"></a>まとめ

お疲れさまでした。 WebVR サポートが追加された、完全な Babylon.js ゲームが完成しました。 学習した内容を利用して、さらにこのゲームを改良したり、変更したりできます。
