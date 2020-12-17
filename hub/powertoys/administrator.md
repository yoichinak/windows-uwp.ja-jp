---
title: Windows 10 用の Powertoy 管理者モード
description: Powertoy が管理者特権で実行されるアプリで動作するには、管理者モードでも Powertoy を実行する必要があります。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618541"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>管理者特権のアクセス許可で実行されている Powertoy

管理者として (昇格されたアクセス許可とも呼ばれる) アプリケーションを実行している場合は、管理者特権のあるアプリケーションがフォーカスを持っているとき、または Fare Yzones などの Powertoy 機能を操作しようとすると、Powertoy が正しく機能しない可能性があります。 これは、管理者として Powertoy も実行することによって対処できます。

## <a name="options"></a>Options

管理者として実行されているアプリケーションをサポートする Powertoy には、次の2つのオプションがあります (高度なアクセス許可を持つ)。

- **[推奨]**: powertoy は、昇格されたプロセスが検出されると、プロンプトを表示します。 **Powertoy 設定** を開きます。 [ **全般** ] タブで、[管理者として再起動] を選択します。

- [ **Powertoy] 設定** の [常に管理者として実行する] を有効にします。

## <a name="run-as-administrator-elevated-processes-explained"></a>管理者特権で実行されるプロセスの説明

既定では、Windows アプリケーションは **ユーザーモード** で実行されます。 アプリケーションを **管理モード** で実行するか、昇格された *プロセス* として実行するために、アプリはオペレーティングシステムへの追加のアクセスで実行されます。

管理モードでアプリまたはプログラムを実行する最も簡単な方法は、プログラムを右クリックし、[ **管理者として実行**] を選択することです。 現在のユーザーが管理者でない場合、Windows は管理者のユーザー名とパスワードを要求します。

ほとんどのアプリは、昇格されたアクセス許可で実行する必要はありません。 ただし、管理者権限を必要とする一般的なシナリオでは、特定の PowerShell コマンドを実行したり、レジストリを編集したりすることができます。

このプロンプト (ユーザー Access Control プロンプト) が表示された場合、アプリケーションは管理者レベルのアクセス許可を要求しています。

![Windows の管理者特権でのアクセス許可のプロンプトのスクリーンショット](../images/pt-admin-prompt.png)

管理者特権のコマンドラインの場合、タイトルバーには通常、タイトル "Administrator" が付加されます。

![Windows 管理者コマンドラインのスクリーンショット](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>Powertoy を使用した管理モードのサポート

Powertoy には、管理者モードで実行されている他のアプリケーションと対話する場合にのみ、管理者特権が必要です。 これらのアプリケーションにフォーカスがある場合、Powertoy は昇格されない限り機能しない可能性があります。

次の2つのシナリオでは機能しません。

- 特定の種類のキーボードストロークを遮断する
- ウィンドウのサイズ変更/移動

### <a name="affected-powertoys-utilities"></a>影響を受ける Powertoy ユーティリティ

次のシナリオでは、管理者モードのアクセス許可が必要になることがあります。

- Fブランコ Yzones
  - 昇格されたウィンドウ (コマンドプロンプトなど) を凝ったゾーンにスナップする
  - 昇格したウィンドウを別のゾーンに移動する
- ショートカットガイド
  - ショートカットの表示
- キーボード再マッパー
  - キーの再マップ
  - グローバルレベルのショートカットの再マップ
  - アプリを対象とするショートカットの再マップ
- Powertoy の実行
  - ショートカットの表示
