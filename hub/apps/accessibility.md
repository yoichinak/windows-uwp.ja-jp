---
title: Windows 10 のユーザー補助機能
description: このページでは、ユーザー補助 Windows アプリの開発を開始するための情報を提供します。
ms.topic: article
ms.date: 04/03/2019
ms.localizationpriority: medium
ms.author: kbridge
author: Karl-Bridge-Microsoft
keywords: Windows 10 のアクセシビリティ, アクセシビリティ, アクセス可能な win32 アプリの構築, アクセス可能な UWP アプリのビルド, アクセス可能な WPF アプリのビルド, アクセス可能な WinForms アプリの構築
ms.openlocfilehash: 63d9b321f71019c164fe4de238a618f2730b2052
ms.sourcegitcommit: 81213db9de2c12400d2e4d176346b466ee0794c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70237791"
---
# <a name="accessibility-in-windows-10"></a>Windows 10 のユーザー補助機能

![hero-accessibility-bar-smaller](images/hero-accessibility-bar-smaller.png)

アクセス可能なアプリは、可能な限り多くの人にとって使いやすさを向上させることによって設計されています。これには、障碍のあるユーザー、個人設定、特定の作業スタイル、状況に関する制約 (運転、料理、光など) などがあります。

このページでは、さまざまな Windows 開発フレームワークが Windows アプリケーションを構築する開発者のためのユーザー補助をサポートしています。スクリーンリーダーや拡大鏡などのツールを作成する支援技術の開発者、ソフトウェアテストアプリケーションをテストするための自動化されたスクリプトを作成するエンジニア。

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
      ![WPF プラットフォーム](images/platform-wpf.png)

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
