---
description: TimePicker は、ユーザーがタッチ、マウス、またはキーボード入力を使って時刻値を選択できる標準化された方法です。
title: 時刻の選択コントロール
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d5391e3d738e7de81362a5fd0e42dc6d007dd
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270247"
---
# <a name="time-picker"></a>時刻の選択コントロール
 
TimePicker は、ユーザーがタッチ、マウス、またはキーボード入力を使って時刻値を選択できる標準化された方法です。

![時刻の選択コントロールの例](images/time-picker-closed.png)

**Windows UI ライブラリを入手する**

:::row:::
   :::column:::
      ![WinUI ロゴ](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
       Windows UI ライブラリ 2.2 以降には、丸めた角を使用するこのコントロールの新しいテンプレートが含まれます。 詳しくは、「[角の半径](../style/rounded-corner.md)」をご覧ください。 WinUI は、Windows アプリの新しいコントロールと UI 機能が含まれる NuGet パッケージです。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **プラットフォーム API**: [TimePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)、[SelectedTime プロパティ](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)


## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?
時刻の選択コントロールを使って、ユーザーが 1 つの時刻を選べるようにします。

適切なコントロールの選択について詳しくは、「[日付と時刻コントロール](date-and-time.md)」をご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/TimePicker">アプリを開き、TimePicker の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

エントリ ポイントには、選んだ時刻が表示されます。ユーザーがエントリ ポイントを選ぶと、選択ツール サーフェスが中央から縦方向に展開されて、時刻を選べるようになります。 時刻の選択は他の UI をオーバーレイし、他の UI を別の位置に移動させることはありません。

![展開した時刻の選択コントロールの例](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>時刻の選択コントロールの作成

次の例は、ヘッダーを含むシンプルな時刻の選択コントロールを作成する方法を示しています。

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

結果として、時刻の選択コントロールは、次のように表示されます。

![時刻の選択コントロールの例](images/time-picker-closed.png)

### <a name="formatting-the-time-picker"></a>時刻の選択の書式設定

既定で、時刻の選択には、12 時間形式の時計と AM/PM セレクターが表示されます。 [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) プロパティを "24HourClock" に設定すると、24 時間形式の時計を表示できます。

```xaml
<TimePicker Header="12HourClock" SelectedTime="14:30"/>
<TimePicker Header="24HourClock" SelectedTime="14:30" ClockIdentifier="24HourClock"/>
```

:::image type="content" source="images/date-time/time-picker-clocks.png" alt-text="12 時間形式の時計が表示された時刻の選択と、24 時間形式の時計が表示された選択。":::

[MinuteIncrement](/uwp/api/windows.ui.xaml.controls.timepicker.minuteincrement) プロパティを設定して、分の選択に表示される時間の増分を指定できます。 たとえば、15 に指定すると、`TimePicker` の分のコントロールには選択肢として 00、15、30、45 のみが表示されます。

```xaml
<TimePicker MinuteIncrement="15"/>
```

:::image type="content" source="images/date-time/time-picker-minute-increment.png" alt-text="15 分の増分で表示された時刻の選択。":::

### <a name="time-values"></a>時刻値

時刻の選択コントロールには、[Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)/[TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged) API と [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)/[SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) API の両方が用意されています。 両者の違いは、`Time` では null 値が許容されないのに対して、`SelectedTime` では null 値が許容されることです。

`SelectedTime` の値は、時刻の選択を設定するのに使用され、既定では `null` に指定されています。 `SelectedTime` が `null` の場合、`Time` プロパティの [TimeSpan](/dotnet/api/system.timespan?view=dotnet-uwp-10.0&preserve-view=true) は 0 に設定されます。それ以外の場合、`Time` の値は `SelectedTime` の値と同期されます。 `SelectedTime` が `null` の場合、ピッカーは 'unset' になり、時刻ではなくフィールド名が表示されます。

:::image type="content" source="images/date-time/time-picker-no-selected-time.png" alt-text="時刻が選択されていない時刻の選択。":::

#### <a name="initializing-a-time-value"></a>時刻値の初期化

コードで、時刻のプロパティを `TimeSpan` 型の値に初期化できます。

```csharp
TimePicker timePicker = new TimePicker
{
    SelectedTime = new TimeSpan(14, 15, 00) // Seconds are ignored.
};
```

時刻値を XAML の属性として設定できます。 XAML で `TimePicker` オブジェクトを既に宣言しており、時刻値にバインドを使用していない場合は、これが最も簡単な方法です。 *Hh:Mm* 形式の文字列を使用します。*Hh* は時間で、0 から 23 までの範囲で指定できます。*Mm* は分で、0 から 59 の範囲で指定できます。

```xaml
<TimePicker SelectedTime="14:15"/>
```

> [!NOTE]
> 日付と時刻の値の重要な情報については、「*日付と時刻コントロール*」の「[DateTime と Calendar の値](date-and-time.md#datetime-and-calendar-values)」をご覧ください。

### <a name="using-the-time-values"></a>時刻値の使用

時刻値をアプリで使用するには、通常、[SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime) または [Time](/uwp/api/windows.ui.xaml.controls.timepicker.time) プロパティへのデータ バインドを使用する、コード内で時間のプロパティを直接使用する、[SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) または [TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged) イベントを処理する、のいずれかの方法を利用します。

> `DatePicker` と `TimePicker` を一緒に使用して単一の `DateTime` 値を更新する方法の例については、[「カレンダー、日付、および時刻コントロール」の「日付の選択と時刻の選択を一緒に使用する」](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)を参照してください。

ここでは、`SelectedTime` プロパティを使用して、選択した時刻と現在の時刻を比較します。

`SelectedTime` プロパティでは null 値が許容されるため、これを `DateTime` に明示的にキャストする必要があることにご注意ください。たとえば、`DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);` のようにします。 ただし、`Time` プロパティをキャストなしで使用することもできます。その場合、`DateTime myTime = DateTime.Today + checkTimePicker.Time;` のようにします。

:::image type="content" source="images/date-time/time-picker-check.png" alt-text="時刻の選択、ボタン、テキスト ラベル。":::

```xaml
<StackPanel>
    <TimePicker x:Name="checkTimePicker"/>
    <Button Content="Check time" Click="{x:Bind CheckTime}"/>
    <TextBlock x:Name="resultText"/>
</StackPanel>
```

```csharp
private void CheckTime()
{
    // Using the Time property.
    // DateTime myTime = DateTime.Today + checkTimePicker.Time;
    // Using the SelectedTime property (nullable requires cast to DateTime).
    DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);
    if (DateTime.Now >= myTime)
    {
        resultText.Text = "Your selected time has already past.";
    }
    else
    {
        string hrs = (myTime - DateTime.Now).Hours.ToString();
        string mins = (myTime - DateTime.Now).Minutes.ToString();
        resultText.Text = string.Format("Your selected time is {0} hours, {1} minutes from now.", hrs, mins);
    }
}
```

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - 対話形式で XAML コントロールのすべてを参照できます。

## <a name="related-topics"></a>関連トピック

- [日付と時刻コントロール](date-and-time.md)
- [カレンダーの日付の選択コントロール](calendar-date-picker.md)
- [カレンダー ビュー](calendar-view.md)
- [日付の選択コントロール](date-picker.md)
