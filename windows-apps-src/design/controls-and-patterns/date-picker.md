---
description: DatePicker は、ユーザーがタッチ、マウス、またはキーボード入力を使ってローカライズされた日付値を選択できる標準化された方法です。
title: 日付の選択コントロール
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 51fa98908fa4a2c1c026ce5a4c924b8f800aee8b
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286601"
---
# <a name="date-picker"></a>日付の選択コントロール

DatePicker は、ユーザーがタッチ、マウス、またはキーボード入力を使ってローカライズされた日付値を選択できる標準化された方法です。

![日付の選択コントロールの例](images/date-picker-closed.png)

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

> **プラットフォーム API:** [DatePicker クラス](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、[SelectedDate プロパティ](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)

## <a name="is-this-the-right-control"></a>これは適切なコントロールですか?

日付の選択コントロールは、ユーザーが誕生日などの既知の日付 (カレンダーのコンテキストが重要ではない日) を選べるようにする場合に使用します。

カレンダーのコンテキストが重要な場合は、[カレンダーの日付の選択](calendar-date-picker.md)または[カレンダー ビュー](calendar-view.md)の使用をご検討ください。

適切な日付コントロールの選択について詳しくは、「[日付と時刻コントロール](date-and-time.md)」をご覧ください。

## <a name="examples"></a>例

<table>
<th align="left">XAML コントロール ギャラリー<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p><strong style="font-weight: semi-bold">XAML コントロール ギャラリー</strong> アプリがインストールされている場合、こちらをクリックして<a href="xamlcontrolsgallery:/item/DatePicker">アプリを開き、DatePicker の動作を確認</a>してください。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">XAML コントロール ギャラリー アプリを入手する (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">ソース コード (GitHub) を入手する</a></li>
    </ul>
</td>
</tr>
</table>

エントリ ポイントには、選んだ日付が表示されます。ユーザーがエントリ ポイントを選ぶと、選択ツール サーフェイスが中央から縦方向に展開されて、日付を選べるようになります。 日付の選択は他の UI をオーバーレイし、他の UI を別の位置に移動させることはありません。

![展開した日付の選択コントロールの例](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>日付の選択コントロールの作成

次の例は、ヘッダーを含むシンプルな日付の選択コントロールを作成する方法を示しています。

```xaml
<DatePicker x:Name="birthDatePicker" Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

結果として、日付の選択コントロールは、次のように表示されます。

![日付の選択コントロールの例](images/date-picker-closed.png)

### <a name="formatting-the-date-picker"></a>日付の選択の書式設定

既定で、日付の選択には日、月、年が表示されます。 日付の選択を使用するシナリオにこれらのフィールドの一部が不要な場合は、不要なフィールドを非表示にできます。 フィールドを非表示にするには、対応する *field* Visible プロパティ ([DayVisible](/uwp/api/windows.ui.xaml.controls.datepicker.dayvisible)、[MonthVisible](/uwp/api/windows.ui.xaml.controls.datepicker.monthvisible)、または [YearVisible](/uwp/api/windows.ui.xaml.controls.datepicker.yearvisible)) を `false` に設定します。

次の例では、年のみが必要であるため、[日] フィールドと [月] フィールドは非表示になっています。

```xaml
<DatePicker x:Name="yearDatePicker" Header="In what year was Microsoft founded?" 
            MonthVisible="False" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-year-only.png" alt-text="[日] フィールドと [月] フィールドが非表示になっている日付の選択。":::

`DatePicker` 内の各 `ComboBox` の文字列の内容は、[DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter) によって作成されます。 *書式テンプレート* か *書式パターン* のいずれかである文字列を指定することにより、日付値の書式を `DateTimeFormatter` に通知します。 詳細については、[DayFormat](/uwp/api/windows.ui.xaml.controls.datepicker.dayformat)、[MonthFormat](/uwp/api/windows.ui.xaml.controls.datepicker.monthformat)、[YearFormat](/uwp/api/windows.ui.xaml.controls.datepicker.yearformat) プロパティを参照してください。

ここでは、*書式パターン* を使用して、月を整数と省略形で表示します。 書式パターンにリテラル文字列を追加できます。たとえば、`({month.abbreviated})` のようにして月の省略形をかっこで囲むことができます。

```xaml
<DatePicker MonthFormat="{}{month.integer(2)} ({month.abbreviated})" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-day-hidden.png" alt-text="[日] フィールドが非表示になっている日付の選択。":::

### <a name="date-values"></a>日付値

日付の選択コントロールには、[Date](/uwp/api/windows.ui.xaml.controls.datepicker.date)/[DateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.datechanged) API と [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)/[SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) API の両方が用意されています。 両者の違いは、`Date` では null 値が許容されないのに対して、`SelectedDate` では null 値が許容されることです。

`SelectedDate` の値は、日付の選択を設定するのに使用され、既定では `null` に指定されています。 `SelectedDate` が `null` の場合、`Date` プロパティは 12/31/1600 に設定されます。それ以外の場合、`Date` の値は `SelectedDate` の値と同期されます。 `SelectedDate` が `null` の場合、ピッカーは "unset" になり、日付ではなくフィールド名が表示されます。

:::image type="content" source="images/date-time/date-picker-no-selected-date.png" alt-text="日付が選択されていない日付の選択。":::

[MinYear](/uwp/api/windows.ui.xaml.controls.datepicker.minyear) プロパティと [MaxYear](/uwp/api/windows.ui.xaml.controls.datepicker.maxyear) プロパティを設定することにより、ピッカー内の日付の値を制限できます。 既定では、`MinYear` は現在の日付の 100 年前、`MaxYear` は現在の日付の 100 年後に設定されています。

`MinYear` または `MaxYear` のどちらかのみを設定する場合、設定した日付と他方の既定値の日付によって有効な期間が作成されることを確認する必要があります。期間が正しくないと、ピッカー内で日付を選択できなくなります。 たとえば、`yearDatePicker.MaxYear = new DateTimeOffset(new DateTime(900, 1, 1));` のみを設定すると、`MinYear` には既定値が指定されているため、無効な期間が作成されます。

#### <a name="initializing-a-date-value"></a>日付値の初期化

日付のプロパティは、XAML 属性文字列として設定することはできません。これは、Windows ランタイム XAML パーサーには、文字列を [DateTime](/uwp/api/windows.foundation.datetime) / [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true) オブジェクトとして日付に変換する変換ロジックがないためです。 これらのオブジェクトをコードで定義し、現在の日付以外の日付に設定できるいくつかの方法を次に説明します。

- [DateTime](/uwp/api/windows.foundation.datetime): [Windows.Globalization.Calendar](/uwp/api/windows.globalization.calendar) オブジェクトをインスタンス化します (現在の日付に初期化されます)。 [Year](/uwp/api/windows.globalization.calendar.year) を設定するか [AddYears](/uwp/api/windows.globalization.calendar.addyears) を呼び出して、日付を調整します。 次に、[Calendar.GetDateTime](/uwp/api/windows.globalization.calendar.getdatetime) を呼び出し、返される `DateTime` を使用して、日付のプロパティを設定します。
- [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true): コンストラクターを呼び出します。 内部の [System.DateTime](/dotnet/api/system.datetime?view=dotnet-uwp-10.0&preserve-view=true) の場合は、そのコンストラクター シグネチャを使用します。 または、既定の [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true) を構築し (現在の日付に初期化されます)、[AddYears](/dotnet/api/system.datetimeoffset.addyears?view=dotnet-uwp-10.0&preserve-view=true) を呼び出します。

考えられる別の方法として、データ オブジェクトとして (またはデータ コンテキストで) 利用可能な日付を定義し、日付のプロパティを、データとしてその日付にアクセスできる [{Binding} マークアップ拡張](/windows/uwp/xaml-platform/binding-markup-extension)を参照する XAML 属性として設定することができます。

> [!NOTE]
> 日付値の重要な情報については、「日付と時刻コントロール」の「[DateTime と Calendar の値](date-and-time.md#datetime-and-calendar-values)」をご覧ください。

次の例では、`SelectedDate`、`MinYear`、`MaxYear` の各プロパティを、異なる `DatePicker` コントロール上で設定する方法を示します。

```xaml
<DatePicker x:Name="yearDatePicker" MonthVisible="False" DayVisible="False"/>
<DatePicker x:Name="arrivalDatePicker" Header="Arrival date"/>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set minimum year to 1900 and maximum year to 1999.
    yearDatePicker.SelectedDate = new DateTimeOffset(new DateTime(1950, 1, 1));
    yearDatePicker.MinYear = new DateTimeOffset(new DateTime(1900, 1, 1));
    // Using a different DateTimeOffset constructor.
    yearDatePicker.MaxYear = new DateTimeOffset(1999, 12, 31, 0, 0, 0, new TimeSpan());

    // Set minimum to the current year and maximum to five years from now.
    arrivalDatePicker.MinYear = DateTimeOffset.Now;
    arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
}
```

### <a name="using-the-date-values"></a>日付値の使用

日付値をアプリで使用するには、通常、[SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate) プロパティへのデータ バインドを使用するか、[SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) イベントを処理します。

> `DatePicker` と `TimePicker` を一緒に使用して単一の `DateTime` 値を更新する方法の例については、[「カレンダー、日付、および時刻コントロール」の「日付の選択と時刻の選択を一緒に使用する」](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)を参照してください。

ここでは、`DatePicker` を使用することにより、ユーザーが到着日を選択できるようにします。 `SelectedDateChanged` イベントを処理して、`arrivalDateTime` という名前の [DateTime](/uwp/api/windows.foundation.datetime) インスタンスを更新します。

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="arrivalDatePicker_SelectedDateChanged"/>
    <Button Content="Clear" Click="ClearDateButton_Click"/>
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

    private void arrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDatePicker.SelectedDate = null;
        arrivalText.Text = string.Empty;
    }
}
```

## <a name="get-the-sample-code"></a>サンプル コードの入手

- [XAML コントロール ギャラリー サンプル](https://github.com/Microsoft/Xaml-Controls-Gallery) - インタラクティブな形で XAML コントロールのすべてを参照できます。

## <a name="related-articles"></a>関連記事

- [日付と時刻コントロール](date-and-time.md)
- [カレンダーの日付の選択コントロール](calendar-date-picker.md)
- [カレンダー ビュー](calendar-view.md)
- [時刻の選択コントロール](time-picker.md)
