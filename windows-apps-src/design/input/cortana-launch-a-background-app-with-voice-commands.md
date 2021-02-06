---
title: 音声コマンドを使用して Cortana でバックグラウンドアプリをアクティブ化する |Cortana UWP の設計と開発
description: 音声コマンドを使用して、アプリの機能を使用して Cortana を (バックグラウンドタスクとして) 拡張します。
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: Cortana
ms.openlocfilehash: f1ed51107f41318cecf2d8fea73484713b4b837c
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606067"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>音声コマンドを使用して Cortana でバックグラウンドアプリをアクティブ化する  

>[!WARNING]
> この機能は、Windows 10 2020 年5月の更新プログラム (バージョン2004、コードネーム "20H1") ではサポートされなくなりました。
>
> Cortana が最新の生産性エクスペリエンスを変革する方法については [、Microsoft 365 の cortana](/microsoft-365/admin/misc/cortana-integration) を参照してください。

**Cortana** 内の音声コマンドを使用してシステム機能にアクセスするだけでなく、実行するアクションまたはコマンドを指定する音声コマンドを使用して、(バックグラウンドタスクとして) アプリの機能を使用して **cortana** を拡張することもできます。 アプリはバックグラウンドで音声コマンドを処理するとき、フォーカスを取得しません。 その代わり、**Cortana** キャンバスと **Cortana** 音声を介して、すべてのフィードバックと結果が返されます。  

