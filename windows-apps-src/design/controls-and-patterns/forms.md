---
title: フォーム
description: ユニバーサル Windows プラットフォーム (UWP) アプリでフォームの XAML レイアウトを設計および作成するためのガイドラインについて説明します。
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Fluent
ms.openlocfilehash: 0c55d98d0680142ad1d4f319b8910fd335fe8d5f
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219465"
---
# <a name="forms"></a>フォーム
フォームは、ユーザーからデータを収集して送信するコントロールのグループです。 通常、フォームは、ページの設定、調査、アカウントの作成、その他に使われます。 

この記事では、フォーム用の XAML レイアウトを作成するためのデザイン ガイドラインについて説明します。

![フォームの例](images/PivotHeader.png)

## <a name="when-should-you-use-a-form"></a>フォームを使用する必要がある場合
フォームは、明確に相互に関連性のあるデータ入力を収集するための専用ページです。 ユーザーから明示的にデータを収集する必要がある場合は、フォームを使用する必要があります。 ユーザー用のフォームは次の目的に作成する場合があります。
- アカウントにログインする
- アカウントにサインアップする
- プライバシーや表示オプションなどのアプリの設定を変更する
- 調査を行う
- アイテムを購入する
- フィードバックの送信

## <a name="types-of-forms"></a>フォームの種類

ユーザー入力を送信および表示する方法を考えた場合、フォームには 2 つの種類があります。

### <a name="1-instantly-updating"></a>1.すぐに更新する
![設定ページ](images/control-examples/toggle-switch-news.png)

フォームで値を変更した結果をユーザーがすぐに確認できるようにする場合は、即時更新フォームを使います。 たとえば、設定ページでは、現在の選択が表示されていて、選択内容を変更するとすぐに適用されます。 アプリで変更を確認するには、各入力コントロールに[イベント ハンドラーを追加する](controls-and-events-intro.md)必要があります。 ユーザーが入力コントロールを変更した場合、アプリで適切に応答できます。

### <a name="2-submitting-with-button"></a>2.ボタンで送信する
もう 1 つの種類のフォームでは、ユーザーがボタンをクリックすることで、データを送信するタイミングを選択できます。

![予定表の新規イベント追加ページ](images/calendar-form.png)

この種類のフォームでは、ユーザーは柔軟に応答できます。 通常、この種類のフォームにはより自由な形式の入力フィールドが含まれているため、受け取る応答も多様です。 有効なユーザー入力と正しく書式設定されたデータの送信が確実に行われるようにするには、次の推奨事項を考慮してください。

- 適切なコントロールを使って、無効な情報を送信できないようにします (つまり、カレンダーの日付には、TextBox ではなく CalendarDatePicker を使います)。 フォームでの適切な入力コントロールの選択について詳しくは、後述する「入力コントロール」セクションをご覧ください。
- TextBox コントロールを使うときは、[PlaceholderText](/uwp/api/Windows.UI.Xaml.Controls.TextBox.PlaceholderText) プロパティで、望ましい入力形式のヒントをユーザーに提供します。
- [InputScope](/uwp/api/windows.ui.xaml.input.inputscope) プロパティでコントロールの想定される入力を指定することで、適切なスクリーン キーボードをユーザーに提供します。
- ラベルにアスタリスク * を付けることで、必須の入力であることを示します。
- 必須の情報がすべて入力されるまで、送信ボタンを無効にしておきます。
- 送信時に無効なデータがある場合は、フィールドを強調表示したりフィールドに境界線を表示したりすることでコントロールの入力が無効であることを示し、ユーザーにフォームの再送信を要求します。
- ネットワーク接続障害などの他のエラーの場合は、適切なエラー メッセージをユーザーに表示するようにします。 


## <a name="layout"></a>レイアウト

ユーザー エクスペリエンスを使いやすくして、ユーザーが確実に正しく入力できるようにするには、フォームのレイアウトをデザインするときに次の推奨事項を考慮してください。 

### <a name="labels"></a>ラベル
[ラベル](labels.md)は左揃えにして、入力コントロールの上に配置する必要があります。 多くのコントロールには、ラベルを表示するための Header プロパティが組み込まれています。 Header プロパティがないコントロールの場合、またはコントロールのグループにラベルを付ける場合は、代わりに [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) を使います。

[アクセシビリティ対応の設計にする](../accessibility/accessibility.md)には、個別のコントロールとコントロールのグループのすべてに、人間とスクリーン リーダーの両方のためにわかりやすいラベルを付けます。 

フォント スタイルには既定の [Windows 書体見本](../style/typography.md)を使います。 ページのタイトルには `TitleTextBlockStyle` を、グループの見出しには `SubtitleTextBlockStyle` を、コントロールのラベルには `BodyTextBlockStyle` を使います。

<div class="mx-responsive-img">
<table>
<tr><th>推奨</th><th>非推奨</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-shortform1col.png" alt="form with top labels"></td>
<td><img src="../controls-and-patterns/images/forms-leftlabel-donot1.png" alt="form with left labels don't"></td>
</tr>
</table>
</div>

