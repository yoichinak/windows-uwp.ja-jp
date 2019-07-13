---
title: UWP アプリでの MongoDB データベースの使用
description: UWP アプリで MongoDB データベースを使用します。
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, データベース
ms.localizationpriority: medium
ms.openlocfilehash: a104993d77828a6383eed39a4ab8c12852533894
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785235"
---
# <a name="use-a-mongodb-database"></a>MongoDB データベースの使用
この記事には、UWP アプリからの MongoDB データベースの使用を有効にするのに必要な手順が含まれます。 これには、コードでどのようにデータベースとやりとりできるかを示す小さなコード スニペットも含まれます。

## <a name="set-up-your-solution"></a>ソリューションを設定する

アプリを MongoDB データベースに直接接続するために、プロジェクトの最小バージョンが確実に Fall Creators Update (ビルド 16299) を対象にするようにします。  UWP プロジェクトのプロパティ ページにその情報があります。

![Fall Creators Update に設定されたターゲット バージョンと最小バージョンを示す Visual Studio のターゲット設定プロパティ ウィンドウの画像](images/min-version-fall-creators.png)

**パッケージ マネージャー コンソール**を開きます ([表示] -> [その他のウィンドウ] -> [パッケージ マネージャー コンソール])。 **Install-Package MongoDB.Driver** コマンドを使用して、MongoDB 用のドライバーをインストールします。 これにより、プログラムを使用して MongoDB データベースにアクセスできるようになります。

## <a name="test-your-connection-using-sample-code"></a>サンプル コードを使用して接続をテストする
次のサンプル コードでは、リモートの MongoDB クライアントからコレクションを取得して、新しいドキュメントをそのコレクションに追加します。 その後、ここでは MongoDB API を使用して、コレクションの新しいサイズと挿入されたドキュメントを取得し、それらを出力します。IP アドレスとデータベース名はカスタマイズする必要があることに注意してください。

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```