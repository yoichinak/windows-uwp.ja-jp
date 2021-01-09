---
description: MRT.DLL コアコンポーネントの概要と、それらがアプリケーションリソースの読み込みにどのように機能するか (Project レユニオン)
title: MRT.DLL Core の概要 (プロジェクトのレユニオン)
ms.topic: article
ms.date: 12/11/2020
keywords: MRT.DLL、MRTCore、pri、makepri、resources、リソースの読み込み
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: ce07491dcab2a11738bd5407e9094d8d780ea219
ms.sourcegitcommit: 30d1a27fd78d198cec5c50af5621f9e65c7b965e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2021
ms.locfileid: "98043061"
---
# <a name="introduction-to-mrt-core-project-reunion"></a>MRT.DLL Core の概要 (プロジェクトのレユニオン)

MRT.DLL Core は、[プロジェクトのレユニオン](../index.md)の一部として配布される最新の Windows[リソース管理システム](/windows/uwp/app-resources/resource-management-system)の合理化されたバージョンです。

MRT.DLL Core には、ビルド時と実行時の両方の機能があります。 ビルド時に、システムは、アプリとしてパッケージ化されているリソースのさまざまなバリエーションすべてのインデックスを作成します。 このインデックスがパッケージ リソース インデックス (PRI) であり、アプリのパッケージにも含まれています。

## <a name="package-resource-index-pri-file"></a>パッケージ リソース インデックス (PRI) ファイル

すべてのアプリ パッケージには、アプリ内のリソースのバイナリ インデックスが格納されている必要があります。 このインデックスは、ビルド時に作成され、1つまたは複数のリソース (resw) ファイルに格納されます。

Resources.resw ファイルには、実際の文字列リソースと、パッケージ内のさまざまなファイルを参照するインデックス付きファイルパスのセットが含まれています。
パッケージには、通常、リソース (リソース. resources.resw) ごとに1つの resources.resw ファイルが含まれています。 ResourceManager がインスタンス化されると、各パッケージのルートにあるリソースの resw ファイルが自動的に読み込まれます。

各 resources.resw ファイルには、リソースマップと呼ばれるリソースの名前付きコレクションが含まれています。 パッケージからの resources.resw ファイルが読み込まれると、パッケージ id 名と一致するようにリソースマップ名が検証されます。

Resw ファイルにはデータのみが含まれているので、ポータブル実行可能 (PE) 形式は使用しません。 これらは、特にデータのみであるように設計されています。

## <a name="using-mrt-core-to-access-app-resources"></a>MRT.DLL Core を使用してアプリリソースにアクセスする

### <a name="resource-loader-basic-functionality"></a>リソースローダー (基本的な機能)

プログラムによってアプリリソースにアクセスする最も簡単な方法は、 [Microsoft の ApplicationModel. .resources](/windows/winui/api/microsoft.applicationmodel.resources) 名前空間と resourceloader クラスを使用することです。 ResourceLoader を使うと、一連のリソース ファイル、参照ライブラリ、または他のパッケージの文字列リソースへの基本的なアクセスが可能になります。

### <a name="resource-manager-advanced-functionality"></a>リソースマネージャー (高度な機能)

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)クラスは、列挙や検査などのリソースに関する追加情報を提供します。 このクラスは、**ResourceLoader** クラスが提供する情報よりも多くの情報を提供します。

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) オブジェクトは、英語向けの文字列 "Hello World" や scale-100 解像度に固有の修飾子付きのイメージ リソースである "logo.scale-100.jpg" など、単一の具象リソース値と修飾子の組み合わせです。

アプリで使うことができるリソースは、[ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) オブジェクトを使ってアクセスできる階層コレクションに格納されます。 **ResourceManager** クラスを使うと、アプリで使われる各種のトップレベルの **ResourceMap** インスタンスにアクセスできます。これらのインスタンスは、アプリのさまざまなパッケージに対応しています。 [ResourceManager. MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap)の値は、現在のアプリケーションパッケージのリソースマップに対応し、参照されているフレームワークパッケージは除外されます。 それぞれの **ResourceMap** には、パッケージのマニフェストに指定されたパッケージ名に基づく名前が付けられます。 **Resourcemap** 内にはサブツリーがあります (「 [resourcemap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)を参照してください)。 通常、サブツリーは、リソースが含まれるリソース ファイルに対応します。

**ResourceManager** は、アプリの文字列リソースへのアクセスをサポートするだけでなく、さまざまなファイル リソースを列挙し検査する機能も保持します。 ファイルとその他のリソース (ファイル内から提供されるリソース) との衝突を回避するために、すべてのインデックス付きファイル パスは、予約済みの "Files" **ResourceMap** サブツリーに配置されます。 たとえば、ファイル ' \Images\logo.png ' はリソース名 ' Files/images/logo.png ' に対応しています。

### <a name="resourcecontext"></a>ResourceContext

リソース候補は、リソース修飾子の値 (言語、スケール、コントラストなど) のコレクションである特定の [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext) に基づいて選ばれます。 既定のコンテキストでは、上書きされない限り、それぞれの修飾子の値に対してアプリの現在の構成が使われます。 たとえば、画像などのリソースはスケールで修飾することができます。スケールはモニターごとに異なり、したがってアプリケーション ビューごとに異なります。 この理由から、アプリケーション ビューはそれぞれ別個の既定のコンテキストを持ちます。 リソース候補を取得するときは常に **ResourceContext** インスタンスを渡して、特定のビューに最も適した値を取得する必要があります。

### <a name="important-apis"></a>重要な API

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>サンプル

MRT.DLL Core API の使用方法を示すサンプルについては、 [Mrt.dll core サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)を参照してください。
