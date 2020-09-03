---
Description: カレンダー ビューを使うと、ユーザーはカレンダーを表示し操作できます (カレンダーは、月、年、または 10 年単位で操作できます)。
title: カレンダー ビュー
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9c4c1dfa6bf4a92b7f8da177b749dfd861d93338
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173556"
---
# <a name="calendar-view"></a>カレンダー ビュー

カレンダー ビューを使うと、ユーザーはカレンダーを表示し操作できます (カレンダーは、月、年、または 10 年単位で操作できます)。 ユーザーは 1 つの日付や日付の範囲を選ぶことができます。 カレンダー ビューには選択コントロール サーフェイスがなく、カレンダーは常に表示されます。

**Windows UI ライブラリを入手する**

|  |  |
| - | - |
| ![WinUI ロゴ](images/winui-logo-64x64.png) | Windows UI ライブラリ 2.2 以降には、丸めた角を使用するこのコントロールの新しいテンプレートが含まれます。 詳しくは、「[角の半径](../style/rounded-corner.md)」をご覧ください。 WinUI は、Windows アプリの新しいコントロールと UI 機能が含まれる NuGet パッケージです。 インストール手順などについて詳しくは、「[Windows UI Library (Windows UI ライブラリ)](/uwp/toolkits/winui/)」をご覧ください。 |

