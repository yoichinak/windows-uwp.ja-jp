---
description: HTTP 2.0 プロトコルと HTTP 1.1 プロトコルを使って情報を送受信するには、HttpClient とその他の Windows.Web.Http 名前空間 API を使います。
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 6/5/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb098aae346c7a81771262793f5f6a042d62d5a3
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721610"
---
# <a name="httpclient"></a>HttpClient

**重要な API**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

HTTP 2.0 プロトコルと HTTP 1.1 プロトコルを使って情報を送受信するには、[**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) とその他の [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 名前空間 API を使います。

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient と Windows.Web.Http 名前空間の概要

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 名前空間、関連する [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 名前空間、[**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters) 名前空間のクラスには、基本的な GET 要求を実行したり、次のようなさらに高度な HTTP 機能を実装したりするための HTTP クライアントとして動作する、ユニバーサル Windows プラットフォーム (UWP) アプリ用のプログラミング インターフェイスが用意されています。

-   一般的な動詞 (**DELETE**、**GET**、**PUT**、**POST**) に対応するメソッド。 これらの各要求は、非同期操作として送られます。

-   一般的な認証設定とパターンのサポート。

-   トランスポートに関する Secure Sockets Layer (SSL) 詳細へのアクセス。

-   カスタマイズされたフィルターを高度なアプリに含める機能。

-   Cookie を取得、設定、削除する機能。

-   非同期メソッドで利用可能な HTTP 要求の進行状況情報。

[  **Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) クラスは、[**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) から送られた HTTP 要求メッセージを表します。 [  **Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) クラスは、HTTP 要求から受け取った HTTP 応答メッセージを表します。 HTTP メッセージは、IETF によって [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) で規定されています。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 名前空間は、クッキーを含む HTTP エンティティ ボディおよびヘッダーとして HTTP コンテンツを表します。 HTTP コンテンツは、HTTP 要求または HTTP 応答に関連付けることができます。 **Windows.Web.Http** 名前空間には、HTTP コンテンツを表す多数のさまざまなクラスが用意されています。

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent)します。 バッファーとしてのコンテンツ。
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent)します。 **application/x-www-form-urlencoded** MIME タイプでエンコードされた名前と値の組としてのコンテンツ。
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent)します。 コンテンツの形式で、**マルチパート/\***  MIME の種類。
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent)します。 **multipart/form-data** MIME タイプとしてエンコードされているコンテンツ。
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent)します。 ストリームとしてのコンテンツ (この内部タイプは、HTTP GET メソッドでのデータの受信、および HTTP POST メソッドでのデータのアップロードに使われます)。
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent)します。 文字列としてのコンテンツ。
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -独自のコンテンツ オブジェクトを作成する開発者向けの基本インターフェイス

「HTTP 経由でシンプルな GET 要求を送信する」セクションのコード スニペットでは、[**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) クラスを使って、HTTP GET 要求からの HTTP 応答を文字列として表しています。

[  **Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 名前空間では、HTTP ヘッダーと Cookie の作成がサポートされます。これらはプロパティとして、[**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) オブジェクトと [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) オブジェクトに関連付けられます。

## <a name="send-a-simple-get-request-over-http"></a>HTTP 経由でシンプルな GET 要求を送信する

この記事の中で既に説明したように、[**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 名前空間は、UWP アプリで GET 要求を送信できるようにします。 次のコード スニペットは、GET 要求を送信する方法を示します http://www.contoso.comを使用して、 [ **Windows.Web.Http.HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)クラスおよび[ **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) GET 要求から応答を読み取るクラス。

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>HTTP 経由で投稿のバイナリ データ

[C +/cli WinRT](/windows/uwp/cpp-and-winrt-apis)フォーム データと POST 要求を使用して、web サーバーにファイルのアップロードと少量のバイナリ データを送信する次のコード例を示しています。 コードを使用して、 [ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent)バイナリのデータを表すクラスと[ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent)クラスマルチパート フォーム データを表します。

> [!NOTE]
> 呼び出す**取得**(として次のコード例でわかる) が適切でない UI スレッドにします。 その場合に使用する正しい方法を参照してください。[同時実行と非同期操作を C +/cli WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)します。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

(上記で使用する明示的なバイナリ データではなく) 実際のバイナリ ファイルの内容を投稿することがわかりますが使いやすく、 [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent)オブジェクト。 構築して、そのコンス トラクターに引数として渡す呼び出しから返される値[StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync)します。 そのメソッドは、バイナリ ファイル内のデータのストリームを返します。

また、大きなファイル (約 10 MB より大きい) をアップロードしている場合をお勧め、Windows ランタイムを使用すること[バック グラウンド転送](/uwp/api/windows.networking.backgroundtransfer)Api。

## <a name="post-json-data-over-http"></a>HTTP 経由での POST JSON データ

次の例は、エンドポイントをいくつかの JSON を投稿し、応答本文を書き込みます。

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http の例外

Uniform Resource Identifier (URI) として無効な文字列が、[**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) オブジェクトのコンストラクターに渡されると、例外がスローされます。

**.NET:**   、 [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)種類[ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)でC#とVB.

C# や Visual Basic では、.NET 4.5 の [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) クラスと、[**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) メソッドの 1 つを使って、URI が作成される前にユーザーから受け取った文字列をテストすることによって、このエラーを回避できます。

C++ では、URI として渡される文字列を試行して解析するメソッドはありません。 アプリがユーザーから [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) の入力を取得する場合、このコンストラクターを try/catch ブロックに配置する必要があります。 例外がスローされた場合、アプリは、ユーザーに通知し、新しいホスト名を要求することができます。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) には便利な関数がありません。 そのため、この名前空間の [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) と他のクラスを使うアプリは、**HRESULT** 値を使う必要があります。

.NET Framework 4.5 を使用してアプリC#、VB.NET、 [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN)例外が発生したときに、アプリの実行中にエラーを表します。 [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) プロパティは、特定の例外に割り当てられた **HRESULT** を返します。 [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) プロパティは、例外を説明するメッセージを返します。 使うことができる **HRESULT** 値は、*Winerror.h* ヘッダー ファイルに記載されています。 アプリは特定の **HRESULT** 値に対するフィルター処理を行い、例外の原因に応じてアプリの動作を変更できます。

Managed C++ を使うアプリでは、アプリの実行中に例外が発生したときに、[Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) がエラーを表します。 [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) プロパティは、特定の例外に割り当てられた **HRESULT** を返します。 [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) プロパティは、**HRESULT** 値に関連付けられた、システムが提供する文字列を返します。 使うことができる **HRESULT** 値は、*Winerror.h* ヘッダー ファイルに記載されています。 アプリは特定の **HRESULT** 値に対するフィルター処理を行い、例外の原因に応じてアプリの動作を変更できます。

ほとんどのパラメーター検証エラー、 **HRESULT**が返される**E\_INVALIDARG**します。 一部の無効なメソッド呼び出しの**HRESULT**が返される**E\_不正な\_メソッド\_呼び出す**。

