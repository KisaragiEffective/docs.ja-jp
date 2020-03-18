---
title: インターフェイス
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, interfaces
- interfaces [Visual Basic], Visual Basic
- interfaces
- interfaces [Visual Basic]
ms.assetid: 61b06674-12c9-430b-be68-cc67ecee1f5b
ms.openlocfilehash: 619aa6695db756e56a836fd76693cc8a3976f8e2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345055"
---
# <a name="interfaces-visual-basic"></a>インターフェイス (Visual Basic)
*インターフェイス*は、クラスが実装できるプロパティ、メソッド、およびイベントを定義します。 インターフェイスでは、密接に関連するプロパティ、メソッド、およびイベントの小さなグループとして機能を定義できます。これにより、既存のコードを損なうことなく、インターフェイスを拡張して実装を開発できるため、互換性の問題を減らすことができます。 追加のインターフェイスと実装を開発することで、いつでも新しい機能を追加できます。  
  
 クラスの継承の代わりにインターフェイスを使用する方が望ましい理由が、その他にもいくつかあります。  
  
- インターフェイスは、アプリケーションが特定の機能を提供するために関連性の低い多数のオブジェクトの種類を必要とする状況に、より適しています。  
  
- インターフェイスは、複数のインターフェイスを実装できる単一の実装を定義できるため、基底クラスよりも柔軟です。  
  
- インターフェイスは、基底クラスから実装を継承する必要がない状況に、より適しています。  
  
- インターフェイスは、クラスの継承を使用できない場合に便利です。 たとえば、構造体はクラスから継承できませんが、インターフェイスを実装できます。  
  
## <a name="declaring-interfaces"></a>インターフェイスの宣言  
 インターフェイスの定義は、`Interface` ステートメントと `End Interface` ステートメントで囲みます。 `Interface` ステートメントの後に、オプションで`Inherits` ステートメントを追加して、継承されるインターフェイスを 1 つ以上指定することができます。 `Inherits` ステートメントは、宣言内のコメントを除く他のすべてのステートメントより前に記述する必要があります。 インターフェイス定義の残りのステートメントは、`Event`、`Sub`、`Function`、`Property`、`Interface`、`Class`、`Structure`、および `Enum` ステートメントです。 インターフェイスには、`End Sub` や `End Property` など、実装コードや実装コードに関連付けられているステートメントを含めることはできません。  
  
 名前空間内で、インターフェイス ステートメントは既定では `Friend` ですが、明示的に `Public` または `Friend` として宣言することもできます。 クラス、モジュール、インターフェイス、および構造体内で定義されたインターフェイスは、既定では `Public` ですが、明示的に `Public`、`Friend`、`Protected`、または `Private` として宣言することもできます。  
  
