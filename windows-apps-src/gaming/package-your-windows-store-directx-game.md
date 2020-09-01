---
title: ユニバーサル Windows プラットフォーム (UWP) DirectX ゲームのパッケージ化
description: 規模の大きいユニバーサル Windows プラットフォーム (UWP) ゲーム (特に、地域固有のアセットや機能オプションによる高解像度アセットを伴って複数言語をサポートするゲーム) は、サイズが容易に膨張する可能性があります。
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ゲーム, DirectX, パッケージ
ms.localizationpriority: medium
ms.openlocfilehash: 8a1e255a5219e39d866915bce25778dc8f1dfdba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175246"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>ユニバーサル Windows プラットフォーム (UWP) DirectX ゲームのパッケージ化

規模の大きいユニバーサル Windows プラットフォーム (UWP) ゲーム (特に、地域固有のアセットや機能オプションによる高解像度アセットを伴って複数言語をサポートするゲーム) は、サイズが容易に膨張する可能性があります。 このトピックでは、ユーザーが実際に必要なリソースのみを受け取ることができるように、アプリ パッケージとアプリ バンドルを使ってアプリをカスタマイズする方法について説明します。

アプリ パッケージ モデルに加え、Windows 10 では、2 種類のパッケージをグループ化するアプリ バンドルがサポートされます。

-   アプリ パッケージには、プラットフォーム固有の実行可能ファイルとライブラリが含まれます。 通常、UWP ゲームでは、それぞれ x86、x64、ARM の CPU アーキテクチャに対応する 3 つのアプリ パッケージを含めることができます。 そのハードウェア プラットフォームに固有のコードやデータはすべて、対応するアプリ パッケージに含める必要があります。 アプリ パッケージには、基本レベルの再現性とパフォーマンスでゲームを実行するための主要アセットもすべて含める必要があります。
-   リソース パッケージにはゲーム アセット (テクスチャ、メッシュ、サウンド、テキスト) など、プラットフォームにとらわれないオプション データまたは拡張データが含まれます。 UWP ゲームには、1 つまたは複数のリソース パッケージを含めることができます。これには、高解像度のアセットまたはテクスチャ、DirectX 機能レベル 11 以上のリソース、言語固有のアセットやリソースなどのリソース パッケージが含まれます。

アプリ バンドルとアプリ パッケージについて詳しくは、「[アプリ リソースの定義](/previous-versions/windows/apps/hh965321(v=win.10))」をご覧ください。

アプリ パッケージにすべてのコンテンツを含めることもできますが、冗長であり非効率的になります。 テクスチャ ファイルは、サイズが大きいリソースです。これを各プラットフォーム用に (特に、不要かもしれない ARM プラットフォーム用を含めて) 3 回もレプリケートする必要はありません。 目標は、ユーザーがダウンロードする対象のサイズをできる限り小さくすることです。これにより、ユーザーはゲームの開始を早め、デバイス上の領域を節約でき、場合によっては従量制の帯域幅コストを回避することができます。

UWP アプリ インストーラーに含まれるこの機能を使用するには、ツールとソースから適切な出力を行いシンプルなパッケージを作成できるように、ゲーム開発の初期段階からアプリとリソース パッケージのディレクトリ レイアウトとファイル命名規則を考慮することが重要になります。 アセットを作成または構成してツールとスクリプトを管理する場合や、リソースの読み込みまたは参照を行うコードを作成する場合は、このドキュメントに示されている規則に従ってください。

## <a name="why-create-resource-packs"></a>リソース パッケージを作成する理由


アプリを作成する場合、特に多くのロケールまたは多様な UWP ハードウェア プラットフォームで販売できるゲーム アプリを作成する場合は、これらのロケールやプラットフォームをサポートするために、多数のファイルの複数バージョンを含める必要が生じることが少なくありません。 たとえば、米国と日本の両方でゲームをリリースする場合、voice ファイルを en-us ロケール向けに英語で 1 セット、jp-jp ロケール向けに日本語で 1 セット用意する必要があります。 また、x86 および x64 プラットフォームと同様に ARM デバイスのゲームで画像を使う場合、同じ画像アセットを 3 回 (CPU アーキテクチャごとに 1 回ずつ) アップロードする必要があります。

