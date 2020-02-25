---
Description: アプリで最新の Windows 10 機能を使用できるように、パッケージ化されていないデスクトップアプリに id を付与する方法について説明します。
title: パッケージ化されていないデスクトップ アプリに ID を付与する
ms.date: 10/25/2019
ms.topic: article
keywords: windows 10、デスクトップ、パッケージ、id、MSIX、Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 10ed6b8e1bd5efce4c9d4429d91849b1333505b6
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521353"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>パッケージ化されていないデスクトップ アプリに ID を付与する

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Windows 10 の拡張機能の多くは、バックグラウンドタスク、通知、ライブタイル、共有ターゲットなど、UWP 以外のデスクトップアプリから[パッケージ id](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)を使用する必要があります。 これらのシナリオでは、OS は、対応する API の呼び出し元を識別できるように id を必要とします。

Windows 10 Insider Preview Build 10.0.19000.0 の前の OS リリースでは、id をデスクトップアプリに付与する唯一の方法は、[署名済みの MSIX パッケージで id をパッケージ化](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)することです。 これらのアプリの場合、id はパッケージマニフェストに指定され、id 登録はマニフェストの情報に基づいて MSIX 配置パイプラインによって処理されます。 パッケージマニフェストで参照されるすべてのコンテンツは、MSIX パッケージ内に存在します。

Windows 10 Insider Preview Build 10.0.19000.0 以降では、アプリに*スパースパッケージ*をビルドして登録することにより、msix パッケージにパッケージ化されていないデスクトップアプリにパッケージ id を与えることができます。 このサポートにより、パッケージ id を必要とする Windows 10 拡張機能を使用するために、展開のために MSIX パッケージを導入することがまだできていないデスクトップアプリが有効になります。 背景情報については、こちらの[ブログ投稿](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)を参照してください。

パッケージ id をデスクトップアプリに付与するスパースパッケージをビルドして登録するには、次の手順に従います。

