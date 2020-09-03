---
title: Windows 10 のアクセシビリティ
description: このページでは、アクセシビリティ対応 Windows アプリの開発を開始するための情報を提供します。
ms.topic: article
ms.date: 09/12/2019
keywords: Windows 10 のアクセシビリティ, アクセシビリティ, アクセシビリティ対応 win32 アプリの構築, アクセシビリティ対応 UWP アプリの構築, アクセシビリティ対応 WPF アプリの構築, アクセシビリティ対応 WinForms アプリの構築
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 7cdfa91bec648ab6f3d09d0aeff0d62e2962db86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169046"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 のアクセシビリティ

![アクセシビリティのヒーローの画像](images/hero-accessibility-bar-smaller.png)

## <a name="build-accessibility-into-your-applications-to-empower-people-of-all-abilities"></a>アプリケーションにアクセシビリティを組み込んで、あらゆる人々の能力を高めます

電子メディアを含む製品やサービスでアクセシビリティ対応を謳うための条件は、完全かつ成功した体験をできるだけ多くの人に提供するように設計されていることです。

(一時的か恒常的かを問わず) 障碍をお持ちの方、個人の趣向、特定の作業スタイル、状況による制約 (共有型の作業スペース、運転中、料理中、まぶしさなど) を考慮し、機能性と使いやすさを向上させることで、アクセシビリティに対応したインクルーシブな Windows アプリケーションを構築します。 一般的なソリューションとしては、ビデオの字幕などの代替形式で情報を提供することや、スクリーン リーダーなどのユーザー補助テクノロジの使用を有効化することなどがあります。

**階段やエレベーターを使う必要があるかどうかにかかわらず、誰もが建物内の同じ部屋に行ける必要があります。**

このページでは、さまざまな Windows 開発フレームワークにおいて、Windows アプリケーションの開発者、スクリーン リーダーや拡大鏡などのツールに代表されるユーザー補助テクノロジの開発者、また、アプリケーションをテストするための自動スクリプトを作成するソフトウェア テスト エンジニアに、アクセシビリティのサポートが提供されるしくみに関しての情報を提供します。

## <a name="platform-specific-documentation"></a>プラットフォームに特化したドキュメント

:::row:::
   :::column:::
      ![ユニバーサル Windows プラットフォーム (UWP)](images/platform-uwp.png)

      **ユニバーサル Windows プラットフォーム (UWP)**

      (PC、携帯電話、Xbox One、HoloLens などの) 各種 Windows デバイスで動作する Windows 10 のアプリケーションとゲームを開発するための最新プラットフォームで、アクセシビリティに対応したアプリやツールを開発し、Microsoft Store に公開します。

      [包括的なソフトウェアの設計](/windows/uwp/accessibility/designing-inclusive-software)

      [包括的な Windows アプリの開発](/windows/uwp/accessibility/developing-inclusive-windows-apps)

      [アクセシビリティ テスト](/windows/uwp/accessibility/accessibility-testing)

      [ストア内のアクセシビリティ](/windows/uwp/accessibility/accessibility-in-the-store)
   :::column-end:::
   :::column:::
      ![Win32 プラットフォーム アプリ](images/platform-win32.png)

      **Win32 プラットフォーム**

      C/C++ Windows アプリケーションの元のプラットフォームで、アクセシビリティに対応したアプリやツールを開発します。

      [Windows のアクセシビリティとオートメーションの新機能](/windows/desktop/accessibility-whatsnew) (英語)

      [Windows 向けのアクセシビリティ対応アプリケーションの開発](/windows/desktop/accessibility-appdev) (英語)

      [Windows 向けのアクセシビリティ対応 UI フレームワークの開発](/windows/desktop/accessibility-uiframeworkdev) (英語)

      [Windows 向けのユーザー補助テクノロジの開発](/windows/desktop/accessibility-atdev) (英語)

      [アクセシビリティのテスト](/windows/desktop/accessibility-testwithuia) (英語)

      [従来のアクセシビリティおよびオートメーション テクノロジ - MSAA から UI オートメーションまで](/windows/desktop/accessibility-legacy) (英語)

      [Windows のアクセシビリティ機能](/windows/desktop/winauto/about-windows-accessibility-features) (英語)

      [アクセシビリティ対応アプリの設計のガイドライン](/windows/desktop/uxguide/inter-accessibility)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![WPF プラットフォーム](images/platform-wpf2-small.png)

      **Windows Presentation Foundation (WPF)**

      Windows マネージド アプリケーション用として実績のあるプラットフォームで、XAML UI モデルと .NET Framework を使用して、アクセシビリティに対応したアプリやツールを開発します。

      [ユーザー補助のベスト プラクティス](/dotnet/framework/ui-automation/accessibility-best-practices)

      [UI オートメーションの基礎](/dotnet/framework/ui-automation/index)

      [マネージド コードの UI オートメーション プロバイダー](/dotnet/framework/ui-automation/ui-automation-providers-for-managed-code)

      [マネージド コードの UI オートメーション クライアント](/dotnet/framework/ui-automation/ui-automation-clients-for-managed-code)

      [UI オートメーション コントロール パターン](/dotnet/framework/ui-automation/ui-automation-control-patterns)

      [UI オートメーション テキスト パターン](/dotnet/framework/ui-automation/ui-automation-text-pattern)

      [UI オートメーション コントロール型](/dotnet/framework/ui-automation/ui-automation-control-types)

      [UI Automation Specification および UI Automation Community Promise](/dotnet/framework/ui-automation/ui-automation-specification-and-community-promise)
   :::column-end:::
   :::column:::
      ![Windows フォーム プラットフォーム アプリ](images/platform-winforms.png)

      **Windows フォーム (WinForms)**

      XAML UI モデルと .NET Framework を使用して、Windows マネージド アプリケーション用の、アクセシビリティに対応したアプリやツールを開発します。

      [Windows フォームのアクセシビリティ](/dotnet/framework/winforms/advanced/windows-forms-accessibility)

      [アクセシビリティ対応の Windows アプリケーションの作成](/dotnet/framework/winforms/advanced/walkthrough-creating-an-accessible-windows-based-application)

      [ユーザ補助ガイドラインをサポートする Windows フォーム コントロールのプロパティ](/dotnet/framework/winforms/advanced/properties-on-windows-forms-controls-that-support-accessibility-guidelines)

      [Windows フォーム上のコントロールのユーザー補助情報の提供](/dotnet/framework/winforms/controls/providing-accessibility-information-for-controls-on-a-windows-form)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Web アクセシビリティ**

      アクセシビリティに対応した Web サイトを設計、構築し、Microsoft Edge でテストします。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Web アクセシビリティについて](/microsoft-edge/accessibility) (英語)

      [アクセシビリティ対応 Web サイトの設計](/microsoft-edge/accessibility/design)
   :::column-end:::
   :::column:::
      [アクセシビリティ対応 Web サイトの構築](/microsoft-edge/accessibility/build)

      [アクセシビリティ対応 Web サイトのテスト](/microsoft-edge/accessibility/test) (英語)
   :::column-end:::