これに加えて、低い DirectX 機能レベルに適用されない高解像度リソースを多数使うゲームの場合、基本アプリ パッケージにこれらを含めても、デバイスで使うことのできない大量のコンポーネントをダウンロードするようにユーザーに求めることになります。 このような高解像度リソースをオプションのリソース パッケージに分離すると、高解像度リソースがサポートされるデバイスを持っているユーザーは、その分の帯域幅を使って (場合によってはその分の料金を支払って) 必要なリソースを取得することができ、ハイエンド デバイスを持っていないユーザーは、低いネットワーク使用コストで迅速にゲームを入手できます。

ゲーム リソース パッケージの内容には、次のようなものが考えられます。

-   国際的なロケール固有アセット (ローカライズされたテキスト、オーディオ、または画像)
-   多様なデバイス倍率 (1.0x、1.4x、1.8x) に対応する高解像度アセット
-   高い DirectX 機能レベル (9、10、11) に対応する高解像度アセット

これらはすべて、UWP プロジェクトの一部である package.appxmanifest と、最終的なパッケージのディレクトリ構造で定義されます。 Visual Studio の新しい UI を使い、このドキュメントのプロセスに従うと、手動で編集する必要はありません。

> **重要**   これらのリソースの読み込みと管理は、Windows の**ApplicationModel. .Resources** api によって処理されます。 \* これらのアプリ モデル リソース API を使ってロケール、スケール ファクター、DirectX 機能レベルに応じた正しいファイルを読み込む場合、明示的なファイル パスを使ってアセットを読み込む必要はありません。リソース API に、必要なアセットの汎用的なファイル名を指定するだけで、ユーザーの現在のプラットフォームとロケール構成 (これらについても同じ API で指定できます) に応じた適切なバリアントのリソースが、リソース管理システムによって取得されます。

 

リソース パッケージのリソースは、次のいずれかの方法で指定します。

-   アセット ファイルに同じファイル名を使用し、リソース パッケージ固有の各バージョンを固有の名前のディレクトリに配置する。 これらのディレクトリ名は、システムによって予約されています。 たとえば、 \\ en-us、 \\ scale-140、 \\ dxfl-dx11 のようになります。
-   アセット ファイルは、任意の名前のフォルダーに格納されますが、ファイル名には共通ラベルが付けられ、言語や他の修飾子を示すためにシステムにより予約されている文字列が付加されます。 具体的には、修飾子文字列は、アンダースコア ("") の後に一般化されたファイル名に添付され \_ ます。 たとえば、assets \\ \\ メニュー \_ オプション 1 \_lang-en-us.png、 \\ assets \\ メニュー \_ オプション 1 \_scale-140.png、 \\ assets \\ coolsign dxfl-dx11 など \_ です。 これらの文字列を連結することもできます。 たとえば、[ \\ アセット \\ ] メニューは \_ \_ 140 オプション 1 \_lang-en-us.png になります。
    > **メモ**   ディレクトリ名で単独ではなくファイル名で使用する場合、言語修飾子は "lang-" の形式にする必要があり <tag> ます。詳細については、「[言語、スケール、およびその他の修飾子用にリソースを調整](../app-resources/tailor-resources-lang-scale-contrast.md)する」を参照してください。

     

ディレクトリ名は、リソース パッケージの追加特性に応じて組み合わせることができます。 ただし、冗長な名前を指定することはできません。 たとえば、 \\ en-us \\ メニュー \_ オプション 1 \_lang-en-us.png は冗長です。

各リソース ディレクトリの構造が同じであれば、リソース ディレクトリの下位には、予約名ではない必要なサブディレクトリ名を指定することができます。 たとえば、 \\ dxfl \\ assets \\ テクスチャ \\ coolsign です。 アセットを読み込むか参照するときは、フォルダー ノードやファイル名に含まれる言語、スケール、DirectX 機能レベルの修飾子をすべて削除して、パス名を汎用化する必要があります。 たとえば、いずれかのバリアントが dxfl-dx10 assets coolsign のアセットにコードを参照するには、 \\ \\ \\ \\ \\ assets \\ テクスチャ \\ coolsign を使用します。 同様に、バリアントイメージの背景scale-140.png を持つアセットを参照するには、 \\ \\ \_background.png 画像を使用し \\ \\ ます。

