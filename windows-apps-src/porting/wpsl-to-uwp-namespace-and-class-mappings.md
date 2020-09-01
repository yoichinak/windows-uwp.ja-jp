---
description: このトピックでは、Windows Phone Silverlight API からユニバーサル Windows プラットフォーム (UWP) の相当する API への包括的なマッピングを示します。
title: WPSL から UWP への名前空間とクラスのマッピング
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ab6bdace41041f03bd4316b1f65de1a5aeb60835
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167496"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight から UWP API へのマッピング


このトピックでは、Windows Phone Silverlight API からユニバーサル Windows プラットフォーム (UWP) の相当する API への包括的なマッピングを示します。 一般に、機能の 1 対 1 での対応関係はありませんが、いずれかのプラットフォームが、名前空間またはクラス内で他方のプラットフォームの対応する API よりも持っている機能が多い場合や少ない場合があります。

UWP プロジェクトで作業する場合、および Windows Phone Silverlight プロジェクトからのソース コードを再利用する場合に、このマッピング表が参考になります。 2 つのプラットフォーム間で、名前空間とクラス (UI コントロールを含む) の名前に違いがあります。 多くの場合、名前空間名を簡単に変更するだけでコードはコンパイルします。 名前空間名に加えて、クラス名または API 名も変更される場合があります。 マッピングに若干の追加作業が必要になり、まれにアプローチの変更が必要になることもあります。

**テーブルの使用方法:  ** まず、使用しているクラスの名前を検索します。 マッピングで単純な名前空間名の変更よりも複雑になる場合は常に、クラスが示されています。 クラスが示されていない場合は、マッピングは名前空間の変更のみです。 したがって、クラスの名前空間名を探すことで、相当する UWP の名前空間名が見つかります。 目的のクラスはその名前空間に含まれています。 名前空間が示されていない場合は、その名前は変更されていません。

**メモ**   Windows 10 では、Windows Phone ストアアプリよりも多くの .NET Framework がサポートされています。 たとえば、Windows 10 にはいくつかの System.servicemodel があります。 \* 名前空間、System.Net、システム .Net. NetworkInformation、およびシステム .Net. Sockets。
また、Windows 10 アプリでは、.NET ネイティブのメリットを受けることができます。これは、MSIL をネイティブに実行可能なマシン コードに変換する事前コンパイル テクノロジです。 .NET ネイティブ アプリは、MSIL アプリに比べて、すばやく起動し、メモリ使用量やバッテリ使用量は少なくなります。

