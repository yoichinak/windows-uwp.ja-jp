---
description: 世界中のユーザーに対してアプリを包括的にしてアクセシビリティ対応にする方法について説明します。
keywords: UWP アプリのアクセシビリティ, グローバリゼーション, インクルーシブ デザイン アプリ, アクセシビリティ アプリの要件
title: UWP アプリの操作性 - Windows アプリ開発
layout: LandingPage
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: a6912932b7ad71fd3d04c038eab7e0aa4dd6cb11
ms.sourcegitcommit: 2fa2d2236870eaabc95941a95fd4e358d3668c0c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076388"
---
# <a name="usability-for-uwp-apps"></a>UWP アプリの操作性

少しの配慮で細かい点に注意するだけで、単に優れたユーザー エクスペリエンスから、世界中のユーザーのニーズを満たす真に包括的なユーザー エクスペリエンスに変えることができます。

このセクションに示す設計とコーディングの手順に従って、アクセシビリティ機能の追加、グローバリゼーションとローカライズの有効化、ユーザーによるエクスペリエンスのカスタマイズの有効化、必要に応じたヘルプの提供を行うと、UWP アプリをより包括的なアプリにすることができます。

## <a name="accessibility"></a>アクセシビリティ

アクセシビリティの目的は、従来のユーザー インターフェイスの使用に支障があるユーザーにとってアプリを使いやすいものにすることです。 状況によってはアクセシビリティの要件が法律で定められているものもありますが、 できるだけ多くの人にアプリを使ってもらえるように、法的要件に関係なくアクセシビリティの問題に対処することをお勧めします。

[アクセシビリティ ポータル](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>アプリの設定

アプリの設定を利用することで、ユーザーがアプリをカスタマイズしたり、個人のニーズや好みに合わせてアプリを最適化したりすることができます。 適切な設定を提供し、適切に保存することで、優れたユーザー エクスペリエンスをさらに向上させることができます。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">ガイドライン</a></b><br/>アプリ設定を作成し表示する際のベスト プラクティス。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">アプリ データの保存と取得</a></b><br/>ローカル アプリ データ、ローミング アプリ データ、一時アプリ データの保存方法と取得方法。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="globalization-and-localization"></a>グローバリゼーションとローカライズ

Windows は世界中で利用されており、言語、地域、文化の異なる多様なユーザーを対象としています。 アプリはさまざまな言語を使用するユーザーによって、さまざまな国や地域で使用されます。 中には複数の言語を話すユーザーもいます。 そのため、アプリが実行される構成には、さまざまな言語、地域、文化システムの組み合わせが関係します。 *グローバリゼーション*と*ローカライズ*によってアプリの適応性を高めることにより、潜在的な市場を拡大することができます

<a href="../globalizing/globalizing-portal.md">グローバリゼーションとローカライズ ポータル</a>

## <a name="in-app-help"></a>アプリ内ヘルプ
アプリの設計がどれほど優れていても、ユーザーはヘルプを必要とする場合があります。

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">アプリのヘルプのガイドライン</a></b><br/>アプリケーションが複雑な場合には、ユーザーに効果的なヘルプを提供することにより、ユーザー エクスペリエンスを大幅に改善できます。
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">説明 UI</a></b><br/>特定のタッチ操作など、ユーザーにわかりにくいアプリの機能をユーザーに伝えると便利な場合があります。 このような場合は、わかりにくいと思われる機能をユーザーが検出し使用できるように、UI を使ってユーザーに指示を示す必要があります。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">アプリ内ヘルプ</a></b><br/>多くの場合、アプリケーション内でユーザーがヘルプの表示を選んだときに、ヘルプが表示されるのが適切な方法です。 アプリ内ヘルプを作成するときは、次のガイドラインを考慮してください。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">外部のヘルプ</a></b><br/>多くの場合、アプリケーション内でユーザーがヘルプの表示を選んだときに、ヘルプが表示されるのが適切な方法です。 アプリ内ヘルプを作成するときは、次のガイドラインを考慮してください。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul>

