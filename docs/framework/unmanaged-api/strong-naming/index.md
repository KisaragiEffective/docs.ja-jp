---
title: 厳密な名前付け (アンマネージ API リファレンス)
ms.date: 03/30/2017
helpviewer_keywords:
- strong naming [.NET Framework], using the unmanaged API
- native API reference [.NET Framework], strong naming
- unmanaged API reference [.NET Framework], strong naming
ms.assetid: 428c68b6-a7b4-44be-b280-75905f46612c
ms.openlocfilehash: 7d18513450111d58b5d26fd834addd465cfc4267
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73140637"
---
# <a name="strong-naming-unmanaged-api-reference"></a>厳密な名前付け (アンマネージ API リファレンス)
厳密な名前付け API を使用すると、アセンブリに対する厳密な名前の署名をクライアントで管理できます。  
  
 厳密な名前を使用してアセンブリに署名すると、アセンブリ マニフェストを格納しているファイルに公開キー暗号化が追加されます。 厳密な名前による署名では、名前の一意性の検証を支援し、名前の悪用を防止し、参照が解決されたときに呼び出し元に一意の ID を提供できます。 しかし、厳密な名前に信頼性のレベルは関連付けられていません。  
  
## <a name="in-this-section"></a>このセクションの内容  
  
> [!NOTE]
> .NET Framework 4 以降、これらの関数すべてが非推奨となりました。 推奨されている代わりの関数については、[ICLRStrongName](../hosting/iclrstrongname-interface.md) インターフェイスを参照してください。  
  
 [GetHashFromAssemblyFile 関数](gethashfromassemblyfile-function.md)  
 指定したハッシュ アルゴリズムを使用して、指定したアセンブリ ファイルのハッシュ値が取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [GetHashFromAssemblyFileW 関数](gethashfromassemblyfilew-function.md)  
 指定したハッシュ アルゴリズムを使用して、Unicode 文字列として指定したアセンブリ ファイルのハッシュ値が取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [GetHashFromBlob 関数](gethashfromblob-function.md)  
 指定したハッシュ アルゴリズムを使用して、指定したメモリ アドレスにあるアセンブリのハッシュが取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [GetHashFromFile 関数](gethashfromfile-function.md)  
 指定したファイルの内容に対してハッシュが生成されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [GetHashFromFileW 関数](gethashfromfilew-function.md)  
 Unicode 文字列で指定されたファイルの内容に対してハッシュが作成されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [GetHashFromHandle 関数](gethashfromhandle-function.md)  
 指定したハッシュ アルゴリズムを使用して、指定したファイル ハンドルを含むファイルの内容に対してハッシュが作成されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameCompareAssemblies 関数](strongnamecompareassemblies-function.md)  
 厳密な名前の署名に基づいて 2 つのアセンブリが異なるかどうかが判定されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameErrorInfo 関数](strongnameerrorinfo-function.md)  
 厳密な名前の関数のいずれかに基づいて最後に発生したエラー コードが取得されます。  
  
 [StrongNameFreeBuffer 関数](strongnamefreebuffer-function.md)  
 [StrongNameGetPublicKey](strongnamegetpublickey-function.md)、[StrongNameTokenFromPublicKey](strongnametokenfrompublickey-function.md)、または[StrongNameSignatureGeneration](strongnamesignaturegeneration-function.md) などの厳密な名前の関数に対する前の呼び出しで割り当てられたメモリが解放されます。   .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameGetBlob 関数](strongnamegetblob-function.md)  
 指定したアドレスにある実行可能ファイルのバイナリ表現が、指定したバッファーに入れられます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameGetBlobFromImage 関数](strongnamegetblobfromimage-function.md)  
 指定したメモリ アドレスにあるアセンブリ イメージのバイナリ表現が取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameGetPublicKey 関数](strongnamegetpublickey-function.md)  
 秘密/公開キーの組から公開キーが取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameHashSize 関数](strongnamehashsize-function.md)  
 指定したハッシュ アルゴリズムを使用して、ハッシュに必須のバッファー サイズが取得されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameKeyDelete 関数](strongnamekeydelete-function.md)  
 指定したキー コンテナーが削除されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameKeyGen 関数](strongnamekeygen-function.md)  
 厳密な名前を使用するために新しい公開/秘密キーの組が作成されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameKeyGenEx 関数](strongnamekeygenex-function.md)  
 厳密な名前を使用するために、指定したキー サイズによって新しい公開/秘密キーの組が作成されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameKeyInstall 関数](strongnamekeyinstall-function.md)  
 公開/秘密キーの組がコンテナーにインポートされます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureGeneration 関数](strongnamesignaturegeneration-function.md)  
 指定したアセンブリに対して厳密な名前の署名が生成されます。   .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureGenerationEx 関数](strongnamesignaturegenerationex-function.md)  
 指定したフラグに基づいて、指定したアセンブリに対する厳密な名前の署名が作成されます。    .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureSize 関数](strongnamesignaturesize-function.md)  
 厳密な名前の署名のサイズが返されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureVerification 関数](strongnamesignatureverification-function.md)  
 指定したパスにあるアセンブリ マニフェストに厳密な名前の署名が含まれるかどうかを示す値が取得されます。これは指定したフラグに従って確認されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureVerificationEx 関数](strongnamesignatureverificationex-function.md)  
 指定したパスにあるアセンブリ マニフェストに厳密な名前の署名が含まれるかどうかを示す値が取得されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameSignatureVerificationFromImage 関数](strongnamesignatureverificationfromimage-function.md)  
 メモリに既にマップされているアセンブリが、関連付けられている公開キーに対して有効であるかどうかが確認されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameTokenFromAssembly 関数](strongnametokenfromassembly-function.md)  
 指定したアセンブリ ファイルから、厳密な名前トークンが作成されます。  .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameTokenFromAssemblyEx 関数](strongnametokenfromassemblyex-function.md)  
 指定したアセンブリ ファイルから厳密な名前のトークンが作成され、公開キーが返されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [StrongNameTokenFromPublicKey 関数](strongnametokenfrompublickey-function.md)  
 公開キーを表すトークンが取得されます。 .NET Framework 4 以降では非推奨とされます。  
  
 [PublicKeyBlob 構造体](publickeyblob-structure.md)  
 公開/秘密キーの組の公開キーがバイナリ形式で表されます。  
  
## <a name="see-also"></a>参照

- [ICLRStrongName インターフェイス](../hosting/iclrstrongname-interface.md)
- [アンマネージ API リファレンス](../index.md)
