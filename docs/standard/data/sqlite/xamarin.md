---
title: Xamarin の制限事項
ms.date: 12/13/2019
description: Xamarin の使用時に発生する制限事項について説明します。
ms.openlocfilehash: 192f25954726919dc66d706e755e0853404b4d85
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450406"
---
# <a name="xamarin-limitations"></a>Xamarin の制限事項

.NET Standard 2.0 は、Xamarin でサポートされています。 次の表は、既定の SQLitePCLRaw バンドルによってのネイティブ SQLite バイナリが提供されるプラットフォームを示しています。 別のバンドルの使用、または独自のネイティブ SQLite バイナリの提供の詳細については、「[カスタム sqlite バージョン](custom-versions.md)」を参照してください。

| Platform | SQLite バイナリ |
| --- | --- |
| **Xamarin.Android** | — |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm64-v8a` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`armeabi-v7a` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86_64` | ✔ |
| **Xamarin.iOS** | ✔ |
| **Xamarin.Mac** | ✔ |
| **TVOS** | ✔ |
| **UWP** | — |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm64` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x64` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86` | ✔ |

## <a name="ios"></a>iOS

SQLitePCLRaw バンドルを自動的に初期化しようとしています。 残念ながら、Xamarin. iOS の事前 (AOT) コンパイルの制限により、試行は失敗し、次のエラーが表示されます。

> `SQLitePCL.raw.SetProvider()`を呼び出す必要があります。 バンドルパッケージを使用している場合、これは `SQLitePCL.Batteries.Init()`を呼び出すことによって行われます。

バンドルを初期化するには、次のコード行をアプリに追加してから、Microsoft. Data. Sqlite を使用します。

```csharp
SQLitePCL.Batteries_V2.Init();
```

## <a name="see-also"></a>関連項目

* [非同期の制限事項](async.md)
* [カスタム SQLite バージョン](custom-versions.md)
