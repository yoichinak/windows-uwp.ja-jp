---
title: UWP アプリでの MySQL データベースの使用
description: UWP アプリで MySQL データベースを使用します。
ms.date: 3/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, データベース
ms.localizationpriority: medium
ms.openlocfilehash: a7708ca082647aef6bbf2261922d2ebd6723923e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785483"
---
# <a name="use-a-mysql-database"></a>MySQL データベースの使用
この記事には、UWP アプリからの MySQL データベースの使用を有効にするのに必要な手順が含まれます。 これには、コードでどのようにデータベースとやりとりできるかを示す小さなコード スニペットも含まれます。

## <a name="set-up-your-solution"></a>ソリューションを設定する

アプリを MySQL データベースに直接接続するために、プロジェクトの最小バージョンが確実に Fall Creators Update (ビルド 16299) を対象にするようにします。  UWP プロジェクトのプロパティ ページにその情報があります。

![Fall Creators Update に設定されたターゲット バージョンと最小バージョンを示す Visual Studio のターゲット設定プロパティ ウィンドウの画像](images/min-version-fall-creators.png)

**パッケージ マネージャー コンソール**を開きます ([表示] -> [その他のウィンドウ] -> [パッケージ マネージャー コンソール])。 **Install-Package MySql.Data** コマンドを使用して、MySQL 用のドライバーをインストールします。 これにより、プログラムを使用して MySQL データベースにアクセスできるようになります。

## <a name="test-your-connection-using-sample-code"></a>サンプル コードを使用して接続をテストする
リモートの MySQL データベースからの接続および読み取りの例は、次のとおりです。 IP アドレス、認証情報、データベース名はカスタマイズする必要があることに注意してください。

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```