---
title: Windows での Android 開発への PWA アプローチ
description: Windows で PWA アプローチを使用して Android アプリの開発を開始しましょう。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows での android、pwa、android、cordova、ionic、phonegap、ハイブリッド web アプリ
ms.date: 04/28/2020
ms.openlocfilehash: 4559e795b4a9737bf68129790029f6f9136b4f81
ms.sourcegitcommit: 99f5544d9642c87a16e3bd21f76c2fcbc97c20d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743595"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Android 用の PWA またはハイブリッド web アプリの開発を開始する

このガイドでは、web およびデバイスプラットフォーム (Android、iOS、Windows) で使用できる単一の HTML/CSS/JavaScript コードベースを使用して、Windows 上でハイブリッド web アプリまたはプログレッシブ Web アプリ (PWA) を作成する方法について説明します。

適切なフレームワークとコンポーネントを使用することにより、web ベースのアプリケーションは、ネイティブアプリと非常によく似た方法で、Android デバイスで動作することができます。

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>PWA またはハイブリッド web アプリの機能

Android デバイスにインストールできる web アプリには、主に2つの種類があります。 主な違いは、アプリケーションコードがアプリケーションパッケージ (ハイブリッド) に埋め込まれているか、web サーバー (pwa) でホストされているかということです。

- **ハイブリッド web アプリ**: コード (HTML、JS、CSS) は apk にパッケージ化されており、Google Play ストア経由で配布できます。 表示エンジンは、ユーザーのインターネットブラウザーから分離され、セッションもキャッシュも共有されません。

- **プログレッシブ Web Apps (PWAs)**: コード (HTML、JS、CSS) は Web 上に存在し、apk としてパッケージ化する必要はありません。 リソースは、サービスワーカーを使用して必要に応じてダウンロードおよび更新されます。 Chrome ブラウザーはアプリをレンダリングして表示しますが、通常のブラウザーのアドレスバーではなく、ネイティブに見えます。ブラウザーでストレージ、キャッシュ、セッションを共有できます。 これは基本的に、特別なモードで Chrome ブラウザーへのショートカットをインストールするのと似ています。 PWAs は、信頼された Web アクティビティを使用して Google Play ストアに表示することもできます。

PWAs とハイブリッド web アプリは、ネイティブの Android アプリと非常によく似ています。

- アプリストア (Google Play ストアまたは Microsoft Store) を使用してインストールできます。
- カメラ、GPS、Bluetooth、通知、連絡先の一覧などのネイティブデバイス機能にアクセスできる
- オフラインで作業する (インターネット接続なし)

PWAs には、いくつかの固有の機能もあります。

