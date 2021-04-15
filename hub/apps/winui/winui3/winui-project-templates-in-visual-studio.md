---
title: WinUI 3 プロジェクトを作成する
description: Visual Studio で使用できる WinUI 3 プロジェクト テンプレートと項目テンプレートの概要。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 823a8698d2d97207a69b31655437d02b70d05857
ms.sourcegitcommit: 99f7edfd79c44768751aca02dab30a03f41d2aae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218217"
---
# <a name="winui-3-project-templates-in-visual-studio"></a>Visual Studio での WinUI 3 プロジェクト テンプレート

Project Reunion 0.5 VSIX パッケージをインストールすると、Visual Studio の WinUI プロジェクト テンプレートのいずれかを使用して WinUI 3 アプリを作成できるようになります。 **[新しいプロジェクトの作成]** ダイアログで WinUI プロジェクト テンプレートにアクセスするには、言語を **C++** または **C#** に、プラットフォームを **Windows** に、プロジェクトの種類を **WinUI** にフィルター処理します。 または、*WinUI* を検索して、使用できる C# または C++ テンプレートのいずれかを選択することもできます。

![WinUI プロジェクト テンプレート](images/winui3-csharp-newproject.png)

> [!NOTE] 
> 開発環境の設定ガイダンス、既知の問題などについては、最新の[一連のリリース ノート](../index.md)を参照してください。

Project Reunion 0.5 リリースの一部として利用できる Visual Studio 拡張機能が 2 つあります。**Project Reunion VSIX** と **Project Reunion Preview VSIX** です。 Project Reunion VSIX には、MSIX パッケージ デスクトップ運用アプリの構築に使用できるプロジェクト テンプレートが含まれています。 Project Reunion Preview VSIX には、MSIX パッケージ デスクトップ アプリや UWP アプリの構築に使用できる試験的プロジェクト テンプレートが含まれています。 

## <a name="project-templates-for-winui-3"></a>WinUI 3 用のプロジェクト テンプレート

これらの WinUI プロジェクト テンプレートを使用してアプリを作成することができます。

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| 空のアプリ、パッケージ (WinUI 3 in Desktop) | C++ および C# | WinUI ベースのユーザー インターフェイスを使用して、デスクトップ .NET 5 (C#) またはネイティブ Win32 (C++) アプリを作成します。 生成されるプロジェクトには、UI の構築を始める際に使用できる WinUI ライブラリの **Microsoft.UI.Xaml.Window** クラスから派生する基本ウィンドウが含まれています。 このプロジェクトの種類の詳細については、「[デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)」を参照してください。<p></p>また、このソリューションには、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むよう構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)も含まれています。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。  |
| 空のアプリ (WinUI 3 in UWP)、試験的 | C++ および C# | WinUI ベースのユーザー インターフェイスを備えた UWP アプリを作成します。 生成されるプロジェクトには、UI の構築を始める際に使用できる、WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Page** クラスから派生した基本的なページが含まれています。 このプロジェクトの種類の詳細については、「[UWP アプリ用の WinUI 3 の概要](get-started-winui3-for-uwp.md)」を参照してください。 |

これらの WinUI プロジェクト テンプレートを使用すると、WinUI ベースのアプリで読み込んで使用できるコンポーネントを構築することができます。

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| クラス ライブラリ (WinUI 3 in Desktop) | C# のみ | WinUI ベースのユーザー インターフェイスを備えた他の .NET 5 デスクトップ アプリで使用できる .NET 5 マネージド クラス ライブラリ (DLL) を C# で作成します。  |
| クラス ライブラリ (WinUI 3 in UWP)、試験的  | C# のみ | WinUI ベースのユーザー インターフェイスを備えた他の UWP アプリで使用できるマネージド クラス ライブラリ (DLL) を C# で作成します。 |
| Windows ランタイム コンポーネント (WinUI 3) | C++ | C++/WinRT で記述した [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。これは、WinUI ベースのユーザー インターフェイスを備えた UWP またはデスクトップ アプリであれば、アプリ作成に使用されたプログラミング言語に関係なく使用できます。 |
| Windows ランタイム コンポーネント (WinUI 3 in UWP)、試験的 | C# | C# で記述された [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。これは、アプリの記述に使用したプログラミング言語に関係なく、WinUI ベースのユーザー インターフェイスを備えた任意の UWP アプリで使用できます。 |

## <a name="item-templates-for-winui-3"></a>WinUI 3 用の項目テンプレート

次の項目テンプレートは、WinUI プロジェクトで使用できます。 これらの WinUI 項目テンプレートにアクセスするには、**ソリューション エクスプローラー** でプロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択し、 **[新しい項目の追加]** ダイアログの **[WinUI]** をクリックします。

![WinUI の項目テンプレート](images/winui3-addnewitem.png)

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| 空白のページ (WinUI 3) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Page** クラスから派生する新しいページを定義する XAML ファイルとコード ファイルを追加します。 |
| 空白のウィンドウ (WinUI 3 in Desktop) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Window** クラスから派生する新しいウィンドウを定義する XAML ファイルとコード ファイルを追加します。 |
| カスタム コントロール (WinUI 3) | C++ および C# | 既定スタイルを使用してテンプレート化されたコントロールを作成するためのコード ファイルを追加します。 テンプレート化されたコントロールは、WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Control** クラスから派生します。<p></p>この項目テンプレートの使用方法を示すチュートリアルについては、「[C++/WinRT を使用した UWP および WinUI 3 アプリ用のテンプレート化された XAML コントロール](xaml-templated-controls-cppwinrt-winui-3.md)」および「[C# を使用した UWP および WinUI 3 アプリ用のテンプレート化された XAML コントロール](xaml-templated-controls-csharp-winui-3.md)」を参照してください。 テンプレート化されたコントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |
| リソース ディクショナリ (WinUI 3) | C++ および C# | 空の、XAML リソースのキー付きコレクションを追加します。 詳細については、「[ResourceDictionary と XAML リソースの参照](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」を参照してください。 |
| リソース ファイル (WinUI 3) | C++ および C# | アプリ用の文字列および条件付きリソースを格納するファイルを追加します。 この項目を使用して、アプリをローカライズすることができます。 詳細については、「[UI とアプリ パッケージ マニフェスト内の文字列をローカライズする](/windows/uwp/app-resources/localize-strings-ui-manifest)」を参照してください。 |
| ユーザー コントロール (WinUI 3) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.UserControl** クラスから派生するユーザー コントロールを作成するための XAML ファイルとコード ファイルを追加します。 通常、ユーザー コントロールによって関連する既存のコントロールがカプセル化され、独自のロジックが提供されます。<p></p>ユーザー コントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |
