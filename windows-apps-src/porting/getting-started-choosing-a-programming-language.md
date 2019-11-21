---
title: プログラミング言語の選択
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: プログラミング言語の選択
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 037c079881dbb2634b31cc0cf5b9248115dbceef
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259158"
---
# <a name="getting-started-choosing-a-programming-language"></a>はじめに: プログラミング言語の選択


## <a name="choosing-a-programming-language"></a>プログラミング言語の選択

先へ進む前に、ユニバーサル Windows プラットフォーム (UWP) アプリを開発するときに選択できるプログラミング言語について理解している必要があります。 この記事のチュートリアルでは C# が使われていますが、UWP アプリは複数のプログラミング言語を使って開発できます (「[言語、ツール、フレームワーク](https://docs.microsoft.com/previous-versions/windows/apps/dn465799(v=win.10))」をご覧ください)。

C++、C#、Microsoft Visual Basic、JavaScript を使って開発できます。 JavaScript では UI のレイアウトに HTML5 マークアップを使い、他の言語では *XAML (Extensible Application Markup Language)* と呼ばれるマークアップ言語を使って UI を記述します。

この記事では C# を中心に扱いますが、独自の利点がある他の言語についても検討することをお勧めします。 たとえば、特にグラフィックスを多用した際のアプリのパフォーマンスが最も重要視される場合は、C++ が適切です。 Visual Basic アプリの開発者にとっては、Microsoft .NET バージョンの Visual Basic が適切です。 JavaScript と HTML5 の組み合わせは、Web 開発の経歴がある開発者向けです。 詳しくは、次のいずれかのトピックをご覧ください。

-   [を使用して最初の UWP アプリを作成するC++](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [または Visual Basic を使用しC#て初めての UWP アプリを作成する](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [JavaScript を使用した初めての UWP アプリの作成](../get-started/create-a-hello-world-app-js-uwp.md)

**注**  3d グラフィックスを使用するアプリの場合、OpenGL および opengl ES 標準は UWP アプリでネイティブに使用できません。 OpenGL ES のコードを Microsoft DirectX に書き換えない場合は、**Angle** に関心を持つかもしれません。 Angle は OpenGL API 呼び出しを DirectX API 呼び出しに翻訳することにより、OpenGL を DirectX に変換するように設計された進行中のプロジェクトです。 詳しくは、次のトピックをご覧ください。
-   [角度](https://bugs.chromium.org/p/angleproject/)
-   [DirectX を使用して初めての UWP アプリを作成する](https://docs.microsoft.com/previous-versions/windows/apps/br229580(v=win.10))
-   [DirectX を使用する UWP アプリのサンプル](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=DirectX)
-   [DirectX SDK はどこにありますか?](https://docs.microsoft.com/windows/desktop/directx-sdk--august-2009-)

## <a name="giving-c-a-go"></a>C# を試してみる

iOS 開発者は、Objective-C と Swift を日常的に使っています。 両方に最も近い Microsoft プログラミング言語は C# です。 ほとんどの開発者とほとんどのアプリにおいて、最も簡単かつ短期間に学習して使用できる言語は C# と考えられます。そこで、この記事の情報とチュートリアルでは、この言語を中心に取り上げています。 C# について詳しくは、次のトピックをご覧ください。

-   [または Visual Basic を使用しC#て初めての UWP アプリを作成する](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [を使用する UWP アプリのサンプルC#](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=ProgrammingLanguage&f%5B0%5D.Value=C%23&f%5B0%5D.Text=C%23)
-   [見た目C#](https://msdn.microsoft.com/library/kx37x362.aspx)

次に示すのは、Objective-C と C# で書かれたクラスです。 最初に Objective-C バージョンを示し、その後に C# バージョンを示します。

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

次は C# のバージョンです。 Swift のように、ヘッダーと実装が別個のファイルに分離されていないことがわかります。

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# は習得が容易な言語であり、.NET を構成する多くのサポート クラスとフレームワークが付属しています。 すぐに、角かっこなしでコードをうまく記述することができるようになります。

## <a name="next-step"></a>次の手順

[はじめに: Visual Studio での作業の開始](getting-started-getting-around-in-visual-studio.md)
