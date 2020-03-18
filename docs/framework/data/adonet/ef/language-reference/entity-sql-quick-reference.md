---
title: Entity SQL クイック リファレンス
ms.date: 03/30/2017
ms.assetid: e53dad9e-5e83-426e-abb4-be3e78e3d6dc
ms.openlocfilehash: 9ccfc461d394af8804c960ebf460e7fbfb025b64
ms.sourcegitcommit: 8a0fe8a2227af612f8b8941bdb8b19d6268748e7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2019
ms.locfileid: "71833874"
---
# <a name="entity-sql-quick-reference"></a>Entity SQL クイック リファレンス
このトピックでは、[!INCLUDE[esql](../../../../../../includes/esql-md.md)] クエリのクイック リファレンスを提供します。 このトピックのクエリは、AdventureWorks Sales model に基づいています。  
  
## <a name="literals"></a>リテラル  
  
### <a name="string"></a>String  
 Unicode と非 Unicode の文字列リテラルがあります。 Unicode 文字列の先頭には N が付きます。たとえば、`N'hello'` です。  
  
 非 Unicode 文字列リテラルの例を次に示します。  
  
```sql  
'hello'  
--same as  
"hello"  
```  
  
 Output:  
  
|値|  
|-----------|  
|hello|  
  
### <a name="datetime"></a>DateTime  
 DateTime リテラルでは、日付部分と時刻部分の両方が必須です。 既定値はありません。  
  
 例:  
  
```sql  
DATETIME '2006-12-25 01:01:00.000'   
--same as  
DATETIME '2006-12-25 01:01'  
```  
  
 Output:  
  
|値|  
|-----------|  
|12/25/2006 1:01:00 AM|  
  
### <a name="integer"></a>整数型  
 整数リテラルには Int32 (123) 型、UInt32 (123U) 型、Int64 (123L) 型、および UInt64 (123UL) 型があります。  
  
 例:  
  
```sql  
--a collection of integers  
{1, 2, 3}  
```  
  
 Output:  
  
|値|  
|-----------|  
|1|  
|2|  
|3|  
  
### <a name="other"></a>その他  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] でサポートされているその他のリテラルは、Guid、Binary、Float/Double、Decimal、および `null` です。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] の NULL リテラルは、概念モデルのその他すべての型と互換性があると見なされます。  
  
## <a name="type-constructors"></a>型コンストラクター  
  
### <a name="row"></a>ROW  
 [行](row-entity-sql.md)は、次のように構造的に型指定された匿名の (レコード) 値を構築します。 `ROW(1 AS myNumber, ‘Name’ AS myName).`  
  
 例:  
  
```sql  
SELECT VALUE row (product.ProductID AS ProductID, product.Name
    AS ProductName) FROM AdventureWorksEntities.Product AS product
```  
  
 Output:  
  
|ProductID|名前|  
|---------------|----------|  
|1|Adjustable Race|  
|879|All-Purpose Bike Stand|  
|712|AWC Logo Cap|  
|[...]|[...]|  
  
### <a name="multiset"></a>MULTISET  
 [マルチセット](multiset-entity-sql.md)は、次のようなコレクションを構築します。  
  
 `MULTISET(1,2,2,3)` `--same as`-`{1,2,2,3}.`  
  
 例:  
  
```sql  
SELECT VALUE product FROM AdventureWorksEntities.Product AS product WHERE product.ListPrice IN MultiSet (125, 300)  
```  
  
 Output:  
  
|ProductID|名前|ProductNumber|…|  
|---------------|----------|-------------------|-------|  
|842|Touring-Panniers, Large|PA-T100|…|  
  
### <a name="object"></a>オブジェクト  
 [名前付きの型コンストラクター](named-type-constructor-entity-sql.md) (名前付き) ユーザー定義オブジェクト (`person("abc", 12)` など) を構築します。  
  
 例:  
  
```sql  
SELECT VALUE AdventureWorksModel.SalesOrderDetail (o.SalesOrderDetailID, o.CarrierTrackingNumber, o.OrderQty,   
o.ProductID, o.SpecialOfferID, o.UnitPrice, o.UnitPriceDiscount,   
o.rowguid, o.ModifiedDate) FROM AdventureWorksEntities.SalesOrderDetail   
AS o  
```  
  
 Output:  
  
