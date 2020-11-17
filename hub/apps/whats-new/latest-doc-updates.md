---
description: Windows 開発者向けドキュメントへの最新の追加事項について説明します。
title: Windows 開発者向けドキュメントの最新の更新
ms.topic: article
ms.date: 11/05/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: a4d05e92399fa0246add4751323439303647d531
ms.sourcegitcommit: 98888dede3c59ee12c64ab0521a85b5fa60cb771
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "93765585"
---
# <a name="latest-updates-to-the-windows-developer-docs"></a>Windows 開発者向けドキュメントの最新の更新

Windows 開発者向けドキュメントは、新しい強化された情報とコンテンツによって定期的に更新されます。 2020 年 11 月 5 日付けの変更のまとめを次に示します。

注: Windows 10 ビルド19041 (2004 とも呼ばれます) の一部として追加された API の特定のリストについては、[こちらのリスト](/windows/uwp/whats-new/windows-10-build-19041-api-diff)を参照してください。


## <a name="news"></a>ニュース

今月、Microsoft は、ナビゲーションを高速化し、オフラインの閲覧エクスペリエンスを向上させるために、多くの絶対リンクを相対リンクに変換しました。 リンクが壊れている場合は、引き続きお知らせいただければ幸いです。ほとんどのページから直接フィードバックを提供できます。または、ハンドル [@WindowsDocs](https://www.twitter.com/windowsdocs) を使用して Twitter でもご連絡いただけます。

今月のハイライトには以下が含まれます。


### <a name="new-topics"></a>新しいトピック

* [従来のコンソール API と仮想端末シーケンス](https://docs.microsoft.com/windows/console/classic-vs-vt)
* [Windows コンソールとターミナル エコシステムのロードマップ](https://docs.microsoft.com/windows/console/ecosystem-roadmap)
* [Windows API セット](https://docs.microsoft.com/windows/win32/apiindex/windows-apisets)
* [API セット ローダー操作](https://docs.microsoft.com/windows/win32/apiindex/api-set-loader-operation)
* [検出 API セットの可用性](https://docs.microsoft.com/windows/win32/apiindex/detect-api-set-availability)
* [Windows の包括的ライブラリ](https://docs.microsoft.com/windows/win32/apiindex/windows-umbrella-libraries)


### <a name="new-and-updated-samples"></a>新しいサンプルと更新されたサンプル

* [チュートリアル: C++/WinRT コンポーネントから .NET 5 プロジェクションを生成し、NuGet を配布する](https://docs.microsoft.com/windows/uwp/csharp-winrt/net-projection-from-cppwinrt-component)


### <a name="other-content-of-interest"></a>その他の価値がある内容

* [Azure Communication Services](https://docs.microsoft.com/azure/communication-services/overview)
* [Azure App Configuration Python のクイックスタート](https://docs.microsoft.com/azure/azure-app-configuration/quickstart-python)

### <a name="deprecated-content"></a>非推奨になった内容

* [BarcodeScanner.GetSupportedProfiles](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedprofiles?view=winrt-19041)

### <a name="updated-documentation"></a>更新されたドキュメント

* [コンソール ドキュメント](https://github.com/MicrosoftDocs/Console-Docs)
* [MIDL 3.0 の概念的コンテンツ](https://docs.microsoft.com/uwp/midl-3/intro#parameters)
* [Docker の使用開始の概要](https://docs.microsoft.com/windows/dev-environment/docker/overview)
* [WSL 2 で Linux ディスクのマウントを開始する](https://docs.microsoft.com/windows/wsl/wsl2-mount-disk)

次の API リファレンス トピックでは、過去 1 か月に重要な更新が行われました。

### <a name="win32-api-reference"></a>Win32 API リファレンス
<ul>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-createmappedbitmap">CreateMappedBitmap 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-createtoolbarex">CreateToolbarEx 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-getwindowsubclass">GetWindowSubclass 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_addmasked">ImageList_AddMasked 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_create">ImageList_Create 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_draw">ImageList_Draw 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_replaceicon">ImageList_ReplaceIcon 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_setoverlayimage">ImageList_SetOverlayImage 関数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-commdlgextendederror">CommDlgExtendedError 関数 (commdlg. h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-findtexta">FindTextA 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-findtextw">FindTextW 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getopenfilenamea">GetOpenFileNameA 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getopenfilenamew">GetOpenFileNameW 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getsavefilenamea">GetSaveFileNameA 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getsavefilenamew">GetSaveFileNameW 関数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dde/nf-dde-unpackddelparam">UnpackDDElParam 関数 (dde.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_clone">DPA_Clone 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_createex">DPA_CreateEx 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_destroycallback">DPA_DestroyCallback 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_enumcallback">DPA_EnumCallback 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_getptr">DPA_GetPtr 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_getptrindex">DPA_GetPtrIndex 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_grow">DPA_Grow 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_insertptr">DPA_InsertPtr 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_search">DPA_Search 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_setptr">DPA_SetPtr 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_sort">DPA_Sort 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_create">DSA_Create 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_deleteitem">DSA_DeleteItem 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_destroycallback">DSA_DestroyCallback 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_enumcallback">DSA_EnumCallback 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_getitem">DSA_GetItem 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_getitemptr">DSA_GetItemPtr 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_insertitem">DSA_InsertItem 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_setitem">DSA_SetItem 関数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-bindmoniker">BindMoniker 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-codosdatetimetofiletime">CoDosDateTimeToFileTime 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofiletimetodosdatetime">CoFileTimeToDosDateTime 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofreealllibraries">CoFreeAllLibraries 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofreelibrary">CoFreeLibrary 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-coinitialize">CoInitialize 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-coloadlibrary">CoLoadLibrary 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createantimoniker">CreateAntiMoniker 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createdataadviseholder">CreateDataAdviseHolder 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createdatacache">CreateDataCache 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createobjrefmoniker">CreateObjrefMoniker 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createpointermoniker">CreatePointerMoniker 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-getrunningobjecttable">GetRunningObjectTable 関数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-oledraw">OleDraw 関数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-oleloadfromstream">OleLoadFromStream 関数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-olesavetostream">OleSaveToStream 関数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-createdataadviseholder">CreateDataAdviseHolder 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-createoleadviseholder">CreateOleAdviseHolder 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreateembeddinghelper">OleCreateEmbeddingHelper 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreatefromdata">OleCreateFromData 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreatemenudescriptor">OleCreateMenuDescriptor 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegetautoconvert">OleGetAutoConvert 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegetclipboard">OleGetClipboard 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegeticonofclass">OleGetIconOfClass 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleinitialize">OleInitialize 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleiscurrentclipboard">OleIsCurrentClipboard 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olequerycreatefromdata">OleQueryCreateFromData 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleregenumverbs">OleRegEnumVerbs 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olesavetostream">OleSaveToStream 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olesetmenudescriptor">OleSetMenuDescriptor 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleuninitialize">OleUninitialize 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-writefmtusertypestg">WriteFmtUserTypeStg 関数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-createpropertysheetpagea">CreatePropertySheetPageA 関数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-createpropertysheetpagew">CreatePropertySheetPageW 関数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-propertysheeta">PropertySheetA 関数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-propertysheetw">PropertySheetW 関数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupcloseinffile">SetupCloseInfFile 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupdigetdevicepropertyw">SetupDiGetDevicePropertyW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupdiopendeviceinfow">SetupDiOpenDeviceInfoW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupfindnextline">SetupFindNextLine 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetfieldcount">SetupGetFieldCount 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinecounta">SetupGetLineCountA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinecountw">SetupGetLineCountW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinetexta">SetupGetLineTextA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinetextw">SetupGetLineTextW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetstringfielda">SetupGetStringFieldA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetstringfieldw">SetupGetStringFieldW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetthreadlogtoken">SetupGetThreadLogToken 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupiteratecabineta">SetupIterateCabinetA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupiteratecabinetw">SetupIterateCabinetW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setuplogerrora">SetupLogErrorA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setuplogerrorw">SetupLogErrorW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupopeninffilea">SetupOpenInfFileA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupopenlog">SetupOpenLog 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupverifyinffilea">SetupVerifyInfFileA 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupverifyinffilew">SetupVerifyInfFileW 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupwritetextlog">SetupWriteTextLog 関数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-assoccreateforclasses">AssocCreateForClasses 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-duplicateicon">DuplicateIcon 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-extracticonexa">ExtractIconExA 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-extracticonexw">ExtractIconExW 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shell_notifyiconw">Shell_NotifyIconW 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shgetimagelist">SHGetImageList 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shgetstockiconinfo">SHGetStockIconInfo 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shsetlocalizedname">SHSetLocalizedName 関数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_autoscroll">DAD_AutoScroll 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_dragenterex2">DAD_DragEnterEx2 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_setdragimage">DAD_SetDragImage 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompath">ILCreateFromPath 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompatha">ILCreateFromPathA 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompathw">ILCreateFromPathW 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilsavetostream">ILSaveToStream 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-pickicondlg">PickIconDlg 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-restartdialog">RestartDialog 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shaddtorecentdocs">SHAddToRecentDocs 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shchangenotify">SHChangeNotify 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shcreateshellitem">SHCreateShellItem 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shdodragdrop">SHDoDragDrop 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shell_getimagelists">Shell_GetImageLists 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shgetrealidl">SHGetRealIDL 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shgetsetsettings">SHGetSetSettings 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shilcreatefrompath">SHILCreateFromPath 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shlimitinputedit">SHLimitInputEdit 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shopenwithdialog">SHOpenWithDialog 関数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-backupeventloga">BackupEventLogA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-backupeventlogw">BackupEventLogW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-cleareventloga">ClearEventLogA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-cleareventlogw">ClearEventLogW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-closeeventlog">CloseEventLog 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-deleteumsthreadcontext">DeleteUmsThreadContext 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-encryptfilea">EncryptFileA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-encryptfilew">EncryptFileW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-enterumsschedulingmode">EnterUmsSchedulingMode 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getcurrenthwprofilea">GetCurrentHwProfileA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getcurrenthwprofilew">GetCurrentHwProfileW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getnumberofeventlogrecords">GetNumberOfEventLogRecords 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getumscompletionlistevent">GetUmsCompletionListEvent 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-openbackupeventloga">OpenBackupEventLogA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-openbackupeventlogw">OpenBackupEventLogW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-queryumsthreadinformation">QueryUmsThreadInformation 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readencryptedfileraw">ReadEncryptedFileRaw 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readeventloga">ReadEventLogA 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readeventlogw">ReadEventLogW 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-umsthreadyield">UmsThreadYield 関数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winnls/nf-winnls-notifyuilanguagechange">NotifyUILanguageChange 関数 (winnls.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winperf/nc-winperf-pm_collect_proc">PM_COLLECT_PROC (winperf.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safercreatelevel">SaferCreateLevel 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safergetlevelinformation">SaferGetLevelInformation 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safergetpolicyinformation">SaferGetPolicyInformation 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-saferidentifylevel">SaferIdentifyLevel 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-saferrecordeventlogentry">SaferRecordEventLogEntry 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safersetlevelinformation">SaferSetLevelInformation 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safersetpolicyinformation">SaferSetPolicyInformation 関数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-addclipboardformatlistener">AddClipboardFormatListener 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-adjustwindowrectex">AdjustWindowRectEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-animatewindow">AnimateWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-appendmenua">AppendMenuA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-appendmenuw">AppendMenuW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-attachthreadinput">AttachThreadInput 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-bringwindowtotop">BringWindowToTop 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-callmsgfiltera">CallMsgFilterA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-callmsgfilterw">CallMsgFilterW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changedisplaysettingsa">ChangeDisplaySettingsA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changedisplaysettingsw">ChangeDisplaySettingsW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changewindowmessagefilterex">ChangeWindowMessageFilterEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkdlgbutton">CheckDlgButton 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkmenuitem">CheckMenuItem 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkmenuradioitem">CheckMenuRadioItem 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-childwindowfrompoint">ChildWindowFromPoint 関数 (winuser. h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-childwindowfrompointex">ChildWindowFromPointEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-closedesktop">CloseDesktop 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-closewindowstation">CloseWindowStation 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createcaret">CreateCaret 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdesktopa">CreateDesktopA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdesktopw">CreateDesktopW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdialogindirectparama">CreateDialogIndirectParamA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdialogindirectparamw">CreateDialogIndirectParamW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createiconindirect">CreateIconIndirect 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defrawinputproc">DefRawInputProc 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defwindowproca">DefWindowProcA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defwindowprocw">DefWindowProcW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-deletemenu">DeleteMenu 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-destroycaret">DestroyCaret 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-destroywindow">DestroyWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dialogboxindirectparama">DialogBoxIndirectParamA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dialogboxindirectparamw">DialogBoxIndirectParamW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessage">DispatchMessage 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessagea">DispatchMessageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessagew">DispatchMessageW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawedge">DrawEdge 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawicon">DrawIcon 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawiconex">DrawIconEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtexta">DrawTextA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextexa">DrawTextExA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextexw">DrawTextExW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextw">DrawTextW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enablemenuitem">EnableMenuItem 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enablescrollbar">EnableScrollBar 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-endmenu">EndMenu 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-endpaint">EndPaint 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesa">EnumDisplayDevicesA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesw">EnumDisplayDevicesW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-fillrect">FillRect 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-findwindowa">FindWindowA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-findwindoww">FindWindowW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-flashwindowex">FlashWindowEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getancestor">GetAncestor 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcapture">GetCapture 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcaretblinktime">GetCaretBlinkTime 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoa">GetClassInfoA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoexa">GetClassInfoExA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoexw">GetClassInfoExW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfow">GetClassInfoW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclasslongptra">GetClassLongPtrA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclasslongptrw">GetClassLongPtrW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassname">GetClassName 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassnamea">GetClassNameA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassnamew">GetClassNameW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassword">GetClassWord 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardformatnamea">GetClipboardFormatNameA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardformatnamew">GetClipboardFormatNameW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardviewer">GetClipboardViewer 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcursorpos">GetCursorPos 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdesktopwindow">GetDesktopWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemint">GetDlgItemInt 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemtexta">GetDlgItemTextA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemtextw">GetDlgItemTextW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdoubleclicktime">GetDoubleClickTime 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getfocus">GetFocus 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getforegroundwindow">GetForegroundWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getkeyboardstate">GetKeyboardState 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getlastactivepopup">GetLastActivePopup 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenudefaultitem">GetMenuDefaultItem 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuinfo">GetMenuInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuitemcount">GetMenuItemCount 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuitemid">GetMenuItemID 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuiteminfoa">GetMenuItemInfoA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuiteminfow">GetMenuItemInfoW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenustate">GetMenuState 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmessageextrainfo">GetMessageExtraInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmessagetime">GetMessageTime 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getparent">GetParent 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerdevices">GetPointerDevices 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerframeinfohistory">GetPointerFrameInfoHistory 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerframetouchinfo">GetPointerFrameTouchInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointertype">GetPointerType 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getprocessdefaultlayout">GetProcessDefaultLayout 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getrawinputdevicelist">GetRawInputDeviceList 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getscrollbarinfo">GetScrollBarInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getscrollinfo">GetScrollInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getshellwindow">GetShellWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsubmenu">GetSubMenu 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsystemmetrics">GetSystemMetrics 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsystemmetricsfordpi">GetSystemMetricsForDpi 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettitlebarinfo">GetTitleBarInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettopwindow">GetTopWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettouchinputinfo">GetTouchInputInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindow">GetWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtexta">GetWindowTextA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextlengtha">GetWindowTextLengthA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextlengthw">GetWindowTextLengthW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextw">GetWindowTextW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowthreadprocessid">GetWindowThreadProcessId 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-initializetouchinjection">InitializeTouchInjection 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-injecttouchinput">InjectTouchInput 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insendmessage">InSendMessage 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenua">InsertMenuA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuitema">InsertMenuItemA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuitemw">InsertMenuItemW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuw">InsertMenuW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-invalidaterect">InvalidateRect 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-invertrect">InvertRect 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isclipboardformatavailable">IsClipboardFormatAvailable 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdialogmessagea">IsDialogMessageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdialogmessagew">IsDialogMessageW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdlgbuttonchecked">IsDlgButtonChecked 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-ishungappwindow">IsHungAppWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isiconic">IsIconic 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-ismenu">IsMenu 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iswindowunicode">IsWindowUnicode 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iswineventhookinstalled">IsWinEventHookInstalled 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iszoomed">IsZoomed 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadicona">LoadIconA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadiconw">LoadIconW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadimagea">LoadImageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-locksetforegroundwindow">LockSetForegroundWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-lockworkstation">LockWorkStation 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-logicaltophysicalpoint">LogicalToPhysicalPoint 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-mapwindowpoints">MapWindowPoints 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messagebeep">MessageBeep 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messagebox">MessageBox 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxa">MessageBoxA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxindirecta">MessageBoxIndirectA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxindirectw">MessageBoxIndirectW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxw">MessageBoxW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-modifymenua">ModifyMenuA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-modifymenuw">ModifyMenuW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfrompoint">MonitorFromPoint 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfromrect">MonitorFromRect 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfromwindow">MonitorFromWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-oemtocharbuffa">OemToCharBuffA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-oemtocharbuffw">OemToCharBuffW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-openinputdesktop">OpenInputDesktop 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-peekmessagea">PeekMessageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-peekmessagew">PeekMessageW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-physicaltologicalpoint">PhysicalToLogicalPoint 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-postmessagea">PostMessageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-postmessagew">PostMessageW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-privateextracticonsa">PrivateExtractIconsA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-privateextracticonsw">PrivateExtractIconsW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-querydisplayconfig">QueryDisplayConfig 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-realgetwindowclassw">RealGetWindowClassW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-redrawwindow">RedrawWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerclipboardformata">RegisterClipboardFormatA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerclipboardformatw">RegisterClipboardFormatW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerdevicenotificationa">RegisterDeviceNotificationA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerdevicenotificationw">RegisterDeviceNotificationW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removemenu">RemoveMenu 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removepropa">RemovePropA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removepropw">RemovePropW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrolldc">ScrollDC 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrollwindow">ScrollWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrollwindowex">ScrollWindowEx 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-senddlgitemmessagea">SendDlgItemMessageA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-senddlgitemmessagew">SendDlgItemMessageW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-sendmessagetimeouta">SendMessageTimeoutA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-sendmessagetimeoutw">SendMessageTimeoutW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcaretblinktime">SetCaretBlinkTime 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setclipboarddata">SetClipboardData 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcoalescabletimer">SetCoalescableTimer 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcursorpos">SetCursorPos 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdlgitemtexta">SetDlgItemTextA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdlgitemtextw">SetDlgItemTextW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdoubleclicktime">SetDoubleClickTime 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setfocus">SetFocus 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setforegroundwindow">SetForegroundWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setlayeredwindowattributes">SetLayeredWindowAttributes 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuinfo">SetMenuInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuiteminfoa">SetMenuItemInfoA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuiteminfow">SetMenuItemInfoW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmessageextrainfo">SetMessageExtraInfo 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setparent">SetParent 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setprocesswindowstation">SetProcessWindowStation 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setpropa">SetPropA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setpropw">SetPropW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowdisplayaffinity">SetWindowDisplayAffinity 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlonga">SetWindowLongA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongptra">SetWindowLongPtrA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongptrw">SetWindowLongPtrW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongw">SetWindowLongW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowpos">SetWindowPos 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowshookexa">SetWindowsHookExA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowshookexw">SetWindowsHookExW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowtexta">SetWindowTextA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowtextw">SetWindowTextW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showscrollbar">ShowScrollBar 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showwindow">ShowWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showwindowasync">ShowWindowAsync 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-shutdownblockreasoncreate">ShutdownBlockReasonCreate 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-shutdownblockreasondestroy">ShutdownBlockReasonDestroy 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-systemparametersinfoa">SystemParametersInfoA 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-systemparametersinfow">SystemParametersInfoW 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-trackmouseevent">TrackMouseEvent 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-updatelayeredwindow">UpdateLayeredWindow 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-waitforinputidle">WaitForInputIdle 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-waitmessage">WaitMessage 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-windowfromdc">WindowFromDC 関数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsdisconnectsession">WTSDisconnectSession 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememory">WTSFreeMemory 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememoryexa">WTSFreeMemoryExA 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememoryexw">WTSFreeMemoryExW 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtslogoffsession">WTSLogoffSession 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsvirtualchannelopenex">WTSVirtualChannelOpenEx 関数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsvirtualchannelquery">WTSVirtualChannelQuery 関数 (wtsapi32.h) </a></li>
</ul>

### <a name="uwp-api-reference"></a>UWP API リファレンス

<ul>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothcachemode">Windows.Devices.Bluetooth.BluetoothCacheMode</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.fromidasync">Windows.Devices.Bluetooth.BluetoothLEDevice.FromIdAsync(System.String)</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.gattserviceschanged">Windows.Devices.Bluetooth.BluetoothLEDevice.GattServicesChanged</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.namechanged">Windows.Devices.Bluetooth.BluetoothLEDevice.NameChanged</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.patterninterface">Windows.UI.Xaml.Automation.Peers.PatternInterface</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.candragitems">Windows.UI.Xaml.Controls.ListViewBase.CanDragItems</a></li>
</ul>


