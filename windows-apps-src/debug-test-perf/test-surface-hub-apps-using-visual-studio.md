---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Visual Studio を使った Surface Hub アプリのテスト
description: Visual Studio シミュレーターは、UWP アプリの設計、開発、デバッグ、テストを行える環境を提供します。これには Surface Hub 用に作成されたアプリを含みます。
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a10a8a6e5b4e5188d28c0f75aace50f7465e5f4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163486"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Visual Studio を使った Surface Hub アプリのテスト
Visual Studio シミュレーターは、ユニバーサル Windows プラットフォーム (UWP) アプリの設計、開発、デバッグ、テストを行える環境を提供します。これには Microsoft Surface Hub 用に作成されたアプリを含みます。 シミュレーターでは、Surface Hub と同じユーザー インターフェイスは使用されませんが、Surface Hub の画面サイズと解像度を使用してアプリの外観と動作をテストするために有用です。

シミュレーター ツール全般の詳細については、「[シミュレーターで UWP アプリを実行する](/visualstudio/debugger/run-windows-store-apps-in-the-simulator)」を参照してください。

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Surface Hub の解像度をシミュレーターに追加する
Surface Hub の解像度をシミュレーターに追加するには、次の手順を実行します。

1. *HardwareConfigurations-SurfaceHub55.xml* という名前のファイルに次の XML コードを保存して、55 インチの Surface Hub 用の構成を作成します。  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. *HardwareConfigurations-SurfaceHub84.xml* という名前のファイルに次の XML コードを保存して、84 インチの Surface Hub 用の構成を作成します。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. 2 つの XML ファイルを *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;バージョン番号&gt;\HardwareConfigurations* にコピーします。

   > [!NOTE]
   > このフォルダーにファイルを保存するには、管理者特権が必要です。

4. Visual Studio シミュレーターでアプリを実行します。 パレットの **[解像度の変更]** をクリックし、一覧から Surface Hub の構成を選択します。

    ![Visual Studio シミュレーターの解像度](images/vs-simulator-resolutions.png)

   > [!TIP]
   > Surface Hub のエクスペリエンスをより適切にシミュレートするには、[タブレット モードを有効にします](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet)。

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Visual Studio から Surface Hub デバイスにアプリを展開する
アプリを Surface Hub に手動で展開することは、簡単なプロセスです。

### <a name="enable-developer-mode"></a>開発者モードを有効にする
既定では、Surface Hub ではアプリを Microsoft Store からのみインストールします。 他のソースによって署名されたアプリをインストールするには、開発者モードを有効にする必要があります。

> [!NOTE]
> 開発者モードを有効にした後に、もう一度無効にする場合は、Surface Hub をリセットする必要があります。 デバイスをリセットすると、すべてのローカル ユーザーのファイルと構成が削除され、Windows が再インストールされます。

1. Surface Hub の**スタート** メニューから設定アプリを開きます。

   > [!NOTE]
   > Surface Hub 上の設定アプリにアクセスするには、管理者特権が必要です。

2. **[更新プログラムとセキュリティ] \>[開発者向け]** の順に移動します。

3. **[開発者モード]** を選択し、警告メッセージに同意します。

### <a name="deploy-your-app-from-visual-studio"></a>Visual Studio からアプリを展開する
展開プロセス全般の詳細については、「[UWP アプリの展開とデバッグ](./deploying-and-debugging-uwp-apps.md)」を参照してください。

   > [!NOTE]
   > この機能には Visual Studio 2015 Update 1 以降が必要ですが、最新バージョンの Visual Studio を使用することをお勧めします。 最新の Visual Studio インスタンスでは、最新のすべての開発およびセキュリティの更新プログラムが提供されます。

1. **[デバッグの開始]** ボタンの横にあるデバッグ ターゲットのドロップダウンに移動し、 **[リモート コンピューター]** を選択します。

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio のデバッグ ターゲットのドロップダウン](images/vs-debug-target.png)

2. Surface Hub ハブの IP アドレスを入力します。 **[ユニバーサル]** 認証モードが選択されていることを確認します。

   > [!TIP] 
   > 開発者モードを有効にした後、ようこそ画面で Surface Hub の IP アドレスを確認することができます。

3. **[デバッグの開始 (F5)]** を選択して、Surface Hub 上にアプリを展開してデバッグします。アプリの展開のみを行うには、Ctrl キーを押しながら F5 キーを押します。

   > [!TIP]
   > Surface Hub によってようこそ画面が表示される場合は、任意のボタンを選んで無視してください。