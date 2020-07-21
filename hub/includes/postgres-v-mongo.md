---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314876"
---
データベース システムの 2 つの[一般的な選択肢](https://insights.stackoverflow.com/survey/2019#technology-_-databases)は、[MongoDB](https://www.mongodb.com/what-is-mongodb) と [PostgreSQL](https://www.postgresql.org/about/) です。 

MongoDB は、JSON で動作し、スキーマフリーのデータを格納するように設計された NoSQL ドキュメントデータベースです。 柔軟性と非構造化データ、リアルタイム分析のキャッシュ、および水平方向の規模拡張に適しています。 

PostgreSQL (Postgres とも呼ばれる) は、拡張性と標準への準拠を重視した SQL リレーショナル データベースです。 現在は JSON も処理できますが、一般的には、構造化データ、垂直方向の規模拡張、および、e コマースや金融取引などの ACID 準拠のニーズに適しています。

スキーマ:

**PostgreSQL:** テーブル | 列 | 値 | レコード。

**MongoDB (NoSQL):** コレクション | キー | 値 | ドキュメント。

選択するデータベースの種類は、データベースを使用するアプリケーションの種類に応じて決める必要があります。 構造化データベースと非構造化データベースの長所と短所を検討し、ユースケースに基づいて選択することをお勧めします。 PostgreSQL や MongoDB 以外にも検討すべきデータベース システムがいくつかあります。