---
title: TypeConverters および XAML
ms.date: 03/30/2017
helpviewer_keywords:
- XAML [WPF], TypeConverter class
ms.assetid: f6313e4d-e89d-497d-ac87-b43511a1ae4b
ms.openlocfilehash: 6b8b58228e94ed12557e97406e55cc4165753076
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095087"
---
# <a name="typeconverters-and-xaml"></a>TypeConverters および XAML
このトピックでは、一般的な XAML 言語機能としての文字列からの型変換の目的について説明します。 .NET Framework では、<xref:System.ComponentModel.TypeConverter> クラスは、XAML 属性の使用でプロパティ値として使用できるマネージカスタムクラスの実装の一部として特定の目的を果たします。 カスタムクラスを作成し、クラスのインスタンスを XAML 設定可能な属性値として使用できるようにする場合は、クラスに <xref:System.ComponentModel.TypeConverterAttribute> を適用したり、カスタム <xref:System.ComponentModel.TypeConverter> クラス、またはその両方を記述したりすることが必要になる場合があります。  

## <a name="type-conversion-concepts"></a>型変換の概念  
  
### <a name="xaml-and-string-values"></a>XAML と文字列値  
 XAML ファイルで属性値を設定すると、その値の最初の型は純粋テキストの文字列になります。 <xref:System.Double> などの他のプリミティブも、最初に XAML プロセッサのテキスト文字列です。  
  
 XAML プロセッサは、属性値を処理するために2つの情報を必要とします。 第 1 の情報は、設定しようとしているプロパティの値の型です。 属性値を定義するすべての文字列は、XAML で処理され、最終的にはその型の値に変換 (解決) される必要があります。 値が、XAML パーサーで認識できるプリミティブ (数値など) である場合は、文字列の直接的な変換が試みられます。 値が列挙型の場合、文字列は、その列挙体の名前付き定数に一致する名前があるかどうかを確認するために使用されます。 値がパーサーで認識されるプリミティブでも列挙でもない場合、問題の型は、変換後の文字列に基づいて、型のインスタンスまたは値を提供できる必要があります。 これを行うには、型コンバータークラスを指定します。 型コンバーターは、実際には、XAML シナリオに対しても、.NET コードのコード呼び出しの場合でも、別のクラスの値を提供するためのヘルパークラスです。  
  
### <a name="using-existing-type-conversion-behavior-in-xaml"></a>XAML での既存の型変換動作の使用  
 基になる XAML の概念に関する知識によっては、基本的なアプリケーション XAML で型変換の動作を使用していることがわかっていない場合があります。 たとえば、WPF では、<xref:System.Windows.Point>型の値を受け取る文字どおり数百のプロパティが定義されています。 <xref:System.Windows.Point> は、2次元の座標空間内の座標を表す値であり、実際には <xref:System.Windows.Point.X%2A> と <xref:System.Windows.Point.Y%2A>の2つの重要なプロパティがあります。 XAML でポイントを指定する場合は、指定した <xref:System.Windows.Point.X%2A> と <xref:System.Windows.Point.Y%2A> の値の間に区切り記号 (通常はコンマ) を含む文字列として指定します。 (例: `<LinearGradientBrush StartPoint="0,0" EndPoint="1,1"/>`)。  
  
 この単純型の <xref:System.Windows.Point> と XAML での単純な使用についても、型コンバーターが必要です。 この場合は、<xref:System.Windows.PointConverter>クラスです。  
  
 クラスレベルで定義されている <xref:System.Windows.Point> の型コンバーターは、<xref:System.Windows.Point>を受け取るすべてのプロパティのマークアップの使用を効率化します。 ここでは型コンバーターを使用しない場合、前に示したのと同じ例について、より詳細なマークアップが必要になります。  

