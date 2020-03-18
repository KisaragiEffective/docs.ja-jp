---
title: クラス、構造体、および共用体のマーシャリング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data marshaling, classes
- marshaling, unions
- marshaling, structures
- marshaling, samples
- data marshaling, structures
- platform invoke, marshaling data
- marshaling, classes
- data marshaling, unions
- data marshaling, samples
- data marshaling, platform invoke
- marshaling, platform invoke
ms.assetid: 027832a2-9b43-4fd9-9b45-7f4196261a4e
ms.openlocfilehash: d761d8ed7488e99f29d4844d061867915a624b96
ms.sourcegitcommit: 42ed59871db1f29a32b3d8e7abeb20e6eceeda7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74960005"
---
# <a name="marshaling-classes-structures-and-unions"></a>クラス、構造体、および共用体のマーシャリング

クラスと構造体は、.NET Framework では類似しています。 どちらもフィールド、プロパティ、およびイベントを持つことができます。 静的メソッドと非静的メソッドを持つこともできます。 1 つの重要な違いは、構造体は値型でクラスは参照型であることです。

次の表は、クラス、構造体、および共用体のマーシャリング オプションをリストし、それぞれの使用方法を説明し、対応するプラットフォーム呼び出しサンプルへのリンクを示しています。

|の型|説明|［サンプル］|
|----------|-----------------|------------|
|値によるクラス。|整数のメンバーを含むクラスは、管理対象クラスと同じように、In/Out パラメーターとして渡します。|[SysTime サンプル](#systime-sample)|
|値による構造体。|In パラメーターとして構造体を渡します。|[構造体のサンプル](#structures-sample)|
|参照による構造体|In/Out パラメーターとして構造体を渡します。|[OSInfo のサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/795sy883(v=vs.100))|
|入れ子になった構造体を含む構造体 (フラット化)。|アンマネージ関数で入れ子になった構造体を含む構造体を表すクラスを渡します。 マネージド プロトタイプで構造体は 1 つの大きな構造体にフラット化されます。|[FindFile サンプル](#findfile-sample)|
|別の構造体へのポインターを持つ構造体。|2 番目の構造体へのポインターをメンバーとして含む構造体を渡します。|[構造体のサンプル](#structures-sample)|
|値による整数のある構造体の配列。|In/Out パラメーターとして整数のみを含む構造体の配列を渡します。 配列のメンバーを変更することができます。|[配列のサンプル](marshaling-different-types-of-arrays.md)|
|参照による整数と文字列のある構造体の配列。|Out パラメーターとして整数と文字列を含む構造体の配列を渡します。 呼び出された関数は、配列のメモリを割り当てます。|[OutArrayOfStructs サンプル](#outarrayofstructs-sample)|
|値型の共用体。|値型 (整数および倍精度) の共用体を渡します。|[共用体のサンプル](#unions-sample)|
|混合型の共用体。|混合型 (整数および文字列) の共用体を渡します。|[共用体のサンプル](#unions-sample)|
|構造体の null 値。|値型への参照の代わりに null 参照 (Visual Basic では **Nothing**) を渡します。|[HandleRef サンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/hc662t8k(v=vs.85))|

## <a name="structures-sample"></a>構造体のサンプル

このサンプルでは、2 番目の構造体を指す構造体を渡す方法、埋め込み構造体のある構造体を渡す方法、および埋め込み配列のある構造体を渡す方法を示します。  
  
Structs のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。

- PinvokeLib.dll からエクスポートされる **TestStructInStruct**。

    ```cpp
    int TestStructInStruct(MYPERSON2* pPerson2);
    ```

- PinvokeLib.dll からエクスポートされる **TestStructInStruct3**。

    ```cpp
    void TestStructInStruct3(MYPERSON3 person3);
    ```

- PinvokeLib.dll からエクスポートされる **TestArrayInStruct**。

    ```cpp
    void TestArrayInStruct(MYARRAYSTRUCT* pStruct);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) はカスタム アンマネージ ライブラリであり、上記の関数および 4 つの構造体 **MYPERSON**、**MYPERSON2**、**MYPERSON3**、および **MYARRAYSTRUCT** に関する実装を含んでいます。 これらの構造体には次の要素が含まれます。

```cpp
typedef struct _MYPERSON
{
   char* first;
   char* last;
} MYPERSON, *LP_MYPERSON;

typedef struct _MYPERSON2
{
   MYPERSON* person;
   int age;
} MYPERSON2, *LP_MYPERSON2;

typedef struct _MYPERSON3
{
   MYPERSON person;
   int age;
} MYPERSON3;

typedef struct _MYARRAYSTRUCT
{
   bool flag;
   int vals[ 3 ];
} MYARRAYSTRUCT;
```

マネージド `MyPerson`、`MyPerson2`、`MyPerson3`、および `MyArrayStruct` 構造体には、以下の特性があります。

- `MyPerson` には文字列メンバーだけが含まれます。 [CharSet](specifying-a-character-set.md) フィールドは、アンマネージ関数に渡されるとき、文字列を ANSI 形式に設定します。

- `MyPerson2` には、`MyPerson` 構造体への **IntPtr** が含まれます。 コードが **unsafe** とマークされている場合を除いて、.NET Framework アプリケーションではポインターを使用しないため、**IntPtr** 型は元のポインターをアンマネージ構造体に置き換えます。

- `MyPerson3` には `MyPerson` が埋め込み構造体として含まれます。 別の構造体に埋め込まれた構造体は、埋め込み構造体の要素をメインの構造体に直接配置することでフラット化することができます。またはこのサンプルのように、埋め込み構造体のままにすることもできます。

- `MyArrayStruct` には整数の配列が含まれます。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は <xref:System.Runtime.InteropServices.UnmanagedType> 列挙値を **ByValArray** に設定します。これは配列内の要素の数を示すために使用されます。

このサンプル内のすべての構造体で、各メンバーが出現する順番でメモリ内に順次配列されることを保証するために、<xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性が適用されています。

`NativeMethods` クラスには、`App` クラスによって呼び出される `TestStructInStruct`、`TestStructInStruct3`、および `TestArrayInStruct` メソッドのマネージド プロトタイプが含まれます。 各プロトタイプは、1 つのパラメーターを以下のように宣言します。

- `TestStructInStruct` は型 `MyPerson2` への参照をそのパラメーターとして宣言します。

- `TestStructInStruct3` は型 `MyPerson3` をそのパラメーターとして宣言し、パラメーターを値によって渡します。

- `TestArrayInStruct` は型 `MyArrayStruct` への参照をそのパラメーターとして宣言します。

メソッドへの引数としての構造体は、パラメーターに **ref** (Visual Basic では **ByRef**) キーワードが含まれない限り、値によって渡されます。 たとえば、`TestStructInStruct` メソッドは型 `MyPerson2` のオブジェクトへの参照 (アドレスの値) をアンマネージ コードに渡します。 `MyPerson2` が指定する構造体を操作するために、サンプルは指定したサイズのバッファーを作成し、<xref:System.Runtime.InteropServices.Marshal.AllocCoTaskMem%2A?displayProperty=nameWithType> と <xref:System.Runtime.InteropServices.Marshal.SizeOf%2A?displayProperty=nameWithType> のメソッドを結合することでそのアドレスを返します。 次に、サンプルはマネージド構造体の内容をアンマネージド バッファーにコピーします。 最後に、サンプルは <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A?displayProperty=nameWithType> メソッドを使用してアンマネージド バッファーからマネージド オブジェクトにデータをマーシャリングし、<xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A?displayProperty=nameWithType> メソッドを使用してメモリのアンマネージド ブロックを解放します。

### <a name="declaring-prototypes"></a>プロトタイプの宣言

[!code-cpp[Conceptual.Interop.Marshaling#23](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#23)]
[!code-csharp[Conceptual.Interop.Marshaling#23](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#23)]
[!code-vb[Conceptual.Interop.Marshaling#23](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#23)]

### <a name="calling-functions"></a>関数の呼び出し

[!code-cpp[Conceptual.Interop.Marshaling#24](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#24)]
[!code-csharp[Conceptual.Interop.Marshaling#24](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#24)]
[!code-vb[Conceptual.Interop.Marshaling#24](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#24)]

## <a name="findfile-sample"></a>FindFile サンプル

このサンプルでは、2 番目の埋め込み構造体を含む構造体をアンマネージ関数に渡す方法を示します。 また、<xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性を使用して構造体内に固定長配列を宣言する方法も示します。 このサンプルでは、埋め込み構造体の要素が親の構造体に追加されます。 フラット化しない埋め込み構造体のサンプルについては、「[構造体のサンプル](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/eadtsekz(v=vs.100))」を参照してください。

FindFile のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。

- Kernel32.dll からエクスポートされる **FindFirstFile**。

    ```cpp
    HANDLE FindFirstFile(LPCTSTR lpFileName, LPWIN32_FIND_DATA lpFindFileData);
    ```

関数に渡された元の構造体には、次に示す要素が含まれています。

```cpp
typedef struct _WIN32_FIND_DATA
{
  DWORD    dwFileAttributes;
  FILETIME ftCreationTime;
  FILETIME ftLastAccessTime;
  FILETIME ftLastWriteTime;
  DWORD    nFileSizeHigh;
  DWORD    nFileSizeLow;
  DWORD    dwReserved0;
  DWORD    dwReserved1;
  TCHAR    cFileName[ MAX_PATH ];
  TCHAR    cAlternateFileName[ 14 ];
} WIN32_FIND_DATA, *PWIN32_FIND_DATA;
```

このサンプルでは、`FindData` クラスに、元の構造体と埋め込み構造体の各要素に対応するデータ メンバーが含まれています。 このクラスは、元の 2 つの文字バッファーを文字列に置き換えます。 **MarshalAsAttribute** は <xref:System.Runtime.InteropServices.UnmanagedType> 列挙を **ByValTStr** に設定します。これは、アンマネージ構造体に出現するインラインの固定長文字配列を識別するために使用されます。

`NativeMethods` クラスには `FindFirstFile` メソッドのマネージド プロトタイプが含まれます。このメソッドは `FindData` クラスをパラメーターとして渡します。 参照型のクラスは既定では In パラメーターとして渡されるため、パラメーターは <xref:System.Runtime.InteropServices.InAttribute> と <xref:System.Runtime.InteropServices.OutAttribute> の属性で宣言する必要があります。

### <a name="declaring-prototypes"></a>プロトタイプの宣言

[!code-cpp[Conceptual.Interop.Marshaling#17](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#17)]
[!code-csharp[Conceptual.Interop.Marshaling#17](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#17)]
[!code-vb[Conceptual.Interop.Marshaling#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#17)]

### <a name="calling-functions"></a>関数の呼び出し

[!code-cpp[Conceptual.Interop.Marshaling#18](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#18)]
[!code-csharp[Conceptual.Interop.Marshaling#18](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#18)]
 [!code-vb[Conceptual.Interop.Marshaling#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#18)]

## <a name="unions-sample"></a>Unions サンプル

このサンプルでは、値型のみを含む構造体、および値型と文字列を含む構造体を、共用体が予期されているアンマネージ関数のパラメーターとして渡す方法を示します。 共用体は、2 つ以上の変数が共有できるメモリ位置を表します。

Unions のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。

- PinvokeLib.dll からエクスポートされる **TestUnion**。

    ```cpp
    void TestUnion(MYUNION u, int type);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) はカスタム アンマネージ ライブラリであり、上記の関数および 2 つの共用体 **MYUNION** および **MYUNION2** に関する実装を含んでいます。 共用体には以下の要素が含まれます。

```cpp
union MYUNION
{
    int number;
    double d;
}

union MYUNION2
{
    int i;
    char str[128];
};
```

マネージド コードでは、共用体は構造体として定義されます。 `MyUnion` 構造体には、メンバーとして整数と倍精度の 2 つの値型が含まれます。 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性は、各データ メンバーの正確な位置を制御するために設定されます。 <xref:System.Runtime.InteropServices.FieldOffsetAttribute> 属性は、共用体のアンマネージ表現内に、フィールドの物理的な位置を指定します。 両方のメンバーに同じオフセット値があるので、メンバーはメモリの同じ部分を定義できることに注意してください。

`MyUnion2_1` と `MyUnion2_2` には、それぞれ値型 (整数) と文字列が含まれています。 マネージド コードでは、値型と参照型が重複することは許可されません。 このサンプルでは、同じアンマネージ関数を呼び出すときに呼び出し元が両方の型を使用できるようにするため、メソッドのオーバーロードを使用します。 `MyUnion2_1` のレイアウトは明示的で、正確なオフセット値を持っています。 これに対し、`MyUnion2_2` にはシーケンシャル レイアウトがあります。参照型では明示的なレイアウトが許可されていないためです。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性は <xref:System.Runtime.InteropServices.UnmanagedType> 列挙を **ByValTStr** に設定します。これは、共用体のアンマネージ表現に出現するインラインの固定長文字配列を識別するために使用されます。

`NativeMethods` クラスには、`TestUnion` と `TestUnion2` メソッドのプロトタイプが含まれます。 `TestUnion2` は `MyUnion2_1` または `MyUnion2_2` をパラメーターとして宣言するためにオーバーロードされています。

### <a name="declaring-prototypes"></a>プロトタイプの宣言

[!code-cpp[Conceptual.Interop.Marshaling#28](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#28)]
[!code-csharp[Conceptual.Interop.Marshaling#28](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#28)]
[!code-vb[Conceptual.Interop.Marshaling#28](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#28)]

### <a name="calling-functions"></a>関数の呼び出し

[!code-cpp[Conceptual.Interop.Marshaling#29](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#29)]
[!code-csharp[Conceptual.Interop.Marshaling#29](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#29)]
[!code-vb[Conceptual.Interop.Marshaling#29](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#29)]

## <a name="systime-sample"></a>SysTime サンプル

このサンプルでは、構造体へのポインターを要求する、クラスへのポインターをアンマネージ関数に渡す方法について説明します。

SysTime のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。

- Kernel32.dll からエクスポートされる **GetSystemTime**。

    ```cpp
    VOID GetSystemTime(LPSYSTEMTIME lpSystemTime);
    ```

関数に渡された元の構造体には、次に示す要素が含まれています。

```cpp
typedef struct _SYSTEMTIME {
    WORD wYear;
    WORD wMonth;
    WORD wDayOfWeek;
    WORD wDay;
    WORD wHour;
    WORD wMinute;
    WORD wSecond;
    WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME;
```

このサンプルでは、`SystemTime` クラスの中には、クラス メンバーとして表される、元の構造体の要素が含まれます。 各メンバーが出現する順番でメモリ内に順次配列されることを保証するために、 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を設定します。

`NativeMethods` クラスには `GetSystemTime` メソッドのマネージド プロトタイプが含まれます。このメソッドは既定では `SystemTime` クラスを In/Out パラメーターとして渡します。 参照型のクラスは既定では In パラメーターとして渡されるため、パラメーターは <xref:System.Runtime.InteropServices.InAttribute> と <xref:System.Runtime.InteropServices.OutAttribute> の属性で宣言する必要があります。 呼び出し元が結果を受け取るには、これらの[方向属性](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))を明示的に適用する必要があります。 `App` クラスは、`SystemTime` クラスの新しいインスタンスを作成して、そのデータ フィールドにアクセスします。

### <a name="code-samples"></a>コード サンプル

[!code-cpp[Conceptual.Interop.Marshaling#25](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/systime.cpp#25)]
[!code-csharp[Conceptual.Interop.Marshaling#25](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/systime.cs#25)]
[!code-vb[Conceptual.Interop.Marshaling#25](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/systime.vb#25)]

## <a name="outarrayofstructs-sample"></a>OutArrayOfStructs サンプル

このサンプルでは、整数および文字列をアンマネージ関数への Out パラメーターとして含む構造体の配列を渡す方法を示します。

このサンプルでは、<xref:System.Runtime.InteropServices.Marshal> クラスを使用することにより、およびアンセーフ コードを使用することにより、ネイティブ関数を呼び出す方法を例示します。

このサンプルでは、[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) で定義されていて、ソース ファイルにも含まれている、ラッパー関数とプラットフォーム呼び出しを使用します。 これは `TestOutArrayOfStructs` 関数および `MYSTRSTRUCT2` 構造を使用します。 構造体には次の要素が含まれます。

```cpp
typedef struct _MYSTRSTRUCT2
{
   char* buffer;
   UINT size;
} MYSTRSTRUCT2;
```

`MyStruct` クラスには ANSI 文字の文字列オブジェクトが含まれています。 <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> フィールドは ANSI 形式を指定します。 `MyUnsafeStruct` は、文字列の代わりに <xref:System.IntPtr> 型を含む構造体です。

`NativeMethods` クラスには、オーバーロードされた `TestOutArrayOfStructs` プロトタイプ メソッドが含まれます。 メソッドでポインターをパラメーターとして宣言している場合、クラスには `unsafe` キーワードでマークを付ける必要があります。 Visual Basic ではアンセーフ コードを使用できないので、オーバーロードされたメソッド、unsafe 修飾子、および `MyUnsafeStruct` 構造体は不要です。

`App` クラスは、配列を渡すために必要なすべてのタスクを実行する、`UsingMarshaling` メソッドを実装します。 配列には、データが呼び出し先から呼び出し元に渡されることを示すため、`out` (Visual Basic では `ByRef`) キーワードでマークが付けられます。 実装は、以下の <xref:System.Runtime.InteropServices.Marshal> クラス メソッドを使用します。

- <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A> は、アンマネージド バッファーからマネージド オブジェクトにデータをマーシャリングします。

- <xref:System.Runtime.InteropServices.Marshal.DestroyStructure%2A> は、構造体で文字列用に予約されていたメモリを解放します。

- <xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A> は、配列用に予約されていたメモリを解放します。

前述のとおり、C# にはアンセーフ コードを使用できますが、Visual Basic には使用できません。 C# サンプルで、`UsingUnsafePointer` は、<xref:System.Runtime.InteropServices.Marshal> クラスの代わりにポインターを使用して `MyUnsafeStruct` 構造体を含む配列を戻す、代替のメソッドの実装です。

### <a name="declaring-prototypes"></a>プロトタイプの宣言

[!code-cpp[Conceptual.Interop.Marshaling#20](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#20)]
[!code-csharp[Conceptual.Interop.Marshaling#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#20)]
[!code-vb[Conceptual.Interop.Marshaling#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#20)]

### <a name="calling-functions"></a>関数の呼び出し

[!code-cpp[Conceptual.Interop.Marshaling#21](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#21)]
[!code-csharp[Conceptual.Interop.Marshaling#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#21)]
[!code-vb[Conceptual.Interop.Marshaling#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#21)]

## <a name="see-also"></a>参照

- [プラットフォーム呼び出しによるデータのマーシャリング](marshaling-data-with-platform-invoke.md)
- [マーシャリング (文字列の)](marshaling-strings.md)
- [さまざまな型の配列のマーシャリング](marshaling-different-types-of-arrays.md)
