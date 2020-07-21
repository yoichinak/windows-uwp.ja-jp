---
ms.assetid: 9FCBAF2E-5419-4169-A17C-9C4058DCF909
description: Microsoft Store は、REST Api を介して呼び出すことができるいくつかのサービスを公開しており、または組織のパートナーセンターアカウントに登録されているアプリの特定の種類のデータにプログラムでアクセスすることができます。
title: Microsoft Store サービス
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, ストア サービス
ms.localizationpriority: medium
ms.openlocfilehash: 948d9d3b4c54c41b1bb4661d5cfdf1b4d0d0849c
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493117"
---
# <a name="microsoft-store-services"></a>Microsoft Store サービス

Microsoft Store は、REST Api を介して呼び出すことができるいくつかのサービスを公開しており、または組織のパートナーセンターアカウントに登録されているアプリの特定の種類のデータにプログラムでアクセスすることができます。

## <a name="in-this-section"></a>このセクションの内容


| トピック            | 説明                 |
|------------------|-----------------------------|
| [分析データへのアクセス](access-analytics-data-using-windows-store-services.md) | *Microsoft Store ANALYTICS API*を使用して、プログラムでアプリの分析データを取得します。 この API では、アプリおよびアドオン (アプリ内製品または IAP とも呼ばれます) の入手数、アプリのエラー、アプリの評価とレビューに関するデータや、アプリ内広告とプロモーション用広告キャンペーンに関するパフォーマンス データを取得できます。 |
| [レビューに回答する](respond-to-reviews-using-windows-store-services.md) | Store のアプリのレビューにプログラムで返信するには、*Microsoft Store レビュー API* を使います。 この API は、パートナーセンターを使用せずに多数のレビューに一括応答する開発者に特に役立ちます。  |
| [広告キャンペーンの実行](run-ad-campaigns-using-windows-store-services.md) | *Microsoft Store 昇格 API*を使用して、アプリのプロモーション広告キャンペーンをプログラムで管理します。 この API を使用して、広告キャンペーンや、ターゲット設定、クリエイティブなど、その他の関連アセットを作成、更新、および監視できます。 この API は、大量のキャンペーンを作成する開発者や、パートナーセンターのダッシュボードを使用せずに実行する開発者に特に役立ちます。 |
| [申請の作成と管理](create-and-manage-submissions-using-windows-store-services.md) | *Microsoft Store 送信 API*を使用して、または組織のパートナーセンターアカウントのアプリ、アドオン、およびパッケージフライトに対してプログラムでクエリを実行し、送信を作成します。 この API は、アカウントで多数のアプリやアドオンを管理していて、それらのアセットの申請プロセスを自動化して最適化する必要がある場合に役立ちます。 |
| [対象となるオファーの管理](manage-targeted-offers-using-windows-store-services.md) | アプリでのアドオンの正常な購入に関連付けられている対象のプランをプログラムによって要求するには、 *Microsoft Store ターゲットオファー API*を使用します。 |
| [サービスによる製品の権利の管理](view-and-grant-products-from-a-service.md)  | ストアにアプリとアドオンのカタログがある場合は、 *Microsoft Store COLLECTION api*と*Microsoft Store purchase api*を使用して、本サービスからこれらの製品の所有権情報にアクセスし、ユーザーに対して使用可能な製品を報告し、無料の製品の権利をユーザーに付与することができます。  |
