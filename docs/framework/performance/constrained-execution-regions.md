---
title: 制約された実行領域
ms.date: 03/30/2017
helpviewer_keywords:
- constrained execution regions
- CERs
ms.assetid: 99354547-39c1-4b0b-8553-938e8f8d1808
ms.openlocfilehash: fde2bab99f156ddffec678022a58e7b14e0af01e
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716172"
---
# <a name="constrained-execution-regions"></a>制約された実行領域
制約された実行領域 (CER) は、信頼性のあるマネージド コードを作成するための機構の一部です。 CER は、領域内のコードが領域全体で実行されるのを防ぐ帯域外の例外を、共通言語ランタイム (CLR) がスローすることが制約された領域を定義します。 その領域内では、ユーザー コードは、帯域外の例外がスローされることになるコードの実行を制約されます。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> メソッドは `try` ブロックの直前にある必要があります。このメソッドによって、`catch`、`finally`、`fault` の各ブロックが制約された実行領域としてマークされます。 制約された領域としてマークされると、コードは信頼性の高いコントラクトでのみ他のコードを呼び出す必要があります。また、コードは、エラーを処理する準備ができている場合を除き、準備されていないメソッドや信頼性のないメソッドの割り当てや仮想呼び出しを行うことはできません。 CLR は、CER で実行されるコードのスレッドの中止を遅らせます。  
  
 制約された実行領域は、注釈付きの `try` ブロックだけでなく、特に <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> クラスから派生したクラス、および <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A> メソッドを使用して実行されるコードで実行するクリティカル ファイナライザーなど、CLR のさまざまな形式で使用されます。  
  
## <a name="cer-advance-preparation"></a>CER の事前準備  
 CLR は、メモリ不足の状態を回避するために、CER を事前に準備します。 JIT コンパイル時や型の読み込み時に、CLR がメモリ不足の状態を引き起こさないように、事前準備が必要となります。  
  
 次のように、開発者はコード領域が CER であることを示す必要があります。  
  
- <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> 属性が適用されたすべての呼び出し先のトップレベルの CER 領域とメソッドは事前に準備されます。 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> は、<xref:System.Runtime.ConstrainedExecution.Cer.Success> または <xref:System.Runtime.ConstrainedExecution.Cer.MayFail> の保証のみを示すことができます。  
  
- 仮想ディスパッチなど、静的に決定できない呼び出しに対して事前準備を行うことはできません。 このような場合には、<xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A> メソッドを使用します。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A> メソッドを使用する場合は、<xref:System.Runtime.ConstrainedExecution.PrePrepareMethodAttribute> 属性をクリーンアップ コードに適用する必要があります。  
  
## <a name="constraints"></a>制約  
 ユーザーは、CER で書き込むことのできるコードの種類で制約を受けます。 コードでは、次の操作によって生じる可能性のある帯域外の例外を発生させることはできません。  
  
- 明示的な割り当て。  
  
- ボックス化。  
  
- ロックの取得。  
  
- 準備されていないメソッドの仮想呼び出し。  
  
- 信頼性の低いコントラクトまたは信頼性のないコントラクトでのメソッド呼び出し。  
  
 .NET Framework Version 2.0 では、これらの制約はガイドラインです。 診断はコード分析ツールを通じて提供されます。  
  
## <a name="reliability-contracts"></a>信頼性のコントラクト  
 <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> は、信頼性の保証と特定のメソッドの破損状態を文書化するカスタム属性です。  
  
### <a name="reliability-guarantees"></a>信頼性の保証  
 <xref:System.Runtime.ConstrainedExecution.Cer> 列挙値によって表される信頼性の保証は、特定のメソッドの信頼度を次のように示します。  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.MayFail>. 例外的な条件下で、メソッドは失敗する可能性があります。 この場合、メソッドは呼び出し元のメソッドに成功したか失敗したかを報告します。 メソッドが戻り値を確実に報告できるように、メソッドは CER に含まれている必要があります。  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.None>. メソッド、型、またはアセンブリには CER の概念がないため、状態の破損を大幅に軽減しなければ、CER 内での呼び出しが安全ではない可能性が最も高くなります。 この値では、CER の保証を利用しません。 これは以下を意味します。  
  
    1. 例外的な条件下で、メソッドが失敗する可能性があります。  
  
    2. メソッドは、失敗したことを報告する場合としない場合があります。  
  
    3. メソッドは、CER の最も可能性の高いシナリオを使用するように書かれていません。  
  
    4. メソッド、型、またはアセンブリの成功が明示的に識別されていない場合、暗黙的に <xref:System.Runtime.ConstrainedExecution.Cer.None> として識別されます。  
  
