---
title: Windows からの Android デバイスまたはエミュレーターの実行
description: Windows から Android デバイスまたはエミュレーターでアプリをテストし、hyper-v と Windows ハイパーバイザープラットフォーム (WHPX) で仮想化を有効にします。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、エミュレーター、仮想デバイス、デバイスのセットアップ、デバイスの有効化、開発者、構成、仮想化、visual studio、hyper-v、intel、haxm、amd、Windows ハイパーバイザープラットフォーム、WHPX
ms.date: 04/28/2020
ms.openlocfilehash: 57e1d8d62ea7b3918c5e52724c11febcb9f03d72
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161556"
---
# <a name="test-on-an-android-device-or-emulator"></a>Android デバイスまたはエミュレーターでテストする

Windows コンピューター上の実際のデバイスまたはエミュレーターを使用して、Android アプリケーションをテストおよびデバッグする方法はいくつかあります。 このガイドではいくつかの推奨事項について説明しました。

## <a name="run-on-a-real-android-device"></a>実際の Android デバイスで実行する

実際の Android デバイスでアプリを実行するには、まず、開発用に Android デバイスを有効にする必要があります。 Android の開発者オプションは、バージョン4.2 では既定で非表示になっており、Android のバージョンによって異なる場合があります。

### <a name="enable-your-device-for-development"></a>デバイスを開発用に有効にする

最新バージョンの Android 9.0 以降を実行しているデバイスの場合:

1. USB ケーブルを使用して、デバイスを Windows 開発用コンピューターに接続します。 USB ドライバーをインストールするように通知される場合があります。
2. Android デバイスで [ **設定** ] 画面を開きます。
3. [ **About phone**] を選択します。
4. 一番下までスクロールし、[ **ビルド番号** ] を7回タップすると、 **開発者になります。** と表示されます。
5. 前の画面に戻り、[ **システム**] を選択します。
6. [ **詳細設定**] を選択し、一番下までスクロールして、[ **開発者オプション**] をタップします。
7. [ **開発者オプション** ] ウィンドウで、下にスクロールして [ **USB デバッグ**の検索と有効化] を選択します。

以前のバージョンの Android を実行しているデバイスについては、「 [開発用にデバイスを設定](/xamarin/android/get-started/installation/set-up-device-for-development)する」を参照してください。

### <a name="run-your-app-on-the-device"></a>デバイスでのアプリの実行

1. Android Studio ツールバーで、[ **実行構成** ] ドロップダウンメニューからアプリを選択します。

    ![Android Studio 実行構成メニュー](../images/android-run-config-menu.png)

2. [ **ターゲットデバイス** ] ドロップダウンメニューから、アプリを実行するデバイスを選択します。

    ![Android Studio ターゲットデバイスメニュー](../images/android-target-device-menu.png)

3. [Run ▷] を選択します。 これにより、接続されているデバイスでアプリが起動します。

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>エミュレーターを使用した仮想 Android デバイスでのアプリの実行

Windows コンピューターで Android エミュレーターを実行する方法については、IDE (Android Studio、Visual Studio など) に関係なく、仮想化のサポートを有効にすることで、エミュレーターのパフォーマンスを大幅に向上させることができます。

### <a name="enable-virtualization-support"></a>仮想化のサポートを有効にする

Android エミュレーターで仮想デバイスを作成する前に、Hyper-v および Windows ハイパーバイザープラットフォーム (WHPX) の機能をオンにして、仮想化を有効にすることをお勧めします。 これにより、コンピューターのプロセッサはエミュレーターの実行速度を大幅に向上させることができます。

