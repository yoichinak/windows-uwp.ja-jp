---
title: ユニバーサル Windows プラットフォーム (UWP) アプリとは
description: Windows 10 を搭載するさまざまなデバイスで実行できるユニバーサル Windows プラットフォーム (UWP) アプリについて説明します。
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperf-fy21q1
keywords: Windows 10, UWP, ユニバーサル
ms.localizationpriority: medium
ms.openlocfilehash: e0d6991f261a9ee207a7e36f3dc5ad6a145f3b12
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104673"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>ユニバーサル Windows プラットフォーム (UWP) アプリとは

UWP は、Windows 用クライアント アプリケーションを作成するさまざまな方法の 1 つです。 UWP アプリは、WinRT API を使用して、インターネットに接続されたデバイスに適した、強力な UI と高度な非同期機能を提供します。

UWP アプリの作成を始めるために必要なツールをダウンロードするには、「[準備](/windows/apps/get-set-up)」を確認し、[初めてのアプリを作成](your-first-app.md)してください。


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>UWP が Microsoft 開発ストーリーの中で占める位置

UWP は、Windows 10 デバイスで実行されるアプリを作成するための 1 つの選択肢であり、他のプラットフォームと組み合わせることができます。 UWP アプリでは、Win32 API や .NET クラスを使用できます ([UWP アプリ用の API セット](/previous-versions/mt186421(v=vs.85))、[UWP アプリ用の DLL](/previous-versions/mt186422(v=vs.85))、[UWP アプリ用の .NET](/dotnet/api/index?view=dotnet-uwp-10.0) に関する各記事をご覧ください)。

