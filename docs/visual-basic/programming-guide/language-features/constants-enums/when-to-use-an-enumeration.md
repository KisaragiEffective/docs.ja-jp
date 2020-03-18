---
title: 列挙型を使用する状況
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
ms.assetid: e6e47b5b-3ed9-452d-a481-9c3fed88519a
ms.openlocfilehash: 5daae8d487ddfe079a54e305e59e32e8ded8f65e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353955"
---
# <a name="when-to-use-an-enumeration-visual-basic"></a>列挙型を使用する状況 (Visual Basic)
列挙体を使用すると、関連する定数のセットを簡単に操作できます。 列挙型 (`Enum`) は、値のセットのシンボル名です。 列挙型はデータ型として扱われ、変数およびプロパティで使用する定数のセットを作成するために使用できます。  
  
## <a name="when-to-use-an-enumeration"></a>列挙型を使用する状況  
 プロシージャが限定された一連の変数を受け入れる場合は、列挙型の使用を検討してください。 列挙型により、特に意味のある名前が使用される場合に、明確で読みやすいコードが作成されます。  
  
 列挙を使用する利点は次のとおりです。  
  
- 数値の転置またはミスによって発生するエラーを減らします。  
  
- では、将来の値の変更が簡単になります。  
  
- コードを読みやすくします。つまり、エラーが発生する可能性は低くなります。  
  
- 上位互換性を確保します。 列挙体を使用すると、後でメンバー名に対応する値を変更すると、コードが失敗する可能性は低くなります。  
  
## <a name="naming-enumerations"></a>列挙型の名前付け  
 列挙メンバーには名前付け規則を使用します。 Visual Basic が列挙メンバー名を検出した場合、他の参照されるタイプライブラリに同じ名前が含まれていると、例外がスローされる可能性があります。 アプリケーションまたはコンポーネントの値を識別する一意のプレフィックスを使用します。  
  
 列挙体のメンバーを参照する場合は、メンバー名を列挙名で修飾するか、または `Imports` ステートメントを使用する必要があります。 詳細については、「[列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)」を参照してください。  
  
## <a name="predefined-enumerations"></a>定義済みの列挙型  
 Visual Basic には、コードを容易にするために、`FirstDayOfWeek` や `MsgBoxResult`など、いくつかの定義済みの列挙型が用意されています。 これらの一覧については、「[定数と列挙型](../../../../visual-basic/language-reference/constants-and-enumerations.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [方法: 列挙型を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [方法 : 列挙型のメンバーを参照する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [方法: Visual Basic 内の列挙体を反復処理する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-iterate-through-an-enumeration.md)
- [方法 : 列挙値に関連付けられている文字列を確認する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Enum ステートメント](../../../../visual-basic/language-reference/statements/enum-statement.md)
- [定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)
