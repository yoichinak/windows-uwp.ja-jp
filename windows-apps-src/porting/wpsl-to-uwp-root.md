---
description: If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10.
title: Move from Windows Phone Silverlight to UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d37e40d58d8825d4361efbd10108c1169b8df87f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260091"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Move from Windows Phone Silverlight to UWP


If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10. With Windows 10, you can create a Universal Windows Platform (UWP) app, which is a single app package that your customers can install onto every kind of device. For more background on Windows 10, UWP apps, and the concepts of adaptive code and adaptive UI that we'll mention in this porting guide, see the [Guide to Universal Windows Platform (UWP) apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

When you port your Windows Phone Silverlight app to a Windows 10 app, you'll be able to catch up on the mobile features that were [introduced in Windows Phone 8.1](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105)), and go far beyond them to use the Universal Windows Platform (UWP) whose app model and UI framework are universal across all Windows 10 devices. これにより、1 つのコード ベースから、1 つのアプリ パッケージで、PC、タブレット、電話、その他の多くの種類のデバイスをサポートできます。 また、これによりアプリの潜在顧客が増加し、共有データ、購入コンシューマブルなどの可能性が高まります。 For more info on new features, see [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest).

If you choose to, the Windows Phone Silverlight version of your app and the Windows 10 version of it can both be available to customers at the same time.

**Note**  This guide is designed to help you port your Windows Phone Silverlight app to Windows 10 manually. このガイドの情報を使って、アプリを移植できるだけではありません。移植プロセス自動化支援用に提供された **Mobilize.NET の Silverlight Bridge** の開発者プレビューを試すこともできます。 This tool analyzes your app's source code and converts references to Windows Phone Silverlight controls and APIs to their UWP counterparts. このツールは、まだ開発者プレビューの段階にあるため、すべての変換シナリオを扱えるとは限りません。 ただし、ほとんどの開発者はこのツールを使い始めることで、時間と労力をいくらかは節約できます。 開発者プレビューを試すには、[Mobilize.NET の Web サイト](https://www.mobilize.net/uwp-bridge)をご覧ください。

## <a name="xaml-and-net-or-html"></a>XAML と .NET、または HTML

Windows Phone Silverlight has a XAML UI framework based on Silverlight 4.0, and you program against a version of the .NET Framework and a small subset of UWP APIs. Since you used Extensible Application Markup Language (XAML) in your Windows Phone Silverlight app, it's likely that XAML will be your choice for your Windows 10 version because most of your knowledge and experience will transfer, as will much of your source code and the software patterns you use. UI のマークアップと設計であっても容易に移植できます。 マネージ API、XAML マークアップ、UI フレームワーク、ツールはすべて使い慣れたものであり、UWP アプリでは、XAML と共に C++、C#、Visual Basic を使うことができます。 若干の課題が発生するとしても、そのプロセスの相対的な容易さに驚くはずです。

