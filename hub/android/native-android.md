---
title: Windows でのネイティブ Android 開発
description: Windows で Android ネイティブアプリの開発を開始しましょう。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: android、windows、android studio、visual studio、c++ android game、windows defender、emulator、virtual device、install、java、kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c9c718d2cccc6a38ac75d3220a79c7b2ec757f54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166846"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Windows でのネイティブ Android 開発の概要

このガイドでは、Windows を使用してネイティブの Android アプリケーションを作成する方法について説明します。 クロスプラットフォームソリューションを使用する場合は、一部のオプションの概要については、「 [Windows での Android 開発の概要](./overview.md) 」を参照してください。

ネイティブ Android アプリを作成するための最も簡単な方法は、 [Java または Kotlin](#java-or-kotlin)で Android Studio を使用することです。ただし、特定の目的がある場合は、 [C または C++ を Android 開発に使用](#use-c-or-c-for-android-game-development) することもできます。 Android Studio SDK ツールは、コード、データ、およびリソースファイルをアーカイブ Android パッケージである apk ファイルにコンパイルします。 1つの APK ファイルには Android アプリのすべてのコンテンツが含まれており、Android 搭載デバイスがアプリをインストールするために使用するファイルです。

## <a name="install-android-studio"></a>Android Studio のインストール

Android Studio は、Google の Android オペレーティングシステム用の正式な統合開発環境です。 [Windows 用の最新バージョンの Android Studio をダウンロード](https://developer.android.com/studio)します。

- .Exe ファイルをダウンロードした場合 (推奨)、ダブルクリックして起動します。
- .Zip ファイルをダウンロードした場合は、ZIP ファイルを展開し、android-studio フォルダーを Program Files フォルダーにコピーしてから、android-studio > bin フォルダーを開き studio64.exe (64 ビットコンピューターの場合) または studio.exe (32 ビットコンピューターの場合) を起動します。

Android Studio のセットアップウィザードに従って、推奨される SDK パッケージをインストールします。 新しいツールやその他の api が使用可能になると、Android Studio はポップアップを通知するか、[**ヘルプ**] [  >  **更新の確認**] の順に選択して更新プログラムを確認します。

## <a name="create-a-new-project"></a>新しいプロジェクトを作成する

[**ファイル**] [新規作成] [  >  **New**  >  **新しいプロジェクト**] を選択します。

[ **プロジェクトの選択** ] ウィンドウでは、次のテンプレートのいずれかを選択できます。

- **基本アクティビティ**: アプリバー、フローティングアクションボタン、2つのレイアウトファイルを含む単純なアプリを作成します。1つはアクティビティ用で、もう1つはテキストコンテンツを分離するためのものです。

- **空のアクティビティ**: 空のアクティビティと、サンプルテキストコンテンツを含む1つのレイアウトファイルを作成します。

- **下部ナビゲーションアクティビティ**: アクティビティの標準の下部ナビゲーションバーを作成します。 Google のマテリアルデザインガイドラインの [下部ナビゲーションコンポーネント](https://material.io/guidelines/components/bottom-navigation.html) を参照してください。

- [アクティビティテンプレートの選択](https://developer.android.com/studio/projects/templates#SelectTemplate)の詳細については、Android Studio のドキュメントを参照してください。

テンプレートは、通常、新規および既存のアプリモジュールにアクティビティを追加するために使用されます。 たとえば、アプリのユーザーのログイン画面を作成するには、 [ログインアクティビティテンプレート](https://developer.android.com/studio/projects/templates#LoginActivity)を使用してアクティビティを追加します。

> [!NOTE]
> Android オペレーティングシステムは、 **コンポーネント** の概念に基づいており、 **アクティビティ** と **目的** という用語を使用して相互作用を定義します。 **アクティビティ**は、ユーザーが実行できる1つのフォーカスがあるタスクを表します。 **アクティビティ**には、**ビュー**クラスに基づくクラスを使用してユーザーインターフェイスを構築するためのウィンドウが用意されています。 Android オペレーティングシステムでは**activities** 、、、、、、 `onCreate()` `onStart()` `onResume()` `onPause()` `onStop()` およびと `onDestroy()` いう6つのコールバックのセットによって定義されるアクティビティのライフサイクルがあります。 アクティビティコンポーネントは、 **インテント** オブジェクトを使用して相互に対話します。 インテントは、開始するアクティビティを定義するか、実行するアクションの種類を記述します (また、システムは適切なアクティビティを選択します。これは、別のアプリケーションからでもかまいません)。 [アクティビティ](https://developer.android.com/reference/android/app/Activity)、[アクティビティのライフサイクル](https://developer.android.com/guide/components/activities/activity-lifecycle)、および Android ドキュメントに含まれる[インテント](https://developer.android.com/reference/android/content/Intent.html)の詳細について説明します。

### <a name="java-or-kotlin"></a>Java または Kotlin

**Java** は1991の言語になり、Sun Microsystems によって開発されましたが、現在は Oracle が所有しています。 世界中の最大のサポートコミュニティの1つである、最も人気のある強力なプログラミング言語の1つになっています。 Java は、可能な限り実装の依存関係を最小限にするように設計されたクラスベースのオブジェクト指向のオブジェクト指向クラスです。 構文は C と C++ に似ていますが、どちらの方法よりも低レベルの機能が少なくなっています。

**Kotlin** は、2011の JetBrains によって最初に新しいオープンソース言語として発表され、2017以降 Android Studio の Java の代わりに含まれています。 2019年5月に、Google は Android アプリ開発者向けの優先言語として Kotlin を発表しました。したがって、新しい言語の場合でも、強力なサポートコミュニティがあり、最も急速に成長するプログラミング言語の1つとして識別されています。 Kotlin は、静的に型指定されたクロスプラットフォームで、Java と完全に相互運用できるように設計されています。

Java は、より広範なアプリケーションで広く使用されており、チェックされた例外、クラスではないプリミティブ型、静的メンバー、非プライベートフィールド、ワイルドカード型、三項演算子など、Kotlin がない機能を提供しています。 Kotlin は、Android で特に設計され、推奨されています。 また、Java にはないいくつかの機能も用意されています。型システムによって制御される null 参照、未加工の型、インバリアント配列、適切な関数の型 (Java の SAM 変換とは異なります)、ワイルドカードを使用しないサイトの分散、スマートキャストなどがあります。 Kotlin のドキュメントでは、Java との [比較について詳しく](https://kotlinlang.org/docs/reference/comparison-to-java.html)説明しています。

### <a name="minimum-api-level"></a>最小 API レベル

アプリケーションの最小 API レベルを決定する必要があります。 これにより、アプリケーションがサポートする Android のバージョンが決まります。 低い API レベルは古いため、一般により多くのデバイスをサポートしていますが、より高い API レベルが新しくなり、れるためにより多くの機能が提供されます。

![Android Studio 最小 API 選択画面](../images/android-minimum-api-selection.png)

[ **ヘルプを選択** してください] リンクを選択すると、プラットフォームバージョンのリリースに関連付けられているデバイスサポートの配布と主な機能を示す比較グラフが表示されます。

![Android Studio 最小 API 比較画面](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>インスタントアプリのサポートと Androidx 成果物

**インスタントアプリをサポート**するチェックボックスと、プロジェクト作成オプションで**androidx 成果物を使用**するためのチェックボックスが表示される場合があります。 [ *インスタントアプリのサポート* ] はオフになっており、推奨される既定値として [ *androidx* ] がオンになっています。

Google Play **インスタントアプリ** を使用すると、最初にアプリやゲームをインストールせずに試すことができます。 これらのインスタントアプリは、Play ストア、Google Search、ソーシャルネットワークのほか、リンクを共有しているどこにでも表示できます。 [サポート- **インスタントアプリ** ] ボックスをオンにすると、プロジェクトと共に Google Play インスタント開発 SDK を含めるように Android Studio 求められます。 [Google Play インスタントアプリ](https://developer.android.com/topic/google-play-instant)の詳細と、[インスタント対応アプリバンドルを作成](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)する方法の詳細については、Android のドキュメントを参照してください。

**Androidx アーティファクト** は、android サポートライブラリの新しいバージョンを表し、android リリース間の下位互換性を提供します。 AndroidX は、使用可能なすべてのパッケージの文字列 androidx で始まる一貫した名前空間を提供します。

> [!NOTE]
> AndroidX が既定のライブラリになりました。 このチェックボックスをオフにして、以前のサポートライブラリを使用するには、最新 Android Q SDK を削除する必要があります。 手順については、StackOverflow での [androidx アーティファクトの使用をオフ](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) にする方法に関する説明を参照してください。ただし、前者のサポートライブラリパッケージが対応する androidx. * パッケージにマップされていることに注意してください。 すべての古いクラスとビルドアーティファクトの完全なマッピングについては、「 [AndroidX への移行](https://developer.android.com/jetpack/androidx/migrate)」を参照してください。

## <a name="project-files"></a>プロジェクト ファイル

[Android Studio **プロジェクト** ] ウィンドウには、次のファイルが含まれています ([Android] ビューがドロップダウンメニューから選択されていることを確認してください)。

**アプリ > java > myfirstapp > MainActivity**

アプリのメインアクティビティとエントリポイント。 アプリをビルドして実行すると、システムはこのアクティビティのインスタンスを起動し、そのレイアウトを読み込みます。

**アプリ > res > レイアウト > activity_main.xml**

アクティビティのユーザーインターフェイス (UI) のレイアウトを定義する XML ファイル。 これには、"Hello World" というテキストを含む TextView 要素が含まれています。

**アプリ > マニフェスト > AndroidManifest.xml**

アプリとその各コンポーネントの基本的な特性を記述するマニフェストファイル。

**Gradle スクリプト > Gradle**

この名前のファイルには、プロジェクト全体の場合は "Project: My First App"、アプリモジュールごとに "Module: app" という2つのファイルがあります。 新しいプロジェクトには、最初に1つのモジュールのみが含まれます。 モジュールの build. ファイルを使用して、Gradle プラグインによるアプリのビルド方法を制御します。 詳細については、Android ドキュメントの「 [ビルドを構成する](https://developer.android.com/studio/build/index) 」を参照してください。

## <a name="use-c-or-c-for-android-game-development"></a>Android ゲーム開発に C または C++ を使用する

Android オペレーティングシステムは、Java または Kotlin で記述されたアプリケーションをサポートするように設計されており、システムのアーキテクチャに組み込まれているツールを活用できます。 Android UI やインテント処理のような多くのシステム機能は、Java インターフェイスを介してのみ公開されます。 関連する課題のいくつかにかかわらず、 **Android Native Development Kit (NDK) で C または C++ コードを使用** する必要がある場合があります。 ゲーム開発は、一般に OpenGL または Vulkan で記述されたカスタムレンダリングロジックを使用して、ゲーム開発に重点を置いた豊富な C ライブラリを活用するための例です。 また、C また *は C++ を* 使用すると、デバイスのパフォーマンスを向上させることで、待機時間を短縮したり、物理シミュレーションなどの大量の負荷の高いアプリケーションを実行したりすることもできます。 ただし、NDK **はほとんどの初心者の Android プログラマーには適していません** 。 NDK を使用する特定の目的がない限り、Java、Kotlin、または [クロスプラットフォームフレームワーク](./overview.md)のいずれかにすることをお勧めします。

C/c + + サポートを使用して新しいプロジェクトを作成するには、次のようにします。

- Android Studio ウィザードの [ **プロジェクトの選択** ] セクションで、 *ネイティブ C++** プロジェクトの種類を選択します。 [ **次へ**] を選択し、残りのフィールドを入力して、[ **次へ** ] を再度選択します。

- ウィザードの [ **C++ サポートのカスタマイズ** ] セクションでは、 **c++ 標準** フィールドを使用してプロジェクトをカスタマイズできます。 ドロップダウンリストを使用して、使用する C++ の標準化を選択します。 [ツール **チェーンの既定値** ] を選択すると、既定の cmake 設定が使用されます。 **[完了]** を選択します。

- Android Studio によって新しいプロジェクトが作成されると、**プロジェクト**ウィンドウに**cpp**フォルダーが見つかります。このフォルダーには、ネイティブソースファイル、ヘッダー、cmake または ndk ビルドのビルドスクリプト、プロジェクトの一部であるビルド済みライブラリが含まれています。 また、サンプル C++ ソースファイル () をフォルダーで検索することもでき `native-lib.cpp` ます。このフォルダーには、 `src/main/cpp/` `stringFromJNI()` "Hello from C++" という文字列を返す単純な関数が用意されています。 また、 [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) ネイティブライブラリを構築するために必要なモジュールのルートディレクトリに、CMake ビルドスクリプトがあります。

詳細については、「Android ドキュメント: [プロジェクトへの c および C++ コードの追加](https://developer.android.com/studio/projects/add-native-code)」を参照してください。 サンプルについては、GitHub の [Android Studio C++ 統合リポジトリを使用した ANDROID NDK のサンプル](https://github.com/android/ndk-samples) を参照してください。 Android で C++ ゲームをコンパイルして実行するには、 [Google Play game SERVICES API](https://developers.google.com/games/services/cpp/gettingStartedAndroid)を使用します。

## <a name="design-guidelines"></a>設計ガイドライン

デバイスユーザーは、アプリケーションが特定の方法で検索および動作することを期待しています...音声コントロールを使用するかタップするか、使用するかにかかわらず、ユーザーには、アプリケーションの外観や使用方法に関する特定の期待があります。 これらの予測は、混乱とフラストレーションを軽減するために、一貫性を維持する必要があります。 Android では、これらのプラットフォームとデバイスの予測に関するガイドを提供しています。これらのプラットフォームとデバイスでは、Google のマテリアルデザイン基盤とビジュアルおよびナビゲーションパターンが統合されており、互換性、パフォーマンス、およびセキュリティに関する品質ガイドラインがあります。

詳細については、 [Android の設計に関するドキュメント](https://developer.android.com/design)を参照してください。

### <a name="fluent-design-system-for-android"></a>Android 用 Fluent デザインシステム

Microsoft では、Microsoft のモバイルアプリのポートフォリオ全体にシームレスなエクスペリエンスを提供するという目標を達成するための設計ガイダンスも提供しています。

[Android 用の Fluent デザインシステム](https://www.microsoft.com/design/fluent/#/android) では、ネイティブな android であるカスタムアプリを設計し、それでも独自の fluent を構築します。

- [Sketch ツールキット](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma ツールキット](https://aka.ms/fluenttoolkits/android/figma)
- [Android フォント](https://fonts.google.com/specimen/Roboto)
- [Android ユーザーインターフェイスのガイドライン](https://developer.android.com/design/)
- [Android アプリアイコンのガイドライン](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>その他のリソース

- [Android アプリケーションの基礎](https://developer.android.com/guide/components/fundamentals)

- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

- [Windows Defender の除外を追加してパフォーマンスを向上させる](defender-settings.md)

- [仮想化サポートを有効にしてエミュレーターのパフォーマンスを向上させる](emulator.md#enable-virtualization-support)