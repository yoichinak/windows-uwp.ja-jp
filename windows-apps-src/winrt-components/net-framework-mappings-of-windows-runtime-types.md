---
title: Windows ランタイム型の .NET マッピング
description: 次の表は、.NET がユニバーサル Windows プラットフォーム (UWP) 型と .NET 型の間で行うマッピングを示しています。
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393657"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Windows ランタイム型の .NET マッピング

次の表は、.NET がユニバーサル Windows プラットフォーム (UWP) 型と .NET 型の間で行うマッピングを示しています。 マネージコードで記述されたユニバーサル Windows アプリでは、Visual Studio IntelliSense は UWP 型ではなく .NET 型を表示します。 たとえば、Windows ランタイム&lt;メソッドが ivector 文字列&gt;型のパラメーターを受け取る場合、IntelliSense は IList&lt;string&gt;型のパラメーターを表示します。 同様に、マネージコードで記述された Windows ランタイムコンポーネントでは、メンバーシグネチャで .NET 型を使用します。 [Windows ランタイムメタデータのエクスポートツール (Winmdexp)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)によって Windows ランタイムコンポーネントが生成されると、.net 型は対応する UWP 型に変換されます。

UWP と .NET の両方で同じ名前空間名と型名を持つほとんどの型は、構造体 (または、列挙型などの構造体に関連付けられた型) です。 UWP では、構造体はフィールド以外のメンバーを持ちません。また、.NET によって隠ぺいされるヘルパー型を必要とします。 これらの構造体の .NET バージョンには、非表示のヘルパー型の機能を提供するプロパティとメソッドがあります。

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>同じ名前と名前空間を持つ .NET 型にマップされる UWP 型

### <a name="in-net-assembly-systemobjectmodeldll"></a>.NET アセンブリシステムの場合

| Namespace | 種類 |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>.NET アセンブリの WindowsRuntime で、

| Namespace | 種類 |
|-|-|
| Windows.Foundation | ポイント |
| Windows.Foundation | Rect |
| Windows.Foundation | サイズ |
| Windows.UI | 色 |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>.NET アセンブリの WindowsRuntime に含まれています。

| Namespace | 種類 |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | 太さ |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | マトリックス |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>異なる名前または名前空間を持つ .NET 型にマップされる UWP 型

### <a name="in-net-assembly-systemobjectmodeldll"></a>.NET アセンブリシステムの場合

| UWP 型/名前空間 | .NET 型/名前空間 |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>.NET アセンブリシステムの .dll

| UWP 型/名前空間 | .NET 型/名前空間 |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>.NET アセンブリ InteropServices で、WindowsRuntime を実行します。

| UWP 型/名前空間 | .NET 型/名前空間 |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>関連トピック

* [と Visual Basic をC#使用したコンポーネントの Windows ランタイム](creating-windows-runtime-components-in-csharp-and-visual-basic.md)