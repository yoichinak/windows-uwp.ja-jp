---
title: Defender 設定を更新してパフォーマンス速度を向上させる
description: Windows Defender の設定を更新して、指定したファイルの種類の確認を除外することで、パフォーマンス速度とビルド時間を向上させる方法について説明します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、windows defender、設定、構成、除外、% USERPROFILE%、devenv.exe、パフォーマンス、速度、ビルド、gradle
ms.date: 04/28/2020
ms.openlocfilehash: 0437ffc263c618e52c7a3e4dc3256e9fcd502c8e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154866"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>パフォーマンスを向上させるための Windows Defender 設定の更新

このガイドでは、windows Defender セキュリティ設定で除外を設定して、ビルド時間と Windows コンピューターの全体的なパフォーマンス速度を向上させる方法について説明します。

## <a name="windows-defender-overview"></a>Windows Defender の概要

Windows 10 バージョン1703以降では、windows [Defender ウイルス対策](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) アプリは windows セキュリティの一部です。 Windows Defender は、ウイルス、ランサムウェア、スパイウェア、およびその他のセキュリティ上の脅威に対する組み込みのリアルタイム保護により、PC を安全な状態に保つことを目的としています。

**ただし**、Windows Defender のリアルタイム保護では、Android アプリの開発時に、ファイルシステムへのアクセスとビルド速度が大幅に低下します。

Android のビルド処理中に、コンピューター上に多くのファイルが作成されます。 ウイルス対策のリアルタイムスキャンを有効にすると、ウイルス対策プログラムによってそのファイルがスキャンされるときに、新しいファイルが作成されるたびにビルドプロセスが停止します。

幸いなことに、Windows Defender には、セキュリティで保護されていることがわかっているファイル、プロジェクトディレクトリ、またはファイルの種類をウイルス対策スキャンプロセスから除外する機能があります。

> [!WARNING]
> 悪意のあるソフトウェアからコンピューターが安全であることを確認するには、リアルタイムスキャンまたは Windows Defender ウイルス対策ソフトウェアを完全に無効にしないでください。

## <a name="add-exclusions-to-windows-defender"></a>Windows Defender への除外の追加

Android のビルド速度を向上させるには、次の方法で [Windows Defender Security Center](windowsdefender://) に除外を追加します。

1. Windows メニューの [ **スタート** ] ボタンを選択する
2. **Windows セキュリティ**を入力してください
3. **ウイルスと脅威の保護**を選択する
4. [**ウイルス & 脅威保護の設定**] の下の [**設定の管理**] を選択します。
5. **除外**の見出しまでスクロールし、[**除外の追加または削除**] を選択します。
6. [ **+ 除外の追加**] を選択します。 次に、追加する除外が **ファイル**、 **フォルダー**、 **ファイルの種類**、または **プロセス**のどれであるかを選択する必要があります。

![Windows Defender の除外の追加のスクリーンショット](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>推奨される除外

次の一覧は、Windows Defender リアルタイムスキャンの除外として追加することを推奨する各 Android Studio ディレクトリの既定の場所を示しています。

- Gradle キャッシュ: `%USERPROFILE%\.gradle`
- Android Studio プロジェクト: `%USERPROFILE%\AndroidStudioProjects`
- Android SDK： `%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio システムファイル: `%USERPROFILE%\.AndroidStudio<version>\system`

Android Studio によって設定された既定の場所を使用していない場合、または GitHub からプロジェクトをダウンロードした場合 (たとえば)、これらのディレクトリの場所は、プロジェクトに適用されない場合があります。 現在の Android 開発プロジェクトのディレクトリに除外を追加することを検討してください。

考慮する必要があるその他の除外事項は次のとおりです。

- Visual Studio 開発環境プロセス: `devenv.exe`
- Visual Studio のビルドプロセス: `msbuild.exe`
- JetBrains ディレクトリ (i): `%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

グループポリシー管理された環境のディレクトリの場所をカスタマイズする方法など、ウイルス対策スキャンの除外を追加する方法の詳細については、Android Studio のドキュメントの「 [ウイルス対策の影響](https://developer.android.com/studio/intro/studio-config#antivirus-impact) 」セクションを参照してください。

> [!Note]
> Daniel Knoodle は、 [Visual Studio 2017 の Windows Defender 除外](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10)を追加するための推奨スクリプトを含む GitHub リポジトリをセットアップしました。

## <a name="additional-resources"></a>その他のリソース

- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

- [Windows Defender の除外を追加してパフォーマンスを向上させる](./defender-settings.md)

- [仮想化サポートを有効にしてエミュレーターのパフォーマンスを向上させる](./emulator.md#enable-virtualization-support)