---
title: MsgBox のサンプル
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- marshaling, MsgBox sample
- data marshaling, MsgBox sample
ms.assetid: 9e0edff6-cc0d-4d5c-a445-aecf283d9c3a
ms.openlocfilehash: 9f1e6d58742f60b6043a4be9218b80b323cd523e
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124104"
---
# <a name="msgbox-sample"></a>MsgBox のサンプル
このサンプルでは、文字列型を In パラメーターとして値渡しする方法と、<xref:System.Runtime.InteropServices.DllImportAttribute.EntryPoint>、<xref:System.Runtime.InteropServices.DllImportAttribute.CharSet>、および <xref:System.Runtime.InteropServices.DllImportAttribute.ExactSpelling> の各フィールドを使用する場合について説明します。  
  
 MsgBox のサンプルで使用するアンマネージ関数とその関数宣言を次に示します。  
  
- User32.dll からエクスポートされる **MessageBox**。  
  
    ```cpp
    int MessageBox(HWND hWnd, LPCTSTR lpText, LPCTSTR lpCaption,   
       UINT uType);  
    ```  
  
 このサンプルでは、`NativeMethods` クラスの中には、`MsgBoxSample` クラスによって呼び出される各アンマネージド 関数に関するマネージド プロトタイプが含まれます。 マネージド プロトタイプ メソッドの `MsgBox`、`MsgBox2`、および `MsgBox3` は、同じアンマネージド 関数に対して異なる宣言を持ちます。  
  
 `MsgBox2` に対する宣言により、メッセージ ボックス内に不正な出力が生成されます。その原因は、ANSI として指定した文字型が、Unicode 関数の名前であるエントリ ポイント `MessageBoxW` と一致しないからです。 `MsgBox3` に対する宣言により、**EntryPoint**、**CharSet**、および **ExactSpelling** の各フィールド間に不一致が発生します。 `MsgBox3` を呼び出すと例外がスローされます。 文字列の名前付けと名前のマーシャリングの詳細については、「[文字セットの指定](specifying-a-character-set.md)」を参照してください。  
  
## <a name="declaring-prototypes"></a>プロトタイプの宣言  
 [!code-cpp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#5)]
 [!code-csharp[Conceptual.Interop.Marshaling#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#5)]
 [!code-vb[Conceptual.Interop.Marshaling#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#5)]  
  
## <a name="calling-functions"></a>関数の呼び出し  
 [!code-cpp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/msgbox.cpp#6)]
 [!code-csharp[Conceptual.Interop.Marshaling#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/msgbox.cs#6)]
 [!code-vb[Conceptual.Interop.Marshaling#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/msgbox.vb#6)]  
  
## <a name="see-also"></a>関連項目

- [マーシャリング (文字列の)](marshaling-strings.md)
- [文字列に対する既定のマーシャリング](default-marshaling-for-strings.md)
- [マネージド コードでのプロトタイプの作成](creating-prototypes-in-managed-code.md)
- [文字セットの指定](specifying-a-character-set.md)
