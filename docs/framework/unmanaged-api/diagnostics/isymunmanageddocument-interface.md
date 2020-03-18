---
title: ISymUnmanagedDocument インターフェイス
ms.date: 03/30/2017
api_name:
- ISymUnmanagedDocument
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedDocument
helpviewer_keywords:
- ISymUnmanagedDocument interface [.NET Framework debugging]
ms.assetid: 5c26b366-6e81-467c-9dd0-02dd26fee0a3
topic_type:
- apiref
ms.openlocfilehash: 3fa7b6b19d81e374cdb09b07ec181a7f4249a5eb
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74449097"
---
# <a name="isymunmanageddocument-interface"></a>ISymUnmanagedDocument インターフェイス
シンボル ストアによって参照されるドキュメントを表します。 ドキュメントは、URL (uniform resource locator) とドキュメントの種類の GUID によって定義されます。 URL とドキュメントの種類の GUID を使用して、ドキュメントがどのように格納されているかに関係なく、ドキュメントを見つけることができます。 ドキュメントソースをシンボルストアに格納し、このインターフェイスを使用して取得することができます。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[FindClosestLine メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-findclosestline-method.md)|このドキュメント内の行が指定されている場合は、シーケンスポイントで最も近い行を返します。|  
|[GetCheckSum メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getchecksum-method.md)|チェックサムを取得します。|  
|[GetCheckSumAlgorithmId メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getchecksumalgorithmid-method.md)|チェックサムアルゴリズム識別子を取得します。チェックサムがない場合は、すべての0の GUID を返します。|  
|[GetDocumentType メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getdocumenttype-method.md)|このドキュメントのドキュメントの種類を取得します。|  
|[GetLanguage メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getlanguage-method.md)|このドキュメントの言語識別子を取得します。|  
|[GetLanguageVendor メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getlanguagevendor-method.md)|このドキュメントの言語販売元を取得します。|  
|[GetSourceLength メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getsourcelength-method.md)|埋め込まれたソースの長さをバイト数で取得します。|  
|[GetSourceRange メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-getsourcerange-method.md)|指定されたバッファーに、埋め込みソースの指定された範囲を返します。|  
|[GetURL メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-geturl-method.md)|このドキュメントの URL を返します。|  
|[HasEmbeddedSource メソッド](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanageddocument-hasembeddedsource-method.md)|ドキュメントにデバッグシンボルに埋め込まれているソースがある場合に `true` を返します。それ以外の場合は `false`を返します。|  
  
## <a name="see-also"></a>参照

- [シンボル ストア診断インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/diagnostics-symbol-store-interfaces.md)
