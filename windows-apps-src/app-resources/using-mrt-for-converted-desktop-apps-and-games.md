---
title: 変換されたデスクトップ アプリとゲームに MRT を使用する
description: .NET または Win32 アプリやゲームを、msix または .appx パッケージとしてパッケージ化することにより、リソース管理システムを活用して、実行時コンテキストに合わせて調整されたアプリリソースを読み込むことができます。 この詳細なトピックでは、この手法について説明します。
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10、UWP、MRT、PRI。 リソース、ゲーム、Centennial、Desktop App Converter、MUI、サテライト アセンブリ
ms.localizationpriority: medium
ms.openlocfilehash: dafce15fa259fdbc8a0afab90b6617dc6cc37cf4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157616"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>レガシ アプリやゲームで Windows 10 のリソース管理システムを使用する

.NET アプリや Win32 アプリは多くの場合、対象市場を拡大するため、さまざまな言語にローカライズされます。 アプリのローカライズの価値提案の詳細については、「[グローバリゼーションとローカライズ](../design/globalizing/globalizing-portal.md)」をご覧ください。 .NET または Win32 アプリやゲームを、msix または .appx パッケージとしてパッケージ化することにより、リソース管理システムを活用して、実行時コンテキストに合わせて調整されたアプリリソースを読み込むことができます。 この詳細なトピックでは、この手法について説明します。

従来の Win32 アプリケーションをローカライズする方法はたくさんありますが、Windows 8 では[新しいリソース管理システム](/previous-versions/windows/apps/jj552947(v=win.10))が導入されました。このリソース管理システムは、さまざまなプログラミング言語やさまざまな種類のアプリケーションで動作し、ローカライズ機能を簡素化するだけでなく、さまざまな機能を提供します。 このトピックでは、このシステムを "MRT" と呼びます。 「モダン」という用語の使用は停止されましたが、MRT は従来 "Modern Resource Technology" を表していました。 リソース マネージャーは、MRM (モダン リソース マネージャー) または PRI (パッケージ リソース インデックス) としても知られています。

MSIX ベースまたは .appx ベースの展開 (たとえば、Microsoft Store) と組み合わせると、MRT.DLL は、アプリケーションのダウンロードとインストールのサイズを最小限に抑えるために、特定のユーザー/デバイスに適用可能なリソースを自動的に提供できます。 ローカライズ コンテンツのサイズが大きなアプリケーションでは、これによって大きなサイズ削減効果があり、高度なゲームの場合では、数*ギガバイト*にも及ぶ削減効果となることがあります。 さらに、Windows シェルと Microsoft Store でローカライズされて表示されることや、ユーザーの使用言語と利用可能なリソースが一致しない場合の自動フォールバック ロジックなども、MRT によるメリットの例です。

このドキュメントでは、MRT のアーキテクチャの概要を説明し、レガシの Win32 アプリケーションを最小限のコード変更で MRT に移行するためのガイドを示します。 MRT への移行により、開発者にはさまざまなメリット (スケール ファクターやシステム テーマを使ったリソースのセグメント化など) があります。 MRT ベースのローカライズは、UWP アプリケーションと、デスクトップ ブリッジ ("Centennial") によって処理される Win32 アプリケーションの両方で動作します。

多くの場合、既存のローカライズ形式とソース コードを引き続き使用しながら、MRT との統合を行い、実行時にリソースを解決して、ダウンロード サイズを最小化することができます。これはオールオアナッシングのアプローチではありません。 各段階での作業とメリット、推定コストを次の表にまとめて示します。 この表には、高解像度またはハイ コントラストのアプリケーション アイコンの提供などの、ローカライズ以外のタスクは含まれていません。 タイル、アイコンなどへの複数のアセットの提供について詳しくは、「[言語、スケール、ハイ コントラスト、その他の修飾子用にリソースを調整する](tailor-resources-lang-scale-contrast.md)」をご覧ください。

<table>
<tr>
<th>作業</th>
<th>特長</th>
<th>推定コスト</th>
</tr>
<tr>
<td>パッケージマニフェストのローカライズ</td>
<td>Windows シェルと Microsoft Store でローカライズ コンテンツが表示されるために必要な最小限の作業</td>
<td>小</td>
</tr>
<tr>
<td>MRT を使ってリソースを識別して検索する</td>
<td>ダウンロードとインストールのサイズの最小化や、言語の自動フォールバックの前提条件</td>
<td>Medium</td>
</tr>
<tr>
<td>リソース パッケージをビルドする</td>
<td>ダウンロードとインストールのサイズを最小化するための最後の手順</td>
<td>小</td>
</tr>
<tr>
<td>MRT リソース形式と API へ移行する</td>
<td>大幅に小さなファイル サイズ (既存のリソース テクノロジによる)</td>
<td>大</td>
</tr>
</table>

## <a name="introduction"></a>はじめに

多くのアプリケーションには通常、アプリケーション コードから分離された、*リソース*と呼ばれるユーザー インターフェイス要素が含まれています (一方、値を*ハードコード*する場合は、ソース コード自体に記述されます)。 ハードコードしないで、リソースを使用することが好ましい理由はいろいろあります。たとえば、開発者以外でも編集が容易であることもその 1 つです。最も重要なメリットの 1 つは、アプリケーションが実行時に、同じ論理リソースの異なる表現を選択できることです。 たとえば、ボタンに表示するテキスト (またはアイコンに表示するイメージ) が、ユーザーの使用言語や、表示デバイスの種類、使用している支援技術などによって、異なる場合があります。