> [!NOTE]
> **重要な API**
>
> - [**Windows.ApplicationModel.VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

アプリは、対話の複雑さに応じて、フォアグラウンドにアクティブ化する (アプリがフォーカスを取得する) か、バックグラウンドでアクティブ化することができます (**Cortana** はフォーカスを保持します)。 たとえば、追加のコンテキストまたはユーザー入力 (特定の連絡先へのメッセージの送信など) を必要とする音声コマンドは、フォアグラウンドアプリで処理することをお勧めします。一方、基本的なコマンド (今後の旅行の一覧表示など) は、バックグラウンドアプリを使用して **Cortana** で処理できます。  

音声コマンドを使用してフォアグラウンドでアプリをアクティブ化する場合は、「[Cortana の音声コマンドを使ったフォアグラウンド アプリのアクティブ化](cortana-launch-a-foreground-app-with-voice-commands.md)」をご覧ください。  

> [!NOTE]
> 音声コマンドは、音声コマンド定義 (VCD) ファイルで定義されている特定のインテントを持つ単一の (発話) であり、インストールされているアプリに **Cortana** 経由で送信されます。
>
> VCD ファイルでは、1 つ以上の音声コマンドが定義されており、各音声コマンドは固有の目的を持っています。
>
> 音声コマンドの定義は、複雑さによって異なります。 1つの制約された (発話) から、より柔軟な自然言語発話のコレクションに至るまで、あらゆることをサポートできます。これはすべて同じ目的を意味します。

ここでは、**Cortana** UI に統合されている **Adventure Works** という旅行の計画および管理アプリを使って、さまざまな概念や機能について説明します。 詳細については、 [Cortana voice コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)を参照してください。

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 起動時アプリのスクリーンショット":::

**Cortana** を使わないで **Adventure Works** の旅行を表示するには、アプリを起動して **[Upcoming trips]** (今後の旅行) ページに移動します。  

**Cortana** で音声コマンドを使用して、バックグラウンドでアプリを起動すると、ユーザーはとしてのようにすることができ `Adventure Works, when is my trip to Las Vegas?` ます。 アプリによりコマンドが処理され、**Cortana** にアプリ アイコンや他のアプリ情報 (ある場合) と共に結果が表示されます。  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="バックグラウンドで AdventureWorks アプリを使用して基本的なクエリと結果画面を含む Cortana のスクリーンショット":::

次の基本的な手順では、音声コマンド機能を追加し、音声またはキーボード入力を使用して、アプリの背景機能を使用して **Cortana** を拡張します。

1. **Cortana** がバックグラウンドで呼び出すアプリ サービスを作成します (「[**Windows.ApplicationModel.AppService**](/uwp/api/Windows.ApplicationModel.AppService)」をご覧ください)。  
2. VCD ファイルを作ります。 VCD ファイルは、ユーザーがアプリをアクティブ化するときにアクションを開始したりコマンドを呼び出したりするために使用できる、すべての音声コマンドを定義する XML ドキュメントです。 「 [**VCD の要素と属性**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)v1.0」を参照してください。  
3. アプリが起動したら、VCD ファイル内のコマンド セットを登録します。  
4. App service のバックグラウンドアクティブ化と、音声コマンドの実行を処理します。  
5. **Cortana** 内で音声コマンドに対する適切なフィードバックを表示して読み上げます。  

> [!TIP]
> **前提条件**
>
> ユニバーサル Windows プラットフォーム (UWP) アプリを開発するのが初めての場合は、以下のトピックに目を通して、ここで説明されているテクノロジをよく理解できるようにしてください。
>
> - [初めてのアプリの作成](/windows/uwp/get-started/your-first-app)
> - 「[イベントとルーティング イベントの概要](/windows/uwp/xaml-platform/events-and-routed-events-overview)」に記載されているイベントの説明
>
> **ユーザーエクスペリエンスのガイドライン**
>
> **Cortana** と [音声](speech-interactions.md)による対話とアプリを統合する方法については、 [cortana のデザインガイドライン](cortana-design-guidelines.md)に関する情報を参照してください。

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>Visual Studio でプライマリプロジェクトを使用して新しいソリューションを作成する  

1. Microsoft Visual Studio 2015 を起動します。  
    Visual Studio 2015 のスタート画面が表示されます。  

2. **[ファイル]** メニューで、 **[新規作成]**  >  **[プロジェクト]** の順に選択します  
    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。 ダイアログの左側のウィンドウで、表示するテンプレートの種類を選択できます。  

3. 左側のウィンドウで、[ **インストールされている > テンプレート > Visual C \# > Windows**] を展開し、[ **ユニバーサル** テンプレート] グループを選択します。 ダイアログの中央のウィンドウには、ユニバーサル Windows プラットフォーム (UWP) アプリ用のプロジェクトテンプレートの一覧が表示されます。  
4. 中央のウィンドウで、**[空白のアプリ (ユニバーサル Windows)]** プロジェクト テンプレートを選びます。  
    **空のアプリケーション** テンプレートでは、コンパイルと実行を行う最小の UWP アプリが作成されます。 **空のアプリケーション** テンプレートには、ユーザーインターフェイスのコントロールもデータも含まれていません。 このページをガイドとして使用して、アプリにコントロールを追加します。  

5. **[名前]** ボックスで、プロジェクト名を入力します。 例: を使用 `AdventureWorks` します。  
6. [ **OK** ] ボタンをクリックして、プロジェクトを作成します。  
    Microsoft Visual Studio によってプロジェクトが作られ、**ソリューション エクスプローラー** に表示されます。  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>イメージアセットをプライマリプロジェクトに追加し、アプリケーションマニフェストで指定する  

UWP アプリでは、最も適切なイメージを自動的に選択する必要があります。 選択は、特定の設定とデバイスの機能 (ハイコントラスト、有効なピクセル、ロケールなど) に基づいています。 イメージを指定し、異なるリソースバージョンに対して、アプリプロジェクト内で適切な名前付け規則とフォルダー編成を使用していることを確認する必要があります。  
推奨されるリソースバージョンを指定しないと、ユーザーエクスペリエンスが次のような影響を受ける可能性があります。

- ユーザー補助
- ローカリゼーション  
- 画質  
リソースバージョンは、ユーザーエクスペリエンスの次の変更を調整するために使用されます。  
- ユーザー設定  
- 機能  
- デバイスの種類  
- 場所  

ハイコントラストとスケールファクターのイメージリソースの詳細については、 [msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)にある [タイルとアイコンのアセット] ページのガイドラインを参照してください。  

リソースには、修飾子を使用して名前を指定する必要があります。 リソース修飾子は、リソースの特定のバージョンが使われるコンテキストを識別するフォルダーとファイル名の修飾子です。  

標準の命名規則は、`foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` です。  
例: `images/logo.scale-100_contrast-white.png` 。ルートフォルダーとファイル名のみを使用してコードを参照することができます。 `images/logo.png`  
詳細については、 [msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10))にある修飾子を使用してリソースに名前を指定する方法に関するページを参照してください。  

`en-US\resources.resw` `logo.scale-100.png` 現在、ローカライズされたリソースまたは複数の解像度リソースを提供する予定がない場合でも、文字列リソースファイル (など) の既定の言語とイメージの既定のスケールファクター (など) をマークすることをお勧めします。 ただし、少なくとも、100、200、および400のスケールファクターの資産を提供することをお勧めします。  

>[!IMPORTANT]
> **Cortana** キャンバスのタイトル領域で使用されるアプリアイコンは、ファイルで指定されている Square44x44Logo アイコンです `Package.appxmanifest` 。  
> また、 **Cortana** キャンバスのコンテンツ領域の各エントリに対してアイコンを指定することもできます。 結果のアイコンに対して有効な画像のサイズは次のとおりです。
>
> - 幅 68 x 高さ 68
> - 幅 68 x 高さ 92  
> - 幅 280 x 高さ 140  

[**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)オブジェクトが [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)クラスに渡されるまで、コンテンツタイルは検証されません。 これらのサイズ比に準拠していないイメージを含むコンテンツタイルを含む **Cortana** に [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)オブジェクトを渡すと、例外が発生する可能性があります。  

例: **Adventure works** アプリ () は、 `VoiceCommandService\\AdventureWorksVoiceCommandService.cs` `GreyTile.png` [**TitleWith68x68IconAndText**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType)タイルテンプレートを使用して、 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)クラスの単純な灰色の正方形 () を指定します。 ロゴのバリエーションはにあり `VoiceCommandService\\Images` 、 [**GetFileFromApplicationUriAsync**](/uwp/api/Windows.Storage.StorageFile) メソッドを使用して取得されます。

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>App Service プロジェクトを作成する

