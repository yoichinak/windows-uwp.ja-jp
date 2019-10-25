---
title: サイドロードされた UWP アプリ用の仲介型 Windows ランタイムコンポーネント
description: このホワイトペーパーでは、Windows 10 でサポートされている企業向けの機能について説明します。これにより、タッチ対応の .NET アプリで主要なビジネスクリティカルな操作を担当する既存のコードを使用できるようになります。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10、uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690343"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>サイドロードされた UWP アプリ用の仲介型 Windows ランタイムコンポーネント

この記事では、Windows 10 でサポートされているエンタープライズ向けの機能について説明します。これにより、タッチ対応の .NET アプリで主要なビジネスクリティカルな操作を担当する既存のコードを使用できるようになります。

## <a name="introduction"></a>はじめに

>このホワイトペーパーに付属するサンプルコードは、 [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample)用に**ダウンロード  こと**があります。 仲介型 Windows ランタイムコンポーネントを構築するための Microsoft Visual Studio テンプレートは、「 [windows 10 用のユニバーサル Windows アプリを対象とする Visual Studio 2015 テンプレート](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)」からダウンロードできます。

Windows には、*サイドロードアプリケーション用の仲介型 Windows ランタイムコンポーネント*と呼ばれる新機能が含まれています。 「IPC (プロセス間通信)」という用語を使用して、UWP アプリでこのコードを操作しながら、既存のデスクトップソフトウェア資産を1つのプロセス (デスクトップコンポーネント) で実行する機能を記述します。 これは、企業の開発者にとって使い慣れたモデルです。データベースアプリケーションや Windows の NT サービスを利用するアプリケーションは、同様のマルチプロセスアーキテクチャを共有します。

アプリのサイドローディングは、この機能の重要なコンポーネントです。
企業固有のアプリケーションは、一般的なコンシューマー Microsoft Store にはありません。企業は、セキュリティ、プライバシー、配布、セットアップ、サービスに関する非常に具体的な要件を持っています。 そのため、サイドローディングモデルは、この機能を使用する必要があるか、または重要な実装の詳細である必要があります。

データ中心のアプリケーションは、このアプリケーションアーキテクチャの重要なターゲットです。 SQL Server など、既存のビジネスルールがデスクトップコンポーネントの一般的な部分になるということが想定されています。 これは、デスクトップコンポーネントで proffered できる唯一の機能ではありませんが、この機能の需要の大部分は、既存のデータとビジネスロジックに関連しています。

最後に、エンタープライズ開発で .NET ランタイムと C\# 言語の膨大な侵入が発生したため、この機能は UWP アプリとデスクトップコンポーネント側の両方に .NET を使用することに重点を置いて開発されました。 UWP アプリで使用できる言語とランタイムは他にもありますが、付随するサンプルは C\#のみを示すものであり、.NET ランタイムのみに制限されています。

## <a name="application-components"></a>アプリケーションコンポーネント

>この機能は、.NET を使用するためだけ**に  ます**。 クライアントアプリとデスクトップコンポーネントの両方が .NET を使用して作成されている必要があります。

**アプリケーション モデル**

この機能は、MVVM (モデルビュービューモデル) と呼ばれる一般的なアプリケーションアーキテクチャを中心に構築されています。 そのため、"モデル" がデスクトップコンポーネント全体に格納されていることを前提としています。 したがって、デスクトップコンポーネントが "ヘッドレス" (つまり、UI が含まれていない) であることをすぐに明らかにする必要があります。 このビューは、サイドロードされたエンタープライズアプリケーションに完全に含まれます。 "ビューモデル" コンストラクトを使用してこのアプリケーションを構築する必要はありませんが、このパターンの使用が一般的であることが予想されます。

**デスクトップコンポーネント**

この機能のデスクトップコンポーネントは、この機能の一部として導入される新しいアプリケーションの種類です。 このデスクトップコンポーネントは C\# でのみ記述でき、Windows 10 の場合は .NET 4.6 以上を対象にする必要があります。 このプロジェクトの種類は、プロセス間の通信形式が UWP の型とクラスを構成し、デスクトップコンポーネントが .NET ランタイムクラスライブラリのすべての部分を呼び出すことができるようにする、UWP をターゲットとする CLR 間のハイブリッドです。 Visual Studio プロジェクトへの影響については、後で詳しく説明します。 このハイブリッド構成では、デスクトップコンポーネント上に構築されたアプリケーション間で UWP 型をマーシャリングしながら、デスクトップコンポーネント実装内でデスクトップ CLR コードを呼び出すことができます。