予約ディレクトリ名と、ファイル名にアンダースコアで連結されるサフィックスを次に示します。

| 資産の種類                   | リソース パッケージのディレクトリ名                                                                                                                  | リソース パッケージのファイル名サフィックス                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| ローカライズされたアセット             | Windows 10 で使用可能なすべての言語または言語とロケールの組み合わせ  (フォルダー名には、修飾子プレフィックス "lang-" は必要ありません)。 | " \_ " の後に、言語、ロケール、または言語ロケール指定子が続きます。 たとえば、それぞれ " \_ en"、" \_ us"、" \_ en-us" などです。 |
| 倍率アセット        | scale-100、scale-140、scale-180。 これらはそれぞれ、1.0x、1.4x、1.8x の UI 倍率を示します。                                     | " \_ " の後に "scale-100"、"scale-140"、または "scale-180" が続きます。                                                                    |
| DirectX 機能レベル アセット | dxfl-dx9、dxfl-dx10、dxfl-dx11。 これらはそれぞれ、DirectX の 9、10、11 機能レベルを示します。                                     | " \_ " の後に "て dx9"、"dx10"、または "dxfl-dx11" が続きます。                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>ローカライズされた言語リソース パッケージの定義


ロケール固有のファイルは、言語に応じた名前 ("en" など) のプロジェクト ディレクトリに配置します。

複数言語用にローカライズされたアセットをサポートできるようにアプリを構成するには、次のことを行う必要があります。

-   サポートする各言語とロケール (たとえば、en-us、jp-jp、zh-cn、fr-fr など) について、アプリのサブディレクトリ (またはファイル バージョン) を作成します。
-   開発中は、言語やロケールによる相違がなくても、すべてのアセット (ローカライズされたオーディオ ファイル、テクスチャ、メニュー用グラフィックスなど) のコピーを、対応する言語ロケールのサブディレクトリに配置します。 最善のユーザー エクスペリエンスを提供するには、ロケールに応じた言語リソース パッケージが存在するにもかかわらず取得されていない場合 (またはダウンロードしてインストールした後に誤って削除された場合) に、ユーザーへの警告が表示されるようにします。
-   各ディレクトリに配置するアセットまたは文字列のリソース ファイル (.resw) は、必ず同じ名前にします。 たとえば、 \_ \\ \\ ファイルの内容が別の言語のものであっても、メニューoption1.png は en-us ディレクトリと en-us ディレクトリの両方で同じ名前にする必要があります。 この例では、 \\ en-us \\ メニュー \_option1.png と \\ jp-jp メニューoption1.png と表示さ \\ れて \_ います。
    > **メモ**   必要に応じて、ロケールをファイル名に追加して、同じディレクトリに格納することもできます。たとえば、[ \\ アセット \\ ] メニュー \_ オプション 1 \_lang-en-us.png]、[ \\ アセット \\ ] メニュー \_ オプション 1 \_lang-jp-jp.png です。

     

-   ロケール固有のリソースをアプリ用に指定し、読み込むには、[**Windows.ApplicationModel.Resources**](/uwp/api/Windows.ApplicationModel.Resources) と [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) の API を使います。 また、これらの API は、ユーザーの設定に基づいて適切なロケールを判別し、ユーザーの適切なリソースを取得するため、特定のロケールが含まれないアセット参照を使ってください。
-   Microsoft Visual Studio 2015 では、**[プロジェクト]、[ストア]、[アプリ パッケージの作成]** の順に選択して、パッケージを作成します。

## <a name="defining-scaling-factor-resource-packs"></a>倍率リソース パッケージの定義


Windows 10 では、ユーザー インターフェイスの倍率として、1.0x、1.4x、1.8x の 3 種類が用意されています。 ディスプレイごとの倍率は、いくつかの要因 (画面のサイズ、画面の解像度、画面からユーザーまでの距離の推定値) の組み合わせに基づいてインストール時に値が設定されます。 ユーザーは、倍率を調整して読みやすくすることもできます。 最善のエクスペリエンスを提供するには、ゲームは DPI 対応と倍率認識に対応する必要があります。 この "認識" には、重要なビジュアル アセットについて、3 つの倍率にそれぞれ対応するバージョンを作成するという意味合いが含まれています。 また、ポインター操作とヒット テストも含まれています。

