---
description: 日付と時刻のコントロールでは、日付と時刻を表示および設定できます。 この記事では設計ガイドラインを示し、適切なコントロールを選ぶのに役立ちます。
title: 日付コントロールと時刻コントロールのガイドライン
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 239296cfb2de5b52b9f9ef1498b5c2c22e2be372
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286611"
---
# <a name="calendar-date-and-time-controls"></a>カレンダー、日付、および時刻コントロール

 

日付コントロールと時刻コントロールを使用することで、その地域に合った標準化された方法で、ユーザーがアプリで日付と時刻を表示および設定できるようにすることができます。 この記事では設計ガイドラインを示し、適切なコントロールを選ぶのに役立ちます。

> **重要な API**: [CalendarView クラス](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、[CalendarDatePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)、[DatePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、[TimePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合は、こちらをクリックして<a href="xamlcontrolsgallery:/category/DataInput">アプリを開き、これらのコントロールの動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>日付または時刻コントロールの選択

4 つの日付および時刻コントロールから選択できますが、シナリオによって使用するコントロールは異なります。 以下の情報を参考にして、アプリに適切なコントロールを選んでください。

| Control | 例 | 説明 |
| ------- | :-----: | ----------- |
| カレンダー ビュー | ![カレンダー ビューの例](images/controls_calendar_monthview_small.png) | 常に表示されるカレンダーから 1 つの日付または日付の範囲を選ぶ場合に使用します。 |
| カレンダーの日付の選択コントロール | ![カレンダーの日付の選択コントールのスクリーンショット。](images/calendar-date-picker-closed.png) | コンテキストに沿ったカレンダーから 1 つの日付を選ぶ場合に使用します。 |
| 日付の選択コントロール | ![日付の選択コントロールの例](images/date-picker-closed.png) | コンテキスト情報が重要でない既知の 1 つの日付を選ぶ場合に使用します。 |
| 時刻の選択ツール | ![時刻の選択コントロールの例](images/time-picker-closed.png) | 1 つの時刻を選ぶ場合に使用します。 |

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>カレンダー ビュー

**CalendarView** を使うと、ユーザーはカレンダーを表示し操作できます (カレンダーは、月、年、または 10 年単位で操作できます)。 ユーザーは 1 つの日付や日付の範囲を選ぶことができます。 カレンダー ビューには選択コントロール サーフェイスがなく、カレンダーは常に表示されます。

カレンダー ビューは、月ビュー、年ビュー、10 年ビューという 3 つの個別のビューで構成されています。 既定では、月ビューが開きますが、任意のビューをスタートアップ ビューとして指定できます。

![月ビュー、年ビュー、10 年ビューを表示した 3 つのカレンダー ビューのスクリーンショット。](images/calendar-view-3-views.png)

- ユーザーが複数の日付を選べるようにする必要がある場合は、**CalendarView** を使う必要があります。
- ユーザーが 1 つの日付しか選べないようにする必要があり、カレンダーを常に表示する必要がない場合は、**CalendarDatePicker** または **DatePicker** コントロールを使うことを検討してください。

### <a name="calendar-date-picker"></a>カレンダーの日付の選択コントロール

**CalendarDatePicker** は、カレンダーの曜日や埋まり具合などのコンテキスト情報が必要となるカレンダー ビューから 1 つの日付を選ぶ用途に最適なドロップダウン コントロールです。 追加のコンテキストを提供したり、使用可能な日付を制限したりするように、カレンダーを変更することもできます。

日付が設定されていない場合、エントリ ポイントにはプレースホルダー テキストが表示されます。設定されている場合は、選んだ日付が表示されます。 ユーザーがエントリ ポイントを選ぶと、カレンダー ビューが展開されて、ユーザーが日付を選べるようになります。 カレンダー ビューは他の UI をオーバーレイし、他の UI を別の位置に移動させることはありません。

![空の日付選択テキスト ボックスが表示されたカレンダーの日付の選択コントロールと、日付が入力されて下にカレンダーが表示されている状態のスクリーンショット。](images/calendar-date-picker-2-views.png)

- カレンダーの日付の選択コントロールは、予定日や出発日の選択などに使います。 

### <a name="date-picker"></a>日付の選択コントロール

**DatePicker** コントロールによって、標準化された方法で特定の日付を選ぶことができます。 

エントリ ポイントには、選んだ日付が表示されます。ユーザーがエントリ ポイントを選ぶと、選択ツール サーフェイスが中央から縦方向に展開されて、日付を選べるようになります。 日付の選択は他の UI をオーバーレイし、他の UI を別の位置に移動させることはありません。

![展開した日付の選択コントロールの例](images/controls_datepicker_expand.png)

- 日付の選択コントロールは、ユーザーが誕生日などの既知の日付 (カレンダーのコンテキストが重要ではない日) を選べるようにする場合に使用します。

### <a name="time-picker"></a>時刻の選択ツール

**TimePicker** は、予定や出発時刻などの 1 つの時刻を選択する場合に使用します。 ユーザーまたはコードによって設定された静的な表示であるため、更新して現在の時刻を表示することはできません。

エントリ ポイントには、選んだ時刻が表示されます。ユーザーがエントリ ポイントを選ぶと、選択ツール サーフェスが中央から縦方向に展開されて、時刻を選べるようになります。 時刻の選択は他の UI をオーバーレイし、他の UI を別の位置に移動させることはありません。

![展開した時刻の選択コントロールの例](images/controls_timepicker_expand.png)

- 時刻の選択コントロールを使って、ユーザーが 1 つの時刻を選べるようにします。

## <a name="create-a-date-or-time-control"></a>日付または時刻コントロールの作成

日付および時刻コントロールの詳細と例については、次の記事をご覧ください。

- [カレンダー ビュー](calendar-view.md)
- [カレンダーの日付の選択コントロール](calendar-date-picker.md)
- [日付の選択コントロール](date-picker.md)
- [時刻の選択コントロール](time-picker.md)

### <a name="use-a-date-picker-and-time-picker-together"></a>日付の選択と時刻の選択を一緒に使用する

次の例で、`DatePicker` と `TimePicker` を一緒に使用して、ユーザーが到着日時を選択できるようにする方法を示します。 `SelectedDateChanged` イベントと `SelectedTimeChanged` イベントを処理して、`arrivalDateTime` という名前の単一の [DateTime](/uwp/api/windows.foundation.datetime) インスタンスを更新します。 ユーザーは、設定された日付の選択と時刻の選択を後からクリアすることもできます。

:::image type="content" source="images/date-time/date-and-time-picker.png" alt-text="日付の選択、時刻の選択、ボタン、テキスト ラベル。":::

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="ArrivalDatePicker_SelectedDateChanged"/>
    <StackPanel Orientation="Horizontal">
        <TimePicker x:Name="arrivalTimePicker" Header="Arrival time"
                MinuteIncrement="15"
                SelectedTimeChanged="ArrivalTimePicker_SelectedTimeChanged"/>
        <Button Content="Clear" Click="ClearDateButton_Click"
                VerticalAlignment="Bottom" Height="30" Width="54"/>
    </StackPanel>
    <TextBlock x:Name="arrivalText" Margin="0,12"/>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    DateTime arrivalDateTime;

    public MainPage()
    {
        this.InitializeComponent();

        // Set minimum to the current year and maximum to five years from now.
        arrivalDatePicker.MinYear = DateTimeOffset.Now;
        arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
    }

    private void ArrivalTimePicker_SelectedTimeChanged(TimePicker sender, TimePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalTimePicker.SelectedTime != null)
        {
            arrivalDateTime = new DateTime(arrivalDateTime.Year, arrivalDateTime.Month, arrivalDateTime.Day,
                                           args.NewTime.Value.Hours, args.NewTime.Value.Minutes, args.NewTime.Value.Seconds);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ArrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            if (VerifyDateIsFuture((DateTimeOffset)arrivalDatePicker.SelectedDate) == true)
            {
                arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day,
                                               arrivalDateTime.Hour, arrivalDateTime.Minute, arrivalDateTime.Second);
                arrivalText.Text = arrivalDateTime.ToString();
            }
            else
            {
                arrivalDatePicker.SelectedDate = null;
                arrivalText.Text = "Arrival date must be later than today.";
            }
        }
    }

    private bool VerifyDateIsFuture(DateTimeOffset date)
    {
        if (date > DateTimeOffset.Now)
        {
            return true;
        }
        return false;
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDateTime = new DateTime();
        arrivalDatePicker.SelectedDate = null;
        arrivalTimePicker.SelectedTime = null;
        arrivalText.Text = string.Empty;
    }
}
```

### <a name="globalization"></a>グローバリゼーション

XAML の日付のコントロールでは、Windows でサポートされる各カレンダー システムがサポートされます。 それらのカレンダーは [Windows.Globalization.CalendarIdentifiers](/uwp/api/Windows.Globalization.CalendarIdentifiers) クラスで指定されます。 各コントロールは、アプリの既定の言語に適したカレンダーを使います。または、**CalendarIdentifier** プロパティを設定して特定のカレンダー システムを使うこともできます。

時刻の選択コントロールでは、[Windows.Globalization.ClockIdentifiers](/uwp/api/Windows.Globalization.ClockIdentifiers) クラスで指定される各クロック システムがサポートされます。 [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) プロパティを設定し、12 時間形式または 24 時間形式を指定できます。 プロパティの型は文字列ですが、ClockIdentifiers クラスの静的な文字列プロパティに対応する値を使用する必要があります。 それらの値とは、TwelveHour (文字列 "12HourClock") と TwentyFourHour (文字列 "24HourClock") です。 既定値は "12HourClock" です。

### <a name="datetime-and-calendar-values"></a>DateTime と Calendar の値

XAML の日付および時刻コントロールで使用される日付オブジェクトでは、プログラミング言語によって表現方法が異なります。

- C# および Visual Basic では、.NET の一部である [System.DateTimeOffset](/dotnet/api/system.datetimeoffset) 構造体が使用されます。 
- C++/CX では、[Windows::Foundation::DateTime](/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime) 構造体が使用されます。 

関連する概念として Calendar クラスがあります。Calendar クラスは、コンテキストでの日付の解釈方法に影響を及ぼします。 すべての Windows ランタイム アプリで、[Windows.Globalization.Calendar](/uwp/api/Windows.Globalization.Calendar) クラスを使用することができます。 C# および Visual Basic アプリでは、代わりに機能が非常によく似た [System.Globalization.Calendar](/dotnet/api/system.globalization.calendar) クラスを使用することができます  (Windows ランタイム アプリでは、基本の .NET Calendar クラスを使用することはできますが、GregorianCalendar などの具体的な実装を使用することはできません)。

.NET では、[DateTime](/dotnet/api/system.datetime) という名前の型もサポートされます。これは、暗黙的に [DateTimeOffset](/dotnet/api/system.datetimeoffset) と読み替えることができます。 したがって、.NET コードで値を設定するために "DateTime" 型が使用されていた場合、それは実際には DateTimeOffset です。 DateTime と DateTimeOffset の違いについて詳しくは、「[DateTimeOffset](/dotnet/api/system.datetimeoffset) クラス」の「注釈」をご覧ください。

> [!NOTE]
> 日付オブジェクトを受け取るプロパティは、XAML 属性文字列として設定することはできません。これは、Windows ランタイム XAML パーサーには、文字列を DateTime/DateTimeOffset オブジェクトとして日付に変換する変換ロジックがないためです。 通常、それらの値はコードで設定します。 考えられる別の方法として、データ オブジェクトとして (またはデータ コンテキストで) 利用可能な日付を定義し、その日付をデータとしてアクセスできる [\{Binding\} マークアップ拡張](../../xaml-platform/binding-markup-extension.md)表現を参照する XAML 属性をプロパティとして設定することができます。

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML UI の基本のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)
- [カレンダーのサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Calendar)
- [日付と時刻の書式設定のサンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/DateTimeFormatting)

## <a name="related-topics"></a>関連トピック

### <a name="for-developers-xaml"></a>開発者向け (XAML)

- [CalendarView クラス](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
