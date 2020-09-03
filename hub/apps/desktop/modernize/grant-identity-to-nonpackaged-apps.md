---
Description: アプリで最新の Windows 10 機能を使用できるように、パッケージ化されていないデスクトップ アプリに ID を付与する方法について説明します。
title: パッケージ化されていないデスクトップ アプリに ID を付与する
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, デスクトップ, パッケージ, ID, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6c2adc41fd33692d3cc3deb78ed8dd0659709a11
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172706"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>パッケージ化されていないデスクトップ アプリに ID を付与する

Windows 10 の拡張機能の多くは、バックグラウンド タスク、通知、ライブ タイル、共有ターゲットなど、UWP 以外のデスクトップ アプリから使用するために[パッケージ ID](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) を必要とします。 これらのシナリオでは、OS は、対応する API の呼び出し元を識別できるようにするために ID を必要とします。

Windows 10 バージョン 2004 より前の OS リリースでは、デスクトップ アプリに ID を付与する唯一の方法は、[署名済みの MSIX パッケージでパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)することです。 これらのアプリの場合、ID はパッケージ マニフェストに指定され、ID 登録はマニフェストの情報に基づいて MSIX 展開パイプラインによって処理されます。 パッケージ マニフェストで参照されるすべてのコンテンツは、MSIX パッケージ内に存在します。

Windows 10 バージョン 2004 以降では、アプリで*スパース パッケージ*をビルドして登録することで、MSIX パッケージにパッケージ化されていないデスクトップ アプリにパッケージ ID を付与できます。 このサポートにより、展開のために MSIX パッケージをまだ導入できないデスクトップ アプリで、パッケージ ID を必要とする Windows 10 拡張機能を使用できるようになります。 詳細な背景情報については、[このブログ投稿](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)を参照してください。

デスクトップ アプリにパッケージ ID を付与するスパース パッケージをビルドして登録するには、次の手順に従います。

