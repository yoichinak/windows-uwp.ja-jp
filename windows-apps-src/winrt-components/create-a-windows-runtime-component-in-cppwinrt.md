---
title: C++/WinRT を使用した Windows ランタイム コンポーネント
description: このトピックでは、 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用し &mdash; て、任意の Windows ランタイム言語を使用して構築されたユニバーサル Windows アプリから呼び出すことができるコンポーネントを、Windows ランタイムコンポーネントを作成および使用する方法について説明します。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、runtime、component、components、Windows ランタイム Component、WRC、C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: adf13308b1a2c360d7db53ded4edfe866de6c260
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220315"
---
# <a name="windows-runtime-components-with-cwinrt"></a>C++/WinRT を使用した Windows ランタイム コンポーネント

このトピックでは、 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用し &mdash; て、任意の Windows ランタイム言語を使用して構築されたユニバーサル Windows アプリから呼び出すことができるコンポーネントを、Windows ランタイムコンポーネントを作成および使用する方法について説明します。

C++/winrtで Windows ランタイムコンポーネントを構築するには、いくつかの理由があります。
- C++ のパフォーマンス上の利点を、複雑な操作や計算量の多い操作で利用できます。
- 既に記述およびテストされている標準 C++ コードを再利用する場合。
- C# など、で記述されたユニバーサル Windows プラットフォーム (UWP) アプリに Win32 機能を公開すること。