**決め**

サイドロードアプリケーションとデスクトップコンポーネントとの間のコントラクトは、UWP 型システムの観点から説明されています。 これには、UWP を表すことができる1つ以上の C\# クラスの宣言が含まれます。 「C [\# での Windows ランタイムコンポーネントの作成」 Visual Basic および](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140))「c\#を使用して Windows ランタイムクラスを作成するための特定の要件」を参照してください。

>**注** 列挙型は、現時点では、デスクトップコンポーネントとサイドロードアプリケーションの Windows ランタイムコンポーネントコントラクトではサポートされていません。

**サイドロードアプリケーション**

サイドロードアプリケーションは、1つの場合を除き、通常の UWP アプリです。これは、Microsoft Store によってインストールされるのではなく、サイドロードされます。 インストールメカニズムの大部分は同じです。マニフェストとアプリケーションのパッケージ化が似ています (マニフェストへの追加の1つは、後で詳しく説明します)。 サイドローディングを有効にすると、簡単な PowerShell スクリプトによって、必要な証明書とアプリケーション自体をインストールできるようになります。 サイドロードアプリケーションは、Visual Studio の [プロジェクト/ストア] メニューに含まれる WACK 認定テストに合格するという、通常のベストプラクティスです。

>**メモ**サイドローディングは、開発者向けの設定-&gt; Update & security-&gt; で有効にすることができます。

注意すべき重要な点の1つは、Windows 10 の一部として出荷された App Broker メカニズムが32ビットのみであることです。 デスクトップコンポーネントは32ビットである必要があります。
サイドロードアプリケーションは64ビット (64 ビットと32ビットの両方のプロキシが登録されている場合) でもかまいませんが、これは例外的なものになります。 通常の "ニュートラル" 構成を使用して、サイドロードされたアプリケーションを C\# ビルドすると、"32 ビットの優先" の既定値では、32ビットのサイドロードアプリケーションが生成されます。

**サーバーのインスタンス化と AppDomains**

各サイドロードされたアプリケーションは、アプリブローカーサーバーの独自のインスタンスを受信します (そのため、"マルチインスタンス化" と呼ばれます)。 サーバーコードは、単一の AppDomain 内で実行されます。 これにより、複数のバージョンのライブラリを別々のインスタンスで実行できます。 たとえば、アプリケーション A にはコンポーネントの v1.1 が必要であり、アプリケーション B には V2 が必要です。 これらは、個別のサーバーディレクトリに v2.0 コンポーネントと V2 コンポーネントを配置し、適切なバージョンをサポートするサーバーをアプリケーションに示すことによって明確に分離されています。

サーバーコードの実装は、複数のアプリケーションを同じサーバーディレクトリにポイントすることによって、複数の App Broker サーバーインスタンス間で共有できます。 引き続き App Broker サーバーのインスタンスは複数存在しますが、同じコードを実行します。 1つのアプリケーションで使用されるすべての実装コンポーネントが同じパスに存在している必要があります。

## <a name="defining-the-contract"></a>コントラクトの定義

この機能を使用してアプリケーションを作成する最初の手順は、サイドロードアプリケーションとデスクトップコンポーネントの間にコントラクトを作成することです。 これは Windows ランタイム型を使用してのみ実行する必要があります。
幸いにも、これらは C\# クラスを使用して簡単に宣言できます。 ただし、これらのメッセージ交換を定義するときは、パフォーマンスに関する重要な考慮事項があります。これについては後のセクションで説明します。

コントラクトを定義するシーケンスは、次のように導入されています。

**手順 1:** Visual Studio で新しいクラスライブラリを作成します。 必ず、 **Windows ランタイムコンポーネント**テンプレートではなく、**クラスライブラリ**テンプレートを使用してプロジェクトを作成してください。

