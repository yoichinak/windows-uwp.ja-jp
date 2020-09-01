---
Description: ホストアプリの実行可能ファイル、エントリポイント、およびランタイム属性を継承するホスト型アプリを構築する方法について説明します。
title: ホステッド アプリの作成
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, デスクトップ, パッケージ, ID, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3424b7a0ead3d4a3abeebc6286aebe935d09eab2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172996"
---
# <a name="create-hosted-apps"></a>ホステッド アプリの作成

Windows 10 バージョン2004以降では、ホストされている *アプリ*を作成できます。 ホストされているアプリは、親 *ホスト* アプリと同じ実行可能ファイルと定義を共有しますが、システム上の別のアプリのように見え、動作します。

ホステッド アプリは、コンポーネント (実行可能ファイルやスクリプト ファイルなど) がスタンドアロンの Windows 10 アプリのように動作する必要があるものの、そのコンポーネントを実行するためにホスト プロセスが必要な場合に役立ちます。 たとえば、PowerShell スクリプトや Python スクリプトを、実行するためにホストをインストールする必要があるホスト型アプリとして配信することができます。 ホステッド アプリには、独自のスタート タイルや ID を指定できるほか、バックグラウンド タスク、通知、タイル、共有ターゲットなどの Windows 10 の機能と緊密に統合することもできます。

ホステッドアプリの機能は、パッケージマニフェスト内のいくつかの要素と属性によってサポートされています。これにより、ホストされているアプリでホストアプリケーションパッケージの実行可能ファイルと定義を使用できるようになります。 ユーザーがホストされているアプリを実行すると、ホストされているアプリの id でホストの実行可能ファイルが OS によって自動的に起動されます。 ホストは、ビジュアルアセット、コンテンツ、または呼び出し Api をホストされたアプリとして読み込むことができます。 ホストされているアプリは、ホストとホストされるアプリの間で宣言された機能の積集合を取得します。 つまり、ホストされているアプリは、ホストが提供するよりも多くの機能を要求することはできません。

## <a name="define-a-host"></a>ホストを定義する

*ホスト*は、ホストされているアプリのメインの実行可能ファイルまたはランタイムプロセスです。 現時点でサポートされているホストは、 *パッケージ id*を持つデスクトップアプリ (.net または C++/Win32) のみです。 現時点では、UWP アプリはホストとしてはサポートされていません。 デスクトップアプリでパッケージ id を使用するには、いくつかの方法があります。

* パッケージ id をデスクトップアプリに付与する最も一般的な方法は、パッケージを [MSIX パッケージにパッケージ化](/windows/msix)することです。
* 場合によっては、 [スパースパッケージ](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)を作成してパッケージ id を許可することもできます。 このオプションは、デスクトップアプリを展開するために MSIX パッケージを導入できない場合に便利です。

ホストは、パッケージマニフェスト内で [**uap10: HostRuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) 拡張機能によって宣言されます。 この拡張機能には、ホストされているアプリのパッケージマニフェストによっても参照される値を割り当てる必要がある **Id** 属性があります。 ホストされているアプリがアクティブになると、ホストされているアプリの id でホストが起動し、ホストされているアプリケーションパッケージからコンテンツまたはバイナリを読み込むことができます。

次の例は、パッケージマニフェストでホストを定義する方法を示しています。 **Uap10: HostRuntime**拡張機能はパッケージ全体であるため、 [**package**](/uwp/schemas/appxpackage/uapmanifestschema/element-package)要素の子として宣言されます。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

次の要素について、これらの重要な詳細をメモしておきます。

