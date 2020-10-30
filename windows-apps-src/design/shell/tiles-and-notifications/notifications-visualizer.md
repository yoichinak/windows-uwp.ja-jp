---
description: Notifications Visualizer は、Microsoft Store の新しい Windows アプリで、開発者が Windows 10 のアダプティブ ライブ タイルをデザインする際に役立ちます。
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55ab7e22dc011555a8d98fd863b7dd9ff67de5fa
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033675"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


通知ビジュアライザーは、開発者が Windows 10 のアダプティブライブタイルと対話型トースト通知をデザインするのに役立つ、 [ストア](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) の新しい Windows アプリです。


## <a name="overview"></a>概要

Notifications Visualizer では、Visual Studio の XAML エディター/デザイン ビューと同様、XML ペイロードを編集すると、タイルとトースト通知の視覚的なプレビューが即座に表示されます。 このアプリではエラーのチェックも行われます。これにより、有効なタイルまたはトースト通知のペイロードを作成できます。

次のアプリのスクリーンショットは、XML ペイロードと、各サイズのタイルが選んだデバイス上でどのように表示されるかを示しています。

![コードとタイルが示されている Notifications Visualizer アプリのエディター](images/notif-visualizer-001.png)

 

Notifications Visualizer を使うと、アプリ自体を編集、展開しなくても、アダプティブ タイルとトーストのペイロードを作成してテストすることができます。 適切な表示のペイロードが完成したら、そのペイロードをアプリに統合できます。 詳しくは、「[ローカル タイル通知の送信](sending-a-local-tile-notification.md)」および 「[ローカル トースト通知の送信](send-local-toast.md)」をご覧ください。

**注**   Notifications Visualizer では Windows のスタート メニューとトースト通知をシミュレートできますが、完全に正確なシミュレーションが常に行われるというわけではありません。また、このシミュレーションでは、一部の高度なペイロード プロパティがサポートされていません。 タイルやトーストを適切にデザインしたら、意図したとおりに表示されることを確認するために、タイルをスタート メニューにピン留めしたり、トーストを表示したりしてテストします。

 

## <a name="features"></a>特徴

Notifications Visualizer にはさまざまなサンプル ペイロードが付属しており、アダプティブ ライブ タイルや対話型トーストの機能を実際に確認して、作成を開始する際の参考にすることができます。 さまざまなテキスト オプション、グループ/サブグループ、背景画像を試すことができます。また、さまざまなデバイスや画面にタイルがどのように対応するかを確認することもできます。 サンプルに変更を加えたら、今後利用するために、更新したペイロードをファイルに保存できます。

エディターでは、リアルタイムにエラーと警告が示されます。 たとえば、アプリのペイロードが 5 KB (プラットフォームの上限) より大きい場合、上限を超えていることを示す警告が Notifications Visualizer によって表示されます。 Notifications Visualizer では誤った属性名や値に関する警告も示され、視覚的な問題をデバッグする際に役立ちます。

表示名、色、ロゴ、ShowName、バッジの値などのタイルのプロパティを制御することができます。 これらのオプションを使うと、タイルのプロパティとタイル通知のペイロードがどのように影響するのか、およびこれらのオプションによって生成される結果をすぐに理解できます。

次のアプリのスクリーンショットは、タイル エディターを示しています。

![タイルが示されている Notifications Visualizer のエディター](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>関連トピック

* [ストアでの Notifications Visualizer の入手](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [アダプティブ タイルの作成](create-adaptive-tiles.md)
* [対話型トースト](adaptive-interactive-toasts.md)
