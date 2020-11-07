---
title: マイ連絡先の共有
description: '[マイユーザー] 共有を使用すると、ユーザーがタスクバーに連絡先をピン留めして、Windows のどこからでも簡単にタッチできます。'
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8cbf82592e91b82e2d9d34d116d00aecf2ddd021
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339410"
---
# <a name="my-people-sharing"></a>マイ連絡先の共有

マイ連絡先の機能を使うと、ユーザーは連絡先をタスク バーにピン留めして、どのアプリケーションを使って接続している場合でも、Windows のどこからでも容易に連絡を取ることができます。 ユーザーは、ファイルをエクスプローラーからピン留めされたマイ連絡先にドラッグして、ピン留めされた連絡先とコンテンツを共有できるようになりました。 標準の共有チャーム経由で、Windows の連絡先ストアの任意の連絡先を共有できるようになりました。 作成したアプリケーションをマイ連絡先の共有のターゲットとするための方法について説明します。

![マイ連絡先の共有パネル](images/my-people-sharing.png)

## <a name="requirements"></a>必要条件

+ Windows 10 と Microsoft Visual Studio 2019。 インストールについて詳しくは、「[Visual Studio のセットアップ](/windows/apps/get-started/get-set-up)」をご覧ください。
+ C# またはこれに類似するオブジェクト指向プログラミング言語に関する基本的な知識。 C# で作業を始めるには、「["Hello, world" アプリを作成する](../get-started/create-a-hello-world-app-xaml-universal.md)」をご覧ください。

## <a name="overview"></a>概要

アプリケーションをマイ連絡先の共有ターゲットとするためには、次の 3 つの手順を行う必要があります。

1. [アプリケーション マニフェストで shareTarget アクティブ化コントラクトのサポートを宣言します。](#declaring-support-for-the-share-contract)
2. [ユーザーがアプリを使用して共有できる連絡先に注釈を付けます。](#annotating-contacts)
3. アプリケーションの複数インスタンスの同時実行をサポートします。  ユーザーは、他のユーザーと共有するためにアプリケーションを使用しながら、アプリケーションの通常版を操作できる必要があります。 ユーザーは複数の共有ウィンドウで同時に使用できます。 これをサポートするには、アプリケーションが複数のビューを同時に実行できる必要があります。 これを行う方法については、「["アプリの複数のビューの表示](../design/layout/show-multiple-views.md)」の記事をご覧ください。

これを行うと、アプリケーションがマイ連絡先の共有ウィンドウで共有ターゲットに表示されます。これは次の 2 つの方法で起動できます。
1. 共有チャームで連絡先を選択します。
2. タスク バーにピン留めされた連絡先にファイルをドラッグ アンド ドロップします。

## <a name="declaring-support-for-the-share-contract"></a>共有コントラクトのサポートを宣言する

アプリケーションでの共有ターゲットのサポートを宣言するには、まず Visual Studio でアプリケーションを開きます。 **ソリューション エクスプローラー** で **Package.appxmanifest** を右クリックして、[ **プログラムから開く** ] を選択します。 メニューから [ **XML (テキスト) エディター** ] を選択し、[ **OK** ] をクリックします。 マニフェストを次のように変更します。


**変更前**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**変更後**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

このコードはすべてのファイルとデータの形式のサポートを追加しますが、サポートするファイルの種類やデータの形式を指定することを選ぶこともできます (詳しくは、「[ShareTarget クラスのドキュメント](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)」をご覧ください)。

## <a name="annotating-contacts"></a>連絡先に注釈を付ける

マイ連絡先の共有ウィンドウでアプリケーションを連絡先の共有ターゲットとして表示するには、それを Windows 連絡先ストアに書き込む必要があります。 連絡先を書き込む方法については、「[連絡先カードの統合のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)」をご覧ください。 

連絡先を共有する場合にアプリケーションがマイ連絡先の共有ターゲットとして表示されるためには、連絡先に注釈を書き込む必要があります。 注釈は、連絡先に関連付けられた、アプリケーションからのデータの一部です。 注釈は、必要なビューに対応する、アクティブ化可能なクラスを、 **ProviderProperties** メンバーに含む必要があります。また **Share** 操作のサポートを宣言する必要があります。

アプリが実行されている任意の時点で連絡先に注釈を付けることができますが、一般には、連絡先がWindows 連絡先ストアに追加されたらすぐに連絡先に注釈を付けるようにします。

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

“appId” はパッケージ ファミリ名の最後に ‘!’ と アクティブ化可能なクラス ID を付けたものです。 パッケージ ファミリ名を見つけるには、既定のエディターを使って **Package.appxmanifest** を開き、"Packaging" タブを検索します。ここで、"App" は、共有ターゲット ビューに対応する、アクティブ化可能なクラスです。

## <a name="running-as-a-my-people-share-target"></a>マイ連絡先の共有ターゲットとして実行する

最後に、アプリを実行するには、共有ターゲットのアクティブ化を行うアプリのメイン クラスで [OnShareTargetActivated](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) メソッドを上書きします。 [ShareTargetActivatedEventArgs.ShareOperation.Contacts](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) プロパティは、共有される連絡先を含むか、または (マイ連絡先の共有ではない) 標準の共有操作の場合には、空となります。

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>関連項目
+ [マイ連絡先のサポートを追加する](my-people-support.md)
+ [ShareTarget クラス](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [連絡先カードの統合のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)