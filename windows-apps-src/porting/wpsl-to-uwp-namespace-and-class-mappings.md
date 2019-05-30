---
description: このトピックでは、対応するユニバーサル Windows プラットフォーム (UWP) への Windows Phone Silverlight Api の包括的なマッピングを提供します。
title: Windows Phone Silverlight は、UWP の名前空間とクラスのマッピング
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb71a95de3f54cb62fa3d2cbc96e5c7935e5d945
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372450"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone の Silverlight UWP API へのマッピングから


このトピックでは、対応するユニバーサル Windows プラットフォーム (UWP) への Windows Phone Silverlight Api の包括的なマッピングを提供します。 一般に、機能の 1 対 1 での対応関係はありませんが、いずれかのプラットフォームが、名前空間またはクラス内で他方のプラットフォームの対応する API よりも持っている機能が多い場合や少ない場合があります。

マッピング テーブルにする UWP プロジェクトで作業しているし、Windows Phone Silverlight プロジェクトからソース コードを再利用しているときに役立ちます。 2 つのプラットフォーム間で、名前空間とクラス (UI コントロールを含む) の名前に違いがあります。 多くの場合、名前空間名を簡単に変更するだけでコードはコンパイルします。 名前空間名に加えて、クラス名または API 名も変更される場合があります。 マッピングに若干の追加作業が必要になり、まれにアプローチの変更が必要になることもあります。

**テーブルを使用する方法。  **最初に、使用しているクラスの名前を検索します。 マッピングで単純な名前空間名の変更よりも複雑になる場合は常に、クラスが示されています。 クラスが示されていない場合は、マッピングは名前空間の変更のみです。 したがって、クラスの名前空間名を探すことで、相当する UWP の名前空間名が見つかります。 目的のクラスはその名前空間に含まれています。 名前空間が示されていない場合は、その名前は変更されていません。

**注**  Windows 10 を補って余りの .NET Framework、Windows Phone ストア アプリはサポートしています。 たとえば、Windows 10 では、いくつかの System.ServiceModel があります。\*名前空間と、System.Net、System.Net.NetworkInformation、および System.Net.Sockets です。
また、Windows 10 アプリで得られます .NET ネイティブが MSIL をネイティブ実行可能なマシン語コードに変換する時間の先行コンパイル テクノロジ。 .NET ネイティブ アプリは、MSIL アプリに比べて、すばやく起動し、メモリ使用量やバッテリ使用量は少なくなります。