- (アプリストアを使用せずに) web から直接 Android ホーム画面にインストールできます。
- また、[信頼できる Web アクティビティを使用し](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/)て Google Play ストア経由でインストールすることもできます。
- Web 検索を使用して検出することも、URL リンクを使用して共有することもできます。
- [サービスワーカー](https://developers.google.com/web/fundamentals/primers/service-workers)に依存して、ネイティブコードをパッケージ化する必要がないようにする

ハイブリッドアプリまたは PWA を作成するためのフレームワークは必要ありませんが、PhoneGap (Cordova) と Ionic (角度を使用した Cordova またはコンデンサー) を含む、このガイドで説明されているいくつかの一般的なフレームワークがあります。

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/)は、[プラグイン](https://cordova.apache.org/plugins/?platforms=cordova-android)を使用してネイティブの Web[ビューとネイティブ](https://developer.android.com/reference/android/webkit/WebView)Android プラットフォームの JavaScript コード間の通信を簡略化できるオープンソースフレームワークです。 これらのプラグインは、コードから呼び出すことができ、ネイティブの Android デバイス Api を呼び出すために使用できる JavaScript エンドポイントを公開します。 Cordova プラグインの例としては、バッテリの状態、ファイルアクセス、振動/リングトーンなどのデバイスサービスへのアクセスなどがあります。これらの機能は、通常、web アプリまたはブラウザーでは使用できません。

Cordova には、次の2つの一般的なディストリビューションがあります。

- [PhoneGap](https://blog.phonegap.com/update-for-customers-using-phonegap-and-phonegap-build-cc701c77502c): サポートは Adobe によって中止されました。

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

最近、サポートが中止されました。 詳細については、 [Adobe のブログ投稿](https://blog.phonegap.com/update-for-customers-using-phonegap-and-phonegap-build-cc701c77502c)を参照してください。

### <a name="install-phonegap"></a>PhoneGap のインストール

PhoneGap を使用して PWA またはハイブリッド web アプリの構築を開始するには、まず次のツールをインストールする必要があります。

- Ionic エコシステムと対話するための Node.js。 Windows[用 NodeJS をダウンロード](https://nodejs.org/en/)するか、windows Subsystem for LINUX (wsl) を使用した[nodejs インストールガイド](../nodejs/setup-on-wsl2.md)に従ってください。 複数のプロジェクトとバージョンの NodeJS を操作する場合は、 [Node Version Manager (nvm)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) を使用することを検討してください。

コマンドラインに次のコマンドを入力して、PhoneGap をインストールします。

```bash
npm install -g phonegap
```

新しい PhoneGap プロジェクトを作成するには、手順に従って [作業を開始](https://phonegap.com/getstarted/)します。 アプリをハイブリッドから PWA に移行する方法については、PhoneGap docs の [Pwa 機能](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) に関するセクションを参照してください。  

## <a name="ionic"></a>Ionic

[Ionic](https://ionicframework.com/) は、各プラットフォームのデザイン言語 (Android、IOS、Windows) に合わせてアプリのユーザーインターフェイス (UI) を調整するフレームワークです。 Ionic を使用すると、 [角度](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) を使用したり、 [反応](https://ionicframework.com/react)したりすることができます。

> [!NOTE]
> 新しいバージョンの Ionic があります。これは、" [コンデンサー](https://capacitor.ionicframework.com/)" と呼ばれる Cordova の代わりに使用されます。 この方法では、コンテナーを使用してアプリを [より web で](https://ionicframework.com/blog/announcing-capacitor-1-0/)使いやすくします。

### <a name="get-started-with-ionic-by-installing-required-tools"></a>必要なツールをインストールして Ionic の使用を開始する

Ionic を使用して PWA またはハイブリッド web アプリの構築を開始するには、まず次のツールをインストールする必要があります。

- Ionic エコシステムと対話するための Node.js。 Windows[用 NodeJS をダウンロード](https://nodejs.org/en/)するか、windows Subsystem for LINUX (wsl) を使用した[nodejs インストールガイド](../nodejs/setup-on-wsl2.md)に従ってください。 複数のプロジェクトとバージョンの NodeJS を操作する場合は、 [Node Version Manager (nvm)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) を使用することを検討してください。

- コードを記述するための VS Code。 [Windows 用の VS Code をダウンロード](https://code.visualstudio.com/)します。 Linux コマンドラインを使用してアプリをビルドする場合は、 [Wsl リモート拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) をインストールすることもできます。

- 任意のコマンドラインインターフェイス (CLI) を操作するための Windows ターミナル。 [Microsoft Store から Windows ターミナルをインストール](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)します。

- バージョン管理用の Git。 [Git をダウンロード](https://git-scm.com/downloads)します。

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>Ionic Cordova と角度を使用して新しいプロジェクトを作成する

コマンドラインに次のコマンドを入力して、Ionic と Cordova をインストールします。

```bash
npm install -g @ionic/cli cordova
```

次のコマンドを入力して、"タブ" アプリテンプレートを使用して Ionic 角速度アプリを作成します。

```bash
ionic start photo-gallery tabs
```

アプリフォルダーに変更します。

```bash
cd photo-gallery
```

Web ブラウザーでアプリを実行します。

```bash
ionic serve
```

詳細については、 [Ionic Cordova の角度ドキュメント](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)を参照してください。アプリをハイブリッドから PWA に移行する方法については、Ionic docs の「 [角度アプリを pwa にする](https://ionicframework.com/docs/angular/pwa) 」セクションを参照してください。

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>Ionic コンデンサーと角度を使用して新しいプロジェクトを作成する

コマンドラインで次のように入力して、Ionic と Cordova-Res をインストールします。

```bash
npm install -g @ionic/cli native-run cordova-res
```

"タブ" アプリテンプレートを使用して Ionic の角度アプリを作成し、コマンドを入力してコンデンサーを追加します。

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

アプリフォルダーに変更します。

```bash
cd photo-gallery
```

アプリケーションを PWA にするコンポーネントを追加します。

```bash
npm install @ionic/pwa-elements
```

@ionic/pwa-elementsファイルに次の内容を追加してインポートし `src/main.ts` ます。

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

Web ブラウザーでアプリを実行します。

```bash
ionic serve
```

詳細については、 [Ionic コンデンサーの傾斜](https://ionicframework.com/docs/angular/your-first-app)に関するドキュメントを参照してください。アプリをハイブリッドから PWA に移行する方法については、Ionic docs の「 [角度アプリを pwa にする](https://ionicframework.com/docs/angular/pwa) 」セクションを参照してください。  

## <a name="create-a-new-project-with-ionic-and-react"></a>Ionic を使用して新しいプロジェクトを作成し、対処する

コマンドラインに次のコマンドを入力して、Ionic CLI をインストールします。

```bash
npm install -g @ionic/cli
```

次のコマンドを入力して、新しいプロジェクトを作成します。

```bash
ionic start myApp blank --type=react
```

アプリフォルダーに変更します。

```bash
cd myApp
```

Web ブラウザーでアプリを実行します。

```bash
ionic serve
```

詳細については、 [Ionic](https://ionicframework.com/docs/react/quickstart)に対応するドキュメントを参照してください。アプリをハイブリッドから PWA に移行する方法については、Ionic ドキュメントの「 [アプリケーションを pwa に応答](https://ionicframework.com/docs/react/pwa) させる」セクションを参照してください。

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>デバイスまたはエミュレーターで Ionic アプリをテストする

Android デバイスで Ionic アプリをテストするには、デバイスにプラグインします ([最初に開発用に有効](emulator.md#enable-your-device-for-development)になっていることを確認します)。次に、コマンドラインで次のように入力します。

```bash
ionic cordova run android
```

Android デバイスエミュレーターで Ionic アプリをテストするには、次のことを行う必要があります。

1. [必要なコンポーネント (Java Development Kit (JDK)、Gradle、および Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements)) をインストールします。

2. [Android 仮想デバイス (AVD) を作成](https://developer.android.com/studio/run/managing-avds.html)します。

3. Ionic のコマンドを入力して、アプリケーションをビルドし、エミュレーターにデプロイし `ionic cordova emulate [<platform>] [options]` ます。 この場合、コマンドは次のようになります。

```bash
ionic cordova emulate android --list
```

詳細については、Ionic ドキュメントの [Cordova エミュレーター](https://ionicframework.com/docs/cli/commands/cordova-emulate) を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

- [Windows Defender の除外を追加してパフォーマンスを向上させる](defender-settings.md)

- [仮想化サポートを有効にしてエミュレーターのパフォーマンスを向上させる](emulator.md#enable-virtualization-support)
