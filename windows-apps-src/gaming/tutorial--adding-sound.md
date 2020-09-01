---
title: サウンドの追加
description: XAudio2 Api を使用してゲームの音楽とサウンド効果を再生する単純なサウンドエンジンを開発します。
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, サウンド
ms.localizationpriority: medium
ms.openlocfilehash: 04a9ea70914be3c60826df8753eca2ad1c30f19d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175186"
---
# <a name="add-sound"></a>サウンドの追加

> [!NOTE]
> このトピックは、「DirectX チュートリアルシリーズ [を含む simple ユニバーサル Windows プラットフォーム (UWP) ゲームの作成](tutorial--create-your-first-uwp-directx-game.md) 」に含まれています。 このリンクのトピックでは、系列のコンテキストを設定します。

このトピックでは、 [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) api を使用して単純なサウンドエンジンを作成します。 __XAudio2__を初めて使用する場合は、[オーディオの概念](#audio-concepts)に関する簡単な概要が記載されています。

>[!Note]
>このサンプルの最新のゲームコードをダウンロードしていない場合は、「 [Direct3D サンプルゲーム](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)」にアクセスしてください。 このサンプルは、UWP 機能のサンプルの大規模なコレクションの一部です。 サンプルをダウンロードする手順については、「[GitHub から UWP のサンプルを取得する](../get-started/get-app-samples.md)」をご覧ください。

## <a name="objective"></a>目的

[XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)を使用して、サンプルゲームにサウンドを追加します。

## <a name="define-the-audio-engine"></a>オーディオエンジンを定義する

サンプルゲームでは、オーディオオブジェクトとビヘイビアーは次の3つのファイルで定義されています。

* __/.Cpp [Audio.h](#audioh)__: サウンド再生用の__XAudio2__リソースを含む__オーディオ__オブジェクトを定義します。 また、ゲームが一時停止または非アクティブにされた場合にオーディオ再生を一時停止して再開するメソッドも定義します。
* __ [Mediareader .h](#mediareaderh)/.cpp__: ローカルストレージからオーディオ .wav ファイルを読み取るためのメソッドを定義します。
* __ [SoundEffect](#soundeffecth)/.cpp__: ゲーム内サウンド再生用のオブジェクトを定義します。

## <a name="overview"></a>概要

ゲームへのオーディオ再生の設定には、主に3つの部分があります。

1. [オーディオリソースを作成して初期化する](#create-and-initialize-the-audio-resources)
2. [オーディオファイルの読み込み](#load-audio-file)
3. [サウンドをオブジェクトに関連付ける](#associate-sound-to-object)

これらはすべて、 [Simple3DGame:: Initialize](#simple3dgameinitialize-method) メソッドで定義されています。 まず、このメソッドを調べて、各セクションの詳細について説明します。

設定後、サウンド効果をトリガーして再生する方法について説明します。 詳細については、「 [サウンドを再生](#play-the-sound)する」を参照してください。

### <a name="simple3dgameinitialize-method"></a>Simple3DGame:: Initialize メソッド

__Simple3DGame:: Initialize__では、 __m \_ コントローラー__と__m \_ レンダラー__も初期化されます。オーディオエンジンを設定し、サウンドを再生できるようにします。

 * [オーディオ](#audioh)クラスのインスタンスである__m \_ audiocontroller__を作成します。
 * [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method)メソッドを使用して必要なオーディオリソースを作成します。 ここでは、2つの __XAudio2__ オブジェクトに &mdash; ミュージックエンジンオブジェクトとサウンドエンジンオブジェクトを作成し、それぞれに対してマスタリング音声を作成しました。 ミュージックエンジンオブジェクトを使用して、ゲームの背景音楽を再生できます。 サウンドエンジンを使用して、ゲームのサウンド効果を再生できます。 詳細については、「 [オーディオリソースの作成と初期化](#create-and-initialize-the-audio-resources)」を参照してください。
 * Mediareader[クラスの](#mediareaderh)インスタンスである__mediareader__を作成します。 [SoundEffect](#soundeffecth)クラスのヘルパークラスである[mediareader](#mediareaderh)は、ファイルの場所から同期的に小さいオーディオファイルを読み取り、音声データをバイト配列として返します。
 * [Mediareader:: LoadMedia](#mediareaderloadmedia-method)を使用して、サウンドファイルの場所からサウンドファイルを読み込み、読み込まれた .wav サウンドデータを保持するために__targethitsound__変数を作成します。 詳細については、「 [オーディオファイルの読み込み](#load-audio-file)」を参照してください。 

サウンド効果は、game オブジェクトに関連付けられています。 そのため、その game オブジェクトで競合が発生すると、再生されるサウンド効果がトリガーされます。 このサンプルゲームでは、ammo (ターゲットのの撮影に使用されるもの) とターゲットのサウンド効果があります。 
    
* このオブジェクト __クラスには、__ サウンド効果をオブジェクトに関連付けるために使用される __ヒット音__ のプロパティがあります。
* [SoundEffect](#soundeffecth)クラスの新しいインスタンスを作成し、初期化します。 初期化中に、サウンド効果のソース音声が作成されます。 
* このクラスは、 [Audio](#audioh) クラスによって提供されるマスタリング音声を使用してサウンドを再生します。 サウンドデータは、 [Mediareader](#mediareaderh) クラスを使用してファイルの場所から読み取られます。 詳細については、「 [オブジェクトへのサウンドの関連付け](#associate-sound-to-object)」を参照してください。

>[!Note]
>サウンドを再生する実際のトリガーは、これらのゲームオブジェクトの動きと衝突によって決まります。 そのため、これらのサウンドを実際に再生する呼び出しは、 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) メソッドで定義されています。 詳細については、「 [サウンドを再生](#play-the-sound)する」を参照してください。

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>オーディオリソースを作成して初期化する

* [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create)(XAudio2 API) を使用して、音楽とサウンド効果エンジンを定義する2つの新しい XAudio2 オブジェクトを作成します。 このメソッドは、すべてのオーディオエンジンの状態、オーディオ処理スレッド、音声グラフなどを管理するオブジェクトの [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) インターフェイスへのポインターを返します。
* エンジンがインスタンス化されたら、 [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) を使用して、各サウンドエンジンオブジェクトのマスター音声を作成します。

詳細については、「 [How to: Initialize XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)」を参照してください。

### <a name="audiocreatedeviceindependentresources-method"></a>Audio:: CreateDeviceIndependentResources メソッド

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>オーディオファイルの読み込み

サンプルゲームでは、オーディオフォーマットファイルを読み取るためのコードは、 [Mediareader .h](#mediareaderh)/cpp__ で定義されています。  エンコードされた .wav オーディオファイルを読み取るには、 [Mediareader:: LoadMedia](#mediareaderloadmedia-method)を呼び出して、入力パラメーターとして .wav のファイル名を渡します。

### <a name="mediareaderloadmedia-method"></a>MediaReader:: LoadMedia メソッド

このメソッドは、[メディア ファンデーション](/windows/desktop/medfound/microsoft-media-foundation-sdk) API を使って、.wav オーディオ ファイルをパルス符号変調 (PCM) バッファーとして読み取ります。

#### <a name="set-up-the-source-reader"></a>ソースリーダーを設定する

1. [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl)を使用して、メディアソースリーダー ([IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)) を作成します。
2. [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype)を使用して、メディアの種類 ([imfmediatype](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) オブジェクト (_mediaType_) を作成します。 メディア形式の説明を表します。 
3. _MediaType_のデコードされた出力が、 __XAudio2__が使用できるオーディオの種類である PCM audio であることを指定します。
4. [IMFSourceReader:: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype)を呼び出すことによって、ソースリーダーに対してデコードされた出力メディアの種類を設定します。

ソースリーダーを使用する理由の詳細については、「 [ソースリーダー](/windows/desktop/medfound/source-reader)」を参照してください。

#### <a name="describe-the-data-format-of-the-audio-stream"></a>オーディオストリームのデータ形式の記述

1. [IMFSourceReader:: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype)を使用して、ストリームの現在のメディアの種類を取得します。
2. [Imfmediatype:: MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype)を使用して、以前の操作の結果を入力として使用して、現在のオーディオメディアの種類を[WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex)バッファーに変換します。 この構造体は、オーディオが読み込まれた後に使用される wave オーディオストリームのデータ形式を指定します。 

__WAVEFORMATEX__形式は、PCM バッファーの記述に使用できます。 [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible)構造体と比較して、オーディオ wave 形式のサブセットの記述にのみ使用できます。 __WAVEFORMATEX__と__WAVEFORMATEXTENSIBLE__の違いの詳細については、「[拡張 Wave 形式の記述子](/windows-hardware/drivers/audio/extensible-wave-format-descriptors)」を参照してください。

#### <a name="read-the-audio-stream"></a>オーディオストリームの読み取り

1.  [IMFSourceReader:: Getプレゼンテーション属性](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute)を呼び出して、オーディオストリームの期間 (秒単位) を取得し、その期間をバイトに変換します。
2.  [IMFSourceReader:: ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample)を呼び出して、のオーディオファイルをストリームとして読み取ります。 __Readsample__ は、メディアソースから次のサンプルを読み取ります。
3.  [Imfsample:: ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer)を使用して、オーディオサンプルバッファー (_サンプル_) の内容を配列 (_mediaBuffer_) にコピーします。

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>サウンドをオブジェクトに関連付ける

[Simple3DGame:: Initialize](#simple3dgameinitialize-method)メソッドでは、ゲームの初期化時に、オブジェクトへのサウンドの関連付けが行われます。

要約: 
* このオブジェクト __クラスには、__ サウンド効果をオブジェクトに関連付けるために使用される __ヒット音__ のプロパティがあります。
* [SoundEffect](#soundeffecth) class オブジェクトの新しいインスタンスを作成し、game オブジェクトに関連付けます。 このクラスは、 __XAudio2__ api を使用してサウンドを再生します。  [オーディオ](#audioh)クラスによって提供されるマスタリング音声を使用します。 サウンドデータは、 [Mediareader](#mediareaderh) クラスを使用してファイルの場所から読み取ることができます。

[SoundEffect:: Initialize](#soundeffectinitialize-method) は、次の入力パラメーターで __SoundEffect__ インスタンスを初期化するために使用します。サウンドエンジンオブジェクトへのポインター ( [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) メソッドで作成された IXAudio2 オブジェクト)、 __mediareader:: GetOutputWaveFormatEx__を使用して .wav ファイルの形式へのポインター、および [mediareader:: loadmedia](#mediareaderloadmedia-method) メソッドを使用して読み込まれたサウンドデータ。 初期化中に、サウンド効果のソース音声も作成されます。

### <a name="soundeffectinitialize-method"></a>SoundEffect:: Initialize メソッド

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>サウンドを再生する

サウンド効果を再生するトリガーは、 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) メソッドで定義されています。これは、オブジェクトの移動が更新され、オブジェクト間の競合が特定されるためです。

オブジェクト間の相互作用は大きく異なるため、ゲームによってはゲームオブジェクトのダイナミクスについては説明しません。 実装について理解を深めたい場合は、 [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) メソッドにアクセスしてください。

原則として、競合が発生すると、 **SoundEffect::P**を呼び出すことによってサウンド効果が再生されるようになります。 このメソッドは、現在再生中のすべてのサウンド効果を停止し、必要なサウンドデータを使用してメモリ内バッファーをキューに格納します。 ソース音声を使用してボリュームを設定し、サウンドデータを送信して、再生を開始します。

### <a name="soundeffectplaysound-method"></a>SoundEffect::P $ Sound メソッド

* ソース音声オブジェクト**m \_ sourceVoice**を使用して、サウンドデータバッファー **m \_ sounddata**の再生を開始します
* サウンドデータバッファーへの参照を提供する [XAUDIO2 \_ バッファー](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer)を作成し、 [IXAudio2SourceVoice:: submitsourcebuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer)への呼び出しを使用して送信します。 
* サウンド データがキューに入ると、**SoundEffect::PlaySound** は、[IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) を呼び出して再生を開始します。

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame:: UpdateDynamics メソッド

__Simple3DGame:: UpdateDynamics__メソッドは、ゲームオブジェクト間の相互作用と競合を処理します。 オブジェクトが衝突する (または交差する) と、関連付けられたサウンド効果が再生されます。

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
        // Start playing the sounds for the impact between the two balls.
        m_ammo[one]->PlaySound(impact, m_player->Position());
        m_ammo[two]->PlaySound(impact, m_player->Position());
    }
}
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.

    if (impact > 0.0f)
    {
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>次の手順

Windows 10 ゲームの UWP フレームワーク、グラフィックス、コントロール、ユーザーインターフェイス、オーディオについて説明しました。 このチュートリアルの次のパート「 [サンプルゲームの拡張](tutorial-resources.md)」では、ゲーム開発時に使用できるその他のオプションについて説明します。

## <a name="audio-concepts"></a>オーディオの概念

Windows 10 ゲーム開発の場合は、XAudio2 バージョン2.9 を使用します。 このバージョンは、Windows 10 に付属しています。 詳細については、 [XAudio2 のバージョン](/windows/desktop/xaudio2/xaudio2-versions)を参照してください。

__AudioX2__ は、信号処理と混合の基盤を提供する低レベルの API です。 詳細については、「 [XAudio2 Key 概念](/windows/desktop/xaudio2/xaudio2-key-concepts)」を参照してください。

### <a name="xaudio2-voices"></a>XAudio2 の声

XAudio2 voice オブジェクトには、ソース、サブミックス、マスタリングの3種類があります。 音声は、オーディオデータを処理、操作、および再生するために使用する XAudio2 オブジェクトです。 
* ソース ボイスは、クライアントから提供されたオーディオ データに適用されます。 
* ソース ボイスとサブミックス ボイスは、1 つ以上のサブミックス ボイスまたはマスタリング ボイスに向けて出力を送信します。 
* サブミックス ボイスとマスタリング ボイスは、それぞれに送られるすべてのボイスからオーディオをミキシングし、その結果に対して作用します。 
* マスタリングの音声は、ソースの音声とサブミックスの音声からデータを受け取り、そのデータをオーディオハードウェアに送信します。

詳細については、 [XAudio2 の音声](/windows/desktop/xaudio2/xaudio2-voices)に関するページを参照してください。

### <a name="audio-graph"></a>オーディオグラフ

Audio graph は、XAudio2 の [音声](/windows/desktop/xaudio2/xaudio2-voices)のコレクションです。 オーディオは、ソース音声でオーディオグラフの片側から開始され、必要に応じて1つ以上のサブミックス音声を通過し、マスタリング音声で終了します。 オーディオグラフには、現在再生中の各サウンド、0個以上のサブミックス音声、およびマスタリング音声が1つずつ含まれます。 最も単純なオーディオグラフと、XAudio2 でノイズを作成するために必要な最小値は、単一のソース音声によってマスタリング音声に直接出力されます。 詳細については、 [オーディオグラフ](/windows/desktop/xaudio2/audio-graphs)に関するページを参照してください。

### <a name="additional-reading"></a>その他の情報

* [方法: XAudio2 の初期化](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [方法: XAudio2 でのオーディオ データ ファイルの読み込み](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [方法: XAudio2 でのサウンド再生](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>キーのオーディオ .h ファイル

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```