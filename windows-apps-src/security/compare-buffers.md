---
title: バッファーの比較
description: 次のコード例は、ユニバーサル Windows プラットフォーム (UWP) アプリでバッファーを比較する方法を示しています。
ms.assetid: CB086E51-544A-470D-B7C8-C055271CD615
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、uwp、セキュリティ
ms.localizationpriority: medium
ms.openlocfilehash: 139514166d623dc9a621b533cd3ce4bb7fdea0c5
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855442"
---
# <a name="compare-buffers"></a>バッファーの比較



次のコード例は、ユニバーサル Windows プラットフォーム (UWP) アプリでバッファーを比較する方法を示しています。

```cs
public void CompareBuffers()
{
    // Create a hexadecimal string.
    String strHex = "30310aff";

    // Create a Base64 string that is equivalent to strHex.
    String strBase64v1 = "MDEK/w==";

    // Create a Base64 string that is not equivalent to strHex.
    String strBase64v2 = "KEDM/w==";

    // Decode strHex to a buffer.
    IBuffer buff1 = CryptographicBuffer.DecodeFromHexString(strHex);

    // Decode strBase64v1 to a buffer.
    IBuffer buff2 = CryptographicBuffer.DecodeFromBase64String(strBase64v1);

    // Decode strBase64v2 to a buffer.
    IBuffer buff3 = CryptographicBuffer.DecodeFromBase64String(strBase64v2);

    // Compare the hexadecimal-decoded buff1 to the Base64-decoded buff2.
    // The code points in the two buffers are equal, and the Boolean value
    // is true.
    Boolean bVal_1 = CryptographicBuffer.Compare(buff1, buff2);

    // Compare the hexadecimal-decoded buff1 to the Base64-decoded buff3.
    // The code points in the two buffers are not equal, and the Boolean value
    // is false.
    Boolean bVal_2 = CryptographicBuffer.Compare(buff1, buff3);
}
```