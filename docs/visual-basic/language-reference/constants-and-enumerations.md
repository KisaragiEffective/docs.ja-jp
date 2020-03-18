---
title: 定数と列挙体
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
- constants [Visual Basic]
- constants [Visual Basic], list of
ms.assetid: 309c0ad5-83e4-4f96-99ea-83cd95107417
ms.openlocfilehash: 5109bf42c9caa7528c5405bb1a5cff0cfb62a5ac
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75705757"
---
# <a name="constants-and-enumerations-visual-basic"></a>定数と列挙型 (Visual Basic)

Visual Basic には、開発者に対して多数の定義済み定数と列挙型が用意されています。 定数は、アプリケーションの実行全体で一定の値を保持します。 一連の関連する定数を操作する場合や、定数値に名前を関連付ける場合は、列挙型を使うと便利です。  
  
## <a name="constants"></a>定数  
  
### <a name="conditional-compilation-constants"></a>条件付きコンパイル定数  

 次の表は、条件付きコンパイルで使用できる定義済みの定数を示しています。  
  
|**定数**|**説明**|  
|---|---|  
|`CONFIG`|**Configuration Manager**の**アクティブソリューション構成**ボックスの現在の設定に対応する文字列。|  
|`DEBUG`|**[プロジェクトのプロパティ]** ダイアログボックスで設定できる `Boolean` 値。 既定では、プロジェクトのデバッグ構成によって `DEBUG`が定義されます。 `DEBUG` が定義されている場合、<xref:System.Diagnostics.Debug>**クラスのメソッドは出力ウィンドウに**出力を生成します。 定義されていない場合、<xref:System.Diagnostics.Debug> クラスのメソッドはコンパイルされず、デバッグ出力は生成されません。|  
|`TARGET`|プロジェクトの出力の種類、またはコマンドライン**ターゲット**オプションの設定を表す文字列。 `TARGET` に指定できる値は次のとおりです。<br /><br /> -Windows アプリケーションの場合は "winexe"。<br />-"exe" (コンソールアプリケーションの場合)。<br />-クラスライブラリの場合は "library"。<br />-モジュールの "module"。<br />- **Target**オプションは、Visual Studio 統合開発環境で設定できます。 詳細については、「 [-target (Visual Basic)](../../visual-basic/reference/command-line-compiler/target.md)」を参照してください。|  
|`TRACE`|**[プロジェクトのプロパティ]** ダイアログボックスで設定できる `Boolean` 値。 既定では、プロジェクトのすべての構成で `TRACE`が定義されます。 `TRACE` が定義されている場合、<xref:System.Diagnostics.Trace>**クラスのメソッドは出力ウィンドウに**出力を生成します。 定義されていない場合、<xref:System.Diagnostics.Trace> クラスのメソッドはコンパイルされず、`Trace` の出力は生成されません。|  
|`VBC_VER`|*メジャー*で Visual Basic のバージョンを表す数値。*マイナー*形式。|  
  
### <a name="print-and-display-constants"></a>出力定数と表示定数  

 印刷と表示の関数を呼び出す場合は、実際の値の代わりに、コードで次の定数を使用できます。  
  
|**定数**|**説明**|  
|---|---|  
|`vbCrLf`|キャリッジリターン/ラインフィード文字の組み合わせ。|  
|`vbCr`|復帰文字。|  
|`vbLf`|ラインフィード文字。|  
|`vbNewLine`|改行文字。|  
|`vbNullChar`|Null 文字。|  
|`vbNullString`|長さ0の文字列 ("") と同じではありません。外部プロシージャを呼び出すために使用されます。|  
|`vbObjectError`|エラー番号。 ユーザー定義のエラー番号は、この値を超える値にする必要があります。 例:<br /><br /> `Err.Raise(Number) = vbObjectError + 1000`|  
|`vbTab`|タブ文字。|  
|`vbBack`|バックスペース文字。|  
|`vbFormFeed`|Microsoft Windows では使用されません。|  
|`vbVerticalTab`|Microsoft Windows では役に立ちません。|  
  
## <a name="enumerations"></a>列挙  

 次の表に、Visual Basic によって提供される列挙体の一覧とその説明を示します。  
  