実装は次のようになりますが、このセクションでは、プロセス間コントラクトの定義についてのみ説明します。 付属のサンプルには、次のような開始図形のクラス (EnterpriseServer.cs) が含まれています。

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

これにより、サイドロードアプリケーションからインスタンス化できるクラス "EnterpriseServer" が定義されます。 このクラスは、RuntimeClass で約束される機能を提供します。 RuntimeClass は、サイドロードされたアプリケーションに含まれる参照 winmd を生成するために使用できます。

**手順 2:** プロジェクトファイルを手動で編集して、プロジェクトの出力の種類を**Windows ランタイムコンポーネント**に変更します。

Visual Studio でこれを行うには、新しく作成したプロジェクトを右クリックして [プロジェクトのアンロード] を選択し、もう一度右クリックして [EnterpriseServer .csproj の編集] を選択し、プロジェクトファイル (XML ファイル) を編集用に開きます。

開いているファイルで、\<の OutputType\> タグを検索し、その値を "winmdobj" に変更します。

**手順 3:** "参照" Windows メタデータファイル (winmd ファイル) を作成するビルド規則を作成します。 つまり、には実装がありません。

**手順 4:** "実装" Windows メタデータファイルを作成するビルド規則を作成します。つまり、同じメタデータ情報を持ちますが、実装も含まれます。

これは、次のスクリプトによって行われます。 ビルド後に実行するコマンドラインの プロジェクトの **[プロパティ]**  >  **[ビルドイベント]** にスクリプトを追加します。

> スクリプト**は、対象**とする windows のバージョン (windows 10) と使用されている Visual Studio のバージョンによって異なります。

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

参照 winmd が作成されると (プロジェクトのターゲットフォルダーの下のフォルダー "reference" に)、side-by-side 読み込みアプリケーションプロジェクトと参照される各アプリケーションプロジェクトに対して、その参照**winmd**が手動で (コピー) されます。 これについては、次のセクションで詳しく説明します。 上記のビルド規則に組み込まれているプロジェクト構造は、混乱を避けるために、実装と参照**winmd**がビルド階層内で明確に分離されたディレクトリにあることを確認します。

## <a name="side-loaded-applications-in-detail"></a>サイドロードされたアプリケーションの詳細
前述のように、サイドロードアプリケーションは他の UWP アプリと同様に構築されていますが、サイドロードアプリケーションのマニフェストで RuntimeClass の可用性を宣言すると、追加の詳細が1つ追加されます。 これにより、アプリケーションは、デスクトップコンポーネントの機能にアクセスするための新しい書き込みだけを行うことができます。 <Extension> セクションの新しいマニフェストエントリには、デスクトップコンポーネントに実装されている RuntimeClass と、その場所に関する情報が記述されています。 アプリケーションのマニフェスト内のこれらの宣言コンテンツは、Windows 10 を対象とするアプリでも同じです。 例:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

このアプリケーション構成に適用できないエントリが outOfProcessServer カテゴリに複数あるため、カテゴリは inProcessServer です。 <Path> コンポーネントには必ず clrhost .dll を含める必要があることに注意してください (ただし、これは強制され**ず**、別の値を指定すると、未定義の方法で失敗します)。

<ActivatableClass> セクションは、アプリのパッケージの Windows ランタイムコンポーネントによって優先される true のインプロセス RuntimeClass と同じです。 <ActivatableClassAttribute> は新しい要素で、属性 Name = "DesktopApplicationPath" と Type = "string" は必須および不変です。 Value 属性は、デスクトップコンポーネントの実装 winmd が置かれている場所を指します (次のセクションで詳しく説明します)。 デスクトップコンポーネントによって優先される各 RuntimeClass には、独自の <ActivatableClass> 要素ツリーが必要です。 ActivatableClassId は、RuntimeClass の完全な名前空間で修飾された名前と一致する必要があります。