1. ソリューション名を右クリックし、[ **新しい > プロジェクト**] を選択します。  
2. [ **インストールされている > テンプレート > Visual C \# > Windows > Universal**] の下で、[ **Windows ランタイムコンポーネント**] を選択します。 **Windows ランタイムコンポーネント** は、app Service ([**AppService**](/uwp/api/Windows.ApplicationModel.AppService)) を実装するコンポーネントです。  
3. プロジェクトの名前を入力し、[ **OK** ] ボタンをクリックします。  
    例: `VoiceCommandService`.  
4. **ソリューションエクスプローラー** で、プロジェクトを選択し、 `VoiceCommandService` `Class1.cs` Visual Studio によって生成されるファイルの名前を変更します。
    例: **Adventure works** で使用 `AdventureWorksVoiceCommandService.cs` します。  
5. [ **はい** ] ボタンをクリックします。すべてのの名前を変更するかどうかを確認するメッセージが表示された場合 `Class1.cs` 。  
6. `AdventureWorksVoiceCommandService.cs` ファイルで次の操作を行います。
    1. 次の using ディレクティブを追加します。  
        `using Windows.ApplicationModel.Background;`  
    2. 新しいプロジェクトを作成するときに、プロジェクト名は、すべてのファイルで既定のルート名前空間として使用されます。 プライマリ プロジェクトの下でアプリ サービスのコードを入れ子にするために、名前空間の名前を変更します。
        例: `namespace AdventureWorks.VoiceCommands`.  
    3. ソリューションエクスプローラーで app service プロジェクトの名前を右クリックし、[ **プロパティ**] を選択します。  
    4. [ **ライブラリ** ] タブで、[ **既定の名前空間** ] フィールドを同じ値に更新します。  
        例: `AdventureWorks.VoiceCommands`。  
    5. [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) インターフェイスを実装する新しいクラスを作成します。 このクラスには、Cortana が音声コマンドを認識するときのエントリ ポイントである [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) メソッドが必要です。  

    例: **Adventure works** アプリの基本的なバックグラウンドタスククラス。  

    >[!NOTE]
    > バックグラウンドタスクのクラス自体、およびバックグラウンドタスクプロジェクトのすべてのクラスは、シールされたパブリッククラスである必要があります。  

    ```csharp
    namespace AdventureWorks.VoiceCommands
    {
        ...
        
        /// <summary>
        /// The VoiceCommandService implements the entry point for all voice commands.
        /// The individual commands supported are described in the VCD xml file. 
        /// The service entry point is defined in the appxmanifest.
        /// </summary>
        public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
        {
            ...

            /// <summary>
            /// The background task entrypoint. 
            /// 
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
            /// </summary>
            /// <param name="taskInstance">Connection to the hosting background service process.</param>
            public void Run(IBackgroundTaskInstance taskInstance)
            {
              //
              // TODO: Insert code 
              //
              //
        }
      }
    }
    ```  

7. アプリ マニフェストでバックグラウンド タスクを **AppService** として宣言します。  
    1. **ソリューションエクスプローラー** で、ファイルを右クリック `Package.appxmanifest` し、[コードの **表示**] を選択します。  
    2. 要素を検索 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) します。  
    3. 要素 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) に要素を追加 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) します。  
    4. 要素 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) に要素を追加 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) します。  
    5. `Category`要素に属性を追加 `uap:Extension` し、属性の値 `Category` をに設定し `windows.appService` ます。  
    6. 属性を `EntryPoint` 要素に追加 `uap: Extension` し、属性の値を、を `EntryPoint` 実装するクラスの名前に設定し [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) ます。  
        例: `AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`.  
    7. 要素 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) に要素を追加 `uap:Extension` します。  
    8. `Name`要素に属性を追加 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) し、属性の値 `Name` を app service の名前 (この場合は) に設定し `AdventureWorksVoiceCommandService` ます。  
    9. 要素に2番目の要素を追加 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) します。  
    10. `Category`この要素に属性を追加 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) し、属性の値 `Category` をに設定し `windows.personalAssistantLaunch` ます。  

    例: Adventure Works アプリのマニフェスト。

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. このアプリ サービス プロジェクトをプライマリ プロジェクトの参照として追加します。  
    1. **参照** を右クリックします。  
    2. [ **参照の追加**] を選択します。  
    3. **[参照マネージャー]** ダイアログ ボックスで、**[プロジェクト]** を展開し、アプリ サービス プロジェクトを選択します。  
    4. [ **OK** ] ボタンをクリックします。  