| Windows Phone Silverlight | Windows ランタイム |
| ------------------------- | --------------- |
| 広告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** クラス | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) クラス |
| アラーム、リマインダー、バックグラウンド エージェント | |
| **Microsoft.Phone.BackgroundAgent** クラス | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)クラス |
| **Microsoft.Phone.Scheduler** 名前空間 | [**Windows.ApplicationModel.Background** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)名前空間 |
| **Microsoft.Phone.Scheduler.Alarm** クラス | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)と[ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)クラス |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** クラス | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)クラス |
| **Microsoft.Phone.Scheduler.Reminder** クラス | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)と[ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)クラス |
| **Microsoft.Phone.PictureDecoder** クラス | [**BitmapDecoder** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)クラス |
| **Microsoft.Phone.BackgroundAudio** 名前空間 | [**Windows.Media.Playback** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback)名前空間 |
| **Microsoft.Phone.BackgroundTransfer** 名前空間 | [**Windows.Networking.BackgroundTransfer** ](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer)名前空間 |
| アプリ モデルと環境 | |
| **System.AppDomain** クラス | 直接相当する要素はなし。 [  **Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) クラスと [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) クラスをご覧ください。 |
| **System.Environment** クラス | 直接相当する要素はなし |
| **System.ComponentModel.Annotations** クラス  | 直接相当する要素はなし |
| **System.ComponentModel.BackgroundWorker** クラス | [**ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool)クラス |
| **System.ComponentModel.DesignerProperties** クラス | [**デザイン モード**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode)クラス |
| **System.Threading.Thread**、**System.Threading.ThreadPool** クラス | [**ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool)クラス |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** メソッド | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** メソッド |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** プロパティ | (S = **System**) <br/> **S.Environment.ManagedThreadId** プロパティ |
| **System.Threading.Timer** クラス | [**ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)クラス |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** クラス | [**CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)クラス |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** クラス | [**DispatcherTimer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)クラス |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** クラス | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity** 名前空間 | [Microsoft.Xaml.Interactivity](https://go.microsoft.com/fwlink/p/?LinkId=328776) 名前空間 |
| **Microsoft.Expression.Interactivity.Core** 名前空間 | [Microsoft.Xaml.Interactions.Core](https://go.microsoft.com/fwlink/p/?LinkId=328773) 名前空間 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** クラス | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity.Input** 名前空間 | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity.Media** 名前空間 | [Microsoft.Xaml.Interactions.Media](https://go.microsoft.com/fwlink/p/?LinkId=328775) 名前空間 |
| **Microsoft.Expression.Shapes** 名前空間 | 直接相当する要素はなし |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** インターフェイス | 直接相当する要素はなし |
| 連絡先とカレンダーのデータ | |
| **Microsoft.Phone.UserData** 名前空間 | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts)、 [ **Windows.ApplicationModel.Appointments** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments)名前空間 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** クラス | [**連絡先**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)クラス |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** クラス | [**AppointmentCalendar** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar)クラス |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** クラス | [**ContactStore** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore)クラス |
| コントロールと UI インフラストラクチャ | |
| **ControlTiltEffect.TiltEffect** クラス | Windows ランタイム アニメーション ライブラリのアニメーションは、コモン コントロールの既定のスタイルに組み込まれています。 「[アニメーション](wpsl-to-uwp-porting-xaml-and-ui.md)」をご覧ください。 |
| **Microsoft.Phone.Controls** 名前空間 | [**Windows.UI.Xaml.Controls** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls)名前空間 |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** クラス | [**ポップアップ メニュー** ](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** クラス | [**DatePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** クラス | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** クラス | [**SemanticZoom** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** クラス | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection)、 [ **WindowActivatedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** クラス | [**ハブ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame** クラス、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** クラス | [**フレーム**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** クラス | [**ページ**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** クラス | [**PointerDownThemeAnimation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** クラス | [**TimePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** クラス | [**WebView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)クラス |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** クラス | 直接相当する要素はなし |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** クラス | 一般的なレイアウトの目的で直接相当する要素はありません。 [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)と[ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid)アイテム コントロールの項目のパネルのテンプレートで使用できます。 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 名前空間 | 直接相当する要素はなし |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 名前空間 | 直接相当する要素はなし |
| **Microsoft.Phone.Globalization** 名前空間 | 直接相当する要素はなし |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** クラス | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)、 [ **MemoryManager** ](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager)クラス。 詳しくは、「[デバイスの状態](wpsl-to-uwp-input-and-sensors.md)」をご覧ください。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** クラス | 直接相当する要素はなし |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** クラス | [**AdvertisingManager** ](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager)クラス |
| **System.Windows** 名前空間 | [**Windows.UI.Xaml** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml)名前空間 |
| **System.Windows.Automation** 名前空間 | [**Windows.UI.Xaml.Automation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation)名前空間 |
| **System.Windows.Controls**、**System.Windows.Input** 名前空間 | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)、 [ **Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、 [ **Windows.UI.Xaml.Controls** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls)名前空間 |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** クラス | [**SwapChainPanel** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)クラス |
| **System.Windows.Controls.RichTextBox** クラス | [**RichEditBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)クラス |
| **System.Windows.Controls.WrapPanel** クラス | 一般的なレイアウトの目的で直接相当する要素はありません。 [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)と[ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid)アイテム コントロールの項目のパネルのテンプレートで使用できます。 |
| **System.Windows.Controls.Primitives** 名前空間 | [**Windows.UI.Xaml.Controls.Primitives** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives)名前空間 |
| **System.Windows.Controls.Shapes** 名前空間 | [**Windows.UI.Xaml.Controls.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes)名前空間 |
| **System.Windows.Data** 名前空間 | [**Windows.UI.Xaml.Data** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data)名前空間 |
| **System.Windows.Documents** 名前空間 | [**Windows.UI.Xaml.Documents** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents)名前空間 |
| **System.Windows.Ink** 名前空間 | 直接相当する要素はなし |
| **System.Windows.Markup** 名前空間 | [**Windows.UI.Xaml.Markup** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup)名前空間 | 
| **System.Windows.Navigation** 名前空間 | [**Windows.UI.Xaml.Navigation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation)名前空間 |
| **System.Windows.UIElement.Tap** イベント、**EventHandler&lt;GestureEventArgs&gt;** デリゲート | [**タップされた**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)イベント、 [ **TappedEventHandler** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler)デリゲート |
| データとサービス |  |
| **System.Data.Linq.DataContext** クラス | 直接相当する要素はなし |
| **System.Data.Linq.Mapping.ColumnAttribute** クラス | 直接相当する要素はなし |
| **System.Data.Linq.SqlClient.SqlHelpers** クラス | 直接相当する要素はなし |
| デバイス | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 名前空間 | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)、 [ **Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp)、 [ **Windows.Devices.Input** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)、 [ **Windows.Devices.Sensors** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)名前空間 |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** クラス | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)クラス。 また [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラス (Windows のみ)。 |
| **Microsoft.Devices.CameraButtons** クラス | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons)クラス |
| **Microsoft.Devices.CameraVideoBrushExtensions** クラス | [**CaptureElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)クラス |
| **Microsoft.Devices.Environment** クラス | 直接相当する要素はなし。 回避策として、条件付きコンパイルを使って、カスタム シンボルを定義します。 または、[IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached?redirectedfrom=MSDN#System_Diagnostics_Debugger_IsAttached) プロパティを使って回避策を作成できます。 |
| **Microsoft.Devices.MediaHistory** クラス | 直接相当する要素はなし |
| **Microsoft.Devices.VibrateController** クラス | [**VibrationDevice** ](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice)クラス |
| **Microsoft.Devices.Radio.FMRadio** クラス | 直接相当する要素はなし |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** クラス | [  **Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 名前空間内 |
| **Microsoft.Devices.Sensors.Gyroscope** クラス | [**ジャイロ メーター** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer)クラス |
| **Microsoft.Devices.Sensors.Motion** クラス | [**傾斜計**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer)クラス |
| グローバリゼーション | |
| **System.Globalization** 名前空間 | [**Windows.Globalization** ](https://docs.microsoft.com/uwp/api/Windows.Globalization)名前空間 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** プロパティ | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** プロパティ |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** プロパティ | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** プロパティ |
| グラフィックスとアニメーション | |
| **Microsoft.Xna.Framework します。\*** 名前空間、 [XNA Framework クラス ライブラリ](https://go.microsoft.com/fwlink/p/?LinkId=263769)、[コンテンツ パイプラインのクラス ライブラリ](https://go.microsoft.com/fwlink/p/?LinkId=263770) | 直接相当する要素はなし。 一般的に、C++ と共に [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) を使います。 「[ゲームの開発](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))」と「[DirectX と XAML の相互運用機能](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))」をご覧ください。 |
| **Microsoft.Xna.Framework.Audio.Microphone** クラス | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)クラス |
| **Microsoft.Xna.Framework.Audio.SoundEffect** クラス | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)クラス |
| **Microsoft.Xna.Framework.GamerServices** 名前空間 | (WPS = **Windows.Phone.System**) <br/> [**WPS します。UserProfile.GameServices.Core** ](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core)名前空間 |
| **Microsoft.Xna.Framework.GamerServices.Guide** クラス | 直接相当する要素はなし |
| **Microsoft.Xna.Framework.Input.GamePad** クラス | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons)クラス |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** クラス | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer)クラス |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**、**MXFM.PhoneExtensions.MediaLibraryExtensions** クラス | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)クラス |
| **Microsoft.Xna.Framework.Media.MediaQueue** クラス | [**SystemMediaTransportControls** ](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls)クラス |
| **Microsoft.Xna.Framework.Media.Playlist** クラス | [**BackgroundMediaPlayer** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer)クラス |
| **System.Windows.Media** 名前空間 | [**Windows.UI.Xaml.Media** ](/uwp/api/Windows.UI.Xaml.Media)名前空間 |
| **System.Windows.Media.RadialGradientBrush** クラス | 直接相当する要素はなし。 「[メディアとグラフィックス](wpsl-to-uwp-porting-xaml-and-ui.md)」をご覧ください。 |
| **System.Windows.Media.Animation** 名前空間 | [**Windows.UI.Xaml.Media.Animation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation)名前空間 |
| **System.Windows.Media.Effects** 名前空間 | 直接相当する要素はなし |
| **System.Windows.Media.Imaging** 名前空間 | [**Windows.UI.Xaml.Media.Imaging** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging)名前空間 |
| **System.Windows.Media.Media3D** 名前空間 | [**Windows.UI.Xaml.Media.Media3D** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D)名前空間 |
| **System.Windows.Shapes** 名前空間 | [**Windows.UI.Xaml.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes)名前空間 |
| ランチャーとセレクター | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** クラス | [**ContactPicker** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker)クラス |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** クラス | [**Windows.ApplicationModel.Wallet** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet)名前空間 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.CameraCaptureTask** クラス | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)クラス。 また [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラス (Windows のみ)。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)クラス ([**RequestAppPurchaseAsync** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)メソッド) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** クラス | [**ランチャー** ](https://docs.microsoft.com/uwp/api/Windows.System.Launcher)クラス |
| **Microsoft.Phone.Tasks.EmailComposeTask** クラス | [**EmailMessage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage)クラス |
| **Microsoft.Phone.Tasks.GameInviteTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.PhoneCallTask** クラス | [**PhoneCallManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager)クラス |
| **Microsoft.Phone.Tasks.PhotoChooserTask** クラス | [**FileOpenPicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)クラス |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** クラス | [**AppointmentManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager)クラス |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** クラス | [**StoredContact** ](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact)クラス (Windows Phone のみ) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** クラス | [**DataPackage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)クラス |
| Location | |
| **System.Device.Location** 名前空間 | [**Windows.Devices.Geolocation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation)名前空間 |
| **System.Device.GeoCoordinateWatcher** クラス | [**Geolocator** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)クラス |
| マップ | |
| **Microsoft.Phone.Maps** 名前空間 | [**Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)名前空間 |
| **Microsoft.Phone.Maps.Controls** 名前空間 | [**Windows.UI.Xaml.Controls.Maps** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps)名前空間 |
| **Microsoft.Phone.Maps.Controls.Map** クラス | [**MapControl** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)クラス |
| **Microsoft.Phone.Maps.Services** 名前空間 | [**Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)名前空間 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** クラス | [**MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)クラス |
| **System.Device.Location.GeoCoordinate** クラス | [**Geopoint** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)クラス |
| **Microsoft.Phone.Maps.Services.Route** クラス | [**MapRoute** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute)クラス |
| **Microsoft.Phone.Maps.Services.RouteQuery** クラス | [**MapRouteFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder)クラス |
| 収益化 | |
| **Microsoft.Phone.Marketplace** 名前空間 | [**Windows.ApplicationModel.Store** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)名前空間 |
| メディア | |
| **Microsoft.Phone.Media** 名前空間 | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)クラス |
| ネットワーク | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** クラス | [**ホスト名**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName)、 [ **NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** クラス | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** クラス | [**ConnectionProfile** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile)クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** クラス | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** クラス | 直接相当する要素はなし |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Networking.Voip** 名前空間 | 直接相当する要素はなし |
| **System.Net.CookieCollection** クラス | 引き続きサポートされますが、一部のプロパティは含まれていません (たとえば、IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) から派生し、進捗状況を測定します。 |
| **System.Net.DnsEndPoint**、**IPAddress** クラス | これらのクラスは引き続きサポートされますが、一部のプロパティは含まれていません。 代わりに、[**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) クラスに移行してください。 |
| **System.Net.HttpUtility** クラス | [**HtmlFormatHelper** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper)クラス |
| **System.Net.HttpWebRequest** クラス | 部分的にサポートされますが、お勧めできません。将来的な代替案は [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)) です。 これらの API では、[System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) を使って HTTP 要求を表します。 |
| **System.Net.HttpWebResponse** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 お勧めできる将来的な代替案は [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)) です。 これらの API では、[System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage(v=vs.110).aspx) を使って HTTP 応答を表します。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** クラス | コンストラクター以外は引き続きサポートされます。 |
| **System.Net.OpenReadCompletedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 代わりに、[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) クラスに移行してください。 |
| **System.Net.Sockets.SocketException** クラス | 引き続きサポートされますが、ErrorCode の代わりに SocketErrorCode プロパティを使います。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** クラス | [**マッピングされています**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)クラス |
| **System.Net.UploadProgressChangedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** クラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** クラス | 部分的にサポートされます (プロパティのセットが異なる) が、お勧めできません。将来的な代替案は [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)) です。 これらの API では、[System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) を使って HTTP 要求を表します。 |
| **System.Net.WebResponse** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 お勧めできる将来的な代替案は [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)) です。 これらの API では、[System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage(v=vs.110).aspx) を使って HTTP 応答を表します。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** クラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** クラス | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラス (または[System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** クラス | **System.FormatException** クラス |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 名前空間 | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)、 [ **Windows.Networking.PushNotifications** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications)名前空間 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** クラス | [**TileNotification** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)クラス |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** クラス | [**PushNotificationChannel** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel)クラス |
| プログラミング | |
| **System** 名前空間 | [**Windows.Foundation** ](https://docs.microsoft.com/uwp/api/Windows.Foundation)名前空間 |
| **System.Diagnostics.StackFrame**、**StackTrace** クラス | 直接相当する要素はなし |
| **System.Diagnostics** 名前空間 | [**Windows.Foundation.Diagnostics** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics)名前空間 |
| **System.ICloneable** インターフェイス | 適切な型を返すカスタム メソッド。 |
| **System.Reflection.Emit.ILGenerator** クラス | 直接相当する要素はなし |
| Reactive Extensions | |
| **Microsoft.Phone.Reactive** 名前空間 | 直接相当する要素はなし |
| リフレクション | |
| **System.Type** クラス | **System.Reflection.TypeInfo** クラス。 「[UWP アプリのための .NET Framework でのリフレクション](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)」を参照してください。 |
| 参考資料 | |
| **System.Resources.ResourceManager** クラス | (WA = **Windows.ApplicationModel**)<br/>[**WA します。Resources.Core** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core)と[ **WA します。リソース**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources)名前空間、 [ **ResourceManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager)クラス。 「[Windows ランタイム アプリのリソースの作成と取得](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140))」をご覧ください。 |
| セキュリティ要素 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** クラス | [**SmartCardConnection** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection)クラス |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** クラス | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| セキュリティ | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** クラス | [**CryptographicEngine** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine)クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** クラス | [**HashAlgorithmProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider)クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** クラス | [**DataProtectionProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider)クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** クラス | [**CryptographicBuffer** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer)クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** クラス | [**CertificateEnrollmentManager** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager)クラス |
| シェル | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** クラス | [**コマンド バー** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** クラス | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)クラス (内部で使用すると、 [ **PrimaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)プロパティ) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** クラス | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)クラス (内部で使用すると、 [ **SecondaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)プロパティ) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** クラス | [**TileTemplateType** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** クラス | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication)、 [ **DisplayRequest** ](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** クラス | [**StatusBarProgressIndicator** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** クラス | [**SecondaryTile** ](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** クラス | [**TileUpdater** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** クラス | [**ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** クラス | [**StatusBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar)クラス |
| ストレージと I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** クラス | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)クラス |
| **System.IO** 名前空間 | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)、 [ **Windows.Storage.Streams** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams)名前空間 |
| **System.IO.Directory** クラス | [**StorageFolder** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)クラス |
| **System.IO.File** クラス | [**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)と[ **PathIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO)クラス
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** クラス |[**ApplicationData.LocalFolder** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)プロパティ |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** クラス | [**ApplicationData.LocalSettings** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)プロパティ |
| **System.IO.Stream** クラス | 引き続きサポートされますが、BeginRead()/EndRead() と BeginWrite()/EndWrite() の代わりに ReadAsync() と WriteAsync() を使います。 |
| ウォレット | |
| **Microsoft.Phone.Wallet** 名前空間 | [**Windows.ApplicationModel.Wallet** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet)名前空間 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** メソッド |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** メソッド |


次のトピックは「[プロジェクトの移植](wpsl-to-uwp-porting-to-a-uwp-project.md)」です。

