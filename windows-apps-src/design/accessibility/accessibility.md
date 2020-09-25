---
Description: Windows アプリに関連するアクセシビリティの概念について説明します。
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: ユーザー補助
label: Accessibility
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67f98db338d201dd4ef80c7b5f265aba3f6fbfd4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216375"
---
# <a name="accessibility"></a>ユーザー補助  

ユーザー補助機能は、さまざまな環境でテクノロジを使用するユーザーが Windows アプリケーションを使用できるようにし、さまざまなニーズやエクスペリエンスで UI にアプローチできるようにするためのエクスペリエンスの構築に関するものです。 状況によってはアクセシビリティの要件が法律で定められているものもありますが、 できるだけ多くの人にアプリを使ってもらえるように、法的要件に関係なくアクセシビリティの問題に対処することをお勧めします。

> アプリのアクセシビリティに関する Microsoft Store 宣言もあります。

| [アーティクル] | 説明 |
|---------|-------------|
| [アクセシビリティの概要](accessibility-overview.md) | この記事では、Windows アプリのユーザー補助のシナリオに関連する概念とテクノロジの概要について説明します。 |
| [包括的なソフトウェアの設計](designing-inclusive-software.md) | Windows 10 用 Windows アプリを使用した包括的設計の進化について説明します。  アクセシビリティを考慮して、包括的なソフトウェアを設計、構築します。 |
| [包括的な Windows アプリの開発](developing-inclusive-windows-apps.md) | この記事は、ユーザー補助 Windows アプリを開発するためのロードマップです。 |
| [アクセシビリティ テスト](accessibility-testing.md) | Windows アプリにアクセスできるようにするための手順をテストします。 |
| [ストア内のアクセシビリティ](accessibility-in-the-store.md) | Microsoft Store で Windows アプリをアクセス可能として宣言するための要件について説明します。 |
| [アクセシビリティのチェック リスト](accessibility-checklist.md) | Windows アプリにアクセスできるかどうかを確認するためのチェックリストを提供します。 |
| [基本的なアクセシビリティ情報の開示](basic-accessibility-information.md) | 基本的なアクセシビリティ情報は、多くの場合、名前、役割、値に分類されます。 このトピックでは、支援技術が必要とする基本情報をアプリで公開するのに役立つコードについて説明します。 |
| [キーボードアクセシビリティ](keyboard-accessibility.md) | アプリに十分なキーボード操作機能が備わっていない場合、視覚障碍や運動障碍のあるユーザーはアプリをうまく使うことができなかったり、まったく使うことができない可能性があります。 |
| [スクリーン リーダーとハードウェア システムのボタン](system-button-narration.md) | [ナレーター](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator)などのスクリーンリーダーは、ハードウェアシステムのボタンイベントを認識して処理し、その状態をユーザーに伝えることができる必要があります。 場合によっては、スクリーンリーダーがボタンイベントを排他的に処理し、他のハンドラーにバブルアップさせないようにすることが必要になることがあります。 |
| [ランドマークと見出し](landmarks-and-headings.md) | ランドマークと見出しは、ユーザー インターフェイスのセクションを定義し、スクリーン リーダーなどのアクセシビリティ対応技術のユーザーの効率的なナビゲーションに役立ちます。 |
| [ハイ コントラスト テーマ](high-contrast-themes.md) | ハイコントラストテーマがアクティブな場合に Windows アプリを使用できるようにするために必要な手順について説明します。 |
| [アクセシビリティに対応したテキストの要件](accessible-text-requirements.md) | このトピックでは、色と背景のコントラスト比を適切な値にすることで、アプリのテキストをアクセシビリティ対応にするためのベスト プラクティスについて説明します。 このトピックでは、Windows アプリのテキスト要素が持つことができる Microsoft UI オートメーションロール、およびグラフィックスのテキストのベストプラクティスについても説明します。 |
| [アクセシビリティ対応にするために避ける事項](practices-to-avoid.md) | アクセス可能な Windows アプリを作成する場合に回避する方法を示します。 |
| [カスタム オートメーション ピア](custom-automation-peers.md) | UI オートメーションに対するオートメーション ピアの概念について説明します。また、独自のカスタム UI クラスに対してオートメーションのサポートを提供する方法についても説明します。 |
| [コントロール パターンとインターフェイス](control-patterns-and-interfaces.md) | Microsoft UI オートメーションのコントロール パターン、それらにアクセスするためにクライアントが使うクラス、それらを実装するためにプロバイダーが使うインターフェイスを紹介します。 |

## <a name="related-topics"></a>関連トピック  
* [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation) 
* [ナレーターの概要](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
