---
title: 拡張された厳密な名前付け
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies
- strong naming [.NET Framework], enhanced
ms.assetid: 6cf17a82-62a1-4f6d-8d5a-d7d06dec2bb5
ms.openlocfilehash: 1d582513b10de88e4e5b9b9ef8c338599d6980f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141174"
---
# <a name="enhanced-strong-naming"></a>拡張された厳密な名前付け
厳密な名前の署名は、アセンブリを識別するための .NET Framework の識別機構です。 これは通常、発行元 (署名者) から受取人 (検証者) に対して渡されるデータの整合性を検証するために使用される、公開キーによるデジタル署名です。 この署名はアセンブリに対する一意の ID として使用され、これによりアセンブリへの参照のあいまいさをなくします。 アセンブリはビルド プロセスの一部として署名され、読み込まれるときに検証されます。  
  
 厳密な名前の署名により、悪意のあるユーザーがアセンブリを不正に変更して、元の署名者のキーでアセンブリを再署名することを防ぐことができます。 ただし、厳密な名前キーには、発行者に関する信頼できる情報も、証明書の階層構造も含まれていません。 厳密な名前の署名は、アセンブリの署名者の信頼性を保証するものではなく、またその署名者がキーの正当な所有者であることを示すものでもありません。厳密な名前の署名は、キーの所有者がアセンブリに署名したことのみを示しています。 したがって、信頼できるサードパーティのコードのためのセキュリティの検証コントロールとして、厳密な名前の署名を使用することはお勧めしません。 コードを認証するために推奨される方法は、Microsoft Authenticode です。  
  
## <a name="limitations-of-conventional-strong-names"></a>従来の厳密な名前の制限  
 .NET Framework 4.5 以前のバージョンで使用されている厳密な名前付けのテクノロジには次の欠点があります。  
  
- キーが常に攻撃を受ける状態に置かれています。テクノロジとハードウェアの向上により、公開キーから秘密キーを推測することが容易になっています。 攻撃からの保護には、より大きなキーが必要です。 .NET Framework 4.5 より前の .NET Framework バージョンでは任意のサイズのキー (既定のサイズは 1024 ビット) で署名する機能を提供していますが、新しいキーでアセンブリを署名すると、アセンブリの古い ID を参照しているすべてのバイナリが動作しなくなります。 したがって、互換性を維持する場合には、署名キーのサイズの更新は非常に困難です。  
  
- 厳密な名前の署名は SHA-1 アルゴリズムのみをサポートします。 SHA-1 は、最近、セキュリティが強化されたハッシュ アプリケーションのためには不十分であることがわかりました。 したがって、より強力なアルゴリズム (SHA-256 以上) が必要です。 SHA-1 は FIPS 準拠の地位を失う可能性があり、これによって FIPS 準拠のソフトウェアとアルゴリズムのみを選択して使用しているユーザーには問題が生じることになります。  
  
## <a name="advantages-of-enhanced-strong-names"></a>強化された厳密な名前の利点  
 強化された厳密な名前の主な利点は、既存の厳密な名前との互換性と、ある ID が別の ID と等価であることを指定できる機能です。  
  
- 既存の署名付きアセンブリを持つ開発者は、以前の ID を参照するアセンブリとの互換性を保ちながら、それらのアセンブリの ID を SHA-2 アルゴリズムに移行できます。  
  
- 新しいアセンブリを作成する際に、既存の厳密な名前の署名を考慮する必要のない開発者は、より安全な SHA-2 アルゴリズムを使用して、今までどおりにアセンブリに署名できます。  
  
## <a name="use-enhanced-strong-names"></a>強化された厳密な名前を使用する  
 厳密な名前のキーは署名キーと ID のキーで構成されます。 アセンブリは署名キーで署名され、ID キーで識別されます。 .NET Framework 4.5 より前では、これらの 2 つのキーは同一です。 .NET Framework 4.5 以降では、ID キーは、.NET Framework の以前のバージョンと同じですが、署名キーは、より強力なハッシュ アルゴリズムを備えて強化されています。 さらに、副署名を作成するために、署名キーは ID キーで署名されます。  
  
 <xref:System.Reflection.AssemblySignatureKeyAttribute> 属性によって、アセンブリのメタデータはアセンブリの ID に既存の公開キーを使用することができ、これによって、古いアセンブリ参照は引き続き動作します。  <xref:System.Reflection.AssemblySignatureKeyAttribute> の属性は副署名を使用して、新しい署名キーの所有者が古い ID キーの所有者にもなるようにします。  
  
### <a name="sign-with-sha-2-without-key-migration"></a>キー移行を伴わない SHA-2 による署名  
 厳密な名前の署名を移行せずにアセンブリに署名するには、コマンド プロンプトから次のコマンドを実行します。  
  