「[C# または Visual Basic を使ったユニバーサル Windows プラットフォーム (UWP) アプリのためのロードマップ](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))」をご覧ください。

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets. So, now is a great time to port your Windows Phone Silverlight and have your .NET code just compile and work on the new platform. 「[名前空間とクラス マッピング](wpsl-to-uwp-namespace-and-class-mappings.md)」をご覧ください。
Another great reason to recompile your existing .NET source code into a Windows 10 app is that you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. .NET ネイティブ アプリは、MSIL アプリに比べて、すばやく起動し、メモリ使用量やバッテリ使用量は少なくなります。

この移植ガイドでは XAML に重点を置いていますが、JavaScript 用 Windows ライブラリと共に、JavaScript、カスケード スタイル シート (CSS)、HTML5 を使って、同じ UWP API の多くを呼び出す機能的に同等のアプリを構築できます。 XAML と HTML の Windows ランタイム UI フレームワークは相互に異なりますが、いずれを選んでも一連のさまざまな Windows デバイスで汎用的に動作します。

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>ターゲットとしてのユニバーサル デバイス ファミリまたはモバイル デバイス ファミリの設定

選択肢の 1 つは、ユニバーサル デバイス ファミリをターゲットとするアプリにアプリを移植することです。 この場合、アプリは多様なデバイスにインストールできます。 アプリがモバイル デバイス ファミリにのみ実装されている API を呼び出す場合は、これらの呼び出しをアダプティブ コードで保護できます。 また、モバイル デバイス ファミリをターゲットとするアプリにアプリを移植することができ、この場合、アダプティブ コードを記述する必要はありません。

## <a name="adapting-your-app-to-multiple-form-factors"></a>複数のフォーム ファクターへのアプリの対応

前のセクションで選択した選択肢によって、アプリが実行されるデバイスの種類が決まります。これは、非常に多種多様なデバイスである場合もあります。 アプリをモバイル デバイス ファミリに制限した場合でも、さまざまな画面サイズをサポートする必要があります。 以前はサポートしていなかったフォーム ファクターでアプリが実行されるため、これらのフォーム ファクターで UI をテストし、必要なすべての変更を加えて、UI が各フォーム ファクターに適切に対応するようにします。 これは、移植後のタスクまたは移植の拡張目標と考えることができます。これについての実践的な例が「[Bookstore2](wpsl-to-uwp-case-study-bookstore2.md)」のケース スタディに紹介されています。

## <a name="approaching-porting-layer-by-layer"></a>レイヤーごとの移植アプローチ

-   **ビュー**。 ビューは (ビュー モデルと共に)、アプリの UI を構成します。 理想的にはビューは、ビュー モデルの監視可能なプロパティに対するマークアップ バインドから成ります。 また、ビュー モデルのもう 1 つのパターンとして、UI 要素を直接操作する分離コード ファイル内の命令型コード用のビュー モデルがあります (このようなビュー モデルは一般的で使いやすいビュー モデルですが、使われるのは短期間です)。 いずれのケースでも、UI マークアップと設計の多く、そして UI 要素を操作するために不可欠なコードについても、簡単に移植できます。
-   **ビュー モデルとデータ モデル**。 形式的な懸念事項分離パターン (MVVM など) を取り入れていなくても、ビュー モデルおよびデータ モデルの関数を実行するコードがアプリ内に必然的に存在します。 ビュー モデル コードでは、UI フレームワークの名前空間内の型を使います。 また、ビュー モデルおよびデータ モデル コードは共に、非視覚的なオペレーティング システム API および .NET API を使います (データ アクセス用の API を含みます)。 このようなコードの大半が [UWP アプリで利用可能であり](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))、したがって変更なくコードの大半を移植できると考えられます。 ただし、ビュー モデルは、ビューのモデルまたは*アブストラクション*であることに注意してください。 ビュー モデルは UI の状態と動作を提供するのに対して、ビューそれ自体は視覚効果を提供します。 このような理由から、UWP で実行可能な異なるフォーム ファクターに適合するどのような UI でも、おそらく対応するビュー モデルの変更が必要になります。 ネットワークとクラウド サービスの呼び出しについては、通常、.NET API と UWP API のいずれを使うかを選択できます。 この決定に関連する要因については、「[クラウド サービス、ネットワーク、データベース](wpsl-to-uwp-business-and-data.md)」をご覧ください。
-   **クラウド サービス**。 アプリのいくつか (おそらく、その多く) が、サービスの形式でクラウド内で実行されます。 クライアント デバイス上で実行する一部のアプリは、こうしたアプリに接続します。 これは、クライアント部分の移植時に、変化しない可能性が最も高い分散アプリの部分です。 クラウド サービスをまだ利用していない場合は、UWP アプリに適したクラウド サービス オプションとして [Microsoft Azure Mobile Services](https://azure.microsoft.com/services/mobile-services/) があります。これは、ライブ タイル更新のための簡単な通知から、サーバー ファームが提供する高負荷のスケーラビリティに至るまで、さまざまなサービスのためにユニバーサル Windows アプリから呼び出すことができる強力なバックエンド コンポーネントを提供します。

移植前または移植中に、同様の目的を持つコードがレイヤー内に集められ、随意に散在しないように、アプリがリファクタリングによって向上するかどうかを考慮します。 前述したように、UWP アプリをレイヤーにファクタリングすることで、アプリの適合性、テスト、以降の読み取りと維持が容易になります。 Model-View-ViewModel ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)) パターンに従うことにより、機能の再利用性を高め、プラットフォーム間の UI API の相違によるいくつかの問題を回避できます。 このパターンにより、アプリのデータ部、ビジネス部、UI 部の相互の分離性が維持されます。 UI 内であっても、状態と動作を別個に維持し、視覚効果から個別にテスト可能にすることができます。 MVVM により、データおよびビジネス ロジックを 1 回記述すれば、UI に関係なく、それをすべてのデバイスで使うことができます。 各デバイスで、ビュー モデルとビュー部品の多くを再利用できる可能性があります。

## <a name="one-or-two-exceptions-to-the-rule"></a>いくつかの例外

この移植ガイドでは、「[名前空間とクラス マッピング](wpsl-to-uwp-namespace-and-class-mappings.md)」も随時ご覧ください。 非常に簡単なマッピングが一般的な規則ですが、名前空間とクラス マッピング表にすべての例外が示されています。