## <a name="create-a-vcd-file"></a>VCD ファイルを作成する

1. Visual Studio で、プライマリプロジェクト名を右クリックし、[ **新しい項目の追加 >**] を選択します。 **XML ファイル** を追加します。  
2. [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)ファイルの名前を入力します。  
    例: `AdventureWorksCommands.xml`.
3. [ **追加** ] ボタンをクリックします。  
4. **ソリューション エクスプローラー** で、[**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) ファイルを選びます。  
5. [ **プロパティ** ] ウィンドウで、[ **ビルドアクション** ] を [ **コンテンツ**] に設定し、[ **出力ディレクトリにコピー** ] を [ **新しい場合はコピー** する] に設定します。  

## <a name="edit-the-vcd-file"></a>VCD ファイルを編集する  

1. `VoiceCommands`を指す属性を持つ要素を追加 `xmlns` `https://schemas.microsoft.com/voicecommands/1.2` します。  
2. アプリでサポートされている言語ごとに、 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) アプリでサポートされている音声コマンドを含む要素を作成します。  
    異なる属性を持つ複数の [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 要素を宣言して、 [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) アプリを異なる市場で使用することができます。 たとえば、米国のアプリには、 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 英語用のとスペイン語用のがあり [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) ます。  

    >[!IMPORTANT]
    > 音声コマンドを使用してアプリをアクティブ化し、アクションを開始するには、アプリは、 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) ユーザーのデバイスで指定された音声言語と一致する言語の要素を含む VCD ファイルを登録する必要があります。 音声認識言語は、[ **設定 > システム > 音声 > 音声言語**] にあります。  