> **プラットフォーム API:** [CalendarView クラス](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、[SelectedDatesChanged イベント](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?
カレンダー ビューを使うと、ユーザーは常に表示されているカレンダーから 1 つの日付または日付の範囲を選ぶことができます。

ユーザーが一度に複数の日付を選べるようにする必要がある場合は、カレンダー ビューを使う必要があります。 ユーザーが 1 つの日付しか選べないようにする必要があり、カレンダーを常に表示する必要がない場合は、[カレンダーの日付の選択コントロール](calendar-date-picker.md)または[日付の選択コントロール](date-picker.md)を使うことを検討してください。

適切なコントロールの選択について詳しくは、「[日付と時刻コントロール](date-and-time.md)」をご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/CalendarView">アプリを開き、CalendarView の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

カレンダー ビューは、月ビュー、年ビュー、10 年ビューという 3 つの個別のビューで構成されています。 既定では、月ビューが開きます。 スタートアップ表示を指定するには、[DisplayMode](/uwp/api/windows.ui.xaml.controls.calendarview.displaymode) プロパティを設定します。

![カレンダー ビューの 3 つのビュー](images/calendar-view-3-views.png)

ユーザーが月ビューのヘッダーをクリックすると年ビューが開き、年ビューのヘッダーをクリックすると 10 年ビューが開きます。 また、10 年ビューで年を選ぶと年ビューに戻り、年ビューで月を選ぶと月ビューに戻ります。 ヘッダーの横にある 2 つの矢印を使うと、月、年、10 年単位で前後に移動できます。

## <a name="create-a-calendar-view"></a>カレンダー ビューの作成

次の例は、1 つのカレンダー ビューを作成する方法を示しています。

```xaml
<CalendarView/>
```

結果のカレンダー ビューは次のように表示されます。

![カレンダー ビューの例](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>日付の選択

既定では、[SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) プロパティは **Single** に設定されています。 このため、ユーザーはカレンダー内の 1 つの日付を選ぶことができます。 日付の選択を無効にするには、SelectionMode を **None** に設定します。

ユーザーが複数の日付を選べるようにするには、SelectionMode を **Multiple** に設定します。 次のように [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) コレクションに [DateTime](/dotnet/api/system.datetime)/[DateTimeOffset](/dotnet/api/system.datetimeoffset) オブジェクトを追加すると、プログラムから複数の日付を選ぶことができます。

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

ユーザーは、選択済みの日付をカレンダー グリッドでクリックまたはタップすると、その日付の選択を解除できます。

[SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) コレクションが変化したときに通知を受け取るようにするには、[SelectedDatesChanged](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged) イベントを処理します。

> [!NOTE]
> 日付値の重要な情報については、「日付と時刻コントロール」の「[DateTime と Calendar の値](date-and-time.md#datetime-and-calendar-values)」をご覧ください。

### <a name="customizing-the-calendar-views-appearance"></a>カレンダー ビューの外観のカスタマイズ

カレンダー ビューは、ControlTemplate で定義される XAML 要素と、コントロールによって直接レンダリングされるビジュアル要素で構成されます。
- コントロール テンプレートで定義される XAML 要素には、コントロールを囲む境界線、ヘッダー、[前へ] ボタンと [次へ] ボタン、および DayOfWeek 要素が含まれています。 すべての XAML コントロールと同様、これらの要素にスタイルを指定し、テンプレートを再適用することができます。
- カレンダー グリッドは、[CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) オブジェクトで構成されています。 これらの要素のスタイルを指定したり、テンプレートを再適用することはできませんが、それらの外観をカスタマイズできるさまざまなプロパティが用意されています。

次の図は、カレンダーの月ビューを構成する要素を示しています。 詳しくは、[CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) クラスの「解説」をご覧ください。

![カレンダーの月ビューの要素](images/calendar-view-month-elements.png)

次の表は、カレンダー要素の外観を変えるために変更できるプロパティを示しています。

要素 | プロパティ
--------|-----------
DayOfWeek | [DayOfWeekFormat](/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground)、[CalendarItemBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush)、[CalendarItemBorderThickness](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness)、[CalendarItemForeground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily)、[DayItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize)、[DayItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle)、[DayItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight)、[HorizontalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment)、[VerticalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment)、[CalendarViewDayItemStyle](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (年ビューと 10 年ビューに含まれていて DayItem と等価) | [MonthYearItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily)、[MonthYearItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize)、[MonthYearItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle)、[MonthYearItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily)、[FirstOfMonthLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize)、[FirstOfMonthLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle)、[FirstOfMonthLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight)、[HorizontalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment)、[VerticalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment)、[IsGroupLabelVisible](/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (年ビューと 10 年ビューに含まれていて、FirstOfMonthLabel と等価) | [FirstOfYearDecadeLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily)、[FirstOfYearDecadeLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize)、[FirstOfYearDecadeLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle)、[FirstOfYearDecadeLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
表示状態の境界線 | [FocusBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush)、[HoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush)、[PressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush)、[SelectedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush)、[SelectedForeground](/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground)、[SelectedHoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush)、[SelectedPressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled)、[OutOfScopeBackground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground)、[OutOfScopeForeground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
現在 | [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted)、[TodayFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight)、[TodayForeground](/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 既定では、月ビューは一度に 6 週間を表示します。 表示する週数を変更するには、[NumberOfWeeksInView](/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview)プロパティを設定します。 表示する週数の最小値は 2 で、最大値は 8 です。

既定では、年ビューと 10 年ビューは 4x4 のグリッドに表示されます。 行または列の数を変更するには、目的の行数と列数を指定して [SetYearDecadeDisplayDimensions](/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) を呼び出します。 これにより、年ビューと 10 年ビューの両方のグリッドが変更されます。

次の例は、年ビューと 10 年ビューを 3x4 のグリッドに表示するよう設定しています。

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

既定では、カレンダー ビューに表示される日付の最小値は 100 年前の現在日で、表示される日付の最大値は 100 年後の現在日です。 カレンダーに表示する最小日付と最大日付を変更するには、[MinDate](/uwp/api/windows.ui.xaml.controls.calendarview.mindate) プロパティと [MaxDate](/uwp/api/windows.ui.xaml.controls.calendarview.maxdate) プロパティを設定します。

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>カレンダーの日付項目の更新

カレンダーの各日付は、[CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem) オブジェクトで表されます。 個々の日付項目にアクセスしてそのプロパティとメソッドを使うには、[CalendarViewDayItemChanging](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging) イベントを処理し、イベント引数の Item プロパティを使って CalendarViewDayItem にアクセスします。

カレンダー ビュー内の特定の日付を選択できないようにするには、その日付の [CalendarViewDayItem.IsBlackout](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout) プロパティを **true** に設定します。

ある日付のイベントの埋まり具合についてのコンテキスト情報を表示するには、[CalendarViewDayItem.SetDensityColors](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors) メソッドを呼び出します。 日付ごとに 0 ～ 10 の範囲の密度コントロール バーを表示し、各バーの色を設定します。

カレンダーの日付項目のいくつかを次に示します。 日付 1 と 2 は暗転しています。日付 2、3、および 4 には、さまざまな密度コントロール バーが設定されています。

![密度コントロール バーが設定されたカレンダーの日付](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>段階的なレンダリング

カレンダー ビューには、多数の CalendarViewDayItem オブジェクトを含めることができます。 UI の応答性を保ち、カレンダー内をスムーズに移動できるようにするため、カレンダー ビューでは段階的なレンダリングがサポートされています。 そのため、日付項目の処理を複数のフェーズに分けることができます。 すべてのフェーズが完了する前に日付がビューの範囲外に移動すると、その項目の処理とレンダリングはそれ以上行われません。

次の例は、予定のスケジュール設定を目的とした、カレンダー ビューの段階的なレンダリングを示しています。
- フェーズ 0 では、既定の日付項目をレンダリングします。
- フェーズ 1 では、予約できない日付を暗転します。 これには、過去の日付、日曜日、既に予定がすべて埋まっている日付などがあります。
- フェーズ 2 では、その日の予定をそれぞれチェックします。 確定済みの予定には緑色の濃度コントロール バーを、仮の予定には青色の濃度コントロール バーを表示します。

この例の `Bookings` クラスは、架空の予約アプリのものなので、そのクラスは示されていません。

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender,
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [日付と時刻コントロール](date-and-time.md)
- [カレンダーの日付の選択コントロール](calendar-date-picker.md)
- [日付の選択コントロール](date-picker.md)
- [時刻の選択コントロール](time-picker.md)