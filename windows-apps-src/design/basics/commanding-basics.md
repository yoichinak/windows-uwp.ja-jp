---
Description: ユニバーサル Windows プラットフォーム (UWP) アプリでは、コマンド要素は、ユーザーがメール送信、項目の削除、フォームの送信などのアクションを実行できる対話型の UI 要素です。
title: ユニバーサル Windows プラットフォーム (UWP) アプリのコマンド設計の基本
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796405"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP アプリのコマンド設計の基本

ユニバーサル Windows プラットフォーム (UWP) アプリでは、"*コマンド要素*" は、ユーザーがメール送信、項目の削除、フォームの送信などのアクションを実行できる対話型の UI 要素です。 "*コマンド インターフェイス*" は、共通のコマンド要素、それをホストするコマンド サーフェス、サポートされている対話、提供されているエクスペリエンスで構成されます。

## <a name="provide-the-best-command-experience"></a>最善のコマンド エクスペリエンスを提供する

コマンド インターフェイスの最も重要な側面は、ユーザーが実行できるようにすることです。 アプリの機能を計画するときは、それらのタスクを実現するために必要な手順と、有効にするユーザー エクスペリエンスを検討します。 これらのエクスペリエンスの最初のドラフトが完成した後は、それらを実装するためのツールと相互作用を決定できます。

一般的なコマンド エクスペリエンスを次に示します。

- 情報の送信または提出
- 設定とオプションの選択
- コンテンツの検索とフィルター処理
- ファイルを開く、保存する、削除する
- コンテンツの編集または作成

クリエイティブにコマンド エクスペリエンスを設計してください。 アプリでサポートする入力デバイスと、各デバイスに対するアプリでの対応方法を選択します。 最大限の範囲の機能と設定をサポートすることにより、アプリの使いやすさ、移植性、アクセシビリティが最大になります (詳しくは [Commanding design for Universal Windows Platform (UWP) apps (ユニバーサル Windows プラットフォーム (UWP) アプリ向けのコマンドのデザイン)](../controls-and-patterns/commanding.md) に関する記事を参照)。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>適切なコマンド要素を選択する

適切な要素をコマンド インターフェイスで使うことが、直感的で使いやすいアプリとなるか、使いにくくてややこしいアプリとなるかの分かれ目になります。 ユニバーサル Windows プラットフォーム (UWP) では、包括的なコマンド要素のセットを使用できます。 最も一般的な UWP のコマンド要素を次に示します。

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

全一覧については、「[コントロールと UI 要素](../controls-and-patterns/index.md)」をご覧ください。

## <a name="place-commands-on-the-right-surface"></a>適切なサーフェスへのコマンドの配置

アプリのキャンバスや、コマンド バー、コマンド バー ポップアップ、メニュー バー、ダイアログといった特殊なコマンド コンテナーなど、アプリ内の多くのサーフェスに、コマンド要素を配置できます。

常に、コンテンツに対して作用するコマンドを介してではなく、コンテンツを直接ユーザーが操作できるようにします。たとえば、リストのアイテムの並べ替えは、上下のコマンド ボタンではなく、ドラッグ アンド ドロップで行えるようにします。 

ただし、特定の入力デバイスの場合、または特定のユーザー機能や設定に対応するときは、これが不可能なことがあります。 このような場合は、できるだけ多くのコマンド アフォーダンスを提供し、これらのコマンド要素をアプリのコマンド サーフェスに配置します。

最も一般的ないくつかのコマンド サーフェスの一覧を次に示します。

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>コマンドのフィードバックを提供する 

コマンドのフィードバックでは、操作やコマンドが検出されたこと、コマンドがどのように解釈および処理されたか、コマンドが成功したかどうかを、ユーザーに伝えます。 これは、自分が実行したこと、そして次に実行できることを、ユーザーが理解するのに役立ちます。 フィードバックが UI に自然に統合されていて、ユーザーの介在が不要であるか、どうしても必要な場合以外は他の操作が不要であることが理想的です。

> [!NOTE]
> 必要なときにのみ、そして他の場所では得られない場合にだけ、フィードバックを提供します。 価値が加わる場合を除き、アプリケーションの UI は無駄がなく整然としたものに保ちます。

アプリでフィードバックを提供する方法をいくつか示します。

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">フライアウト</a>は、その外側をタップまたはクリックして閉じることができる、軽量な状況依存のポップアップです。
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> アプリで使う確認ダイアログの量に注意してください。ユーザーが間違えたときはとても役に立ちますが、ユーザーが意図的にアクションを実行しようとしているときは邪魔になります。

### <a name="when-to-confirm-or-undo-actions"></a>アクションを確認または元に戻すタイミング

アプリケーションの UI がどれほど適切に設計されていたとしても、すべてのユーザーが望んだとおりにアクションを実行できることはありません。 アクションの確認を求めたり、最近のアクションを元に戻す方法を用意したりすることにより、アプリでこのような状況に対処できます。

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>特定の入力タイプの最適化

特定の入力の種類やデバイスを中心としたユーザー エクスペリエンスの最適化について詳しくは、「[操作の基本情報](../input/index.md)」をご覧ください。

