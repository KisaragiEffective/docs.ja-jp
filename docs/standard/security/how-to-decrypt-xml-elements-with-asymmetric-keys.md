---
title: '方法: 非対称キーで XML 要素を復号化する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- System.Security.Cryptography.RSACryptoServiceProvider class
- asymmetric keys
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- decryption
ms.assetid: dd5de491-dafe-4b94-966d-99714b2e754a
ms.openlocfilehash: 5ed1e2eb2518366da0a57655d7a8691e23f725d5
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706137"
---
# <a name="how-to-decrypt-xml-elements-with-asymmetric-keys"></a>方法: 非対称キーで XML 要素を復号化する
<xref:System.Security.Cryptography.Xml> 名前空間のクラスを使用して、XML ドキュメント内の要素を暗号化および復号化することができます。  XML 暗号化は、データが簡単に読み取られる心配なく、暗号化された XML データを交換または保存する標準的な方法です。  XML 暗号化標準の詳細については、「World Wide Web コンソーシアム (W3C) 勧告」 [Xml 署名の構文と処理](https://www.w3.org/TR/xmldsig-core/)に関する説明を参照してください。  
  
 この手順の例では、「[方法: 非対称キーで Xml 要素を暗号化](../../../docs/standard/security/how-to-encrypt-xml-elements-with-asymmetric-keys.md)する」で説明されているメソッドを使用して、暗号化された xml 要素を復号化します。  <`EncryptedData`> 要素を検索し、要素を復号化してから、要素を元のプレーンテキスト XML 要素に置き換えます。  
  
 この例では、2 つのキーを使用して XML 要素を復号化します。  以前に生成された RSA 秘密キーをキーコンテナーから取得した後、RSA キーを使用して、<`EncryptedData`> 要素の <`EncryptedKey`> 要素に格納されているセッションキーの暗号化を解除します。  例では、セッション キーを使用して XML 要素を復号化します。  
  
 この例は、複数のアプリケーションが暗号化されたデータを共有する必要がある状況や、1 つのアプリケーションが、実行する時間の間に暗号化されたデータを保存する必要がある状況に適しています。  
  
### <a name="to-decrypt-an-xml-element-with-an-asymmetric-key"></a>非対称キーで XML 要素を復号化するには  
  
1. <xref:System.Security.Cryptography.CspParameters> オブジェクトを作成し、キーのコンテナーの名前を指定します。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#2)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#2)]  
  
2. <xref:System.Security.Cryptography.RSACryptoServiceProvider> オブジェクトを使用して、コンテナーから以前に生成された非対称キーを取得します。  <xref:System.Security.Cryptography.CspParameters> オブジェクトを <xref:System.Security.Cryptography.RSACryptoServiceProvider> コンストラクターに渡すと、キー コンテナーからキーが自動的に取得されます。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#3)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#3)]  
  
3. <xref:System.Security.Cryptography.Xml.EncryptedXml> オブジェクトを新規作成してドキュメントを復号化します。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#5)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#5)]  
  
4. 復号化が必要なドキュメント内で RSA キーと要素と関連付けるには、キーと名前のマッピングを追加します。  ドキュメントを暗号化したときに使用したキーと同じ名前を使用する必要があります。  この名前は、手順 1 で指定されたキー コンテナー内のキーを識別するために使用する名前とは別であることに注意してください。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#6)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#6)]  
  
5. <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> メソッドを呼び出して、<`EncryptedData`> 要素の暗号化を解除します。  このメソッドは、RSA キーを使用してセッション キーを復号化してから、自動的にセッション キーを使用して、XML 要素を復号化します。  また、<`EncryptedData`> 要素を元のプレーンテキストに自動的に置き換えます。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#7)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#7)]  
  
6. XML ドキュメントを保存します。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#8)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#8)]  
  
## <a name="example"></a>使用例  
 この例では、`test.xml` という名前のファイルがコンパイル済みのプログラムと同じディレクトリに存在することを前提としています。  また、「[方法: 非対称キーで Xml 要素を暗号化](../../../docs/standard/security/how-to-encrypt-xml-elements-with-asymmetric-keys.md)する」で説明されている手法を使用して暗号化された xml 要素が `test.xml` に含まれていることを前提としています。  
  
 [!code-csharp[HowToDecryptXMLElementAsymmetric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#1)]
 [!code-vb[HowToDecryptXMLElementAsymmetric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル方法  
  
- この例をコンパイルするには、`System.Security.dll` への参照を含める必要があります。  
  
- 名前空間 <xref:System.Xml>、<xref:System.Security.Cryptography>、および <xref:System.Security.Cryptography.Xml> を含めます。  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 対称暗号化キーをプレーンテキストで保存したり、対称キーをコンピューター間でプレーンテキストで転送したりしないでください。  加えて、非対称キー ペアの秘密キーをプレーンテキストで保存または転送しないでください。  対称暗号化キーと非対称暗号化キーの詳細については、[暗号化と復号化キーを生成する](../../../docs/standard/security/generating-keys-for-encryption-and-decryption.md)を参照してください。  
  
 キーをソース コードに直接埋め込まないでください。  埋め込みキーは、 [ildasm.exe (IL 逆アセンブラー)](../../../docs/framework/tools/ildasm-exe-il-disassembler.md)を使用するか、メモ帳などのテキストエディターでアセンブリを開くことによって、簡単にアセンブリから読み取ることができます。  
  
 暗号化キーを使用して完了したら、各バイトをゼロ (0) にするか、マネージド暗号化クラスの <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> メソッドを呼び出してメモリから消去します。  暗号化キーは、デバッガーによってメモリから読み取られるか、メモリの位置がディスクにページングされている場合はハード ドライブから読み取られることがあります。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Security.Cryptography.Xml>
- [方法: 非対称キーで XML 要素を暗号化する](../../../docs/standard/security/how-to-encrypt-xml-elements-with-asymmetric-keys.md)
