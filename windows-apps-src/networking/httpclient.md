---
description: HTTP 2.0 プロトコルと HTTP 1.1 プロトコルを使って情報を送受信するには、HttpClient とその他の Windows.Web.Http 名前空間 API を使います。
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 06/05/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3a46d04cec817dfcacb9c2bab8433a3eb4e363b
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104693"
---
# <a name="httpclient"></a>HttpClient

**重要な API**

-   [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)

HTTP 2.0 プロトコルと HTTP 1.1 プロトコルを使って情報を送受信するには、[**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) とその他の [**Windows.Web.Http**](/uwp/api/Windows.Web.Http) 名前空間 API を使います。

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient と Windows.Web.Http 名前空間の概要

[  **Windows.Web.Http**](/uwp/api/Windows.Web.Http) 名前空間、関連する [**Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers) 名前空間、[**Windows.Web.Http.Filters**](/uwp/api/Windows.Web.Http.Filters) 名前空間のクラスには、基本的な GET 要求を実行したり、次のようなさらに高度な HTTP 機能を実装したりするための HTTP クライアントとして動作する、ユニバーサル Windows プラットフォーム (UWP) アプリ用のプログラミング インターフェイスが用意されています。

-   一般的な動詞 (**DELETE**、**GET**、**PUT**、**POST**) に対応するメソッド。 これらの各要求は、非同期操作として送られます。

-   一般的な認証設定とパターンのサポート。

-   トランスポートに関する Secure Sockets Layer (SSL) 詳細へのアクセス。

-   カスタマイズされたフィルターを高度なアプリに含める機能。

-   Cookie を取得、設定、削除する機能。

-   非同期メソッドで利用可能な HTTP 要求の進行状況情報。