### <a name="spacing"></a>間隔
コントロールのグループを視覚的に相互に分離するには、[配置、余白、パディング](../layout/alignment-margin-padding.md)を使います。 個々の入力コントロールの高さは 80 ピクセルであり、24 ピクセルの間を空ける必要があります。 入力コントロールのグループの間には、48 ピクセルのスペースを設ける必要があります。

![フォームのグループ](images/forms-groups.png)

### <a name="columns"></a>列
列を作成すると、フォームの不要な空白を減らすことができます (特に大きい画面サイズの場合)。 ただし、複数列のフォームを作成する場合は、ページ上の入力コントロールの数と、アプリ ウィンドウの画面サイズに応じて、列の数を決める必要があります。 画面に多数の入力コントロールを詰め込むのではなく、複数ページのフォームの作成を検討します。  

<div class="mx-responsive-img">
<table>
<tr><th>推奨</th><th>非推奨</th></tr>
<tr>
<td><img src="../controls-and-patterns/images/forms-2cols.png" alt="form with 2 columns"></td>
<td><img src="../controls-and-patterns/images/forms-2cols-bad.png" alt="form with 2 bad columns"></td>
</tr>
<tr><td><img src="../controls-and-patterns/images/forms-3cols.png" alt="form with 3 columns"></td></tr>
</table>

</div>

### <a name="responsive-layout"></a>レスポンシブ レイアウト
画面やウィンドウのサイズが変化したらフォームのサイズを変更し、ユーザーが入力フィールドを見落とさないようにする必要があります。 詳しくは、「[レスポンシブ デザインの手法](../layout/responsive-design.md)」をご覧ください。 たとえば、画面がどのようなサイズであっても、フォームの特定の領域が常に表示されているようにします。

![フォームのフォーカス](images/forms-focus2.png)

