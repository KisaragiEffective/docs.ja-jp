---
title: '#ExternalSource ディレクティブ'
ms.date: 07/20/2015
f1_keywords:
- '#Externalsource'
- '#ExternalSource'
- vb.ExternalSource
- Externalsource
- vb.#ExternalSource
- ExternalSource
helpviewer_keywords:
- ExternalSource directive (#ExternalSource)
- '#ExternalSource directive'
ms.assetid: 243bc6a2-34c3-4eeb-a776-9fd2bf988149
ms.openlocfilehash: fa0a40827c1b3865b90c7d796ea4dd364774e1c4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343830"
---
# <a name="externalsource-directive"></a>#ExternalSource ディレクティブ

ソースコードの特定の行と、ソースの外部のテキストとの間のマッピングを示します。  
  
## <a name="syntax"></a>構文  
  
```vb  
#ExternalSource( StringLiteral , IntLiteral )  
    [ LogicalLine+ ]  
#End ExternalSource  
```  
  
## <a name="parts"></a>指定項目  

 `StringLiteral`  
 外部ソースへのパス。  
  
 `IntLiteral`  
 外部ソースの最初の行の行番号。  
  
 `LogicalLine`  
 外部ソースでエラーが発生した行。  
  
 `#End ExternalSource`  
 `#ExternalSource` ブロックを終了します。  
  
## <a name="remarks"></a>コメント  

 このディレクティブは、コンパイラとデバッガーでのみ使用されます。  
  
 ソースファイルには、ソースファイル内の特定のコード行と、.aspx ファイルなどのソースの外部テキストとの間のマッピングを示す、外部ソースディレクティブを含めることができます。 コンパイル時に指定したソースコードでエラーが発生した場合は、外部ソースからのものとして識別されます。  
  
 外部ソースディレクティブはコンパイルには影響しません。入れ子にすることはできません。 アプリケーションでの内部使用のみを目的としています。  
  
## <a name="see-also"></a>参照

- [条件付きコンパイル](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)
