---
title: 選択的単体テストの実行
description: .NET Core において dotnet test コマンドでフィルター式を使用して、選択的単体テストを実行する方法。
author: smadala
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.topic: reference
ms.openlocfilehash: 7f072952c6200e10602ec476b5d075819044fc5a
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740944"
---
# <a name="run-selective-unit-tests"></a>選択的単体テストの実行

.NET Core で [`dotnet test`](../tools/dotnet-test.md) コマンドを使用することで、フィルター式を使用して、選択的テストを実行することができます。 この記事では、実行するテストをフィルター処理する方法を示します。 次の例では、`dotnet test` を使用します。 `vstest.console.exe` を使用している場合は、`--filter` を `--testcasefilter:` に置き換えます。

## <a name="character-escaping"></a>文字のエスケープ

`*nix` で感嘆符 `!` を含むフィルターを使用するときは、`!` が予約されているため、エスケープが必要になります。 たとえば、次のように名前空間に IntegrationTests が含まれている場合、このフィルターではすべてのテストがスキップされます。

```dotnetcli
dotnet test --filter FullyQualifiedName\!~IntegrationTests
```

> [!IMPORTANT]
> 感嘆符の前に円記号を付けると (`\!`)、それがエスケープされた文字であることを示します。

ジェネリック型パラメーターで `FullyQualifiedName` 値にコンマを含める場合は、`%2C` を指定してコンマをエスケープします。 次に例を示します。

```dotnetcli
dotnet test --filter "FullyQualifiedName=MyNamespace.MyTestsClass<ParameterType1%2CParameterType2>.MyTestMethod"
```

:::zone pivot="mstest"

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace MSTestNamespace
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod, Priority(1), TestCategory("CategoryA")]
        public void TestMethod1()
        {
        }

        [TestMethod, Priority(2)]
        public void TestMethod2()
        {
        }
    }
}
```

| 正規表現 | 結果 |
|--|--|
| `dotnet test --filter Method` | <xref:System.Reflection.Module.FullyQualifiedName> に `Method` が含まれるテストを実行します。 `vstest 15.1+` で使用できます。 |
| `dotnet test --filter Name~TestMethod1` | 名前に `TestMethod1` が含まれるテストを実行します。 |
| `dotnet test --filter ClassName=MSTestNamespace.UnitTest1` | クラス `MSTestNamespace.UnitTest1` 内にあるテストを実行します。<br>**注:** `ClassName` 値には名前空間があるため、`ClassName=UnitTest1` は機能しません。 |
| `dotnet test --filter FullyQualifiedName!=MSTestNamespace.UnitTest1.TestMethod1` | `MSTestNamespace.UnitTest1.TestMethod1` 以外のテストをすべて実行します。 |
| `dotnet test --filter TestCategory=CategoryA` | `[TestCategory("CategoryA")]` の注釈が付けられているテストを実行します。 |
| `dotnet test --filter Priority=2` | `[Priority(2)]` の注釈が付けられているテストを実行します。 |

条件演算子 `|` と `&` の使用例:

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれるか、**または**、<xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1|TestCategory=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれ、**かつ**、<xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1&TestCategory=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれ、**かつ**、<xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> が `"CategoryA"` であるか、**または**、<xref:Microsoft.VisualStudio.TestTools.UnitTesting.PriorityAttribute> の優先順位が `1` であるテストを実行するには。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~UnitTest1&TestCategory=CategoryA)|Priority=1"
```

:::zone-end
:::zone pivot="xunit"

```csharp
using Xunit;

namespace XUnitNamespace
{
    public class TestClass1
    {
        [Fact, Trait("Priority", "1"), Trait("Category", "CategoryA")]
        public void Test1()
        {
        }

        [Fact, Trait("Priority", "2")]
        public void Test2()
        {
        }
    }
}
```

