---
title: .NET Framework の暗号モデル
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- cryptography [.NET Framework], model
- encryption [.NET Framework], model
ms.assetid: 12fecad4-fbab-432a-bade-2f05976a2971
ms.openlocfilehash: f878f73497b83aaf31f2ba3b23cca1f685867b3e
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095269"
---
# <a name="net-framework-cryptography-model"></a>.NET Framework の暗号モデル

.NET Framework は、多くの標準的な暗号化アルゴリズムの実装を提供します。 これらのアルゴリズムは簡単に使用でき、またできるだけ安全な既定のプロパティを提供しています。 さらに、オブジェクトの継承、ストリームの設計、および構成の .NET Framework の暗号モデルは非常に拡張性に優れています。

## <a name="object-inheritance"></a>オブジェクトの継承

.NET Framework のセキュリティ システムは、拡張可能な派生クラス継承のパターンを実装しています。 階層は次のとおりです。

- <xref:System.Security.Cryptography.SymmetricAlgorithm>、<xref:System.Security.Cryptography.AsymmetricAlgorithm> または <xref:System.Security.Cryptography.HashAlgorithm> などのアルゴリズム型クラス。 このレベルは抽象レベルです。

- <xref:System.Security.Cryptography.Aes>、<xref:System.Security.Cryptography.RC2>、または <xref:System.Security.Cryptography.ECDiffieHellman> など、アルゴリズム型クラスから継承されるアルゴリズム クラス。 このレベルは抽象レベルです。

- <xref:System.Security.Cryptography.AesManaged>、<xref:System.Security.Cryptography.RC2CryptoServiceProvider>、または <xref:System.Security.Cryptography.ECDiffieHellmanCng> など、アルゴリズム クラスから継承されるアルゴリズム クラスの実装。 このレベルは完全に実装されます。

派生クラスのこのパターンを使用すると、新しいアルゴリズムの追加、または既存アルゴリズムの新規実装の追加を簡単に行えます。 たとえば、新しい公開キー アルゴリズムを作成するには、<xref:System.Security.Cryptography.AsymmetricAlgorithm> クラスから継承します。 特定のアルゴリズムの実装を新しく作成するには、そのアルゴリズムの非抽象派生クラスを作成します。

## <a name="how-algorithms-are-implemented-in-the-net-framework"></a>.NET Framework でアルゴリズムを実装する方法

アルゴリズムに使用できるさまざまな実装例として、対称アルゴリズムを検討します。 すべての対称アルゴリズムのベースは <xref:System.Security.Cryptography.SymmetricAlgorithm> であり、次のアルゴリズムによって継承されます。

* <xref:System.Security.Cryptography.Aes>
* <xref:System.Security.Cryptography.DES>
* <xref:System.Security.Cryptography.RC2>
* <xref:System.Security.Cryptography.Rijndael>
* <xref:System.Security.Cryptography.TripleDES>

<xref:System.Security.Cryptography.Aes> は、<xref:System.Security.Cryptography.AesCryptoServiceProvider> と <xref:System.Security.Cryptography.AesManaged> の 2 つのクラスによって継承されます。 <xref:System.Security.Cryptography.AesCryptoServiceProvider> クラスは Aes の Windows 暗号化 API (CAPI) 実装のラッパーですが、<xref:System.Security.Cryptography.AesManaged> クラスは全体がマネージド コードで書かれています。 さらに、マネージド実装と CAPI 実装に加え、3 つ目の実装、Cryptography Next Generation (CNG) もあります。 CNG アルゴリズムの例が <xref:System.Security.Cryptography.ECDiffieHellmanCng> です。 CNG アルゴリズムは、Windows Vista 以降のバージョンで利用可能です。

ご自身にとって最適な実装を選択できます。 マネージ実装は、.NET Framework をサポートするすべてのプラットフォームで使用できます。 CAPI 実装は、以前のオペレーティングシステムで使用でき、開発されなくなりました。 CNG は、新しい開発が行われる最新の実装です。 ただし、マネージド実装は連邦情報処理規格 (FIPS: Federal Information Processing Standard) に認定されておらず、ラッパー クラスよりも低速である場合があります。

## <a name="stream-design"></a>ストリーム デザイン

共通言語ランタイムは、対称アルゴリズムおよびハッシュ アルゴリズムを実装するためのストリーム指向デザインを使用しています。 この設計の中心となるは、<xref:System.Security.Cryptography.CryptoStream> クラスから派生する <xref:System.IO.Stream> クラスです。 ストリーム ベースの暗号化オブジェクトは、単一の標準インターフェイス (`CryptoStream`) をサポートし、オブジェクトのデータ転送部分を処理します。 すべてのオブジェクトは標準のインターフェイス上に構築されるため、複数のオブジェクト (ハッシュ オブジェクトに続く暗号化オブジェクトなど) を連結したり、データ用の中間ストレージなしでデータ上で複数の操作を実行したりできます。 また、ストリーミング モデルを使用して、より小さなオブジェクトからオブジェクトを構築することもできます。 たとえば、複合暗号化とハッシュ アルゴリズムは 1 つのストリーム オブジェクトと見ることができますが、このオブジェクトは一連の複数のストリーム オブジェクトから作成されているかもしれません。

## <a name="cryptographic-configuration"></a>暗号化の構成

暗号化の構成によって、アルゴリズムの特定の実装のアルゴリズム名への解決が可能になり、.NET Framework の暗号化クラスの機能を拡張できます。 アルゴリズムの独自のハードウェアまたはソフトウェア実装を追加して、実装を任意のアルゴリズム名にマップすることができます。 構成ファイルでアルゴリズムを指定しない場合は、既定の設定が使用されます。 暗号化の構成の詳細については、「[暗号化クラスの構成](../../../docs/framework/configure-apps/configure-cryptography-classes.md)」を参照してください。

## <a name="choosing-an-algorithm"></a>アルゴリズムの選択

データの整合性、データのプライバシー保護、またはキー生成など、さまざまな理由のためにアルゴリズムを選択することができます。 対称アルゴリズムおよびハッシュ アルゴリズムは、整合性の理由 (変更の防止) またはプライバシー上の理由 (表示の防止) のいずれかのためにデータを保護することを意図しています。 ハッシュ アルゴリズムは、主にデータの整合性用に使用されます。

アプリケーションで推奨されるアルゴリズムの一覧を示します。

- データのプライバシー :
  - <xref:System.Security.Cryptography.Aes>
- データの整合性 :
  - <xref:System.Security.Cryptography.HMACSHA256>
  - <xref:System.Security.Cryptography.HMACSHA512>
- デジタル署名:
  - <xref:System.Security.Cryptography.ECDsa>
  - <xref:System.Security.Cryptography.RSA>
- キー交換 :
  - <xref:System.Security.Cryptography.ECDiffieHellman>
  - <xref:System.Security.Cryptography.RSA>
- 乱数生成 :
  - <xref:System.Security.Cryptography.RNGCryptoServiceProvider>
- パスワードからのキー生成 :
  - <xref:System.Security.Cryptography.Rfc2898DeriveBytes>

## <a name="see-also"></a>参照

- [暗号サービス](../../../docs/standard/security/cryptographic-services.md)
- [C での暗号化プロトコル、アルゴリズム、およびソースコードの適用 (Schneier)](https://www.schneier.com/books/applied_cryptography/)
