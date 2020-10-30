---
description: IME (Input Method Editor) は、標準の QWERTY キーボードでは簡単に表現できない言語のテキストをユーザーが入力できるようにするソフトウェアコンポーネントです。
title: 入力方式エディター (IME)
label: Input Method Editors (IME)
template: detail.hbs
keywords: ime、Input Method Editor、入力、対話
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1f70f08e61df1b609a0ab505e35ef314a90a91fd
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030085"
---
# <a name="input-method-editors-ime"></a>入力方式エディター (IME)

IME (Input Method Editor) は、標準の QWERTY キーボードでは簡単に表現できない言語のテキストをユーザーが入力できるようにするソフトウェアコンポーネントです。 これは通常、さまざまな東アジア言語など、ユーザーが記述した言語の文字数に起因します。

1つのキーボードキーに表示される各文字の代わりに、ユーザーは、IME によって解釈されるキーの組み合わせを入力します。 IME は、キーストロークのセットに一致する文字か、選択する候補文字のリストを生成します。 選択された文字が、ユーザーが操作している編集コントロールに挿入されます。

> [!NOTE]
> Ime は、ハードウェアキーボードとスクリーンキーボードまたはタッチキーボードの両方をサポートします。

アプリで IME を直接操作する必要はありません。 タッチキーボードと同じように、IME がシステムに組み込まれています。 アプリにテキスト入力があり、IME が必要な言語でテキスト入力をサポートする場合は、テキスト入力のエンドツーエンドのカスタマーエクスペリエンスをテストする必要があります。 これにより、タッチキーボードや IME の候補ウィンドウによって occluded されないように UI を調整するなど、問題を修正できます。

## <a name="creating-an-ime"></a>IME の作成

すべてのユーザーにとって優れた入力エクスペリエンスを実現するために、Microsoft では、さまざまな言語で同梱されている Ime を提供しています。

インボックス ime に加えて、ユーザーがインボックス IME と同様にインストールして使用できる独自のカスタム Ime を作成することもできます。

すべての Ime は Windows システムで実行されます。これは、悪意のある Ime を停止し、すべての Ime のセキュリティとユーザーエクスペリエンスを向上させるために強化されています。

カスタム Ime は、既定のタッチキーボードにリンクし、そのレイアウトを使用して、エンドユーザーがタッチキーボードで IME を使用できるようにします。 ただし、独自の独立したタッチキーボードを指定することはできません。また、タッチキーボード用のインボックス Ime の特定の機能をカスタム Ime で使用することはできません。

## <a name="requirements-for-imes"></a>Ime の要件

サードパーティの IME は、次の要件を満たしている必要があります。

- デジタル署名が必要
- 適切な IME フラグが正しく設定されている場合は、 [テキストサービスフレームワーク (TSF)](/windows/win32/tsf/text-services-framework) を認識している必要があります。
- 「 [Input Method Editor (IME) の要件](input-method-editor-requirements.md)」に記載されているガイドラインに従って、 [Windows アプリの設計とコーディング](../index.md)を行う必要があります。

これらの要件を満たしていないサードパーティの IME は、実行がブロックされます。

> [!NOTE]
> レガシカスタム Ime はデスクトップアプリで実行できますが、Windows アプリではブロックされます。

また、Windows Defender は、システムから悪意のある Ime を削除します。 このため、IME コーディングの要件について理解しておくことが重要です。 詳細については、「 [ime (Input Method Editor) の要件](input-method-editor-requirements.md)」を参照してください。

## <a name="design-guidelines-for-imes"></a>Ime の設計ガイドライン

Ime のベストプラクティスと設計ガイドラインの詳細については、「 [ime (Input Method Editor) の要件](input-method-editor-requirements.md) 」を参照してください。 一般に、すべての IME Ui で次のことを行う必要があります。

- Windows ランタイムアプリの UX ガイドラインに従う
- モーダルエクスペリエンスを回避し、必要なときにのみ IME ウィンドウを表示する
- 黒と白のみのアイコンを含める

## <a name="related-topics"></a>関連トピック

- [IME (Input Method Editor) の要件](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [Itfthreadmグリーン x:: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView:: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
