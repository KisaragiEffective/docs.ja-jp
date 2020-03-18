---
title: 暗号署名
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- digital signatures
- cryptography [.NET Framework], signatures
- digital signatures, XML signing
- signatures, cryptographic
- digital signatures, generating
- verifying signatures
- generating signatures
- digital signatures, about
- encryption [.NET Framework], signatures
- security [.NET Framework], signatures
- XML signing
- digital signatures, verifying
- signing XML
ms.assetid: aa87cb7f-e608-4a81-948b-c9b8a1225783
ms.openlocfilehash: 1de6b3f2eb30df270339910e7b8287101bde65ca
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706254"
---
# <a name="cryptographic-signatures"></a>暗号署名

暗号デジタル署名は、公開キー アルゴリズムを使用してデータの整合性を提供します。 デジタル署名を使用してデータに署名すると、第三者が署名を検証し、データが署名者から発信され、署名後に変更されていないことを証明できます。 デジタル署名の詳細については、「 [Cryptographic Services](../../../docs/standard/security/cryptographic-services.md)」を参照してください。

ここでは、 <xref:System.Security.Cryptography?displayProperty=nameWithType> 名前空間のクラスを使用してデジタル署名を生成して検証する方法について説明します。

## <a name="generating-signatures"></a>署名の生成

デジタル署名は、通常、大きなデータを表現するハッシュ値に適用されます。 ハッシュ値にデジタル署名を適用する例を次に示します。 まず、 <xref:System.Security.Cryptography.RSACryptoServiceProvider> クラスの新しいインスタンスを作成して、公開キー/秘密キーのペアを生成します。 次に、 <xref:System.Security.Cryptography.RSACryptoServiceProvider> を <xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter> クラスの新しいインスタンスに渡します。 これにより、デジタル署名を実際に実行する <xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter>に秘密キーが渡されます。 ハッシュ コードに署名するためには、使用するハッシュ アルゴリズムを指定する必要があります。 この例では、SHA1 アルゴリズムを使用します。 最後に、 <xref:System.Security.Cryptography.AsymmetricSignatureFormatter.CreateSignature%2A> メソッドを呼び出して署名を実行します。

SHA1 の衝突の問題のため、SHA256 以上をお勧めします。

```vb
Imports System.Security.Cryptography

Module Module1
    Sub Main()
        'The hash value to sign.
        Dim hashValue As Byte() = {59, 4, 248, 102, 77, 97, 142, 201, 210, 12, 224, 93, 25, 41, 100, 197, 213, 134, 130, 135}

        'The value to hold the signed value.
        Dim signedHashValue() As Byte

        'Generate a public/private key pair.
        Dim rsa As New RSACryptoServiceProvider()

        'Create an RSAPKCS1SignatureFormatter object and pass it
        'the RSACryptoServiceProvider to transfer the private key.
        Dim rsaFormatter As New RSAPKCS1SignatureFormatter(rsa)

        'Set the hash algorithm to SHA1.
        rsaFormatter.SetHashAlgorithm("SHA1")

        'Create a signature for hashValue and assign it to
        'signedHashValue.
        signedHashValue = rsaFormatter.CreateSignature(hashValue)
    End Sub
End Module
```

```csharp
using System;
using System.Security.Cryptography;

class Class1
{
   static void Main()
   {
      //The hash value to sign.
      byte[] hashValue = {59,4,248,102,77,97,142,201,210,12,224,93,25,41,100,197,213,134,130,135};

      //The value to hold the signed value.
      byte[] signedHashValue;

      //Generate a public/private key pair.
      RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();

      //Create an RSAPKCS1SignatureFormatter object and pass it the
      //RSACryptoServiceProvider to transfer the private key.
      RSAPKCS1SignatureFormatter rsaFormatter = new RSAPKCS1SignatureFormatter(rsa);

      //Set the hash algorithm to SHA1.
      rsaFormatter.SetHashAlgorithm("SHA1");

      //Create a signature for hashValue and assign it to
      //signedHashValue.
      signedHashValue = rsaFormatter.CreateSignature(hashValue);
   }
}
```

### <a name="signing-xml-files"></a>XML ファイルへの署名

