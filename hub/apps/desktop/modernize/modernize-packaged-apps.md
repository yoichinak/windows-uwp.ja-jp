---
Description: Windows アプリケーションパッケージにパッケージ化したデスクトップアプリケーションで Windows 10 ユーザー向けの最新のエクスペリエンスを追加する方法について説明します。
title: パッケージデスクトップアプリの最新化
ms.date: 04/22/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142511"
---
# <a name="features-that-require-package-identity"></a>パッケージ ID が必要な機能

[最新の Windows 10 エクスペリエンス](index.md)でデスクトップアプリを更新する場合、多くの機能は、[パッケージ id](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)を持つデスクトップアプリでのみ使用できます。 デスクトップアプリにパッケージ id を与えるには、いくつかの方法があります。

* これを[Msix パッケージ](/windows/msix/desktop/desktop-to-uwp-root)にパッケージ化します。 MSIX は、すべての Windows アプリ、WPF、Windows フォームおよび Win32 アプリに対してユニバーサルパッケージエクスペリエンスを提供する最新のアプリケーションパッケージ形式です。 堅牢なインストールと更新のエクスペリエンス、柔軟な機能システムを備えた管理されたセキュリティモデル、Microsoft Store、エンタープライズ管理、および多数のカスタム配布モデルのサポートを提供します。 詳しくは、MSIX ドキュメントの「[デスクトップ アプリケーションのパッケージ化](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)」をご覧ください。
* デスクトップアプリを配置するために MSIX パッケージを導入できない場合は、Windows 10 Insider Preview Build 10.0.19000.0 以降では、パッケージマニフェストのみを含む*スパース MSIX パッケージ*を作成することにより、パッケージ id を付与できます。 詳細については、「パッケージ化されてい[ないデスクトップアプリへの id の付与](grant-identity-to-nonpackaged-apps.md)」を参照してください。

デスクトップアプリにパッケージ id がある場合は、アプリで次の機能を使用できます。

## <a name="use-uwp-apis-that-require-package-identity"></a>パッケージ id を必要とする UWP Api を使用する

次の UWP Api の一覧では、パッケージ id をデスクトップアプリで使用する必要があります: [api の一覧](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>パッケージ拡張機能との統合

アプリケーションをシステムと統合する必要がある場合 (ファイアウォール規則を確立する場合など) は、アプリケーションのパッケージマニフェストでそれらを記述します。それ以外の処理はシステムによって行われます。 これらのタスクのほとんどは、まったくコードを記述する必要がありません。 マニフェストに XML を使用すると、ユーザーがログオンしたときにプロセスを開始したり、アプリケーションをエクスプローラーに統合したり、他のアプリに表示される印刷ターゲットのリストをアプリケーションに追加したりできます。

詳細については、「[デスクトップアプリとパッケージ拡張機能の統合](desktop-to-uwp-extensions.md)」を参照してください。

## <a name="extend-with-uwp-components"></a>UWP コンポーネントによる拡張

一部の Windows 10 エクスペリエンス (タッチ対応 UI ページなど) は、最新のアプリ コンテナー内で実行する必要があります。 一般に、既存のデスクトップアプリケーションを UWP Api で[強化](desktop-to-uwp-enhance.md)することによって、エクスペリエンスを追加できるかどうかを判断する必要があります。 UWP コンポーネントを使用してエクスペリエンスを実現する必要がある場合は、UWP プロジェクトをソリューションに追加し、app services を使用してデスクトップアプリケーションと UWP コンポーネントの間で通信を行うことができます。

詳細については、「 [UWP コンポーネントによるデスクトップアプリの拡張](desktop-to-uwp-extend.md)」を参照してください。

## <a name="distribute"></a>配布

アプリを MSIX パッケージでパッケージ化する場合は、Microsoft Store に発行するか、他のシステムにサイドローディングすることによって配布できます。

「[パッケージデスクトップアプリの配布](desktop-to-uwp-distribute.md)」を参照してください。