| 要素              | 詳細 |
|----------------------|-------|
| [**uap10:Extension**](/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | カテゴリは、ホストされている `windows.hostRuntime` アプリをアクティブ化するときに使用されるランタイム情報を定義する、パッケージ全体の拡張機能を宣言します。 ホストされるアプリは、拡張機能で宣言された定義を使用して実行されます。 前の例で宣言したホストアプリを使用する場合、ホストされるアプリは実行可能 **PyScriptEngine.exe** ファイルとして **mediumIL** 信頼レベルで実行されます。<br/><br/>**Executable**、 **Uap10: runtimebehavior**、および**uap10: TrustLevel**属性は、パッケージ内のホストプロセスバイナリの名前と、ホストされているアプリの実行方法を指定します。 たとえば、前の例の属性を使用してホストされているアプリは、mediumIL 信頼レベルで実行可能 PyScriptEngine.exe として実行されます。 |
| [**uap10:HostRuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | **Id**属性は、パッケージ内のこの特定のホストアプリの一意の識別子を宣言します。 パッケージには複数のホストアプリを含めることができ、それぞれに一意の**Id**を持つ**Uap10: hostruntime**要素が必要です。

## <a name="declare-a-hosted-app"></a>ホストされているアプリを宣言する

ホストされている *アプリ* は、 *ホスト*に対するパッケージの依存関係を宣言します。 ホストされているアプリは、独自のパッケージでエントリポイント実行可能ファイルを指定する代わりに、ホストの ID (つまり、ホストパッケージの**uap10: HostRuntime**拡張機能の**id**属性) を使用してアクティブ化します。 ホストされるアプリには、通常、ホストからアクセスできるコンテンツ、ビジュアルアセット、スクリプト、またはバイナリが含まれています。 ホストされるアプリケーションパッケージの [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 値は、ホストと同じ値をターゲットにする必要があります。

ホストされているアプリケーションパッケージは、署名または署名なしにすることができます。

* 署名付きパッケージには実行可能ファイルが含まれる場合があります。 これは、バイナリ拡張機構を持つシナリオで役立ちます。これにより、ホストは、ホストされているアプリケーションパッケージに DLL または登録されたコンポーネントを読み込むことができます。
* 署名されていないパッケージには、実行可能でないファイルのみを含めることができます。 これは、ホストがイメージ、アセット、およびコンテンツまたはスクリプトファイルの読み込みのみを行う必要がある場合に便利です。 署名されて `OID` いないパッケージは、 [**id**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 要素に特別な値を含める必要があります。そうでないと、登録できません。 これにより、署名されたパッケージの id との競合や、署名されていないパッケージのスプーフィングを防ぐことができます。

ホストされるアプリを定義するには、パッケージマニフェストで次の項目を宣言します。

* [**Uap10: HostRuntimeDependency**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)要素。 これは [Dependencies](/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) 要素の子です。
* [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)要素 (アプリの場合) または[**拡張**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)要素 (アクティブ化可能な拡張機能の場合) の**uap10: HostId**属性。

次の例は、署名されていないホスト型アプリのパッケージマニフェストの関連セクションを示しています。

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

次の要素について、これらの重要な詳細をメモしておきます。

| 要素              | 詳細 |
|----------------------|-------|
| [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | この例のホステッドアプリパッケージは署名されていないため、 **Publisher** 属性には文字列を含める必要があり `OID.2.25.311729368913984317654407730594956997722=1` ます。 これにより、署名されていないパッケージが署名付きパッケージの id を偽装できないようにします。 |
| [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | **MinVersion**属性では、10.0.19041.0 またはそれ以降のバージョンの OS を指定する必要があります。 |
| [**uap10:HostRuntimeDependency**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | この要素要素は、ホストアプリケーションパッケージに対する依存関係を宣言します。 これは、ホストパッケージの **名前** と **発行元** と、それが依存している **MinVersion** で構成されます。 これらの値は、ホストパッケージの [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 要素の下にあります。 |
| [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) | **Uap10: HostId**属性は、ホストの依存関係を表します。 ホストされたアプリケーションパッケージは、[**アプリケーション**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)または[**拡張**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension)要素の通常の**実行可能ファイル**と**EntryPoint**属性ではなく、この属性を宣言する必要があります。 その結果、ホストされているアプリは、対応する**HostId**値を使用して、ホストから**実行可能ファイル**、**エントリポイント**、およびランタイム属性を継承します。<br/><br/>**Uap10: parameters**属性は、ホスト実行可能ファイルのエントリポイント関数に渡されるパラメーターを指定します。 ホストは、これらのパラメーターの処理方法を認識する必要があるため、ホストとホストされているアプリの間に暗黙のコントラクトがあります。 |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>実行時に署名されていないホストされたアプリケーションパッケージを登録する

**Uap10: HostRuntime**拡張機能の利点の1つは、ホストが実行時にホストされているアプリケーションパッケージを動的に生成し、署名しなくても[**、パッケージ化**](/uwp/api/windows.management.deployment.packagemanager)されたアプリケーションパッケージを実行時に登録できるようにすることです。 これにより、ホストはホストされているアプリケーションパッケージのコンテンツとマニフェストを動的に生成し、登録することができます。

次のよう [**に、パッケージ**](/uwp/api/windows.management.deployment.packagemanager) 化されていないホストされたアプリケーションパッケージを登録するには、使用します。 これらのメソッドは、Windows 10 バージョン2004以降で使用できます。

* [**AddPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.addpackagebyuriasync): *Options*パラメーターの**allowunsigned**プロパティを使用して、署名されていない msix パッケージを登録します。
* [**RegisterPackageByUriAsync**](/uwp/api/windows.management.deployment.packagemanager.registerpackagebyuriasync): パッケージマニフェストファイルの厳密な登録を実行します。 パッケージが署名されている場合、マニフェストを含むフォルダーには、 [ますファイル](/windows/msix/overview#inside-an-msix-package) とカタログが含まれている必要があります。 Unsigned の場合は、 *options*パラメーターの**allowunsigned**プロパティを設定する必要があります。

### <a name="requirements-for-unsigned-hosted-apps"></a>署名されていないホスト型アプリの要件

* パッケージマニフェストの [**アプリケーション**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 要素または [**拡張**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 要素に、 **実行可能ファイル**、 **EntryPoint**属性、 **TrustLevel** 属性などのアクティベーションデータを含めることはできません。 代わりに、これらの要素には、ホストと**uap10: Parameters**属性の依存関係を表す**uap10: HostId**属性のみを含めることができます。
* パッケージはメインパッケージである必要があります。 バンドル、フレームワークパッケージ、リソース、またはオプションのパッケージを指定することはできません。

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>署名されていないホストされたアプリケーションパッケージをインストールして登録するホストの要件

* ホストには、 [パッケージ id](#define-a-host)が必要です。
* ホストに **packageManagement** [制限機能](../packaging/app-capability-declarations.md#restricted-capabilities)が必要です。
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](../packaging/app-capability-declarations.md#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>サンプル

ホストとして自身を宣言し、実行時にホストされるアプリケーションパッケージを動的に登録する完全に機能するサンプルアプリについては、「 [ホステッドアプリのサンプル](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps)」を参照してください。

### <a name="the-host"></a>ホスト

ホストには **PyScriptEngine**という名前が付けられます。 これは、python スクリプトを実行する C# で記述されたラッパーです。 パラメーターを指定して実行すると `-Register` 、スクリプトエンジンによって、python スクリプトを含むホストされるアプリがインストールされます。 新しくインストールされたホステッドアプリをユーザーが起動しようとすると、ホストが起動され、 **NumberGuesser** python スクリプトが実行されます。

ホストアプリのパッケージマニフェスト (PyScriptEnginePackage フォルダー内の package.appxmanifest ファイル) には、アプリを ID **Python ホスト**および実行可能ファイル**PyScriptEngine.exe**を持つホストとして宣言する**uap10: hostruntime**拡張機能が含まれています。  

> [!NOTE]
> このサンプルでは、パッケージマニフェストは package.appxmanifest という名前で、 [Windows アプリケーションパッケージプロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)の一部です。 このプロジェクトをビルドすると、 [AppxManifest.xmlという名前のマニフェストが生成 ](/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) され、ホストアプリの msix パッケージがビルドされます。

### <a name="the-hosted-app"></a>ホストされているアプリ

ホストされているアプリは、python スクリプトとパッケージマニフェストなどのパッケージアーティファクトで構成されます。 PE ファイルは含まれていません。

ホストされるアプリ (NumberGuesser/AppxManifest.xml ファイル) のパッケージマニフェストには、次の項目が含まれています。

* [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)要素の**Publisher**属性には、署名されていない `OID.2.25.311729368913984317654407730594956997722=1` パッケージに必要な識別子が含まれています。
* [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application)要素の**uap10: HostId**属性は、そのホストとして**python ホスト**を識別します。

### <a name="run-the-sample"></a>サンプルを実行する

このサンプルでは、バージョン10.0.19041.0 以降の Windows 10 と Windows SDK が必要です。

1. 開発用コンピューターのフォルダーに [サンプル](https://aka.ms/hostedappsample) をダウンロードします。
2. Visual Studio で PyScriptEngine ソリューションを開き、 **PyScriptEnginePackage** プロジェクトをスタートアッププロジェクトとして設定します。
3. **PyScriptEnginePackage**プロジェクトをビルドします。
4. ソリューションエクスプローラーで、 **PyScriptEnginePackage** プロジェクトを右クリックし、[ **配置**] を選択します。
5. サンプルファイルをコピーしたディレクトリにコマンドプロンプトウィンドウを開き、次のコマンドを実行してサンプル **NumberGuesser** アプリ (ホステッドアプリ) を登録します。 `D:\repos\HostedApps`サンプルファイルをコピーしたパスに変更します。

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > `pyscriptengine`サンプル内のホストで[**Appexecutionalias**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)が宣言されているため、コマンドラインでを実行できます。

6. [ **スタート** ] メニューを開き、[ **NumberGuesser** ] をクリックして、ホストされているアプリを実行します。