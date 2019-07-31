---
Description: 隣接するコントロールに入力する必要がある内容をユーザーに説明するためにラベルを使います。 また、関連するコントロールのグループにラベルを付けることや、関連するコントロールのグループの近くに説明テキストを表示することができます。
title: ラベル
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2b642f1d6c3f2a04bacdf293858492ea095af1a8
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319527"
---
# <a name="labels"></a>ラベル

 

ラベルは、コントロールまたは関連するコントロールのグループの名前やタイトルです。

> **重要な API**:Header プロパティ、[TextBlock クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

XAML では、多くのコントロールに組み込みの Header プロパティがあり、これを使ってラベルを表示します。 Header プロパティがないコントロールの場合、またはコントロールのグループにラベルを付ける場合は、代わりに [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) を使います。

![標準的なラベル コントロールを示すスクリーンショット](images/label-standard.png)

## <a name="recommendations"></a>推奨事項


-   隣接するコントロールに入力する必要がある内容をユーザーに説明するためにラベルを使います。 また、関連するコントロールのグループにラベルを付けることや、関連するコントロールのグループの近くに説明テキストを表示することができます。
-   コントロールにラベルを付ける場合、説明テキストの文ではなく、名詞や簡潔な名詞句のラベルを入力します。 コロン、その他の句読点は使わないでください。
-   ラベルに説明テキストを入力するときは、テキスト文字列を長くすることができ、句読点も使うことができます。


## <a name="get-the-sample-code"></a>サンプル コードを入手する
* [XAML UI の基本のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>関連トピック
* [テキスト コントロール](text-controls.md)
* [TextBox.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header プロパティ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock クラス](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 