「コントラクトの定義」で説明したように、デスクトップコンポーネントの参照 winmd に対してプロジェクト参照を作成する必要があります。 通常、Visual Studio プロジェクトシステムでは、同じ名前の2つのレベルのディレクトリ構造が作成されます。 このサンプルでは、EnterpriseIPCApplication\\EnterpriseIPCApplication です。 参照**winmd**がこの2番目のレベルのディレクトリに手動でコピーされ、[プロジェクト参照] ダイアログが使用されます (**参照**をクリックします。 ボタン) をクリックして、この**winmd**を検索して参照します。 その後、デスクトップコンポーネントの最上位レベルの名前空間 (たとえば、Fabrikam) は、プロジェクトの参照部分の最上位ノードとして表示されます。

>**メモ**サイドロードアプリケーションで**参照 winmd**を使用することは非常に重要です。 サイドロードされたアプリディレクトリに誤って**実装の winmd**を渡し、それを参照すると、"IStringable が見つかりません" に関連するエラーが発生する可能性があります。 これは、間違った**winmd**が参照されていることを確認するための1つです。 IPC server アプリのビルド後のルール (次のセクションで詳しく説明します) では、これらの2つの**winmd**を別のディレクトリに慎重に分離します。

環境変数 (特に% ProgramFiles%)<ActivatableClassAttribute Value="path"> で使用できます。前述のように、App Broker は32ビットのみをサポートするため、アプリケーションが64ビット OS で実行されている場合、% ProgramFiles% は C:\\Program Files (x86) に解決されます。

## <a name="desktop-ipc-server-detail"></a>デスクトップ IPC サーバーの詳細

前の2つのセクションでは、クラスの宣言と、サイドロードされたアプリケーションプロジェクトへの参照**winmd**の転送のしくみについて説明します。 デスクトップコンポーネントの残存作業の大部分は、実装に関係します。 デスクトップコンポーネントのすべてのポイントは、デスクトップコードを呼び出すことができる (通常は既存のコード資産を再利用する) 必要があるため、プロジェクトを特別な方法で構成する必要があります。
通常、.NET を使用する Visual Studio プロジェクトでは、2つの "profiles" のいずれかを使用します。
1つはデスクトップ ("用です。NetFramework ") と1つは、CLR (") の UWP アプリの部分を対象としています。NetCore ")。 この機能のデスクトップコンポーネントは、これらの2つの間のハイブリッドです。 このため、参照セクションは、これら2つのプロファイルをブレンドするために非常に慎重に構築されています。

Windows ランタイム API サーフェイス全体が暗黙的に含まれるため、通常の UWP アプリプロジェクトには明示的なプロジェクト参照が含まれていません。
通常は、他のプロジェクト間参照のみが行われます。 ただし、デスクトップコンポーネントプロジェクトには、非常に特殊な参照のセットがあります。 このプロジェクトは、"従来のデスクトップ\\クラスライブラリ" として有効期間を開始するため、デスクトッププロジェクトになります。 そのため、( **winmd**ファイルへの参照を使用して) Windows ランタイム API への明示的な参照を行う必要があります。 次に示すように、適切な参照を追加します。

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

上記の参照は、このハイブリッドサーバーを適切に運用するために不可欠な eferences を十分に組み合わせたものです。 このプロトコルでは、.csproj ファイル (「プロジェクトの OutputType の編集方法」を参照) を開き、必要に応じてこれらの参照を追加します。