|SalesOrderDetailID|CarrierTrackingNumber|OrderQty|ProductID|[...]|  
|------------------------|---------------------------|--------------|---------------|---------|  
|1|4911-403C-98|1|776|[...]|  
|2|4911-403C-98|3|777|[...]|  
|...|...|...|...|[...]|  
  
## <a name="references"></a>リファレンス  
  
### <a name="ref"></a>REF  
 [REF](ref-entity-sql.md)は、エンティティ型のインスタンスへの参照を作成します。 たとえば、次のクエリは、Orders エンティティ セットの各 Order エンティティへの参照を返します。  
  
```sql  
SELECT REF(o) AS OrderID FROM Orders AS o  
```  
  
 Output:  
  
|値|  
|-----------|  
|1|  
|2|  
|3|  
|[...]|  
  
 次の例では、プロパティ抽出演算子 (.) を使用して、エンティティのプロパティにアクセスします。 プロパティ抽出演算子を使用すると、参照は自動的に逆参照されます。  
  
 例:  
  
```sql  
SELECT VALUE REF(p).Name FROM   
    AdventureWorksEntities.Product AS p
```  
  
 Output:  
  
|値|  
|-----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|[...]|  
  
### <a name="deref"></a>DEREF  
 [DEREF](deref-entity-sql.md)は参照値を逆参照し、その逆参照の結果を生成します。 たとえば、次のクエリは、`SELECT DEREF(o2.r) FROM (SELECT REF(o) AS r FROM LOB.Orders AS o) AS o2` のように、Orders エンティティ セットの各 Order について Order エンティティを生成します。  
  
 例:  
  
```sql  
SELECT VALUE DEREF(REF(p)).Name FROM   
    AdventureWorksEntities.Product AS p
```  
  
 Output:  
  
|値|  
|-----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|[...]|  
  
### <a name="createref-and-key"></a>CREATEREF と KEY  
 [CREATEREF](createref-entity-sql.md)は、キーを渡す参照を作成します。 [Key](key-entity-sql.md)は、型参照を持つ式のキー部分を抽出します。  
  
 例:  
  
```sql  
SELECT VALUE Key(CreateRef(AdventureWorksEntities.Product, row(p.ProductID)))   
    FROM AdventureWorksEntities.Product AS p
```  
  
 Output:  
  
|ProductID|  
|---------------|  
|980|  
|365|  
|771|  
|[...]|  
  
## <a name="functions"></a>関数  
  
### <a name="canonical"></a>正規  
 [正規関数](canonical-functions.md)の名前空間は、`Edm.Length("string")` のように Edm です。 正規関数と同じ名前の関数を含んでいる別の名前空間がインポートされない限り、名前空間を指定する必要はありません。 2 つの名前空間に同じ関数が存在する場合は、完全な名前を指定する必要があります。  
  
 例:  
  
```sql  
SELECT Length(c. FirstName) AS NameLen FROM
    AdventureWorksEntities.Contact AS c   
    WHERE c.ContactID BETWEEN 10 AND 12  
```  
  
 Output:  
  
|NameLen|  
|-------------|  
|6|  
|6|  
|5|  
  
### <a name="microsoft-provider-specific"></a>Microsoft プロバイダー固有  
 [Microsoft プロバイダー固有の関数](../sqlclient-for-ef-functions.md)は、@no__t 1 つの名前空間にあります。  
  
 例:  
  
```sql  
SELECT SqlServer.LEN(c.EmailAddress) AS EmailLen FROM
    AdventureWorksEntities.Contact AS c WHERE   
    c.ContactID BETWEEN 10 AND 12  
```  
  
 Output:  
  
|EmailLen|  
|--------------|  
|27|  
|27|  
|26|  
  
## <a name="namespaces"></a>名前空間  
 [を使用して](using-entity-sql.md)、クエリ式で使用される名前空間を指定します。  
  
 例:  
  
```sql  
using SqlServer; LOWER('AA');  
```  
  
 Output:  
  
