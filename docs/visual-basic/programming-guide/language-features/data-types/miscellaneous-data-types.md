---
title: その他のデータ型
ms.date: 07/20/2015
helpviewer_keywords:
- Object data type [Visual Basic], data types
- data types [Visual Basic], choosing
ms.assetid: 64c71a12-9057-4dbf-baca-7379c4aada69
ms.openlocfilehash: cc6262b5bb305bb839917e222d831fa3340a1b14
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346342"
---
# <a name="miscellaneous-data-types-visual-basic"></a>その他のデータ型 (Visual Basic)
Visual Basic には、数値や文字に向いていない複数のデータ型が用意されています。 代わりに、yes/no 値、日付/時刻値、オブジェクトアドレスなどの特殊なデータを処理します。  
  
 Visual Basic のデータ型の並列比較を示す表については、「[データ型の概要](../../../../visual-basic/language-reference/data-types/index.md)」を参照してください。  
  
## <a name="boolean-type"></a>ブール型  
 [ブールデータ型](../../../../visual-basic/language-reference/data-types/boolean-data-type.md)は、`True` または `False`として解釈される符号なしの値です。 データ幅は、実装するプラットフォームによって異なります。 変数に、true/false、yes/no、on/off などの2つの状態の値のみを含めることができる場合は、それを `Boolean`として宣言します。  
  
## <a name="date-type"></a>日付型  
 [日付データ型](../../../../visual-basic/language-reference/data-types/date-data-type.md)は、日付と時刻の両方の情報を保持する64ビット値です。 各インクリメントは、グレゴリオ暦の1年1月1日の開始 (12:00 AM) からの経過時間の100ナノ秒を表します。 変数に日付値、時刻値、またはその両方を含めることができる場合は、`Date`として宣言します。  
  
## <a name="object-type"></a>オブジェクトの型  
 [Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)は、アプリケーション内または他のアプリケーション内のオブジェクトインスタンスを指す32ビットアドレスです。 `Object` 変数は、アプリケーションが認識する任意のオブジェクト、または任意のデータ型のデータを参照できます。 これには、`Integer`、`Boolean`、構造体のインスタンスなどの*値型*と、`String` や <xref:System.Windows.Forms.Form>、配列インスタンスなどのクラスから作成されたオブジェクトのインスタンスである*参照型*の両方が含まれます。  
  
 コンパイル時にわからないクラスのインスタンスへのポインターが変数に格納されている場合、または、さまざまなデータ型のデータを参照できる場合は、それを `Object`として宣言します。  
  
 `Object` データ型の利点は、任意のデータ型のデータを格納するために使用できることです。 欠点は、実行時間のかかる追加操作が発生し、アプリケーションの実行速度が低下することです。 値型に `Object` 変数を使用すると、*ボックス*化と*ボックス化解除*が発生します。 参照型に使用すると、*遅延バインディング*が発生します。  
  
## <a name="see-also"></a>参照

- [型文字](../../../../visual-basic/programming-guide/language-features/data-types/type-characters.md)
- [基本データ型](../../../../visual-basic/programming-guide/language-features/data-types/elementary-data-types.md)
- [数値のデータ型](../../../../visual-basic/programming-guide/language-features/data-types/numeric-data-types.md)
- [文字データ型](../../../../visual-basic/programming-guide/language-features/data-types/character-data-types.md)
- [トラブルシューティング (データ型)](../../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [事前バインディングと遅延バインディング](../../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
