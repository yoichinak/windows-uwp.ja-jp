---
Description: Windows アプリのパッケージにパッケージ化したデスクトップ アプリケーションで Windows 10 ユーザー向けの最新のエクスペリエンスを追加する方法について説明します。
title: パッケージのデスクトップ アプリを最新化します。
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 191a8b8a007a866f37780a7c52cd40047dc9817f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215207"
---
# <a name="features-that-require-package-identity"></a>パッケージ ID が必要な機能

使用してデスクトップ アプリを更新する場合[最新の Windows 10 エクスペリエンス](index.md)、多くの機能が MSIX パッケージにパッケージ化、デスクトップ アプリでのみ使用できます。

MSIX には、すべての Windows アプリ、WPF、Windows フォーム、Win32 アプリのユニバーサル パッケージ化エクスペリエンスを提供する最新 Windows アプリ パッケージ形式です。 デスクトップの Windows アプリをパッケージ化するには、ライブ タイルや通知などの最新の Windows 10 エクスペリエンスをアプリに統合することができます。 また、堅牢なインストールと更新エクスペリエンス、機能の柔軟性の高いシステムでは、Microsoft Store、エンタープライズ管理、および多くのカスタムの配布モデルのサポートで管理セキュリティ モデルへのアクセスを取得します。 詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。

デスクトップ アプリをパッケージ化する場合は、パッケージ id、パッケージの拡張機能、およびパッケージ化されたアプリでの UWP コンポーネントを必要とする UWP Api を使用できます。 詳細については、次の記事を参照してください。

## <a name="use-uwp-apis-that-require-package-identity"></a>パッケージ id を必要とする UWP Api を使用して、

一部の UWP Api を必要と[パッケージ identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)デスクトップ アプリで使用します。 (パッケージ マニフェストを含む) MSIX パッケージは、この id を提供します。

詳細については、次を参照してください。 [Api の一覧はこの](desktop-to-uwp-supported-api.md#list-of-apis)します。

## <a name="integrate-with-package-extensions"></a>パッケージ拡張機能との統合

アプリケーションは、システムと統合する必要がある場合 (例: ファイアウォール規則の確立) と、アプリケーションのパッケージ マニフェストではこの記述、システムが処理してくれます。 これらのタスクのほとんどは、まったくコードを記述する必要がありません。 Xml マニフェスト内のビットを使用して操作を実行できます、ユーザーがログオンしたときにプロセスを開始、ファイル エクスプ ローラーで、アプリケーションに統合およびアプリケーションを追加するように他のアプリに表示される印刷のターゲットの一覧。

詳細については、次を参照してください。[パッケージ拡張機能とデスクトップ アプリを統合](desktop-to-uwp-extensions.md)します。

## <a name="extend-with-uwp-components"></a>UWP コンポーネントによる拡張

一部の Windows 10 エクスペリエンス (タッチ対応 UI ページなど) は、最新のアプリ コンテナー内で実行する必要があります。 一般に、最初に決定する必要あるかどうかによって、エクスペリエンスを追加することができます[強化](desktop-to-uwp-enhance.md)UWP Api を使用した既存のデスクトップ アプリケーションです。 コード型の場合は、エクスペリエンスを実現するために、UWP コンポーネントを使用する必要が UWP プロジェクトをソリューションに追加するアプリ サービスを使用して、お客様のデスクトップ アプリケーションと UWP コンポーネント間の通信し、ことができます。

詳細については、次を参照してください。 [UWP コンポーネントを使ってデスクトップ アプリを拡張](desktop-to-uwp-extend.md)します。

## <a name="distribute"></a>配布

アプリケーションを配布するには、Microsoft Store を発行すること、またはサイドローディングによって他のシステムにします。

参照してください[パッケージ化されたデスクトップ アプリを配布](desktop-to-uwp-distribute.md)します。
