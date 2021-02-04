---
title: Windows 10 用の Powertoy PowerRename ユーティリティ
description: ファイルの一括名前変更を行うための windows シェル拡張機能
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c751624c93fec5996885c766e73b5ab1849fd4c
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534391"
---
# <a name="powerrename-utility"></a>PowerRename ユーティリティ

PowerRename は、次のことを可能にする一括名前変更ツールです。

- 多数のファイルのファイル名を変更します *(すべてのファイルの名前を同じ名前に* 変更する必要はありません)。
- ファイル名の対象となるセクションで検索と置換を実行します。
- 複数のファイルに対して正規表現の名前変更を実行します。
- 一括名前変更を終了する前に、[予想される名前の変更] の結果をプレビューウィンドウで確認します。
- 完了後に名前変更操作を元に戻します。

## <a name="demo"></a>デモ

このデモでは、ファイル名 "Pampalona" のすべてのインスタンスが "Pamplona" に置き換えられています。 すべてのファイルには一意の名前が付けられるため、手動で1つずつ完了するまでに長い時間がかかります。 PowerRename を使用すると、1回の一括名前変更が可能になります。 [名前の変更を元に戻す] (Ctrl + Z) コマンドを使用すると、変更を元に戻すことができます。

![PowerRename のデモ](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>PowerRename メニュー

Windows エクスプローラーでいくつかのファイルを選択し、右クリックして [ *PowerRename* (powertoy で有効になっている場合のみ表示されます)] を選択すると、[PowerRename] メニューが表示されます。 選択した項目 (ファイル) の数と共に、検索と置換の値、オプションの一覧、および入力した検索結果と置換値を表示するプレビューウィンドウが表示されます。

![PowerRename Menu スクリーンショット](../images/powerrename-menu.png)

### <a name="search-for"></a>検索

テキストまたは [正規表現](https://wikipedia.org/wiki/Regular_expression) を入力して、選択したエントリに一致する条件を含むファイルを検索します。 [ *プレビュー* ] ウィンドウに一致する項目が表示されます。

### <a name="replace-with"></a>置換後の文字列

選択したファイルと一致する前に入力した値 *の検索* を置き換えるテキストを入力します。 [ *プレビュー* ] ウィンドウで、元のファイル名と名前を変更したファイルを表示できます。

### <a name="options---use-regular-expressions"></a>オプション-正規表現を使用する

オンにした場合、検索値は [正規表現](https://wikipedia.org/wiki/Regular_expression) (regex) として解釈されます。 置換値には、regex 変数を含めることもできます (以下の例を参照してください)。  このチェックボックスをオンにしない場合、検索値は、[置換] フィールドのテキストに置換されるプレーンテキストとして解釈されます。

`Use Boost library`拡張 regex 機能の [設定] メニューのオプションに関する詳細については、「[正規表現」](#regular-expressions)を参照してください。

### <a name="options---case-sensitive"></a>オプション-大文字と小文字を区別する

このチェックボックスをオンにした場合、検索フィールドで指定されたテキストは、テキストが同じ場合にのみ項目内のテキストと一致します。 既定では、大文字と小文字の一致は区別されません (大文字と小文字の違いは認識されません)。

### <a name="options---match-all-occurrences"></a>オプション-すべてのオカレンスに一致します。

オンにすると、検索フィールド内のテキストのすべての一致が置換テキストで置き換えられます。  それ以外の場合は、ファイル名に含まれるテキスト検索の最初のインスタンスのみが置換されます (左から右)。

たとえば、次のファイル名が指定されているとします。 `powertoys-powerrename.txt`

- 検索: `power`
- 名前の変更後の名前: `super`

名前が変更されたファイルの値は次のようになります。

- すべてのオカレンスに一致する (オフ): `supertoys-powerrename.txt`
- すべてのオカレンスに一致 (チェック済み): `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>オプション-ファイルを除外する

ファイルは操作に含まれません。 フォルダーだけが含められます。

### <a name="options---exclude-folders"></a>オプション-フォルダーを除外する

フォルダーは操作に含まれません。 ファイルのみが含まれます。

### <a name="options---exclude-subfolder-items"></a>オプション-サブフォルダーの項目を除外する

フォルダー内の項目は、操作に含まれません。 既定では、すべてのサブフォルダーアイテムが含まれます。

### <a name="options---enumerate-items"></a>オプション-項目の列挙

操作で変更されたファイル名に数字のサフィックスを追加します。 例: `foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>オプション-項目名のみ

操作によって変更されるのは、ファイル名の部分 (ファイル拡張子ではありません) だけです。 例: `txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>オプション-項目拡張のみ

操作によって変更されるのは、ファイル拡張子部分 (ファイル名ではありません) だけです。 例: `txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>ファイル作成日時を使用して置換

ファイルの作成日時属性は、次の表に従って変数パターンを入力することで、[ *置換後* の文字列] で使用できます。

変数パターン |説明
|:---|:---|
|`$YYYY`|使用する暦に応じて、4桁または5桁の数字で表される年。
|`$YY`|最後の2桁のみで表される年。 1桁の年には、先行ゼロが追加されます。
|`$Y`|最後の数字のみで表される年。
|`$MMMM`|月の名前
|`$MMM`|月の省略名
|`$MM`|月は、1桁の月の先頭にゼロを付けた数字として使用します。
|`$M`|月を数字として、1桁の月の先頭にゼロを付けません。
|`$DDDD`|曜日の名前
|`$DDD`|曜日の省略名
|`$DD`|1桁の日の先頭に0を付けた数字です。
|`$D`|1桁の日の先頭にゼロを付けない、月の日にち。
|`$hh`|1桁の時間に先行ゼロがある時間
|`$h`|1桁の時間に先行ゼロを付けない時間
|`$mm`|分数は、1桁の分の先頭にゼロが付きます。
|`$m`|1桁の分に先行ゼロを付けない分数。
|`$ss`|1桁の秒の先頭にゼロを付けた秒数。
|`$s`|1桁の秒の先頭にゼロを付けない秒数。
|`$fff`|最大3桁で表されるミリ秒数。
|`$ff`|最初の2桁だけで表されるミリ秒数。
|`$f`|1番目の桁だけで表されるミリ秒数。

たとえば、次のようなファイル名を指定します。

- `powertoys.png`11/02/2020 に作成されました
- `powertoys-menu.png`11/03/2020 に作成されました

項目の名前を変更するための条件を入力してください:

- 検索: `powertoys`
- 名前の変更後の名前: `$MMM-$DD-$YY-powertoys`

名前が変更されたファイルの値は次のようになります。

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>[正規表現]

ほとんどのユースケースでは、単純な検索と置換で十分です。 ただし、複雑な名前変更タスクによって、より多くの制御が必要になる場合もあります。 [正規表現](https://wikipedia.org/wiki/Regular_expression) は役に立ちます。

正規表現は、テキストの検索パターンを定義します。 これらの文字列を使用して、テキストの検索、編集、操作を行うことができます。 正規表現で定義されているパターンは、特定の文字列に対して1回、複数回、またはまったく一致しない場合があります。 PowerRename は [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 文法を使用します。これは、最新のプログラミング言語で共通です。

正規表現を有効にするには、[正規表現を使用する] チェックボックスをオンにします。

**注:** 正規表現を使用しているときに、"すべての出現箇所を照合" を確認することが必要になる場合があります。

標準ライブラリではなく [ブーストライブラリ](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html) を使用するには、[powertoy] 設定のオプションをオンにし `Use Boost library` ます。 これにより、標準ライブラリでサポートされていないなどの拡張機能が有効になり `[lookbehind](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html#boost_regex.syntax.perl_syntax.lookbehind)` ます。

### <a name="examples-of-regular-expressions"></a>正規表現の例

#### <a name="simple-matching-examples"></a>単純一致の例

| 検索       | [説明]                                           |
| ---------------- | ------------- |
| `^`              | ファイル名の先頭と一致します                   |
| `$`              | ファイル名の末尾と一致します                         |
| `.*`             | 名前に含まれるすべてのテキストと一致します。                        |
| `^foo`           | "Foo" で始まるテキストを検索する                     |
| `bar$`           | "Bar" で終わるテキストを検索する                       |
| `^foo.*bar$`     | "Foo" で始まり、"bar" で終わるテキストを検索します |
| `.+?(?=bar)`     | すべてを "バー" に合わせる                          |
| `foo[\s\S]*bar`  | "Foo" と "bar" の間のすべてに一致              |

#### <a name="matching-and-variable-examples"></a>一致と変数の例

*変数を使用する場合は、[すべてのオカレンスに一致] オプションを有効にする必要があります。*

| 検索   | 置換後の文字列    | [説明]                                |
| ------------ | --------------- |--------------------------------------------|
| `(.*).png`   | `foo_$1.png`   | \_既存のファイル名に "foo" を付加する |
| `(.*).png`   | `$1_foo.png`   | \_既存のファイル名に "foo" を追加します。  |
| `(.*)`       | `$1.txt`        | 既存のファイル名に ".txt" 拡張子を追加します。 |
| `(^\w+\.$)|(^\w+$)` | `$2.txt` | 拡張子がない場合にのみ、既存のファイル名に ".txt" 拡張子を追加します。 |
|  `(\d\d)-(\d\d)-(\d\d\d\d)` | `$3-$2-$1` | ファイル名 "29-03-2020" の数値の移動が "2020-03-29" になります。 |

### <a name="additional-resources-for-learning-regular-expressions"></a>正規表現の学習に関するその他のリソース

オンラインで利用できる優れたサンプル/カンニングシートがあります。

[正規表現によるチュートリアル—例による簡単な表](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[ECMAScript 正規表現のチュートリアル](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>ファイルリストフィルター

PowerRename でフィルターを使用して、名前変更の結果を絞り込むことができます。 予想される結果を確認するには、 *プレビュー* ウィンドウを使用します。 フィルターを切り替える列ヘッダーを選択します。

- [**元**]:*プレビュー* ウィンドウの最初の列は次のように切り替わります。
  - Checked: ファイルの名前が変更されます。
  - Unchecked: ファイルは、検索条件に入力された値に適合していても、名前を変更するように選択されていません。

- **名前を変更** すると、 *プレビュー* ウィンドウの2番目の列を切り替えることができます。
  - 既定のプレビューでは、選択したすべてのファイルが表示され、更新された名前の値を表示する *検索条件に* 一致するファイルのみが表示されます。
  - 名前を *変更* したヘッダーを選択すると、プレビューが切り替わり、名前が変更されるファイルのみが表示されます。 元の選択項目から選択した他のファイルは表示されません。

![Powertoy PowerRename Filter デモ](../images/powerrename-demo2.gif)
