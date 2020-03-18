---
title: ユーザー定義定数
ms.date: 07/20/2015
helpviewer_keywords:
- constants [Visual Basic], circular references
- Const statement [Visual Basic], user-defined constants
- user-defined constants [Visual Basic]
- scope [Visual Basic], constants
- constants [Visual Basic], user-defined
- circular references between constants [Visual Basic]
ms.assetid: a1206d5c-c45e-4ac2-970a-4a0be6a05fdd
ms.openlocfilehash: 194a420b3749ca5c858a65c07b8c164287c1582a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354007"
---
# <a name="user-defined-constants-visual-basic"></a>ユーザー定義定数 (Visual Basic)
定数は、変更されない数値または文字列の代わりとなるわかりやすい名前です。 定数に格納された値は、その名が示すとおり、アプリケーションの実行中に変わることはありません。 操作するコントロールまたはコンポーネントによって定義された定数を使用することも、独自の定数を作成することもできます。 自分で作成した定数は、*ユーザー定義*として記述されます。  
  
 変数名を作成する場合と同じガイドラインを使用して、`Const` ステートメントを使用して定数を宣言します。 `Option Strict` が `On`場合は、定数型を明示的に宣言する必要があります。  
  
## <a name="const-statement-usage"></a>Const ステートメントの使用法  
 `Const` ステートメントは、数値または日付/時刻の数量を表すことができます。  
  
 [!code-vb[VbEnumsTask#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#10)]  
  
 また、`String` 定数を定義することもできます。  
  
 [!code-vb[VbEnumsTask#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#13)]  
  
 等号 (`=`) の右側にある式は、多くの場合、数値またはリテラル文字列ですが、数値または文字列を生成する式にすることもできます (ただし、その式に関数の呼び出しを含めることはできません)。 以前に定義された定数の観点から定数を定義することもできます。  
  
 [!code-vb[VbEnumsTask#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#15)]  
  
## <a name="scope-of-user-defined-constants"></a>ユーザー定義定数のスコープ  
 `Const` ステートメントのスコープは、同じ場所で宣言された変数と同じです。 スコープは、次のいずれかの方法で指定できます。  
  
- プロシージャ内にのみ存在する定数を作成するには、そのプロシージャ内で宣言します。  
  
- クラス内のすべてのプロシージャで使用できる定数を作成し、そのモジュールの外部のコードには使用できないようにするには、クラスの宣言セクションで宣言します。  
  
- アセンブリのすべてのメンバーが使用できる定数を作成するには、アセンブリの外部のクライアントではなく、クラスの宣言セクションで `Friend` キーワードを使用して宣言します。  
  
- アプリケーション全体で使用可能な定数を作成するには、クラスの宣言セクションで `Public` キーワードを使用して宣言します。  
  
 詳細については、「[方法: 定数を宣言](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-a-constant.md)する」を参照してください。  
  
### <a name="avoiding-circular-references"></a>循環参照の回避  
 定数は他の定数として定義できるので、誤って2つ以上の定数間に循環*参照を作成*することもできます。 循環は、次の例のように、2つ以上のパブリック定数がある場合に発生します。各定数は、もう一方の項で定義されています。  
  
 [!code-vb[VbEnumsTask#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#16)]  
[!code-vb[VbEnumsTask#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#17)]  
  
 循環が発生した場合、Visual Basic によってコンパイラエラーが生成されます。  
  
## <a name="see-also"></a>関連項目

- [Const ステートメント](../../../../visual-basic/language-reference/statements/const-statement.md)
- [定数とリテラルのデータ型](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)
- [定数と列挙体](../../../../visual-basic/programming-guide/language-features/constants-enums/index.md)
- [定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)
- [列挙型の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
- [定数の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/constants-overview.md)
- [方法: 列挙型を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