UWP アプリの各種の倍率に応じたリソース パッケージをサポートできるようにアプリを構成するには、次のことを行う必要があります。

-   サポートする各倍率 (scale-100、scale-140、scale-180) に対応するアプリ サブディレクトリ (またはファイル バージョン) を作成します。
-   開発中は、倍率による相違がなくても、倍率に応じたすべてのアセットのコピーを、各倍率用のリソース ディレクトリに配置します。
-   各ディレクトリに配置するアセットは、必ず同じ名前にします。 たとえば、メニューoption1.png は、 \_ \\ \\ ファイルの内容が異なる場合でも、スケール100とスケール180の両方のディレクトリに同じ名前を持つ必要があります。 この場合は、 \\ スケール-100 \\ メニュー \_option1.png と \\ スケール-140 メニューoption1.png として表示さ \\ \_ れます。
    > **メモ**   ここでも、必要に応じて、スケールファクターサフィックスをファイル名に追加して、同じディレクトリに格納することができます。たとえば、[ \\ アセット \\ ] メニュー \_ オプション 1 \_scale-100.png]、[ \\ アセット \\ ] メニュー \_ オプション 1 \_scale-140.png です。

     

-   アセットを読み込むには、[**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) の API を使います。 アセット参照は、特定のスケール バリエーションを除外して汎用化 (サフィックスなし) する必要があります。 システムは、ディスプレイの適切なスケール アセットとユーザーの設定を取得します。
-   Visual Studio 2015 では、**[プロジェクト]、[ストア]、[アプリ パッケージの作成]** の順に選択して、パッケージを作成します。

## <a name="defining-directx-feature-level-resource-packs"></a>DirectX 機能レベル リソース パッケージの定義


DirectX の機能レベルは、以前のバージョンと現在のバージョンの DirectX (Direct3D) の GPU 機能セットに対応しています。 これには、シェーダー モデルの仕様と機能、シェーダー言語サポート、テクスチャ圧縮サポート、全体的なグラフィックス パイプライン機能が含まれています。

基本アプリ パッケージでは、基本のテクスチャ圧縮形式 (BC1、BC2、または BC3) を使う必要があります。 これらの形式は、ローエンドの ARM プラットフォームから、専用のマルチ GPU ワークステーションやメディア コンピューターまで、任意の UWP デバイスで使うことができます。

ローカル ディスク領域とダウンロード帯域幅を節約するには、DirectX 機能レベル 10 以上のテクスチャ形式サポートをリソース パッケージに追加する必要があります。 これにより、BC6H や BC7 など、11 に対応する高度な圧縮方式の使用が有効になります  (詳しくは、「[Direct3D 11 でのテクスチャ ブロック圧縮](/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11)」をご覧ください)。これらの形式は最新の GPU でサポートされる高解像度のテクスチャ アセットに有効です。また、これらを使うと、ハイエンドのプラットフォーム上で、優れたゲームの外観、パフォーマンス、ディスク領域要件を実現できます。

| DirectX 機能レベル | サポートされるテクスチャ圧縮 |
|-----------------------|-------------------------------|
| 9                     | BC1、BC2、BC3                 |
| 10                    | BC4、BC5                      |
| 11                    | BC6H、BC7                     |

 

また、各 DirectX 機能レベルでは、別々のシェーダー モデル バージョンがサポートされています。 機能レベルごとにコンパイル済みのシェーダー リソースを作成し、DirectX 機能レベル リソース パッケージに含めることができます。 また、一部の新しいバージョンのシェーダー モデルでは、法線マップなど、以前のシェーダー モデル バージョンで使うことができなかったアセットを使うことができます。 これらのシェーダー モデル固有のアセットも、DirectX 機能レベル リソース パッケージに含める必要があります。

リソース メカニズムは、アセット用にサポートされるテクスチャ形式に焦点を合わせているため、3 つの全体機能レベルのみがサポート対象になります。 て DX9 1 vs て DX9 3 のように、サブレベル (ドットバージョン) に個別のシェーダーを使用する必要がある場合は \_ \_ 、アセット管理とレンダリングコードでそれらを明示的に処理する必要があります。