```xaml
<LinearGradientBrush>
  <LinearGradientBrush.StartPoint>
    <Point X="0" Y="0"/>
  </LinearGradientBrush.StartPoint>
  <LinearGradientBrush.EndPoint>
    <Point X="1" Y="1"/>
  </LinearGradientBrush.EndPoint>
</LinearGradientBrush>
 ```
  
 型変換文字列を使用するか、より詳細な同等の構文を使用するかは、通常、コーディングスタイルとして選択します。 XAML ツールワークフローは、値の設定方法にも影響を与える可能性があります。 XAML ツールによっては、デザイナービューや独自のシリアル化機構にラウンドトリップしやすくなるため、最も詳細なマークアップ形式が生成される傾向があります。  
  
 既存の型コンバーターは、通常、クラス (またはプロパティ) で適用された <xref:System.ComponentModel.TypeConverterAttribute>が存在するかどうかをチェックすることによって、WPF および .NET Framework 型で検出できます。 この属性は、XAML の目的だけでなく、他の目的でも、その型の値に対してサポートされている型コンバーターであるクラスに名前を付けます。  
  
### <a name="type-converters-and-markup-extensions"></a>型コンバーターとマークアップ拡張機能  
 マークアップ拡張機能と型コンバーターは、XAML プロセッサの動作と、それらが適用されるシナリオに関して直交ロールを設定します。 マークアップ拡張機能の使用時にはコンテキストを利用できますが、マークアップ拡張機能が値を提供するプロパティの型変換動作は一般にマークアップ拡張機能の実装ではチェックされません。 つまり、マークアップ拡張機能が `ProvideValue` 出力としてテキスト文字列を返す場合でも、特定のプロパティまたはプロパティ値型に適用される、その文字列に対する型変換動作は呼び出されません。一般に、マークアップ拡張機能の目的は、文字列を処理し、型コンバーターを含まないオブジェクトを返すことです。  
  
 型コンバーターではなく、マークアップ拡張機能が必要となる一般的な状況の1つは、既に存在するオブジェクトへの参照を作成することです。 最良の場合、ステートレスな型コンバーターは新しいインスタンスを生成するだけで、望ましくない可能性があります。 マークアップ拡張機能の詳細については、「[マークアップ拡張機能」および「WPF XAML](markup-extensions-and-wpf-xaml.md)」を参照してください。  
  
### <a name="native-type-converters"></a>ネイティブな型コンバーター  
 XAML パーサーの WPF および .NET Framework 実装では、ネイティブ型の変換処理を持つ特定の型がありますが、プリミティブとして考えられる型ではありません。 このような型の例として、 <xref:System.DateTime>が挙げられます。 この理由は、.NET Framework アーキテクチャの動作に基づいています。型 <xref:System.DateTime> は、.NET の最も基本的なライブラリである mscorlib で定義されています。 <xref:System.DateTime> は、依存関係が導入された別のアセンブリからの属性 (システムからの<xref:System.ComponentModel.TypeConverterAttribute> である) を使用して属性を設定することは許可されていないため、属性による通常の型コンバーターの検出メカニズムはサポートされません。 代わりに、XAML パーサーには、このようなネイティブ処理を必要とする型のリストがあり、これらは真のプリミティブの処理方法と同様に処理されます。 (<xref:System.DateTime> 場合は、<xref:System.DateTime.Parse%2A>を呼び出す必要があります)。  
  
<a name="Implementing_a_Type_Converter"></a>   
## <a name="implementing-a-type-converter"></a>型コンバーターの実装  
  
### <a name="typeconverter"></a>TypeConverter  
 前に示した <xref:System.Windows.Point> の例では、クラス <xref:System.Windows.PointConverter> について説明しました。 XAML の .NET 実装では、XAML の目的で使用されるすべての型コンバーターは、<xref:System.ComponentModel.TypeConverter>基底クラスから派生するクラスです。 <xref:System.ComponentModel.TypeConverter> クラスは、XAML の存在前に .NET Framework のバージョンに存在しています。最初の使用方法の1つは、ビジュアルデザイナーのプロパティダイアログに文字列変換を提供することでした。 XAML の場合、<xref:System.ComponentModel.TypeConverter> のロールが拡張され、文字列属性値の解析を有効にし、場合によっては特定のオブジェクトプロパティの実行時の値を属性としてシリアル化するために文字列に変換することができます。  
  
 <xref:System.ComponentModel.TypeConverter> は、XAML 処理のために文字列との間の変換に関連する4つのメンバーを定義します。  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>  
  
