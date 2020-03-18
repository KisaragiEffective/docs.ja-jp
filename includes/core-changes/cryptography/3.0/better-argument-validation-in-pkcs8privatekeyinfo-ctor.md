---
ms.openlocfilehash: 8d3a8712528d2d35c706cc26b8c388b65d6ad506
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77449221"
---
### <a name="better-argument-validation-in-the-pkcs8privatekeyinfo-constructor"></a>Pkcs8PrivateKeyInfo コンストラクターでの引数の検証の改善

.NET Core 3.0 Preview 9 以降では、`Pkcs8PrivateKeyInfo` コンストラクターが `algorithmParameters` パラメーターを単一の BER でエンコードされた値として検証します。

#### <a name="change-description"></a>変更の説明

.NET Core 3.0 Preview 9 以前では、[`Pkcs8PrivateKeyInfo` コンストラクター](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean))では、`algorithmParameters` 引数が検証されていませんでした。  この引数が不正な値を表した場合、コンストラクターは成功していましたが、引数が許容されないものであった場合 (<xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode>)、<xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncode%2A>、<xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encrypt%2A>、<xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncrypt%2A>、または <xref:System.ArgumentException> メソッドのいずれかの呼び出しに対し、`preEncodedValue` または <xref:System.Security.Cryptography.CryptographicException> のいずれかがスローされていました。

Preview 9 より前の .NET Core 3.0 で実行した場合、<xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode> メソッドが呼び出された場合のみ、次のコードで例外がスローされます。

```csharp
byte[] algorithmParameters = { 0x05, 0x00, 0x05, 0x00 };

var info = new Pkcs8PrivateKeyInfo(algorithmId, algorithmParameters, privateKey);
// This line throws an ArgumentException for `preEncodedValue`:
byte[] encoded = info.Encode();
```

この引数は、.NET Core 3.0 Preview 9 からはコンストラクター内で検証され、値が不正な場合、メソッドにより <xref:System.Security.Cryptography.CryptographicException> がスローされます。 この変更により、例外はデータ エラーのソースに近づきます。 次に例を示します。

```csharp
byte[] algorithmParameters = { 0x05, 0x00, 0x05, 0x00 };

// This line throws a CryptographicException
var info = new Pkcs8PrivateKeyInfo(algorithmId, algorithmParameters, privateKey);
```

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 9

#### <a name="recommended-action"></a>推奨アクション

例外の処理が必要な場合、有効な `algorithmParameters` 値のみを指定するようにします。そのようにしなければ、`Pkcs8PrivateKeyInfo` コンストラクターへのその呼び出しは <xref:System.ArgumentException> と <xref:System.Security.Cryptography.CryptographicException> の両方に対してテストされます。

### <a name="category"></a>カテゴリ

暗号

### <a name="affected-apis"></a>影響を受ける API

- [Pkcs8PrivateKeyInfo コンストラクター](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean))

<!--

### Affected APIs

- `M:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.#ctor(System.Security.Cryptography.Oid,System.Nullable{System.ReadOnlyMemory{System.Byte}},System.ReadOnlyMemory{System.Byte},System.Boolean)`

-->
