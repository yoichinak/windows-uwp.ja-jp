---
title: Windows 10 のユーザー補助機能
description: このページでは、ユーザー補助 Windows アプリの開発を開始するための情報を提供します。
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10 のアクセシビリティ, アクセシビリティ, アクセス可能な win32 アプリの構築, アクセス可能な UWP アプリのビルド, アクセス可能な WPF アプリのビルド, アクセス可能な WinForms アプリの構築
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 76bbd3f0e04bbb2f729ad0950bae190b2fffb6ac
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987196"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 のユーザー補助機能

![hero-accessibility-bar-smaller](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>アプリケーションにアクセシビリティを構築して、すべての機能を強化

電子メディアを含む製品とサービスには、できるだけ多くのユーザーエクスペリエンスを提供するように設計されている場合にアクセスできます。

優れた機能と使いやすさを備えた、アクセス可能で包括的な Windows アプリケーションを構築できます。障碍のある方 (一時的なものでも永続的でもあります)、個人設定、特定の作業スタイル、状況制約 (共有ワークスペースなど) があります。運転、料理、グレアなど)。 一般的な解決策としては、別の形式 (ビデオのキャプションなど) の情報を提供したり、補助技術 (スクリーンリーダーなど) の使用を有効にしたりすることがあります。

**すべてのユーザーは、階段またはエレベーターを使用する必要があるかどうかにかかわらず、建物内の同じ部屋にアクセスできる必要があります。**

このページでは、windows アプリケーションを構築する開発者のためのユーザー補助機能のサポート、スクリーンリーダーや拡大鏡などのツールを作成する支援技術開発者、およびソフトウェアに関する情報を提供します。テストエンジニアは、アプリケーションをテストするための自動化されたスクリプトを作成します。

## <a name="platform-specific-documentation"></a>プラットフォーム固有ドキュメント