したがって、リソース管理テクノロジの主な目的は、実行時に、論理またはシンボリックな*リソース名* (`SAVE_BUTTON_LABEL`など) の要求を、一連の*候補* (たとえば、「Save」、「Speichern」、「保存」) の中から、実際に最適な*値* (たとえば「保存」) に変換することです。 MRT はそのための機能を提供し、アプリケーションは、*修飾子*と呼ばれる、ユーザーの言語、ディスプレイのスケール ファクター、ユーザーが選択したテーマ、その他の環境の要因などの、さまざまな属性を使って、リソースの候補を識別することができます。 さらに MRT は、アプリケーションが必要とする、カスタムの修飾子をサポートします (たとえば、アプリケーションが、ゲスト ユーザーとアカウントを使ってログインしたユーザーに対して、別のグラフィック アセットを提供することができます。これを、アプリケーションのあらゆる部分にチェックのロジックを追加することなく、行えます)。 MRT は、文字列リソースとファイル ベースのリソース (外部データ、つまりファイル自体への参照として実装されている場合) の両方で動作します。

### <a name="example"></a>例

2 つのボタン (`openButton` と `saveButton`) 上のテキスト ラベルと、ロゴ (`logoImage`) に使用する PNG ファイルを持つアプリケーションでの、簡単な例を次に示します。 テキスト ラベルは英語とドイツ語にローカライズされ、ロゴは通常のデスクトップ (100% スケール ファクター) と高解像度の電話 (300% スケール ファクター) 用に最適化されています。 この図は、モデルの概念と概要を説明するためのものであり、実装に正確に対応するものではないことにご注意ください。

<p><img src="images\conceptual-resource-model.png"/></p>

この図で、アプリケーション コードは 3 つの論理リソース名を参照しています。 実行時に、擬似関数 `GetResource` は、MRT を使用して、リソース テーブル (PRI ファイルと呼ばれます) で、そのリソース名を検索し、環境条件 (ユーザーの言語とディスプレイのスケール ファクター) に基づいて、最適な候補を見つけます。 ラベルの場合は、文字列が直接使用されます。 ロゴ イメージの場合は、文字列がファイル名として解釈され、ファイルがディスクから読み取られます。 

ユーザーが英語またはドイツ語以外の言語を話す場合、または100% または300% 以外の表示スケールファクターを使用している場合、MRT.DLL はフォールバック規則のセットに基づいて "最も近い" 一致候補を選択します (詳細については、「 [リソース管理システム](/previous-versions/windows/apps/jj552947(v=win.10)) 」を参照してください)。

MRT は複数の修飾子に合わせてカスタマイズされたリソースをサポートします。たとえば、ロゴ イメージに埋め込みのテキストが含まれ、そのテキストをローカライズする必要がある場合には、そのロゴは 4 つの候補 (EN/スケール-100、DE/スケール-100、EN/スケール-300、DE/スケール-300) を持つことができます。

### <a name="sections-in-this-document"></a>このドキュメントのセクション

以下のセクションでは、アプリケーションを MRT と統合するために必要なタスクの概要を説明します。

#### <a name="phase-0-build-an-application-package"></a>フェーズ 0: アプリケーション パッケージをビルドする

このセクションでは、既存のデスクトップ アプリケーションを、アプリケーション パッケージとしてビルドする方法について説明します。 この段階では、MRT の機能は使用しません。

#### <a name="phase-1-localize-the-application-manifest"></a>フェーズ 1: アプリケーション マニフェストをローカライズする

このセクションでは、アプリケーションのマニフェストをローカライズする (これにより Windows Shell に正しく表示されるようになります) 方法について説明します。この段階では、引き続き、レガシのリソース形式と API を使用して、リソースのパッケージ化と検索を行っています。 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>フェーズ 2: MRT を使用してリソースを識別して検索する

このセクションでは、アプリケーション コード (および場合によってリソース レイアウト) を変更し、MRT を使用してリソースを検索する方法について説明します。この段階では、引き続き、既存のリソース形式と API を使用して、リソースの読み込みと使用を行っています。 

#### <a name="phase-3-build-resource-packs"></a>フェーズ 3: リソース パッケージをビルドする

このセクションでは、リソースを別の*リソース パッケージ*に分離して、アプリのダウンロード (およびインストール) のサイズを最小化するために必要な、最終的な変更について説明します。

### <a name="not-covered-in-this-document"></a>このドキュメントで扱わない内容

上記のフェーズ0-3 を完了すると、Microsoft Store に送信できるアプリケーション "バンドル" が作成されます。これにより、必要のないリソース (たとえば、読み上げない言語) を省略して、ユーザーのダウンロードとインストールのサイズを最小限に抑えることができます。 次の最後の手順を実行すると、アプリケーションのサイズと機能をさらに向上することができます。

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>フェーズ 4: MRT リソース形式と API に移行する

このフェーズは、このドキュメントの対象範囲外です。このフェーズでは、MUI DLL や .NET リソース アセンブリなどの、レガシのリソース形式から、PRI ファイルに、リソース (特に文字列) を移行します。 これにより、ダウンロードとインストールのサイズをさらに節約できます。 さらに、MRT の他の機能を活用でき、たとえば、スケール ファクターや、アクセシビリティ設定などに基づいて、イメージ ファイルのダウンロードとインストールを最小化できます。

## <a name="phase-0-build-an-application-package"></a>フェーズ 0: アプリケーション パッケージをビルドする

アプリケーションのリソースへの変更を行う前にまず、現在使っているパッケージ化とインストールのテクノロジを、UWP の標準のパッケージ化と展開のテクノロジに置き換える必要があります。 これには 3 つの方法があります。

* 複雑なインストーラーを使用している大規模なデスクトップアプリケーションがある場合、または多くの OS 機能拡張ポイントを利用している場合は、Desktop App Converter ツールを使用して、既存のアプリのインストーラー (MSI など) から UWP ファイルのレイアウトとマニフェスト情報を生成できます。
* 比較的少ないファイルまたは単純なインストーラーと、機能拡張フックのない小さいデスクトップアプリケーションがある場合は、ファイルレイアウトとマニフェストの情報を手動で作成できます。
* ソースから再構築し、純粋な UWP アプリケーションとしてアプリを更新する場合は、Visual Studio で新しいプロジェクトを作成し、IDE を利用して作業の多くを行うことができます。