| 正規表現 | 結果 |
|--|--|
| `dotnet test --filter DisplayName=XUnitNamespace.TestClass1.Test1` | `XUnitNamespace.TestClass1.Test1` という 1 つのテストのみを実行します。 |
| `dotnet test --filter FullyQualifiedName!=XUnitNamespace.TestClass1.Test1` | `XUnitNamespace.TestClass1.Test1` 以外のテストをすべて実行します。 |
| `dotnet test --filter DisplayName~TestClass1` | 表示名に `TestClass1` が含まれるテストを実行します。 |

コード例では、特性をキー `"Category"` や `"Priority"` で定義すると、フィルター処理に使用できます。

| 正規表現 | 結果 |
|--|--|
| `dotnet test --filter XUnit` | <xref:System.Reflection.Module.FullyQualifiedName> に `XUnit` が含まれるテストを実行します。  `vstest 15.1+` で使用できます。 |
| `dotnet test --filter Category=CategoryA` | `[Trait("Category", "CategoryA")]` があるテストを実行します。 |

条件演算子 `|` と `&` の使用例:

<xref:System.Reflection.Module.FullyQualifiedName> に `TestClass1` が含まれるか、**または**、`Trait` のキーが `"Category"` で値が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~TestClass1|Category=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `TestClass1` が含まれ、**かつ**、`Trait` のキーが `"Category"` で値が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~TestClass1&Category=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `TestClass1` が含まれ、**かつ**、`Trait` のキーが `"Category"` で値が `"CategoryA"` であるか、**または**、`Trait` のキーが `"Priority"` で値が `1` であるテストを実行するには。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~TestClass1&Category=CategoryA)|Priority=1"
```

:::zone-end
:::zone pivot="nunit"

```csharp
using NUnit.Framework;

namespace NUnitNamespace
{
    public class UnitTest1
    {
        [Test, Property("Priority", 1), Category("CategoryA")]
        public void TestMethod1()
        {
        }

        [Test, Property("Priority", 2)]
        public void TestMethod2()
        {
        }
    }
}
```

| 正規表現 | 結果 |
|--|--|
| `dotnet test --filter Method` | <xref:System.Reflection.Module.FullyQualifiedName> に `Method` が含まれるテストを実行します。 `vstest 15.1+` で使用できます。 |
| `dotnet test --filter Name~TestMethod1` | 名前に `TestMethod1` が含まれるテストを実行します。 |
| `dotnet test --filter FullyQualifiedName~NUnitNamespace.UnitTest1` | クラス `NUnitNamespace.UnitTest1` 内にあるテストを実行します。 |
| `dotnet test --filter FullyQualifiedName!=NUnitNamespace.UnitTest1.TestMethod1` | `NUnitNamespace.UnitTest1.TestMethod1` 以外のテストをすべて実行します。 |
| `dotnet test --filter TestCategory=CategoryA` | `[Category("CategoryA")]` の注釈が付けられているテストを実行します。 |
| `dotnet test --filter Priority=2` | `[Priority(2)]` の注釈が付けられているテストを実行します。 |

条件演算子 `|` と `&` の使用例:

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれるか、**または**、`Category` が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1|TestCategory=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれ、**かつ**、`Category` が `"CategoryA"` であるテストを実行するには。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1&TestCategory=CategoryA"
```

<xref:System.Reflection.Module.FullyQualifiedName> に `UnitTest1` が含まれ、**かつ**、`Category` が `"CategoryA"` であるか、**または**、`Property` の `"Priority"` が `1` であるテストを実行するには。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~UnitTest1&TestCategory=CategoryA)|Priority=1"
```

詳細については、[TestCase フィルター](https://github.com/Microsoft/vstest-docs/blob/master/docs/filter.md)に関するページを参照してください。

:::zone-end

## <a name="see-also"></a>関連項目

- [dotnet test](../tools/dotnet-test.md)
- [dotnet test --filter](../tools/dotnet-test.md#filter-option-details)

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [単体テストの順序を設定する](order-unit-tests.md)
