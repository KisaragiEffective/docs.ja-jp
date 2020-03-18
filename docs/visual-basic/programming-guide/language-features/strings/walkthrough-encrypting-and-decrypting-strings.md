---
title: 文字列の暗号化と複合化
ms.date: 07/20/2015
helpviewer_keywords:
- encryption [Visual Basic], strings
- strings [Visual Basic], encrypting
- decryption [Visual Basic], strings
- strings [Visual Basic], decrypting
ms.assetid: 1f51e40a-2f88-43e2-a83e-28a0b5c0d6fd
ms.openlocfilehash: 36e405c7362993471d3e6da8e319bccb854e1026
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343584"
---
# <a name="walkthrough-encrypting-and-decrypting-strings-in-visual-basic"></a>チュートリアル : Visual Basic での文字列の暗号化と複合化
このチュートリアルでは、<xref:System.Security.Cryptography.DESCryptoServiceProvider> クラスを使用して、トリプルデータ暗号化標準 (<xref:System.Security.Cryptography.TripleDES>) アルゴリズムの暗号化サービスプロバイダー (CSP) バージョンを使用して文字列を暗号化および復号化する方法について説明します。 最初の手順では、3DES アルゴリズムをカプセル化し、暗号化されたデータを base-64 エンコード文字列として格納する単純なラッパークラスを作成します。 次に、このラッパーを使用して、プライベートユーザーデータをパブリックにアクセスできるテキストファイルに安全に格納します。  
  
 暗号化を使用してユーザーシークレット (パスワードなど) を保護し、承認されていないユーザーが資格情報を読み取れないようにすることができます。 これにより、承認されたユーザーの id を盗難から保護し、ユーザーの資産を保護し、否認不可を提供できます。 また、暗号化を使用すると、承認されていないユーザーがユーザーのデータにアクセスするのを防ぐことができます。  
  
 詳細については、「[暗号サービス](../../../../standard/security/cryptographic-services.md)」をご覧ください。  
  
> [!IMPORTANT]
> Rijndael (現時点では Advanced Encryption Standard [AES]) と Triple Data Encryption Standard (3DES) アルゴリズムは、計算に負荷がかかるため、DES よりも強力なセキュリティを提供します。 詳細については、「<xref:System.Security.Cryptography.DES>」および「<xref:System.Security.Cryptography.Rijndael>」を参照してください。  
  
### <a name="to-create-the-encryption-wrapper"></a>暗号化ラッパーを作成するには  
  
1. `Simple3Des` クラスを作成して、暗号化と復号化の方法をカプセル化します。  
  
     [!code-vb[VbVbalrStrings#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#38)]  
  
2. 暗号化名前空間のインポートを、`Simple3Des` クラスを含むファイルの先頭に追加します。  
  
     [!code-vb[VbVbalrStrings#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#77)]  
  
3. `Simple3Des` クラスで、3DES 暗号化サービスプロバイダーを格納するためのプライベートフィールドを追加します。  
  
     [!code-vb[VbVbalrStrings#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#39)]  
  
4. 指定したキーのハッシュから、指定した長さのバイト配列を作成するプライベートメソッドを追加します。  
  
     [!code-vb[VbVbalrStrings#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#41)]  
  
5. 3DES 暗号化サービスプロバイダーを初期化するコンストラクターを追加します。  
  
     `key` パラメーターは、`EncryptData` メソッドと `DecryptData` メソッドを制御します。  
  
     [!code-vb[VbVbalrStrings#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#40)]  
  
6. 文字列を暗号化するパブリックメソッドを追加します。  
  
     [!code-vb[VbVbalrStrings#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#42)]  
  
7. 文字列を復号化するパブリックメソッドを追加します。  
  
     [!code-vb[VbVbalrStrings#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#43)]  
  
     これで、ラッパークラスを使用してユーザー資産を保護できるようになりました。 この例では、プライベートユーザーデータをパブリックにアクセスできるテキストファイルに安全に格納するために使用されます。  
  
### <a name="to-test-the-encryption-wrapper"></a>暗号化ラッパーをテストするには  
  
1. 別のクラスで、ラッパーの `EncryptData` メソッドを使用して文字列を暗号化し、ユーザーの [マイドキュメント] フォルダーに書き込むメソッドを追加します。  
  
     [!code-vb[VbVbalrStrings#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#78)]  
  
2. 暗号化された文字列をユーザーのマイドキュメントフォルダーから読み取り、その文字列をラッパーの `DecryptData` メソッドで復号化するメソッドを追加します。  
  
     [!code-vb[VbVbalrStrings#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#79)]  
  
3. `TestEncoding` および `TestDecoding` メソッドを呼び出すユーザーインターフェイスコードを追加します。  
  
4. アプリケーションを実行します。  
  
     アプリケーションをテストするときに、間違ったパスワードを指定した場合、データの暗号化が解除されないことに注意してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Security.Cryptography>
- <xref:System.Security.Cryptography.DESCryptoServiceProvider>
- <xref:System.Security.Cryptography.DES>
- <xref:System.Security.Cryptography.TripleDES>
- <xref:System.Security.Cryptography.Rijndael>
- [暗号サービス](../../../../standard/security/cryptographic-services.md)
