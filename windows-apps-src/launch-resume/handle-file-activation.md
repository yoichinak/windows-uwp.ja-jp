---
title: ファイルのアクティブ化の処理
description: アプリは、特定のファイルの種類の既定のハンドラーとして登録することができます。
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 0c334caa159b423771bfe64cf7c1a769d5f84c99
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164786"
---
# <a name="handle-file-activation"></a>ファイルのアクティブ化の処理

**重要な API**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated)

アプリは、特定のファイルの種類の既定のハンドラーになるように登録できます。 Windows デスクトップ アプリケーションとユニバーサル Windows プラットフォーム (UWP) アプリの両方を、既定のファイル ハンドラーとして登録できます。 ユーザーがアプリを特定のファイルの種類の既定のハンドラーとして選ぶと、アプリはその種類のファイルを起動したときにアクティブ化されます。

ファイルの種類に登録するのは、その種類のファイルのすべてのファイル起動を処理する場合のみにすることをお勧めします。 アプリをそのファイルの種類に内部的にのみ使う場合、既定のハンドラーに登録する必要はありません。 ファイルの種類に登録する場合は、そのファイルの種類のためにアプリをアクティブ化した際に期待される機能をエンド ユーザーに提供する必要があります。 たとえば、.jpg ファイルを表示する画像ビューアー アプリを登録できます。 ファイルの関連付けについて詳しくは、「[ファイルの種類と URI のガイドライン](../files/index.md)」をご覧ください。

以下の手順では、カスタムのファイルの種類 .alsdk を登録する方法と、ユーザーによって .alsdk ファイルが起動されたときにアプリをアクティブ化する方法について説明します。

> **メモ**   UWP アプリでは、組み込みアプリとオペレーティングシステムで使用するために、特定の Uri とファイル拡張子が予約されています。 予約されている URI またはファイル拡張子にアプリを登録しようとしても無視されます。 詳しくは、「[予約済みのファイルと URI スキーム名](reserved-uri-scheme-names.md)」をご覧ください。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>ステップ 1: パッケージ マニフェストに拡張点を指定する

アプリは、パッケージ マニフェストに一覧表示されるファイル拡張子のアクティブ化イベントだけを受け取ります。 アプリが `.alsdk` 拡張子を持つファイルを処理することを示す方法は次のとおりです。

1.  **ソリューション エクスプローラー**で、package.appxmanifest をダブルクリックしてマニフェスト デザイナーを開きます。 **[宣言]** タブを選び、**[使用可能な宣言]** ドロップダウンから **[ファイルの種類の関連付け]** を選んで **[追加]** をクリックします。 ファイルの関連付けで使われる識別子について詳しくは、「[プログラムの識別子](/windows/desktop/shell/fa-progids)」をご覧ください。

    マニフェスト デザイナーで指定することができる各フィールドについて、以下で簡単に説明します。

| フィールド | 説明 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **表示名** | ファイルの種類のグループの表示名を指定します。 表示名は、**コントロール パネル**の [[既定のプログラムを設定する]](/windows/desktop/shell/default-programs) でファイルの種類を識別するために使われます。 |
| **ロゴ** | デスクトップと**コントロール パネル**の [[既定のプログラムを設定する]](/windows/desktop/shell/default-programs) でファイルの種類を識別するために使われるロゴを指定します。 ロゴを指定しない場合は、アプリケーションの小さいロゴが使われます。 |
| **InfoTip** | ファイルの種類のグループの [InfoTip](/windows/desktop/shell/fa-progids) を指定します。 このヒントのテキストは、ユーザーがこの種類のファイルのアイコンの上にマウス ポインターを置くと表示されます。 |
| **名前** | 同じ表示名、ロゴ、InfoTip、編集フラグを共有するファイルの種類のグループの名前を選びます。 このグループ名は、アプリの更新後も維持される名前にします。 **注**  名前はすべて小文字である必要があります。 |
| **コンテンツの種類** | 特定のファイルの種類の MIME コンテンツの種類 (**image/jpeg** など) を指定します。 **許可されるコンテンツの種類に関する重要な注意事項:** 次に示すのは、パッケージマニフェストに入力できない MIME コンテンツタイプのアルファベット順です。 **アプリケーション/フォースダウンロード**、 **アプリケーション/オクテットストリーム**、 **アプリケーション/不明**、 **アプリケーション/x-msdownload**のいずれかです。 |
| **ファイルの種類** | 登録するファイルの種類を指定します。先頭にはピリオドを付けます (例: ".jpeg")。 **予約および禁止されているファイルの種類:** 予約または禁止されているために UWP アプリを登録できない組み込みアプリ用のファイルの種類の一覧 (アルファベット順) については、「[予約済みのファイルと URI スキーム名](reserved-uri-scheme-names.md)」をご覧ください。 |