3. `Command`サポートするコマンドごとに要素を追加します。  
    `Command` [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)ファイルで宣言されている各には、次の情報が含まれている必要があります。  
    - `Name`アプリケーションが実行時に音声コマンドを識別するために使用する属性。  
    - `Example`ユーザーがコマンドを呼び出す方法を説明する語句を含む要素。 **Cortana** は、ユーザーが、 `What can I say?` `Help` 、またはタップ **して [詳細表示**] をタップしたときの例を示しています。  
    - `ListenFor`アプリがコマンドとして認識する単語または語句を含む要素。 各 `ListenFor` 要素には `PhraseList` 、コマンドに関連する特定の単語を含む1つまたは複数の要素への参照を含めることができます。  

       >[!NOTE]
       > `ListenFor` プログラムによって要素を変更することはできません。 ただし、 `PhraseList` 要素に関連付けられている要素は、 `ListenFor` プログラムによって変更される場合があります。 アプリケーションでは、 `PhraseList` ユーザーがアプリを使用するときに生成されたデータセットに基づいて、実行時に要素の内容を変更する必要があります。
       >
       > 詳細については、「 [Cortana の VCD 語句一覧を動的に変更](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)する」を参照してください。  

    - `Feedback`アプリケーションの起動時に **Cortana** が表示して読み上げるテキストを含む要素。  

要素は、音声コマンドによって `Navigate` アプリがフォアグラウンドにアクティブ化されることを示します。 この例では、```showTripToDestination``` コマンドはフォアグラウンド タスクです。  

要素は、 `VoiceCommandService` 音声コマンドがバックグラウンドでアプリをアクティブ化することを示します。 `Target`この要素の属性の値は、 `Name` package.appxmanifest ファイル内の要素の属性の値と一致している必要があり [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) ます。 この例では、 `whenIsTripToDestination` コマンドとコマンドは、 `cancelTripToDestination` app service の名前をとして指定するバックグラウンドタスクです `AdventureWorksVoiceCommandService` 。  

詳しくは、「[**VCD 要素および属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)」をご覧ください。  

例: [](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) `en-us` **Adventure works** アプリの音声コマンドを定義する VCD ファイルの一部。  

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
<CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>
    
    <Command Name="showTripToDestination">
        <Example> Show trip to London </Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Showing trip to {destination} </Feedback>
        <Navigate />
    </Command>
      
    <Command Name="whenIsTripToDestination">
        <Example> When is my trip to Las Vegas?</Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Looking for trip to {destination}</Feedback>
        <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
        <Example> Cancel my trip to Las Vegas </Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Cancelling trip to {destination}</Feedback>
        <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
        <Item>London</Item>
        <Item>Las Vegas</Item>
        <Item>Melbourne</Item>
        <Item>Yosemite National Park</Item>
    </PhraseList>