.NET Framework に用意されている <xref:System.Security.Cryptography.Xml> 名前空間を使用すると、XML に署名できます。 XML が特定のソースから送信されたことを検証する場合は、XML への署名が重要です。 たとえば、XML を使用する株価情報サービスを使用している場合であれば、署名されているかどうかによって XML のソースを検証できます。

この名前空間のクラスは、World Wide Web コンソーシアムの「 [XML 署名の構文と処理に関する勧告](https://www.w3.org/TR/xmldsig-core/) 」に従っています。

## <a name="verifying-signatures"></a>署名の検証

データが特定の関係者によって署名されたことを検証するには、次の情報が必要です。

- データに署名した関係者の公開キー。

- デジタル署名。

- 署名されたデータ。

- 署名者が使用したハッシュ アルゴリズム。

<xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter> クラスによって署名された署名を検証するには、 <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> クラスを使用します。 <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> クラスに対しては、署名者の公開キーを提供する必要があります。 公開キーを指定するには、剰余値と指数部の値が必要になります (公開キーと秘密キーのペアを生成したパーティは、これらの値を提供する必要があります)。まず、署名を検証する公開キーを保持する <xref:System.Security.Cryptography.RSACryptoServiceProvider> オブジェクトを作成し、次に、<xref:System.Security.Cryptography.RSAParameters> 構造体を、公開キーを指定する剰余値と指数部の値に初期化します。

次のコードは、 <xref:System.Security.Cryptography.RSAParameters> 構造体の作成を示しています。 `Modulus` プロパティは `modulusData` というバイト配列の値に設定し、 `Exponent` プロパティは `exponentData`というバイト配列の値に設定します。

```vb
Dim rsaKeyInfo As RSAParameters
rsaKeyInfo.Modulus = modulusData
rsaKeyInfo.Exponent = exponentData
```

```csharp
RSAParameters rsaKeyInfo;
rsaKeyInfo.Modulus = modulusData;
rsaKeyInfo.Exponent = exponentData;
```

<xref:System.Security.Cryptography.RSAParameters> オブジェクトを作成した後、 <xref:System.Security.Cryptography.RSACryptoServiceProvider> クラスの新しいインスタンスを <xref:System.Security.Cryptography.RSAParameters>で指定した値に初期設定できます。 次に、 <xref:System.Security.Cryptography.RSACryptoServiceProvider> を <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> のコンストラクターに渡してキーを転送します。

このプロセスを説明する例を次に示します。 この例で、 `hashValue` と `signedHashValue` は、リモートにいる関係者から提供されるバイト配列です。 リモートにいる関係者は、SHA1 アルゴリズムを使用して `hashValue` に署名し、デジタル署名 `signedHashValue`を生成します。 <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter.VerifySignature%2A?displayProperty=nameWithType> メソッドは、デジタル署名が有効であり、`hashValue`に署名するために使用されたことを検証します。

```vb
Dim rsa As New RSACryptoServiceProvider()
rsa.ImportParameters(rsaKeyInfo)
Dim rsaDeformatter As New RSAPKCS1SignatureDeformatter(rsa)
rsaDeformatter.SetHashAlgorithm("SHA1")
If rsaDeformatter.VerifySignature(hashValue, signedHashValue) Then
   Console.WriteLine("The signature is valid.")
Else
   Console.WriteLine("The signature is not valid.")
End If
```

```csharp
RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();
rsa.ImportParameters(rsaKeyInfo);
RSAPKCS1SignatureDeformatter rsaDeformatter = new RSAPKCS1SignatureDeformatter(rsa);
rsaDeformatter.SetHashAlgorithm("SHA1");
if(rsaDeformatter.VerifySignature(hashValue, signedHashValue))
{
   Console.WriteLine("The signature is valid.");
}
else
{
   Console.WriteLine("The signature is not valid.");
}
```

上記のコードでは、署名が有効であれば "`The signature is valid`" を表示し、署名が無効であれば "`The signature is not valid`" を表示します。

## <a name="see-also"></a>関連項目

- [Cryptographic Services](../../../docs/standard/security/cryptographic-services.md)
