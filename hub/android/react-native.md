---
title: Windows での Android 開発のためにネイティブに対応
description: Windows で Xamarin Native を使用して Android アプリの開発を開始します。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、ネイティブ、エミュレーター、エキスポ、metro bundler、ターミナルを対応
ms.date: 04/28/2020
ms.openlocfilehash: 50c117154b103ca4e201f21bc643e7cbfa609b84
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157716"
---
# <a name="get-started-developing-for-android-using-react-native"></a>ネイティブな応答を使用した Android 向けの開発の開始

このガイドでは、Windows でのネイティブ対応の使用を開始し、Android デバイスで動作するクロスプラットフォームアプリを作成する方法について説明します。

## <a name="overview"></a>概要

応答ネイティブは、Facebook によって作成された [オープンソース](https://github.com/facebook/react-native) のモバイルアプリケーションフレームワークです。 これは、ネイティブ UI コントロールとネイティブプラットフォームへのフルアクセスを提供する、Android、iOS、Web、UWP (Windows) 用のアプリケーションを開発するために使用されます。 ネイティブな応答を使用するには、JavaScript の基礎を理解する必要があります。

## <a name="get-started-with-react-native-by-installing-required-tools"></a>必要なツールをインストールしてネイティブ対応を開始する

1. [Visual Studio Code](https://code.visualstudio.com) (または任意のコードエディター) をインストールします。

2. [Windows 用の Android Studio をインストール](https://developer.android.com/studio)します。 Android Studio は、既定で最新の Android SDK をインストールします。 ネイティブに対応するには、Android 6.0 (Marshmallow) SDK 以降が必要です。 最新の SDK を使用することをお勧めします。

3. Java SDK と Android SDK の環境変数パスを作成します。
    - Windows の検索メニューで、「システム環境変数の編集」と入力すると、[ **システムのプロパティ** ] ウィンドウが開きます。
    - [**環境変数**] を選択し、[**ユーザー変数**] で [**新規作成**] を選択します。
    - 変数名と値 (path) を入力します。 Java Sdk と Android Sdk の既定のパスは次のとおりです。 Java Sdk と Android Sdk をインストールする特定の場所を選択した場合は、それに応じて変数のパスを更新してください。
        - JAVA_HOME: C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME: C:\Users\username\AppData\Local\Android\Sdk

    ![環境変数パスの追加のスクリーンショット](../images/add-environmental-variable-path.png)

4. [Windows 用 NodeJS のインストール](https://nodejs.org/en/) 複数のプロジェクトとバージョンの NodeJS を操作する場合は、 [Windows 用の Node Version Manager (nvm)](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) を使用することを検討してください。 新しいプロジェクトには、最新の LTS バージョンをインストールすることをお勧めします。

> [!NOTE]
> また、推奨されるコマンドラインインターフェイス (CLI)、および[バージョン管理用の Git](https://git-scm.com/downloads)を操作するために、 [Windows ターミナル](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)をインストールして使用することを検討してください。 [JAVA jdk](https://www.oracle.com/java/technologies/javase-downloads.html)は Android Studio v2.0 以降でパッケージ化されていますが、Android Studio とは別に jdk を更新する必要がある場合は、 [Windows x64 インストーラー](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)を使用してください。

## <a name="create-a-new-project-with-react-native"></a>ネイティブな応答を使用した新しいプロジェクトの作成

1. Npm を使用して、Windows コマンドプロンプト、PowerShell、 [Windows ターミナル](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)、VS Code の統合ターミナル (> 統合ターミナルを表示) から[エキスポ CLI](https://docs.expo.io/versions/latest/)コマンドラインユーティリティをインストールします。

    ```powershell
    npm install -g expo-cli
    ```

2. エキスポを使用して、iOS、Android、web で動作する反応するネイティブアプリを作成します。 次に、 **空白**、 **空白 (typescript)**、 **タブ** (反応ナビゲーションを使用した画面の例)、 **最小**、または **最小 (typescript)** を含むプロジェクトテンプレートを選択する必要があります。

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > を使用してを使用していて `npx create-react-native-app` も、これは引き続き機能しますが、エキスポ init には [いくつかの追加の利点](https://github.com/react-native-community/discussions-and-proposals/issues/23)があります。

3. 新しい "my new-app" ディレクトリを開きます。

    ```powershell
    cd my-new-app
    ```

4. プロジェクトを実行するには、次のコマンドを入力します。 これにより、既定のインターネットブラウザーでノード Metro Bundler が表示されている localhost ウィンドウが開きます。 また、コマンドラインと Metro Bundler ブラウザーウィンドウの両方で QR コードが表示されます。 * コマンドを使用することも `npm start` でき `npm run android` ます。

     ```powershell
    expo start
    ```

    ![ブラウザーの Metro Bundler のスクリーンショット](../images/metro-bundler.png)

5. Android デバイスで実行されているプロジェクトを表示するには、最初に Android デバイスに [Google Play ストアを使用してエキスポクライアントアプリをインストール](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) する必要があります。 エキスポ client アプリがインストールされたら、デバイスでそれを開き、[ **QR コードのスキャン**] を選択します。 QR コードが登録されると、お使いのデバイスと、ローカルのブラウザーで localhost で実行されている Metro Bundler ウィンドウの両方でパッケージのビルドを確認できるようになります。

6. Android エミュレーターで実行されているプロジェクトを表示するには、最初に Android Studio を開き、仮想デバイスを作成して起動する必要があります。 **ツール**  > **Avd マネージャー**  > **[+ 仮想デバイスの作成](https://developer.android.com/studio/run/managing-avds#createavd)**...仮想デバイスが作成されたら、Android 仮想デバイスマネージャーの [**アクション**] 列の下にある起動ボタン▷を選択して、デバイスのエミュレーションを開始します。 仮想デバイスが開いたら、インターネットブラウザーウィンドウで実行されている Metro Bundler ウィンドウに戻り、左側の列から [Android デバイス/エミュレーターで実行] を選択します。 Metro Bundler が "シミュレーターを開こうとしています..." であることを知らせるポップアップが表示されます。エミュレートされた Android デバイスでエキスポクライアントアプリを開いていることを確認します。 JavaScript バンドルのダウンロードが完了すると、[対応するネイティブアプリ] が表示されます。 (問題が発生した場合は、 [エキスポ Android emulator のドキュメントを確認して](https://docs.expo.io/workflow/android-studio-emulator/)ください)。

7. 反応するネイティブプロジェクトを開いて、アプリでの作業を開始します。 デバイス上のエキスポクライアントまたは Android Emulator で実行されているアプリで、変更が自動更新されていることがわかります。

8. ランディングページの表示テキストを変更してみてください。「Hello World!」と言います。 この操作は、選択した IDE で行うことができます。 (VS Code または Android Studio することをお勧めします)。ランディングページファイルは、選択したテンプレートによって異なります。 `App.js`、、またはのいずれかになり `App.tsx` `HomeScreen.js` ます。

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. イメージを追加してみてください。 まず、アプリの "android" フォルダーと "ios" フォルダーと同じレベルでフォルダーを作成する必要があります。 "images" というフォルダーを呼び出しましょう。 たとえば、イメージをそのフォルダーに配置 `your-image.png` します。 下の形式を使用してイメージを参照し、高さと幅でスタイルを設定します。

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Windows 10 アプリとして実行されるように、対応するネイティブアプリのサポートを追加する場合は、 [windows 用ネイティブアプリの応答に](https://microsoft.github.io/react-native-windows/docs/getting-started) 関するドキュメントを参照してください。

## <a name="additional-resources"></a>その他のリソース

- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

- [Windows Defender の除外を追加してパフォーマンスを向上させる](defender-settings.md)

- [仮想化サポートを有効にしてエミュレーターのパフォーマンスを向上させる](emulator.md#enable-virtualization-support)