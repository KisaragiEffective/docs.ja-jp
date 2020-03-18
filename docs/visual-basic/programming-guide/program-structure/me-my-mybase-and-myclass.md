---
title: Me、My、MyBase、および MyClass
ms.date: 07/20/2015
f1_keywords:
- MyClass
- vb.Me
- MyBase
- vb.MyBase
- Me
- vb.MyClass
- vb.This
- vb.My
helpviewer_keywords:
- My object
- self-reference [Visual Basic], Me keyword
- MyClass keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], referring to the current instance of an object
- Me keyword [Visual Basic]
- self-reference
- current instance [Visual Basic], Me keyword
- MyBase keyword [Visual Basic], relationship to similar programming elements
ms.assetid: f8e241ae-b1ed-4886-9aa0-08c632154029
ms.openlocfilehash: a21dfeb12e8d99f5f8b8afede084846711c299ab
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347334"
---
# <a name="me-my-mybase-and-myclass-in-visual-basic"></a>Visual Basic における Me、My、MyBase、MyClass
Visual Basic の `Me`、`My`、`MyBase`、および `MyClass` の名前は似ていますが、目的は異なります。 このトピックでは、これらの各エンティティについて説明します。  
  
## <a name="me"></a>Me  
 `Me` キーワードは、コードが現在実行されているクラスまたは構造体の特定のインスタンスを参照する手段を提供します。 `Me` は、オブジェクト変数または現在のインスタンスを参照する構造体変数のいずれかのように動作します。 `Me` の使用は、クラスまたは構造体の現在実行中のインスタンスに関する情報を、別のクラス、構造体、またはモジュール内のプロシージャに渡す場合に特に便利です。  
  
 たとえば、モジュールに次のプロシージャがあるとします。  
  
```vb  
Sub ChangeFormColor(FormName As Form)  
   Randomize()  
   FormName.BackColor = Color.FromArgb(Rnd() * 256, Rnd() * 256, Rnd() * 256)  
End Sub  
```  
  
 次のステートメントを使用して、このプロシージャを呼び出して、<xref:System.Windows.Forms.Form> クラスの現在のインスタンスを引数として渡すことができます。  
  
```vb  
ChangeFormColor(Me)  
```  
  
## <a name="my"></a>My  
 `My` 機能を使用すると、さまざまな .NET Framework クラスに簡単かつ直感的にアクセスできるので、Visual Basic ユーザーはコンピューター、アプリケーション、設定、リソースなどを操作できます。  
  
## <a name="mybase"></a>MyBase  
 `MyBase` キーワードは、クラスの現在のインスタンスの基底クラスを参照するオブジェクト変数のように動作します。 `MyBase` は、通常、派生クラスでオーバーライドまたはシャドウされる基底クラスのメンバーにアクセスするために使用されます。 `MyBase.New` は、派生クラスのコンストラクターから基底クラスのコンストラクターを明示的に呼び出すために使用されます。  
  
## <a name="myclass"></a>MyClass  
 `MyClass` キーワードは、もともと実装されているクラスの現在のインスタンスを参照するオブジェクト変数のように動作します。 `MyClass` は `Me`に似ていますが、メソッドの呼び出しはすべて、メソッドが `NotOverridable`されているかのように扱われます。  
  
## <a name="see-also"></a>関連項目

- [継承の基本](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