> [!NOTE]
> `Shadows` キーワードは、すべてのインターフェイス メンバーに適用できます。 `Overloads` キーワードは、インターフェイス定義で宣言された `Sub`、`Function`、および `Property` ステートメントに適用できます。 さらに、`Property` ステートメントには `Default`、`ReadOnly`、または `WriteOnly` 修飾子を付けることができます。 他の修飾子 (`Public`、`Private`、`Friend`、`Protected`、`Shared`、`Overrides`、`MustOverride`、または `Overridable`) は許可されていません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。  
  
 たとえば、次のコードは、1 つの関数、1 つのプロパティ、および 1 つのイベントを持つインターフェイスを定義します。  
  
 [!code-vb[VbVbalrOOP#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#17)]  
  
## <a name="implementing-interfaces"></a>インターフェイスの実装  
 Visual Basic の予約語 `Implements` は、次の2つの方法で使用されます。 `Implements` ステートメントは、クラスまたは構造体がインターフェイスを実装することを示します。 `Implements` キーワードは、クラス メンバーまたは構造体メンバーが特定のインターフェイス メンバーを実装することを示します。  
  
### <a name="implements-statement"></a>Implements ステートメント  
 クラスまたは構造体が 1 つ以上のインターフェイスを実装する場合は、`Implements` ステートメントを `Class` または `Structure` ステートメントの直後に記述する必要があります。 `Implements` ステートメントには、クラスによって実装されるインターフェイスのコンマ区切りのリストが必要です。 クラスまたは構造体は、すべてのインターフェイス メンバーを `Implements` キーワードを使用して実装する必要があります。  
  
### <a name="implements-keyword"></a>Implements キーワード  
 `Implements` キーワードには、実装されるインターフェイス メンバーのコンマ区切りのリストが必要です。 一般的には、1 つのインターフェイス メンバーのみが指定されますが、複数のメンバーを指定することもできます。 インターフェイス メンバーの指定は、クラス内の implements ステートメントで指定する必要があるインターフェイス名と、ピリオドと、実装されるメンバー関数、プロパティ、またはイベントの名前で構成されます。 インターフェイスメンバーを実装するメンバーの名前には、任意の有効な識別子を使用できます。また、以前のバージョンの Visual Basic で使用されている `InterfaceName_MethodName` 規則に限定されません。  
  
 たとえば、次のコードは、インターフェイスのメソッドを実装する `Sub1` という名前のサブルーチンを宣言する方法を示しています。  
  
 [!code-vb[VbVbalrOOP#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#69)]  
  
 実装するメンバーのパラメーターの型と戻り値の型は、インターフェイスのインターフェイス プロパティまたはメンバー宣言と一致する必要があります。 インターフェイスの要素を実装する最も一般的な方法は、前の例で示されているように、インターフェイスと同じ名前を持つメンバーを使用する方法です。  
  
 インターフェイス メソッドの実装を宣言するには、インスタンス メソッドの宣言で有効な任意の属性を使用できます。たとえば、`Overloads`、`Overrides`、`Overridable`、`Public`、`Private`、`Protected`、`Friend`、`Protected Friend`、`MustOverride`、`Default`、`Static` などです。 `Shared` 属性は、インスタンス メソッドではなくクラスを定義するため、無効です。  
  
 `Implements` を使用すると、次の例のように、インターフェイスで定義されている複数のメソッドを実装する 1 つのメソッドを記述することもできます。  
  
 [!code-vb[VbVbalrOOP#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#70)]  
  
 インターフェイス メンバーを実装するには、プライベート メンバーを使用することができます。 プライベート メンバーでインターフェイスのメンバーを実装すると、そのメンバーは、クラスのオブジェクト変数で直接利用できない場合でも、インターフェイスを通じて利用できるようになります。  
  
### <a name="interface-implementation-examples"></a>インターフェイスの実装の例  
 インターフェイスを実装するクラスは、そのすべてのプロパティ、メソッド、およびイベントを実装する必要があります。  
  
 次の例では、2 つのインターフェイスが定義されます。 2 番目のインターフェイス `Interface2` は `Interface1` を継承し、追加のプロパティとメソッドを定義します。  
  
 [!code-vb[VbVbalrOOP#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#39)]  
  
 次の例は、前の例で定義されたインターフェイスである `Interface1` を実装します。  
  
 [!code-vb[VbVbalrOOP#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#40)]  
  
 最後の例は、`Interface2` から継承されたメソッドを含めて、`Interface1` を実装します。  
  
 [!code-vb[VbVbalrOOP#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#41)]  
  
 readwrite プロパティを使用して、readonly プロパティを実装できます (つまり、実装するクラスで readonly を宣言する必要はありません)。  インターフェイスを実装する場合、少なくともインターフェイスが宣言しているメンバーを実装することになりますが、プロパティを書き込み可能にするなど、追加の機能を提供することもできます。  
  
## <a name="related-topics"></a>関連トピック  
  
|[タイトル]|説明|  
|-----------|-----------------|  
|[チュートリアル: インターフェイスの作成と実装](../../../../visual-basic/programming-guide/language-features/interfaces/walkthrough-creating-and-implementing-interfaces.md)|独自のインターフェイスを定義および実装する処理の詳細な手順を説明します。|  
|[ジェネリック インターフェイスの分散](../../concepts/covariance-contravariance/variance-in-generic-interfaces.md)|ジェネリック インターフェイスでの共変性と反変性について説明し、.NET Framework でのバリアント ジェネリック インターフェイスの一覧を示します。|
