---
title: Xamarin を使用して Android アプリを作成する
description: Xamarin を使用して Android アプリの作成を開始する方法
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin. forms、xaml、チュートリアル
ms.date: 04/28/2020
ms.openlocfilehash: b6c6a00c430dcc04994c0f8fd537d7c5c2083d4e
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254256"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Xamarin を使用して Android 向けの開発を始める

このガイドでは、Windows で Xamarin を使用して、Android デバイスで動作するクロスプラットフォームアプリを作成する方法について説明します。

この記事では、Xamarin と Visual Studio 2019 を使用して、簡単な Android アプリを作成します。

## <a name="requirements"></a>必要条件

このチュートリアルを使用するには、次のものが必要です。

- Windows 10
- [Visual Studio 2019: Community、Professional、または Enterprise](https://visualstudio.microsoft.com/downloads/) (注を参照)
- Visual Studio 2019 の ".NET を使用したモバイル開発" ワークロード

> [!NOTE]
> このガイドは、Visual Studio 2017 または2019で動作します。 Visual Studio 2017 を使用している場合、Visual Studio の2つのバージョン間の UI の違いにより、一部の命令が正しくないことがあります。

また、Android フォンを使用するか、アプリを実行するように構成されたエミュレーターを使用することもできます。 「 [Android デバイスまたはエミュレーターでのテスト」を](emulator.md)参照してください。

## <a name="create-a-new-xamarinforms-project"></a>新しい Xamarin. フォームプロジェクトを作成する

Visual Studio を起動します。 [ファイル > 新規 > プロジェクト] をクリックして、新しいプロジェクトを作成します。

[新しいプロジェクト] ダイアログで、[ **モバイルアプリ (Xamarin)** ] テンプレートを選択し、[ **次へ**] をクリックします。

プロジェクトに **Timechangerforms** という名前を指定し、[ **作成**] をクリックします。

[新しいクロスプラットフォームアプリ] ダイアログで、[ **空白**] を選択します。 [プラットフォーム] セクションで、[ **Android** ] をオンにし、他のすべてのボックスをオフにします。 **[OK]** をクリックします。

Xamarin は、 **timechangerforms** と **timechangerforms** という2つのプロジェクトを含む新しいソリューションを作成します。

## <a name="create-a-ui-with-xaml"></a>XAML を使った UI の作成

**Timechangerforms** プロジェクトを展開し、 **mainpage.xaml** を開きます。 このファイルの XAML は、TimeChanger を開くときにユーザーに表示される最初の画面を定義します。

TimeChanger の UI はシンプルです。 現在の時刻が表示され、1時間単位で時間を調整するためのボタンがあります。 この例では、垂直方向の StackLayout を使用して、ボタンの上に時間を配置し、水平方向の StackLayout を使用してボタンを並べて配置しています。 このコンテンツは、垂直方向の StackLayout の **水平オプション** と垂直 **オプション** を **"センター andexpand"** に設定することによって、画面の中央に配置されます。

Mainpage.xaml の内容を次のコードに置き換えます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

この時点で、UI は完成です。 ただし、 **UpButton_Clicked** および **DOWNBUTTON_CLICKED** メソッドは XAML で参照されますが、どこにも定義されていないため、timechangerforms はビルドされません。 アプリが実行された場合でも、現在の時刻は表示されません。 次のセクションでは、これらのエラーを修正し、UI に機能を追加します。

## <a name="add-logic-code-with-c"></a>C でロジックコードを追加する#

ソリューションエクスプローラーで Mainpage.xaml を右クリックし、[コードの **表示**] をクリックします。 このファイルには、UI に機能を追加する分離コードが含まれています。

### <a name="set-the-current-time"></a>現在の時刻を設定する

このファイル内のコードは、コントロールの **x:Name** 属性の値を使用して、XAML で宣言されたコントロールを参照できます。 この場合、現在の時刻を表示するラベルが呼び出され `time` ます。

メインスレッドで UI コントロールを更新する必要があります。 別のスレッドから行った変更によって、画面に表示されるコントロールが正しく更新されない場合があります。 このコードは常にメインスレッドで実行されるという保証がないので、 **Begininvokeonmainthread** メソッドを使用して、更新が正しく表示されることを確認してください。 完全な UpdateTimeLabel メソッドを次に示します。

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>1秒ごとに現在の時刻を更新する

この時点で、現在の時刻は、TimeChangerForms が起動された後、最大で1秒で正確になります。 時間を正確に保つために、ラベルを定期的に更新する必要があります。 **タイマー** オブジェクトは、現在の時刻でラベルを更新するコールバックメソッドを定期的に呼び出します。

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>HourOffset の追加

上矢印ボタンと下矢印ボタンは、1時間単位で時間を調整します。 **HourOffset** プロパティを追加して、現在の調整を追跡します。

```csharp
public int HourOffset { get; private set; }
```

ここで、HourOffset プロパティを認識するように UpdateTimeLabel メソッドを更新します。

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>ボタンクリックイベントハンドラーの追加

上矢印ボタンと下矢印ボタンは、HourOffset プロパティをインクリメントまたはデクリメントし、UpdateTimeLabel を呼び出す必要があります。

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

完了すると、MainPage.xaml.cs は次のようになります。

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>アプリを実行する

アプリを実行するには、 **F5** キーを押すか、[デバッグ] をクリック > てデバッグを開始します。 [デバッガーが](emulator.md)どのように構成されているかによって、アプリはデバイスまたはエミュレーターで起動します。

## <a name="related-links"></a>関連リンク

- [Android デバイスまたはエミュレーターでテスト](emulator.md)します。

- [Xamarin Android を使用して Android サンプルアプリを作成する](xamarin-android.md)