### <a name="tab-stops"></a>タブ位置
ユーザーは、キーボードを使って[タブ ストップ](../input/keyboard-interactions.md#tab-stops)でコントロール間を移動できます。 既定では、コントロールのタブ オーダーには、XAML でのコントロールの作成順序が反映されます。 既定の動作をオーバーライドするには、コントロールの **IsTabStop** または **TabIndex** プロパティを変更します。 

![フォーム内のコントロールに対するタブ フォーカス](images/forms-focus1.png)

## <a name="input-controls"></a>入力コントロール
入力コントロールは、ユーザーがフォームに情報を入力できる UI 要素です。 フォームに追加できる一般的なコントロールと、それらを使用する状況に関する情報と、以下に示します。

### <a name="text-input"></a>テキスト入力
Control | vmmblue_2 | 例
 - | - | -
[TextBox](text-box.md) | 1 行または複数行のテキストをキャプチャします | 名前、自由形式の応答、フィードバック
[PasswordBox](password-box.md) | 文字を非表示にすることで、プライベート データを収集します | パスワード、社会保障番号 (SSN)、暗証番号、クレジット カード情報 
[AutoSuggestBox](auto-suggest-box.md) | ユーザーが入力した対応するデータのセットから候補の一覧をユーザーに示します | データベース検索、メールの宛先行、前のクエリ
[RichEditBox](rich-edit-box.md) | 書式設定されたテキスト、ハイパーリンク、およびイメージでテキスト ファイルを編集します | アップロード ファイル、プレビュー、アプリでの編集

### <a name="selection"></a>選択
Control | vmmblue_2 | 例
- | - | - 
| [CheckBox](checkbox.md) | 1 つまたは複数のアクション項目を選択または選択解除します | 使用条件への同意、省略可能な項目の追加、該当するものすべての選択
[RadioButton](radio-button.md) | 2 つ以上の選択肢から 1 つのオプションを選択します | 種類の選択、出荷方法など
[ToggleSwitch](toggles.md) | 相互排他の 2 つのオプションのいずれかを選択します | オン/オフ

> **注**:5 つ以上の選択項目がある場合は、リスト コントロールを使います。

### <a name="lists"></a>リスト
Control | vmmblue_2 | 例
- | - | -
[ComboBox](combo-box.md) | コンパクトな状態で開始し、展開して選択可能な項目の一覧を表示します | 州や国などの長い項目一覧からの選択
[ListView](./lists.md#list-views) | 項目の分類とグループ ヘッダーの割り当て、項目のドラッグ アンド ドロップ、コンテンツの整理、項目の順序変更を行います | ランク オプション
[GridView](./lists.md#grid-views) | イメージ ベースのコレクションを配置および参照します | 写真、色、表示テーマの選択

### <a name="numeric-input"></a>数値入力
Control | vmmblue_2 | 例
- | - | -
[スライダー](slider.md) | 連続する数値の範囲から数を選択します | パーセンテージ、ボリューム、再生速度
[Rating](rating.md) | 星で評価します | カスタマー フィードバック

### <a name="date-and-time"></a>日時

Control | vmmblue_2 
- | - 
[CalendarView](calendar-view.md) | 常に表示されるカレンダーから 1 つの日付または日付の範囲を選択します 
[CalendarDatePicker](calendar-date-picker.md) | コンテキストに対応するカレンダーから 1 つの日付を選択します 
[DatePicker](date-picker.md) | コンテキスト情報が重要ではないときに、ローカライズされた 1 つの日付を選択します
[TimePicker](time-picker.md) | 1 つの時刻の値を選択します

### <a name="additional-controls"></a>その他のコントロール 
UWP コントロールの完全な一覧については、[機能別コントロールのインデックス](controls-by-function.md)に関する記事をご覧ください。

さらに複雑な UI コントロールおよびカスタム UI コントロールについては、[Telerik](https://www.telerik.com/)、[SyncFusion](https://www.syncfusion.com/uwp-ui-controls)、[DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)、[Infragistics](https://www.infragistics.com/products/universal-windows-platform)、[ComponentOne](https://www.componentone.com/Studio/Platform/UWP)、[ActiPro](https://www.actiprosoftware.com/products/controls/universal) などの企業から入手できるリソースをご覧ください。

## <a name="one-column-form-example"></a>1 列のフォームの例
この例では、Acrylic の[マスター/詳細](master-details.md)[リスト ビュー](lists.md)および [NavigationView](navigationview.md) コントロールを使います。
![別のフォームの例のスクリーンショット](images/FormExample2.png)
```xaml
<StackPanel>
    <TextBlock Text="New Customer" Style="{StaticResource TitleTextBlockStyle}"/>
    <Button Height="100" Width="100" Background="LightGray" Content="Add photo" Margin="0,24" Click="AddPhotoButton_Click"/>
    <TextBox x:Name="Name" Header= "Name" Margin="0,24,0,0" MaxLength="32" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
    <TextBox x:Name="PhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="15" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
    <TextBox x:Name="Email" Header="Email" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
    <RelativePanel>
        <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="EmailNameOrAddress" />
        <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
             <x:String>WA</x:String>
        </ComboBox>
    </RelativePanel>
    <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Margin="0,24" Click="SaveButton_Click"/>
        <Button Content="Cancel" Margin="24" Click="CancelButton_Click"/>
    </StackPanel>  
</StackPanel>
```

## <a name="two-column-form-example"></a>2 列のフォームの例
この例では、入力コントロールに加えて [Pivot](pivot.md) コントロール、[Acrylic](../style/acrylic.md) 背景、[CommandBar](app-bars.md) を使います。
![フォームの例のスクリーンショット](images/FormExample.png)
```xaml
<Grid>
    <Pivot Background="{ThemeResource SystemControlAccentAcrylicWindowAccentMediumHighBrush}" >
        <Pivot.TitleTemplate>
            <DataTemplate>
                <Grid>
                    <TextBlock Text="Company Name" Style="{ThemeResource HeaderTextBlockStyle}"/>
                </Grid>
            </DataTemplate>
        </Pivot.TitleTemplate>
        <PivotItem Header="Orders" Margin="0"/>    
        <PivotItem Header="Customers" Margin="0">
            <!--Form Example-->
            <Grid Background="White">
                <RelativePanel>
                    <StackPanel x:Name="Customer" Margin="20">
                        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="CustomerPhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="AlphanumericFullWidth" />
                        <RelativePanel>
                            <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" MaxLength="50" Width="200" HorizontalAlignment="Left" InputScope="Text" />
                            <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0"  Width="100" RelativePanel.RightOf="City">
                                <x:String>WA</x:String>
                            </ComboBox>
                        </RelativePanel>
                        <TextBox x:Name="ZipCode" PlaceholderText="Zip Code" Margin="0,24,0,0" MaxLength="6" Width="100" HorizontalAlignment="Left" InputScope="Number" />
                    </StackPanel>
                    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
                        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" MaxLength="320" Width="400" HorizontalAlignment="Left" InputScope="PersonalFullName"/>
                        <TextBox x:Name="AssociatePhoneNumber" Header="Phone Number" Margin="0,24,0,0" MaxLength="50" Width="400" HorizontalAlignment="Left" InputScope="TelephoneNumber" />
                        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
                        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
                    </StackPanel>
                </RelativePanel>
            </Grid>
        </PivotItem>
        <PivotItem Header="Invoices"/>
        <PivotItem Header="Stock"/>
        <Pivot.RightHeader>
            <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit" />
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
            </CommandBar>
        </Pivot.RightHeader>
    </Pivot>
</Grid>
```

## <a name="customer-orders-database-sample"></a>顧客注文データベースのサンプル
![顧客注文データベースのスクリーンショット](images/customerorderform.png) フォーム入力を **Azure** データベースに接続する方法、および完全に実装されたフォームについては、[顧客注文データベース](https://github.com/Microsoft/Windows-appsample-customers-orders-database) アプリのサンプルをご覧ください。

## <a name="related-topics"></a>関連トピック
- [入力コントロール](controls-and-events-intro.md)
- [文字体裁](../style/typography.md)
