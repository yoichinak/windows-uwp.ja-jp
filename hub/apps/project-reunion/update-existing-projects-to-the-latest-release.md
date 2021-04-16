---
description: この記事では、以前のプレビューバージョンまたはリリースバージョンの Project レユニオンまたは WinUI 3 で作成されたプロジェクトを最新バージョンに更新する手順について説明します。
title: 既存のプロジェクトを最新リリースの Project レユニオンに更新する
ms.topic: article
ms.date: 04/07/2021
keywords: windows win32, デスクトップ開発, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b4836da83b6bbb2214cc6c5ad446144aac0e168d
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574738"
---
# <a name="update-existing-projects-to-the-latest-release-of-project-reunion"></a>既存のプロジェクトを最新リリースの Project レユニオンに更新する

以前の preview またはリリースバージョンの Project レユニオンまたは WinUI 3 を使用してプロジェクトを作成した場合は、最新の安定したリリース (バージョン 0.5.5) を使用するようにプロジェクトを更新できます。

> [!NOTE]
> アプリの個別のシナリオはそれぞれ独自であるため、これらの手順で問題が発生する可能性があります。 これらには慎重に従い、問題が見つかった場合は、[GitHub リポジトリでバグを報告](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)してください。

## <a name="update-from-project-reunion-050"></a>Project レユニオン0.5.0 からの更新

Project レユニオンバージョン0.5.0 を使用してプロジェクトを作成した場合は、次の手順に従ってプロジェクトを Project レユニオンバージョン 0.5.5 (最新の安定版リリース) に更新できます。 このバージョンには、いくつかの重要なバグ修正が含まれています。

> [!NOTE]
> Project レユニオン 0.5 VSIX を使用してプロジェクトを作成した場合は、次の手動の手順を実行せずに、Visual Studio 拡張機能マネージャーを使用してプロジェクトを自動的に更新できます。 Visual Studio 2019 で、[**拡張** 機能の管理] をクリックし、  ->  左側のメニューバーから [**更新プログラム**] を選択します。 一覧から [Project レユニオン] を選択し、[ **更新**] をクリックします。 

開始する前に、最新のプロジェクトレユニオン VSIX と NuGet パッケージを含め、すべての Project レユニオン0.5 前提条件がインストールされていることを確認します。 詳細については、 [インストール手順](get-started-with-project-reunion.md#set-up-your-development-environment)を参照してください。

まず、次の手順を実行します。
- Wapproj ファイルで、 **Targetplatformminversion** が10.0.17763.0 よりも古い場合は、10.0.17763.0 に変更します。

次に、これらの変更をプロジェクトに加えます。
1. 最新の安定版リリースからのすべての変更を取得するには、.NET SDK を最新バージョンに明示的に設定する必要があります。 これを行うには、.csproj ファイルに次の項目グループを追加し、プロジェクトを保存します。

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    .NET 5.0.6 が5月に利用可能になると、これらの行を削除できることに注意してください。 

3. Visual Studio で、 **[ツール]**  ->  **[NuGet パッケージ マネージャー]**  ->  **[パッケージ マネージャー コンソール]** の順に移動します。

4. 次のコマンドを入力します。

    ```Console
    uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.5 -ProjectName {yourProjectName}
    ```

5. アプリケーション (パッケージ).wapproj で次の変更を行います。
  
    1. 次のセクションを追加します。

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.5]">
                <IncludeAssets>build</IncludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

    2. 次の行を削除します。

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        および

        ```xml
        <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
        <Import Project="$(Microsoft_WinUI_AppX_targets)" />
        ```

        この項目グループは次のとおりです。

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
            <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

## <a name="update-from-project-reunion-05-preview"></a>Project レユニオン0.5 プレビューからの更新

Project レユニオン 0.5 Preview を使用してプロジェクトを作成した場合は、次の手順に従ってプロジェクトを Project レユニオンバージョン 0.5.5 (最新の安定版リリース) に更新できます。