:::row:::
   :::column:::
      ![ユニバーサル Windows プラットフォーム (UWP)](images/platform-uwp.png)

      **ユニバーサル Windows プラットフォーム (UWP)**

      Windows デバイス (Pc、携帯電話、Xbox One、HoloLens など) 上の Windows 10 アプリケーションやゲーム用の最新プラットフォームで、アクセス可能なアプリとツールを開発し、それらを Microsoft Store に公開します。

      [包括的なソフトウェアの設計](https://docs.microsoft.com/windows/uwp/accessibility/designing-inclusive-software)

      [包括的な Windows アプリの開発](https://docs.microsoft.com/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [アクセシビリティ テスト](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-testing)

      [ストア内のアクセシビリティ](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 プラットフォームアプリ](images/platform-win32.png)

      **Win32 プラットフォーム**

      C/C++ Windows アプリケーションの元のプラットフォームで、アクセス可能なアプリとツールを開発します。

      [Windows のアクセシビリティと自動化の新機能](https://docs.microsoft.com/windows/desktop/accessibility-whatsnew)

      [Windows 用のアクセス可能なアプリケーションの開発](https://docs.microsoft.com/windows/desktop/accessibility-appdev)

      [Windows 用のアクセス可能な UI フレームワークの開発](https://docs.microsoft.com/windows/desktop/accessibility-uiframeworkdev)

      [Windows の支援技術の開発](https://docs.microsoft.com/windows/desktop/accessibility-atdev)

      [アクセシビリティのテスト](https://docs.microsoft.com/windows/desktop/accessibility-testwithuia)

      [従来のアクセシビリティとオートメーションテクノロジ-MSAA to UI Automation](https://docs.microsoft.com/windows/desktop/accessibility-legacy)

      [Windows ユーザー補助機能](https://docs.microsoft.com/windows/desktop/winauto/about-windows-accessibility-features)

      [アクセシビリティ対応アプリの設計のガイドライン](https://docs.microsoft.com/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF プラットフォーム](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      XAML UI モデルと .NET Framework を使用して、マネージ Windows アプリケーション用に確立されたプラットフォームで、アクセス可能なアプリとツールを開発します。

      [ユーザー補助のベストプラクティス](https://docs.microsoft.com/dotnet/framework/ui-automation/accessibility-best-practices)

      [UI オートメーションの基礎](https://docs.microsoft.com/dotnet/framework/ui-automation/index)

      [マネージコードの UI オートメーションプロバイダー](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [マネージコードの UI オートメーションクライアント](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI オートメーションコントロールパターン](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI オートメーションテキストパターン](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [UI オートメーションコントロール型](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-control-types)

      [UI オートメーション仕様とコミュニティの Promise](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Windows フォームプラットフォームアプリ](images/platform-winforms.png)

      **Windows フォーム (WinForms)**

      XAML UI モデルと .NET Framework を使用して、マネージ Windows アプリケーション用のアクセス可能なアプリとツールを開発します。

      [Windows フォームアクセシビリティ](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [ユーザー補助 Windows アプリケーションの作成](https://docs.microsoft.com/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [アクセシビリティガイドラインをサポートする Windows フォームコントロールのプロパティ](https://docs.microsoft.com/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Windows フォーム上のコントロールのユーザー補助情報の提供](https://docs.microsoft.com/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Web アクセシビリティ**

      Microsoft Edge で、アクセス可能な web サイトを設計、ビルド、テストします。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Web アクセシビリティの概要](https://docs.microsoft.com/microsoft-edge/accessibility)

      [アクセシビリティ対応 Web サイトの設計](https://docs.microsoft.com/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [アクセシビリティ対応 Web サイトの構築 ](https://docs.microsoft.com/microsoft-edge/accessibility/build)

      [アクセス可能な web サイトのテスト](https://docs.microsoft.com/microsoft-edge/accessibility/test)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>サンプル

さまざまなユーザー補助機能と機能を示す完全な Windows サンプルをダウンロードして実行します。

:::row:::
   :::column:::
      [コードサンプルブラウザー](https://docs.microsoft.com/en-us/samples/browse/)

      新しいサンプルブラウザーでは、MSDN コードギャラリーが置き換えられています。
   :::column-end:::
   :::column:::
      [MSDN コードギャラリー (廃止予定)](https://code.msdn.microsoft.com/site/search?query=accessibility&f%5B0%5D.Value=accessibility&f%5B0%5D.Type=SearchText&ac=2)

      Windows、Windows Phone、Microsoft Azure、Office、SharePoint、Silverlight などの製品のサンプルをダウンロードします。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub の Windows クラシックサンプル](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility)

      これらのサンプルでは、Windows および Windows Server の機能とプログラミングモデルについて説明します。 
   :::column-end:::
   :::column:::
      [GitHub のユニバーサル Windows プラットフォーム (UWP) のサンプル](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility)

      これらのサンプルでは、windows 10 用 Windows ソフトウェア開発キット (SDK) のユニバーサル Windows プラットフォーム (UWP) の API の使用パターンについて説明します。
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML コントロール ギャラリー](https://github.com/microsoft/Xaml-Controls-Gallery)

      このアプリは、Fluent デザインシステムでサポートされているさまざまな Xaml コントロールを示しています。
   :::column-end:::
:::row-end:::

## <a name="videos"></a>ビデオ

ユーザー補助機能に関する一般的な問題と、Microsoft がそれらに対処する方法を説明するさまざまなビデオです。

:::row:::
   :::column:::
      **Windows アプリのユーザー補助の使用を開始する方法**
   :::column-end:::
   :::column:::
      **1つの開発時間 (分):ユーザー補助用アプリの開発**
   :::column-end:::
   :::column:::
      **障碍とアクセシビリティの概要**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4072/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Kl4CT4DaypM]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **ハッキングから製品、Windows 10 の視線制御まで**
   :::column-end:::
   :::column:::
      **Windows 10 のユーザー補助機能**
   :::column-end:::
   :::column:::
      **アクセス可能な UWP アプリの構築の概要**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/AShNPfmAkvY]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P541/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2016/P497/player]
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      **Windows ユーザー補助機能の設計**
   :::column-end:::
   :::column:::
      **Windows 10 のユーザー補助機能によってすべてのユーザーが強化**
   :::column-end:::
   :::column:::
      **マウスポインターを見やすくする**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/Y_NJbE7wxlk]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/BseTf-4q9GA]
   :::column-end:::
   :::column:::
      > [!VIDEO https://www.youtube.com/embed/4UzaF7_T3bw]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>その他のリソース

:::row:::
   :::column span="3":::
      **ブログとニュース**

      Microsoft アクセシビリティの世界の最新バージョン。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [ニュース](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [アクセシビリティのブログ](https://blogs.microsoft.com/accessibility/)
   :::column-end:::
   :::column:::
      [Windows UI オートメーションのブログ](https://blogs.msdn.microsoft.com/winuiautomation/)
   :::column-end:::
:::row-end:::

:::row:::
   :::column span="3":::
      **コミュニティとサポート**

      Windows の開発者とユーザーが一緒に会って学習する場所。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows コミュニティ-アクセシビリティ](https://community.windows.com/search?q=accessibility)
   :::column-end:::
   :::column:::
      [Windows アクセシビリティと Automation の開発フォーラム](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation)
   :::column-end:::
   :::column:::
      [障碍の回答のデスク](https://www.microsoft.com/Accessibility/disability-answer-desk)
   :::column-end:::
:::row-end:::