2.  `alsdk`**名前**として「」と入力します。
3.  **[ファイルの種類]** に `.alsdk` と入力します。
4.  \\ロゴとして「imagesIcon.png」と入力します。
5.  Ctrl + S キーを押して、変更を package.appxmanifest に保存します。

上記の手順により、次のような [**Extension**](/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 要素がパッケージ マニフェストに追加されます。 **windows.fileTypeAssociation** カテゴリは、アプリが `.alsdk` 拡張子を持つファイルを処理することを示しています。

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>ステップ 2: 適切なアイコンを追加する

ファイルの種類の既定となるアプリは、そのアイコンがシステムのさまざまな場所に表示されます。 アイコンは、たとえば次の場所に表示されます。

-   エクスプローラーの項目ビュー、コンテキスト メニュー、リボン
-   [既定のプログラム] コントロール パネル
-   ファイル ピッカー
-   スタート画面での検索結果

ロゴがこれらの場所に表示されるように、プロジェクトに 44 x 44 のアイコンを含めます。 アプリのタイルのロゴの外観を調和させ、アイコンを透明にするのではなく、アプリの背景色を使います。 パディングせずにロゴを端まで拡張します。 アイコンは、白い背景でテストします。 アイコンについて詳しくは、「[タイルとアイコン アセットのガイドライン](../design/style/app-icons-and-logos.md)」をご覧ください。

## <a name="step-3-handle-the-activated-event"></a>ステップ 3: アクティブ化イベントを処理する

[**OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated) イベント ハンドラーは、すべてのファイル アクティブ化イベントを受け取ります。

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> ファイルコントラクトによって起動された場合は、[戻る] ボタンをクリックすると、アプリを起動した画面に戻り、アプリの以前のコンテンツではなくなります。

新しいページを開くアクティブ化イベントごとに、新しい XAML **フレーム** を作成することをお勧めします。 このようにして、新しい XAML フレームのナビゲーションバックスタックには、中断されたときに、アプリが現在のウィンドウに保持している可能性がある以前のコンテンツは含まれません。 起動とファイルコントラクトに対して1つの XAML **フレーム** を使用する場合は、新しいページに移動する前に、 **フレーム**のナビゲーション履歴のページをクリアする必要があります。

ファイルのアクティブ化によってアプリを起動する場合は、ユーザーがアプリの一番上のページに戻ることができる UI を含めることを検討してください。

## <a name="remarks"></a>注釈

受け取るファイルは、信頼できないソースからのファイルである可能性があります。 操作する前に、ファイルのコンテンツを検証することをお勧めします。

## <a name="related-topics"></a>関連トピック

### <a name="complete-example"></a>コード例全体

* [Association Launching サンプル](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>概念

* [既定のプログラム](/windows/desktop/shell/default-programs)
* [ファイルの種類とプロトコルの関連付けのモデル](/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>タスク

* [ファイルに応じた既定のアプリの起動](launch-the-default-app-for-a-file.md)
* [URI のアクティブ化の処理](handle-uri-activation.md)

### <a name="guidelines"></a>ガイドライン

* [ファイルの種類と URI のガイドライン](../files/index.md)

### <a name="reference"></a>参照先
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows.UI.Xaml.Application.OnFileActivated](/uwp/api/windows.ui.xaml.application.onfileactivated)