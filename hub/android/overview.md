---
title: Windows での Android 開発の概要
description: Windows で Android の開発を始める際に役立つガイドです。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows での android、xamarin android、ネイティブ、cordova、ionic、phonegap、c++ android game、windows defender、emulator
ms.date: 04/28/2020
ms.openlocfilehash: e215d9e08fcef7ddb1caae40bd8f3a83e183d197
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157706"
---
# <a name="overview-of-android-development-on-windows"></a>Windows での Android 開発の概要

Windows オペレーティングシステムを使用して Android デバイスアプリを開発するための複数のパスがあります。 これらのパスは、 **[ネイティブ Android 開発](#native-android)**、 **[クロスプラットフォーム開発](#cross-platform)**、 **[android ゲーム開発](#game-development)** という3つの主要な種類に分類されます。 この概要では、Android アプリの開発に使用する開発パスを決定し、 [次の手順](#next-steps) に従って、を使用した Windows の開発を開始する方法について説明します。

- [ネイティブ Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova、Ionic、または PhoneGap](pwa.md)
- [ゲーム開発用の c/c + +](native-android.md#use-c-or-c-for-android-game-development)

また、このガイドでは、Windows を使用して次のことを行うためのヒントを提供します。

- [Android デバイスまたはエミュレーターでテストする](emulator.md)
- [パフォーマンスを向上させるための Windows Defender 設定の更新](defender-settings.md)
- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)

## <a name="native-android"></a>ネイティブ Android

[Windows でのネイティブ android 開発](./native-android.md) は、アプリが android のみを対象としていることを意味します (iOS デバイスでも Windows デバイスでもありません)。 [Android Studio](https://developer.android.com/studio/install#windows)または[Visual Studio](https://visualstudio.microsoft.com/vs/android/)を使用して、Android オペレーティングシステム専用に設計されたエコシステム内で開発を行うことができます。 Android デバイス向けにパフォーマンスが最適化され、ユーザーインターフェイスのルックアンドフィールがデバイス上の他のネイティブアプリと一貫性を持つようになります。また、ユーザーのデバイスのすべての機能がアクセスして使用できるようになります。 ネイティブ形式でアプリを開発すると、Android デバイス専用に確立されたすべての対話パターンとユーザーエクスペリエンス標準に従っているため、"安心感" を実現できます。

## <a name="cross-platform"></a>クロスプラットフォーム

クロスプラットフォームフレームワークには、Android、iOS、および Windows デバイス間で (ほとんど) 共有できる単一のコードベースが用意されています。 クロスプラットフォームフレームワークを使用すると、アプリは、デバイスプラットフォーム全体にわたって同じ外観、操作性、エクスペリエンスを維持できるだけでなく、更新や修正プログラムの自動ロールアウトによるメリットも得られます。 さまざまなデバイス固有のコード言語を理解する必要はありませんが、アプリは共有コードベース (通常は1つの言語) で開発されています。

クロスプラットフォームフレームワークは、可能な限りネイティブアプリに近いルックアンドフィールを目的としていますが、ネイティブに開発されたアプリとしてシームレスに統合されることはなく、速度の低下とパフォーマンスの低下の影響を受ける可能性があります。 また、クロスプラットフォームアプリのビルドに使用されるツールには、各デバイスプラットフォームで提供されるすべての機能が含まれていない場合があります。回避策が必要になる可能性があります。

コードベースは、通常、ページ、ボタンコントロール、ラベル、リストなどのユーザーインターフェイスを作成するために、web サービスの呼び出し、データベースへのアクセス、ハードウェア機能の呼び出し、および状態の管理のため**に、** **UI コード**で構成されます。 平均して、このの90% を再利用できますが、通常はデバイスプラットフォームごとにコードをカスタマイズする必要があります。 この一般化は、構築しているアプリの種類によって大きく異なりますが、お客様の意思決定に役立つ少しのコンテキストを提供します。  

## <a name="choosing-a-cross-platform-framework"></a>クロスプラットフォームフレームワークの選択

[Xamarin Native (Xamarin Android)](xamarin-android.md)

- UI コード: Android Designer のある XML と、マテリアルのテーマ
- ロジックコード: C# または F#
- まだいくつかのネイティブ Android 要素を利用できますが、他のプラットフォーム (iOS、Windows) のコードベースを再利用するのに適しています。
- ロジックコードだけが、UI コードではなく、プラットフォーム間で共有されます。
- デバイス固有のユーザーインターフェイスを使用した、より複雑なアプリに適しています。

[Xamarin フォーム (Xamarin. Forms)](xamarin-forms.md)

- UI コード: XAML と .NET (Visual Studio を使用)
- ロジックコード: C#
- Android、iOS、および Windows デバイスアプリ全体で、ロジックと UI コードの 60 ~ 90% を共有します。 
- ボタン、ラベル、エントリ、ListView、StackLayout、Calendar、TabbedPage などの一般的なユーザーコントロールを使用します。ボタンと Xamarin フォームを作成すると、バインドライブラリを使用して各プラットフォームのネイティブボタンを呼び出し、C# から Java または Swift コードを呼び出す方法を確認できます。
- 内部または基幹業務 (LOB) アプリ、プロトタイプ、Mvp などの単純なアプリに最適です。 単純なユーザーインターフェイスを使用して、標準またはジェネリックを表示できる任意のアプリ。

[React Native](react-native.md)

- UI コード: JavaScript
- ロジックコード: JavaScript
- ネイティブに対応する目的は、コードを1回記述し、任意のプラットフォームで実行することではなく、1回だけ (反応する方法で)、どこにでも書き込みを行うことではありません。
- このコミュニティでは、エキスポなどのツールを追加して、Xcode や Android Studio を使用せずにアプリを構築することを望むネイティブアプリを作成できます。
- Xamarin (C#) と同様に、ネイティブ (JavaScript) 呼び出しのネイティブ UI 要素 (Java/Kotlin または Swift を記述する必要はありません) を認識します。

[プログレッシブ Web アプリ (PWA)](pwa.md)

- UI コード: HTML、CSS、JavaScript
- ロジックコード: JavaScript
- PWAs は、web アプリとネイティブアプリ機能の両方を利用できるように、標準パターンでビルドされた web apps です。 フレームワークを使用せずにビルドすることもできますが、いくつかの一般的なフレームワークとして、 [Ionic](https://ionicframework.com/docs/intro) と [PhoneGap](https://phonegap.com/about/)があります。
- PWAs は、デバイス (Android、iOS、または Windows) にインストールすることができ、サービスワーカーを組み込むことにより、オフラインで動作することができます。
- PWAs は、web URL のみを使用してアプリストアなしで配布およびインストールすることができます。 Microsoft Store と Google Play ストアで PWAs が一覧表示されますが、現在のところ、Apple ストアは12.2 以降を実行している iOS デバイスにインストールできます。
- 詳細については、MDN での [PWAs の概要に](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) 関するページをご覧ください。

## <a name="game-development"></a>ゲーム開発

Android 用のゲーム開発は、通常は OpenGL または Vulkan で記述されたカスタムレンダリングロジックを使用するため、標準の Android アプリの開発とは異なります。 この理由から、ゲーム開発をサポートする多くの C ライブラリが使用されているため、開発者は、android [Native Development Kit (NDK)](/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)と共に[c/c + +](/cpp/cross-platform/?view=vs-2019)を使用して android 用のゲームを作成するのが一般的です。 [ゲーム開発用の C/c + + の使用を開始](native-android.md#use-c-or-c-for-android-game-development)します。

Android 用にゲームを開発するためのもう1つの一般的なパスは、ゲームエンジンを使用することです。 利用可能な無料のオープンソースエンジンは多数あります。たとえば、 [Visual Studio](/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019)、 [unreal Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html)、 [モノゲーム (xamarin](/xamarin/graphics-games/monogame/introduction/))、 [urhosharp With](/xamarin/graphics-games/urhosharp/introduction)Xamarin、 [SkiaSharp](/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) WITH xamarin、アプリゲームキット、Fusion、コロナ SDK、ココス2d などがあります。

## <a name="next-steps"></a>次の手順

- [Windows でのネイティブ Android 開発の概要](native-android.md)
- [Xamarin を使用して Android 向けの開発を始める](xamarin-android.md)
- [Xamarin を使用して Android 向けの開発を始める](xamarin-forms.md)
- [ネイティブな応答を使用した Android 向けの開発の開始](react-native.md)
- [Android 用 PWA の開発を開始する](pwa.md)
- [Android 用のデュアルスクリーンアプリを開発し、Surface Duo デバイス SDK を入手する](/dual-screen/android/)
- [Windows Defender の除外を追加してパフォーマンスを向上させる](defender-settings.md)
- [仮想化サポートを有効にしてエミュレーターのパフォーマンスを向上させる](emulator.md#enable-virtualization-support)