</CommandSet>
```  

## <a name="install-the-vcd-commands"></a>VCD コマンドをインストールする  

VCD をインストールするには、アプリを 1 回実行する必要があります。  

>[!NOTE]
> 音声コマンド データは、アプリのインストールで保持されません。 アプリの音声コマンド データをそのまま保持する場合は、アプリを起動またはアクティブ化するたびに VCD ファイルを初期化するか、VCD が現在インストールされているかどうかを示す設定を保持することを考慮してください。  

`app.xaml.cs` ファイルで次の操作を行います。  

1. 次の using ディレクティブを追加します。

   ```csharp
   using Windows.Storage;
   ```

2. 非同期修飾子を使用して、メソッドをマークし `OnLaunched` ます。  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. ハンドラーでメソッドを呼び出して、 [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) 認識する必要がある音声コマンドを登録します。  
    例: Adventure Works アプリは、オブジェクトを定義し [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) ます。  
    例: ファイルを [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) 使用してオブジェクトを初期化するには、メソッドを呼び出し [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) `AdventureWorksCommands.xml` ます。  
    [`StorageFile`](/uwp/api/Windows.Storage.StorageFile)次に、オブジェクトがメソッドに渡され [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) ます。  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>アクティブ化の処理  

アプリがそれ以降の音声コマンドのアクティブ化にどのように応答するかを指定します。

>[!NOTE]
> 音声コマンドセットがインストールされた後、少なくとも1回はアプリを起動する必要があります。  

1. アプリが音声コマンドによってアクティブになったことを確認します。  

    イベントをオーバーライド [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) し、 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)かどうかを確認します。[**Kind**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) は [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)です。  

2. コマンドの名前と内容を判断します。  

    IActivatedEventArgs からオブジェクトへの参照を取得 [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) し、オブジェクトのプロパティに対してクエリを実行し[](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ます。  

    ユーザーの説明を確認するには、辞書内の認識された語句の [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) またはセマンティックプロパティの値を確認し [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) ます。  

3. アプリで適切なアクションを実行します (希望するページに移動するなど)。  

    >[!NOTE]
    > VCD を参照する必要がある場合は、「 [Vcd ファイルの編集](#edit-the-vcd-file) 」セクションを参照してください。

    音声コマンドの音声認識の結果を受け取った後、配列の最初の値からコマンド名を取得し [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ます。 VCD ファイルは複数の音声コマンドを定義するため、値が VCD のコマンド名と一致することを確認し、適切な操作を行う必要があります。  

    アプリケーションの最も一般的なアクションは、音声コマンドのコンテキストに関連するコンテンツを含むページに移動することです。  
    例: **TripPage** ページを開き、音声コマンドの値、コマンドの入力方法、および認識されている送信先の語句 (該当する場合) を渡します。 または、 **TripPage** ページに移動するときに、アプリがナビゲーションパラメーターを [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)に送信することもあります。  

    アプリを起動した音声コマンドが実際に読み上げられたかどうか、または [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) **commandmode** キーを使用してディクショナリからテキストとして入力されたかどうかを確認できます。 このキーの値は、またはのいずれかになり `voice` `text` ます。 キーの値がの場合は `voice` 、アプリで音声合成 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) を使用して、音声によるフィードバックをユーザーに提供することを検討してください。  

    [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)を使用して、 `PhraseList` `PhraseTopic` 要素のまたは制約で話されるコンテンツを検索します。 `ListenFor` ディクショナリキーは、 `Label` `PhraseList` 要素または要素の属性の値です `PhraseTopic` 。
    例: **{destination}** 句の値にアクセスする方法のコード例を次に示します。  

    ```csharp
    /// <summary>
    /// Entry point for an application activated by some means other than normal launching. 
    /// This includes voice commands, URI, share target from another app, and so on. 
    /// 
    /// NOTE:
    /// A previous version of the VCD file might remain in place 
    /// if you modify it and update the app through the store. 
    /// Activations might include commands from older versions of your VCD. 
    /// Try to handle these commands gracefully.
    /// </summary>
    /// <param name="args">Details about the activation method.</param>
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
        // a content frame is in place (unlike OnLaunched).
        // Navigate to either the main trip list page, or if a valid voice command
        // was provided, to the details page for that trip.
        rootFrame.Navigate(navigationToPageType, navigationCommand);
        
        // Ensure the current window is active
        Window.Current.Activate();
    }

    /// <summary>
    /// Returns the semantic interpretation of a speech result. 
    /// Returns null if there is no interpretation for that key.
    /// </summary>
    /// <param name="interpretationKey">The interpretation key.</param>
    /// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
    /// <returns></returns>
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>App Service で音声コマンドを処理する  

アプリ サービスで音声コマンドを処理します。  

1. 次の using ディレクティブを音声コマンドサービスファイルに追加します。  
    例: `AdventureWorksVoiceCommandService.cs`.  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. 音声コマンドを処理するときにアプリ サービスが終了しないように、サービス遅延を取得します。  
3. バックグラウンド タスクが、音声コマンドによってアクティブ化されたアプリ サービスとして実行されていることを確認します。  
    1. [**IBackgroundTaskInstance.TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) を [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails) にキャストします。  
    2. [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration)がファイル内の app service の名前であることを確認 `Package.appxmanifest` します。  