:::row-end:::

## <a name="samples"></a>サンプル

アクセシビリティのためのさまざまな特徴や機能の例を示す、完全な Windows サンプルをダウンロードして実行します。

:::row:::
   :::column:::
      [コード サンプル ブラウザー](/samples/browse/)

      MSDN コード ギャラリーに代わる新しいサンプル ブラウザーです。
   :::column-end:::
   :::column:::
      [MSDN コード ギャラリー (GitHub アーカイブ)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample)

      Windows、Windows Phone、Microsoft Azure、Office、SharePoint、Silverlight、その他の製品に関するサンプルをダウンロードします。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [GitHub 上の Windows クラシック サンプル](https://github.com/microsoft/Windows-classic-samples/search?q=accessibility&unscoped_q=accessibility) (英語)

      これらのサンプルは、Windows および Windows Server の機能とプログラミング モデルの例を示しています。 
   :::column-end:::
   :::column:::
      [GitHub 上のユニバーサル Windows プラットフォーム (UWP) サンプル](https://github.com/microsoft/Windows-universal-samples/search?q=accessibility&unscoped_q=accessibility) (英語)

      これらのサンプルでは、Windows 10 用の Windows ソフトウェア開発キット (SDK) における、ユニバーサル Windows プラットフォーム (UWP) のための API 使用パターンの例を示しています。
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      [XAML コントロール ギャラリー](https://github.com/microsoft/Xaml-Controls-Gallery)

      このアプリは、Fluent Design System でサポートされているさまざまな Xaml コントロールの例を示しています。
   :::column-end:::
:::row-end:::

## <a name="videos"></a>ビデオ

アクセシビリティ対応 Windows アプリケーションの構築方法、アクセシビリティに関する一般的な課題、それらの課題に対する Microsoft の取り組みなどをビデオで紹介しています。

:::row:::
   :::column:::
      **Windows アプリのアクセシビリティ導入入門** (英語)
   :::column-end:::
   :::column:::
      **One Dev Minute: アクセシビリティのためのアプリ開発** (英語)
   :::column-end:::
   :::column:::
      **障碍とアクセシビリティの概要** (英語)
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
      **ハッキングから製品へ: Windows 10 の視線制御** (英語)
   :::column-end:::
   :::column:::
      **Windows 10 のアクセシビリティ** (英語)
   :::column-end:::
   :::column:::
      **アクセシビリティ対応 UWP アプリ構築入門** (英語)
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
      **Windows のアクセシビリティ機能の設計** (英語)
   :::column-end:::
   :::column:::
      **Windows 10 のアクセシビリティ機能: すべての人のために** (英語)
   :::column-end:::
   :::column:::
      **マウス ポインターを見やすくする** (英語)
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

      Microsoft アクセシビリティの世界からの最新情報。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [関連ニュース](https://news.microsoft.com/presskits/accessibility/)
   :::column-end:::
   :::column:::
      [アクセシビリティのブログ](https://blogs.microsoft.com/accessibility/) (英語)
   :::column-end:::
   :::column:::
      [Windows UI オートメーションのブログ](/archive/blogs/winuiautomation/) (英語)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **コミュニティとサポート**

      Windows の開発者とユーザーが意見を交わし、共に学ぶための場所です。
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Windows コミュニティ - アクセシビリティ](https://community.windows.com/search?q=accessibility) (英語)
   :::column-end:::
   :::column:::
      [Windows のアクセシビリティとオートメーション: 開発フォーラム](https://social.msdn.microsoft.com/Forums/windows/home?forum=windowsaccessibilityandautomation) (英語)
   :::column-end:::
   :::column:::
      [障碍のある方のためのアンサー デスク](https://www.microsoft.com/Accessibility/disability-answer-desk) (英語)
   :::column-end:::
:::row-end:::