- <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>  
  
 これらのうち、最も重要な方法は <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A>です。 このメソッドは、入力文字列を必要なオブジェクト型に変換します。 厳密に言えば、<xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> メソッドを実装して、より広範な型をコンバーターの目的の型に変換することができます。これにより、実行時の変換をサポートするなど、XAML を超えて拡張できるようになりますが、XAML の目的では、重要な <xref:System.String> 入力を処理できるコードパスにすぎません。  
  
 次に重要なメソッドは <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>です。 アプリケーションがマークアップ表現に変換された場合 (たとえば、ファイルとして XAML に保存されている場合)、<xref:System.ComponentModel.TypeConverter.ConvertTo%2A> はマークアップ表現の生成を行います。 この場合、XAML にとって重要なコードパスは、<xref:System.String> の `destinationType` を渡すときに使用されます。  
  
 <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> と <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> は、サービスが <xref:System.ComponentModel.TypeConverter> の実装の機能を照会する時に使用されるサポート メソッドです。 これらのメソッドは、その型について、相当する変換メソッドをコンバーターがサポートしている場合に `true` を返すように実装する必要があります。 XAML の目的では、通常、 <xref:System.String> 型であることを意味します。  
  
### <a name="culture-information-and-type-converters-for-xaml"></a>カルチャ情報と XAML の型コンバーター  

 各 <xref:System.ComponentModel.TypeConverter> 実装では、変換の有効な文字列を構成する内容を独自に解釈できます。また、パラメーターとして渡される型の説明を使用したり無視したりすることもできます。 カルチャと XAML の型変換に関しては、重要な考慮事項があります。 属性値としてローカライズ可能な文字列を使用することは、XAML で完全にサポートされています。 ただし、このローカライズ可能な文字列を特定のカルチャ要件での型コンバーター入力として使用することはサポートされていません。これは、XAML 属性値の型コンバーターは、`en-US` カルチャを使用して、必ずしも固定言語の解析動作を行うためです。 この制限の設計上の理由の詳細については、「XAML 言語仕様 ([\[\]](https://download.microsoft.com/download/0/A/6/0A6F7755-9AF5-448B-907D-13985ACCF53E/[MS-XAML].pdf)」を参照してください。  
  
 カルチャが問題になる可能性のある例として、一部のカルチャでは、数値の小数点の区切り記号としてコンマを使用しています。 これは、WPF XAML 型コンバーターの多くが持つ動作と競合します。これは、コンマを区切り記号として使用します。これは、共通の X、Y 形式、またはコンマ区切りの一覧などの履歴の参照元に基づいています。 周囲の XAML でカルチャを渡すこともできます (`Language` または `xml:lang` を `sl-SI` カルチャに設定します。この方法では、10進数にコンマを使用するカルチャの例) では問題は解決されません。  
  
### <a name="implementing-convertfrom"></a>ConvertFrom の実装  
 XAML をサポートする <xref:System.ComponentModel.TypeConverter> の実装としてコンバーターを使用できるようにするためには、そのコンバーターの <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> メソッドが `value` パラメーターとして文字列を受け入れる必要があります。 文字列が有効な形式であり、<xref:System.ComponentModel.TypeConverter> の実装で変換できる場合、返されるオブジェクトは、プロパティで想定される型へのキャストをサポートする必要があります。 それ以外の場合、 <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> 実装は `null`を返す必要があります。  
  
 各 <xref:System.ComponentModel.TypeConverter> 実装では、変換のために有効な文字列を構成する内容を独自に解釈できます。また、パラメーターとして渡される型の説明またはカルチャコンテキストを使用したり無視したりすることもできます。 ただし、WPF XAML 処理では、常に型の説明のコンテキストに値を渡さないことがあります。また、`xml:lang`に基づいてカルチャを渡さない場合もあります。  
  
> [!NOTE]
> 中かっこ文字 (特に {) は、文字列形式の可能な要素として使用しないでください。 これらの文字は、マークアップ拡張シーケンスの開始および終了を示す文字として予約されています。  
  
### <a name="implementing-convertto"></a>ConvertTo の実装  
 <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> は、シリアル化のサポートで使用される可能性があります。 カスタム型およびその型コンバーターに対して <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> によるシリアル化をサポートすることは、絶対要件ではありません。 ただし、コントロールを実装する場合、またはクラスの機能または設計の一部としてシリアル化を使用する場合は、 <xref:System.ComponentModel.TypeConverter.ConvertTo%2A>を実装する必要があります。  
  
 XAML をサポートする <xref:System.ComponentModel.TypeConverter> の実装として使用できるようにするには、そのコンバーターの <xref:System.ComponentModel.TypeConverter.ConvertTo%2A> メソッドが、`value` パラメーターとしてサポートされている型のインスタンス (または値) を受け入れる必要があります。 `destinationType` パラメーターが <xref:System.String>型の場合、返されるオブジェクトは <xref:System.String>としてキャストできる必要があります。 返される文字列は、 `value`のシリアル化された値を表している必要があります。 理想的には、選択したシリアル化形式で、同じコンバーターの <xref:System.ComponentModel.TypeConverter.ConvertFrom%2A> の実装にその文字列が渡された場合に、情報が大幅に失われることなく、同じ値を生成できるようにする必要があります。  
  
 値をシリアル化できない場合、またはコンバーターがシリアル化をサポートしていない場合、<xref:System.ComponentModel.TypeConverter.ConvertTo%2A> の実装は `null`を返す必要があり、この場合は例外をスローすることが許可されます。 ただし、例外をスローする場合は、<xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> 実装の一部としてその変換を使用できないことを報告して、例外を回避するために <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> 最初に確認することをお勧めします。  
  
 `destinationType` パラメーターの型が <xref:System.String>でない場合は、独自のコンバーター処理を選択できます。 通常は、基本実装の処理に戻します。これは、basemost によって特定の例外が発生します。<xref:System.ComponentModel.TypeConverter.ConvertTo%2A>  
  
### <a name="implementing-canconvertto"></a>CanConvertTo の実装  
 <xref:System.ComponentModel.TypeConverter.CanConvertTo%2A> の実装は、 `true` が `destinationType` 型の場合は <xref:System.String>を返し、それ以外の場合は基底の実装に任せる必要があります。  
  
### <a name="implementing-canconvertfrom"></a>CanConvertFrom の実装  
 <xref:System.ComponentModel.TypeConverter.CanConvertFrom%2A> の実装は、 `true` が `sourceType` 型の場合は <xref:System.String>を返し、それ以外の場合は基底の実装に任せる必要があります。  
  
<a name="Applying_the_TypeConverterAttribute"></a>   
## <a name="applying-the-typeconverterattribute"></a>TypeConverterAttribute の適用  
 カスタム型コンバーターを XAML プロセッサによってカスタムクラスの動作する型コンバーターとして使用するには、<xref:System.ComponentModel.TypeConverterAttribute> をクラス定義に適用する必要があります。 属性を通して指定する <xref:System.ComponentModel.TypeConverterAttribute.ConverterTypeName%2A> は、カスタム型コンバーターの型名である必要があります。 この属性を適用すると、プロパティ型がカスタムクラス型を使用する値を XAML プロセッサが処理するときに、入力文字列を入力して、オブジェクトインスタンスを返すことができます。  
  
 また、プロパティごとに型コンバーターを提供することもできます。 クラス定義に <xref:System.ComponentModel.TypeConverterAttribute> を適用する代わりに、プロパティ定義に適用します (`get`/`set` の実装ではなく、メイン定義)。 プロパティの型は、カスタム型コンバーターによって処理される型と一致する必要があります。 この属性を適用すると、プロパティの値を XAML プロセッサが処理する際に、入力文字列を処理して、オブジェクトのインスタンスを返すことができます。 プロパティごとの型コンバーターの手法は、Microsoft .NET Framework から、またはクラス定義を制御できない他のライブラリからプロパティ型を使用する場合に特に便利であり、そこに <xref:System.ComponentModel.TypeConverterAttribute> を適用することはできません。  
  
## <a name="see-also"></a>参照

- <xref:System.ComponentModel.TypeConverter>
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
- [マークアップ拡張機能と WPF XAML](markup-extensions-and-wpf-xaml.md)
- [XAML 構文の詳細](xaml-syntax-in-detail.md)