Microsoft の開発ストーリーは進化を続けており、[WinUI](/windows/apps/winui/)、[MSIX](/windows/msix/)、[Project Reunion](https://github.com/microsoft/ProjectReunion) などのイニシアチブと並んで、UWP はクライアント アプリを作成するための強力なツールです。


## <a name="features-of-a-uwp-app"></a>UWP アプリの機能

UWP アプリには次のような特長があります。

- 高いセキュリティ。UWP アプリはアクセスするデバイス リソースとデータを宣言します。 ユーザーは、そのアクセスを承認する必要があります。
- Windows 10 を実行しているすべてのデバイスで共通の API を使用できます。
- デバイス固有の機能を使用して、異なるデバイスの画面サイズ、解像度、DPI に合わせて UI を調整することができます。
- Windows 10 を実行するすべてのデバイス (または指定したデバイスのみ) で Microsoft Store から入手できます。 Microsoft Store では、アプリで収益を上げるいくつかの方法を提供します。
- コンピューターへのリスクや "コンピューターの劣化" を気にせずにインストールおよびアンインストールできます。
- 魅力的。ライブ タイル、プッシュ通知、Windows タイムラインや Cortana の前回終了した位置から再開を利用したユーザー アクティビティによって、ユーザーのエンゲージメントを高めます。
- C#、C++、Visual Basic、JavaScript でプログラミング可能です。 UI には、WinUI、XAML、HTML、または DirectX を使用します。

これらをさらに詳しく見てみましょう。

### <a name="secure"></a>セキュリテイ保護

UWP アプリでは、マイク、位置情報、Web カメラ、USB デバイス、ファイルなどへのアクセスに必要なデバイスの機能をマニフェストで宣言します。 アプリに機能が付与される前に、ユーザーがそのアクセスを確認して承認する必要があります。

### <a name="a-common-api-surface-across-all-devices"></a>すべてのデバイスに共通の API セット

Windows 10 ではユニバーサル Windows プラットフォーム (UWP) が導入され、Windows 10 を実行する各デバイスで共通のアプリ プラットフォームが提供されます。 UWP のコア API は、すべての Windows デバイスで同じです。 アプリがコア API のみを使う場合は、対象となるのがデスクトップ PC、Xbox、Mixed Reality ヘッドセットなどのいずれかに関係なく、そのアプリはどの Windows 10 デバイスでも動作します。

C++/WinRT または C++/CX で記述された UWP アプリは UWP の一部である Win32 API にアクセスします。 これらの Win32 API は、すべての Windows 10 デバイスによって実装されます。

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>拡張 SDK が特定のデバイスの種類に固有の機能を公開する

ユニバーサル API を対象としている場合、アプリは Windows 10 が動作しているすべてのデバイスで実行できます。 ただし、UWP アプリでデバイス固有の API を利用したい場合は、そうすることもできます。

拡張 SDK では、さまざまなデバイスに特化した API を呼び出すことができます。 たとえば、UWP アプリで IoT デバイスをターゲットにしている場合、IoT デバイスに固有の機能をターゲットにしたプロジェクトに IoT 拡張 SDK を追加できます。 拡張 SDK の追加に関する詳細については、「[拡張 SDK を使用するプログラミング](/uwp/extension-sdks/device-families-overvieww#extension-sdks)」の **拡張 SDK** のセクションを参照してください。

特定の種類のデバイスでのみ実行されることを想定し、Microsoft Store からの配布をその種類のデバイスのみに制限するようにアプリを記述することができます。 または、実行時に API の存在を条件付きでテストし、結果に応じてアプリの動作を調整できます。 詳細については、「[拡張 SDK を使用するプログラミング](/uwp/extension-sdks/device-families-overview#writing-code)」の **コードの記述** のセクションを参照してください。<br>

次のビデオでは、デバイス ファミリとアダプティブ コードの作成に関する簡単な概要を示します。
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>アダプティブ コントロールおよび入力

UI 要素では、そのレイアウトやスケールを調整して、アプリが実行されている画面のサイズや DPI に対応します。 UWP アプリは、キーボード、マウス、タッチ、ペン、Xbox One コントローラーなど、さまざまな種類の入力デバイスで問題なく機能します。 特定の画面サイズやデバイスに合わせて UI をさらに調整する必要がある場合は、新しく追加されたレイアウト パネルとツールを使用して、アプリが実行されるさまざまなデバイスやフォーム ファクターに合わせて調整可能な UI を設計できます。

![Windows デバイス](images/1894834-hig-device-primer-01-500.png)

Windows では、UI を次の機能を備えた複数のデバイスを対象としたものにすることができます。

- UI をデバイスの画面の解像度に合わせて最適化するために役立つユニバーサル コントロールとレイアウト パネル。 たとえば、ボタンやスライダーなどのコントロールは、デバイスの画面サイズや DPI 密度に合わせて自動的に調整されます。 レイアウト パネルは、画面のサイズに基づいてコンテンツのレイアウトを調整するのに役立ちます。 デバイス間での解像度と DPI の相違を調整するアダプティブ スケーリング。
- 一般的な入力処理では、タッチ、ペン、マウス、キーボード、またはコントローラー (Microsoft Xbox コントローラーなど) による入力を受け取ることができます。
- さまざまな画面の解像度に合わせて変化する UI の設計に役立つツール。

アプリの UI の一部はデバイス間で自動的に対応します。 ただし、アプリのユーザー エクスペリエンスの設計では、アプリが実行されているデバイスへの対応が必要になる場合があります。 たとえば、フォト アプリが小型のハンドヘルド デバイスで実行されている場合、UI を片手での使用に適するように調整できます。 フォト アプリがデスクトップ コンピューターで実行されている場合、UI は広い画面スペースを利用するように調整する必要があります。

### <a name="theres-one-store-for-all-devices"></a>1 つのストアですべてのデバイスに対応する

統一されたアプリ ストアによって、PC、タブレット、Xbox、HoloLens、Surface Hub、IoT (モノのインターネット) デバイス などの Windows 10 デバイスでアプリを入手できます。 開発者は、アプリをストアに提出し、すべての種類のデバイスまたは選択した種類のデバイス向けに販売できます。 Windows デバイス向けのすべてのアプリを 1 か所で提出、管理できます。 UWP の機能を使って最新化し、Microsoft Store での販売を希望する C++ デスクトップ アプリがある場合も 問題ありません。

UWP アプリは、詳細なテレメトリや分析のために [Application Insights](https://azure.microsoft.com/services/application-insights/) に統合されています。これは、ユーザーを理解し、アプリの品質を向上させるために欠かせないツールです。

UWP アプリは、[MSIX](/windows/msix/) を使用してパッケージ化し、Microsoft Store またはその他の方法で配布することができます。 MSIX を使用すると、アプリがどのような方法で配布されても、アプリを更新できます。「[ストアに公開されていないアプリ パッケージをコードから更新する](/windows/msix/non-store-developer-updates)」をご覧ください。

### <a name="monetize-your-app"></a>アプリの収益の獲得

アプリを収益化する方法を選択できます。 アプリで収益を得る方法は多数あります。 必要なのは、ニーズに合った最適な方法を選ぶことだけです。たとえば、次のような方法があります。

- 有料のダウンロードは最も簡単な方法です。 必要な作業は価格の指定だけです。
- 試用版を使うとユーザーは購入前にアプリを試すことができ、従来の "フリーミアム" オプションよりも目につきやすく、コンバージョンも簡単です。
- セール価格はユーザーに対して動機付けとなります。
- アプリ内購入。


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>関連するリアルタイム情報をユーザーに表示して繰り返し集客する

UWP アプリに対するユーザーのエンゲージメントを維持するためのさまざまな方法があります。

- ライブ タイルとロック画面タイルでは、アプリからのコンテキストに関連したタイムリーな情報をひとめでわかるように表示します。
- プッシュ通知は、ユーザーがリアルタイムの通知に注目できるようにします。
- ユーザー アクティビティでは、異なるデバイスでも、アプリで前回中断したところから作業を始めることができます。
- アクション センターでは、アプリの通知を整理できます。
- バックグラウンドの実行とトリガーにより、ユーザーが必要とするときにアプリが有効になります。
- アプリで音声と Bluetooth LE デバイスを使うと、ユーザーはそれらのデバイスを中心に広がる世界とやり取りすることができます。
- 音声コマンド機能をアプリに追加するには、Cortana を統合します。

##  <a name="use-a-language-you-already-know"></a>使い慣れた言語の使用

UWP アプリでは、オペレーティング システムによって提供されるネイティブな API である Windows ランタイムを使います。 この API は C++ で実装され、C#、Visual Basic、C++、JavaScript でサポートされます。 UWP アプリを作成するための一部のオプションを次に示します。

- XAML UI と、C#、VB、または C++
- DirectX UI と C++
- JavaScript と HTML
- WinUI

## <a name="links-to-help-you-get-going"></a>役に立つリンク

### <a name="get-set-up"></a>準備

「[準備](/windows/apps/get-started/get-set-up)」を確認し、アプリの作成を始めるために必要なツールをダウンロードしてから、[初めてのアプリを作成](your-first-app.md)してください。

### <a name="design-your-app"></a>アプリをデザインする

Microsoft デザイン システムは Fluent と呼ばれます。 Fluent Design System は、すべての種類の Windows デバイスで適切に動作するアプリを作成するためのベスト プラクティスと組み合わされた、UWP 機能のセットです。 Fluent エクスペリエンスは、タブレット、ノート PC、デスクトップ、テレビから、仮想現実デバイスまで、さまざまなデバイスに対応し、自然に操作できます。 Fluent Design の概要については、[UWP アプリ向けの Fluent Design System](/windows/uwp/design/fluent-design-system) に関するページを参照してください。

適切な[設計](http://design.windows.com/)とは、アプリの外観や機能に加えて、ユーザーによるアプリの操作方法を決定するプロセスです。 ユーザー エクスペリエンスは、ユーザーがアプリでどの程度満足するかを判断する場合に大きな役割を果たします。そのため、この手順は必ず守ってください。 [設計の基本](https://developer.microsoft.com/windows/apps/design)に関するページでは、ユニバーサル Windows アプリの設計を紹介します。 ユーザーを楽しませる UWP アプリの設計の情報については、[デザイナー向けユニバーサル Windows プラットフォーム (UWP) アプリの紹介](../design/basics/design-and-ui-intro.md)に関するページをご覧ください。 コーディングを開始する前に、ターゲットにするすべての異なるフォーム ファクターでアプリを使用する際の操作エクスペリエンスを検討するために役立つ、[デバイスの基本情報](../design/devices/index.md)に関するページをご覧ください。

さまざまなデバイスでの操作に加えて、複数のデバイスで動作する利点を取り入れるように[アプリの計画](./plan-your-app.md)を行います。 次に、例を示します。

- モバイル、小型画面デバイス、大型画面デバイスに対応するには、「[UWP アプリのナビゲーション デザインの基本](../design/basics/navigation-basics.md)」を使用してワークフローをデザインします。 さまざまな画面サイズと解像度に対応するように[ユーザー インターフェイスをレイアウト](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)します。

- 複数の入力の種類の対処方法を検討してください。 ユーザーが [Cortana](/cortana/skills/)、[音声認識](../design/input/speech-interactions.md)、[タッチ操作](../design/input/touch-interactions.md)、[タッチ キーボード](../design/input/keyboard-interactions.md)などを使ってアプリを操作する方法については、[操作のガイドライン](../design/layout/index.md)に関するページをご覧ください。  または、従来の操作エクスペリエンスの詳細については、[テキストとテキスト入力のガイドライン](../design/controls-and-patterns/text-controls.md)に関するページを参照してください。

### <a name="add-services"></a>サービスの追加

- [クラウド サービス](https://azure.microsoft.com/documentation/services/cloud-services)を使用して、デバイス間で同期します。
- アプリのエクスペリエンスをサポートするには、[Web サービスに接続する](/previous-versions/windows/apps/hh761504(v=win.10))方法を参照してください。
- [プッシュ通知](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)と[アプリ内購入](../monetize/enable-in-app-product-purchases.md)を計画に盛り込みます。 これらの機能はさまざまなデバイスで動作する必要があります。

### <a name="submit-your-app-to-the-store"></a>アプリをストアに提出する

[パートナー センター](https://partner.microsoft.com/dashboard)を使うと、Windows デバイス向けのすべてのアプリの管理と申請を 1 か所で行うことができます。 Microsoft Store にアプリの公開を申請する方法については、「[Windows アプリとゲームを公開する](../publish/index.md)」をご覧ください。

新しい機能が追加されたことで、より高度な管理が可能になった一方、プロセスは簡単になりました。 また、詳しい[分析レポート](../publish/analytics.md)に加えて、[支払いの詳細](/partner-center/payout-statement)も得られるようになりました。いずれも[アプリの宣伝と顧客エンゲージメントの獲得](../publish/attract-customers-and-promote-your-apps.md)に役立ちます。

別の入門資料については、「[Windows 10 デバイス向け Windows アプリのビルドの概要](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices)」をご覧ください。

### <a name="more-advanced-topics"></a>高度なトピック

- アプリでのユーザー アクティビティが Windows タイムラインと Cortana の 前回終了した位置から再開機能に表示されるようにするには、[ユーザー アクティビティ](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97)の使用方法に関するページを参照してください。
- [UWP アプリのタイル、バッジ、通知](../design/shell/tiles-and-notifications/index.md)の使用方法に関するページを参照してください。
- UWP アプリで利用可能な Win32 API の完全な一覧については、[UWP アプリの API セット](/previous-versions/mt186421(v=vs.85))および [UWP アプリの DLL](/previous-versions/mt186422(v=vs.85)) に関するページをご覧ください。
- .NET UWP アプリの作成の概要については、[.NET でのユニバーサル Windows アプリ](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/)に関するページを参照してください。
- UWP アプリで使用できる .NET 型の一覧については、[UWP アプリ用 .NET](/dotnet/api/index?view=dotnet-uwp-10.0) に関するページを参照してください。
- [.NET Native によるアプリのコンパイル](/dotnet/framework/net-native/)
- [デスクトップ ブリッジ](https://developer.microsoft.com/windows/bridges/desktop)を使用して、既存のデスクトップ アプリに Windows 10 ユーザー向けの最新のエクスペリエンスを追加し、そのアプリを Microsoft Store で配布する方法を確認してください。

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>ユニバーサル Windows プラットフォームと Windows ランタイム API の関連性
ユニバーサル Windows プラットフォーム (UWP) アプリをビルドする場合は、"ユニバーサル Windows プラットフォーム (UWP)" と "Windows ランタイム (WinRT)" をほぼ同義と考えると便利です。 ただし、テクノロジの実体を明らかにして、この 2 つの概念の違いを確認しておくことも可能 *です*。 この最後のセクションは、この点に興味があるユーザーを対象としています。

Windows ランタイム (WinRT) API は、Windows API が進化したものです。 元々 Windows は、フラットな C スタイルの Win32 API でプログラミングされました。 そこに追加されたのが COM API です (主な例は [DirectX](/windows/desktop/directx))。 Windows フォーム、WPF、.NET、およびマネージ言語は、Windows アプリの記述方法や API テクノロジの特徴がそれぞれに異なっていました。 COM の次の段階というのが Windows ランタイムの実体です。 実際のアプリケーション バイナリ インターフェイス (ABI) レイヤーで、COM におけるそのルーツが可視化されます。 ただし、Windows ランタイムはさまざまなプログラミング言語から呼び出すことができるように設計されています。 また、それぞれの言語にとってきわめて自然な方法で呼び出すことができます。 このために、Windows ランタイムは言語プロジェクションと呼ばれる仕組みを介してアクセスできるようになっています。 Windows ランタイムの言語プロジェクションは、C#、Visual Basic、標準 C++、JavaScript などに対応しています。 さらに、適切にパッケージ化すれば (「[デスクトップ ブリッジ](/windows/msix/desktop/source-code-overview)」を参照)、次のような広範囲の言語モデルのいずれかでビルドされたアプリから WinRT API を呼び出すことができます:Win32、.NET、WinForms、WPF。

もちろん、UWP アプリから WinRT API を呼び出すこともできます。 UWP は、Windows ランタイム上に構築されたアプリケーション モデルです。 技術的な UWP アプリケーション モデルのベースは [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication) ですが、選択したプログラミング言語によっては、そうした詳細を意識することはありません。 このトピックで説明したように、提供する価値という観点で考えると、UWP は、必要に応じて Microsoft Store に公開でき、かつさまざまなデバイス フォーム ファクターで稼働する単一バイナリを作成するのに適しています。 UWP アプリが対応するデバイスの範囲は、アプリで呼び出す対象として限定した Windows ランタイム API のサブセット、または条件付きで呼び出す Windows ランタイム API のサブセットによって異なります。

Windows ランタイム API の基盤であるテクノロジと、ユニバーサル Windows プラットフォームのメカニズムおよびビジネス価値の違いを、このセクションで理解していただけたら幸いです。