- <xref:System.Runtime.ConstrainedExecution.Cer.Success>. 例外的な条件下で、メソッドは成功することが保証されます。 このレベルの信頼性を実現するには、メソッドが CER 以外の領域内から呼び出される場合でも、呼び出されるメソッドの周辺に CER を常に構築する必要があります。 成功の判断は主観的なものですが、メソッドが意図したとおりに実行された場合、そのメソッドは成功ということになります。 たとえば、Count を `ReliabilityContractAttribute(Cer.Success)` でマークした場合、CER での実行時に、常に <xref:System.Collections.ArrayList> の要素数が返され、内部フィールドを不明な状態のままにすることはできないことを意味します。  <xref:System.Threading.Interlocked.CompareExchange%2A> メソッドも同様に成功としてマークされますが、その成功は競合状態によって値を新しい値に置き換えることができなかったことを意味する場合があります。  重要な点は、メソッドが文書化されたとおりに動作することと、正しいにもかかわらず信頼性のないコードの外観以外の異常な動作を予測して CER コードを記述する必要はないということです。  
  
### <a name="corruption-levels"></a>破損レベル  
 <xref:System.Runtime.ConstrainedExecution.Consistency> 列挙値によって表される破損レベルは、特定の環境で状態がどの程度破損する可能性があるかを示します。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain>. 例外的な条件下で、共通言語ランタイム (CLR) は、現在のアプリケーション ドメインにおける状態の一貫性に関して一切保証しません。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptInstance>. 例外的な条件下で、メソッドは状態の破損を現在のインスタンスに限定することが保証されます。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>。例外的な条件下で、CLR は状態の一貫性に関して一切保証しません。つまり、条件によってプロセスが破損する可能性があります。  
  
- <xref:System.Runtime.ConstrainedExecution.Consistency.WillNotCorruptState>. 例外的な条件下で、メソッドは状態を破損しないことが保証されます。  
  
## <a name="reliability-trycatchfinally"></a>信頼性の try/catch/finally  
 信頼性の `try/catch/finally` は、予測可能性の保証がアンマネージ バージョンと同じレベルである例外処理機構です。 `catch/finally` ブロックは CER です。 ブロック内のメソッドには事前準備が必要であり、中断不可能である必要があります。  
  
 .NET Framework バージョン 2.0 では、コードは、try ブロックの直前に <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> を呼び出すことにより、try が信頼できることをランタイムに通知します。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> は、コンパイラ サポート クラスである <xref:System.Runtime.CompilerServices.RuntimeHelpers> のメンバーです。 コンパイラで使用できるようになるまで <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> を直接呼び出します。  
  
## <a name="noninterruptible-regions"></a>中断不可能な領域  
 中断不可能な領域は、一連の命令を CER にグループ化します。  
  
 .NET Framework バージョン 2.0 では、コンパイラ サポートで使用できるようになるまで、ユーザー コードは、前に <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> メソッド呼び出しがある空の try/catch ブロックを含む信頼性のある try/catch/finally を使用して、中断不可能な領域を作成します。  
  
## <a name="critical-finalizer-object"></a>クリティカル ファイナライザー オブジェクト  
 <xref:System.Runtime.ConstrainedExecution.CriticalFinalizerObject> は、ガベージ コレクションでファイナライザーが実行されることを保証します。 割り当て時に、ファイナライザーとその呼び出し先は事前に準備されます。 ファイナライザー メソッドは CER で実行されるため、CER とファイナライザーのすべての制約に従う必要があります。  
  
 <xref:System.Runtime.InteropServices.SafeHandle> および <xref:System.Runtime.InteropServices.CriticalHandle> から継承するすべての型は、CER 内でファイナライザーが実行されることが保証されます。 <xref:System.Runtime.InteropServices.SafeHandle> の派生クラスで <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> を実装して、ハンドルを解放する必要のあるコードを実行します。  
  
## <a name="code-not-permitted-in-cers"></a>CER で許可されないコード  
 CER では、次の操作は許可されません。  
  
- 明示的な割り当て。  
  
- ロックの取得。  
  
- ボックス化。  
  
- 多次元配列アクセス。  
  
- リフレクションを通じたメソッド呼び出し。  
  
- <xref:System.Threading.Monitor.Enter%2A> または <xref:System.IO.FileStream.Lock%2A>。  
  
- セキュリティ チェック。 要求は実行しないでください (リンク確認要求のみ)。  
  
- COM オブジェクトおよびプロキシの <xref:System.Reflection.Emit.OpCodes.Isinst> と <xref:System.Reflection.Emit.OpCodes.Castclass>。  
  
- 透過プロキシのフィールドの取得または設定。  
  
- シリアル化。  
  
- 関数ポインターとデリゲート。  
  
## <a name="see-also"></a>関連項目

- [信頼性に関するベスト プラクティス](reliability-best-practices.md)
