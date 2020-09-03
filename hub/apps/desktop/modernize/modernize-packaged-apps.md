---
Description: Windows アプリ パッケージにパッケージ化したデスクトップ アプリケーションに、Windows 10 ユーザー向けの最新のエクスペリエンスを追加する方法について説明します。
title: パッケージ化したデスクトップ アプリの現代化
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d1ce2e7dc434558ac1efd52f6def99d63b38c57e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161516"
---
# <a name="features-that-require-package-identity"></a>パッケージ ID が必要な機能

[最新の Windows 10 エクスペリエンス](index.md)によって、デスクトップ アプリを更新する場合、多くの機能が、[パッケージ ID](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) を持つデスクトップ アプリでのみ利用できます。 デスクトップ アプリにパッケージ ID を付与する方法はいくつかあります。

* [MSIX パッケージ](/windows/msix/desktop/desktop-to-uwp-root)でパッケージ化します。 MSIX は、すべての Windows アプリ、WPF、Windows フォーム、Win32 アプリ用のユニバーサルなパッケージ化エクスペリエンスを提供するモダンなアプリ パッケージ形式です。 これは、堅牢なインストールと更新のエクスペリエンス、柔軟な機能システムによる管理されたセキュリティ モデル、Microsoft Store のサポート、エンタープライズ管理、および多くのカスタム配布モデルを提供します。 詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。
* デスクトップ アプリを配置するために MSIX パッケージを導入できない場合、Windows 10 バージョン 2004 以降では、パッケージ マニフェストのみを含む "*スパース MSIX パッケージ*" を作成することでパッケージ ID を付与できます。 詳細については、「[パッケージ化されていないデスクトップ アプリに ID を付与する](grant-identity-to-nonpackaged-apps.md)」を参照してください。

デスクトップ アプリにパッケージ ID がある場合は、アプリで次の機能を使用できます。

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>パッケージ ID が必要な Windows ランタイム API の使用

次の一覧の Windows ランタイム API では、デスクトップ アプリでパッケージ ID を使用する必要があります。[API の一覧](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>パッケージ拡張機能との統合

アプリケーションをシステムと統合する必要がある場合 (ファイアウォール規則を確立する場合など)、アプリケーションのパッケージ マニフェストにそのことを記述すると、システムによって残りの処理が行われます。 これらのタスクのほとんどは、まったくコードを記述する必要がありません。 マニフェストに少し XML を追加するだけで、ユーザーがログオンしたときにプロセスを開始する、アプリケーションをエクスプローラーに統合する、他のアプリに表示される印刷先の一覧に対象アプリケーションを追加する、などの処理を行うことができます。

詳細については、[デスクトップ アプリケーションとパッケージ拡張機能の統合](desktop-to-uwp-extensions.md)に関するページをご覧ください。

## <a name="extend-with-uwp-components"></a>UWP コンポーネントによる拡張

一部の Windows 10 エクスペリエンス (タッチ対応 UI ページなど) は、最新のアプリ コンテナー内で実行する必要があります。 一般的に、Windows ランタイム API を使用して既存のデスクトップ アプリケーションを[強化](desktop-to-uwp-enhance.md)することでエクスペリエンスを追加できるかどうかを、最初に判断する必要があります。 エクスペリエンスを実現するために UWP コンポーネントを使用する必要がある場合、ソリューションに UWP プロジェクトを追加すると、アプリ サービスを使用してデスクトップ アプリケーションと UWP コンポーネントの間で通信を行うことができます。

詳細については、[UWP コンポーネントによるデスクトップ アプリケーションの拡張](desktop-to-uwp-extend.md)に関するページを参照してください。

## <a name="distribute"></a>配布

MSIX パッケージにアプリをパッケージ化する場合、Microsoft Store に公開するか、他のシステムにサイドローディングすることで、アプリを配布できます。

「[パッケージ化されたデスクトップ アプリを配布する](desktop-to-uwp-distribute.md)」をご覧ください。