---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: データ アクセス
description: このセクションでは、デバイス上のデータをプライベート データベースに保存する方法と、ユニバーサル Windows プラットフォーム (UWP) アプリでオブジェクト リレーショナル マッピングを使う方法について説明します。
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, データ, データベース, リレーショナル, テーブル, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: d324c5b17e167b841761b92507eec2134e4b7009
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173646"
---
# <a name="data-access"></a>データ アクセス

SQLite データベースを使用することで、ユーザーのデバイスにデータを格納できます。 また、いずれのサービス レイヤーを使用しなくても、SQL Server データベースにアプリを直接接続できます。

| トピック | 説明|
|-------|------------|
| [UWP アプリでの SQLite データベースの使用](sqlite-databases.md) | SQLite を使用して、ユーザー デバイス上の軽量なデータベースにデータを保存し、取得する方法を示します。 SQLite は、サーバーを使わない埋め込みデータベース エンジンです。 |
| [UWP アプリでの SQL Server データベースの使用](sql-server-databases.md) | [System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 名前空間のクラスを使用して、SQL Server データベースに直接接続し、データを保存および取得する方法を示します。 必要なサービス レイヤーはありません。 |

## <a name="related-topics"></a>関連トピック

* [顧客注文データベースのサンプル](https://github.com/Microsoft/Windows-appsample-customers-orders-database)