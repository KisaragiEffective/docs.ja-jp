---
title: 相互運用性の概要 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- COM interop
- C# language, interoperability
- C++ Interop
- interoperability, about interoperability
- platform invoke
ms.assetid: c025b2e0-2357-4c27-8461-118f0090aeff
ms.openlocfilehash: 2c9eb2a8e6c2db8dc06ebe48ca6eb37d5cf638e7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75700732"
---
# <a name="interoperability-overview-c-programming-guide"></a>相互運用性の概要 (C# プログラミング ガイド)
C# マネージド コードとアンマネージド コード間で相互運用を可能にする方法について説明します。  
  
## <a name="platform-invoke"></a>プラットフォーム呼び出し  
 "*プラットフォーム呼び出し*" とは、Microsoft Windows API にあるような、ダイナミックリンク ライブラリ (DLL) で実装されているアンマネージド関数をマネージド コードで呼び出すことを可能にするサービスです。 これはエクスポートされた関数を見つけて呼び出し、必要に応じて相互運用の境界を越えて、その引数 (整数、文字列、配列、構造体、その他) をマーシャリングします。  
  
詳細については、「[アンマネージ DLL 関数の処理](../../../framework/interop/consuming-unmanaged-dll-functions.md)」と「[プラットフォーム呼び出しを使用して WAV ファイルを再生する方法](./how-to-use-platform-invoke-to-play-a-wave-file.md)」を参照してください。
  
> [!NOTE]
> [共通言語ランタイム](../../../standard/clr.md) (CLR) が、システム リソースへのアクセスを管理します。 CLR の外部のアンマネージ コードを呼び出すと、このセキュリティ メカニズムがバイパスされるため、セキュリティ リスクが生じます。 たとえば、アンマネージ コードがアンマネージ コード内のリソースを直接呼び出した場合、CLR のセキュリティ機構がバイパスされます。 詳細については、「[.NET でのセキュリティ](../../../standard/security/index.md)」を参照してください。  
  
## <a name="c-interop"></a>C++ Interop  
 It Just Works (IJW) とも呼ばれる C++ interop を使用してネイティブ C++ クラスをラップすると、このクラスを C# またはその他の .NET Framework 言語で作成されたコードで使用できるようになります。 これを行うには、C++ コードを記述して、ネイティブ DLL または COM コンポーネントをラップします。 他の .NET Framework 言語とは異なり、Visual C++ には相互運用性サポートが備えられています。これにより、マネージド コードとアンマネージド コードは同じアプリケーション内、また同じファイルでも共存できるようになります。 C++ コードは、マネージド アセンブリを生成する **/clr** コンパイラ スイッチを使用して構築できます。 最後に、C# プロジェクトのアセンブリへの参照を追加し、他のマネージド クラスを使用するときと同じように、ラップされたオブジェクトを使用します。  
  
## <a name="exposing-com-components-to-c"></a>C\# への COM コンポーネントの公開
 C# プロジェクトから COM コンポーネントを使用することができます。 一般的な手順は次のとおりです。  
  
1. 使用する COM コンポーネントを探して登録します。 regsvr32.exe を使用して、COM DLL の登録または登録解除を行います。  
  
2. プロジェクトに、COM コンポーネントまたはタイプ ライブラリへの参照を追加します。  
  
     参照を追加する際、Visual Studio は [Tlbimp.exe (タイプ ライブラリ インポーター)](../../../framework/tools/tlbimp-exe-type-library-importer.md) を使用します。これにより、タイプ ライブラリを入力として取得し、.NET Framework 相互運用アセンブリを出力します。 このアセンブリは、ランタイム呼び出し可能ラッパー (RCW) とも呼ばれ、タイプ ライブラリ内の COM クラスとインターフェイスをラップする、マネージド クラスとインターフェイスを含みます。 Visual Studio は、生成されたアセンブリへの参照をプロジェクトに追加します。  
  
3. RCW で定義されているクラスのインスタンスを作成します。 これにより、COM オブジェクトのインスタンスが作成されます。  
  
4. 他のマネージド オブジェクトを使用するときと同様に、オブジェクトを使用します。 オブジェクトがガベージ コレクションによって解放されるとき、COM オブジェクトのインスタンスもメモリから解放されます。  
  
 詳細については、「[.NET Framework への COM コンポーネントの公開](../../../framework/interop/exposing-com-components.md)」を参照してください。  
  
## <a name="exposing-c-to-com"></a>COM への C# の公開  
 COM クライアントは、適切に公開されている C# 型を使用できます。 C# 型を公開する基本的な手順は次のとおりです。  
  
1. C# プロジェクトに相互運用属性を追加します。  
  
     アセンブリ COM は、Visual C# プロジェクト プロパティを変更することで参照できるようになります。 詳細については、「[[アセンブリ情報] ダイアログ ボックス](/visualstudio/ide/reference/assembly-information-dialog-box)」を参照してください。  
  
2. COM タイプ ライブラリを生成し、COM の使用状況に登録します。  
  
     Visual C# プロジェクト プロパティを変更して、C# アセンブリが COM 相互運用に自動的に登録されるようにできます。 Visual Studio は [ コマンド ライン スイッチを使用して ](../../../framework/tools/regasm-exe-assembly-registration-tool.md)Regasm.exe (アセンブリ登録ツール)`/tlb` を使用します。これにより、マネージド アセンブリが入力として取得され、タイプ ライブラリを生成できます。 タイプ ライブラリは、アセンブリ内の `public` 型を記述し、レジストリ エントリを追加することで、COM クライアントがマネージド クラスを作成できるようにします。  
  
 詳細については、「[COM への .NET Framework コンポーネントの公開](../../../framework/interop/exposing-dotnet-components-to-com.md)」と「[COM クラスの例](./example-com-class.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [相互運用パフォーマンスの向上](https://docs.microsoft.com/previous-versions/msp-n-p/ff647812%28v=pandp.10%29)
- [COM と .NET の相互運用性の概要](/office/client-developer/outlook/pia/introduction-to-interoperability-between-com-and-net)
- [COM 相互運用の概要 (Visual Basic)](../../../visual-basic/programming-guide/com-interop/introduction-to-com-interop.md)
- [マネージド コードとアンマネージド コード間でのマーシャリング](../../../framework/interop/interop-marshaling.md)
- [アンマネージ コードとの相互運用](../../../framework/interop/index.md)
- [C# プログラミングガイド](../index.md)