開始する前に、最新のプロジェクトレユニオン VSIX と NuGet パッケージを含め、すべての Project レユニオン0.5 前提条件がインストールされていることを確認します。 詳細については、 [インストール手順](get-started-with-project-reunion.md#set-up-your-development-environment)を参照してください。

まず、次の手順を実行します。

- Wapproj ファイルで、 **Targetplatformminversion** が10.0.17763.0 よりも古い場合は、10.0.17763.0 に変更します。
- アプリで `Application.Suspending` イベントが使用される場合は、`Application.Suspending` がデスクトップ アプリに対して呼び出されなくなったため、その行を削除または変更してください。 詳細については、[API リファレンス ドキュメント](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending)を参照してください。
- C++ と C# の両方のアプリの既定のプロジェクトテンプレートには、次の行が含まれています。 これらの行がまだコード内に存在する場合は、必ず削除してください。

    ```csharp
    this.Suspending += OnSuspending;
    ```

    ```cpp
    Suspending({ this, &App::OnSuspending });
    ```

次に、これらの変更をプロジェクトに加えます。
1. 最新の安定版リリースからのすべての変更を取得するには、.NET SDK を最新バージョンに明示的に設定する必要があります。 これを行うには、.csproj ファイルに次の項目グループを追加し、プロジェクトを保存します。

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    .NET 5.0.6 が5月に利用可能になると、これらの行を削除できることに注意してください。

2. Visual Studio で、 **[ツール]**  ->  **[NuGet パッケージ マネージャー]**  ->  **[パッケージ マネージャー コンソール]** の順に移動します。
3. 次のコマンドを入力します。

    ```Console
    uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}
    uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.5 -ProjectName {yourProjectName}
    ```

4. アプリケーション (パッケージ).wapproj で次の変更を行います。
  
    1. 次のセクションを追加します。

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.5]">
                <IncludeAssets>build</IncludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

    2. 次の行を削除します。

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        および

        ```xml
        <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
        <Import Project="$(Microsoft_WinUI_AppX_targets)" />
        ```

        この項目グループは次のとおりです。

        ```xml
        <ItemGroup>
            <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
            <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
              <ExcludeAssets>all</ExcludeAssets>
            </PackageReference>
        </ItemGroup>
        ```

## <a name="update-from-winui-3-preview-4"></a>WinUI 3 Preview 4 からの更新

WinUI 3 Preview 4 を使用してプロジェクトを作成した場合は、次の手順に従ってプロジェクトを Project レユニオンバージョン 0.5.5 (最新の安定版リリース) に更新できます。

開始する前に、最新のプロジェクトレユニオン VSIX と NuGet パッケージを含め、すべての Project レユニオン0.5 前提条件がインストールされていることを確認します。 詳細については、 [インストール手順](get-started-with-project-reunion.md#set-up-your-development-environment)を参照してください。

まず、次の手順を実行します。

- Wapproj ファイルで、TargetPlatformMinVersion が10.0.17763.0 よりも古い場合は、10.0.17763.0 に変更します。
- アプリで `Application.Suspending` イベントが使用される場合は、`Application.Suspending` がデスクトップ アプリに対して呼び出されなくなったため、その行を削除または変更してください。 詳細については、[API リファレンス ドキュメント](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true)を参照してください。
- C++ と C# の両方のアプリの既定のプロジェクトテンプレートには、次の行が含まれています。 これらの行がまだコード内に存在する場合は、必ず削除してください。

    ```csharp
    this.Suspending += OnSuspending;
    ```

    ```cpp
    Suspending({ this, &App::OnSuspending });
    ```

次に、これらの変更をプロジェクトに加えます。
1. 最新の安定版リリースからのすべての変更を取得するには、.NET SDK を最新バージョンに明示的に設定する必要があります。 これを行うには、.csproj ファイルに次の項目グループを追加し、プロジェクトを保存します。

    ```xml
    <ItemGroup>            
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
        <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
    </ItemGroup>
    ```

    .NET 5.0.6 が5月に利用可能になると、これらの行を削除できることに注意してください。
2. Visual Studio で、 **[ツール]**  ->  **[NuGet パッケージ マネージャー]**  ->  **[パッケージ マネージャー コンソール]** の順に移動します。
3. 次のコマンドを入力します。

    ```Console
    uninstall-package Microsoft.WinUI -ProjectName {yourProject}
    install-package Microsoft.ProjectReunion -Version 0.5.2 -ProjectName {yourProjectName}
    ```

4. アプリケーション (パッケージ).wapproj で次の変更を行います。

    1. 次のセクションを追加します。

        ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.2]">
            <IncludeAssets>build</IncludeAssets>
          </PackageReference>
        </ItemGroup>
        ```

    2. 次の行を削除します。

        ```xml
        <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
        ```

        ```xml
        <Import Project="$(AppxTargetsLocation)Microsoft.WinUI.AppX.targets" />
        ```

5. プロジェクトの {自分のプロジェクト}(パッケージ)/build/ フォルダーにある、既存の `Microsoft.WinUI.AppX.targets` ファイルを削除します。