参照が適切に構成されたら、次にサーバーの機能を実装します。 「 [Windows ランタイムコンポーネントとの相互運用性のベストプラクティス (C\#/VB/C++および XAML を使用した UWP アプリ)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))」を参照してください。
このタスクでは、実装の一部としてデスクトップコードを呼び出すことができる Windows ランタイムコンポーネント dll を作成します。 付属のサンプルには、Windows ランタイムで使用される主なパターンが含まれています。

-   メソッド呼び出し

-   デスクトップコンポーネントによるイベントソースの Windows ランタイム

-   非同期操作の Windows ランタイム

-   基本型の配列を返す

**インストール**

アプリをインストールするには、関連付けられているサイドロードアプリケーションのマニフェストに指定されている正しいディレクトリに実装**winmd**をコピーします: <ActivatableClassAttribute>の値 = "path"。 関連するサポートファイルとプロキシ/スタブ dll もコピーします (この詳細については後述します)。 実装**winmd**をサーバーディレクトリの場所にコピーできないと、サイドロードされたすべてのアプリケーションの RuntimeClass に対する呼び出しで、"クラスが登録されていません" というエラーがスローされます。 プロキシ/スタブをインストールできなかった場合 (または登録に失敗した場合) は、すべての呼び出しが戻り値なしで失敗します。 この後者のエラーは、多くの場合、表示されている例外に関連して**いません**。
この構成エラーが原因で例外が発生した場合は、"無効なキャスト" が参照されている可能性があります。

**サーバー実装に関する考慮事項**

デスクトップ Windows ランタイムサーバーは、"ワーカー" または "タスク" に基づいていると考えることができます。 サーバーへのすべての呼び出しは UI 以外のスレッドで動作し、すべてのコードはマルチスレッド対応で安全である必要があります。 サイドロードアプリケーションのどの部分がサーバーの機能を呼び出しているかも重要です。 サイドロードアプリケーションの UI スレッドから長時間実行されるコードを呼び出さないようにすることが重要です。 これを実現するには、主に次の2つの方法があります。

1.  UI スレッドからサーバーの機能を呼び出す場合は、常に、サーバーの公開領域と実装で非同期パターンを使用します。

2.  サイドロードアプリケーションのバックグラウンドスレッドからサーバーの機能を呼び出します。

**サーバーでの非同期 Windows ランタイム**

アプリケーションモデルのプロセス間の性質上、サーバーの呼び出しには、インプロセスでのみ実行されるコードよりも多くのオーバーヘッドがあります。 通常は、メモリ内の値を返す単純なプロパティを呼び出すことができます。これは、UI スレッドのブロックが問題にならないほど高速に実行されるためです。 ただし、任意の並べ替えの i/o を含むすべての呼び出し (すべてのファイル処理とデータベースの取得を含む) は、呼び出し元の UI スレッドをブロックし、無応答によってアプリケーションを終了させる可能性があります。 また、パフォーマンス上の理由から、オブジェクトのプロパティ呼び出しは、このアプリケーションアーキテクチャでは使用しないことをお勧めします。
詳細については、次のセクションで説明します。

適切に実装されたサーバーは、通常、Windows ランタイム非同期パターンを使用して、UI スレッドから直接行われた呼び出しを実装します。 これは、このパターンに従うことによって実装できます。 最初に、宣言 (付随するサンプルからの宣言) を示します。

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

これは、整数を返す Windows ランタイム非同期操作を宣言します。
通常、非同期操作の実装では、次の形式をとります。

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**メモ**実装の書き込み中に、長時間実行される可能性のある他の操作を待機することは一般的です。 その場合は、次のように、**タスクの実行**コードを宣言する必要があります。

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

この非同期メソッドのクライアントは、他の Windows ランタイム aysnc 操作と同様に、この操作を待機できます。

**アプリケーションのバックグラウンドスレッドからサーバーの機能を呼び出す**

クライアントとサーバーの両方が同じ組織によって記述されるのが一般的であるため、プログラミングプラクティスを採用して、サーバーへのすべての呼び出しが、サイドロードアプリケーションのバックグラウンドスレッドによって行われるようにすることができます。 サーバーから1つ以上のデータバッチを収集する直接呼び出しは、バックグラウンドスレッドから行うことができます。 結果が完全に取得されると、アプリケーションプロセス内のメモリ内のデータのバッチは、通常、UI スレッドから直接取得できます。 C @ no__t_0_ オブジェクトは、バックグラウンドスレッドと UI スレッド間の自然なアジャイルであるため、この種の呼び出しパターンでは特に便利です。\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Windows ランタイムプロキシの作成と展開

IPC アプローチでは、2つのプロセス間で Windows ランタイムインターフェイスをマーシャリングする必要があるため、グローバルに登録された Windows ランタイムプロキシとスタブを使用する必要があります。

**Visual Studio でのプロキシの作成**

通常の UWP アプリパッケージ内で使用するプロキシおよびスタブを作成および登録するプロセスについては、「 [Windows ランタイムコンポーネントでのイベントの発生](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))」を参照してください。
この記事で説明する手順は、以下で説明するプロセスよりも複雑です。これは、(グローバルに登録するのではなく) アプリケーションパッケージ内にプロキシ/スタブを登録する必要があるためです。

**手順 1:** デスクトップコンポーネントプロジェクトのソリューションを使用して、Visual Studio でプロキシ/スタブプロジェクトを作成します。

**ソリューション > Visual C++ > Win32 コンソールの [DLL の選択] オプション > > プロジェクトを追加します。**

以下の手順では、サーバーコンポーネントが**Mywinrtcomponent**として使用されていることを前提としています。

**手順 3:** すべての CPP/H ファイルをプロジェクトから削除します。

**手順 4:** 前のセクション「コントラクトの定義」には、 **winmdidl**、 **midl**、 **mdmerge .Exe**などを実行するビルド後コマンドが含まれています。 このビルド後コマンドの midl ステップからの出力の1つに、次の4つの重要な出力が生成されます。

a) Dlldata. c