各種の DirectX 機能レベルに応じたリソース パッケージをサポートできるようにアプリを構成するには、次のことを行う必要があります。

-   サポートする各 DirectX 機能レベル (dxfl-dx9、dxfl-dx10、dxfl-dx11) に対応するアプリ サブディレクトリ (またはファイル バージョン) を作成します。
-   開発中は、機能レベル固有のアセットを、各機能レベル用のリソース ディレクトリに配置します。 ロケールや倍率の場合とは異なり、ゲーム内の各機能レベルに対してレンダリング コードの別々の分岐を割り当てることができます。サポートされるすべての機能レベルのうち、いずれか 1 つの機能レベルかサブセットのみで使うテクスチャ、コンパイル済みシェーダー、またはその他のアセットがある場合は、これらを使う機能レベルのディレクトリのみに、対応するアセットを配置します。 すべての機能レベルで読み込むアセットについては、各機能レベルのリソース ディレクトリに、同じ名前を持つ該当バージョンを必ず含めます。 たとえば、"coolsign" という特徴レベルに依存しないテクスチャの場合は、BC3 のバージョンを \\ dxfl-て dx9 ディレクトリに、BC7 圧縮バージョンを \\ dxfl-dx11 ディレクトリに配置します。
-   各ディレクトリに配置するアセット (複数の機能レベル用に存在する場合) は、必ず同じ名前にします。 たとえば、coolsign は、 \\ \\ ファイルの内容が異なる場合でも、て dx9 と dxfl-dx11 ディレクトリの両方で同じ名前を持つ必要があります。 この場合、それらは \\ dxfl-て dx9 \\ coolsign と \\ dxfl-dx11 coolsign として表示さ \\ れます。
    > **メモ**   ここでも、必要に応じて、機能レベルのサフィックスをファイル名に追加して、同じディレクトリに格納することができます。たとえば、 \\ テクスチャ \\ coolsign \_ dxfl-dx9、 \\ テクスチャ \\ coolsign \_ dxfl-dx11 です。

     

-   グラフィックス リソースを構成する際には、サポートされる DirectX 機能レベルを宣言します。
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   リソースを読み込むには、[**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) の API を使います。 アセット参照は、機能レベルを除外して汎用化 (サフィックスなし) する必要があります。 ただし、言語やスケールとは異なり、システムは特定のディスプレイに適した機能レベルを自動的に判別しません。コード ロジックに基づいてご自身で判断してください。 この決定を行ったら、API を使って希望する機能レベルを OS に通知します。 その後、システムはその設定に基づいて適切なアセットを取得できます。 次のコード サンプルでは、プラットフォームの現在の DirectX 機能レベルをアプリに通知する方法を示します。
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **メモ**   コード内で、名前 (または機能レベルのディレクトリの下にあるパス) を使用してテクスチャを直接読み込みます。 機能レベルのディレクトリ名またはサフィックスを含めないでください。 たとえば、" \\ dx11 \\ テクスチャ \\ coolsign" または "coolsign" ではなく、"" というロードを読み込み \\ \_ ます。

     

-   ここで、[**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) を使って、現在の DirectX 機能レベルに合致するファイルを見つけます。 **ResourceManager** は [**ResourceMap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) を返します。その [**ResourceMap::GetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue) (または [**ResourceMap::TryGetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) と渡された [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext) を使って照会します。 これにより、[**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue) を呼び出して指定された DirectX 機能レベルと最も近い [**ResourceCandidate**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate) が返されます。
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   Visual Studio 2015 では、**[プロジェクト]、[ストア]、[アプリ パッケージの作成]** の順に選択して、パッケージを作成します。
-   package.appxmanifest マニフェスト設定で、アプリ バンドルを必ず有効にしてください。

## <a name="related-topics"></a>関連トピック


* [アプリ リソースの定義](/previous-versions/windows/apps/hh965321(v=win.10))
* [アプリのパッケージ化](../packaging/index.md)
* [アプリケーションパッケージャー (MakeAppx.exe)](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 