4. [**IBackgroundTaskInstance.TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) を使って **Cortana** への [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) を作成し、音声コマンドを取得します。
5. [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)のイベントハンドラーを登録します。  [**VoiceCommandCompleted**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) は、ユーザーのキャンセルによって app service が終了したときに通知を受信します。  
6. 予期しないエラーのためにアプリ サービスが閉じたときに通知を受け取ることができるようにするため、[**IBackgroundTaskInstance.Canceled**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) のイベント ハンドラーを登録します。  
7. コマンドの名前と内容を判断します。  
    1. [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand).[**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand) プロパティを使って、音声コマンドの名前を決定します。  
    2. ユーザーの説明を確認するには、辞書内の認識された語句の [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) またはセマンティックプロパティの値を確認し [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) ます。  
8. アプリ サービスで適切なアクションを実行します。  
9. **Cortana** を使用して音声コマンドに対するフィードバックを表示し、読み上げます。  
    1. **Cortana** で表示する文字列を決定し、音声コマンドに応答してユーザーとの対話を行い、オブジェクトを作成し [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) ます。 **Cortana** が表示して読み上げるフィードバック文字列を選ぶ方法については、「[Cortana の設計ガイドライン](cortana-design-guidelines.md)」をご覧ください。  
    2. [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)インスタンスを使用して、 [**reportprogress Async**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)または [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)をオブジェクトと共に呼び出して、 **Cortana** に進行状況または完了を報告し `VoiceCommandServiceConnection` ます。  

    >[!NOTE]
    > VCD を参照する必要がある場合は、「 [Vcd ファイルの編集](#edit-the-vcd-file) 」セクションを参照してください。  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
                        case "whenIsTripToDestination":
                            {
                                var destination = 
                                voiceCommand.Properties["destination"][0];
                                SendCompletionMessageForDestination(destination);
                                break;
                            }
                            
                            // As a last resort, launch the app in the foreground.
                        default:
                            LaunchAppInForeground();
                            break;
                    }
                }
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
            var userMessage = new VoiceCommandUserMessage();
            userMessage.SpokenMessage = "Launching Adventure Works";
            
            var response = VoiceCommandResponse.CreateResponse(userMessage);
            
            // When launching the app in the foreground, pass an app 
            // specific launch parameter to indicate what page to show.
            response.AppLaunchArgument = "showAllTrips=true";
            
            await voiceServiceConnection.RequestAppLaunchAsync(response);
        }
    }
    ```  

アクティブ化されると、app service は [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)を呼び出すために0.5 秒で終了します。 **Cortana** は、フィードバック文字列を示しています。  

>[!NOTE]
> VCD ファイルで **フィードバック** 文字列を宣言できます。 この文字列は、cortana キャンバスに表示される UI テキストには影響しません。 **cortana** が読み上げるテキストにのみ影響します。  

アプリの呼び出しに0.5 秒以上かかる場合は、次に示すように、 **Cortana** によってハンドオフ画面が挿入されます。 アプリケーションが **ReportSuccessAsync** を呼び出すまで最大 5 秒間、**Cortana** にハンドオフ画面が表示されます。 App service が **ReportSuccessAsync** を呼び出していない場合、または [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) **Cortana** に情報を提供するいずれかのメソッドを呼び出さない場合、ユーザーはエラーメッセージを受信し、app service はキャンセルされます。  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="バックグラウンドで AdventureWorks アプリを使用して、Cortana と基本的なクエリのスクリーンショット (進行状況と結果画面を含む)":::

## <a name="related-articles"></a>関連記事

- [Windows アプリにおける Cortana のやり取り](cortana-interactions.md)
- [Cortana を使用して音声コマンドでフォアグラウンドアプリをアクティブ化する](cortana-launch-a-foreground-app-with-voice-commands.md)
- [VCD 要素および属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana のデザインガイドライン](cortana-design-guidelines.md)
- [Cortana 音声コマンドのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=619899)
