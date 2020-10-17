---
description: Windows ランタイム8.x から UWP に移植するときに発生する可能性のある問題のトラブルシューティング
title: Windows ランタイム 8.x から UWP への移植のトラブルシューティング'
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0e915aec7f37600ce821f1a097a8aa5ac7ca76b
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133105"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Windows ランタイム 8.x から UWP への移植のトラブルシューティング


前のトピックは、「[プロジェクトの移植](w8x-to-uwp-porting-to-a-uwp-project.md)」でした。

この移植ガイドは最後まで読むことを強くお勧めしますが、早く先へ進んで、プロジェクトのビルドと実行の段階まで到達したいと思われるのも無理のないことです。 このために、重要でないコードに対してコメント アウトやスタブの挿入を行って一時的に先に進み、後でその部分に戻って対応することもできます。 このトピックには、トラブルシューティングの現象とその対処法を示す表が記載されており、以降のいくつかのトピックに示されている情報に代わるものではありませんが、この段階での作業に役立ちます。 以降のトピックを読み進む中で、いつでもこの表に戻って参考にすることができます。

## <a name="tracking-down-issues"></a>問題の検出

XAML 解析例外は診断が難しい場合があります。特に、わかりやすいエラー メッセージが例外に含まれていない場合は、診断が難しくなります。 デバッガーが初回例外をキャッチするように構成されていることを確してください (早い段階で解析例外のキャッチを試行するため)。 デバッガーで例外変数を調べて、HRESULT やメッセージ内に役立つ情報が含まれているかどうかを確認できます。 また、XAML パーサーを使って、Visual Studio の出力ウィンドウを調べ、エラー メッセージの出力を確認することもできます。

アプリが終了したとき、確認できたことが、ハンドルされていない例外が XAML マークアップの解析中にスローされたことのみである場合は、存在しないリソース (システム **TextBlock** スタイル キーなどのキーが Windows 10 アプリには存在せず、ユニバーサル 8.1 アプリに存在するリソース) への参照が原因であると考えられます。 または、 **UserControl**、カスタムコントロール、またはカスタムレイアウトパネル内でスローされる例外も考えられます。

最終手段として、バイナリ分割を使うことができます。 ページからマークアップのおよそ半分を削除し、アプリを再実行します。 その後、削除した半分 (任意のケースで復元する必要があります) また *は削除しなかった* 半分のいずれかの場所にエラーがあるかどうかがわかります。 問題が特定されるまで、エラーを含む半分をさらに分割するプロセスを繰り返します。

## <a name="targetplatformversion"></a>TargetPlatformVersion

このセクションでは、Visual Studio で Windows 10 プロジェクトを開いたとき、"Visual Studio 更新プログラムが必要。 1 つ以上のプロジェクトでは、インストールされていないか、Visual Studio に対する今後の更新の一部として含まれるプラットフォーム SDK \<version\> が必要です。"

-   まず、インストールされている SDK for Windows 10 のバージョン番号を確認します。 **C: \\ Program Files (x86) \\ Windows kit \\ 10 \\ が含ま \\ \<versionfoldername\> **れていることを確認し *\<versionfoldername\>* ます。これは、"Major. Minor. ビルド. Revision" という4つの表記で構成されています。
-   編集用のプロジェクト ファイルを開き、`TargetPlatformVersion` 要素と `TargetPlatformMinVersion` 要素を探します。 次のように編集し *\<versionfoldername\>* ます。は、ディスク上で検出されたクワッド表記のバージョン番号に置き換えます。

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>各現象のトラブルシューティングと対処法

この表の対処法に関する情報では、自身で問題を解消するために十分な情報を提供することが意図されています。 以降のトピックを読み進むことで、こうした各問題についてさらに詳細な情報が提供されます。

| 症状 | 解決方法 |
|---------|--------|
| Visual Studio で Windows 10 プロジェクトを開いたとき、"Visual Studio 更新プログラムが必要。 1 つ以上のプロジェクトでは、インストールされていないか、Visual Studio に対する今後の更新の一部として含まれるプラットフォーム SDK &lt;バージョン&gt; が必要です。" というメッセージが表示されます。 | このトピックの「[TargetPlatformVersion](#targetplatformversion)」セクションをご覧ください。 |
| InitializeComponent が xaml.cs ファイルで呼び出されると、System.InvalidCastException がスローされます。| これは、同じ xaml.cs ファイルを共有している xaml ファイルが複数あり (少なくとも 1 つは MRT 修飾されたファイル)、要素が持つ x:Name 属性が 2 つの xaml ファイル間で整合性がとれていない場合に発生することがあります。 両方の xaml ファイルで同じ要素に同じ名前を追加してみるか、どちらの名前も省略してください。 |
| デバイスで実行すると、アプリが終了するか、Visual Studio から起動すると、"Windows ランタイム 8. x アプリをアクティブにできませ \[ ん.. \] ." というエラーが表示されます。 アクティベーション要求がエラー "Windows はターゲット アプリケーションと通信できませんでした。 これは通常、ターゲット アプリケーションのプロセスが中止したことを示します。 \[…\]”. | この問題は、初期化時に独自のページ、またはバインド プロパティ (またはその他の型) で実行する命令型コードにあることが考えられます。 また、アプリの終了時に表示される XAML ファイルの解析でエラーが発生した可能性があります (Visual Studio から起動した場合は、これは起動ページになります)。 無効なリソース キーを検索するか、このトピックの「問題の検出」セクションのガイダンスを実行します。|
| XAML のパーサーやコンパイラ、またはランタイム例外で、"*リソース "\<resourcekey\>" を解決できませんでした*" というエラーが発生します。 | リソース キーは、ユニバーサル Windows プラットフォーム (UWP) アプリには適用されません (たとえば、一部の Windows Phone リソースがこの場合に該当します)。 同等の正しいリソースを探し、マークアップを更新します。 発生する可能性が高い例としては、`PhoneAccentBrush` などのシステム キーが挙げられます。 |
| C# コンパイラでは、"*型または名前空間の名前 ' ' \<name\> が見つかりませんでした \[ \] *..." または "名前空間名 ' ' が* \<name\> 名前空間 \[ .. \] .*" または "*現在の \<name\> コンテキストに存在*しません" というエラーが発生します。 | これは、型が拡張 SDK に実装されていることを示していると考えられます (ただし、この場合、対処法が難しくなる可能性があります)。 [Windows api](/uwp/)のリファレンスコンテンツを使用して、API を実装する拡張 SDK を特定し、 **Add**Visual Studio の [  >  **参照**の追加] コマンドを使用してその sdk への参照をプロジェクトに追加します。 アプリがユニバーサル デバイス ファミリと呼ばれる一連の API をターゲットとしている場合は、API を呼び出す前に、[**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) クラスを使って、実行時にテストを行い、拡張 SDK の有無を確認する必要があります (これはアダプティブ コードと呼ばれます)。 ユニバーサル API が存在する場合、拡張 SDK の API では常にこれが推奨されます。 詳しくは、「[拡張 SDK](w8x-to-uwp-porting-to-a-uwp-project.md)」をご覧ください。 |

次のトピックは、「[XAML と UI の移植](w8x-to-uwp-porting-xaml-and-ui.md)」です。