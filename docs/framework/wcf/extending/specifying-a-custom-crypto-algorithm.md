---
description: '詳細情報: カスタム暗号アルゴリズムの指定'
title: カスタム暗号アルゴリズムの指定
ms.date: 03/30/2017
ms.assetid: d662a305-8e09-451d-9a59-b0f12b012f1d
ms.openlocfilehash: 4d4cb525b46b33a0d0df8dd6a3db9e9fafe84f8f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643970"
---
# <a name="specifying-a-custom-crypto-algorithm"></a>カスタム暗号アルゴリズムの指定

WCF では、データの暗号化やデジタル署名の計算を行う際に使用するカスタム暗号アルゴリズムを指定できます。 そのためには、次の手順に従います。  
  
1. <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> の派生クラスを作成します。  
  
2. アルゴリズムを登録します。  
  
3. <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> の派生クラスを使用してバインディングを構成します。  
  
## <a name="derive-a-class-from-securityalgorithmsuite"></a>SecurityAlgorithmSuite の派生クラスの作成  

 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> は、セキュリティ関連のさまざまな操作を実行する際に使用するアルゴリズムを指定できるようにする抽象基本クラスです。 たとえば、デジタル署名のハッシュ計算やメッセージの暗号化などの操作です。 次のコードは、<xref:System.ServiceModel.Security.SecurityAlgorithmSuite> から派生クラスを作成する方法を示しています。  
  
```csharp  
public class MyCustomAlgorithmSuite : SecurityAlgorithmSuite  
    {  
        public override string DefaultAsymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaOaepKeyWrap; }  
        }  
  
        public override string DefaultAsymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.RsaSha1Signature; }  
        }  
  
        public override string DefaultCanonicalizationAlgorithm  
        {  
            get { return SecurityAlgorithms.ExclusiveC14n; ; }  
        }  
  
        public override string DefaultDigestAlgorithm  
        {  
            get { return SecurityAlgorithms.MyCustomHashAlgorithm; }  
        }  
  
        public override string DefaultEncryptionAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override int DefaultEncryptionKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSignatureKeyDerivationLength  
        {  
            get { return 128; }  
        }  
  
        public override int DefaultSymmetricKeyLength  
        {  
            get { return 128; }  
        }  
  
        public override string DefaultSymmetricKeyWrapAlgorithm  
        {  
            get { return SecurityAlgorithms.Aes128Encryption; }  
        }  
  
        public override string DefaultSymmetricSignatureAlgorithm  
        {  
            get { return SecurityAlgorithms.HmacSha1Signature; }  
        }  
  
        public override bool IsAsymmetricKeyLengthSupported(int length)  
        {  
            return length >= 1024 && length <= 4096;  
        }  
  
        public override bool IsSymmetricKeyLengthSupported(int length)  
        {  
            return length >= 128 && length <= 256;  
        }  
    }  
```  
  
## <a name="register-the-custom-algorithm"></a>カスタム アルゴリズムの登録  

 登録は、構成ファイルまたは命令型コードで行うことができます。 カスタム アルゴリズムを登録するには、暗号サービス プロバイダーを実装するクラスとエイリアスの間のマッピングを作成します。 その後、WCF サービスのバインディングでアルゴリズムを指定する際に使用する URI にエイリアスをマップします。 次の構成スニペットは、config でカスタム アルゴリズムを登録する方法を示しています。  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
           <cryptoClasses>  
              <cryptoClass SHA256CSP="System.Security.Cryptography.SHA256CryptoServiceProvider, System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
           </cryptoClasses>  
           <nameEntry name="http://contoso.com/CustomAlgorithms/CustomHashAlgorithm"  
              class="SHA256CSP" />  
           </cryptoNameMapping>  
        </cryptographySettings>  
    </mscorlib>  
</configuration>  
```  
  
 <`cryptoClasses`> 要素の下のセクションで、SHA256CryptoServiceProvider とエイリアス "SHA256CSP" の間のマッピングを作成します。 <`nameEntry`> 要素によって、"SHA256CSP" エイリアスと指定した URL (`http://contoso.com/CustomAlgorithms/CustomHashAlgorithm`) の間のマッピングを作成します。  
  
 コードでカスタム アルゴリズムを登録するには、<xref:System.Security.Cryptography.CryptoConfig.AddAlgorithm(System.Type,System.String[])> メソッドを使用します。 このメソッドによって、両方のマッピングを作成します。 次の例は、このメソッドを呼び出す方法を示しています。  
  
```csharp
// Register the custom URI string defined for the hashAlgorithm in MyCustomAlgorithmSuite class to create the
// SHA256CryptoServiceProvider hash algorithm object.  
CryptoConfig.AddAlgorithm(typeof(SHA256CryptoServiceProvider), "http://contoso.com/CustomAlgorithms/CustomHashAlgorithm");  
```  
  
## <a name="configure-the-binding"></a>バインディングの構成  

 バインディングを構成するには、次のコード スニペットに示すように、<xref:System.ServiceModel.Security.SecurityAlgorithmSuite> のカスタム派生クラスをバインディング設定で指定します。  
  
```csharp  
WSHttpBinding binding = new WSHttpBinding();  
            binding.Security.Message.AlgorithmSuite = new MyCustomAlgorithmSuite();  
```  
  
 コード例全体については、「[WCF セキュリティの暗号化方式の指定](../samples/cryptographic-agility-in-wcf-security.md)」のサンプルを参照してください。  
  
## <a name="see-also"></a>関連項目

- [サービスおよびクライアントのセキュリティ保護](../feature-details/securing-services-and-clients.md)
- [サービスのセキュリティ保護](../securing-services.md)
- [セキュリティの概要](../feature-details/security-overview.md)
- [セキュリティの概念](../feature-details/security-concepts.md)