機能レベルでは、さいわいなことに、UWP でサポートされないものは非常にわずかです。 この移植ガイドの以降の部分では、自身のスキル セットとソース コードの大半を UWP アプリで有効に活用できます。 But, here are the few Windows Phone Silverlight features that you may have used for which there is no UWP equivalent.

| UWP に相当要素がない機能 | Windows Phone Silverlight documentation for the feature |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA。 一般に、C++ を使った [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) に置き換えられます。 「[ゲームの開発](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))」と「[DirectX と XAML の相互運用機能](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))」をご覧ください。 | [XNA Framework Class Library](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|レンズ アプリ | [Lenses for Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| トピック| 説明|
|------|------------| 
| [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md) | This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their UWP equivalents. |
| [Porting the project](wpsl-to-uwp-porting-to-a-uwp-project.md) | You begin the porting process by creating a new Windows 10 project in Visual Studio and copying your files into it. |
| [トラブルシューティング](wpsl-to-uwp-troubleshooting.md) | この移植ガイドは最後まで読むことを強くお勧めしますが、早く先へ進んで、プロジェクトのビルドと実行の段階まで到達したいと思われるのも無理のないことです。 このために、重要でないコードに対してコメント アウトやスタブの挿入を行って一時的に先に進み、後でその部分に戻って対応することもできます。 このトピックには、トラブルシューティングの現象とその対処法を示す表が記載されており、以降のいくつかのトピックに示されている情報に代わるものではありませんが、この段階での作業に役立ちます。 以降のトピックを読み進む中で、いつでもこの表に戻って参考にすることができます。 |
| [Porting XAML and UI](wpsl-to-uwp-porting-xaml-and-ui.md) | The practice of defining UI in the form of declarative XAML markup translates extremely well from Windows Phone Silverlight to UWP apps. システムのリソース キーの参照の更新、いくつかの要素型名の変更、"clr-namespace" から "using" への変更を行うことによって、大きなマークアップ セクションで互換性が得られることがわかります。 |
| [Porting for I/O, device, and app model](wpsl-to-uwp-input-and-sensors.md) | デバイス自体とそのセンサーに統合するコードには、ユーザーに対する入力と出力が含まれます。 また、データ処理を含むこともあります。 ただしこのコードは一般には、UI レイヤーまたはデータ レイヤーのいずれにも見なされません。 このコードには、振動コントローラー、加速度計、ジャイロスコープ、マイクとスピーカー (音声認識と音声合成で使います)、地理位置情報、およびタッチ、マウス、キーボード、ペンなどの入力モダリティとの統合が含まれます。 |
| [Porting business and data layers](wpsl-to-uwp-business-and-data.md) | UI の背後には、ビジネス レイヤーとデータ レイヤーがあります。 こうしたレイヤーのコードでは、オペレーティング システムと .NET Framework API (たとえば、バックグラウンド処理、場所、カメラ、ファイル システム、ネットワーク、その他のデータ アクセスなど) を呼び出します。 このようなコードの大半が [UWP アプリで利用可能であり](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))、したがって変更なくコードの大半を移植できると考えられます。 |
| [Porting for form factor and UX](wpsl-to-uwp-form-factors-and-ux.md) | Windows アプリは、PC、モバイル デバイス、その他の多くの種類のデバイスで同じ外観を共有します。 ユーザー インターフェイス、入力パターン、操作パターンは非常に類似しており、デバイス間を移行するユーザーには使い慣れたエクスペリエンスは歓迎されるはずです。|
|[Case study: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | This topic presents a case study of porting a very simple Windows Phone Silverlight app to a Windows 10 UWP app. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. |
| [Case study: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | This case study—which builds on the info given in [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)—begins with a Windows Phone Silverlight app that displays grouped data in a **LongListSelector**. ビュー モデルでは、**Author** クラスの各インスタンスは、該当する著者によって書かれた書籍のグループを表します。**LongListSelector** では、著者ごとにグループ化された書籍の一覧を表示したり、縮小して著者のジャンプ リストを表示したりすることができます。 |

## <a name="related-topics"></a>関連トピック

**ドキュメント**
* [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [ユニバーサル Windows プラットフォーム (UWP) アプリのガイド](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Roadmap for Universal Windows Platform (UWP) apps using C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [Windows Phone 8 開発者にとっての選択肢](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**MSDN マガジンの記事**
* [Visual Studio Magazine の「Windows Phone 8.1 における集約への大きな前進」に関する記事](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**プレゼンテーション**
* [The Story of Bringing Nokia Music from Windows Phone to Windows 8](https://channel9.msdn.com/Events/Build/2013/2-219)
 