一般に、C++/WinRT コンポーネントを作成する場合は、標準 C++ ライブラリの型と組み込み型を使用できます。ただし、別のパッケージのコードとの間でデータをやり取りしているアプリケーションバイナリインターフェイス (ABI) の境界は除き `.winmd` ます。 ABI では Windows ランタイム型を使用します。 さらに、C++/WinRT コードでは、デリゲートやイベントなどの型を使用して、コンポーネントから発生させ、別の言語で処理できるイベントを実装します。 C++/winrtの詳細については、「 [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 」を参照してください。

このトピックの残りの部分では、C++/WinRT で Windows ランタイムコンポーネントを作成する方法と、それをアプリケーションから使用する方法について説明します。

このトピックで作成する Windows ランタイムコンポーネントには、銀行口座を表すランタイムクラスが含まれています。 また、このトピックでは、bank account runtime クラスを使用して、残高を調整する関数を呼び出すコアアプリについても説明します。

> [!NOTE]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

> [!IMPORTANT]
> C++/WinRT でランタイム クラスを使用および作成する方法についての理解をサポートするために重要な概念と用語については、「[C++/WinRT での API の使用](../cpp-and-winrt-apis/consume-apis.md)」と「[C++/WinRT での作成者 API](../cpp-and-winrt-apis/author-apis.md)」を参照してください。

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Windows ランタイム コンポーネントの作成 (BankAccountWRC)

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **Windows ランタイム コンポーネント (C++/WinRT)** プロジェクトを作成し、("銀行口座 Windows ランタイム コンポーネント" 用に) *BankAccountWRC* と名前を付けます。 **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。 プロジェクトに *BankAccountWRC* という名前を付けると、このトピックの残りの手順が非常に簡単になります。 

プロジェクトはまだビルドしないでください。

新しく作成したプロジェクトには、`Class.idl` という名前のファイルが含まれています。 ソリューション エクスプローラーで、そのファイル `BankAccount.idl` の名前を変更します (`.idl` ファイルの名前を変更すると、依存ファイル `.h` および `.cpp` も自動的に名前変更されます)。 `BankAccount.idl` の内容を、以下のリストで置き換えます。

> [!NOTE]
> 言うまでもありませんが、この方法で運用財務ソフトウェアを実装することはありません。便宜上、 `Single` この例ではを使用します。

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

ファイルを保存します。 現時点ではプロジェクトは完成するまでビルドされませんが、ここでビルドすることは有用です。それによって、**BankAccount** ランタイム クラスを実装するためのソース コード ファイルが生成されるからです。 それでは、今すぐビルドしてみましょう (この段階で表示されることが予想されるビルド エラーは、`Class.h` と `Class.g.h` が見つからないことに関係しています)。

ビルド プロセス中に、`midl.exe` ツールが実行されてコンポーネントの Windows ランタイム メタデータ ファイルが作成されます (これは `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` です)。 次に、`cppwinrt.exe` ツールが (`-component` オプションで) 実行され、コンポーネントの作成をサポートするソース コード ファイルが生成されます。 これらのファイルには、IDL で宣言した **BankAccount** ランタイム クラスの実装を開始するためのスタブが含まれています。 これらのスタブは `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` と `BankAccount.cpp` です。

プロジェクト ノードを右クリックし、 **[エクスプローラーでフォルダーを開く]** をクリックします。 これにより、エクスプローラーでプロジェクト フォルダーが開きます。 そこで、スタブ ファイル `BankAccount.h` および `BankAccount.cpp` をフォルダー `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` から、ご自分のプロジェクト ファイルを含むフォルダー `\BankAccountWRC\BankAccountWRC\` にコピーし、コピー先のファイルを置き換えます。 ここで、`BankAccount.h` と `BankAccount.cpp` を開いてランタイム クラスを実装してみましょう。 で `BankAccount.h` 、 **BankAccount**の実装 (ファクトリ実装では*なく*) に新しいプライベートメンバーを追加します。

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

で `BankAccount.cpp` 、次の一覧に示すように **AdjustBalance** メソッドを実装します。

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

また、両方のファイルから `static_assert` を削除する必要もあります。

任意の警告によってビルドが妨げられる場合は、該当する警告を解決するか、またはプロジェクトのプロパティ **C/C++**  > **General** > **Treat Warnings As Errors** を **No (/WX-)** に設定して、もう一度プロジェクトをビルドします。

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>コア アプリ (BankAccountCoreApp) を作成して Windows ランタイム コンポーネントをテストする

ここで (*BankAccountWRC* ソリューション、または新しいソリューションのいずれかに) 新しいプロジェクトを作成します。 **コア アプリ (C++/WinRT)** プロジェクトを作成し、*BankAccountCoreApp* と名前を付けます。 2 つのプロジェクトが同じソリューションに属している場合は、*BankAccountCoreApp* をスタートアップ プロジェクトとして設定します。

> [!NOTE]
> 既に説明したように、Windows ランタイム コンポーネント (そのプロジェクト名は *BankAccountWRC*) の Windows ランタイム メタデータ ファイルは `\BankAccountWRC\Debug\BankAccountWRC\` フォルダーに作成されます。 このパスの最初のセグメントは、ソリューション ファイルを含むフォルダーの名前です。次のセグメントは、`Debug` という名前のサブディレクトリです。最後のセグメントは、Windows ランタイム コンポーネントの名前を付けられたサブディレクトリです。 プロジェクトに *BankAccountWRC* という名前を指定しなかった場合、メタデータ ファイルは `\<YourProjectName>\Debug\<YourProjectName>\` フォルダーに配置されます。

次に、コア アプリ プロジェクト (*BankAccountCoreApp*) で、参照を追加し、`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` を参照します (または 2 つのプロジェクトが同じソリューションに属している場合は、プロジェクト間の参照を追加します)。 **[追加]** をクリックして **[OK]** をクリックします。 ここで *BankAccountCoreApp* をビルドします。 万が一、ペイロード ファイル `readme.txt` が存在しないというエラーが表示された場合、Windows ランタイム コンポーネント プロジェクトからそのファイルを除外し、これを再ビルドしてから、*BankAccountCoreApp* を再ビルドします。

ビルド プロセス中に、`cppwinrt.exe` ツールが実行され、参照されている `.winmd` ファイルを投影型を含むソース コード ファイルに処理して、コンポーネントの使用をサポートします。 コンポーネントのランタイム クラス (`BankAccountWRC.h` という名前) の投影型のヘッダーは、フォルダー `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` 内に生成されます。

そのヘッダーを `App.cpp` に含めます。

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

また `App.cpp` 、次のコードを追加して、 **BankAccount** オブジェクトをインスタンス化し (射影された型の既定のコンストラクターを使用)、銀行口座オブジェクトのメソッドを呼び出します。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

ウィンドウをクリックするたびに、銀行口座オブジェクトの残高が増加します。 コードをステップ実行して、アプリケーションが実際に Windows ランタイムコンポーネントを呼び出していることを確認する場合は、ブレークポイントを設定できます。

## <a name="next-steps"></a>次の手順

C++/WinRT Windows ランタイムコンポーネントにさらに多くの機能または新しい Windows ランタイムの種類を追加するには、上に示したのと同じパターンに従うことができます。 まず、IDL を使用して、公開する機能を定義します。 次に、Visual Studio でプロジェクトをビルドして、スタブ実装を生成します。 次に、必要に応じて実装を完了します。 IDL で定義したメソッド、プロパティ、およびイベントは、Windows ランタイムコンポーネントを使用するアプリケーションから参照できます。 IDL の詳細については、「 [Microsoft インターフェイス定義言語3.0 の概要](/uwp/midl-3/intro)」を参照してください。

Windows ランタイムコンポーネントにイベントを追加する方法の例については、「 [C++/WinRT でイベントを作成](../cpp-and-winrt-apis/author-events.md)する」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

| 症状 | 解決方法 |
|---------|--------|
|C++/WinRT アプリで、XAML を使用する [C# Windows ランタイムコンポーネント](./creating-windows-runtime-components-in-csharp-and-visual-basic.md) を使用すると、コンパイラによって "' MyNamespace_XamlTypeInfo ' の形式のエラーが生成*されます: は ' WinRT:: MyNamespace ' のメンバーではありません。* &mdash; *MyNamespace* は Windows ランタイムコンポーネントの名前空間の名前です。 | 使用中の `pch.h` C++/WinRT アプリので、必要に応じ `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; て*MyNamespace*を置き換えます。 |