---
title: Marble Maze のサンプルへのオーディオの追加
description: このドキュメントでは、オーディオを扱う際に考慮する必要のある主な手法について説明すると共に、それらが Marble Maze でどのように適用されているかを紹介します。
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10、UWP、オーディオ、ゲーム、サンプル
ms.localizationpriority: medium
ms.openlocfilehash: df1be02cd962f086e52609550e9cae65e5280b71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172136"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Marble Maze のサンプルへのオーディオの追加

このドキュメントでは、オーディオを扱う際に考慮する必要のある主な手法について説明すると共に、それらが Marble Maze でどのように適用されているかを紹介します。 Marble Maze は、[Microsoft メディア ファンデーション](/windows/desktop/medfound/microsoft-media-foundation-sdk)を使ってオーディオ リソースをファイルから読み込みます。また、オーディオのミキシングと再生、効果の適用は、[XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) を使って行います。

Marble Maze は、バックグラウンドで再生する音楽に加え、ゲームのイベント (大理石が壁に当たったときなど) を示すゲームプレイ音を使っています。 実装で重要となる部分は、大理石がバウンドする音をシミュレートするためにリバーブ (反響) エフェクトを使っている点です。 リバーブ エフェクトを実装すると、小さい空間では反響音がより早く大きい音量で聞こえ、大きい空間では反響音がより遅く小さい音量で聞こえるようになります。

