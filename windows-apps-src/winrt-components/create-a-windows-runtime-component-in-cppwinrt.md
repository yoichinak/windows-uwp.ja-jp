---
title: C++/WinRT を使用した Windows ランタイム コンポーネント
description: このトピックでは、 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用し &mdash; て、任意の Windows ランタイム言語を使用して構築されたユニバーサル Windows アプリから呼び出すことができるコンポーネントを、Windows ランタイムコンポーネントを作成および使用する方法について説明します。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、runtime、component、components、Windows ランタイム Component、WRC、C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646295"
---
# <a name="windows-runtime-components-with-cwinrt"></a>C++/WinRT を使用した Windows ランタイム コンポーネント

このトピックでは、 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用し &mdash; て、任意の Windows ランタイム言語を使用して構築されたユニバーサル Windows アプリから呼び出すことができるコンポーネントを、Windows ランタイムコンポーネントを作成および使用する方法について説明します。

C++/winrtで Windows ランタイムコンポーネントを構築するには、いくつかの理由があります。
- C++ のパフォーマンス上の利点を、複雑な操作や計算量の多い操作で利用できます。
- 既に記述およびテストされている標準 C++ コードを再利用する場合。
- C# など、で記述されたユニバーサル Windows プラットフォーム (UWP) アプリに Win32 機能を公開すること。