b) ヘッダーファイル (例、MyWinRTComponent .h)

c) \*\_i. c ファイル (例、MyWinRTComponent\_i. c)

d) \*\_p. c ファイル (たとえば、MyWinRTComponent\_p .c)

**手順 5:** これら4つの生成されたファイルを "MyWinRTProxy" プロジェクトに追加します。

**手順 6:** Def ファイルを "MyWinRTProxy" プロジェクト **(プロジェクト > 追加新しい項目 > コード > モジュール定義ファイル**) に追加し、内容を次のように更新します。

ライブラリ MyWinRTComponent. Proxy .dll

エクスポート

DllCanUnloadNow プライベート

DllGetClassObject プライベート

DllRegisterServer プライベート

DllUnregisterServer プライベート

**手順 7:** "MyWinRTProxy" プロジェクトのプロパティを開きます。

**Comfiguration プロパティ > 全般 > ターゲット名:**

MyWinRTComponent。プロキシ

**C/C++ > プリプロセッサ定義 > 追加**

32\_WINDOWS;\_プロキシ\_DLL "を登録する

**C/C++ > プリコンパイル済みヘッダー: [プリコンパイル済みヘッダーを使用しない] を選択します。**

**リンカー > 全般 > インポートライブラリを無視する: [はい] を選択します**

**リンカー > 追加の依存関係を入力 >: rpcrt4 を追加します。**

**リンカー > windows メタデータを生成 > windows メタデータ: [いいえ] を選択します。**

**手順 8:** "MyWinRTProxy" プロジェクトをビルドします。

**プロキシの展開**

プロキシはグローバルに登録されている必要があります。 これを行う最も簡単な方法は、インストールプロセスがプロキシ dll で DllRegisterServer を呼び出すようにすることです。 この機能は x86 用に構築されたサーバーのみをサポートしているため (つまり、64ビットをサポートしていない)、最も簡単な構成は、32ビットサーバー、32ビットプロキシ、および32ビットのサイドロードアプリケーションを使用することです。 通常、プロキシは、デスクトップコンポーネントの実装**winmd**と共に配置されます。

追加の構成手順を1つ実行する必要があります。 サイドロードされたプロセスでプロキシを読み込んで実行するには、ディレクトリを ALL_APPLICATION_PACKAGES に対して "読み取り/実行" としてマークする必要があります。 これを行うには、 **icacls**コマンドラインツールを使用します。 このコマンドは、実装**winmd**とプロキシ/スタブ dll が存在するディレクトリで実行する必要があります。

*icacls./T/grant \*S-1-15-2-1: RX*

## <a name="patterns-and-performance"></a>パターンとパフォーマンス

クロスプロセストランスポートのパフォーマンスは慎重に監視することが非常に重要です。 プロセス間呼び出しは、インプロセス呼び出しと比べて、少なくとも2倍の負荷がかかります。 "高い" メッセージ交換の作成クロスプロセス、またはビットマップイメージなどの大きなオブジェクトの繰り返し転送を実行すると、予期しないアプリケーションのパフォーマンスが発生する可能性があります。

考慮すべき事項の一覧を次に示します。

-   アプリケーションの UI スレッドからサーバーへの同期メソッド呼び出しは、常に避ける必要があります。 アプリケーションのバックグラウンドスレッドからメソッドを呼び出し、必要に応じて CoreWindowDispatcher を使用して結果を UI スレッドに取得します。

