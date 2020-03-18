---
title: JIT トレース ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- JIT tracing events [.NET Framework]
- ETW, JIT tracing events (CLR)
ms.assetid: 926adde2-c123-452e-bf4f-4b977bf06ffb
ms.openlocfilehash: 37bfd09516589f3422ee005233e576b110ef1288
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716004"
---
# <a name="jit-tracing-etw-events"></a>JIT トレース ETW イベント
これらのイベントは、Just-in-time (JIT) インライン展開と JIT 末尾呼び出しの成功または失敗に関する情報を収集します。

## <a name="jit-inlining-events"></a>JIT インライン展開イベント

### <a name="methodjitinliningfailed-event"></a>MethodJitInliningFailed イベント
 次の表に、キーワードとレベルを示します。 (詳細については、「 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)」を参照してください)。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|詳細 (5)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`MethodJitInliningFailed`|186|JIT インライン展開が失敗した。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|コンパイルされるメソッドの名前空間。|  
|MethodBeingCompiledName|win:UnicodeString|コンパイルされるメソッドの名前。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|コンパイルされるメソッドのシグネチャ。|  
|InlinerNamespace|win:UnicodeString|JIT コンパイラのコード生成の対象であるメソッドの名前空間。|  
|InlinerName|win:UnicodeString|コンパイラのコード生成の対象であるメソッドの名前。 コンパイラが `MethodBeingCompiledName` への呼び出しを生成するのではなく `MethodBeingCompiledName` へのインライン コードを生成しようとしている場合、これは `InlinerName`とは異なる可能性があります。|  
|InlinerNameSignature|win:UnicodeString|このインライナのシグネチャは次のとおりです。|  
|InlineeNamespace|win:UnicodeString|インライン展開先の名前空間。|  
|InlineeName|win:UnicodeString|コンパイラによるインライン先のメソッド (呼び出しの生成先ではない)。|  
|InlineeNameSignature|win:UnicodeString|インライン展開先のシグネチャ。|  
|FailAlways|win:Boolean|そのインライン展開先に対してインライン展開が常に失敗するという JIT コンパイラへのヒント。|  
|FailReason|win:UnicodeString|INLINE_NEVER は、前回のインライン展開の試みで、インライン展開が他の理由により成功しないと判断されたことを意味します。それ以外の場合は、自由形式のテキストです。|  
|ClrInstanceID|win:UnicodeString|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="methodjitinliningsucceeded-event"></a>MethodJitInliningSucceeded イベント  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|詳細 (5)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`MethodJitInliningSucceeded`|185|メソッドのインライン展開が成功した。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|コンパイルされるメソッドの名前空間。|  
|MethodBeingCompiledName|win:UnicodeString|コンパイルされるメソッドの名前。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|コンパイルされるメソッドのシグネチャ。|  
|InlinerNamespace|win:UnicodeString|JIT コンパイラのコード生成の対象であるメソッドの名前空間。|  
|InlinerName|win:UnicodeString|コンパイラのコード生成の対象であるメソッドの名前。 コンパイラが `MethodBeingCompiledName` への呼び出しを生成するのではなく `MethodBeingCompiledName` へのインライン コードを生成しようとしている場合、これは `InlinerName`とは異なる可能性があります。|  
|InlinerNameSignature|win:UnicodeString|このインライナのシグネチャは次のとおりです。|  
|InlineeNamespace|win:UnicodeString|インライン展開先の名前空間。|  
|InlineeName|win:UnicodeString|コンパイラによるインライン先のメソッド (呼び出しの生成先ではない)。|  
|InlineeNameSignature|win:UnicodeString|インライン展開先のシグネチャ。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  

## <a name="jit-tail-call-events"></a>JIT 末尾呼び出しイベント  
  
### <a name="methodjittailcallfailed-event"></a>MethodJITTailCallFailed イベント  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|詳細 (5)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallFailed`|189|メソッドの末尾の呼び出しが失敗した。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|コンパイルされるメソッドの名前空間。|  
|MethodBeingCompiledName|win:UnicodeString|コンパイルされるメソッドの名前。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|コンパイルされるメソッドのシグネチャ。|  
|CallerNamespace|win:UnicodeString|JIT コンパイラのコード生成の対象であるメソッドの名前空間。|  
|CallerName|win:UnicodeString|コンパイラのコード生成の対象であるメソッドの名前。|  
|CallerNameSignature|win:UnicodeString|呼び出し元のシグネチャ。|  
|CalleeNamespace|win:UnicodeString|呼び出し先の名前空間。|  
|CalleeName|win:UnicodeString|コンパイラによる末尾呼び出し先のメソッド (呼び出しの生成先ではない)。|  
|CalleeNameSignature|win:UnicodeString|呼び出し先のシグネチャ。|  
|TailPrefix|win:Boolean|末尾呼び出しのプレフィックス。|  
|FailReason|win:UnicodeString|末尾の呼び出しが失敗した理由。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="methodjittailcallsucceeded-event"></a>MethodJITTailCallSucceeded イベント  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|レベル|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|詳細 (5)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallSucceeded`|188|メソッドの末尾の呼び出しが成功した。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|[データ型]|説明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|コンパイルされるメソッドの名前空間。|  
|MethodBeingCompiledName|win:UnicodeString|コンパイルされるメソッドの名前。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|コンパイルされるメソッドのシグネチャ。|  
|CallerNamespace|win:UnicodeString|JIT コンパイラのコード生成の対象であるメソッドの名前空間。|  
|CallerName|win:UnicodeString|コンパイラのコード生成の対象であるメソッドの名前。|  
|CallerNameSignature|win:UnicodeString|呼び出し元のシグネチャ。|  
|CalleeNamespace|win:UnicodeString|呼び出し先の名前空間。|  
|CalleeName|win:UnicodeString|コンパイラによる末尾呼び出し先のメソッド (呼び出しの生成先ではない)。|  
|CalleeNameSignature|win:UnicodeString|呼び出し先のシグネチャ。|  
|TailPrefix|win:Boolean|末尾呼び出しのプレフィックス。|  
|TailCallType|win:UnicodeString|末尾呼び出しの種類。|  
|ClrInstanceID|win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
## <a name="see-also"></a>関連項目

- [CLR ETW イベント](clr-etw-events.md)
