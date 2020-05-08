---
title: Xamarin Android で簡単な Android アプリを作成する
description: Xamarin android で Android アプリの作成を開始する方法
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: android、windows、xamarin android、チュートリアル、xaml
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255207"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Xamarin を使用して Android 向けの開発を始める

このガイドでは、Windows で Xamarin Android を使用して、Android デバイスで動作するクロスプラットフォームアプリを作成する方法について説明します。

この記事では、Xamarin Android と Visual Studio 2019 を使用して、簡単な Android アプリを作成します。

## <a name="requirements"></a>必要条件

このチュートリアルを使用するには、次のものが必要です。

- Windows 10
- [Visual Studio 2019: Community、Professional、または Enterprise](https://visualstudio.microsoft.com/downloads/) (注を参照)
- Visual Studio 2019 の ".NET を使用したモバイル開発" ワークロード

> [!NOTE]
> このガイドは、Visual Studio 2017 または2019で動作します。 Visual Studio 2017 を使用している場合、Visual Studio の2つのバージョン間の UI の違いにより、一部の命令が正しくないことがあります。

また、Android フォンを使用するか、アプリを実行するように構成されたエミュレーターを使用することもできます。 「 [Android emulator の構成](emulator.md)」を参照してください。

## <a name="create-a-new-xamarinandroid-project"></a>新しい Xamarin.Android プロジェクトを作成する

Visual Studio を起動します。 新しいプロジェクトを作成するには、[ファイル > 新しい > プロジェクト] を選択します。

[新しいプロジェクト] ダイアログで、[ **Android アプリ (Xamarin)** ] テンプレートを選択し、[**次へ**] をクリックします。

プロジェクトに**Timechangerandroid**という名前を指定し、[**作成**] をクリックします。

[新しいクロスプラットフォームアプリ] ダイアログで、[**空のアプリ**] を選択します。 Android の**最小バージョン**で、[ **Android 5.0 (ロリポップ)**] を選択します。 **[OK]** をクリックします。

Xamarin は、 **Timechangerandroid**という名前の1つのプロジェクトで新しいソリューションを作成します。

## <a name="create-a-ui-with-xaml"></a>XAML を使った UI の作成

プロジェクトの**Resources\ layout**ディレクトリで**activity_main .xml**を開きます。 このファイルの XML は、TimeChanger を開くときにユーザーに表示される最初の画面を定義します。

TimeChanger の UI はシンプルです。 現在の時刻が表示され、1時間単位で時間を調整するためのボタンがあります。 この例では`LinearLayout` 、垂直方向`LinearLayout`を使用して、ボタンを横に並べて配置します。 コンテンツは、 **android: 重力**属性を垂直方向`LinearLayout`の**中央**に設定することによって、画面の中央に配置されます。

**Activity_main**の内容を次のコードに置き換えます。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

この時点で、 **Timechangerandroid**を実行して、作成した UI を確認することができます。 次のセクションでは、現在の時刻を表示し、ボタンがアクションを実行できるようにする機能を UI に追加します。

## <a name="add-logic-code-with-c"></a>C でロジックコードを追加する#

**MainActivity.cs**を開きます。 このファイルには、UI に機能を追加する分離コードロジックが含まれています。

### <a name="set-the-current-time"></a>現在の時刻を設定する

最初に、時刻を表示する`TextView`への参照を取得します。 **FindViewById**を使用して、正しい**android: id** (前の手順の xml ではに`"@+id/timeDisplay"`設定されていたもの) を持つすべての UI 要素を検索します。 これは、 `TextView`現在の時刻を表示するです。

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Ui スレッドで UI コントロールを更新する必要があります。 別のスレッドから行った変更によって、画面に表示されるコントロールが正しく更新されない場合があります。 このコードが常に UI スレッドで実行されるという保証はないので、 **Runonuithread**メソッドを使用して、更新が正しく表示されることを確認してください。 完全な`UpdateTimeLabel`メソッドを次に示します。

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>1秒ごとに現在の時刻を更新する

この時点で、現在の時刻は、TimeChangerAndroid を起動した後、最大で1秒で正確になります。 時間を正確に保つために、ラベルを定期的に更新する必要があります。 **タイマー**オブジェクトは、現在の時刻でラベルを更新するコールバックメソッドを定期的に呼び出します。

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>HourOffset の追加

上矢印ボタンと下矢印ボタンは、1時間単位で時間を調整します。 **HourOffset**プロパティを追加して、現在の調整を追跡します。

```csharp
public int HourOffset { get; private set; }
```

ここで、HourOffset プロパティを認識するように UpdateTimeLabel メソッドを更新します。

```csharp
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>ボタンクリックイベントハンドラーを作成する

上矢印ボタンと下矢印ボタンは、HourOffset プロパティをインクリメントまたはデクリメントし、UpdateTimeLabel を呼び出す必要があります。

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>対応するイベントハンドラーに上矢印と下矢印ボタンを接続する

これらのボタンを対応するイベントハンドラーに関連付けるには、まず FindViewById を使用して、その id でボタンを検索します。 Button オブジェクトへの参照を取得したら、イベントに`Click`イベントハンドラーを追加できます。

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>完了した MainActivity.cs ファイル

完了すると、MainActivity.cs は次のようになります。

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>アプリケーションを実行する

アプリを実行するには、 **F5**キーを押すか、[デバッグ] をクリック > てデバッグを開始します。 [デバッガーが](emulator.md)どのように構成されているかによって、アプリはデバイスまたはエミュレーターで起動します。

## <a name="related-links"></a>関連リンク

- [Android デバイスまたはエミュレーターでテスト](emulator.md)します。
- [Xamarin. Forms を使用して Android サンプルアプリを作成する](xamarin-forms.md)
