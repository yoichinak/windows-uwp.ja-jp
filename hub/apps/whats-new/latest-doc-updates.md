---
description: Windows 開発者向けドキュメントへの最新の追加事項について説明します。
title: Windows 開発者向けドキュメントの最新の更新
ms.topic: article
ms.date: 2/10/2021
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 385d6b0a2cb150c13f75183fd3c5592c0f6f9868
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824066"
---
# <a name="latest-updates-to-the-windows-developer-docs"></a>Windows 開発者向けドキュメントの最新の更新

Windows 開発者向けドキュメントは、新しい強化された情報とコンテンツによって定期的に更新されます。 2021 年 2 月 10 日時点での変更の概要を以下に示します。

注: Windows 10 ビルド19041 (2004 とも呼ばれます) の一部として追加された API の特定のリストについては、[こちらのリスト](/windows/uwp/whats-new/windows-10-build-19041-api-diff)を参照してください。

Windows 開発者向けドキュメントの最新情報について、またはコメントや質問がある場合、弊社の Twitter ハンドルは [@WindowsDocs](https://twitter.com/windowsdocs) です。

ドキュメント セット全体で配慮にかけた言葉の使用状況を検索し、削除し続けてきました。

Microsoft ドキュメントに寄稿する方法の詳細については、[投稿のガイド](/contribute/)を参照してください。

今月のハイライトには以下が含まれます。

### <a name="new-content"></a>新しい内容:


* [C++/WinRT から C# への移植](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)
* [チュートリアル:C#/WinRT コンポーネントを作成し、C++/WinRT から使用する](/windows/uwp/csharp-winrt/create-windows-runtime-component-cswinrt)
* [C#/WinRT コンポーネント エラーの診断](/windows/uwp/csharp-winrt/authoring-diagnostics)
* ビデオ ショー「[タブとスペース](https://channel9.msdn.com/Shows/Tabs-vs-Spaces)」の新しいエピソード




### <a name="updated-topics"></a>更新されたトピック

* C++/WinRT サンプル コードを使用した、ナビゲーション コントロール トピックの書き直し。 
    * [Windows アプリでのナビゲーション履歴と前に戻る移動](/windows/uwp/design/basics/navigation-history-and-backwards-navigation)
    * [ナビゲーション ビュー](/windows/uwp/design/controls-and-patterns/navigationview)
* [上位レベル シェーダー言語 (HLSL)](/windows/win32/direct3dhlsl/dx-graphics-hlsl)
* [WinHttpQueryDataAvailable](/windows/win32/api/winhttp/nf-winhttp-winhttpquerydataavailable)
* [.NET Core アプリでの機能フラグの使用に関するチュートリアル |Microsoft Docs](/azure/azure-app-configuration/use-feature-flags-dotnet-core)
* [WinUI 2](../winui/winui2/index.md) および [WinUI 3](../winui/winui3/index.md) のドキュメントに対する継続的な更新。
* WSL と Windows ターミナルについて説明している「[Windows 10 で開発環境を設定する](../../dev-environment/overview.md)」に対する定期的な更新。
* Windows ターミナルの新しい[設定 UI](/windows/terminal/customize-settings/startup) に関する情報。

次のリファレンス トピックでは、過去 1 か月に重要な更新が行われました。

## <a name="winrt-conceptual"></a>WinRT の概念

<ul>
<li><a href="/windows/uwp/composition/relation-animations">関係ベース アニメーション</a></li>
<li><a href="/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp">C# から C++/WinRT への移行</a></li>
<li><a href="/windows/uwp/csharp-winrt/authoring-diagnostics">C#/WinRT コンポーネント エラーの診断</a></li>
<li><a href="/windows/uwp/csharp-winrt/create-windows-runtime-component-cswinrt">C#/WinRT コンポーネントを作成し、C++/WinRT から使用する</a></li>
<li><a href="/windows/uwp/csharp-winrt/index">C#/WinRT</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-api-core">Windows デバイス ポータル コア REST API リファレンス</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-desktop">デスクトップ用 Windows デバイス ポータル</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-mobile">モバイル用 Windows デバイス ポータル</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal">Windows Device Portal の概要</a></li>
<li><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">設定と他のアプリ データを保存して取得する</a></li>
<li><a href="/windows/uwp/design/basics/navigation-history-and-backwards-navigation">ナビゲーション履歴と前に戻る移動</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/infobar">InfoBar</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/media-playback">メディア プレーヤー</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/navigationview">ナビゲーション ビュー</a></li>
<li><a href="/windows/uwp/design/input/cortana-deep-link-into-your-app">Cortana のバックグラウンド アプリからフォアグラウンド アプリへのディープ リンク - Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-design-guidelines">Cortana の設計ガイドライン - Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists">Cortana VCD の語句一覧の動的な変更 - Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-interact-with-a-background-app">Cortana でのバックグラウンド アプリの操作- Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-interactions">Windows アプリでの Cortana の操作</a></li>
<li><a href="/windows/uwp/design/input/cortana-launch-a-background-app-with-voice-commands">Cortana の音声コマンドを使ったバックグラウンド アプリのアクティブ化 | Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-launch-a-foreground-app-with-voice-commands">Cortana の音声コマンドを使ったフォアグラウンド アプリのアクティブ化 - Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/design/input/cortana-support-natural-language-voice-commands">Cortana でのより自然な音声コマンドのサポート - Cortana UWP の設計と開発</a></li>
<li><a href="/windows/uwp/gaming/about-the-uwp-user-interface-and-directx">アプリ オブジェクトと DirectX</a></li>
<li><a href="/windows/uwp/graphics-concepts/graphics-pipeline">グラフィックス パイプライン</a></li>
<li><a href="/windows/uwp/monetize/get-the-stack-trace-for-an-error-in-your-desktop-application">デスクトップ アプリケーションのエラーに関するスタック トレースの取得</a></li>
<li><a href="/windows/uwp/monetize/manage-add-on-submissions">アドオンの申請の管理</a></li>
<li><a href="/windows/uwp/monetize/manage-add-ons">アドオンの管理</a></li>
<li><a href="/windows/uwp/monetize/manage-app-submissions">アプリの申請の管理</a></li>
<li><a href="/windows/uwp/monetize/manage-flight-submissions">パッケージ フライトの申請の管理</a></li>
<li><a href="/windows/uwp/monetize/view-and-grant-products-from-a-service">サービスによる製品の権利の管理</a></li>
<li><a href="/windows/uwp/publish/set-custom-permissions-for-account-users">アカウント ユーザーの役割またはカスタムのアクセス許可の設定</a></li>
<li><a href="/windows/uwp/xbox-apps/development-lanes-unity-versioning">Unity - UWP プロジェクトのバージョン管理</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-deployinfo-api">デバイス ポータル展開情報 API リファレンス</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-networkcredentials-api">デバイス ポータル ネットワーク資格情報 API リファレンス</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-remoteinput-api">Device Portal リモコン入力 API リファレンス</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-remoteinput-controllers-api">デバイス ポータル コントローラー API リファレンス</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-sshpins-api">Device Portal の SSH ピン API のリファレンス</a></li>
<li><a href="/windows/uwp/xbox-apps/wdp-media-capture-api">メディア キャプチャ API のリファレンス</a></li>
</ul>

## <a name="win32-conceptual"></a>Win32 の概念

<ul>
<li><a href="/windows/desktop/Bits/using-windows-powershell-to-create-bits-transfer-jobs">Windows PowerShell を使用した BITS 転送ジョブの作成</a></li>
<li><a href="/windows/desktop/Bits/using-winrm-windows-powershell-cmdlets-to-manage-bits-transfer-jobs">WinRM Windows PowerShell コマンドレットを使った BITS 転送ジョブの管理</a></li>
<li><a href="/windows/desktop/CIMWin32Prov/win32-optionalfeature">Win32_OptionalFeature クラス</a></li>
<li><a href="/windows/desktop/CIMWin32Prov/win32-pnpdeviceproperty">Win32_PnPDeviceProperty クラス</a></li>
<li><a href="/windows/desktop/Controls/toolbar-standard-button-image-index-values">ツール バーの標準ボタン イメージのインデックス値 (CommCtrl. h)</a></li>
<li><a href="/windows/desktop/Direct2D/border">境界線の効果</a></li>
<li><a href="/windows/desktop/Direct2D/profiling-directx-applications">DirectX アプリのプロファイリング</a></li>
<li><a href="/windows/desktop/DirectWrite/dwritecore-overview">DWriteCore の概要</a></li>
<li><a href="/windows/desktop/DxTechArts/installing-and-using-input-method-editors">入力方式エディターのインストールと使用</a></li>
<li><a href="/windows/desktop/HyperV_v2/getting-the-virtual-system-dns-name">仮想マシンの DNS 名の取得</a></li>
<li><a href="/windows/desktop/LearnWin32/appendix--matrix-transforms">付録マトリックスの変換</a></li>
<li><a href="/windows/desktop/Midl/-target">/target スイッチ</a></li>
<li><a href="/windows/desktop/Midl/declare-guid">declare_guid 属性</a></li>
<li><a href="/windows/desktop/Midl/sh-composition">sh_composition キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-event">sh_event キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-file">sh_file キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-job">sh_job キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-mutex">sh_mutex キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-pipe">sh_pipe キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-process">sh_process キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-reg-key">sh_reg_key キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-section">sh_section キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-semaphore">sh_semaphore キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-socket">sh_socket キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-thread">sh_thread キーワード</a></li>
<li><a href="/windows/desktop/Midl/sh-token">sh_token キーワード</a></li>
<li><a href="/windows/desktop/Midl/system-handle">system_handle 属性</a></li>
<li><a href="/windows/desktop/Msi/patching-per-user-managed-applications">ユーザーごとのマネージド アプリケーションのパッチ適用</a></li>
<li><a href="/windows/desktop/Multimedia/buffered-services">バッファーに格納されるサービス</a></li>
<li><a href="/windows/desktop/NDF/using-network-monitor-to-view-etl-files">ネットワーク モニターを使った ETL ファイルの表示</a></li>
<li><a href="/windows/desktop/OpenGL/glfogf">glFogf 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogfv">glFogfv 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogi">glFogi 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogiv">glFogiv 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap1d">glMap1d 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap1f">glMap1f 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap2d">glMap2d 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap2f">glMap2f 関数 (Gl.h)</a></li>
<li><a href="/windows/desktop/ProcThread/using-thread-local-storage">スレッド ローカル ストレージの使用</a></li>
<li><a href="/windows/desktop/SecAuthN/protocols-in-tls-ssl--schannel-ssp-">TLS/SSL のプロトコル (Schannel SSP)</a></li>
<li><a href="/windows/desktop/SecAuthN/schannel-cipher-suites-in-windows-vista">Windows Vista の TLS 暗号スイート</a></li>
<li><a href="/windows/desktop/SecCrypto/alternatives-to-using-capicom">CAPICOM の使用に代わる方法</a></li>
<li><a href="/windows/desktop/SecCrypto/cryptuidlgselectcertificate">CryptUIDlgSelectCertificate 関数</a></li>
<li><a href="/windows/desktop/Sync/synchronization-and-multiprocessor-issues">同期とマルチプロセッサに関する問題</a></li>
<li><a href="/windows/desktop/VDS/volume-and-lun-binding">ボリュームと LUN のバインド</a></li>
<li><a href="/windows/desktop/VSS/setting-vss-restore-methods">VSS の復元方法の設定</a></li>
<li><a href="/windows/desktop/Win7AppQual/preventing-hangs-in-windows-applications">Windows アプリケーションでのハングの防止</a></li>
<li><a href="/windows/desktop/WinProg/large-integer-structures">大きな整数構造体</a></li>
<li><a href="/windows/desktop/WinProg64/shared-registry-keys">WOW64 の影響を受けるレジストリ キー</a></li>
<li><a href="/windows/desktop/WinRM/winrm-powershell-commandlets">WinRM Windows PowerShell コマンド クラスのマネージド参照</a></li>
<li><a href="/windows/desktop/WinSock/ipproto-ip-socket-options">IPPROTO_IP ソケット オプション</a></li>
<li><a href="/windows/desktop/WinSock/ipproto-ipv6-socket-options">IPPROTO_IPV6 ソケット オプション</a></li>
<li><a href="/windows/desktop/WinSock/winsock-ioctls">Winsock IOCTL (Winsock2.h)</a></li>
<li><a href="/windows/desktop/application-installing-and-servicing">アプリケーションのインストールとサービス</a></li>
<li><a href="/windows/desktop/cossdk/configuring-security-for-library-applications">ライブラリ アプリケーションのセキュリティの構成</a></li>
<li><a href="/windows/desktop/delivery_optimization/ibackgroundcopymanager-createjob">IBackgroundCopyManager CreateJob メソッド (Deliveryoptimization.h)</a></li>
<li><a href="/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Direct3D 12 の基本的なコンポーネントの作成</a></li>
<li><a href="/windows/desktop/direct3d12/d3d12decomposesubresource">D3D12DecomposeSubresource 関数 (D3dx12.h)</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_depth_space_order">DML_DEPTH_SPACE_ORDER</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_graph_edge_type">DML_GRAPH_EDGE_TYPE</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_graph_node_type">DML_GRAPH_NODE_TYPE</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_depth_to_space1_operator_desc">DML_DEPTH_TO_SPACE1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_desc">DML_GRAPH_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_edge_desc">DML_GRAPH_EDGE_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_node_desc">DML_GRAPH_NODE_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_max_pooling_grad_operator_desc">DML_MAX_POOLING_GRAD_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_random_generator_operator_desc">DML_RANDOM_GENERATOR_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_space_to_depth1_operator_desc">DML_SPACE_TO_DEPTH1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_top_k1_operator_desc">DML_TOP_K1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/dynamic-indexing-using-hlsl-5-1">HLSL 5.1 を使用した動的インデックス作成</a></li>
<li><a href="/windows/desktop/direct3d9/d3dxboxboundprobe">D3DXBoxBoundProbe 関数 (D3DX9Mesh.h)</a></li>
<li><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-state-object">状態オブジェクト</a></li>
<li><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl">上位レベル シェーダー言語 (HLSL)</a></li>
<li><a href="/windows/desktop/directx-sdk--august-2009-">DirectX SDK の場所</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/columnvalueofstruct-t-class">ColumnValueOfStruct(T) クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentcorruptionexception-class">EsentCorruptionException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentfatalexception-class">EsentFatalException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentfragmentationexception-class">EsentFragmentationException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentinconsistentexception-class">EsentInconsistentException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentmemoryexception-class">EsentMemoryException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentobsoleteexception-class">EsentObsoleteException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentoperationexception-class">EsentOperationException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentstateexception-class">EsentStateException クラス</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentusageexception-class">EsentUsageException クラス</a></li>
<li><a href="/windows/desktop/gdiplus/-gdiplus-flatapi-flat">GDI+ フラット API</a></li>
<li><a href="/windows/desktop/inputdev/using-raw-input">未加工の入力の使用</a></li>
<li><a href="/windows/desktop/inputmsg/wm-pointerup">WM_POINTERUP メッセージ</a></li>
<li><a href="/windows/desktop/lwef/animations">アニメーション</a></li>
<li><a href="/windows/desktop/lwef/creating-animations">アニメーションの作成</a></li>
<li><a href="/windows/desktop/lwef/iagentcharacterex--getanimationnames">IAgentCharacterEx GetAnimationNames</a></li>
<li><a href="/windows/desktop/lwef/toolbar-buttons-">ツール バー ボタン (Microsoft エージェント文字エディター)</a></li>
<li><a href="/windows/desktop/lwef/toolbar-buttons">ツール バー ボタン (言語情報サウンド編集ツール)</a></li>
<li><a href="/windows/desktop/medfound/direct3d-video-motion-estimation">Direct3D ビデオ モーションの推定</a></li>
<li><a href="/windows/desktop/shell/band-objects">カスタムのエクスプローラー バー、ツール バンド、デスク バンドの作成</a></li>
<li><a href="/windows/desktop/tablet/security-and-trust">セキュリティと信頼</a></li>
<li><a href="/windows/desktop/tablet/using-the-managed-library">マネージド ライブラリの使用</a></li>
<li><a href="/windows/desktop/updateorchestrator/index">Orchestrator API の更新</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-hasmoratoriumpassed">IUniversalOrchestrator::HasMoratoriumPassed</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-iuniversalorchestrator">IUniversalOrchestrator インターフェイス</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-schedulework">IUniversalOrchestrator::ScheduleWork</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-workcompleted">IUniversalOrchestrator::WorkCompleted</a></li>
<li><a href="/windows/desktop/uxguide/cmd-toolbars">[ツール バー]</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-balloons">バルーン</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-command-buttons">[コマンド ボタン]</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-command-links">コマンド リンク</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-drop">ドロップダウン リストのコンボ ボックス</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-group-boxes">グループ ボックス</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-search-boxes">検索ボックス</a></li>
<li><a href="/windows/desktop/uxguide/glossary">用語集 (設計の基本)</a></li>
<li><a href="/windows/desktop/uxguide/inter-mouse">マウスとポインター</a></li>
<li><a href="/windows/desktop/uxguide/inter-pen">ペン</a></li>
<li><a href="/windows/desktop/uxguide/inter-touch">タッチ</a></li>
<li><a href="/windows/desktop/uxguide/mess-confirm">確認</a></li>
<li><a href="/windows/desktop/uxguide/mess-error">エラー メッセージ (設計の基本)</a></li>
<li><a href="/windows/desktop/uxguide/vis-animations">アニメーションと切り替え</a></li>
<li><a href="/windows/desktop/uxguide/vis-graphic">グラフィック要素</a></li>
<li><a href="/windows/desktop/uxguide/vis-icons">アイコン (設計の基本)</a></li>
<li><a href="/windows/desktop/uxguide/vis-std-icons">標準アイコン</a></li>
<li><a href="/windows/desktop/uxguide/win-wizards">ウィザード</a></li>
<li><a href="/windows/desktop/uxguide/winenv-notification">通知領域</a></li>
<li><a href="/windows/desktop/w8cookbook/secured-boot">起動時マルウェア対策</a></li>
<li><a href="/windows/desktop/wcs/a">A</a></li>
<li><a href="/windows/desktop/wcs/about-windows-color-system-version-1-0">Windows カラー システム バージョン1.0 について</a></li>
<li><a href="/windows/desktop/wcs/additive-primary-colors">加法混色の原色</a></li>
<li><a href="/windows/desktop/wcs/advanced-functions-for-use-outside-of-a-device-context">デバイス コンテキストの外部で使用する高度な関数</a></li>
<li><a href="/windows/desktop/wcs/alphabetical-list-of-all-wcs-functions">すべての WCS 関数のアルファベット順の一覧</a></li>
<li><a href="/windows/desktop/wcs/b">B</a></li>
<li><a href="/windows/desktop/wcs/basic-color-management-concepts">基本的な色の管理の概念</a></li>
<li><a href="/windows/desktop/wcs/basic-functions-for-use-within-a-device-context">デバイス コンテキスト内で使用する基本的な関数</a></li>
<li><a href="/windows/desktop/wcs/c">C</a></li>
<li><a href="/windows/desktop/wcs/cmm-transform-creation-flags">CMM 変換作成フラグ</a></li>
<li><a href="/windows/desktop/wcs/cmy-and-cmyk-color-spaces">CMY と CMYK の色空間</a></li>
<li><a href="/windows/desktop/wcs/color-conversion-and-color-matching">色の変換とカラー マッチング</a></li>
<li><a href="/windows/desktop/wcs/color-in-imaging">イメージングの色</a></li>
<li><a href="/windows/desktop/wcs/color-space-constants">色空間定数</a></li>
<li><a href="/windows/desktop/wcs/color-space-type-identifiers">色空間の種類識別子</a></li>
<li><a href="/windows/desktop/wcs/color-spaces">色空間</a></li>
<li><a href="/windows/desktop/wcs/common-color-messages">一般的な色のメッセージ</a></li>
<li><a href="/windows/desktop/wcs/d">D</a></li>
<li><a href="/windows/desktop/wcs/describing-color">色の記述</a></li>
<li><a href="/windows/desktop/wcs/device-calibration-and-characterization-functions">デバイスの調整と特性化の機能</a></li>
<li><a href="/windows/desktop/wcs/device-dependent-color-spaces">デバイスに依存する色空間</a></li>
<li><a href="/windows/desktop/wcs/device-independent-color-spaces">デバイスに依存しない色空間</a></li>
<li><a href="/windows/desktop/wcs/enumerations">列挙型</a></li>
<li><a href="/windows/desktop/wcs/error-codes-specific-to-wcs">WCS に固有のエラー コード</a></li>
<li><a href="/windows/desktop/wcs/functions">関数</a></li>
<li><a href="/windows/desktop/wcs/further-information">詳細情報</a></li>
<li><a href="/windows/desktop/wcs/g">G</a></li>
<li><a href="/windows/desktop/wcs/h">H</a></li>
<li><a href="/windows/desktop/wcs/hls-color-spaces">HLS 色空間</a></li>
<li><a href="/windows/desktop/wcs/hsv-color-spaces">HSV 色空間</a></li>
<li><a href="/windows/desktop/wcs/human-color-perception">人間による色認識</a></li>
<li><a href="/windows/desktop/wcs/icmprogressproccallback">ICMProgressProcCallback コールバック関数</a></li>
<li><a href="/windows/desktop/wcs/interfaces">インターフェイス</a></li>
<li><a href="/windows/desktop/wcs/l">L</a></li>
<li><a href="/windows/desktop/wcs/macros-for-cmyk-values-and-colors">CMYK の値と色のマクロ</a></li>
<li><a href="/windows/desktop/wcs/obsolete-wcs-functions">廃止された WCS 関数</a></li>
<li><a href="/windows/desktop/wcs/p">P</a></li>
<li><a href="/windows/desktop/wcs/profile-management-functions">プロファイル管理関数</a></li>
<li><a href="/windows/desktop/wcs/r">R</a></li>
<li><a href="/windows/desktop/wcs/reference">リファレンス</a></li>
<li><a href="/windows/desktop/wcs/rendering-intents">レンダリングの目的</a></li>
<li><a href="/windows/desktop/wcs/rgb-color-spaces">RGB 色空間</a></li>
<li><a href="/windows/desktop/wcs/s">S</a></li>
<li><a href="/windows/desktop/wcs/security-considerations--windows-color-system">セキュリティに関する考慮事項: Windows カラー システム</a></li>
<li><a href="/windows/desktop/wcs/srgb--a-standard-color-space">sRGB: 標準の色空間</a></li>
<li><a href="/windows/desktop/wcs/structures">構造体</a></li>
<li><a href="/windows/desktop/wcs/subtractive-primary-colors">減法混色の原色</a></li>
<li><a href="/windows/desktop/wcs/t">T</a></li>
<li><a href="/windows/desktop/wcs/using-color-management-modules--cmm">カラー管理モジュール (CMM) の使用</a></li>
<li><a href="/windows/desktop/wcs/using-color-management-on-the-internet">インターネットでの色の管理の使用</a></li>
<li><a href="/windows/desktop/wcs/using-device-profiles-with-wcs">WCS でのデバイス プロファイルの使用</a></li>
<li><a href="/windows/desktop/wcs/using-gdi-functions-with-wcs">WCS での GDI 関数の使用</a></li>
<li><a href="/windows/desktop/wcs/using-structures-in-wcs-1-0">WCS 1.0 での構造体の使用</a></li>
<li><a href="/windows/desktop/wcs/using-the-color-mapping-process-with-wcs">WCS でのカラー マッピング プロセスの使用</a></li>
<li><a href="/windows/desktop/wcs/using-wcs-1-0">WCS 1.0 の使用</a></li>
<li><a href="/windows/desktop/wcs/w">W</a></li>
<li><a href="/windows/desktop/wcs/wcs-1-0-availability">WCS 1.0 の可用性</a></li>
<li><a href="/windows/desktop/wcs/wcs-1-0-glossary">WCS 1.0 用語集</a></li>
<li><a href="/windows/desktop/wcs/wcs-calibration-schema">WCS 調整スキーマ</a></li>
<li><a href="/windows/desktop/wcs/wcs-color-appearance-model-profile-schema-and-algorithm">WCS カラー表示モデル プロファイルのスキーマとアルゴリズム</a></li>
<li><a href="/windows/desktop/wcs/wcs-color-device-model-profile-schema-and-algorithms">WCS カラー デバイス モデル プロファイルのスキーマとアルゴリズム</a></li>
<li><a href="/windows/desktop/wcs/wcs-constants">WCS 定数</a></li>
<li><a href="/windows/desktop/wcs/wcs-functions-for-color-management-modules--cmms--to-implement">実装するカラー管理モジュール (CMM) の WCS 関数</a></li>
<li><a href="/windows/desktop/wcs/wcs-gamut-map-model-profile-schema-and-algorithms">WCS 色域マップ モデル プロファイルのスキーマとアルゴリズム</a></li>
<li><a href="/windows/desktop/wcs/wcs-registry-keys">WCS レジストリ キー</a></li>
<li><a href="/windows/desktop/wcs/wcs-transform-creation-algorithms">WCS 変換作成アルゴリズム</a></li>
<li><a href="/windows/desktop/wcs/what-s-new-in-version-1-0-of-wcs">WCS のバージョン 1.0 の新機能</a></li>
<li><a href="/windows/desktop/wcs/what-s-new-in-windows-vista">Windows Vista の新機能</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system-common-profile-types-schema--versioning-and-localization-strategies">Windows カラー システムの一般的なプロファイルの種類のスキーマ、バージョン管理、およびローカリゼーションの戦略</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system-schemas-and-algorithms">Windows カラー システムのスキーマとアルゴリズム</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system">Windows カラー システム</a></li>
<li><a href="/windows/desktop/wmformat/drm-for-more-information">詳細情報 (Microsoft Windows Media DRM クライアント)</a></li>
<li><a href="/windows/desktop/wmformat/for-more-information">詳細情報 (Windows Media Format SDK)</a></li>
</ul>

## <a name="win32-api-reference"></a>Win32 API リファレンス

<ul>
<li><a href="/windows/win32/api/cimfs/index">cimfs </a></li>
<li><a href="/windows/win32/api/commctrl/nf-commctrl-loadiconmetric">LoadIconMetric 関数 (commctrl.h) </a></li>
<li><a href="/windows/win32/api/d3d12/ne-d3d12-d3d12_pipeline_state_subobject_type">D3D12_PIPELINE_STATE_SUBOBJECT_TYPE (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/nf-d3d12-id3d12device2-createpipelinestate">ID3D12Device2::CreatePipelineState (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/nf-d3d12-id3d12pipelinelibrary1-loadpipeline">ID3D12PipelineLibrary1::LoadPipeline (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/ns-d3d12-d3d12_pipeline_state_stream_desc">D3D12_PIPELINE_STATE_STREAM_DESC (d3d12.h) </a></li>
<li><a href="/windows/win32/api/dxgi/nf-dxgi-idxgifactory-makewindowassociation">IDXGIFactory::MakeWindowAssociation (dxgi.h) </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pbmcallbackfn">PBMCALLBACKFN </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pcmscallbacka">PCMSCALLBACKA </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pcmscallbackw">PCMSCALLBACKW </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-bmformat">BMFORMAT </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colordatatype">COLORDATATYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colorprofilesubtype">COLORPROFILESUBTYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colorprofiletype">COLORPROFILETYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colortype">COLORTYPE </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-associatecolorprofilewithdevicea">AssociateColorProfileWithDeviceA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-associatecolorprofilewithdevicew">AssociateColorProfileWithDeviceW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-checkbitmapbits">CheckBitmapBits </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-checkcolors">CheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-closecolorprofile">CloseColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcheckcolors">CMCheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcheckcolorsingamut">CMCheckColorsInGamut </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmconvertcolornametoindex">CMConvertColorNameToIndex </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmconvertindextocolorname">CMConvertIndexToColorName </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatedevicelinkprofile">CMCreateDeviceLinkProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatemultiprofiletransform">CMCreateMultiProfileTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreateprofile">CMCreateProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreateprofilew">CMCreateProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatetransform">CMCreateTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatetransformw">CMCreateTransformW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmdeletetransform">CMDeleteTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetinfo">CMGetInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetnamedprofileinfo">CMGetNamedProfileInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetps2colorrenderingdictionary">CMGetPS2ColorRenderingDictionary </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetps2colorspacearray">CMGetPS2ColorSpaceArray </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmisprofilevalid">CMIsProfileValid </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmtranslatecolors">CMTranslateColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmtranslatergbsext">CMTranslateRGBsExt </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetdisplaycurrentstateid">ColorAdapterGetDisplayCurrentStateID </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetdisplaytransformdata">ColorAdapterGetDisplayTransformData </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetsystemmodifywhitepointcaps">ColorAdapterGetSystemModifyWhitePointCaps </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterregisteroemcolorservice">ColorAdapterRegisterOEMColorService </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterunregisteroemcolorservice">ColorAdapterUnregisterOEMColorService </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterupdatedeviceprofile">ColorAdapterUpdateDeviceProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterupdatedisplaygamma">ColorAdapterUpdateDisplayGamma </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofileadddisplayassociation">ColorProfileAddDisplayAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplaydefault">ColorProfileGetDisplayDefault </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplaylist">ColorProfileGetDisplayList </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplayuserscope">ColorProfileGetDisplayUserScope </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofileremovedisplayassociation">ColorProfileRemoveDisplayAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilesetdisplaydefaultassociation">ColorProfileSetDisplayDefaultAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createcolortransforma">CreateColorTransformA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createcolortransformw">CreateColorTransformW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createdevicelinkprofile">CreateDeviceLinkProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createmultiprofiletransform">CreateMultiProfileTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createprofilefromlogcolorspacea">CreateProfileFromLogColorSpaceA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createprofilefromlogcolorspacew">CreateProfileFromLogColorSpaceW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-deletecolortransform">DeleteColorTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-enumcolorprofilesa">EnumColorProfilesA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-enumcolorprofilesw">EnumColorProfilesW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcmminfo">GetCMMInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileelement">GetColorProfileElement </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileelementtag">GetColorProfileElementTag </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofilefromhandle">GetColorProfileFromHandle </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileheader">GetColorProfileHeader </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcountcolorprofileelements">GetCountColorProfileElements </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getnamedprofileinfo">GetNamedProfileInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getps2colorrenderingdictionary">GetPS2ColorRenderingDictionary </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getps2colorspacearray">GetPS2ColorSpaceArray </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getstandardcolorspaceprofilea">GetStandardColorSpaceProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getstandardcolorspaceprofilew">GetStandardColorSpaceProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-installcolorprofilea">InstallColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-installcolorprofilew">InstallColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-iscolorprofiletagpresent">IsColorProfileTagPresent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-iscolorprofilevalid">IsColorProfileValid </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-opencolorprofilea">OpenColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-opencolorprofilew">OpenColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-selectcmm">SelectCMM </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelement">SetColorProfileElement </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelementreference">SetColorProfileElementReference </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelementsize">SetColorProfileElementSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileheader">SetColorProfileHeader </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setupcolormatchinga">SetupColorMatchingA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setupcolormatchingw">SetupColorMatchingW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-translatebitmapbits">TranslateBitmapBits </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-translatecolors">TranslateColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-unregistercmma">UnregisterCMMA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-unregistercmmw">UnregisterCMMW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsassociatecolorprofilewithdevice">WcsAssociateColorProfileWithDevice </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcscheckcolors">WcsCheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcscreateiccprofile">WcsCreateIccProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsdisassociatecolorprofilefromdevice">WcsDisassociateColorProfileFromDevice </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsenumcolorprofiles">WcsEnumColorProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsenumcolorprofilessize">WcsEnumColorProfilesSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetcalibrationmanagementstate">WcsGetCalibrationManagementState </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultcolorprofile">WcsGetDefaultColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultcolorprofilesize">WcsGetDefaultColorProfileSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultrenderingintent">WcsGetDefaultRenderingIntent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetuseperuserprofiles">WcsGetUsePerUserProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsopencolorprofilea">WcsOpenColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsopencolorprofilew">WcsOpenColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetcalibrationmanagementstate">WcsSetCalibrationManagementState </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetdefaultcolorprofile">WcsSetDefaultColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetdefaultrenderingintent">WcsSetDefaultRenderingIntent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetuseperuserprofiles">WcsSetUsePerUserProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcstranslatecolors">WcsTranslateColors </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-cmykcolor">CMYKCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-displaystateid">DisplayStateID </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-displaytransformlut">DisplayTransformLut </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-enumtypea">ENUMTYPEA </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-enumtypew">ENUMTYPEW </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-generic3channel">GENERIC3CHANNEL </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-graycolor">GRAYCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-hificolor">HiFiCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-labcolor">LabCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-named_profile_info">NAMED_PROFILE_INFO </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-namedcolor">NAMEDCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-profile">PROFILE </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-profileheader">PROFILEHEADER </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-rgbcolor">RGBCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-xyypoint">XYYPoint </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-xyzcolor">XYZCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-yxycolor">YxyCOLOR </a></li>
<li><a href="/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminateprocess">TerminateProcess 関数 (processthreadsapi.h) </a></li>
<li><a href="/windows/win32/api/projectedfslib/ns-projectedfslib-prj_callbacks">PRJ_CALLBACKS (projectedfslib.h) </a></li>
<li><a href="/windows/win32/api/propidl/ns-propidl-propvariant">PROPVARIANT (propidl.h) </a></li>
<li><a href="/windows/win32/api/shellapi/ns-shellapi-shellexecuteinfoa">SHELLEXECUTEINFOA (shellapi.h) </a></li>
<li><a href="/windows/win32/api/shellapi/ns-shellapi-shellexecuteinfow">SHELLEXECUTEINFOW (shellapi.h) </a></li>
<li><a href="/windows/win32/api/shobjidl_core/nf-shobjidl_core-shassocenumhandlers">SHAssocEnumHandlers 関数 (shobjidl_core.h) </a></li>
<li><a href="/windows/win32/api/winbase/nf-winbase-createprocesswithlogonw">CreateProcessWithLogonW 関数 (winbase.h) </a></li>
<li><a href="/windows/win32/api/winbase/nf-winbase-createprocesswithtokenw">CreateProcessWithTokenW 関数 (winbase.h) </a></li>
<li><a href="/windows/win32/api/winsock2/nf-winsock2-recvfrom">recvfrom 関数 (winsock2.h) </a></li>
<li><a href="/windows/win32/api/wmp/nf-wmp-iwmpsettings-get_baseurl">IWMPSettings::get_baseURL (wmp.h) </a></li>
</ul>

## <a name="uwp-api-reference"></a>UWP API リファレンス

<ul>
<li><a href="/uwp/api/windows.ui.xaml.debugsettings.enableframeratecounter">Windows.UI.Xaml.DebugSettings.EnableFrameRateCounter</a></li>
</ul>