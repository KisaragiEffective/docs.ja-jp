---
title: 完全修飾型名の指定
ms.date: 02/21/2019
helpviewer_keywords:
- names [.NET Framework], fully qualified type names
- reflection, fully qualified type names
- names [.NET Framework], assemblies
- tokens
- BNF
- assemblies [.NET Framework], names
- languages, grammar
- fully qualified type names
- type names
- special characters
- IDENTIFIER
ms.assetid: d90b1e39-9115-4f2a-81c0-05e7e74e5580
ms.openlocfilehash: 707c71482196d789ed9a88db34af048ec57734fb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130028"
---
# <a name="specifying-fully-qualified-type-names"></a>完全修飾型名の指定

多様なリフレクション操作に対して有効な入力の型名を指定する必要があります。 完全修飾型名は、アセンブリ名の指定、名前空間の指定、および型名で構成されます。 型名の指定は、<xref:System.Type.GetType%2A?displayProperty=nameWithType>、<xref:System.Reflection.Module.GetType%2A?displayProperty=nameWithType>、<xref:System.Reflection.Emit.ModuleBuilder.GetType%2A?displayProperty=nameWithType>、<xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> などのメソッドで使用されます。

## <a name="grammar-for-type-names"></a>型名の文法

 文法で正式な言語の構文が定義されます。 次の表は、有効な入力を認識する方法を示す構文規則の一覧です。 終端要素 (これ以上は単純化できない要素) はすべて大文字で記載しています。 非終端要素 (これ以上単純化することができる要素) は、大文字と小文字の混在、または単一引用符で囲まれた文字列で記載していますが、単一引用符 (') 自体は構文の一部ではありません。 パイプ文字 (&#124;) は、サブ規則がある規則を示します。

<!-- markdownlint-disable MD010 -->
```antlr
TypeSpec
    : ReferenceTypeSpec
    | SimpleTypeSpec
    ;

ReferenceTypeSpec
    : SimpleTypeSpec '&'
    ;

SimpleTypeSpec
    : PointerTypeSpec
    | GenericTypeSpec
    | TypeName
    ;

GenericTypeSpec
   : SimpleTypeSpec ` NUMBER

PointerTypeSpec
    : SimpleTypeSpec '*'
    ;

ArrayTypeSpec
    : SimpleTypeSpec '[ReflectionDimension]'
    | SimpleTypeSpec '[ReflectionEmitDimension]'
    ;

ReflectionDimension
    : '*'
    | ReflectionDimension ',' ReflectionDimension
    | NOTOKEN
    ;

ReflectionEmitDimension
    : '*'
    | Number '..' Number
    | Number '…'
    | ReflectionDimension ',' ReflectionDimension
    | NOTOKEN
    ;

Number
    : [0-9]+
    ;

TypeName
    : NamespaceTypeName
    | NamespaceTypeName ',' AssemblyNameSpec
    ;

NamespaceTypeName
    : NestedTypeName
    | NamespaceSpec '.' NestedTypeName
    ;

NestedTypeName
    : IDENTIFIER
    | NestedTypeName '+' IDENTIFIER
    ;

NamespaceSpec
    : IDENTIFIER
    | NamespaceSpec '.' IDENTIFIER
    ;

AssemblyNameSpec
    : IDENTIFIER
    | IDENTIFIER ',' AssemblyProperties
    ;

AssemblyProperties
    : AssemblyProperty
    | AssemblyProperties ',' AssemblyProperty
    ;

AssemblyProperty
    : AssemblyPropertyName '=' AssemblyPropertyValue
    ;
```
<!-- markdownlint-enable MD010 -->

## <a name="specifying-special-characters"></a>特殊文字の指定

型名の IDENTIFIER は、言語の規則で決められた任意の有効な名前です。

IDENTIFIER の一部として使用する場合、次のトークンを区切るには、エスケープ文字としてバックスラッシュ (\\) を使用します。

|トークン|説明|
|-----------|-------------|
|\\、|アセンブリの区切り文字。|
|\\+|入れ子にされた型の区切り文字。|
|\\&|参照型。|
|\\*|ポインター型。|
|\\[|配列次元の区切り文字。|
|\\]|配列次元の区切り文字。|
|\\.|配列の指定内にピリオドを使用する場合にのみ、ピリオドの前にバックスラッシュを使用します。 NamespaceSpec 内のピリオドにはバックスラッシュを使用できません。|
|\\\|文字列リテラルとして必要な場合のバックスラッシュ。|

AssemblyNameSpec を除くすべての TypeSpec コンポーネントで、スペースは関係があります。 AssemblyNameSpec の場合、',' 区切り文字の前にあるスペースは関係がありますが、',' の後のスペースは無視されます。

<xref:System.Type.FullName%2A?displayProperty=nameWithType> などのリフレクション クラスは、`MyType.GetType(myType.FullName)` と同様に返される名前を <xref:System.Type.GetType%2A> の呼び出しに使用できるように、壊れた名前を返します。

たとえば、型の完全修飾型名が `Ozzy.OutBack.Kangaroo+Wallaby,MyAssembly` だとします。

名前空間が `Ozzy.Out+Back` の場合、プラス記号の前にはバックスラッシュを指定する必要があります。 そうしないと、パーサーから入れ子の区切り文字として解釈されます。 リフレクションからこの文字列は `Ozzy.Out\+Back.Kangaroo+Wallaby,MyAssembly` として出力されます。

## <a name="specifying-assembly-names"></a>アセンブリ名の指定

アセンブリ名の指定に必要な最小限の情報は、アセンブリのテキスト形式の名前 (IDENTIFIER) です。 次の表で説明されているプロパティ/値ペアのコンマ区切りの一覧で、IDENTIFIER に従うことができます。 IDENTIFIER の名前付けは、ファイルの名前付け規則に従う必要があります。 IDENTIFIER は大文字と小文字が区別されません。

|プロパティ名|説明|使用できる値|
|-------------------|-----------------|----------------------|
|**Version**|アセンブリのバージョン番号|*Major.Minor.Build.Revision*。*Major*、*Minor*、*Build*、*Revision* は 0 以上 65535 以下の整数です。|
|**PublicKey**|完全な公開鍵|16 進数形式の完全な公開鍵の文字列値。 プライベート アセンブリを明示的に指定するには、null 参照を指定します (Visual Basic では **Nothing**)。|
|**PublicKeyToken**|公開鍵トークン (完全な公開鍵の 8 バイト ハッシュ)|16 進数形式の公開鍵トークンの文字列値。 プライベート アセンブリを明示的に指定するには、null 参照を指定します (Visual Basic では **Nothing**)。|
|**カルチャ**|アセンブリのカルチャ|RFC 1766 形式のアセンブリのカルチャ。言語に依存しない (非サテライト) アセンブリの場合は "neutral"。|
|**カスタム**|カスタム バイナリ ラージ オブジェクト (BLOB)。 現在、 [Native Image Generator (Ngen)](../tools/ngen-exe-native-image-generator.md) で生成されたアセンブリでのみ使用されています。|アセンブリがインストールされていることをアセンブリ キャッシュに通知するために Native Image Generator ツールに使用されているカスタム文字列は、ネイティブ イメージです。そのため、ネイティブ イメージ キャッシュにインストールされます。 zap 文字列とも呼ばれます。|

次に、カルチャが既定で単純な名前のアセンブリの**AssemblyName** の例を示します。

```csharp
com.microsoft.crypto, Culture=""
```

次に、カルチャが "en" の厳密な名前のアセンブリに対する完全に指定された参照の例を示します。

```csharp
com.microsoft.crypto, Culture=en, PublicKeyToken=a5d015c7d5a0b012,
    Version=1.0.0.0
```

次の各例は、部分的に指定された **AssemblyName** を示しています。これは厳密な名前または単純な名前のアセンブリで満たすことができます。

```csharp
com.microsoft.crypto
com.microsoft.crypto, Culture=""
com.microsoft.crypto, Culture=en
```

次の各例は、部分的に指定された **AssemblyName** を示しています。これは簡易な名前のアセンブリで満たす必要があります。

```csharp
com.microsoft.crypto, Culture="", PublicKeyToken=null
com.microsoft.crypto, Culture=en, PublicKeyToken=null
```

次の各例は、部分的に指定された **AssemblyName** を示しています。これは厳密な名前のアセンブリで満たす必要があります。

```csharp
com.microsoft.crypto, Culture="", PublicKeyToken=a5d015c7d5a0b012
com.microsoft.crypto, Culture=en, PublicKeyToken=a5d015c7d5a0b012,
    Version=1.0.0.0
```

## <a name="specifying-generic-types"></a>ジェネリック型の指定

SimpleTypeSpec\`NUMBER は、ジェネリック型パラメーター が 1 ～ *n* であるオープン ジェネリック型を表します。 たとえば、オープン ジェネリック型 List\<T> またはクローズ ジェネリック型 List\<String> に対する参照を取得するには、``Type.GetType("System.Collections.Generic.List`1")`` を使用します。ジェネリック型 Dictionary\<TKey,TValue> に対する参照を取得するには、``Type.GetType("System.Collections.Generic.Dictionary`2")`` を使用します。

## <a name="specifying-pointers"></a>ポインターの指定

SimpleTypeSpec* はアンマネージ ポインターを示します。 たとえば、型 MyType に対するポインターを取得するには、`Type.GetType("MyType*")` を使用します。 型 MyType のポインターに対するポインターを取得するには、`Type.GetType("MyType**")` を使用します。

## <a name="specifying-references"></a>参照の指定

SimpleTypeSpec &amp; はマネージド ポインターまたは参照を表します。 たとえば、型 MyType に対する参照を取得するには、`Type.GetType("MyType &")` を使用します。 ポインターとは異なり、参照は 1 つのレベルに制限されます。

## <a name="specifying-arrays"></a>配列の指定

BNF 文法では、ReflectionEmitDimension は <xref:System.Reflection.Emit.ModuleBuilder.GetType%2A?displayProperty=nameWithType> を使用して取得された不完全な型定義にのみ適用されます。 不完全な型定義とは、<xref:System.Reflection.Emit.TypeBuilder.CreateType%2A?displayProperty=nameWithType> で呼び出されていない、<xref:System.Reflection.Emit?displayProperty=nameWithType> を使用して構築された <xref:System.Reflection.Emit.TypeBuilder> オブジェクトです。 ReflectionDimension を使用して、完了している任意の型定義 (読み込まれている型) を取得できます。

配列のランクを指定して、リフレクションで配列にアクセスします。

- `Type.GetType("MyArray[]")` は 0 個の下限がある 1 次元配列を取得します。

- `Type.GetType("MyArray[*]")` は不明な下限がある 1 次元配列を取得します。
- `Type.GetType("MyArray[][]")` は 2 次元配列の配列です。

- `Type.GetType("MyArray[*,*]")` と `Type.GetType("MyArray[,]")` は、下限が不明な四角形の 2 次元配列を取得します。

ランタイムの観点からは、多次元配列を別にすれば、`MyArray[] != MyArray[*]` の 2 つの表記は同等です。 つまり、`Type.GetType("MyArray [,]") == Type.GetType("MyArray[*,*]")` は **true** と評価されます。

**ModuleBuilder.GetType** の場合、`MyArray[0..5]` はサイズが 6 で下限が 0 の 1 次元配列です。 `MyArray[4…]` は、不明なサイズで下限が 4 の 1 次元配列です。

## <a name="see-also"></a>関連項目

- <xref:System.Reflection.AssemblyName>
- <xref:System.Reflection.Emit.ModuleBuilder>
- <xref:System.Reflection.Emit.TypeBuilder>
- <xref:System.Type.FullName%2A?displayProperty=nameWithType>
- <xref:System.Type.GetType%2A?displayProperty=nameWithType>
- <xref:System.Type.AssemblyQualifiedName%2A?displayProperty=nameWithType>
- [型情報の表示](viewing-type-information.md)