一般に、C++/WinRT コンポーネントを作成する場合は、標準 C++ ライブラリの型と組み込み型を使用できます。ただし、別のパッケージのコードとの間でデータをやり取りしているアプリケーションバイナリインターフェイス (ABI) の境界は除き `.winmd` ます。 ABI では Windows ランタイム型を使用します。 さらに、C++/WinRT コードでは、デリゲートやイベントなどの型を使用して、コンポーネントから発生させ、別の言語で処理できるイベントを実装します。 C++/winrtの詳細については、「 [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 」を参照してください。

このトピックの残りの部分では、C++/WinRT で Windows ランタイムコンポーネントを作成する方法と、それをアプリケーションから使用する方法について説明します。

このトピックで作成する Windows ランタイムコンポーネントには、温度計を表すランタイムクラスが含まれています。 また、このトピックでは、温度計のランタイムクラスを使用し、関数を呼び出して温度を調整するコアアプリについても示します。

> [!NOTE]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

> [!IMPORTANT]
> C++/WinRT でランタイム クラスを使用および作成する方法についての理解をサポートするために重要な概念と用語については、「[C++/WinRT での API の使用](../cpp-and-winrt-apis/consume-apis.md)」と「[C++/WinRT での作成者 API](../cpp-and-winrt-apis/author-apis.md)」を参照してください。

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>Windows ランタイムコンポーネント (ThermometerWRC) を作成する

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **Windows ランタイムコンポーネント (C++/WinRT)** プロジェクトを作成し、 *ThermometerWRC* ("温度計 Windows ランタイムコンポーネント") という名前を付けます。 **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。 プロジェクトに *ThermometerWRC* という名前を付けると、このトピックの残りの手順で最も簡単に作業できるようになります。 

プロジェクトはまだビルドしないでください。

新しく作成したプロジェクトには、`Class.idl` という名前のファイルが含まれています。 ソリューション エクスプローラーで、そのファイル `Thermometer.idl` の名前を変更します (`.idl` ファイルの名前を変更すると、依存ファイル `.h` および `.cpp` も自動的に名前変更されます)。 `Thermometer.idl` の内容を、以下のリストで置き換えます。

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

ファイルを保存します。 現時点では、プロジェクトは完了までビルドされませんが、ここでは、 **温度計** のランタイムクラスを実装するソースコードファイルが生成されるため、ビルドを行うと便利です。 それでは、今すぐビルドしてみましょう (この段階で表示されることが予想されるビルド エラーは、`Class.h` と `Class.g.h` が見つからないことに関係しています)。

ビルド プロセス中に、`midl.exe` ツールが実行されてコンポーネントの Windows ランタイム メタデータ ファイルが作成されます (これは `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` です)。 次に、`cppwinrt.exe` ツールが (`-component` オプションで) 実行され、コンポーネントの作成をサポートするソース コード ファイルが生成されます。 これらのファイルには、IDL で宣言した **温度計** のランタイムクラスの実装を開始するためのスタブが含まれています。 これらのスタブは `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` と `Thermometer.cpp` です。

プロジェクト ノードを右クリックし、 **[エクスプローラーでフォルダーを開く]** をクリックします。 これにより、エクスプローラーでプロジェクト フォルダーが開きます。 そこで、スタブ ファイル `Thermometer.h` および `Thermometer.cpp` をフォルダー `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` から、ご自分のプロジェクト ファイルを含むフォルダー `\ThermometerWRC\ThermometerWRC\` にコピーし、コピー先のファイルを置き換えます。 ここで、`Thermometer.h` と `Thermometer.cpp` を開いてランタイム クラスを実装してみましょう。 で `Thermometer.h` 、**温度計**の実装 (ファクトリ実装では*なく*) に新しいプライベートメンバーを追加します。

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

で `Thermometer.cpp` 、次の一覧に示すように **AdjustTemperature** メソッドを実装します。

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

また、両方のファイルから `static_assert` を削除する必要もあります。

任意の警告によってビルドが妨げられる場合は、該当する警告を解決するか、またはプロジェクトのプロパティ **C/C++**  > **General** > **Treat Warnings As Errors** を **No (/WX-)** に設定して、もう一度プロジェクトをビルドします。

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>Windows ランタイムコンポーネントをテストするためのコアアプリ (ThermometerCoreApp) を作成する

次に、新しいプロジェクトを作成します ( *ThermometerWRC* ソリューション内または新しいプロジェクトを作成します)。 **コアアプリ (C++/WinRT)** プロジェクトを作成し、 *ThermometerCoreApp*という名前を付けます。 2つのプロジェクトが同じソリューション内にある場合は、 *ThermometerCoreApp* をスタートアッププロジェクトとして設定します。

> [!NOTE]
> 前述のように、Windows ランタイムコンポーネント ( *ThermometerWRC*という名前のプロジェクト) の Windows ランタイムメタデータファイルがフォルダーに作成され `\ThermometerWRC\Debug\ThermometerWRC\` ます。 このパスの最初のセグメントは、ソリューション ファイルを含むフォルダーの名前です。次のセグメントは、`Debug` という名前のサブディレクトリです。最後のセグメントは、Windows ランタイム コンポーネントの名前を付けられたサブディレクトリです。 プロジェクトに *ThermometerWRC*という名前を指定しなかった場合は、メタデータファイルがフォルダーに配置され `\<YourProjectName>\Debug\<YourProjectName>\` ます。

次に、コアアプリプロジェクト (*ThermometerCoreApp*) で参照を追加し、を参照し `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` ます (2 つのプロジェクトが同じソリューション内にある場合は、プロジェクト間参照を追加します)。 **[追加]** をクリックして **[OK]** をクリックします。 次に、 *ThermometerCoreApp*をビルドします。 ペイロードファイルが存在しないというエラーが表示される場合は、 `readme.txt` そのファイルを Windows ランタイムコンポーネントプロジェクトから除外し、再構築してから、 *ThermometerCoreApp*をリビルドします。

ビルド プロセス中に、`cppwinrt.exe` ツールが実行され、参照されている `.winmd` ファイルを投影型を含むソース コード ファイルに処理して、コンポーネントの使用をサポートします。 コンポーネントのランタイム クラス (`ThermometerWRC.h` という名前) の投影型のヘッダーは、フォルダー `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\` 内に生成されます。

そのヘッダーを `App.cpp` に含めます。

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

また `App.cpp` 、次のコードを追加して、 **温度計** オブジェクト (射影された型の既定のコンストラクターを使用) をインスタンス化し、温度計オブジェクトに対してメソッドを呼び出します。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

ウィンドウをクリックするたびに、温度計のオブジェクトの温度が増加します。 コードをステップ実行して、アプリケーションが実際に Windows ランタイムコンポーネントを呼び出していることを確認する場合は、ブレークポイントを設定できます。

## <a name="next-steps"></a>次のステップ

C++/WinRT Windows ランタイムコンポーネントにさらに多くの機能または新しい Windows ランタイムの種類を追加するには、上に示したのと同じパターンに従うことができます。 まず、IDL を使用して、公開する機能を定義します。 次に、Visual Studio でプロジェクトをビルドして、スタブ実装を生成します。 次に、必要に応じて実装を完了します。 IDL で定義したメソッド、プロパティ、およびイベントは、Windows ランタイムコンポーネントを使用するアプリケーションから参照できます。 IDL の詳細については、「 [Microsoft インターフェイス定義言語3.0 の概要](/uwp/midl-3/intro)」を参照してください。

Windows ランタイムコンポーネントにイベントを追加する方法の例については、「 [C++/WinRT でイベントを作成](../cpp-and-winrt-apis/author-events.md)する」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

| 症状 | 解決方法 |
|---------|--------|
|C++/WinRT アプリで、XAML を使用する [C# Windows ランタイム コンポーネント](./creating-windows-runtime-components-in-csharp-and-visual-basic.md)を使用すると、" *'MyNamespace_XamlTypeInfo' は 'winrt::MyNamespace' のメンバーではありません*" という形式のエラーがコンパイラによって生成されます&mdash;この *MyNamespace* は、Windows ランタイム コンポーネントの名前空間の名前です。 | 使用する側の C++/WinRT アプリの `pch.h` で、`#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` を追加します&mdash;必要に応じて *MyNamespace* を置き換えます。 |
