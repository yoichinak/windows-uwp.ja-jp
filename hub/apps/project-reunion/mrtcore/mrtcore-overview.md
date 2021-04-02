---
description: MRT.DLL コアコンポーネントの概要と、それらがアプリケーションリソースの読み込みにどのように機能するか (Project レユニオン)
title: リソースの管理 MRT.DLL コア (プロジェクトのレユニオン)
ms.topic: article
ms.date: 03/31/2021
keywords: MRT.DLL、MRTCore、pri、makepri、resources、リソースの読み込み
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 4b86e3d1b232a9c0da87808e8fa07a13b18ffdd6
ms.sourcegitcommit: d793c82587b8368e241d74be1473f4f0af5bb9ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/01/2021
ms.locfileid: "106164372"
---
# <a name="manage-resources-with-mrt-core"></a>MRT Core を使用してリソースを管理する 

MRT.DLL Core は、[プロジェクトのレユニオン](../index.md)の一部として配布される最新の Windows[リソース管理システム](/windows/uwp/app-resources/resource-management-system)の合理化されたバージョンです。

MRT.DLL Core には、ビルド時と実行時の両方の機能があります。 ビルド時に、システムは、アプリとしてパッケージ化されているリソースのさまざまなバリエーションすべてのインデックスを作成します。 このインデックスがパッケージ リソース インデックス (PRI) であり、アプリのパッケージにも含まれています。

## <a name="package-resource-index-pri-file"></a>パッケージ リソース インデックス (PRI) ファイル

すべてのアプリ パッケージには、アプリ内のリソースのバイナリ インデックスが格納されている必要があります。 このインデックスは、ビルド時に作成され、1つまたは複数の PRI ファイルに格納されます。 各 PRI ファイルは、リソース マップと呼ばれる、リソースの名前付きコレクションを含みます。

PRI ファイルには、実際の文字列リソースが含まれています。 埋め込みバイナリおよびファイルパスのリソースには、プロジェクトファイルから直接インデックスが付けられます。 パッケージには、通常、language という名前の単一の PRI ファイルが含まれています **。** [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)オブジェクトがインスタンス化されると、各パッケージのルートにある **リソースの pri** ファイルが自動的に読み込まれます。

PRI ファイルにはデータのみが格納され、移植可能な実行可能ファイル (PE) 形式は使用されません。 これらは、特にデータのみであるように設計されています。

> [!NOTE]
> MRT.DLL Core を使用して、C#/.NET 5 を使用する WinUI 3 プロジェクトの文字列およびイメージを取得する前に、これらのリソースがリソースの pri ファイルにインデックスを作成できるように構成されていることを確認する必要があります。 それ以外の場合、これらのリソースを MRT.DLL Core で取得することはできません。
>
> * 文字列リソースファイル (resw) の場合は、ファイルの [ **ビルドアクション** ] プロパティが [プライマリ **リソース**] に設定されていることを確認します。
> * イメージファイルの場合は、ファイルの [ **ビルドアクション** ] プロパティが [ **コンテンツ**] に設定されていることを確認します。

## <a name="access-app-resources-with-mrt-core"></a>MRT.DLL Core を使用してアプリリソースにアクセスする

MRT.DLL Core には、アプリリソースにアクセスするためのさまざまな方法が用意されています。

### <a name="basic-functionality-with-resourceloader"></a>ResourceLoader を使用した基本的な機能

プログラムによってアプリリソースにアクセスする最も簡単な方法は、 [Resourceloader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader) クラスを使用することです。 **ResourceLoader** を使うと、一連のリソース ファイル、参照ライブラリ、または他のパッケージの文字列リソースへの基本的なアクセスが可能になります。

### <a name="advanced-functionality-with-resourcemanager"></a>ResourceManager を使用した高度な機能

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)クラスは、列挙や検査などのリソースに関する追加情報を提供します。 このクラスは、**ResourceLoader** クラスが提供する情報よりも多くの情報を提供します。

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) オブジェクトは、英語向けの文字列 "Hello World" や scale-100 解像度に固有の修飾子付きのイメージ リソースである "logo.scale-100.jpg" など、単一の具象リソース値と修飾子の組み合わせです。

アプリで使うことができるリソースは、[ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) オブジェクトを使ってアクセスできる階層コレクションに格納されます。 **ResourceManager** クラスを使うと、アプリで使われる各種のトップレベルの **ResourceMap** インスタンスにアクセスできます。これらのインスタンスは、アプリのさまざまなパッケージに対応しています。 [ResourceManager. MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap)の値は、現在のアプリケーションパッケージのリソースマップに対応し、参照されているフレームワークパッケージは除外されます。 それぞれの **ResourceMap** には、パッケージのマニフェストに指定されたパッケージ名に基づく名前が付けられます。 **Resourcemap** 内にはサブツリーがあります (「 [resourcemap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)を参照してください)。 通常、サブツリーは、リソースが含まれるリソース ファイルに対応します。

**ResourceManager** は、アプリの文字列リソースへのアクセスをサポートするだけでなく、さまざまなファイル リソースを列挙し検査する機能も保持します。 ファイルとその他のリソース (ファイル内から提供されるリソース) との衝突を回避するために、すべてのインデックス付きファイル パスは、予約済みの "Files" **ResourceMap** サブツリーに配置されます。 たとえば、ファイル ' \Images\logo.png ' はリソース名 ' Files/images/logo.png ' に対応しています。

### <a name="qualify-resource-selection-with-resourcecontext"></a>リソースの選択を ResourceContext で修飾する

リソース候補は、リソース修飾子の値 (言語、スケール、コントラストなど) のコレクションである特定の [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext) に基づいて選ばれます。 既定のコンテキストでは、上書きされない限り、それぞれの修飾子の値に対してアプリの現在の構成が使われます。 たとえば、画像などのリソースはスケールで修飾することができます。スケールはモニターごとに異なり、したがってアプリケーション ビューごとに異なります。 この理由から、アプリケーション ビューはそれぞれ別個の既定のコンテキストを持ちます。 リソース候補を取得するときは常に **ResourceContext** インスタンスを渡して、特定のビューに最も適した値を取得する必要があります。

## <a name="sample"></a>サンプル

MRT.DLL Core API の使用方法を示すサンプルについては、 [Mrt.dll core サンプル](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)を参照してください。
