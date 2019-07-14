---
Description: このセクションでは、別のアプリ プラットフォームの特定のキーの Windows 機能をサポートする方法と、コードで、機能の使用を開始する方法を理解できます。
title: 機能とテクノロジ
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 433869c2f6b9a540259073127de1f139b70e9de0
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215113"
---
# <a name="features-and-technologies-for-windows-apps"></a>機能およびテクノロジの Windows アプリ

どのような種類のアプリを構築またはデバイスを対象としている場合に関係なくは、Windows は、重要なアプリ シナリオの重要な構成要素には多くの機能をサポートします。 これらの機能の一部をユニバーサル Windows プラットフォーム (UWP)、Win32 は、(Windows API)、およびさまざまな方法で他のアプリ プラットフォーム。 次の記事では、特定の Windows 機能が別のアプリ プラットフォームでサポートされている方法と、コードで、機能の使用を開始する方法を理解するのに役立ちます。

この記事ではカスタマイズされた一覧の記事を読む場合は重要な Windows 機能および UWP と Win32 のテクノロジにアクセスする方法について (Windows API)、WPF と Windows フォーム アプリ プラットフォームです。 各プラットフォームの開発機能については、次のリソースを参照してください。

* [UWP ドキュメント](/windows/uwp/index)
* [Win32 の (Windows API) のドキュメント](/windows/desktop/index)
* [WPF のドキュメント](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Windows フォームのドキュメント](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>キーの Windows 機能およびテクノロジ

次のセクションでは、いくつかの重要な Windows 機能と最新の配信し、顧客に魅力的なエクスペリエンスを提供するためのテクノロジを選択します。

### <a name="windows-ink"></a>Windows Ink

![Surface ペン](images/hero-small.png)  

Windows Ink プラットフォームでペン デバイスを使うと、自然な形でデジタルの手書きノート、描画、コメントを作れます。 このプラットフォームは、デジタイザー入力のインク データとしてのキャプチャ、インク データの生成、インク データの管理、出力デバイスのインク ストロークとしてのインク データのレンダリング、手書き認識によるインクからテキストへの変換をサポートします。

Windows アプリで Windows Ink を使用するさまざまな方法の詳細については、[Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)を参照してください。

### <a name="speech-interactions"></a>音声操作

![SGRS 文法ファイルに基づく制約の場合の、最初の認識画面](images/speech-listening-initial.png)

![SGRS 文法ファイルに基づく制約の場合の、最終的な認識画面](images/speech-listening-complete.png)

Windows では、アプリのユーザー エクスペリエンスに直接音声認識と音声合成 (TTS、または音声合成とも呼ばれます) を統合するさまざまな方法を提供します。 音声は、アプリ、補完するものとして、または偶数の置換では、キーボード、マウス、タッチとジェスチャと対話するための堅牢で楽しい方法です。

Windows アプリで音声の相互作用を使用するさまざまな方法の詳細については、[音声の相互作用](/windows/uwp/design/input/speech-interactions)を参照してください。

### <a name="windows-ai"></a>Windows AI

![Windows AI](images/windows-ai.png)

Windows アプリケーションを強化するために使用できるさまざまな AI ソリューションをいくつか提供しています。 Windows Machine Learning では、トレーニング済みのマシン学習モデルをアプリに統合し、デバイス上のローカルに実行します。 Windows のビジョンのスキルを使用すると、構築済みのライブラリを使用して一般的なイメージ処理タスクを実行するか、独自のカスタム ソリューションを作成できます。 DirectML は低レベル、DirectX スタイルの API を使用できます、ハードウェアの活用します。

Windows アプリに AI を統合するさまざまな方法の詳細については、[Windows AI](https://docs.microsoft.com/windows/ai/)参照してください。

## <a name="features-and-technologies-by-platform"></a>機能とプラットフォームでのテクノロジ

次のセクションは、Windows のコア機能と、メイン アプリ プラットフォームからのテクノロジと統合する方法の詳細についての役に立つリンクを提供します。UWP、Win32 (Windows API)、WPF と Windows フォームです。

### <a name="user-interface-and-accessibility"></a>ユーザー インターフェイスとユーザー補助機能

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [デザイン](/windows/uwp/design/basics/)<br/><br/>[レイアウト](/windows/uwp/design/layout/)<br/><br/>[コントロール](/windows/uwp/design/controls-and-patterns/)<br/><br/>[入力](/windows/uwp/design/input/)<br/><br/>[タイル](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[ビジュアル レイヤー](/windows/uwp/composition/visual-layer)<br/><br/>[XAML プラットフォーム](/windows/uwp/xaml-platform/)<br/><br/>[起動、再開、バックグラウンド タスク](/windows/uwp/launch-resume/)<br/><br/>[Windows のアクセシビリティ](/windows/uwp/design/accessibility/accessibility)<br/><br/>  |  [デスクトップのユーザー インターフェイス](/windows/desktop/windows-application-ui-development)<br/><br/>[デスクトップ環境とシェル](/windows/desktop/user-interface)<br/><br/>[windows コントロール](/windows/desktop/controls/window-controls)<br/><br/>[デスクトップ アプリケーションの UWP コントロール (XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[デスクトップ アプリでの UWP ビジュアル層](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows とメッセージ](/windows/desktop/winmsg/windowing)<br/><br/>[メニューとその他のリソース](/windows/desktop/menurc/resources)<br/><br/>[高 DPI](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[ユーザー補助](/windows/desktop/accessibility)<br/><br/>  |  [WPF での Windows](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[ナビゲーションの概要](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[WPF の XAML](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[コントロール](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[ビジュアル層のプログラミング](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[入力](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[ユーザー補助](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>  | [Windows フォームを作成](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[コントロール](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[ダイアログ ボックス](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[ユーザー入力](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Windows フォームのユーザー補助](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/> |

### <a name="audio-video-and-graphics"></a>オーディオ、ビデオ、およびグラフィック

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [オーディオ、ビデオ、およびカメラ](/windows/uwp/audio-video-camera/)<br/><br/>[メディア再生](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[ビジュアル レイヤー](/windows/uwp/composition/visual-layer)<br/><br/>[XAML プラットフォーム](/windows/uwp/xaml-platform/) |  [オーディオとビデオ](/windows/desktop/audio-and-video)<br/><br/>[グラフィックスとゲーム](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI +](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [グラフィックス](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimedia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [グラフィックスと描画](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms?view=netframework-4.8)<br/><br/>[SoundPlayer クラス](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>データ アクセスとアプリのリソース

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [データ アクセス](/windows/uwp/data-access/)<br/><br/>[データ バインディング](/windows/uwp/data-binding/)<br/><br/>[ファイル、フォルダー、およびライブラリ](/windows/uwp/files/)<br/><br/>[アプリのリソース](/windows/uwp/app-resources/) |  [データ アクセスおよびストレージ](/windows/desktop/data-access-and-storage)<br/><br/>[ローカル ファイル システム](/windows/desktop/fileio/file-systems)<br/><br/>[リソースの概要](/windows/desktop/menurc/resources-overviews)</li>  |  [データとモデリング](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[データ バインディング](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[.NET アプリでのリソース](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[アプリケーション リソース、コンテンツ、およびデータ ファイル](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [データとモデリング](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[データ バインディング](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[.NET アプリでのリソース](https://docs.microsoft.com/dotnet/framework/resources/?view=netframework-4.8)<br/><br/>[アプリケーションの設定](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>デバイス、ドキュメント、および印刷

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [デバイス機能を有効にする](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[デバイスの列挙](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[センサー](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[印刷とスキャン](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [センサー API](/windows/desktop/sensorsapi/portal)<br/><br/>[印刷](/desktop/printdocs/printdocs-printing)<br/><br/>[UPnP API](/desktop/upnp/universal-plug-and-play-start-page) |  [印刷と印刷システムの管理](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [印刷のサポート](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>システム、ネットワーク、および電源

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [デバイスの列挙](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[バッテリー情報の取得](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[スレッド化と非同期プログラミング](/windows/uwp/threading-async/)<br/><br/>[ネットワークと Web サービス](/windows/uwp/networking/) | [システム サービス](/windows/desktop/system-services)<br/><br/>[メモリ管理](/windows/desktop/memory/memory-management)<br/><br/>[電源管理](/windows/desktop/power/power-management-portal)<br/><br/>[プロセスとスレッド](/windows/desktop/procthread/processes-and-threads)<br/><br/>[ネットワークとインターネット](/windows/desktop/networking)<br/><br/>[Windows システム情報](/windows/desktop/sysinfo/windows-system-information) |  [スレッド モデル](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[.NET Framework のネットワーク プログラミング](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [システム情報](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[電源管理](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[.NET Framework のネットワーク プログラミング](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Windows フォームにおけるネットワーク](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="packaging-and-deployment"></a>パッケージ化と配置

|  UWP  |  Win32 (Windows API) |  WPF  |  Windows フォーム  |
|-------|----------------------|-------|-----------------|
| [アプリのパッケージ化](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[アプリ パッケージ マニフェスト スキーマ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [パッケージの Windows デスクトップ アプリ (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[アプリケーションのインストールとサービス](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows インストーラー](/windows/desktop/msi/windows-installer-portal) |  [パッケージの Windows デスクトップ アプリ (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework およびアプリケーションの配置](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[WPF アプリケーションの配置](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [パッケージの Windows デスクトップ アプリ (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[.NET Framework およびアプリケーションの配置](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Windows フォームの ClickOnce 配置](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
