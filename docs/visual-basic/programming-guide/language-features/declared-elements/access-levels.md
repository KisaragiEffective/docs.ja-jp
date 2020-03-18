---
title: アクセス レベル
ms.date: 05/10/2018
helpviewer_keywords:
- members [Visual Basic], accessing in Visual Basic
- Friend access modifier
- access levels, declared elements
- access levels
- access modifiers
- Public access modifier
- Protected access modifier
- Protected Friend access modifier
- Private Protected access modifier
- Private access modifier
- declared elements [Visual Basic], access level
ms.assetid: 6e06c1ab-fd78-47f0-83a8-1152780b5e1a
ms.openlocfilehash: 33a218a2acc3c876428d6c9a887280a559f84323
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348679"
---
# <a name="access-levels-in-visual-basic"></a>Visual Basic でのアクセス レベル

宣言された要素の*アクセスレベル*は、その要素にアクセスするためのアクセス許可の範囲です。つまり、読み取りまたは書き込みのアクセス許可を持っているコードにアクセスできます。 これは、要素自体を宣言する方法だけでなく、要素のコンテナーのアクセスレベルによっても決定されます。 コンテナー要素にアクセスできないコードは、`Public`として宣言されていても、含まれている要素のいずれにもアクセスできません。 たとえば、`Private` 構造体の `Public` 変数は、構造体を含むクラス内からアクセスできますが、そのクラスの外部からはアクセスできません。

## <a name="public"></a>Public

宣言ステートメント内の [Public](../../../language-reference/modifiers/public.md) キーワードは、同じプロジェクト内の任意の場所のコード、プロジェクトを参照する他のプロジェクト、およびプロジェクトからビルドされた任意のアセンブリから、要素にアクセスできることを指定します。 次のコードは、`Public` の宣言の例を示しています。

```vb
Public Class ClassForEverybody
```

`Public` は、モジュール、インターフェイス、または名前空間レベルでのみ使用できます。 つまり、パブリック要素は、ソースファイルまたは名前空間のレベルで、またはインターフェイス、モジュール、クラス、または構造体の内部で宣言できますが、プロシージャ内では宣言できません。
  
## <a name="protected"></a>Protected

宣言ステートメント内の [Protected](../../../language-reference/modifiers/protected.md) キーワードは、同じクラス内から、またはこのクラスから派生したクラスからのみ要素にアクセスできることを指定します。 次のコードは、`Protected` の宣言の例を示しています。

```vb
Protected Class ClassForMyHeirs
```

`Protected` は、クラスのメンバーを宣言する場合にのみ、クラスレベルで使用できます。 つまり、プロテクト要素はクラスで宣言できますが、ソースファイルまたは名前空間のレベル、またはインターフェイス、モジュール、構造体、またはプロシージャの内部では宣言できません。

## <a name="friend"></a>Friend

宣言ステートメントの [Friend](../../../language-reference/modifiers/friend.md) キーワードは、要素がアセンブリの外部からではなく、同じアセンブリ内からアクセスできることを指定します。 次のコードは、`Friend` の宣言の例を示しています。

```vb
Friend stringForThisProject As String
```

`Friend` は、モジュール、インターフェイス、または名前空間レベルでのみ使用できます。 これは、ソースファイルまたは名前空間のレベルで、またはインターフェイス、モジュール、クラス、または構造体内で、プロシージャ内ではなく、フレンド要素を宣言できることを意味します。

## <a name="protected-friend"></a>Protected Friend

宣言ステートメントで [Protected Friend](../../../language-reference/modifiers/protected-friend.md) キーワードを組み合わせると、派生クラスまたは同じアセンブリ内、またはその両方から要素にアクセスできることが指定されます。 次のコードは、`Protected Friend` の宣言の例を示しています。

```vb
Protected Friend stringForProjectAndHeirs As String
```

`Protected Friend` は、クラスのメンバーを宣言する場合にのみ、クラスレベルで使用できます。 つまり、保護された friend 要素はクラスで宣言できますが、ソースファイルまたは名前空間のレベル、またはインターフェイス、モジュール、構造体、またはプロシージャの内部では宣言できません。

## <a name="private"></a>Private

宣言ステートメントの [Private](../../../language-reference/modifiers/private.md) キーワードは、同じモジュール、クラス、または構造体内からのみ要素にアクセスできることを指定します。 次のコードは、`Private` の宣言の例を示しています。

```vb
Private _numberForMeOnly As Integer
```

`Private` は、モジュール レベルでのみ使用できます。 つまり、モジュール、クラス、または構造体内でプライベート要素を宣言できますが、ソースファイルまたは名前空間のレベル、インターフェイス内、またはプロシージャ内では宣言できません。

モジュールレベルでは、アクセスレベルキーワードのない `Dim` ステートメントは、`Private` 宣言と同じです。 ただし、`Private` キーワードを使用して、コードを読みやすくし、解釈しやすくすることもできます。