-   アプリケーションの UI スレッドからの非同期操作の呼び出しは安全ですが、次に説明するパフォーマンスの問題について検討してください。

-   結果の一括転送によって、プロセス間の頻繁な通信が軽減されます。 これは通常、Windows ランタイム配列構成体を使用して実行されます。

-   *リスト<T>* を返します。ここで、 *t*は非同期操作またはプロパティフェッチからのオブジェクトです。これにより、多数のプロセス間頻繁な通信が発生します。 たとえば、 *&lt;People&gt;* オブジェクトの一覧を返すとします。 各イテレーションパスは、プロセス間の呼び出しになります。 返される各*People*オブジェクトはプロキシによって表され、その個々のオブジェクトのメソッドまたはプロパティを呼び出すたびに、プロセス間の呼び出しが発生します。 そのため、 *Count*のサイズが大きい場合に、"無害な"*リスト&lt;People&gt;* オブジェクトによって、多数の低速呼び出しが発生します。 配列内のコンテンツの構造体の一括転送によるパフォーマンスの向上。 例:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

次に、*リスト&lt;個人オブジェクト&gt;* ではなく、* 個人構造体\[\]* を返します。
これにより、1つのプロセス間の "ホップ" 内のすべてのデータが取得されます。

パフォーマンスに関するすべての考慮事項と同様に、測定とテストは重要です。 時間の長さを決定するために、さまざまな操作に最適なテレメトリを挿入する必要があります。 範囲全体で測定することが重要です。たとえば、サイドロードアプリケーションで特定のクエリのすべての*People*オブジェクトを使用するには、どのくらいの時間がかかりますか。

別の方法として、さまざまなロードテストがあります。 これを行うには、パフォーマンステストフックをアプリケーションに配置して、サーバー処理に遅延読み込みを発生させることができます。 これにより、さまざまな種類の負荷と、サーバーのパフォーマンスに対するアプリケーションの反応をシミュレートできます。
このサンプルでは、適切な非同期手法を使用して、時間の遅延をコードに含める方法を示します。 挿入する遅延の正確な量と、人為的な負荷に入れるランダム化の範囲は、アプリケーションの設計と、アプリケーションが実行される環境によって異なります。

## <a name="development-process"></a>開発プロセス

サーバーに変更を加えた場合は、以前に実行したインスタンスが実行されていないことを確認する必要があります。 COM は最終的にプロセスを清掃しますが、ランダウンタイマーの時間は反復開発の効率よりも長くなります。 したがって、以前に実行されたインスタンスを強制終了するのは、開発時の通常の手順です。 そのためには、サーバーをホストしている dllhost インスタンスを開発者が追跡する必要があります。

サーバープロセスは、タスクマネージャーまたは他のサードパーティ製アプリを使用して検出および強制終了できます。 コマンドラインツール * * TaskList * * も含まれており、柔軟な構文があります。次に例を示します。

  
 | **コマンド** | **アクション** |
 | ------------| ---------- |
 | tasklist | 実行中のすべてのプロセスを作成時間のおおよその順序で一覧表示します。最後に作成されたプロセスは一番下の方にあります。 |
 | tasklist/FI "IMAGENAME eq dllhost.exe .exe"/M | すべての dllhost.exe インスタンスに関する情報を一覧表示します。 /M スイッチには、読み込まれたモジュールが一覧表示されます。 |
 | tasklist/FI "PID eq 12564"/M | このオプションを使用すると、その PID がわかっている場合に dllhost.exe を照会できます。 |

ブローカーサーバーのモジュールリストでは、読み込まれたモジュールの一覧に*clrhost .dll*が一覧表示されます。

## <a name="resources"></a>リソース

-   [Windows 10 および VS 2015 用の仲介型 WinRT コンポーネントプロジェクトテンプレート](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT 仲介型 WinRT コンポーネントのサンプル](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [信頼性と信頼性に優れた Microsoft Store アプリの提供](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [アプリのコントラクトと拡張機能 (Windows ストアアプリ)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Windows 10 でアプリをサイドロードする方法](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [UWP アプリを企業に展開する](https://go.microsoft.com/fwlink/p/?LinkID=264770)