| Windows Phone Silverlight | Windows ランタイム |
| ------------------------- | --------------- |
| 広告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** クラス | [AdControl](../monetize/display-ads-in-your-app.md) クラス |
| アラーム、リマインダー、バックグラウンド エージェント | |
| **Microsoft.Phone.BackgroundAgent** クラス | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラス |
| **Microsoft.Phone.Scheduler** 名前空間 | [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background) 名前空間 |
| **Microsoft.Phone.Scheduler.Alarm** クラス | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラスと [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) クラス |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** クラス | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラス |
| **Microsoft.Phone.Scheduler.Reminder** クラス | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) クラスと [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) クラス |
| **Microsoft.Phone.PictureDecoder** クラス | [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) クラス |
| **Microsoft.Phone.BackgroundAudio** 名前空間 | [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) 名前空間 |
| **Microsoft.Phone.BackgroundTransfer** 名前空間 | [**Windows.Networking.BackgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) 名前空間 |
| アプリ モデルと環境 | |
| **System.AppDomain** クラス | 同等の軸はありません。 [**Application**](/uwp/api/Windows.UI.Xaml.Application) クラスと [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) クラスをご覧ください。 |
| **System.Environment** クラス | 直接相当する要素はなし |
| **System.ComponentModel.Annotations** クラス  | 直接相当する要素はなし |
| **System.ComponentModel.BackgroundWorker** クラス | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) クラス |
| **System.ComponentModel.DesignerProperties** クラス | [**DesignMode**](/uwp/api/Windows.ApplicationModel.DesignMode) クラス |
| **System.Threading.Thread**、**System.Threading.ThreadPool** クラス | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) クラス |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** メソッド | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** メソッド |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** プロパティ | (S = **System**) <br/> **S.Environment.ManagedThreadId** プロパティ |
| **System.Threading.Timer** クラス | [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) クラス |
| ( **SWT = system.object**) <br/> **SWT.Dispatcher** クラス | [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) クラス |
| ( **SWT = system.object**) <br/> **SWT.DispatcherTimer** クラス | [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) クラス |
| Blend for Visual Studio | |
| (MEDC = **Microsoft. Expression. Drawing**) <br/> **MEDC.GeometryHelper** クラス | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity** 名前空間 | [Microsoft.Xaml.Interactivity](/previous-versions/dn458193(v=vs.120)) 名前空間 |
| **Microsoft.Expression.Interactivity.Core** 名前空間 | [Microsoft.Xaml.Interactions.Core](/previous-versions/dn458182(v=vs.120)) 名前空間 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** クラス | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity.Input** 名前空間 | 直接相当する要素はなし |
| **Microsoft.Expression.Interactivity.Media** 名前空間 | [Microsoft.Xaml.Interactions.Media](/previous-versions/dn458186(v=vs.120)) 名前空間 |
| **Microsoft.Expression.Shapes** 名前空間 | 直接相当する要素はなし |
| (MI = **Microsoft 内部**) <br/> **MI.IManagedFrameworkInternalHelper** インターフェイス | 直接相当する要素はなし |
| 連絡先とカレンダーのデータ | |
| **Microsoft.Phone.UserData** 名前空間 | [**Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts)、[**Windows.ApplicationModel.Appointments**](/uwp/api/Windows.ApplicationModel.Appointments) 名前空間 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** クラス | [**Contact**](/uwp/api/Windows.ApplicationModel.Contacts.Contact) クラス |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** クラス | [**AppointmentCalendar**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) クラス |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** クラス | [**ContactStore**](/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) クラス |
| コントロールと UI インフラストラクチャ | |
| **ControlTiltEffect.TiltEffect** クラス | Windows ランタイム アニメーション ライブラリのアニメーションは、コモン コントロールの既定のスタイルに組み込まれています。 「 [アニメーション](wpsl-to-uwp-porting-xaml-and-ui.md)」を参照してください。 |
| **Microsoft.Phone.Controls** 名前空間 | [**Windows. UI. .xaml**](/uwp/api/Windows.UI.Xaml.Controls) 名前空間 |
| (MPC = **Microsoft. Phone. コントロール**) <br/> **MPC.ContextMenu** クラス | [**PopupMenu**](/uwp/api/Windows.UI.Popups.PopupMenu) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.DatePickerPage** クラス | [**DatePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.GestureListener** クラス | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.LongListSelector** クラス | [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.ObscuredEventArgs** クラス | [**SystemProtection**](/uwp/api/Windows.Phone.System.SystemProtection)、[**WindowActivatedEventArgs**](/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.Panorama** クラス | [**ハブ**](/uwp/api/Windows.UI.Xaml.Controls.Hub) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.PhoneApplicationFrame** クラス、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** クラス | [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.PhoneApplicationPage** クラス | [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.TiltEffect** クラス | [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.TimePickerPage** クラス | [**TimePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.WebBrowser** クラス | [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) クラス |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.WebBrowserExtensions** クラス | 直接相当する要素はなし |
| (MPC = **Microsoft. Phone. コントロール**) <br/>**MPC.WrapPanel** クラス | 一般的なレイアウトの目的で直接相当する要素はありません。 [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) と [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) は、アイテム コントロールのアイテム パネル テンプレートで使うことができます。 |
| (MPD = **Microsoft. Phone. Data**) <br/>**MPD.Linq** 名前空間 | 直接相当する要素はなし |
| (MPD = **Microsoft. Phone. Data**) <br/>**MPD.Linq.Mapping** 名前空間 | 直接相当する要素はなし |
| **Microsoft.Phone.Globalization** 名前空間 | 直接相当する要素はなし |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** クラス | [**EasClientDeviceInformation**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)、[**MemoryManager**](/uwp/api/Windows.System.MemoryManager) クラス 詳細については、「 [デバイスの状態](wpsl-to-uwp-input-and-sensors.md)」を参照してください。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** クラス | 直接相当する要素はなし |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** クラス | [**AdvertisingManager**](/uwp/api/Windows.System.UserProfile.AdvertisingManager) クラス |
| **System.Windows** 名前空間 | [**Windows.UI.Xaml**](/uwp/api/Windows.UI.Xaml) 名前空間 |
| **System.Windows.Automation** 名前空間 | [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation) 名前空間 |
| **System.Windows.Controls**、**System.Windows.Input** 名前空間 | [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)、[**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls) 名前空間 |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** クラス | [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) クラス |
| **System.Windows.Controls.RichTextBox** クラス | [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) クラス |
| **System.Windows.Controls.WrapPanel** クラス | 一般的なレイアウトの目的で直接相当する要素はありません。 [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) と [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) は、アイテム コントロールのアイテム パネル テンプレートで使うことができます。 |
| **System.Windows.Controls.Primitives** 名前空間 | [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) 名前空間 |
| **System.Windows.Controls.Shapes** 名前空間 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 名前空間 |
| **System.Windows.Data** 名前空間 | [**Windows.UI.Xaml.Data**](/uwp/api/Windows.UI.Xaml.Data) 名前空間 |
| **System.Windows.Documents** 名前空間 | [**Windows.UI.Xaml.Documents**](/uwp/api/Windows.UI.Xaml.Documents) 名前空間 |
| **System.Windows.Ink** 名前空間 | 直接相当する要素はなし |
| **System.Windows.Markup** 名前空間 | [**Windows.UI.Xaml.Markup**](/uwp/api/Windows.UI.Xaml.Markup) 名前空間 | 
| **System.Windows.Navigation** 名前空間 | [**Windows.UI.Xaml.Navigation**](/uwp/api/Windows.UI.Xaml.Navigation) 名前空間 |
| **System.Windows.UIElement.Tap** イベント、**EventHandler&lt;GestureEventArgs&gt;** デリゲート | [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped) イベント、[**TappedEventHandler**](/uwp/api/windows.ui.xaml.input.tappedeventhandler) デリゲート |
| データとサービス |  |
| **System.Data.Linq.DataContext** クラス | 直接相当する要素はなし |
| **System.Data.Linq.Mapping.ColumnAttribute** クラス | 直接相当する要素はなし |
| **System.Data.Linq.SqlClient.SqlHelpers** クラス | 直接相当する要素はなし |
| デバイス | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 名前空間 | [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)、[**Windows.Devices.Enumeration.Pnp**](/uwp/api/Windows.Devices.Enumeration.Pnp)、[**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)、[**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 名前空間 |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** クラス | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) クラス。 また [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラス (Windows のみ)。 |
| **Microsoft.Devices.CameraButtons** クラス | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) クラス |
| **Microsoft.Devices.CameraVideoBrushExtensions** クラス | [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) クラス |
| **Microsoft.Devices.Environment** クラス | 同等の軸はありません。 回避策として、条件付きコンパイルを使って、カスタム シンボルを定義します。 または、[IsAttached](/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) プロパティを使って回避策を作成できます。 |
| **Microsoft.Devices.MediaHistory** クラス | 直接相当する要素はなし |
| **Microsoft.Devices.VibrateController** クラス | [**VibrationDevice**](/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) クラス |
| **Microsoft.Devices.Radio.FMRadio** クラス | 直接相当する要素はなし |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** クラス | [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 名前空間内 |
| **Microsoft.Devices.Sensors.Gyroscope** クラス | [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) クラス |
| **Microsoft.Devices.Sensors.Motion** クラス | [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) クラス |
| グローバリゼーション | |
| **System. グローバリゼーション** 名前空間 | [**Windows. グローバリゼーション**](/uwp/api/Windows.Globalization) 名前空間 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** プロパティ | (SG = **System. グローバリゼーション**) <br/> **S.CultureInfo.CurrentCulture** プロパティ |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** プロパティ | (SG = **System. グローバリゼーション**) <br/> **S.CultureInfo.CurrentUICulture** プロパティ |
| グラフィックスとアニメーション | |
| **Microsoft. .net \* ** . 名前空間, [xna フレームワーククラスライブラリ](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)),[コンテンツパイプラインクラスライブラリ](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 同等の軸はありません。 一般的に、C++ と共に [Microsoft DirectX](/windows/desktop/directx) を使います。 「[ゲームの開発](/previous-versions/windows/apps/hh452744(v=win.10))」と「[DirectX と XAML の相互運用機能](/previous-versions/windows/apps/hh825871(v=win.10))」をご覧ください。 |
| **Microsoft.Xna.Framework.Audio.Microphone** クラス | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) クラス |
| **Microsoft.Xna.Framework.Audio.SoundEffect** クラス | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) クラス |
| **Microsoft.Xna.Framework.GamerServices** 名前空間 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) 名前空間 |
| **Microsoft.Xna.Framework.GamerServices.Guide** クラス | 直接相当する要素はなし |
| **Microsoft.Xna.Framework.Input.GamePad** クラス | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) クラス |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** クラス | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) クラス |
| (MXFM = **Microsoft. Xna. フレームワーク**) <br/> **MXFM.MediaLibrary**、**MXFM.PhoneExtensions.MediaLibraryExtensions** クラス | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) クラス |
| **Microsoft.Xna.Framework.Media.MediaQueue** クラス | [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) クラス |
| **Microsoft.Xna.Framework.Media.Playlist** クラス | [**BackgroundMediaPlayer**](/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) クラス |
| **System.Windows.Media** 名前空間 | [**Windows. UI. .xaml. Media**](/uwp/api/Windows.UI.Xaml.Media) 名前空間 |
| **System.Windows.Media.RadialGradientBrush** クラス | 同等の軸はありません。 「[メディアとグラフィックス](wpsl-to-uwp-porting-xaml-and-ui.md)」をご覧ください。 |
| **System.Windows.Media.Animation** 名前空間 | [**Windows.UI.Xaml.Media.Animation**](/uwp/api/Windows.UI.Xaml.Media.Animation) 名前空間 |
| **System.Windows.Media.Effects** 名前空間 | 直接相当する要素はなし |
| **System.Windows.Media.Imaging** 名前空間 | [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) 名前空間 |
| **System.Windows.Media.Media3D** 名前空間 | [**Windows.UI.Xaml.Media.Media3D**](/uwp/api/Windows.UI.Xaml.Media.Media3D) 名前空間 |
| **System.Windows.Shapes** 名前空間 | [**Windows. UI. .xaml**](/uwp/api/Windows.UI.Xaml.Shapes) 名前空間 |
| ランチャーとセレクター | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** クラス | [**ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) クラス |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** クラス | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) 名前空間 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.CameraCaptureTask** クラス | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) クラス。 また [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) クラス (Windows のみ)。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) クラス ([**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) メソッド) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** クラス | [**Launcher**](/uwp/api/Windows.System.Launcher) クラス |
| **Microsoft.Phone.Tasks.EmailComposeTask** クラス | [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) クラス |
| **Microsoft.Phone.Tasks.GameInviteTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.PhoneCallTask** クラス | [**PhoneCallManager**](/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) クラス |
| **Microsoft.Phone.Tasks.PhotoChooserTask** クラス | [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) クラス |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** クラス | [**AppointmentManager**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) クラス |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** クラス | [**StoredContact**](/uwp/api/Windows.Phone.PersonalInformation.StoredContact) クラス (Windows Phone のみ) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** クラス | [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) クラス |
| 場所 | |
| **System.Device.Location** 名前空間 | [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) 名前空間 |
| **System.Device.GeoCoordinateWatcher** クラス | [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) クラス |
| マップ | |
| **Microsoft.Phone.Maps** 名前空間 | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 名前空間 |
| **Microsoft.Phone.Maps.Controls** 名前空間 | [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 名前空間 |
| **Microsoft.Phone.Maps.Controls.Map** クラス | [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) クラス |
| **Microsoft.Phone.Maps.Services** 名前空間 | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 名前空間 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** クラス | [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) クラス |
| **System.Device.Location.GeoCoordinate** クラス | [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) クラス |
| **Microsoft.Phone.Maps.Services.Route** クラス | [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) クラス |
| **Microsoft.Phone.Maps.Services.RouteQuery** クラス | [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) クラス |
| 収益化 | |
| **Microsoft.Phone.Marketplace** 名前空間 | [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) 名前空間 |
| メディア | |
| **Microsoft.Phone.Media** 名前空間 | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) クラス |
| ネットワーク | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** クラス | [**Hostname**](/uwp/api/Windows.Networking.HostName)、[**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** クラス | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** クラス | [**ConnectionProfile**](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** クラス | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) クラス |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** クラス | 直接相当する要素はなし |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** クラス | 直接相当する要素はなし |
| **Microsoft.Phone.Networking.Voip** 名前空間 | 直接相当する要素はなし |
| **System.Net.CookieCollection** クラス | 引き続きサポートされますが、一部のプロパティは含まれていません (たとえば、IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [システム .Net. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118)))。 [System.Net.Http.StreamContent](/previous-versions/visualstudio/hh138119(v=vs.118)) から派生し、進捗状況を測定します。 |
| **System.Net.DnsEndPoint**、**IPAddress** クラス | これらのクラスは引き続きサポートされますが、一部のプロパティは含まれていません。 代わりに、[**HostName**](/uwp/api/Windows.Networking.HostName) クラスに移行してください。 |
| **System.Net.HttpUtility** クラス | [**HtmlFormatHelper**](/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) クラス |
| **System.Net.HttpWebRequest** クラス | 部分的にサポートされますが、お勧めできません。将来的な代替案は [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) です。 これらの API では、[System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) を使って HTTP 要求を表します。 |
| **System.Net.HttpWebResponse** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 お勧めできる将来的な代替案は [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) です。 これらの API では、[System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) を使って HTTP 応答を表します。 |
| (SNN = **システム .net. NetworkInformation**) <br/> **SNN.NetworkChange** クラス | コンストラクター以外は引き続きサポートされます。 |
| **System.Net.OpenReadCompletedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 代わりに、[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) クラスに移行してください。 |
| **System.Net.Sockets.SocketException** クラス | 引き続きサポートされますが、ErrorCode の代わりに SocketErrorCode プロパティを使います。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** クラス | [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) クラス |
| **System.Net.UploadProgressChangedEventArgs** クラスと、**System.Net.WebClient** に関連する同様のクラス | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** クラス | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** クラス | 部分的にサポートされます (プロパティのセットが異なる) が、お勧めできません。将来的な代替案は [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) です。 これらの API では、[System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) を使って HTTP 要求を表します。 |
| **System.Net.WebResponse** クラス | 引き続きサポートされますが、Close() の代わりに Dispose() を使います。 お勧めできる将来的な代替案は [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) です。 これらの API では、[System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) を使って HTTP 応答を表します。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** クラス | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** クラス | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラス (または [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** クラス | **System.FormatException** クラス |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 名前空間 | [**Windows.UI.Notifications**](/uwp/api/Windows.UI.Notifications)、[**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) 名前空間 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** クラス | [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) クラス |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** クラス | [**PushNotificationChannel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) クラス |
| プログラミング | |
| **System** 名前空間 | [**Windows. Foundation**](/uwp/api/Windows.Foundation) 名前空間 |
| **System.Diagnostics.StackFrame**、**StackTrace** クラス | 直接相当する要素はなし |
| **System.Diagnostics** 名前空間 | [**Windows.Foundation.Diagnostics**](/uwp/api/Windows.Foundation.Diagnostics) 名前空間 |
| **System.ICloneable** インターフェイス | 適切な型を返すカスタム メソッド。 |
| **System.Reflection.Emit.ILGenerator** クラス | 直接相当する要素はなし |
| Reactive Extensions | |
| **Microsoft.Phone.Reactive** 名前空間 | 直接相当する要素はなし |
| リフレクション | |
| **System.Type** クラス | **System.Reflection.TypeInfo** クラス。 「[UWP アプリのための .NET Framework でのリフレクション](/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)」を参照してください。 |
| リソース | |
| **System.Resources.ResourceManager** クラス | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) と [**WA.Resources**](/uwp/api/Windows.ApplicationModel.Resources) 名前空間、[**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) クラス。 「[Windows ランタイム アプリのリソースの作成と取得](/previous-versions/windows/apps/hh694557(v=vs.140))」をご覧ください。 |
| セキュリティ要素 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** クラス | [**SmartCardConnection**](/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) クラス |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** クラス | [**SmartCardReader**](/uwp/api/Windows.Devices.SmartCards.SmartCardReader) クラス |
| セキュリティ | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** クラス | [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** クラス | [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** クラス | [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** クラス | [**CryptographicBuffer**](/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) クラス |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** クラス | [**CertificateEnrollmentManager**](/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) クラス |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** クラス | [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** クラス | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) クラス ([**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) プロパティ内で使う場合) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** クラス | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) クラス ([**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) プロパティ内で使う場合) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** クラス | [**TileTemplateType**](/uwp/api/Windows.UI.Notifications.TileTemplateType) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** クラス | [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication)、[**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** クラス | [**StatusBarProgressIndicator**](/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** クラス | [**SecondaryTile**](/uwp/api/Windows.UI.StartScreen.SecondaryTile) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** クラス | [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** クラス | [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) クラス |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** クラス | [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) クラス |
| ストレージと I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** クラス | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) クラス |
| **System.IO** 名前空間 | [**Windows.Storage**](/uwp/api/Windows.Storage)、[**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams) 名前空間 |
| **System.IO.Directory** クラス | [**Storagefolder**](/uwp/api/Windows.Storage.StorageFolder) クラス |
| **System.IO.File** クラス | [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) クラスと [**PathIO**](/uwp/api/Windows.Storage.PathIO) クラス
| (SII = **& lt;**) <br/> **SII.IsolatedStorageFile** クラス |[**ApplicationData.LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) プロパティ |
| (SII = **& lt;**) <br/> **SII.IsolatedStorageSettings** クラス | [**ApplicationData.LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) プロパティ |
| **System.IO.Stream** クラス | 引き続きサポートされますが、BeginRead()/EndRead() と BeginWrite()/EndWrite() の代わりに ReadAsync() と WriteAsync() を使います。 |
| ウォレット | |
| **Microsoft.Phone.Wallet** 名前空間 | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) 名前空間 |
| xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** メソッド |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** メソッド |


次のトピックは「[プロジェクトの移植](wpsl-to-uwp-porting-to-a-uwp-project.md)」です。