[デスクトップアプリコンバーター](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw)を使用する場合は、変換プロセスの詳細について、「[デスクトップアプリコンバーターを使用してデスクトップアプリケーションをパッケージ化](/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter)する」を参照してください。 Desktop Converter のサンプルの完全なセットは、[GitHub リポジトリの Desktop Bridge to UWP サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)にあります。

パッケージを手動で作成する場合は、すべてのアプリケーションのファイル (実行可能ファイルとコンテンツ) とパッケージマニフェストファイル (package.appxmanifest) を含むディレクトリ構造を作成する必要があります。 例は [Hello, World GitHub サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)にありますが、というデスクトップ実行可能ファイルを実行する基本的なパッケージマニフェストファイル `ContosoDemo.exe` は次のようになります。 <span style="background-color: yellow">強調表示</span> されたテキストは、独自の値に置き換えられます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

パッケージマニフェストファイルとパッケージレイアウトの詳細については、「 [アプリケーションパッケージマニフェスト](/uwp/schemas/appxpackage/appx-package-manifest)」を参照してください。

最後に、Visual Studio を使用して新しいプロジェクトを作成し、既存のコードを移行する場合は、「 [Hello, world "アプリを作成](../get-started/create-a-hello-world-app-xaml-universal.md)する」を参照してください。 新しいプロジェクトには既存のコードを含めることができますが、純粋な UWP アプリとして実行するには (特にユーザーインターフェイスで) コードを大幅に変更することが必要になる場合があります。 それらの変更は、このドキュメントの対象範囲外です。

## <a name="phase-1-localize-the-manifest"></a>フェーズ 1: マニフェストをローカライズする

### <a name="step-11-update-strings--assets-in-the-manifest"></a>手順 1.1: マニフェスト内の資産 & 文字列を更新する

フェーズ0では、アプリケーションの基本的なパッケージマニフェスト (. package.appxmanifest) ファイルを作成しました。このファイルは、コンバーターに指定された値に基づいて、MSI から抽出されるか、手動でマニフェストに入力されますが、ローカライズされた情報は含まれません。また、高解像度のスタートタイル資産などの追加機能もサポートし

アプリケーションの名前と説明が正しくローカライズされていることを確認するには、リソースファイルのセットでいくつかのリソースを定義し、それらを参照するようにパッケージマニフェストを更新する必要があります。

#### <a name="creating-a-default-resource-file"></a>既定のリソース ファイルを作成する

最初の手順では、既定の言語 (たとえば、英語 (米国)) で既定のリソース ファイルを作成します。 これは、テキスト エディターを使って手動で作成するか、または Visual Studio のリソース デザイナーによって行うことができます。

リソースを手動で作成する場合には:

1. `resources.resw` という XML ファイルを作成して、プロジェクトの `Strings\en-us` サブフォルダーに配置します。 既定の言語が米国英語以外の場合は、適切な BCP-47 コードを使用します。
2. XML ファイルに次のコンテンツを追加します。使用する既定の言語で、<span style="background-color: yellow">強調表示されたテキスト</span>を、アプリのために適切なテキストに置き換えます。

> [!NOTE]
> これらの文字列の長さには制限があります。 詳しくは、「[VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)」をご覧ください。

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Visual Studio でデザイナーを使用する場合は:

1. `Strings\en-us`プロジェクトにフォルダー (またはその他の言語) を作成し、既定の名前を使用して、プロジェクトのルートフォルダーに**新しい項目**を追加し `resources.resw` ます。 リソース**ディクショナリではなくリソース****ファイル (resw)** を選択してください。リソースディクショナリは、XAML アプリケーションによって使用されるファイルです。
2. デザイナーを使って次の文字列を入力します (同じ `Names` を使用し、`Values` をアプリケーションのために適切なテキストに置き換えます)。

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Visual Studio デザイナーから始める場合は、を押して、いつでも XML を直接編集でき `F7` ます。 ただし、最小限の XML ファイルで開始する場合、さまざまな追加のメタデータが含まれていないため、*デザイナーは、ファイルを認識しません*。手動で編集する XML ファイルに、デザイナーが生成したファイルからスケルトンの XSD 情報をコピーすると、この問題を解決できます。

#### <a name="update-the-manifest-to-reference-the-resources"></a>マニフェストを更新してリソースを参照する

ファイルで値を定義した後 `.resw` 、次の手順では、リソース文字列を参照するようにマニフェストを更新します。 ここでも、XML ファイルを直接編集するか、Visual Studio のマニフェスト デザイナーを利用できます。

XML を直接編集する場合は、`AppxManifest.xml` ファイルを開き、<span style="background-color: lightgreen">強調表示された値</span>に次の変更を行います。これは、アプリケーション固有ではなく、このテキストを*そのまま*使用します。 これらの正確なリソース名を使用する必要はありません &mdash; が、独自に選択でき &mdash; ますが、ファイル内の内容と正確に一致している必要があり `.resw` ます。 これらの名前は `.resw` ファイルで作成した `Names` と一致し、`ms-resource:` スキームと `Resources/` 名前空間のプレフィックスが付く必要があります。 

> [!NOTE]
> マニフェストの多くの要素は、このスニペットから除外されています。何も削除しないでください。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Visual Studio マニフェストデザイナーを使用している場合は、package.appxmanifest ファイルを開き、 <span style="background-color: lightgreen">強調表示</span> されている値を * [*アプリケーション* ] タブと [ *パッケージ化* ] タブで変更します。

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>手順 1.2: PRI ファイルをビルドし、MSIX パッケージを作成して、動作していることを確認する

`.pri` ファイルをビルドし、アプリケーションを展開して、(既定の言語で) 正しい情報がスタート メニューに表示されることを確認します。

Visual Studio でビルドを行う場合は、`Ctrl+Shift+B` を押すとプロジェクトをビルドできます。次にプロジェクトを右クリックして、コンテキスト メニューから `Deploy` を選択します。

手動でビルドする場合は、次の手順に従ってツールの構成ファイルを作成 `MakePRI` し、ファイル自体を生成し `.pri` ます (詳細については、「 [アプリの手動パッケージング](/windows/msix/package/manual-packaging-root)」を参照してください)。

1. [スタート] メニューの **Visual studio 2017** または **visual studio 2019** フォルダーから、開発者コマンドプロンプトを開きます。
2. プロジェクトルートディレクトリ (package.appxmanifest ファイルと **Strings** フォルダーを含むディレクトリ) に切り替えます。
3. "Contoso_demo.xml" をプロジェクトに適した名前に置き換え、"en-US" をアプリの既定の言語に置き換えて (または必要に応じて "en-US" のままとして)、次のコマンドを入力します。 XML ファイルは、(プロジェクトディレクトリでは**なく** ) 親ディレクトリに作成されます。これは、アプリケーションの一部ではないためです (任意の他のディレクトリを選択できますが、今後のコマンドでは必ず置き換えてください)。

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    「`makepri createconfig /?`」と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです。
      * `/cf` 構成ファイル名 (このコマンドの出力) を設定します。
      * `/dq` 既定の修飾子 (この場合は言語) を設定します。 `en-US`
      * `/pv` プラットフォームのバージョン (この場合は Windows 10) を設定します
      * `/o` 出力ファイルが存在する場合に上書きするように設定します

4. 構成ファイルが作成されました。`MakePRI` を再度実行して、ディスクにあるリソースを検索し、それを PRI ファイルにパッケージ化します。 "Contoso_demop.xml" を、前の手順で使った XML ファイル名に置き換えます。入力と出力の両方に、必ず親ディレクトリを指定します。 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    `makepri new /?` と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです:
      * `/pr` プロジェクトルート (この場合は現在のディレクトリ) を設定します。
      * `/cf` 前の手順で作成した構成ファイル名を設定します。
      * `/of` 出力ファイルを設定します。 
      * `/mf` マッピングファイルを作成します (これにより、後の手順でパッケージ内のファイルを除外できます)。
      * `/o` 出力ファイルが存在する場合に上書きするように設定します

5. 既定の言語リソース (たとえば en-US) を持つ `.pri` が作成されました。 次のコマンドを実行して、正しく動作することを確認します。

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    `makepri dump /?` と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです:
      * `/if` 入力ファイル名を設定します。 
      * `/of` 出力ファイル名を設定します ( `.xml` 自動的に追加されます)
      * `/o` 出力ファイルが存在する場合に上書きするように設定します

6. 最後に、`..\resources.xml` をテキスト エディターで開き、`<NamedResource>` の値 (`ApplicationDescription` や `PublisherDisplayName` など) が含まれること、さらに選択した既定の言語の `<Candidate>` が含まれることを確認します (ファイルの先頭には、その他のコンテンツがありますが、ここでは無視してください)。

マッピングファイルを開いて、 `..\resources.map.txt` プロジェクトに必要なファイルが含まれていることを確認できます (プロジェクトのディレクトリに含まれていない PRI ファイルを含む)。 マッピング ファイルには、`resources.resw` ファイルへの参照は含まれて*いません*。これは重要です。そのファイルの内容は既に PRI ファイルに埋め込まれているためです。 ただし、イメージのファイル名などの他のリソースは含まれています。

#### <a name="building-and-signing-the-package"></a>パッケージをビルドして署名する 

PRI ファイルがビルドされました。次はパッケージをビルドして署名します。

1. アプリケーションパッケージを作成するには、次のコマンドを実行し `contoso_demo.appx` ます。は、作成する msix/.appx ファイルの名前に置き換え、ファイル用に別のディレクトリを選択します (このサンプルでは親ディレクトリを使用しますが、プロジェクトディレクトリにすることはでき **ません** )。

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    `makeappx pack /?` と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです:
      * `/m` 使用するマニフェストファイルを設定します
      * `/f` (前の手順で作成した) 使用するマッピングファイルを設定します。 
      * `/p` 出力パッケージ名を設定します
      * `/o` 出力ファイルが存在する場合に上書きするように設定します

2. パッケージが作成された後は、署名されている必要があります。 署名証明書を取得する最も簡単な方法は、Visual Studio で空のユニバーサル Windows プロジェクトを作成し、作成したファイルをコピーすることです `.pfx` 。ただし、 `MakeCert` `Pvk2Pfx` 「 [アプリパッケージ署名証明書を作成する方法](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)」の説明に従って、とのユーティリティを使用して手動で作成できます。

    > [!IMPORTANT]
    > 署名証明書を手動で作成する場合は、ソースプロジェクトまたはパッケージソースとは別のディレクトリにファイルを配置してください。そうしないと、秘密キーを含むパッケージの一部として含まれる可能性があります。

3. パッケージに署名するには、次のコマンドを使用します。 `AppxManifest.xml` の `Identity` 要素で指定されている `Publisher` は、証明書の `Subject` と一致する必要があります (これは `<PublisherDisplayName>` 要素では**ありません**。それはユーザーに表示されるローカライズされた表示名です)。 通常と同様に、`contoso_demo...` のファイル名をプロジェクトに適した名前で置き換えます。さらに `.pfx` ファイルが現在のディレクトリにないことを確認します (**これは非常に重要です**。そうしない場合、プライベート署名キーを含めて、パッケージの一部として作成されてしまいます)。

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    `signtool sign /?` と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです:
      * `/fd` ファイルダイジェストアルゴリズム (.appx は .appx の既定値) を設定します。
      * `/a` 最適な証明書が自動的に選択されます
      * `/f` 署名証明書を含む入力ファイルを指定します。

最後に、`.appx` ファイルをダブルクリックしてインストールします。またはコマンド ラインを使用する場合には、PowerShell プロンプトを開き、パッケージを含むディレクトリへ移動し、次のように入力します (`contoso_demo.appx` を使用するパッケージ名に置き換えます)。

```CMD
add-appxpackage contoso_demo.appx
```

証明書が信頼されていないというエラーが発生した場合、証明書が (ユーザー ストア**ではなく**) コンピューターのストアに追加されていることを確認します。 コンピューターのストアに証明書を追加するには、コマンドライン、または Windows エクスプローラーを使用します。

コマンドラインを使用する場合:

1. Visual Studio 2017 または Visual Studio 2019 のコマンドプロンプトを管理者として実行します。
2. `.cer` ファイルを含むディレクトリに移動します (このディレクトリが、ソース ディレクトリまたはプロジェクト ディレクトリではないことを確認してください)
3. `contoso_demo.cer` を使用するファイル名と置き換えて、次のコマンドを入力します。
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    `certutil -addstore /?` と入力すると、各パラメーターの意味が表示されますが、主なパラメーターは次のとおりです:
      * `-addstore` 証明書ストアに証明書を追加します。
      * `TrustedPeople` 証明書の配置先のストアを示します。

Windows エクスプローラーを使用する場合:

1. `.pfx` ファイルが含まれているフォルダーに移動します
2. `.pfx` ファイルをダブルクリックすると、**証明書インポート ウィザード**が表示されます
3. 選択 `Local Machine` してクリック `Next`
4. ユーザーアカウント制御管理者の昇格時のプロンプトが表示されたら、それを受け入れ、[ `Next`
5. 秘密キーのパスワードを入力し (存在する場合)、[] をクリックします。 `Next`
6. `Place all certificates in the following store` を選択
7. `Browse` をクリックして、(「信頼された発行元」**ではなく**) `Trusted People` フォルダーを選択します
8. [] をクリックし `Next` 、 `Finish`

`Trusted People` ストアに証明書を追加したら、もう一度パッケージをインストールします。

これでアプリは、`.resw` / `.pri` ファイルの正しい情報を使って、スタート メニューの [すべてのアプリ] のリストに表示されます。 空の文字列または `ms-resource:...` の文字列が表示された場合には、何かが間違っています。編集を再度確認し、すべて正しいかどうか確認します。 スタート メニューでアプリを右クリックすると、タイルとしてピン留めすることができ、さらに適切な情報が表示されることを確認できます。

### <a name="step-13-add-more-supported-languages"></a>手順 1.3: サポート言語を追加する

パッケージマニフェストに変更が加えられ、初期ファイルが作成された後、言語を追加するの `resources.resw` は簡単です。

#### <a name="create-additional-localized-resources"></a>追加のローカライズ リソースを作成する

最初に、追加のローカライズ リソースの値を作成します。 

`Strings` フォルダーで、適切な BCP-47 コードを使って、サポートする各言語のための、追加のフォルダーを作成します (たとえば、`Strings\de-DE`)。 各フォルダー内に、(XML エディターまたは Visual Studio デザイナーを使用して) 翻訳されたリソースの値を含む `resources.resw` ファイルを作成します。 ここでは、既にローカライズされた文字列が利用可能であり、それを `.resw` ファイルにコピーして利用できるものと想定します。このドキュメントでは、翻訳の手順そのものは扱いません。 

たとえば、`Strings\de-DE\resources.resw`ファイルは、次のようになります。<span style="background-color: yellow">強調表示されたテキスト</span>は `en-US` から変更されました。

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

次の手順では、`de-DE` と `fr-FR` の両方のリソースを追加したものと想定します。他の言語でも同じパターンで行うことができます。

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>パッケージマニフェストを更新して、サポートされる言語を一覧表示する

アプリでサポートされる言語を一覧表示するには、パッケージマニフェストを更新する必要があります。 Desktop App Converter は、既定の言語を追加しますが、他の言語は明示的に追加する必要があります。 `AppxManifest.xml` ファイルを直接編集する場合、`Resources` ノードを次のように更新します。必要な要素を追加し、<span style="background-color: yellow">サポートする適切な言語</span>を置き換え、一覧の最初のエントリが既定の (フォールバック) 言語となるようにします。 この例では、既定値は英語 (米国) で、ドイツ語 (ドイツ)、フランス語 (フランス) が追加でサポートされます。

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Visual Studio を使っている場合、何もする必要はありません。`Package.appxmanifest` には、特別な <span style="background-color: yellow">x-generate</span> 値が含まれます。これによってビルド プロセスで、プロジェクトに含まれる (BCP-47 コードを使った名前のフォルダーに基づく) 言語が挿入されます。 これは実際のパッケージマニフェストの有効な値ではないことに注意してください。これは、Visual Studio プロジェクトに対してのみ機能します。

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>ローカライズされた値でリビルドする

ここで再度、アプリケーションのビルドを行ってデプロイすることができます。Windows で言語の選択を変更すると、新たにローカライズされた値がスタート メニューに表示されます (言語を変更する方法については、以下で説明します)。

ここでも、Visual Studio では `Ctrl+Shift+B` を使ってビルドを行い、プロジェクトで右クリックして `Deploy` できます。

プロジェクトを手動でビルドする場合は、上記と同じ手順に従いますが、構成ファイルを作成する際に、既定の修飾子の一覧 (`/dq`) にアンダー スコアで区切られた追加の言語を追加します。 たとえば、前の手順で追加された、英語、ドイツ語、フランス語のリソースをサポートするには:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

これにより、指定されたすべての言語を含む PRI ファイルが作成され、それをテスト用に簡単に使用できます。 リソースの合計サイズが小さい場合、または少数の言語のみをサポートする場合は、配布するアプリとしてこれで十分である場合もあります。リソースのインストールとダウンロードのサイズを最小化するメリットを必要とする場合のみ、言語ごとの個別の言語パックをビルドする追加の作業を行います。

#### <a name="test-with-the-localized-values"></a>ローカライズされた値をテストする

新しくローカライズされた変更をテストするには、使用する新しい UI 言語を Windows に追加します。 言語パックをダウンロードしたり、システムを再起動したり、Windows UI 全体を他言語で表示させたりする必要はありません。 

1. `Settings` アプリを実行します (`Windows + I`)
2. [https://resources.azure.com](`Time & language`) に移動します
3. [https://resources.azure.com](`Region & language`) に移動します
4. [`Add a language`] をクリックします。
5. 必要な言語を入力 (または選択) します (たとえば `Deutsch` または `German`)
 * サブ言語がある場合は、必要なものを選びます (たとえば `Deutsch / Deutschland`)
6. 言語の一覧で新しい言語を選択します
7. [`Set as default`] をクリックします。

スタート メニューを開き、作成したアプリケーションを検索します。選択した言語のローカライズされた値が表示されます (他のアプリもローカライズされて表示される場合があります)。 ローカライズされた名前がすぐに表示されない場合は、スタート メニューのキャッシュが更新されるまで、数分待機します。 元の言語に戻すには、言語の一覧で既定の言語を変更します。 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>手順 1.4: パッケージマニフェストのより多くの部分をローカライズする (省略可能)

パッケージマニフェストのその他のセクションはローカライズできます。 たとえば、アプリがファイル拡張子を処理する場合、マニフェストに `windows.fileTypeAssociation` 拡張があります。<span style="background-color: lightgreen">緑の強調表示のテキスト</span>を表示されているとおりに使い (これはリソースの参照です)、<span style="background-color: yellow">黄色の強調表示のテキスト</span>をアプリに固有の情報で置き換えます。

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Visual Studio のマニフェスト デザイナーを使って、この情報を追加することもできます。`Declarations` タブを使い、<span style="background-color: lightgreen">強調表示の値</span>を記録します。

<p><img src="images\editing-declarations-info.png"/></p>

対応するリソース名を、各 `.resw` ファイルに追加し、<span style="background-color: yellow">強調表示のテキスト</span>をアプリに適切なテキストで置き換えます (*サポートされているそれぞれの言語*で行います)。

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

これは、エクスプローラーなどの Windows シェルの一部で表示されます。

<p><img src="images\file-type-tool-tip.png"/></p>

再び、ビルドを行い、パッケージをテストして、新しい UI 文字列が表示される新しいシナリオを試します。

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>フェーズ 2: MRT を使用してリソースを識別して検索する

前のセクションでは、MRT を使用してアプリのマニフェスト ファイルをローカライズする方法を紹介しました。これによって、Windows シェルに、アプリの名前とその他のメタデータを正しく表示できるようになりました。 これにはコードの変更は必要なく、`.resw` ファイルとその他のいくつかのツールのみを必要としました。 このセクションでは、MRT を使用して、既存のリソース形式のリソースを検索し、既存のリソース処理コードを最小限の変更で使用する方法を説明します。

### <a name="assumptions-about-existing-file-layout--application-code"></a>既存のファイル レイアウトとアプリケーションのコードについての想定

Win32 デスクトップ アプリをローカライズする方法は多数あるため、このホワイトペーパーでは、既存のアプリケーションの構造について、いつかの簡単な想定を置きます。この想定と実際のアプリの環境をマッピングしながら利用してください。 MRT の要件に準拠するため、既存のコードベースやリソース レイアウトを変更する必要がある場合があります。それらの多くは、このドキュメントの対象範囲外となります。

#### <a name="resource-file-layout"></a>リソース ファイルのレイアウト

この記事では、すべてのローカライズされたリソースのファイル名が同じであることを前提としています (たとえば、 `contoso_demo.exe.mui` また `contoso_strings.dll` はまたは) が、 `contoso.strings.xml` BCP-47 の名前 (、など) の異なるフォルダーに配置されていることを前提としてい `en-US` `de-DE` ます。 使用するリソース ファイルの数、名前、ファイル形式、関連付けられている API などは問題とはなりません。すべての*論理*リソースが同じファイル名である (そして異なる*物理*ディレクトリに配置されている) ことのみが問題となります。 

反対の例として、アプリケーションがフラットなファイル構造を使用していて、1 つの `Resources` ディレクトリに、`english_strings.dll` と `french_strings.dll` が含まれている場合には、これは MRT にうまくマッピングされません。 望ましい構造は、`Resources` ディレクトリにサブディレクトリがあり、そこにファイルが配置されている (たとえば `en\strings.dll` や `fr\strings.dll`) 構造です。 `strings.lang-en.dll` や `strings.lang-fr.dll` などのように、同じ基本ファイル名を使用して、修飾子を埋め込むことも可能ですが、言語コードと同じディレクトリの使用がコンセプトとしてシンプルであり、ここではそれを使います。

>[!NOTE]
> このファイルの名前付け規則に従わない場合でも、MRT.DLL を使用してパッケージ化する利点は引き続きあります。これには、さらに多くの作業が必要です。

たとえば、アプリケーションが、<span style="background-color: yellow">ui.txt</span> という名前の単純なテキスト ファイルに含まれる、(ボタンのラベルなどに使用される) カスタムの UI コマンドのセットを持ち、<span style="background-color: yellow">UICommands</span> フォルダーの下に配置される場合があります。

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>リソースの読み込みコード

この記事では、コード内のある時点で、ローカライズされたリソースを含むファイルを検索し、それを読み込んで使用することを前提としています。 リソースを読み込むために使用する API や、リソースを抽出するために使用する API などは重要ではありません。 この疑似コードでは、基本的に 3 つの手順があります。

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT では、このプロセスの最初の 2 つの手順のみを変更する必要があります。つまり、必要なリソース候補の決定と、その検索です。 リソースの読み込みと使用については、変更する必要はありません (ただし、それらを活用する場合は、それを行える機能を提供しています)。

たとえば、アプリケーションは、Win32 API `GetUserPreferredUILanguages`、CRT 関数 `sprintf`、Win32 API `CreateFile` を使用して、上記の 3 つの疑似コードの関数を置き換え、次にテキスト ファイルを手動で解析して、`name=value` のペアを検索できます。 (この詳細は重要ではありません。MRT は、リソースが検索された後の、リソースの処理に使用する手法には影響がないことを示すために説明しました)。

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>手順 2.1: MRT を使ってファイルを検索するためのコードの変更

リソースの検索に MRT を使用するため、コードを切り替えることは難しくはありません。 いくつかの WinRT の種類と数行のコードを使う必要があります。 主に使用される種類は次のとおりです。

* [ResourceContext](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext) 現在アクティブな修飾子の値のセット (言語、スケール ファクターなど) をカプセル化します
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (.NETバージョンではなく、WinRT バージョン) PRI ファイルからすべてのリソースへのアクセスを実現します
* [ResourceMap](/uwp/api/windows.applicationmodel.resources.core.resourcemap) PRI ファイル内のリソースの特定のサブセット (この例では、文字列リソースではなく、ファイル ベースのリソース) を表します
* [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource) 論理リソースとそのすべての候補を表します
* [ResourceCandidate](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate) 1 つの具体的な候補リソースを表す 

あるリソースのファイル名 (たとえば、上のサンプルの `UICommands\ui.txt` など) を解決する方法は、疑似コードでは、次のようになります。

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

このコードでは、(実際のディスク上のファイルの配置にかかわらず) 特定の言語のフォルダー (`UICommands\en-US\ui.txt` など) を要求して**いない**ことに、特に注意してください。 代わりに*論理*ファイル名 `UICommands\ui.txt` を要求して、ディスク上の言語ディレクトリからの適切なファイルの選択を MRT に依存しています。

これ以降、サンプル アプリは以前と同様に、`CreateFile` を使って `absoluteFileName` を読み込み、`name=value` ペアを解析できます。アプリ内のロジックの変更はまったく必要ありません。 C# または C++/CX で記述している場合、実際のコードは、この擬似コード以上にそれほど複雑になることはありません (さらに多くの中間変数は省略できます)。下記の **.NET リソースを読み込む**のセクションをご覧ください。 C++/WRL ベースのアプリケーションでは、WinRT API のアクティブ化と呼び出しに使用される低レベルの COM ベースの API によってより複雑になりますが、基本的な手順は同じです。下記の **Win32 MUI リソースを読み込む**のセクションをご覧ください。

#### <a name="loading-net-resources"></a>.NET リソースを読み込む

.NET には、リソースを検索して読み込むための組み込みのメカニズム (「サテライト アセンブリ」と呼ばれます) があるため、上記の合成の例のように、明示的なコードの置き換えは必要ありません。.NET では、適切なディレクトリにリソース DLL が存在することのみが必要であり、それらは自動的に検索されます。 アプリがリソースパックを使用して MSIX または .appx としてパッケージ化されている場合、ディレクトリ構造は多少異なります。これは、リソースディレクトリがメインアプリケーションディレクトリのサブディレクトリであるか、それともピアである (ユーザーの設定に言語が指定されていない場合はまったく存在しない) ことです。 

たとえば、次のレイアウトを持つ .NET アプリケーションを考えます。ここでは、すべてのファイルが `MainApp` フォルダーの下に存在しています。

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

.Appx に変換した後、レイアウトは次のようになります。これは既定の言語であり、 `en-US` ユーザーの言語一覧にドイツ語とフランス語の両方が表示されていることを前提としています。

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

ローカライズ リソースが、メイン実行可能ファイルのインストール場所の下のサブディレクトリに存在しないため、組み込みの .NET リソースの解決が失敗します。 さいわい、.NET は、失敗したアセンブリの読み込みの試行を処理するための、明確に定義されたメカニズムである、`AssemblyResolve` イベントを備えています。 MRT を使用する .NET アプリは、このイベントに登録して、見つからなかったアセンブリを .NET リソース サブシステムに提供する必要があります。 

WinRT API を使用して、.NET で使われるサテライト アセンブリを検索する方法の簡単な例は次のとおりです。このコードは、最小限の実装を示すように、意図的に圧縮されていますが、上記の疑似コードによく対応しています。ここでは、渡されている `ResolveEventArgs` が検索する必要があるアセンブリの名前を提供します。 このコードの実行可能なバージョン (詳細なコメントとエラー処理を含む) は、[GitHub の **.NET アセンブリ リゾルバー** サンプル](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo) の `PriResourceRsolver.cs` にあります。

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

上記のクラスでは、次のコードを事前にアプリケーションのスタートアップ コードのどこかに (いずれかのローカライズ リソースを読み込む前に) 追加する必要があります。

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

.NET ランタイムは、リソース DLL が見つからない場合には、`AssemblyResolve` イベントを発生させます。その場合、提供されたイベント ハンドラーは、MRT を使って必要なファイルを検索し、アセンブリを返します。

> [!NOTE]
> アプリに他の目的のハンドラーが既にある場合は `AssemblyResolve` 、リソースを解決するコードを既存のコードと統合する必要があります。

#### <a name="loading-win32-mui-resources"></a>Win32 MUI リソースを読み込む

Win32 MUI リソースの読み込みは、.NET サテライト アセンブリの読み込みと基本的には同じですが、C++/CX または C++/WRL コードを使います。 C++/CX を使うと、コードがよりシンプルになり、上記の C# にとても近くなりますが、C++ 言語拡張、コンパイラ スイッチ、その他のランタイム オーバーヘッドを使うため、これはあまり望ましくない場合があります。 その場合は、C++/WRL を使うと、コードは冗長になりますが、影響のより小さいソリューションとなります。 それでも、ATL プログラミング (または COM 一般) に慣れている場合には、WRL がなじみやすい選択となります。 

次のサンプル関数は、C++/WRL を使って、特定のリソース DLL を読み込み、`HINSTANCE` を返します。これにより、通常の Win32 リソース API を使って、さらにリソースを読み込むことができます。 .NET ランタイムに要求された言語を使って明示的に `ResourceContext` を初期化する C# のサンプルとは異なり、このコードはユーザーの現在の言語に依存しています。

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>フェーズ 3: リソース パッケージをビルドする

これで、すべてのリソースを含む "ファット" パッケージができました。ダウンロードとインストールのサイズを最小化するために、メイン パッケージとリソース パッケージを分離してビルドするには、2 つの方法があります。

* 既存のファット パッケージを使い、[Bundle Generator ツール](https://www.microsoft.com/store/apps/9nblggh43pmq)を実行して、自動的にリソース パッケージを作成します。 これは、既にファット パッケージを作成するビルド システムがあり、ポスト プロセスとしてリソース パッケージを生成する場合に、推奨されるアプローチです。
* 直接、個々のリソース パッケージを作成し、それらをビルドしてバンドルを作成します。 これは、ビルド システムをより細かく制御して、パッケージを直接作成する場合に、推奨されるアプローチです。

### <a name="step-31-creating-the-bundle"></a>手順 3.1: バンドルを作成する

#### <a name="using-the-bundle-generator-tool"></a>Bundle Generator ツールを使用する

Bundle Generator ツールを使用するには、パッケージ用に作成した PRI 構成ファイルを手動で更新して、`<packaging>` セクションを削除する必要があります。

Visual Studio を使用している場合は、ファイルとを作成して、すべての言語をメインパッケージに組み込む方法について、 [デバイスにリソースがインストールされているかどうかに関係なく、リソースがデバイスにインストールされている](/previous-versions/dn482043(v=vs.140)) ことを確認してください `priconfig.packaging.xml` `priconfig.default.xml` 。

ファイルを手動で編集する場合は、次の手順に従います。 

1. 正しいパス、ファイル名、言語を置き換えて、以前と同じ方法で構成ファイルを作成します。

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. 作成された `.xml` ファイルを手動で開き、`&lt;packaging&rt;` セクション全体を削除します (それ以外はそのまま残します)。

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. 更新された構成ファイルと適切なディレクトリとファイル名を使って、以前と同じ方法で `.pri` ファイルと `.appx` パッケージをビルドします (これらのコマンドについて詳しくは上記をご覧ください)。

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. パッケージが作成されたら、次のコマンドを使用して、適切なディレクトリとファイル名を使用してバンドルを作成します。

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

最後の手順である署名 (下記参照) に進むことができます。

#### <a name="manually-creating-resource-packages"></a>手動でリソース パッケージを作成する

リソース パッケージを手動で作成する場合は、やや異なるコマンドのセットを実行して、別の `.pri` ファイルと `.appx` ファイルをビルドする必要があります。これらはすべて、上でファット パッケージの作成に使用したコマンドに似ていますので、簡単な説明にとどめます。 注: すべてのコマンドでは、現在のディレクトリは `AppXManifest.xml` ファイルを含むディレクトリであると想定していますが、すべてのファイルは親ディレクトリに配置されます (必要な場合は、別のディレクトリを使用できますが、プロジェクト ディレクトリはクリーンに保ち、これらのファイルを配置しないようにします)。 いつもと同様に、"Contoso" のファイル名を独自のファイル名で置き換えます。

1. 次のコマンドを使用して、既定の言語**のみ**を既定の修飾子として命名する、構成ファイルを作成します。この場合では `en-US` です。

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. 次のコマンドを使って、メイン パッケージの既定の `.pri` ファイルと`.map.txt` ファイルを作成し、さらにプロジェクトの各言語の追加のファイルのセットを作成します。

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. 次のコマンドを使って、メイン パッケージを作成します (これには実行可能コードと既定の言語リソースが含まれています)。 いつもと同様に、必要に応じて名前を変更します。後でバンドルの作成が容易になるように、パッケージは別のディレクトリに配置する必要があります (この例では、`..\bundle` ディレクトリを使います)。

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. メイン パッケージが作成されたら、次のコマンドを追加言語ごとに 1 回ずつ使います (つまり、前の手順で生成された各言語マップ ファイルに、このコマンドを繰り返します)。 ここでも、出力は別のディレクトリ (メイン パッケージと同じディレクトリ) にする必要があります。 言語が `/f` オプションと `/p` オプションの **両方** で指定されていることに注意してください。また、新しい `/r` 引数の使い方 (リソース パッケージが必要であることを指定します) に注意してください。

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. バンドル ディレクトリからのすべてのパッケージをまとめて、1 つの `.appxbundle` ファイルを作成します。 新しい `/d` オプションは、バンドル内のすべてのファイルが使用するディレクトリを指定します (前の手順で `.appx` ファイルを別のディレクトリに配置したのはこのためです)。

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

パッケージをビルドする最後の手順は署名です。

### <a name="step-32-signing-the-bundle"></a>手順 3.2: バンドルに署名する

(Bundle Generator ツールを使うか、または手動で) `.appxbundle` を作成したら、メイン パッケージとすべてのリソース パッケージを含む、1 つのファイルが作成されます。  最後の手順は、ファイルに署名を行い、Windows がインストールできるようにすることです。

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

これによって、メイン パッケージとすべての言語固有のリソース パッケージを含む、署名済みの `.appxbundle` ファイルが作成されます。 これはパッケージ ファイルと同様にダブルクリックでき、それによって、アプリに加えて、ユーザーの Windows の言語設定に基づく適切な言語をインストールすることができます。

## <a name="related-topics"></a>関連トピック

* [言語、スケール、ハイ コントラスト、その他の修飾子用にリソースを調整する](tailor-resources-lang-scale-contrast.md)