[  **Windows.Web.Http.HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage) クラスは、[**Windows.Web.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) から送られた HTTP 要求メッセージを表します。 [  **Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage) クラスは、HTTP 要求から受け取った HTTP 応答メッセージを表します。 HTTP メッセージは、IETF によって [RFC 2616](https://tools.ietf.org/html/rfc2616) で規定されています。

[  **Windows.Web.Http**](/uwp/api/Windows.Web.Http) 名前空間は、クッキーを含む HTTP エンティティ ボディおよびヘッダーとして HTTP コンテンツを表します。 HTTP コンテンツは、HTTP 要求または HTTP 応答に関連付けることができます。 **Windows.Web.Http** 名前空間には、HTTP コンテンツを表す多数のさまざまなクラスが用意されています。

-   [**HttpBufferContent**](/uwp/api/Windows.Web.Http.HttpBufferContent)。 バッファーとしてのコンテンツ。
-   [**HttpFormUrlEncodedContent**](/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent)。 **application/x-www-form-urlencoded** MIME タイプでエンコードされた名前と値の組としてのコンテンツ。
-   [**HttpMultipartContent**](/uwp/api/Windows.Web.Http.HttpMultipartContent)。 **multipart/\** _ MIME タイプ形式のコンテンツ。
-   [_ *HttpMultipartFormDataContent**](/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent)。 **multipart/form-data** MIME タイプとしてエンコードされているコンテンツ。
-   [**HttpStreamContent**](/uwp/api/Windows.Web.Http.HttpStreamContent)。 ストリームとしてのコンテンツ (この内部タイプは、HTTP GET メソッドでのデータの受信、および HTTP POST メソッドでのデータのアップロードに使われます)。
-   [**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent)。 文字列としてのコンテンツ。
-   [**IHttpContent**](/uwp/api/Windows.Web.Http.IHttpContent) - 開発者が独自のコンテンツ オブジェクトを作成するための基本インターフェイス

「HTTP 経由でシンプルな GET 要求を送信する」セクションのコード スニペットでは、[**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent) クラスを使って、HTTP GET 要求からの HTTP 応答を文字列として表しています。

[  **Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers) 名前空間では、HTTP ヘッダーと Cookie の作成がサポートされます。これらはプロパティとして、[**HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage) オブジェクトと [**HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage) オブジェクトに関連付けられます。

## <a name="send-a-simple-get-request-over-http"></a>HTTP 経由でシンプルな GET 要求を送信する

この記事の中で既に説明したように、[**Windows.Web.Http**](/uwp/api/Windows.Web.Http) 名前空間は、UWP アプリで GET 要求を送信できるようにします。 次のコード スニペットでは、[**Windows.Web.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) クラスを使って http:\//www.contoso.com に GET 要求を送信し、[**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage) クラスを使って GET 要求からの応答を読み取る方法を示します。

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

## <a name="post-binary-data-over-http"></a>HTTP 経由でバイナリ データを POST する

次の [C++/WinRT](../cpp-and-winrt-apis/index.md) コードでは、フォーム データと POST 要求を使って、少量のバイナリ データをファイルのアップロードとして Web サーバーに送信する例を示します。 そのコードでは、[**HttpBufferContent**](/uwp/api/windows.web.http.httpbuffercontent) クラスを使ってバイナリ データを表し、[**HttpMultipartFormDataContent**](/uwp/api/windows.web.http.httpmultipartformdatacontent) クラスを使ってマルチパートのフォーム データを表しています。

> [!NOTE]
> UI スレッドの場合は、(次のコード例で示されているように) **get** を呼び出すのは適切ではありません。 その場合に使用する正しい方法については、「[Concurrency and asynchronous operations with C++/WinRT](../cpp-and-winrt-apis/concurrency.md)」(C++/WinRT を使用した同時実行操作と非同期操作) をご覧ください。

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

実際のバイナリ ファイルの内容を POST するには (上で使用した明示的なバイナリ データではなく)、[HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) オブジェクトを使う方が簡単であることがわかります。 それを構築し、そのコンストラクターへの引数として、[StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync) の呼び出しから返された値を渡します。 そのメソッドからは、バイナリ ファイル内のデータに対するストリームが返されます。

また、大きいファイル (約 10 MB より大) をアップロードする場合は、Windows ランタイムの[バックグラウンド転送](/uwp/api/windows.networking.backgroundtransfer) API を使うことをお勧めします。

## <a name="post-json-data-over-http"></a>HTTP 経由で JSON データを POST する

次の例では、エンドポイントに JSON を POST した後、応答本文を書き出しています。

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

    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        Windows::Web::Http::HttpClient httpClient;
        Uri requestUri{ L"https://www.contoso.com/post" };

        // Construct the JSON to post.
        Windows::Web::Http::HttpStringContent jsonContent(
            L"{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding::Utf8,
            L"application/json");

        // Post the JSON, and wait for a response.
        httpResponseMessage = httpClient.PostAsync(
            requestUri,
            jsonContent).get();

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
        std::wcout << httpResponseBody.c_str();
    }
    catch (winrt::hresult_error const& ex)
    {
        std::wcout << ex.message().c_str();
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http の例外

Uniform Resource Identifier (URI) として無効な文字列が、[**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) オブジェクトのコンストラクターに渡されると、例外がスローされます。

**.NET:** [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) 型は、C# や VB では [**System.Uri**](/dotnet/api/system.uri) と表示されます。

C# や Visual Basic では、.NET 4.5 の [**System.Uri**](/dotnet/api/system.uri) クラスと、[**System.Uri.TryCreate**](/dotnet/api/system.uri.trycreate#overloads) メソッドの 1 つを使って、URI が作成される前にユーザーから受け取った文字列をテストすることによって、このエラーを回避できます。

C++ では、URI として渡される文字列を試行して解析するメソッドはありません。 アプリがユーザーから [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) の入力を取得する場合、このコンストラクターを try/catch ブロックに配置する必要があります。 例外がスローされた場合、アプリは、ユーザーに通知し、新しいホスト名を要求することができます。

[  **Windows.Web.Http**](/uwp/api/Windows.Web.Http) には便利な関数がありません。 そのため、この名前空間の [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) と他のクラスを使うアプリは、**HRESULT** 値を使う必要があります。

[C++/WinRT](../cpp-and-winrt-apis/index.md) を使用しているアプリでは、[**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 構造体はアプリの実行中に発生する例外を表します。 [winrt::hresult_error::code](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorcode-function) 関数では、特定の例外に割り当てられた **HRESULT** が返します。 [winrt::hresult_error::message](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 関数では、**HRESULT** 値に関連付けられた、システムが提供する文字列を返します。 詳細については、「[C++/WinRT でのエラー処理](../cpp-and-winrt-apis/error-handling.md)」を参照してください。

使うことができる **HRESULT** 値は、*Winerror.h* ヘッダー ファイルに記載されています。 アプリで特定の **HRESULT** 値に対するフィルター処理を行い、例外の原因に応じてアプリの動作を変更できます。

C# と VB.NET で .NET Framework 4.5 を使うアプリでは、アプリの実行中に例外が発生した場合、[System.Exception](/dotnet/api/system.exception) でエラーが表されます。 [System.Exception.HResult](/dotnet/api/system.exception.hresult#System_Exception_HResult) プロパティは、特定の例外に割り当てられた **HRESULT** を返します。 [System.Exception.Message](/dotnet/api/system.exception.message#System_Exception_Message) プロパティは、例外を説明するメッセージを返します。

C++/CX は、[C++/WinRT](../cpp-and-winrt-apis/index.md) に置き換えられました。 ただし、C++/CX を使うアプリでは、アプリの実行中に例外が発生したときに、[Platform::Exception](/cpp/cppcx/platform-exception-class) がエラーを表します。 [Platform::Exception::HResult](/cpp/cppcx/platform-exception-class#hresult) プロパティは、特定の例外に割り当てられた **HRESULT** を返します。 [Platform::Exception::Message](/cpp/cppcx/platform-exception-class#message) プロパティは、**HRESULT** 値に関連付けられた、システムが提供する文字列を返します。

ほとんどのパラメーター検証エラーの場合、返される **HRESULT** は **E\_INVALIDARG** です。 一部の無効なメソッド呼び出しでは、返される **HRESULT** は **E\_ILLEGAL\_METHOD\_CALL** です。