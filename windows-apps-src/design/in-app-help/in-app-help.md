---
description: ユーザーのヘルプを表示する既定の方法として、アプリ内のアプリケーション内ヘルプを使用する方法、およびアプリ内のヘルプの種類について説明します。
title: アプリ内ヘルプの設計のためのガイドライン。
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: d7144a72dd3af1c9c902e0dfd401799f50ea55b0
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054172"
---
# <a name="in-app-help-pages"></a>アプリ内ヘルプのページ

多くの場合、ヘルプはアプリケーション内でユーザーがヘルプの表示を選択したときに表示されることが望ましい方法です。

## <a name="when-to-use-in-app-help-pages"></a>アプリ内ヘルプのページを使用する状況

アプリ内ヘルプは、ユーザーにヘルプを表示する既定の方法として使用します。 これはすべてのヘルプで使用し、シンプルで単純にして、ユーザーに未知のコンテンツを表示しないようにします。 使用方法、アドバイス、ヒントやコツなどはすべて、アプリ内ヘルプに適しています。

複雑な使用方法やチュートリアルはすばやい参照には適しておらず、また大量の領域を占有します。 そのため、それらはアプリ自体には組み込まず、外部で提供するようにします。

Users should not have to seek out help for basic instructions or to discover new features. ユーザーのためになる情報を提供する場合には、説明 UI を使用します。

## <a name="types-of-in-app-help"></a>アプリ内ヘルプの種類

アプリ内ヘルプにはいくつかの形式がありますが、それらはすべて同じ設計と操作性の原則に従っています。

#### <a name="help-pages"></a>ヘルプ ページ

アプリ内に別のヘルプ ページを作成することは、役に立つ使用方法を表示するための、すばやく手軽な方法です。

-   **Be concise:** A large library of help topics is unwieldy and unsuited for in-app help.
-   **一貫性を持たせる:** アプリ内のどこからでも、同じ方法でヘルプ ページを表示できるようにします。 ユーザーがヘルプを探し回る必要がないようにします。
-   **Users scan, not read:** Because the help a user is looking for might be on the same page as other help topics, make sure they can easily tell which one they need to focus on.


#### <a name="popups"></a>ポップアップ

ポップアップを使うと、高度なコンテキスト ヘルプを実現でき、ユーザーが実行している具体的なタスクに直接役立つ手順やアドバイスを表示できます。

-   **1 つの問題に集中する:** ポップアップではヘルプ ページよりも、スペースがさらに制限されます。 ヘルプ ポップアップを効果的にするには、1 つの具体的なタスクのみに関して説明するようにします。
-   **Visibility is important:** Because help popups can only be viewed from one location, make sure that they're clearly visible to the user without being obstructive. If the user misses it, they might move away from the popup in search of a help page.
-   **Don't use too many resources:** Help shouldn't lag or be slow-loading. Using videos or audio files or high resolution images in popups is more likely to frustrate the user than it is to help them.

#### <a name="descriptions"></a>説明

Sometimes, it can be useful to provide more information about a feature when a user inspects it. 説明は、説明 UI に似ていますが、説明 UI はユーザーが知らない機能についてユーザーに教えようとするものであり、詳細な説明はユーザーが既に関心を持っているアプリの機能についてのユーザーの理解を深めるためのものであることが、主な相違点です。

-   **Don't teach the basics:** Assume that the user already knows the fundamentals of how to use the item being described. Clarifying or offering further information is useful. Telling them what they already know is not.
-   **興味を引く操作について説明します:** 説明の最適な使用方法の 1 つは、ユーザーが既に知っている機能の操作の方法を説明することです。 This helps users learn more about things they already like to use.
-   **Stay out of the way:** Much like instructional UI, descriptions need to avoid interfering with a user's enjoyment of the app.

## <a name="related-articles"></a>関連記事

* [アプリのヘルプのガイドライン](guidelines-for-app-help.md)
