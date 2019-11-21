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
ms.openlocfilehash: 079746d3c1619fe940ba243410f0247b7b850ed9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259455"
---
# <a name="handle-file-activation"></a>ファイルのアクティブ化の処理

**重要な API**

-   [**FileActivatedEventArgs (Windows. ApplicationModel)** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
-   [**Windows. UI. .Xaml. Application. OnFileActivated 化**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)

アプリは、特定のファイルの種類の既定のハンドラーになるように登録できます。 Windows デスクトップ アプリケーションとユニバーサル Windows プラットフォーム (UWP) アプリの両方を、既定のファイル ハンドラーとして登録できます。 ユーザーがアプリを特定のファイルの種類の既定のハンドラーとして選ぶと、アプリはその種類のファイルを起動したときにアクティブ化されます。

ファイルの種類に登録するのは、その種類のファイルのすべてのファイル起動を処理する場合のみにすることをお勧めします。 アプリをそのファイルの種類に内部的にのみ使う場合、既定のハンドラーに登録する必要はありません。 ファイルの種類に登録する場合は、そのファイルの種類のためにアプリをアクティブ化した際に期待される機能をエンド ユーザーに提供する必要があります。 たとえば、.jpg ファイルを表示する画像ビューアー アプリを登録できます。 ファイルの関連付けについて詳しくは、「[ファイルの種類と URI のガイドライン](https://docs.microsoft.com/windows/uwp/files/index)」をご覧ください。

以下の手順では、カスタムのファイルの種類 .alsdk を登録する方法と、ユーザーによって .alsdk ファイルが起動されたときにアプリをアクティブ化する方法について説明します。

> **注**  UWP アプリでは、特定の uri とファイル拡張子は、組み込みアプリとオペレーティングシステムで使用するために予約されています。 予約されている URI またはファイル拡張子にアプリを登録しようとしても無視されます。 詳しくは、「[予約済みのファイルと URI スキーム名](reserved-uri-scheme-names.md)」をご覧ください。

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>ステップ 1: パッケージ マニフェストに拡張点を指定する

アプリは、パッケージ マニフェストに一覧表示されるファイル拡張子のアクティブ化イベントだけを受け取ります。 アプリが `.alsdk` 拡張子を持つファイルを処理することを示す方法は次のとおりです。

1.  **ソリューション エクスプローラー**で、package.appxmanifest をダブルクリックしてマニフェスト デザイナーを開きます。 **[宣言]** タブを選び、 **[使用可能な宣言]** ドロップダウンから **[ファイルの種類の関連付け]** を選んで **[追加]** をクリックします。 ファイルの関連付けで使われる識別子について詳しくは、「[プログラムの識別子](https://docs.microsoft.com/windows/desktop/shell/fa-progids)」をご覧ください。

    マニフェスト デザイナーで指定することができる各フィールドについて、以下で簡単に説明します。

| フィールド | 説明 |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **表示名** | ファイルの種類のグループの表示名を指定します。 表示名は、[コントロール パネル](https://docs.microsoft.com/windows/desktop/shell/default-programs)の **[既定のプログラムを設定する]** でファイルの種類を識別するために使われます。 |
| **Logo** | デスクトップと[コントロール パネル](https://docs.microsoft.com/windows/desktop/shell/default-programs)の **[既定のプログラムを設定する]** でファイルの種類を識別するために使われるロゴを指定します。 ロゴを指定しない場合は、アプリケーションの小さいロゴが使われます。 |
| **情報ヒント** | ファイルの種類のグループの [InfoTip](https://docs.microsoft.com/windows/desktop/shell/fa-progids) を指定します。 このヒントのテキストは、ユーザーがこの種類のファイルのアイコンの上にマウス ポインターを置くと表示されます。 |
| **名前** | 同じ表示名、ロゴ、InfoTip、編集フラグを共有するファイルの種類のグループの名前を選びます。 このグループ名は、アプリの更新後も維持される名前にします。 **注**  名前はすべて小文字である必要があります。 |
| **コンテンツの種類** | 特定のファイルの種類の MIME コンテンツの種類 (**image/jpeg** など) を指定します。 **許可されるコンテンツの種類に関する重要な注意:** MIME コンテンツの種類のうち、**application/force-download**、**application/octet-stream**、**application/unknown**、**application/x-msdownload** は予約または禁止されているため、パッケージ マニフェストに入力できません。 |
| **ファイルの種類** | 登録するファイルの種類を指定します。先頭にはピリオドを付けます (例: ".jpeg")。 **予約および禁止されているファイルの種類:** 予約または禁止されているために UWP アプリを登録できない組み込みアプリ用のファイルの種類の一覧 (アルファベット順) については、「[予約済みのファイルと URI スキーム名](reserved-uri-scheme-names.md)」をご覧ください。 |

2.  `alsdk`[名前]**に** と入力します。
3.  `.alsdk`[ファイルの種類]**に** と入力します。
4.  ロゴとして「images\\Icon .png」と入力します。
5.  Ctrl + S キーを押して、変更を package.appxmanifest に保存します。

上記の手順により、次のような [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) 要素がパッケージ マニフェストに追加されます。 **windows.fileTypeAssociation** カテゴリは、アプリが `.alsdk` 拡張子を持つファイルを処理することを示しています。

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

ロゴがこれらの場所に表示されるように、プロジェクトに 44 x 44 のアイコンを含めます。 アプリのタイルのロゴの外観を調和させ、アイコンを透明にするのではなく、アプリの背景色を使います。 パディングせずにロゴを端まで拡張します。 アイコンは、白い背景でテストします。 アイコンについて詳しくは、「[タイルとアイコン アセットのガイドライン](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets)」をご覧ください。

## <a name="step-3-handle-the-activated-event"></a>ステップ 3: アクティブ化イベントを処理する

[  **OnFileActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated) イベント ハンドラーは、すべてのファイル アクティブ化イベントを受け取ります。

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

新しいページを開くアクティブ化イベントごとに、新しい XAML**フレーム**を作成することをお勧めします。 このようにして、新しい XAML フレームのナビゲーションバックスタックには、中断されたときに、アプリが現在のウィンドウに保持している可能性がある以前のコンテンツは含まれません。 起動とファイルコントラクトに対して1つの XAML**フレーム**を使用する場合は、新しいページに移動する前に、**フレーム**のナビゲーション履歴のページをクリアする必要があります。

ファイルのアクティブ化によってアプリを起動する場合は、ユーザーがアプリの一番上のページに戻ることができる UI を含めることを検討してください。

## <a name="remarks"></a>注釈

受け取るファイルは、信頼できないソースからのファイルである可能性があります。 操作する前に、ファイルのコンテンツを検証することをお勧めします。

## <a name="related-topics"></a>関連トピック

### <a name="complete-example"></a>完全な例

* [アソシエーションの開始のサンプル](https://code.msdn.microsoft.com/windowsapps/Association-Launching-535d2cec)

### <a name="concepts"></a>概念

* [既定のプログラム](https://docs.microsoft.com/windows/desktop/shell/default-programs)
* [ファイルの種類とプロトコルの関連付けモデル](https://docs.microsoft.com/windows/desktop/w8cookbook/file-type-and-protocol-associations-model)

### <a name="tasks"></a>タスク

* [ファイルに応じた既定のアプリの起動](launch-the-default-app-for-a-file.md)
* [URI のアクティブ化の処理](handle-uri-activation.md)

### <a name="guidelines"></a>ガイドライン

* [ファイルの種類と Uri に関するガイドライン](https://docs.microsoft.com/windows/uwp/files/index)

### <a name="reference"></a>リファレンス
* [FileActivatedEventArgs (Windows. ApplicationModel)](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.FileActivatedEventArgs)
* [Windows. UI. .Xaml. Application. OnFileActivated 化](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onfileactivated)