## <a name="private-protected"></a>Private Protected

宣言ステートメント内の [Private Protected](../../../language-reference/modifiers/private-protected.md) キーワードの組み合わせは、同じクラス内から、および親クラスと同じアセンブリ内にある派生クラスからのみ、要素にアクセスできることを指定します。 `Private Protected` アクセス修飾子は Visual Basic 15.5 以降でサポートされています。

次の例は、`Private Protected` 宣言を示しています。

```vb
Private Protected internalValue As Integer
```

`Private Protected` 要素は、クラス内でのみ宣言できます。 インターフェイスまたは構造体内で宣言することはできません。また、インターフェイスまたは構造体の内部、またはプロシージャ内のソースファイルや名前空間のレベルで宣言することもできません。

`Private Protected` アクセス修飾子は Visual Basic 15.5 以降でサポートされています。 これを使用するには、次の要素を Visual Basic プロジェクト ( *\*.vbproj*) ファイルに追加します。 システムに Visual Basic 15.5 以降がインストールされている限り、最新バージョンの Visual Basic コンパイラでサポートされているすべての言語機能を利用できます。

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

`Private Protected` アクセス修飾子を使用するには、Visual Basic プロジェクト ( *\*.vbproj*) ファイルに次の要素を追加する必要があります。

```xml
<PropertyGroup>
   <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

詳細については、「[Visual Basic の言語バージョンの設定](../../../language-reference/configure-language-version.md)」を参照してください。

## <a name="access-modifiers"></a>アクセス修飾子

アクセスレベルを指定するキーワードは、*アクセス修飾子*と呼ばれます。 次の表は、アクセス修飾子を比較したものです。

|アクセス修飾子|付与されたアクセスレベル|このアクセスレベルで宣言できる要素|この修飾子を使用できる宣言コンテキスト|
|---------------------|--------------------------|-----------------------------------------------------|----------------------------------------------------------------|
|`Public`|有無<br /><br /> パブリック要素を参照できるすべてのコードは、それにアクセスできます。|インターフェイス<br /><br /> モジュール<br /><br /> クラス<br /><br /> 構造体<br /><br /> 構造体のメンバー<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|ソース ファイル<br /><br /> Namespace<br /><br /> インターフェイス<br /><br /> モジュール<br /><br /> クラス<br /><br /> 構造体|
|`Protected`|Derivational:<br /><br /> 保護された要素を宣言するクラスのコード、またはプロテクト要素から派生したクラスは、要素にアクセスできます。|インターフェイス<br /><br /> クラス<br /><br /> 構造体<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|クラス|
|`Friend`|アセンブリ:<br /><br /> フレンド要素を宣言するアセンブリ内のコードがそれにアクセスできる|インターフェイス<br /><br /> モジュール<br /><br /> クラス<br /><br /> 構造体<br /><br /> 構造体のメンバー<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|ソース ファイル<br /><br /> Namespace<br /><br /> インターフェイス<br /><br /> モジュール<br /><br /> クラス<br /><br /> 構造体|
|`Protected` `Friend`|`Protected` と `Friend`の和集合:<br /><br /> 同じクラスのコード、または保護された friend 要素と同じアセンブリ、または要素のクラスから派生した任意のクラス内のコードは、このコードにアクセスできます|インターフェイス<br /><br /> クラス<br /><br /> 構造体<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|クラス|
|`Private`|宣言コンテキスト:<br /><br /> 含まれている型内のコードを含む、プライベート要素を宣言する型のコードは、要素にアクセスできます。|インターフェイス<br /><br /> クラス<br /><br /> 構造体<br /><br /> 構造体のメンバー<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|モジュール<br /><br /> クラス<br /><br /> 構造体|
|`Private Protected`|プライベートに保護された要素を宣言するクラスのコード、または bas クラスと同じアセンブリ内に存在する派生クラスのコード。|インターフェイス<br /><br /> クラス<br /><br /> 構造体<br /><br /> 手順<br /><br /> プロパティ<br /><br /> メンバー変数<br /><br /> 定数<br /><br /> 列挙<br /><br /> イベント<br /><br /> 外部宣言<br /><br /> デリゲート|クラス|

## <a name="see-also"></a>関連項目

- [Dim ステートメント](../../../language-reference/statements/dim-statement.md)
- [Static](../../../language-reference/modifiers/static.md)
- [宣言された要素の名前](declared-element-names.md)
- [宣言された要素の参照](references-to-declared-elements.md)
- [宣言された要素の特性](declared-element-characteristics.md)
- [Visual Basic の有効期間](lifetime.md)
- [Visual Basic におけるスコープ](scope.md)
- [方法: 変数の可用性を制御する](how-to-control-the-availability-of-a-variable.md)
- [変数](../variables/index.md)
- [変数宣言](../variables/variable-declaration.md)
