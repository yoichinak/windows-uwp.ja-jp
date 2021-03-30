---
title: Project レユニオンを使用するアプリをデプロイする
description: この記事では、Project レユニオンを使用するアプリをデプロイする手順について説明します。
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32、windows アプリ開発、プロジェクトのレユニオン
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 967b38be2ee1485c28175b86c016e2bca1a8ce5b
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730797"
---
# <a name="deploy-apps-that-use-project-reunion"></a>Project レユニオンを使用するアプリをデプロイする

Project レユニオン0.5 を使用するアプリを他のコンピューターに展開するには、 [Msix](/windows/msix)を使用してアプリをパッケージ化する必要があります。 Project レユニオンは、今後のリリースで、アンパッケージアプリの展開をサポートします。 今後の計画の詳細については、「 [ロードマップ](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md)」を参照してください。

既定では、Visual Studio の Project レユニオン拡張機能に用意されている [WinUI プロジェクトテンプレート](..\winui\winui3\winui-project-templates-in-visual-studio.md) のいずれかを使用してプロジェクトを作成すると、アプリを msix パッケージにビルドするように構成された [Windows アプリケーションパッケージプロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) がプロジェクトに含まれます。 このプロジェクトを構成して、アプリの MSIX パッケージを作成する方法の詳細については、「[Visual Studio でのデスクトップまたは UWP アプリのパッケージ化](/windows/msix/package/packaging-uwp-apps)」を参照してください。

アプリの MSIX パッケージを作成したら、他のコンピューターに展開するためのオプションがいくつかあります。 詳細については、「[MSIX の展開を管理する](/windows/msix/desktop/managing-your-msix-deployment-overview)」を参照してください。

> [!NOTE]
> Project レユニオン0.5 は、実稼働環境での MSIX パッケージデスクトップアプリ (C#/.NET 5 または C++/Win32) での使用がサポートされています。 Project レユニオン0.5 を使用するパッケージデスクトップアプリは、Microsoft Store に発行できます。

## <a name="dependencies-on-the-project-reunion-framework-package"></a>Project レユニオンフレームワークパッケージへの依存関係

プロジェクトレユニオンを使用するアプリをビルドすると、アプリは、 *フレームワークパッケージ* を使用してエンドユーザーに配布される一連の project レユニオンランタイムコンポーネントを参照します。 フレームワークパッケージを使用すると、パッケージアプリは、アプリパッケージにバンドルするのではなく、ユーザーのデバイス上の単一の共有ソースを介して、プロジェクトのレユニオンコンポーネントにアクセスできます。 フレームワークパッケージには、Dll や API 定義 (COM および Windows ランタイム登録) など、独自のリソースも含まれています。 これらのリソースはアプリのコンテキストで実行されるため、アプリの機能と特権を継承し、独自の機能や特権をアサートすることはありません。

Project レユニオンフレームワークパッケージは、Microsoft Store を通じてエンドユーザーに展開される MSIX パッケージです。 セキュリティと信頼性の修正に加えて、最新のリリースで簡単かつ迅速に更新することができます。 コンピューター上で Project レユニオンを使用するすべてのアプリは、次の図に示すように、フレームワークパッケージの共有インスタンスに依存しています。

![アプリが Project レユニオンフレームワークパッケージにアクセスする方法の図](images/framework.png)

依存関係がどのように構成されるかは、アプリ内で Project レユニオンのプレリリース版とリリースバージョンのどちらを使用しているかによって異なります。 プロジェクトのレユニオンは、プレリリース版とリリース版で使用できるようになります。これらには、フレームワークパッケージのプレリリース版とリリース版が含まれます。 アプリは、必要な機能の正しいパッケージを参照していることを確認する必要があります。

## <a name="configure-dependencies-on-preview-versions-of-the-framework-package"></a>フレームワークパッケージのプレビューバージョンでの依存関係の構成

Project レユニオンのプレビュー版は、新機能に関する調査とフィードバックを目的としています。 これらは運用環境で使用するためのライセンスを付与されていないため、Microsoft Store に発行しないでください。

Visual Studio 用の Project レユニオン拡張機能のプレビューバージョン、または開発用コンピューターに Project レユニオン NuGet パッケージをインストールすると、ビルド時に NuGet パッケージの依存関係として、プレビューバージョンのフレームワークパッケージが配置されます。

## <a name="configure-dependencies-on-release-versions-of-the-framework-package"></a>Framework パッケージのリリースバージョンでの依存関係の構成

Project レユニオンのリリースバージョンは、運用環境での使用に対応しています。

開発用コンピューターにリリース版の Project レユニオン拡張機能または Project レユニオン NuGet パッケージをインストールし、指定された WinUI 3 プロジェクトテンプレートのいずれかを使用してプロジェクトを作成すると、生成されたパッケージマニフェストには、フレームワークパッケージに対する依存関係を指定する [Packagedependency](/uwp/schemas/appxpackage/uapmanifestschema/element-packagedependency) 要素が含まれます。

```xml
<Dependencies>
    <PackageDependency Name="Microsoft.ProjectReunion.0.5" MinVersion="0.52103.9000.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
</Dependencies>
```

ただし、アプリケーションパッケージを手動でビルドする場合は、この **packagedependency** 要素をパッケージマニフェストに追加して、Project レユニオンフレームワークパッケージに対する依存関係を宣言する必要があります。

## <a name="updates-and-versioning-of-the-framework-package"></a>フレームワークパッケージの更新とバージョン管理

新しいバージョンの Project レユニオンフレームワークパッケージがリリースされると、すべてのアプリが新しいバージョンに更新されます。その際、コピーを再配布する必要はありません。 Windows では、最新バージョンのフレームワークがリリースされたときに更新されます。アプリは再起動時に最新のフレームワークパッケージバージョンを自動的に参照します。 以前のバージョンの framework パッケージは、システム上のアプリによって実行またはアクティブに使用されなくなるまで、システムから削除されません。

![アプリがプロジェクトのレユニオンフレームワークパッケージに更新プログラムを取得する方法の図](images/framework-update.png)

アプリの互換性は Microsoft およびプロジェクトのレユニオンに依存するアプリにとって重要であるため、Project レユニオンフレームワークパッケージは、 [セマンティックバージョン管理の 2.0.0](https://semver.org/) 規則に従います。 つまり、プロジェクトレユニオンのバージョン1.0 をリリースした後、Project レユニオンフレームワークパッケージはマイナーバージョンと修正プログラムバージョンの変更間の互換性を保証し、重大な変更はメジャーバージョンの更新間でのみ行われます。

## <a name="related-topics"></a>関連トピック

- [Project レユニオンを使用したデスクトップ Windows アプリの構築](index.md)
- [Project レユニオンを使ってみる](get-started-with-project-reunion.md)