|列挙体|説明|  
|---|---|  
|<xref:Microsoft.VisualBasic.AppWinStyle>|<xref:Microsoft.VisualBasic.Interaction.Shell%2A> 関数を呼び出すときに、実行されるプログラムで使用するウィンドウ スタイルを示します。|  
|<xref:Microsoft.VisualBasic.AudioPlayMode>|オーディオ関連のメソッドを呼び出すときに、サウンドの再生方法を示します。|  
|<xref:Microsoft.VisualBasic.ApplicationServices.BuiltInRole>|<xref:Microsoft.VisualBasic.ApplicationServices.User.IsInRole%2A> メソッドを呼び出すときに、チェックするロールの種類を示します。|  
|<xref:Microsoft.VisualBasic.CallType>|<xref:Microsoft.VisualBasic.Interaction.CallByName%2A> 関数を呼び出すときに呼び出すプロシージャの種類を示します。|  
|<xref:Microsoft.VisualBasic.CompareMethod>|比較関数を呼び出すときに、文字列を比較する方法を示します。|  
|<xref:Microsoft.VisualBasic.DateFormat>|<xref:Microsoft.VisualBasic.Strings.FormatDateTime%2A> 関数を呼び出すときに、日付の表示方法を示します。|  
|<xref:Microsoft.VisualBasic.DateInterval>|日付関連の関数を呼び出すときに使用する、日付の間隔の決定方法と形式の設定方法を示します。|  
|<xref:Microsoft.VisualBasic.FileIO.DeleteDirectoryOption>|削除対象のディレクトリ内にファイルまたはディレクトリが存在する場合の処理を指定します。|  
|<xref:Microsoft.VisualBasic.DueDate>|財務関連のメソッドを呼び出すときに、支払い期日を示します。|  
|<xref:Microsoft.VisualBasic.FileIO.FieldType>|テキストフィールドを区切るか、固定幅にするかを示します。|  
|<xref:Microsoft.VisualBasic.FileAttribute>|ファイル アクセス用の関数を呼び出すときに、使用するファイル属性を示します。|  
|<xref:Microsoft.VisualBasic.FirstDayOfWeek>|日付関連の関数を呼び出すときに使用する、1 週間の最初の日を示します。|  
|<xref:Microsoft.VisualBasic.FirstWeekOfYear>|日付関連の関数を呼び出すときに使用する、1 年の最初の週を示します。|  
|<xref:Microsoft.VisualBasic.MsgBoxResult>|<xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 関数によって返され、メッセージ ボックスのどのボタンが押されたかを示します。|  
|<xref:Microsoft.VisualBasic.MsgBoxStyle>|<xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 関数を呼び出すときに表示するボタンを示します。|  
|<xref:Microsoft.VisualBasic.OpenAccess>|ファイル アクセス用の関数を呼び出すときにファイルを開く方法を示します。|  
|<xref:Microsoft.VisualBasic.OpenMode>|ファイル アクセス用の関数を呼び出すときにファイルを開く方法を示します。|  
|<xref:Microsoft.VisualBasic.OpenShare>|ファイル アクセス用の関数を呼び出すときにファイルを開く方法を示します。|  
|<xref:Microsoft.VisualBasic.FileIO.RecycleOption>|ファイルを完全に削除するか、ごみ箱に配置するかを指定します。|  
|<xref:Microsoft.VisualBasic.FileIO.SearchOption>|すべてのディレクトリまたは最上位レベルのディレクトリの、いずれを探索するのかを指定します。|  
|<xref:Microsoft.VisualBasic.TriState>|`Boolean` 値を示します。または、数値書式指定関数を呼び出すときに既定値を使用するかどうかを示します。|  
|<xref:Microsoft.VisualBasic.FileIO.UICancelOption>|操作中にユーザーが **[キャンセル]** をクリックした場合の処理方法を指定します。|  
|<xref:Microsoft.VisualBasic.FileIO.UIOption>|ファイルまたはディレクトリをコピー、削除、または移動するときに進行状況ダイアログを表示するかどうかを指定します。|  
|<xref:Microsoft.VisualBasic.VariantType>|<xref:Microsoft.VisualBasic.Information.VarType%2A> 関数が返すバリアント オブジェクトの型を示します。|  
|<xref:Microsoft.VisualBasic.VbStrConv>|<xref:Microsoft.VisualBasic.Strings.StrConv%2A> 関数の呼び出しで実行する変換の種類を示します。|  
  
## <a name="see-also"></a>関連項目

- [Visual Basic の言語リファレンス](../../visual-basic/language-reference/index.md)
- [定数の概要](../../visual-basic/programming-guide/language-features/constants-enums/constants-overview.md)
- [列挙型の概要](../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
