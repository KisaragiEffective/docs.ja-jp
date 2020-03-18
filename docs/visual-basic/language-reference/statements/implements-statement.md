---
title: Implements ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Implements
- Implements
helpviewer_keywords:
- Implements statement [Visual Basic], syntax
- Implements statement [Visual Basic]
- interface implementation [Visual Basic], Implements statement
ms.assetid: 1fafb83f-f55a-4215-8ea9-681e8622613d
ms.openlocfilehash: e2e279b2c935dd082cbf832265a8ad09e6dffe9e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351146"
---
# <a name="implements-statement"></a>Implements ステートメント
表示されるクラスまたは構造体の定義に実装する必要がある1つ以上のインターフェイス、またはインターフェイスメンバーを指定します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Implements interfacename [, ...]  
' -or-  
Implements interfacename.interfacemember [, ...]  
```  
  
## <a name="parts"></a>指定項目  
 `interfacename`  
 必須。 クラスまたは構造体の対応するメンバーによって実装されるプロパティ、プロシージャ、およびイベントを持つインターフェイス。  
  
 `interfacemember`  
 必須。 実装されているインターフェイスのメンバー。  
  
## <a name="remarks"></a>コメント  
 インターフェイスは、インターフェイスによってカプセル化されるメンバー (プロパティ、プロシージャ、およびイベント) を表すプロトタイプのコレクションです。 インターフェイスには、メンバーの宣言のみが含まれます。クラスと構造体は、これらのメンバーを実装します。 詳細については、「 [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)で定義されているインターフェイスのプライベート C++ 固有の実装です。  
  
 `Implements` ステートメントは、`Class` または `Structure` ステートメントの直後に記述する必要があります。  
  
 インターフェイスを実装する場合は、インターフェイスで宣言されたすべてのメンバーを実装する必要があります。 メンバーを省略すると、構文エラーと見なされます。 個々のメンバーを実装するには、クラスまたは構造体でメンバーを宣言するときに、 [Implements](../../../visual-basic/language-reference/statements/implements-clause.md)キーワード (`Implements` ステートメントとは別のもの) を指定します。 詳細については、「 [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)で定義されているインターフェイスのプライベート C++ 固有の実装です。  
  
 クラスでは、プロパティとプロシージャの [Private](../../../visual-basic/language-reference/modifiers/private.md) 実装を使用できますが、これらのメンバーには、インターフェイスの型として宣言された変数に、実装するクラスのインスタンスをキャストすることによってのみアクセスできます。  
  
## <a name="example"></a>例  
 次の例は、`Implements` ステートメントを使用してインターフェイスのメンバーを実装する方法を示しています。 これは、イベント、プロパティ、およびプロシージャを使用して `ICustomerInfo` という名前のインターフェイスを定義します。 クラス `customerInfo`、インターフェイスで定義されているすべてのメンバーを実装します。  
  
 [!code-vb[VbVbalrStatements#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#33)]  
  
 クラス `customerInfo` は、クラスが `ICustomerInfo` インターフェイスのすべてのメンバーを実装していることを示すために、別のソースコード行で `Implements` ステートメントを使用することに注意してください。 次に、クラスの各メンバーは、メンバー宣言の一部として `Implements` キーワードを使用して、そのインターフェイスメンバーを実装していることを示します。  
  
## <a name="example"></a>例  
 次の2つの手順は、前の例で実装されたインターフェイスを使用する方法を示しています。 実装をテストするには、これらのプロシージャをプロジェクトに追加し、`testImplements` プロシージャを呼び出します。  
  
 [!code-vb[VbVbalrStatements#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#34)]  
  
## <a name="see-also"></a>参照

- [Implements](../../../visual-basic/language-reference/statements/implements-clause.md)
- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)
- [インターフェイス](../../../visual-basic/programming-guide/language-features/interfaces/index.md)
