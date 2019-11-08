---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314876"
---
データベースシステムには、 [MongoDB](https://www.mongodb.com/what-is-mongodb)と[PostgreSQL](https://www.postgresql.org/about/)の2つの[一般的な選択肢](https://insights.stackoverflow.com/survey/2019#technology-_-databases)があります。 

MongoDB は、JSON で使用できるように設計された NoSQL ドキュメントデータベースであり、スキーマフリーのデータを格納します。 柔軟性と非構造化データに適しており、リアルタイム分析をキャッシュし、水平方向のスケーリングを行うことができます。 

PostgreSQL (Postgres とも呼ばれる) は、拡張性と標準への準拠を重視した SQL リレーショナルデータベースです。 JSON は今でも処理できますが、一般に、構造化データ、垂直方向のスケーリング、およびコマースや金融取引などの ACID 準拠のニーズに適しています。

スキーマ

**PostgreSQL**Table |Column |値 |レコード.

**MongoDB (NoSQL):** Collection |キー |値 |ガイド.

選択するデータベースの種類は、データベースを使用するアプリケーションの種類によって異なります。 構造化データベースと非構造化データベースの長所と短所を検討し、ユースケースに基づいて選択することをお勧めします。 PostgreSQL と MongoDB 以外にも考慮すべきデータベースシステムがいくつかあります。