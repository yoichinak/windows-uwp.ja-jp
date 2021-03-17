---
title: Rust を使用した Windows での開発の概要
description: Rust を使用した Windows での開発に興味を持っている初心者に向けた概要。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、 microsoft、 rust の学習、windows での rust (初心者向け)、vs code を使用した rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 7d3d2cbe392fe54a82af982a23b866a745e993ae
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194154"
---
# <a name="overview-of-developing-on-windows-with-rust"></a>Rust を使用した Windows での開発の概要

[Rust](https://www.rust-lang.org/) を使い始めるのは難しくありません。 Windows 10 を使用する Rust の学習に興味を持っている初心者の方は、このステップ バイ ステップ ガイドの、それぞれの詳細に従うことをお勧めします。 そこに、何をインストールするかと、どのように開発環境を設定するかが示されています。

> [!TIP]
> 既に Rust に夢中で Rust 環境が既に設定されていて、すぐにでも Windows API の呼び出しを始めたい場合は、自由に「[Windows 用 Rust と windows クレート](rust-for-windows.md)」というトピックに進むことができます。

## <a name="what-is-rust"></a>Rust とは

Rust はシステム プログラミング言語であるため、システム (オペレーティング システムなど) の記述のために使用されます。 しかし、パフォーマンスと信頼性が重要なアプリケーションに向けて使用することもできます。 Rust 言語の構文は、C++ の構文と類似しており、最新の C++ と同等のパフォーマンスを発揮します。また、経験豊富な開発者の多くにとっては、コンパイルとランタイムのモデル、型システム、および確定的な終了処理に関して、Rust は適切な言語となります。

さらに、Rust は、メモリの安全性が保証されるという裏付けを中核として設計されていて、ガベージ コレクションを必要としません。

では、Microsoft が、Windows に向けた最新の言語計画のために Rust を選択したのはなぜでしょうか。 1 つの要因は、Stack Overflow の[年間開発者向けアンケート](https://insights.stackoverflow.com/survey)で、Rust が年を追って、圧倒的に最も愛されているプログラミング言語となっていると示されていることです。 学習が軌道に乗るまで時間がかかると思われるかもしれませんが、いったん山を越えれば、気に入らないでいることが難しい言語です。

さらに、Microsoft は [Rust Foundation](https://foundation.rust-lang.org/) の設立メンバーです。 この団体は独立した非営利組織であり、大規模な、参加方式のオープン ソース エコシステムを維持し、成長させるための新しいアプローチを採用しています。

## <a name="the-pieces-of-the-rust-development-toolsetecosystem"></a>Rust 開発ツールセット/エコシステムの構成要素

このセクションでは、Rust のツールと用語の一部を紹介します。 どの説明についても、ここを参照し直して記憶を呼び起こすことができます。

* "*クレート*" は、コンパイルとリンクから成る Rust の単位です。 クレートはソース コード形式で存在することができ、そこから、バイナリ実行可能ファイル (短い呼び名は "*バイナリ*") またはバイナリ ライブラリ (短い呼び名は "*ライブラリ*") のいずれかの形式のクレートへと処理することができます。
* Rust プロジェクトは "*パッケージ*" と呼ばれます。 1 つのパッケージには、1 つ以上のクレートと共に、それらのクレートをビルドする方法を説明する `Cargo.toml` ファイルが含まれます。
* `rustup` は、Rust ツールチェーンのインストーラーかつアップデーターです。
* *Cargo* は、Rust のパッケージ管理ツールの名前です。
* `rustc` は Rust のコンパイラです。 ほとんどの場合、`rustc` は直接呼び出しません。Cargo を介して間接的に呼び出します。
* *crates.io* (`https://crates.io/`) は、Rust コミュニティのクレート レジストリです。

## <a name="setting-up-your-development-environment"></a>開発環境を設定する

次のトピックでは、[Windows 上で Rust のための開発環境を設定する](setup.md)方法について説明します。

## <a name="related"></a>関連項目

* [Rust Web サイト](https://www.rust-lang.org/)
* [Windows 用 Rust と windows クレート](rust-for-windows.md)
* [Stack Overflow の年間開発者向けアンケート](https://insights.stackoverflow.com/survey)
* [Rust Foundation](https://foundation.rust-lang.org/)
* [crates.io](https://crates.io/)
* [Windows で Rust 用の開発環境を設定する](setup.md)