1. [スパース パッケージのパッケージ マニフェストを作成する](#create-a-package-manifest-for-the-sparse-package)
2. [スパース パッケージをビルドして署名する](#build-and-sign-the-sparse-package)
3. [デスクトップ アプリケーション マニフェストにパッケージ ID メタデータを追加する](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [実行時にスパース パッケージを登録する](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要な概念

次の機能を使用すると、パッケージ化されていないデスクトップ アプリでパッケージ ID を取得できます。

### <a name="sparse-packages"></a>スパース パッケージ

*スパース パッケージ*にはパッケージ マニフェストが含まれていますが、他のアプリ バイナリやコンテンツは含まれていません。 スパース パッケージのマニフェストでは、事前に定義された外部の場所で、パッケージ外部のファイルを参照できます。 これにより、アプリ全体に対して MSIX パッケージをまだ導入できないアプリケーションで、一部の Windows 10 拡張機能によって必要とされるパッケージ ID を取得できるようになります。

> [!NOTE]
> スパース パッケージを使用するデスクトップ アプリでは、MSIX パッケージを使用して完全に展開される場合の一部の利点が得られません。 これらの利点には、改ざん防止、ロックダウンされた場所へのインストール、展開、実行時、およびアンインストール時の OS による完全な管理などがあります。

### <a name="package-external-location"></a>パッケージ外部の場所

スパース パッケージをサポートするために、パッケージ マニフェスト スキーマでは、[**Properties**](/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 要素の下にある省略可能な [**uap10:AllowExternalContent**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) 要素がサポートされるようになりました。 これにより、パッケージ マニフェストでは、ディスク上の特定の場所にあるパッケージ外部のコンテンツを参照できます。

たとえば、アプリの実行可能ファイルとその他のコンテンツを C:\Program Files\MyDesktopApp\, にインストールする、パッケージ化されていない既存のデスクトップ アプリがある場合は、マニフェスト内に **uap10:AllowExternalContent** 要素を含むスパース パッケージを作成できます。 アプリのインストール プロセス中、またはアプリの初回起動時に、スパース パッケージをインストールし、アプリで使用する外部の場所として C:\Program Files\MyDesktopApp\ を宣言することができます。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>スパース パッケージのパッケージ マニフェストを作成する

スパース パッケージをビルドするには、まず、デスクトップ アプリのパッケージ ID メタデータとその他の必要な詳細を宣言する[パッケージ マニフェスト](/uwp/schemas/appxpackage/appx-package-manifest) (AppxManifest.xml という名前のファイル) を作成する必要があります。 スパース パッケージのパッケージ マニフェストを作成する最も簡単な方法は、次の例を用いて、[スキーマ参照](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を使用してアプリ用にカスタマイズすることです。

パッケージ マニフェストに次の項目が含まれることを確認してください。

* デスクトップ アプリの ID 属性を記述する [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 要素。
* [**Properties**](/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 要素の下にある [**uap10:AllowExternalContent**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) 要素。 この要素には値 `true` を割り当てる必要があります。これにより、パッケージ マニフェストでは、ディスク上の特定の場所にあるパッケージ外部のコンテンツを参照できます。 後の手順で、インストーラーまたはアプリで実行されるコードからスパース パッケージを登録するときに、外部の場所のパスを指定します。 パッケージ自体には存在しないマニフェスト内で参照するすべてのコンテンツは、外部の場所にインストールする必要があります。
* [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 要素の **MinVersion** 属性は、`10.0.19000.0` 以降のバージョンに設定する必要があります。
* [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素の **TrustLevel=mediumIL** 属性と **RuntimeBehavior=Win32App** 属性では、スパース パッケージに関連付けられているデスクトップ アプリが、レジストリとファイル システムの仮想化やその他の実行時の変更なしに、パッケージ化されていない標準のデスクトップ アプリと同様に動作することが宣言されます。

次の例は、スパース パッケージ マニフェスト (AppxManifest.xml) の完全な内容を示しています。 このマニフェストには、パッケージ ID を必要とする `windows.sharetarget` 拡張機能が含まれます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>スパース パッケージをビルドして署名する

パッケージ マニフェストを作成したら、Windows SDK の [MakeAppx.exe ツール](/windows/msix/package/create-app-package-with-makeappx-tool)を使用して、スパース パッケージをビルドします。 スパース パッケージにはマニフェストで参照されるファイルが含まれていないため、パッケージのセマンティック検証をスキップする `/nv` オプションを指定する必要があります。

次の例は、コマンド ラインからスパース パッケージを作成する方法を示しています。  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

スパース パッケージがターゲット コンピューターに正常にインストールされるようにするには、ターゲット コンピューター上で信頼されている証明書を使用してそのパッケージに署名する必要があります。 開発目的の新しい自己署名証明書を作成し、Windows SDK で使用できる [SignTool](/windows/msix/package/sign-app-package-using-signtool) を使用してスパース パッケージに署名することができます。

次の例は、コマンド ラインからスパース パッケージに署名する方法を示しています。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>デスクトップ アプリケーション マニフェストにパッケージ ID メタデータを追加する

また、デスクトップ アプリと共に[サイドバイサイド アプリケーション マニフェスト](/windows/win32/sbscs/application-manifests)を追加し、アプリの ID 属性を宣言する属性と共に [**msix**](/windows/win32/sbscs/application-manifests#msix) 要素を含める必要があります。 これらの属性の値は、実行可能ファイルの起動時にアプリの ID を決定するために OS によって使用されます。

次の例は、**msix** 要素を含むサイドバイサイド アプリケーション マニフェストを示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

**.msix** 要素の属性は、スパース パッケージのパッケージ マニフェストの次の値と一致する必要があります。

* **packageName** 属性と **publisher** 属性はそれぞれ、パッケージ マニフェストの [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 要素の **Name** 属性と **Publisher** 属性と一致する必要があります。
* **applicationId** 属性は、パッケージ マニフェストの [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素の **Id** 属性と一致する必要があります。

サイドバイサイド アプリケーション マニフェストは、デスクトップ アプリの実行可能ファイルと同じディレクトリに存在する必要があります。また、規則により、これはアプリの実行可能ファイルと同じ名前で、`.manifest` 拡張子を付加する必要があります。 たとえば、アプリの実行可能ファイル名が `ContosoPhotoStore` の場合、アプリケーション マニフェスト ファイル名は `ContosoPhotoStore.exe.manifest` である必要があります。

## <a name="register-your-sparse-package-at-run-time"></a>実行時にスパース パッケージを登録する

デスクトップ アプリにパッケージ ID を付与するには、[**PackageManager**](/uwp/api/windows.management.deployment.packagemanager) クラスの [**AddPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.addpackagebyuriasync) メソッドを使用して、スパース パッケージを登録する必要があります。 このメソッドは、Windows 10 バージョン 2004 以降で使うことができます。 アプリを初めて実行するときにスパース パッケージを登録するコードをアプリに追加したり、デスクトップ アプリのインストール中にパッケージを登録するコードを実行したりできます (たとえば、MSI を使用してデスクトップ アプリをインストールする場合、カスタム アクションからこのコードを実行できます)。

次の例は、スパース パッケージを登録する方法を示しています。 このコードでは [**AddPackageOptions**](/uwp/api/windows.management.deployment.addpackageoptions) オブジェクトが作成されます。このオブジェクトには、パッケージ マニフェストがパッケージ外部のコンテンツを参照できる外部の場所へのパスが含まれています。 次に、このオブジェクトが **AddPackageByUriAsync** メソッドに渡されて、スパース パッケージが登録されます。 また、このメソッドは、署名されたスパース パッケージの場所を URI として受け取ります。 詳細な例については、関連する[サンプル](#sample)の `StartUp.cs` コードファイルを参照してください。

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>［サンプル］

スパース パッケージを使用してデスクトップ アプリにパッケージ ID を付与する方法を示す、完全な機能を備えたサンプル アプリについては、[SparesePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages) のサンプルを参照してください。 サンプルのビルドと実行の詳細については、[このブログ投稿](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)を参照してください。

このサンプルには、次の項目が含まれます。

* PhotoStoreDemo という名前の WPF アプリのソース コード。 起動時に、アプリが ID を使用して実行されているかどうかが確認されます。 ID を使用して実行されていない場合は、スパース パッケージが登録され、アプリが再起動されます。 これらの手順を実行するコードについては、`StartUp.cs` を参照してください。
* `PhotoStoreDemo.exe.manifest` という名前のサイドバイサイド アプリケーション マニフェスト。
* `AppxManifest.xml` という名前のパッケージ マニフェスト。