|値|  
|-----------|  
|aa|  
  
## <a name="paging"></a>ページング  
 ページングは、 [SKIP](skip-entity-sql.md)および[LIMIT](limit-entity-sql.md)サブ句を[ORDER by](order-by-entity-sql.md)句に宣言することによって表現できます。  
  
 例:  
  
```sql  
SELECT c.ContactID as ID, c.LastName AS Name FROM
    AdventureWorks.Contact AS c ORDER BY c.ContactID SKIP 9 LIMIT 3;  
```  
  
 Output:  
  
|ID|名前|  
|--------|----------|  
|10|Adina|  
|11|Agcaoili|  
|12|Aguilar|  
  
## <a name="grouping"></a>グループ化  
 [GROUPING BY](group-by-entity-sql.md)は、クエリ ([SELECT](select-entity-sql.md)) 式によって返されるオブジェクトのグループを指定します。  
  
 例:  
  
```sql  
SELECT VALUE name FROM AdventureWorksEntities.Product AS P
    GROUP BY P.Name HAVING MAX(P.ListPrice) > 5  
```  
  
 Output:  
  
|NAME|  
|----------|  
|LL Mountain Seat Assembly|  
|ML Mountain Seat Assembly|  
|HL Mountain Seat Assembly|  
|[...]|  
  
## <a name="navigation"></a>ナビゲーション  
 リレーションシップ ナビゲーション操作を使用すると、開始側のエンティティと終了側のエンティティ間のリレーションシップをナビゲートできます。 [NAVIGATE](navigate-entity-sql.md)は、\<namespace > として修飾されたリレーションシップの種類を取得します。 \<relationship 型名 >。 To end のカーディナリティが1の場合、Navigate は Ref @ no__t-0T > を返します。 終点のカーディナリティが n の場合、コレクション < Ref @ no__t-0T > > が返されます。  
  
 例:  
  
```sql  
SELECT a.AddressID, (SELECT VALUE DEREF(v) FROM   
    NAVIGATE(a, AdventureWorksModel.FK_SalesOrderHeader_Address_BillToAddressID) AS v)   
    FROM AdventureWorksEntities.Address AS a  
```  
  
 Output:  
  
|AddressID|  
|---------------|  
|1|  
|2|  
|3|  
|[...]|  
  
## <a name="select-value-and-select"></a>SELECT VALUE AND SELECT  
  
### <a name="select-value"></a>SELECT VALUE  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、暗黙の行の構築をスキップする SELECT VALUE 句が用意されています。 SELECT VALUE 句には 1 つの項目のみを指定できます。 このような句を使用すると、SELECT 句の項目の周囲に行ラッパーは構築されず、たとえば `SELECT VALUE a` のように、目的の図形のコレクションを作成できます。  
  
 例:  
  
```sql  
SELECT VALUE p.Name FROM AdventureWorksEntities.Product AS p
```  
  
 Output:  
  
|名前|  
|----------|  
|Adjustable Race|  
|All-Purpose Bike Stand|  
|AWC Logo Cap|  
|[...]|  
  
### <a name="select"></a>SELECT  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、任意の行を構築するための行コンストラクターも用意されています。 SELECT は、投影内の 1 つまたは複数の要素、および `SELECT a, b, c` などのフィールドを持つデータ レコードの結果を取得します。  
  
 例:  
  
 SELECT p.Name, p.ProductID FROM AdventureWorksEntities.Product as p Output:  
  
|名前|ProductID|  
|----------|---------------|  
|Adjustable Race|1|  
|All-Purpose Bike Stand|879|  
|AWC Logo Cap|712|  
|[...]|[...]|  
  
## <a name="case-expression"></a>CASE 式  
 [Case 式](case-entity-sql.md)は、一連のブール式を評価して結果を決定します。  
  
 例:  
  
```sql  
CASE WHEN AVG({25,12,11}) < 100 THEN TRUE ELSE FALSE END  
```  
  
 Output:  
  
|値|  
|-----------|  
|true|  
  
## <a name="see-also"></a>関連項目

- [Entity SQL リファレンス](entity-sql-reference.md)
- [Entity SQL の概要](entity-sql-overview.md)
