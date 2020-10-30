---
description: アプリの認定の妨げになることが多い問題、またはアプリの公開後のスポット チェックで識別された問題を回避するために、このリストを確認します。
title: 一般的な認定エラーの回避
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 672da214582fb6b206d7e16e1e776be40caeec90
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031225"
---
# <a name="avoid-common-certification-failures"></a>一般的な認定エラーの回避


アプリの認定の妨げになることが多い問題、またはアプリの公開後のスポット チェックで識別された問題を回避するために、このリストを確認します。

> [!NOTE]
> [Microsoft Store ポリシー](store-policies.md)を確認して、アプリがそこに記載されているすべての要件を満たしていることを確認してください。

-   アプリを申請するのは、アプリが完成した場合だけにします。 アプリの説明を使って今後の機能を言及することをお勧めしますが、不完全なセクションや、作成中の Web ページへのリンクなど、アプリが不完全であるという印象をユーザーに与えるものをアプリに含めないようにしてください。

-   アプリを提出する前に、[Windows アプリ認定キットでアプリをテスト](../debug-test-perf/windows-app-certification-kit.md)します。

-   できるだけアプリの安定性が高くなるように、複数の異なる構成でアプリをテストします。

-   ネットワークに接続できない状況でもアプリがクラッシュしないことを確認します。 実際に使うためには接続を必要とするアプリであっても、接続が存在しない状態で正常に動作する必要があります。

-   テスト アカウントのユーザー名とパスワード (ユーザーがサービスにログインする必要のあるアプリの場合) や、非表示の機能やロックされている機能へのアクセスに必要な手順など、アプリを使うために [必要な情報を提供](notes-for-certification.md) してください。

-   アプリに必要な場合は、 [プライバシーポリシーの URL](enter-app-properties.md#privacy-policy-url) を含めます。たとえば、アプリが何らかの方法で任意の種類の個人情報にアクセスする場合や、法律によって要求される場合などです。 アプリにプライバシーポリシーが必要かどうかを判断するには、 [アプリ開発者契約](/legal/windows/agreements/app-developer-agreement) と [Microsoft Store ポリシー](store-policies.md)を確認します。

-   アプリの内容を明確に表せるように、アプリの説明はできるだけ詳しく記載します。 ヘルプが必要な場合は、[アプリに関する優れた説明を記載する](write-a-great-app-description.md) ためのガイダンスをご覧ください。

-   [年齢区分](age-ratings.md)のセクションのすべての質問に正確で完全な回答を入力してください。

-   アクセシビリティのシナリオを想定して具体的な設計とテストを行っていない限り、[アプリをアクセシビリティ対応として宣言](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines)しないでください。

-   アプリが [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) 名前空間から商取引 API を使う場合、アプリを必ずテストして、一般的な例外が処理されることを確認します。 また、アプリがテスト用途のみの [**CurrentAppSimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) クラスではなく、 [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラスを使っていることも確認してください。 (アプリが Windows 10 バージョン 1607 以降のバージョンをターゲットにする場合は、Windows.ApplicationModel.Store 名前空間ではなく、[Windows.Services.Store](/uwp/api/windows.services.store) 名前空間を使用することをお勧めします)。


 

 