1. [スパースパッケージのパッケージマニフェストを作成する](#create-a-package-manifest-for-the-sparse-package)
2. [スパースパッケージをビルドして署名する](#build-and-sign-the-sparse-package)
3. [デスクトップアプリケーションマニフェストにパッケージ id メタデータを追加する](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [実行時にスパースパッケージを登録する](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>重要な概念

次の機能を使用すると、パッケージ化されていないデスクトップアプリでパッケージ id を取得できます。

### <a name="sparse-packages"></a>スパースパッケージ

*スパースパッケージ*にはパッケージマニフェストが含まれていますが、他のアプリバイナリおよびコンテンツは含まれていません。 スパースパッケージのマニフェストでは、事前に定義された外部の場所で、パッケージ外部のファイルを参照できます。 これにより、Windows 10 の機能拡張機能によって必要なパッケージ id を取得するために、アプリ全体に対して MSIX パッケージを導入することがまだできていないアプリケーションが許可されます。

> [!NOTE]
> スパースパッケージを使用するデスクトップアプリには、MSIX パッケージを使用して完全に展開されるという利点がありません。 これらの利点には、改ざん防止、ロックダウンされた場所へのインストール、デプロイ時の OS による完全な管理、実行時、アンインストールなどがあります。

### <a name="package-external-location"></a>パッケージ外部の場所

スパースパッケージをサポートするために、パッケージマニフェストスキーマでは、 [ **\<プロパティ\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)要素の下でオプションの **\<allowexternalcontent\>** 要素がサポートされるようになりました。 これにより、パッケージマニフェストは、ディスク上の特定の場所にあるパッケージの外部のコンテンツを参照できます。

たとえば、アプリの実行可能ファイルとその他のコンテンツを C:\Program\, Files\MyDesktopApp にインストールする、パッケージ化されていない既存のデスクトップアプリがある場合は、マニフェストに **\<AllowExternalContent\>** 要素を含むスパースパッケージを作成できます。 アプリのインストールプロセスまたはアプリの初回起動時に、スパースパッケージをインストールし、アプリで使用する外部の場所として C:\Program Files\MyDesktopApp\ を宣言できます。

## <a name="create-a-package-manifest-for-the-sparse-package"></a>スパースパッケージのパッケージマニフェストを作成する

スパースパッケージをビルドするには、まず、デスクトップアプリのパッケージ id メタデータとその他の必要な詳細を宣言する[パッケージマニフェスト](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)(package.appxmanifest という名前のファイル) を作成する必要があります。 スパースパッケージのパッケージマニフェストを作成する最も簡単な方法は、次の例を使用し、[スキーマ参照](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)を使用してアプリ用にカスタマイズすることです。

パッケージマニフェストに次の項目が含まれていることを確認してください。

* デスクトップアプリの id 属性を記述する[ **\<id\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)要素。
* [ **\<プロパティ\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties)要素の下に **\<allowexternalcontent\>** 要素。 この要素には `true`値を割り当てる必要があります。これにより、パッケージマニフェストは、ディスク上の特定の場所で、パッケージの外部のコンテンツを参照できます。 後の手順で、インストーラーまたはアプリで実行されるコードからスパースパッケージを登録するときに、外部の場所のパスを指定します。 マニフェスト内で参照するコンテンツは、パッケージ自体には存在しないため、外部の場所にインストールする必要があります。
* [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)要素の**MinVersion**属性は `10.0.19000.0` 以降のバージョンに設定する必要があります。
* [ **\<アプリケーション\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)要素宣言の**TrustLevel = MediumIL**と**runtimebehavior = Win32App**属性は、スパースパッケージに関連付けられているデスクトップアプリが標準のパッケージ化されていないデスクトップアプリと同様に動作することを宣言します。これは、レジストリとファイルシステムの仮想化やその他の実行時の変更は不要です。

次の例は、スパースパッケージマニフェスト (Package.appxmanifest) の完全な内容を示しています。 このマニフェストには、パッケージ id を必要とする `windows.sharetarget` の拡張機能が含まれています。

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

## <a name="build-and-sign-the-sparse-package"></a>スパースパッケージをビルドして署名する

パッケージマニフェストを作成したら、Windows SDK の[Makeappx ツール](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool)を使用して、スパースパッケージをビルドします。 スパースパッケージにはマニフェストで参照されるファイルが含まれていないため、パッケージのセマンティック検証をスキップする `/nv` オプションを指定する必要があります。

次の例は、コマンドラインからスパースパッケージを作成する方法を示しています。  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

スパースパッケージが対象のコンピュータに正常にインストールされるようにするには、ターゲットコンピュータ上で信頼されている証明書を使用してそのパッケージに署名する必要があります。 開発目的のために新しい自己署名証明書を作成し、Windows SDK で使用できる[SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)を使用してスパースパッケージに署名することができます。

次の例は、コマンドラインからスパースパッケージに署名する方法を示しています。

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>デスクトップアプリケーションマニフェストにパッケージ id メタデータを追加する

また、デスクトップアプリと[side-by-side アプリケーションマニフェスト](https://docs.microsoft.com/windows/win32/sbscs/application-manifests)を追加し、アプリの id 属性を宣言する属性を持つ **\<.msix\>** 要素を含める必要があります。 これらの属性の値は、実行可能ファイルが起動されたときにアプリの id を決定するために OS によって使用されます。

次の例は、 **\<.msix\>** 要素を含む side-by-side アプリケーションマニフェストを示しています。

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

**\<.msix\>** 要素の属性は、スパースパッケージのパッケージマニフェストの次の値と一致している必要があります。

* **PackageName**属性と**publisher**属性は、それぞれパッケージマニフェストの[ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)要素の**Name**属性と**publisher**属性が一致している必要があります。
* **ApplicationId**属性は、パッケージマニフェスト内の[ **\<アプリケーション\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)要素の**Id**属性と一致する必要があります。

Side-by-side アプリケーションマニフェストは、デスクトップアプリの実行可能ファイルと同じディレクトリに存在する必要があります。また、規則により、アプリの実行可能ファイルと同じ名前を付けて、`.manifest` 拡張子を追加する必要があります。 たとえば、アプリの実行可能ファイル名が `ContosoPhotoStore`の場合、アプリケーションマニフェストファイル名を `ContosoPhotoStore.exe.manifest`する必要があります。

## <a name="register-your-sparse-package-at-run-time"></a>実行時にスパースパッケージを登録する

アプリでパッケージ id をデスクトップアプリに付与するには、アプリで、整理[emanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager)クラスを使用してスパースパッケージを登録する必要があります。 アプリを初めて実行するときにスパースパッケージを登録するコードをアプリに追加したり、デスクトップアプリのインストール中にパッケージを登録するコードを実行したりすることができます (たとえば、MSI を使用してデスクトップアプリをインストールする場合など)。では、このコードをカスタムアクションから実行できます)。

次の例では、スパースパッケージを登録する方法を示します。 このコードは、パッケージマニフェストがパッケージ外部のコンテンツを参照できる外部の場所へのパスを含む**Addpackageoptions**オブジェクトを作成します。 次に、このオブジェクトを**AddPackageByUriAsync**メソッドに渡してスパースパッケージを登録します。 また、このメソッドは、署名されたスパースパッケージの場所を URI として受け取ります。 詳細な例については、関連する[サンプル](#sample)の `StartUp.cs` コードファイルを参照してください。

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

## <a name="sample"></a>サンプル

スパースパッケージを使用してデスクトップアプリにパッケージ id を付与する方法を示す完全な機能を備えたサンプルアプリについては、「 [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages)」を参照してください。 このサンプルのビルドと実行の詳細については、[このブログの投稿](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)を参照してください。

このサンプルには、次のものが含まれます。

* PhotoStoreDemo という名前の WPF アプリのソースコード。 起動時に、アプリは id で実行されているかどうかを確認します。 Id を使用して実行されていない場合は、スパースパッケージが登録され、アプリが再起動されます。 これらの手順を実行するコードについては、「`StartUp.cs`」を参照してください。
* `PhotoStoreDemo.exe.manifest`という名前の side-by-side アプリケーションマニフェスト。
* `AppxManifest.xml`という名前のパッケージマニフェスト。