> [!NOTE]
> このドキュメントに対応するサンプル コードは、[DirectX Marble Maze ゲームのサンプルに関するページ](https://github.com/microsoft/Windows-appsample-marble-maze)にあります。

このドキュメントでは、ゲームでオーディオを扱う際に重要となるいくつかの事柄について説明します。取り上げる内容は次のとおりです。

- オーディオ アセットのデコードにはメディア ファンデーションを使い、オーディオの再生には XAudio2 を使います。 ただし、オーディオ アセットを読み込むための機構が既にあり、ユニバーサル Windows プラットフォーム (UWP) アプリで正常に動作する場合は、それを使ってかまいません。

- オーディオ グラフは、アクティブなサウンド 1 つにつき 1 つのソース ボイス、0 個以上のサブミックス ボイス、1 つのマスタリング ボイスを含んでいます。 ソース ボイスは、サブミックス ボイスまたはマスタリング ボイス (あるいはその両方) に送ることができます。 サブミックス ボイスは、他のサブミックス ボイスか、マスタリング ボイスに送られます。

- BGM ファイルが大きい場合は、メモリの使用量を抑えるために、より小さいバッファーに音楽をストリーミングします。

- 状況 (アプリがフォーカスを失った、表示されなくなった、中断されたなど) に応じてオーディオの再生を一時停止します。 アプリが再びフォーカスを取得するか、表示されるか、再開されたら、再生を再開します。

- オーディオのカテゴリは、各サウンドの役割を反映するように設定します。 たとえば、サウンド効果を実現するには、ゲームのバックグラウンドオーディオと**AudioCategory \_ **の**AudioCategory \_ GameMedia**を使用します。

- ヘッドホンなどのデバイスの変更は、オーディオ リソースとインターフェイスをすべて解放し、再作成することによって処理します。

- ディスク領域とストリーミングのコストを最小限に抑える必要がある場合は、オーディオ ファイルを圧縮します。 それ以外の場合は、オーディオを圧縮しない方が、読み込み速度が高くなります。

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>XAudio2 と Microsoft メディア ファンデーションの概要

XAudio2 は、ゲームのオーディオ サポートに特化した Windows 向けの低水準オーディオ ライブラリです。 ゲーム用のデジタル シグナル処理 (DSP) やオーディオ グラフ エンジンを備えています。 XAudio2 は前身の DirectSound と XAudio を拡張したものであり、SIMD 浮動小数点アーキテクチャや HD オーディオなどのコンピューター トレンドをサポートします。 また、今日のゲームに求められる複雑なサウンド処理のニーズにも対応します。

ドキュメント「[XAudio2 の主要な概念](/windows/desktop/xaudio2/xaudio2-key-concepts)」では、XAudio2 を使ううえでの重要な概念について説明しています。 その要点は以下のとおりです。

- [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) エンジンのコアは IXAudio2 インターフェイス。 Marble Maze は、このインターフェイスを使って音声を作成します。また、出力デバイスの変更時や障害発生時の通知も、このインターフェイスを使って受け取ります。

- オーディオ データは**ボイス**によって処理、調整、再生される。

- **ソース音声**とは、オーディオチャネル (mono、5.1 など) のコレクションであり、オーディオデータの1つのストリームを表します。 XAudio2 では、ソース ボイスがオーディオ処理の開始点になります。 通常、サウンド データは、ファイルやネットワークなどの外部ソースから読み込まれて、ソース ボイスに送られます。 Marble Maze は、[メディア ファンデーション](/windows/desktop/medfound/microsoft-media-foundation-sdk) を使って、サウンド データをファイルから読み込みます。 メディア ファンデーションについては、この後で説明します。

- **サブミックス ボイス**はオーディオ データの処理を行う。 たとえば、オーディオ ストリームに変更を加えたり、複数のストリームを 1 つにまとめたりすることができます。 Marble Maze は、サブミックスを使ってリバーブ エフェクトを作成しています。

- **マスタリング**は、ソースとサブミックスの音声からデータを結合し、そのデータをオーディオハードウェアに送信します。

- **オーディオグラフ**には、アクティブなサウンドごとに1つのソース音声、0個以上のサブミックス音声、および1つのマスタリング音声が含まれています。

- ボイスまたはエンジン オブジェクト内でイベントが発生したことは、**コールバック**によってクライアント コードに通知される。 コールバックを使うことで、XAudio2 がバッファーを残して終了した場合にメモリを再利用したり、オーディオ デバイスが変更された場合 (ヘッドホンの接続時や切断時など) に対応したりできます。 Marble Maze がこの機構を使ってデバイスの変更を処理するしくみについては、このドキュメントの「[ヘッドホンとデバイスの変更の処理](#handling-headphones-and-device-changes)」で説明します。

Marble Maze は、2 つのオーディオ エンジン (つまり、2 つの [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) オブジェクト) を使ってオーディオを処理します。 一方のエンジンが BGM を、もう一方のエンジンがゲームプレイ音を処理します。

また、各エンジンについて、マスタリング ボイスを 1 つ作成する必要があります。 既に説明したように、マスター エンジンはオーディオ ストリームを 1 つのストリームに結合し、そのストリームをオーディオ ハードウェアに送ります。 BGM ストリーム (ソース ボイス) は、マスタリング ボイスと 2 つのサブミックス ボイスにデータを出力します。 サブミックス ボイスはリバーブ エフェクトを実行します。

メディア ファンデーションは、さまざまなオーディオ形式とビデオ形式をサポートするマルチメディア ライブラリです。 XAudio2 とメディア ファンデーションは相互補完的な関係にあります。 Marble Maze は、メディア ファンデーションを使ってオーディオ アセットをファイルから読み込み、XAudio2 を使ってオーディオを再生しています。 オーディオ アセットの読み込みに、必ずしもメディア ファンデーションを使う必要はありません。 オーディオ アセットを読み込むための機構が既にあり、ユニバーサル Windows プラットフォーム (UWP) アプリで正常に動作する場合は、それを使ってください。 UWP アプリでオーディオを実装するいくつかの方法については、「[オーディオ、ビデオ、およびカメラ](../audio-video-camera/index.md)」で説明します。

XAudio2 について詳しくは、「[プログラミング ガイド](/windows/desktop/xaudio2/programming-guide)」をご覧ください。 メディア ファンデーションについて詳しくは、[Microsoft メディア ファンデーションに関するページ](/windows/desktop/medfound/microsoft-media-foundation-sdk) をご覧ください。

## <a name="initializing-audio-resources"></a>オーディオ リソースの初期化

Marble Maze では、BGM に Windows Media オーディオ (.wma) ファイルが、ゲームプレイ音に WAV (.wav) ファイルが使われています。 これらの形式は、メディア ファンデーションによってサポートされます。 .wav ファイル形式は XAudio2 がネイティブにサポートしていますが、適切な XAudio2 データ構造体にデータを設定するためには、ゲーム内からファイル形式を手動で解析する必要があります。 メディア ファンデーションを使った方が簡単に .wav ファイルを扱うことができるため、Marble Maze ではメディア ファンデーションを使っています。 メディア ファンデーションでサポートされる全メディア形式の一覧については、「[メディア ファンデーションでサポートされるメディア形式](/windows/desktop/medfound/supported-media-formats-in-media-foundation)」をご覧ください。 Marble Maze では、デザイン時と実行時に別々のオーディオ形式を使うことはしていません。また、XAudio2 でサポートされる ADPCM 圧縮機能は使っていません。 XAudio2 の ADPCM 圧縮について詳しくは、「[ADPCM の概要](/windows/desktop/xaudio2/adpcm-overview)」をご覧ください。

**MarbleMazeMain::LoadDeferredResources** から呼び出される **Audio::CreateResources** メソッドは、ファイルからオーディオ ストリームを読み込み、XAudio2 エンジン オブジェクトを初期化して、ソース ボイス、サブミックス ボイス、マスタリング ボイスを作成します。

### <a name="creating-the-xaudio2-engines"></a>XAudio2 エンジンの作成

既に説明したように、Marble Maze では、オーディオ エンジンを表す [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) オブジェクト (使うオーディオ エンジンごとに 1 つ) を作成します。 オーディオ エンジンを作成するには、[XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) メソッドを呼び出します。 BGM を処理するオーディオ エンジンの作成方法を次の例に示します。

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

ゲームプレイ音を再生するオーディオ エンジンについても、同様の手順で作成します。

UWP アプリでの [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) インターフェイスの扱いは、デスクトップ アプリの場合とは 2 つの点で異なります。 まず、[XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) を呼び出す前に、[CoInitializeEx](/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) を呼び出す必要はありません。 また、**IXAudio2** では、デバイスの列挙がサポートされません。 オーディオ デバイスの列挙方法については、「[デバイスの列挙](/previous-versions/windows/apps/hh464977(v=win.10))」をご覧ください。

### <a name="creating-the-mastering-voices"></a>マスタリング ボイスの作成

以下の例では、**Audio::CreateResources** メソッドが [IXAudio2::CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) メソッドを使って BGM のマスタリング ボイスを作成しています。 この例では、 **m \_ MusicMasteringVoice** は [IXAudio2MasteringVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) オブジェクトです。 2 つの出力チャネルを指定しています。これにより、リバーブ エフェクトのロジックを単純化します。 

入力サンプル レートとして 48000 を指定します。 オーディオ品質と必要な CPU 処理量のバランスを考慮してこのサンプル レートを選びました。 サンプル レートをこれより大きくしても、体感できるほどは品質が上がらず、必要な CPU 処理も増える可能性があります。 

最後に、オーディオ ストリームのカテゴリとして **AudioCategory\_GameMedia** を指定します。こうすることで、ゲームをプレイしている間、ユーザーは、異なるアプリケーションからの音楽を聴くことができます。 音楽アプリの再生中は、 **AudioCategory \_ GameMedia** オプションによって作成されたすべての音声がミュートされます。 再生中のサウンドは、AudioCategory のユーザー設定オプションに **よっ \_ ** て作成されているので、ユーザーはそのまま聞こえます。 オーディオカテゴリの詳細については、「 [オーディオ \_ ストリームの \_ カテゴリ](/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category)」を参照してください。

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

**Audio:: createresources.vb**メソッドは、同じ手順を実行して、再生されたサウンドのマスタリング音声を作成します。ただし、 *Streamcategory*パラメーターの AudioCategory の設定 (既定値) を指定する点が異なります。 ** \_ **

### <a name="creating-the-reverb-effect"></a>リバーブ エフェクトの作成

それぞれのボイスには、オーディオを処理する一連の効果を XAudio2 を使って作成することができます。 そのような一連の効果をエフェクト チェーンと呼びます。 エフェクト チェーンは、1 つまたは複数の効果をボイスに適用するときに使います。 エフェクト チェーンは破壊的に実行できます。つまり、チェーン内の各効果がオーディオ バッファーを上書きできます。 XAudio2 では出力バッファーが無音で初期化される保証がないため、この特性は重要です。 XAudio2 では、エフェクト オブジェクトが、XAPO (Cross-Platform Audio Processing Object) によって表されます。 XAPO について詳しくは、「[XAPO の概要](/windows/desktop/xaudio2/xapo-overview)」をご覧ください。

エフェクト チェーンを作成する際は、以下の手順に従います。

1. エフェクト オブジェクトを作成します。

2. 有効なデータを含む [XAUDIO2 \_ effect \_ 記述子](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) 構造体を設定します。

3. [XAUDIO2 \_ 効果 \_ チェーン](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)構造にデータを設定します。

4. エフェクト チェーンをボイスに適用します。

5. 効果のパラメーターの構造体にデータを設定し、その構造体を効果に適用します。

6. 必要に応じて効果の無効と有効を切り替えます。

**Audio** クラスは、リバーブを実装するエフェクト チェーンを作成するための **CreateReverb** メソッドを定義します。 このメソッドは [XAudio2CreateReverb](/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) メソッドを呼び出して、リバーブ エフェクトのサブミックス ボイスとして機能する **ComPtr&lt;IUnknown&gt;** オブジェクト **soundEffectXAPO** を作成します。

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

[XAUDIO2 \_ effect \_ 記述子](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor)構造体には、効果チェーンで使用される xapo に関する情報が含まれています。たとえば、出力チャネルの目標数です。 **Audio:: CreateReverb**メソッドは、無効状態に設定されている**XAUDIO2 \_ EFFECT \_ 記述子**オブジェクト**soundEffectdescriptor**を作成し、2つの出力チャネルを使用して、リバーブ効果の**soundEffectXAPO**を参照します。 **soundEffectdescriptor** オブジェクトの開始状態を無効としているのは、先にパラメーターを設定してから、効果に伴うゲーム音の変更を開始する必要があるためです。 Marble Maze は、2 つの出力チャネルを使ってリバーブ エフェクトのロジックを単純化します。

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

エフェクト チェーンに複数の効果が存在する場合、各効果についてオブジェクトが必要となります。 [XAUDIO2 \_ 効果 \_ チェーン](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)構造体は、効果に関与する[XAUDIO2 \_ effect \_ 記述子](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor)オブジェクトの配列を保持します。 次の例は、**Audio::CreateReverb** メソッドが 1 つの効果を指定してリバーブを実装する方法を示します。

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

**Audio::CreateReverb** メソッドは、[IXAudio2::CreateSubmixVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) メソッドを呼び出して、効果のサブミックス ボイスを作成します。 これは、 *pEffectChain*パラメーターの[XAUDIO2 \_ 効果 \_ チェーン](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain)オブジェクト**soundEffectChain**を指定して、効果チェーンを音声に関連付けます。 また、Marble Maze では、2 つの出力チャネルと 48 KHz のサンプル レートを指定しています。

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> 既にあるサブミックス ボイスに対し、既にあるエフェクト チェーンをアタッチする場合、または、現在のエフェクト チェーンを置き換える場合は、[IXAudio2Voice::SetEffectChain](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) メソッドを使います。

**Audio::CreateReverb** メソッドは、別途効果に関連付けるパラメーターを設定するために、[IXAudio2Voice::SetEffectParameters](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) を呼び出しています。 このメソッドは、効果に固有のパラメーター構造体を受け取ります。 リバーブの効果パラメーターを含む**M_reverbParametersSmall** [XAUDIO2FX \_ リバーブ \_ PARAMETERS](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)オブジェクトは、すべてのリバーブ効果が同じパラメーターを共有するため、 **Audio:: Initialize**メソッドで初期化されます。 次の例は、**Audio::Initialize** メソッドがニアフィールド リバーブのリバーブ パラメーターを初期化する方法を示します。

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

この例では、ほとんどのリバーブ パラメーターに既定値を使っていますが、ニアフィールド リバーブを指定するために **DisableLateField** を TRUE に、平らな近くの表面をシミュレートするために **EarlyDiffusion** を 4 に、非常に散乱性が高い遠くの表面をシミュレートするために **LateDiffusion** を 15 に設定します。 平らな近くの表面では反響音がより早く大きい音量で聞こえ、散乱性の高い遠くの表面では反響音がより遅く小さい音量で聞こえるようになります。 リバーブ値を調整しながらゲームに適した効果を得ることも、**ReverbConvertI3DL2ToNative** メソッドを使って業界標準の I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0) のパラメーターを使うこともできます。

次の例は、**Audio::CreateReverb** がリバーブのパラメーターを設定する方法を示します。 **newSubmix** は [IXAudio2SubmixVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)** オブジェクトです。 **parameters** は [XAUDIO2FX \_ リバーブ \_ parameters](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)* オブジェクトです。

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

**Audio::CreateReverb** メソッドは、**enableEffect** フラグが設定されている場合は [IXAudio2Voice::EnableEffect](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) を使用して効果を有効にし、終了します。 また、[IXAudio2Voice::SetVolume](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) を使用してボリュームを設定し、[IXAudio2Voice::SetOutputMatrix](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix) を使用して出力マトリックスを設定します。 この部分でボリュームをフル (1.0) に設定し、左右の入力スピーカーと左右の出力スピーカーの両方に対してボリューム マトリックスが無音になるように指定します。 このような処理を行うのは、後から他のコードが (壁の近くから大きな空間の中への移行をシミュレートして) 2 つのリバーブ間のクロスフェードを行うか、または必要な場合は両方のリバーブをミュートするためです。 後でリバーブ パスがミュート解除されたときに、ゲームはマトリックス {1.0f, 0.0f, 0.0f, 1.0f} を設定して、左のリバーブ出力をマスタリング ボイスの左の入力に、右のリバーブ出力をマスタリング ボイスの右の入力にルーティングします。

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze は、**Audio::CreateReverb** メソッドを 4 回 (BGM のために 2 回、ゲームプレイ音のために 2 回) 呼び出します。 次のコードは、BGM 用の **CreateReverb** メソッド呼び出しを示します。

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

XAudio2 で利用できる効果のソースの一覧については、「[XAudio2 のオーディオ エフェクト](/windows/desktop/xaudio2/xaudio2-audio-effects)」をご覧ください。

### <a name="loading-audio-data-from-file"></a>ファイルからのオーディオ データの読み込み

Marble Maze に定義されている **MediaStreamer** クラスは、メディア ファンデーションを使ってオーディオ リソースをファイルから読み込みます。 Marble Maze は、各オーディオ ファイルの読み込みに **MediaStreamer** オブジェクトを 1 つ使います。

各オーディオ ストリームは、**MediaStreamer::Initialize** メソッドを呼び出して初期化されます。 **Audio::CreateResources** メソッドが **MediaStreamer::Initialize** を呼び出して BGM のオーディオ ストリームを初期化する方法を次に示します。

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

**MediaStreamer::Initialize** メソッドは、メディア ファンデーションを初期化する [MFStartup](/windows/desktop/api/mfapi/nf-mfapi-mfstartup) メソッドの呼び出しによって開始されます。 **MF_VERSION** は **mfapi.h** で定義されているマクロで、使用するメディア ファンデーションのバージョンとして指定するものです。

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

次に、**MediaStreamer::Initialize** は [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) を呼び出して [IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader) オブジェクトを作成します。 **IMFSourceReader** オブジェクト **m_reader** は、**url** で指定されたファイルからメディア データを読み取ります。

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

**MediaStreamer::Initialize** メソッドはさらに、[MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) を使用して、オーディオ ストリームの形式を記述する [IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) オブジェクトを作成します。 オーディオ形式にはメジャー タイプとサブタイプの 2 種類があります。 メジャー タイプではメディア全体の形式 (ビデオ、オーディオ、スクリプトなど) を定義します。 サブタイプでは PCM、ADPCM、WMA などの形式を定義します。

**Mediastreamer:: Initialize**メソッドは、 [Imfattributes:: setguid](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid)メソッドを使用して、メジャータイプ ([MF_MT_MAJOR_TYPE](/windows/desktop/medfound/mf-mt-major-type-attribute)) を audio (**mfmediatype \_ オーディオ**)、マイナータイプ ([MF_MT_SUBTYPE](/windows/desktop/medfound/mf-mt-subtype-attribute)) を非圧縮 PCM オーディオ (**mfaudioformat \_ pcm**) として指定します。 **MF_MT_MAJOR_TYPE** と **MF_MT_SUBTYPE** は[メディア ファンデーション属性](/windows/desktop/medfound/media-foundation-attributes)です。 **MFMediaType_Audio** と **MFAudioFormat_PCM** タイプとサブタイプの GUID です。詳細については、[オーディオ メディア タイプ](/windows/desktop/medfound/audio-media-types)に関するページを参照してください。 [IMFSourceReader::SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) メソッドが、メディア タイプをストリーム リーダーに関連付けます。

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

次に、**MediaStreamer::Initialize** メソッドは、[IMFSourceReader::GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) を使用してメディア ファンデーションから完全な出力メディア形式を取得し、[MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) メソッドを呼び出してメディア ファンデーションのオーディオ メディア タイプを [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 構造体に変換します。 **WAVEFORMATEX** 構造体は Waveform オーディオ データの形式を定義します。 Marble Maze はこの構造体を使ってソース ボイスを作成し、大理石の転がる音に低域フィルターを適用します。

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) メソッドは、**CoTaskMemAlloc** を使って [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) オブジェクトを割り当てます。 したがって、このオブジェクトを使い終えたら必ず、**CoTaskMemFree** を呼び出す必要があります。

 

**Mediastreamer:: Initialize**メソッドは、ストリームの長さ**m \_ maxstreamlength inbytes**(バイト単位) を計算することによって終了します。 そのために、[IMFSourceReader::GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) メソッドを呼び出してオーディオ ストリームの継続時間 (100 ナノ秒単位) を取得し、継続時間をセクションに変換した後、平均データ転送速度 (バイト/秒単位) を乗算します。 Marble Maze は後でこの値を使って、各ゲームプレイ音を保持するバッファーを割り当てます。

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>ソース ボイスの作成

Marble Maze は、XAudio2 ソース ボイスを作成して、ソース ボイスに含まれるそれぞれのゲーム音と音楽を再生します。 **Audio** クラスには、BGM 用の [IXAudio2SourceVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) オブジェクトと、ゲームプレイ音を格納する **SoundEffectData** オブジェクトの配列が定義されます。 **SoundEffectData** 構造体は、効果の **IXAudio2SourceVoice** オブジェクトを保持するほか、効果に関連したその他のデータ (オーディオ バッファーなど) を定義します。 **SoundEvent**列挙型を定義し**ます。** Marble Maze は、この列挙体を使って各ゲームプレイ音を識別します。 また、 **Audio** クラスは、この列挙を使用して、 **SoundEffectData** オブジェクトの配列にインデックスを作成します。

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

次の表は、列挙体の各値と、それに関連したサウンド データが格納されているファイル、表現される音の簡単な説明を一覧にしたものです。 オーディオファイルは、 ** \\ Media \\ audio**フォルダーにあります。

| SoundEvent 値  | ファイル名      | 説明                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | 大理石が転がるときに再生されます。                              |
| FallingEvent      | MarbleFall.wav | 大理石が迷路から落ちるときに再生されます。               |
| CollisionEvent    | MarbleHit.wav  | 大理石が迷路にぶつかるときに再生されます。           |
| CheckpointEvent   | Checkpoint.wav | 大理石がチェックポイントを通過するときに再生されます。         |
| MenuChangeEvent   | MenuChange.wav | ユーザーが現在のメニュー項目を変更するときに再生されます。 |
| MenuSelectedEvent | MenuSelect.wav | ユーザーがメニュー項目を選択するときに再生されます。           |

 

以下の例では、**Audio::CreateResources** メソッドを使って BGM のソース ボイスを作成しています。 [XAUDIO2 \_ 送信 \_ 記述子](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor)構造体は、別の音声からの対象となる宛先の音声を定義し、フィルターを使用するかどうかを指定します。 Marble Maze は、**Audio::SetSoundEffectFilter** メソッドを呼び出し、フィルターを使って、転がるボールの音に変化を与えています。 [XAUDIO2 \_ VOICE \_ send](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends)構造体は、1つの出力音声からデータを受信するための一連の音声を定義します。 Marble Maze は、ソース ボイスからのデータを、マスタリング ボイス (再生するサウンドのドライ、つまり変更されない部分が対象) と 2 つのサブミックス ボイス (再生するサウンドのウェット、つまりリバーブのかかった部分を実装) に送ります。

ソース ボイスの作成と構成は、[IXAudio2::CreateSourceVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) メソッドで行います。 このメソッドは、ボイスに送られるオーディオ バッファーの形式を定義する [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) 構造体を受け取ります。 既に説明したように、Marble Maze では PCM 形式を使います。

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>BGM の再生


ソース ボイスは停止した状態で作成されます。 Marble Maze はゲーム ループ内で BGM を開始します。 **MarbleMazeMain::Update** の最初の呼び出しで、**Audio::Start** を呼び出して BGM を開始します。

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

**Audio::Start** メソッドは [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) を呼び出して、BGM のソース ボイスの処理を開始します。

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

ソース ボイスは、そのオーディオ データをオーディオ グラフの次のステージに渡します。 Marble Maze の場合、次のステージには、オーディオにリバーブ エフェクトを適用する 2 つのサブミックス ボイスが含まれます。 1 つは近距離の後期フィールド リバーブを適用するもので、もう 1 つは遠距離の後期フィールド リバーブを適用するものです。

最終的なミキシングに各サブミックス ボイスがどれだけ含まれるかは、空間のサイズと形状によって決定します。 玉が壁の近くまたは小さな空間内にある場合はニアフィールド リバーブの占める量が多くなり、玉が大きな空間内にある場合は後期フィールド リバーブの占める量が多くなります。 この手法により、大理石が迷路内を移動するときに、よりリアルなエコー効果が得られます。 Marble Maze におけるこの効果の実装について詳しくは、Marble Maze のソース コードで **Audio::SetRoomSize** と **Physics::CalculateCurrentRoomSize** をご覧ください。

> [!NOTE]
> ほとんどの空間のサイズが同じであるゲームでは、もっと基本的なリバーブ モデルを使うことができます。 たとえば、すべての空間に 1 つのリバーブ設定を使ったり、定義済みのリバーブ設定を空間ごとに作成したりできます。

**Audio::CreateResources** メソッドは、メディア ファンデーションを使って BGM を読み込みます。 しかし、この時点では、処理対象のオーディオ データがソース ボイスにありません。 さらに、BGM はループするので、データを使いソース ボイスを定期的に更新して、音楽を再生し続ける必要があります。

ソース ボイスにデータが入力された状態を維持するために、ゲーム ループはフレームごとにオーディオ バッファーを更新します。 **MarbleMazeMain::Render** メソッドは、**Audio::Render** を呼び出して BGM のオーディオ バッファーを処理します。 **Audio**クラスは、3つのオーディオバッファー ( **m \_ audiobuffers**) の配列を定義します。 各バッファーは 64 KB (65536 バイト) のデータを保持します。 ループは、メディア ファンデーション オブジェクトからデータを読み取り、ソース ボイスのキューに 3 つのバッファーが入るまで、そのデータをソース ボイスに書き込みます。

> [!CAUTION]
> Marble Maze は 64 KB のバッファーを使って音楽データを保持しますが、より大きいバッファーまたはより小さいバッファーが必要になる場合もあります。 その量は、ゲームの要件によって異なります。

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

メディア ファンデーション オブジェクトがストリームの末尾に到達したときの処理もループで行います。 この場合、[IMFSourceReader::SetCurrentPosition](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) メソッドを呼び出してオーディオ ソースの位置をリセットします。

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

1つのバッファー (またはメモリに完全に読み込まれるサウンド全体) に対してオーディオループを実装するには、サウンドを初期化するときに、 [XAUDIO2_BUFFER](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: LoopCount フィールドを **XAUDIO2 \_ LOOP \_ 無限** に設定します。 Marble Maze は、この手法を使って大理石の転がる音を再生します。

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

ただし、BGM に関しては、Marble Maze は、メモリの使用量を細かく制御できるようにバッファーを直接管理します。 音楽ファイルが大きい場合は、小さいバッファーに音楽データをストリームできます。 そうすることで、ゲームがオーディオ データを処理してストリームできる頻度と、メモリ サイズとのバランスを取ることができます。

> [!TIP]
> フレーム レートが低いまたは変動するゲームでは、メイン スレッドでのオーディオの処理中に、オーディオ内で予期しない一時停止またはポップが発生する可能性があります。これは、オーディオ エンジンに処理対象のオーディオ データが十分にバッファーされていないためです。 ゲームでこの問題が発生しやすい場合は、レンダリングを実行しない別のスレッドでオーディオを処理することを検討してください。 この方法は、ゲームがアイドル状態のプロセッサを使うことができるため、複数のプロセッサを搭載したコンピューターで特に役立ちます。

## <a name="reacting-to-game-events"></a>ゲームのイベントへの対応

ゲームでサウンドの再生と停止のタイミングを制御し、ボリュームやピッチなどのサウンド プロパティを制御するために、**Audio** クラスには **PlaySoundEffect**、**IsSoundEffectStarted**、**StopSoundEffect**、**SetSoundEffectVolume**、**SetSoundEffectPitch**、**SetSoundEffectFilter** などのメソッドが用意されています。 たとえば、大理石が迷路から落ちた場合、**MarbleMazeMain::Update** メソッドが **Audio::PlaySoundEffect** メソッドを呼び出して **FallingEvent** サウンドを再生します。

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

**Audio::PlaySoundEffect** メソッドは [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) メソッドを呼び出してサウンドの再生を開始します。 **IXAudio2SourceVoice::Start** メソッドが既に呼び出されている場合は、もう開始されません。 **Audio::PlaySoundEffect** は、続いて特定のサウンドのカスタム ロジックを実行します。

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

転がる以外の音に関しては、**Audio::PlaySoundEffect** メソッドは [IXAudio2SourceVoice::GetState](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) を呼び出して、ソース ボイスが再生しているバッファーの数を調べます。 アクティブなバッファーがない場合、[IXAudio2SourceVoice::SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) を呼び出して、サウンドのオーディオ データをボイスの入力キューに追加します。 また、**Audio::PlaySoundEffect** メソッドは、ぶつかる音を 2 回連続して再生できるようにします。 これはたとえば、大理石が迷路の角にぶつかったときに発生します。

既に説明したように、Audio クラスは、ローリングイベントのサウンドを初期化するときに、 **XAUDIO2 \_ LOOP の \_ 無限** フラグを使用します。 このイベントに対して最初に **Audio::PlaySoundEffect** が呼び出されたときに、サウンドはループ再生を開始します。 転がる音の再生のロジックを単純化するために、Marble Maze ではサウンドを停止するのではなくミュートします。 Marble Maze は、よりリアルな効果を実現するために、大理石の速度の変化に合わせてサウンドのピッチとボリュームを変化させます。 **MarbleMazeMain::Update** メソッドが速度の変化に合わせて大理石のピッチとボリュームを更新し、大理石が停止したときにボリュームを 0 に設定してサウンドをミュートする方法を次に示します。

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>イベントの中断と再開への対応

ドキュメント「[Marble Maze のアプリケーション構造](marble-maze-application-structure.md)」では、Marble Maze が中断と再開をどのようにサポートするかを説明しています。 ゲームが中断されると、オーディオが一時停止されます。 ゲームが再開されると、オーディオが中断した箇所から再開されます。 このような処理を行うのは、不要な場合はリソースを使わないというベスト プラクティスに従うためです。

ゲームが中断されると、**Audio::SuspendAudio** メソッドが呼び出されます。 このメソッドは [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) メソッドを呼び出して、すべてのオーディオを停止します。 **IXAudio2::StopEngine** はすべてのオーディオ出力を直ちに停止しますが、オーディオ グラフとその効果のパラメーター (たとえば大理石がバウンドするときに適用されるリバーブ エフェクト) は保持します。

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

ゲームが再開されると、**Audio::ResumeAudio** メソッドが呼び出されます。 このメソッドは、[IXAudio2::StartEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) メソッドを使ってオーディオを再開します。 [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) の呼び出しでオーディオ グラフとその効果のパラメーターが保持されているため、オーディオ出力は中断した箇所から再開されます。

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>ヘッドホンとデバイスの変更の処理

Marble Maze は、オーディオ デバイスの変更時など、XAudio2 エンジンのエラーをコールバックを使って処理します。 デバイスの変更の一般的な原因は、ユーザーによるヘッドホンの接続と切断です。 デバイスの変更を処理するエンジン コールバックを実装することをお勧めします。 これを行わないと、ユーザーがヘッドホンの接続または切断を行ったときに、ゲームが再開されるまでサウンドの再生が停止します。

**AudioEngineCallbacks**クラスを定義し**ます**。 このクラスは、[IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) インターフェイスを実装します。

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

[IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) インターフェイスは、オーディオ処理イベントが発生したときや、エンジンで重大なエラーが発生したときにコードに通知されるようにします。 コールバックを登録するために、Marble Maze は、音楽エンジン用の [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) オブジェクトを作成した後に **Audio::CreateResources** で [IXAudio2::RegisterForCallbacks](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) メソッドを呼び出します。

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze では、オーディオ処理の開始または終了時の通知は必要ありません。 したがって、何も処理を行わない [IXAudio2EngineCallback::OnProcessingPassStart](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) メソッドと [IXAudio2EngineCallback::OnProcessingPassEnd](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) メソッドを実装します。 [IXAudio2EngineCallback:: OnCriticalError](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror)メソッドの場合、大理石迷路は**SetEngineExperiencedCriticalError**メソッドを呼び出します。これにより、 **m \_ engineExperiencedCriticalError**フラグが設定されます。

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

重大なエラーが発生した場合、オーディオ処理は停止し、それ以降の XAudio2 への呼び出しはすべて失敗します。 この状況から回復するには、XAudio2 インスタンスを解放して新しく作成する必要があります。 すべてのフレームでゲームループから呼び出される **Audio:: Render** メソッドは、最初に **m \_ engineExperiencedCriticalError** フラグをチェックします。 このフラグが設定されている場合、フラグをクリアし、現在の XAudio2 インスタンスを解放してリソースを初期化した後、BGM を開始します。

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

また、 ** \_ engineExperiencedCriticalError** フラグを使用して、オーディオデバイスが使用できない場合に XAudio2 への呼び出しを防止します。 たとえば、**MarbleMazeMain::Update** メソッドは、このフラグが設定されているときは転がるイベントまたは衝突するイベントのオーディオを処理しません。 アプリは、必要に応じて、すべてのフレームに対してオーディオエンジンの修復を試行します。ただし、コンピューターにオーディオデバイスが搭載されていない場合、またはヘッドフォンが取り外されていて、他の利用可能なオーディオデバイスがない場合は、 **m \_ engineExperiencedCriticalError** フラグが常に設定されることがあります。

> [!CAUTION]
> 原則として、エンジン コールバックの本体でブロック操作を実行しないでください。 これを行うと、パフォーマンスの問題が発生することがあります。 Marble Maze は **OnCriticalError** コールバック内でフラグを設定し、後で通常のオーディオ処理フェーズ中にエラーを処理します。 XAudio2 のコールバックについて詳しくは、「[XAudio2 のコールバック](/windows/desktop/xaudio2/xaudio2-callbacks)」をご覧ください。

## <a name="conclusion"></a>まとめ

ここでは、Marble Maze ゲーム サンプルについてまとめてみます。 このゲームは、比較的単純なゲームですが、すべての UWP DirectX ゲームに重要な部分の多くが含まれており、独自のゲームを作成するときに役立つ適切な例になっています。

これでチュートリアルは終了です。ソース コードをいろいろ手を加えて、どのような結果になるか確認してみてください。 または、「[DirectX によるシンプルな UWP ゲームの作成](tutorial--create-your-first-uwp-directx-game.md)」で別の UWP DirectX ゲームのサンプルを参照してください。

DirectX を使いこなす準備ができたら、 「[DirectX プログラミング](directx-programming.md)」でさまざまなガイドを確認してください。

UWP でのゲーム開発全般に関心がある場合は、「[ゲーム プログラミング](index.md)」のドキュメントを参照してください。

## <a name="related-topics"></a>関連トピック

* [Marble Maze サンプルへの入力と対話機能の追加](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Marble Maze、C++ と DirectX での UWP ゲームの開発](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)