> Hyper-v および Windows ハイパーバイザープラットフォームを実行するには、コンピューターで次のことを行う必要があります。
>
> * 4 GB のメモリを使用可能
> * 64ビットの Intel プロセッサまたは AMD Ryzen CPU と第2レベルのアドレス変換 (SLAT) があること
> * Windows 10 ビルド1803を実行している ([ビルド番号を確認](ms-settings:about)する)
> * グラフィックスドライバーを更新しました (デバイスマネージャー > ディスプレイアダプター > 更新ドライバー)
>
> コンピューターがこの条件を満たすことができない場合は、 [Intel HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) または [AMD ハイパーバイザー](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)を実行できる可能性があります。 詳細については、「 [エミュレーターのパフォーマンスのハードウェア高速化](/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) 」または [Android Studio エミュレーターのドキュメント](https://developer.android.com/studio/run/emulator)を参照してください。

1. コマンドプロンプトを開き、次のコマンドを入力して、コンピューターのハードウェアとソフトウェアが Hyper-v と互換性があることを確認します。 `systeminfo`

    ![コマンドプロンプトでの systeminfo の hyper-v の要件](../images/systeminfo.png)

2. Windows の検索ボックス (左下) で、「windows の機能」と入力します。 検索結果から [ **Windows の機能の有効化または無効化** ] を選択します。

3. [ **Windows の機能** ] の一覧が表示されたら、[ **Hyper-v** (管理ツールとプラットフォームを含む)] と [ **windows ハイパーバイザープラットフォーム**] の両方を選択し、[両方を有効にする] チェックボックスがオンになっていることを確認してから、[ **OK]** を選択します。

4. 求められた場合はコンピューターを再起動します。

### <a name="emulator-for-native-development-with-android-studio"></a>Android Studio を使用したネイティブ開発用のエミュレーター

ネイティブ Android アプリをビルドしてテストする場合は、 [Android Studio を使用する](./native-android.md)ことをお勧めします。 アプリをテストする準備ができたら、次の方法でアプリをビルドして実行できます。

1. Android Studio ツールバーで、[ **実行構成** ] ドロップダウンメニューからアプリを選択します。

    ![Android Studio 実行構成メニュー](../images/android-run-config-menu.png)

2. [ **ターゲットデバイス** ] ドロップダウンメニューから、アプリを実行するデバイスを選択します。

    ![Android Studio ターゲットデバイスメニュー](../images/android-target-device-menu.png)

3. [Run ▷] を選択します。 これにより、 [Android Emulator](https://developer.android.com/studio/run/emulator)が起動します。

> [!TIP]
> アプリがエミュレーターデバイスにインストールされたら、[変更の [適用](https://developer.android.com/studio/run#apply-changes) ] を使用して、新しい apk を作成せずに特定のコードとリソースの変更をデプロイできます。

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Visual Studio を使用したクロスプラットフォーム開発用のエミュレーター

Windows Pc には、多くの [Android emulator オプション](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) が用意されています。 Google の [android エミュレーター](https://developer.android.com/studio/run/emulator)を使用することをお勧めします。これは、最新の android OS イメージと Google Play サービスへのアクセスを提供するためです。

### <a name="install-android-emulator-with-visual-studio"></a>Visual Studio で Android emulator をインストールする

1. まだインストールしていない場合は、 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)をダウンロードします。 Visual Studio インストーラーを使用して、 [ワークロードを変更](/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) し、 **.net ワークロードを**使用したモバイル開発があることを確認します。

2. 新しいプロジェクトを作成します。 [Android Emulator を設定](/xamarin/android/get-started/installation/android-emulator/)したら、 [Android Device Manager](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#requirements)を使用して、さまざまな Android 仮想デバイスを作成、複製、カスタマイズ、および起動できます。 [ツール] メニューから [**ツール**] [Android Android Device Manager] の Android Device Manager を起動  >  **Android**  >  **Android Device Manager**します。

3. Android Device Manager が開いたら、[ **+ 新規** ] を選択して新しいデバイスを作成します。

4. デバイスに名前を付け、ドロップダウンメニューから基本デバイスの種類を選択し、プロセッサと OS のバージョンを仮想デバイス用の他のいくつかの変数と共に選択する必要があります。 詳細については、 [Android Device Manager メイン画面](/xamarin/android/get-started/installation/android-emulator/device-manager?pivots=windows&tabs=windows#main-screen)を参照してください。

5. Visual Studio のツールバーで、[ **デバッグ** ] (アプリの開始後にエミュレーター内で実行されているアプリケーションプロセスにアタッチ) または [ **リリース** モード] (デバッガーを無効にする) を選択します。 次に、[デバイス] ドロップダウンメニューから仮想デバイスを選択し、[Play] \ ( **再生** \) ボタン▷を選択してエミュレーターでアプリケーションを実行します。

    ![Visual Studio の起動 Android Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>その他のリソース

- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

- [Windows Defender の除外を追加してパフォーマンスを向上させる](defender-settings.md)