1. 新しい ID キーを生成します (必要な場合)。  
  
    ```console  
    sn -k IdentityKey.snk  
    ```  
  
2. ID の公開キーを抽出し、このキーで署名する場合に SHA-2 アルゴリズムを使用するように指定します。  
  
    ```console  
    sn -p IdentityKey.snk IdentityPubKey.snk sha256  
    ```  
  
3. ID 公開キー ファイルで、アセンブリに遅延署名します。  
  
    ```console  
    csc MyAssembly.cs /keyfile:IdentityPubKey.snk /delaySign+  
    ```  
  
4. 完全な ID キーのペアでアセンブリに再署名します。  
  
    ```console  
    sn -Ra MyAssembly.exe IdentityKey.snk  
    ```  
  
### <a name="sign-with-sha-2-with-key-migration"></a>キー移行を伴う SHA-2 による署名  
 移行した厳密な名前の署名でアセンブリに署名するには、コマンド プロンプトで次のコマンドを実行します。  
  
1. ID キーと署名キーのペアを生成します (必要な場合)。  
  
    ```console  
    sn -k IdentityKey.snk  
    sn -k SignatureKey.snk  
    ```  
  
2. 署名の公開キーを抽出し、このキーで署名する場合に SHA-2 アルゴリズムを使用するように指定します。  
  
    ```console  
    sn -p SignatureKey.snk SignaturePubKey.snk sha256  
    ```  
  
3. 副署名を生成するハッシュ アルゴリズムを決定する ID の公開キーを抽出します。  
  
    ```console  
    sn -p IdentityKey.snk IdentityPubKey.snk  
    ```  
  
4. <xref:System.Reflection.AssemblySignatureKeyAttribute> の属性のパラメーターを生成し、アセンブリに属性を追加します。  
  
    ```console  
    sn -a IdentityPubKey.snk IdentityKey.snk SignaturePubKey.snk  
    ```  

    これにより、次のような出力が生成されます。

    ```output
    Information for key migration attribute.
    (System.Reflection.AssemblySignatureKeyAttribute):
    publicKey=
    002400000c80000094000000060200000024000052534131000400000100010005a3a81ac0a519
    d96244a9c589fc147c7d403e40ccf184fc290bdd06c7339389a76b738e255a2bce1d56c3e7e936
    e4fc87d45adc82ca94c716b50a65d39d373eea033919a613e4341c66863cb2dc622bcb541762b4
    3893434d219d1c43f07e9c83fada2aed400b9f6e44ff05e3ecde6c2827830b8f43f7ac8e3270a3
    4d153cdd

    counterSignature=
    e3cf7c211678c4d1a7b8fb20276c894ab74c29f0b5a34de4d61e63d4a997222f78cdcbfe4c91eb
    e1ddf9f3505a32edcb2a76f34df0450c4f61e376b70fa3cdeb7374b1b8e2078b121e2ee6e8c6a8
    ed661cc35621b4af53ac29c9e41738f199a81240e8fd478c887d1a30729d34e954a97cddce66e3
    ae5fec2c682e57b7442738
    ```

    この出力は AssemblySignatureKeyAttribute に変換できます。

    ```
    [assembly:System.Reflection.AssemblySignatureKeyAttribute(
    "002400000c80000094000000060200000024000052534131000400000100010005a3a81ac0a519d96244a9c589fc147c7d403e40ccf184fc290bdd06c7339389a76b738e255a2bce1d56c3e7e936e4fc87d45adc82ca94c716b50a65d39d373eea033919a613e4341c66863cb2dc622bcb541762b43893434d219d1c43f07e9c83fada2aed400b9f6e44ff05e3ecde6c2827830b8f43f7ac8e3270a34d153cdd",
    "e3cf7c211678c4d1a7b8fb20276c894ab74c29f0b5a34de4d61e63d4a997222f78cdcbfe4c91ebe1ddf9f3505a32edcb2a76f34df0450c4f61e376b70fa3cdeb7374b1b8e2078b121e2ee6e8c6a8ed661cc35621b4af53ac29c9e41738f199a81240e8fd478c887d1a30729d34e954a97cddce66e3ae5fec2c682e57b7442738"
    )]
    ```
  
5. ID 公開キーでアセンブリに遅延署名します。  
  
    ```console  
    csc MyAssembly.cs /keyfile:IdentityPubKey.snk /delaySign+  
    ```  
  
6. 完全な署名キーのペアでアセンブリに署名します。  
  
    ```console  
    sn -Ra MyAssembly.exe SignatureKey.snk  
    ```  
  
## <a name="see-also"></a>参照

- [厳密な名前付きアセンブリの作成と使用](create-use-strong-named.md)
