---
title: Override キーワードと New キーワードを使用する場合について - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- override keyword [C#]
- new keyword [C#]
- polymorphism [C#], using override and new [C#]
ms.assetid: 323db184-b136-46fc-8839-007886e7e8b0
ms.openlocfilehash: 493c6c5f5bf47c6b2cd140ac0f6922f91ca4252b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170261"
---
# <a name="knowing-when-to-use-override-and-new-keywords-c-programming-guide"></a>Override キーワードと New キーワードを使用する場合について (C# プログラミング ガイド)

C# では、派生クラスのメソッドを基底クラスのメソッドと同じ名前にすることができます。 [new](../../language-reference/keywords/new-modifier.md) および [override](../../language-reference/keywords/override.md) キーワードを使って、メソッドでの処理を指定できます。 `override` 修飾子は基底クラスの *メソッドを "* 拡張`virtual`" し、`new` 修飾子はアクセス可能な基底クラスのメソッドを "*非表示*" にします。 このトピックの例ではその違いを示します。  
  
 コンソール アプリケーションで、次の 2 つのクラス `BaseClass` と `DerivedClass` を宣言します。 `DerivedClass` は `BaseClass` を継承します。  
  
```csharp  
class BaseClass  
{  
    public void Method1()  
    {  
        Console.WriteLine("Base - Method1");  
    }  
}  
  
class DerivedClass : BaseClass  
{  
    public void Method2()  
    {  
        Console.WriteLine("Derived - Method2");  
    }  
}  
```  
  
 `Main` メソッドで、変数 `bc`、`dc`、`bcdc` を宣言します。  
  
- `bc` は `BaseClass` 型であり、その値は `BaseClass` です。  
  
- `dc` は `DerivedClass` 型であり、その値は `DerivedClass` です。  
  
- `bcdc` は `BaseClass` 型であり、その値は `DerivedClass` です。 この変数には注意する必要があります。  
  
 `bc` と `bcdc` は `BaseClass` 型なので、キャストを使わない限り、直接アクセスできるのは `Method1` だけです。 変数 `dc` は、`Method1` と `Method2` の両方にアクセスできます。 これらの関係を次のコードに示します。  
  
```csharp  
class Program  
{  
    static void Main(string[] args)  
    {  
        BaseClass bc = new BaseClass();  
        DerivedClass dc = new DerivedClass();  
        BaseClass bcdc = new DerivedClass();  
  
        bc.Method1();  
        dc.Method1();  
        dc.Method2();  
        bcdc.Method1();  
    }  
    // Output:  
    // Base - Method1  
    // Base - Method1  
    // Derived - Method2  
    // Base - Method1  
}  
```  
  
 次に、以下の `Method2` メソッドを `BaseClass` に追加します。 このメソッドのシグネチャは、`Method2` の `DerivedClass` メソッドのシグネチャと一致します。  
  
```csharp  
public void Method2()  
{  
    Console.WriteLine("Base - Method2");  
}  
```  
  
 `BaseClass` に `Method2` メソッドを追加したので、次のコードに示すように、`BaseClass` の変数 `bc` および `bcdc` に 2 つめの呼び出しステートメントを追加できます。  
  
```csharp  
bc.Method1();  
bc.Method2();  
dc.Method1();  
dc.Method2();  
bcdc.Method1();  
bcdc.Method2();  
```  
  
 プロジェクトをビルドすると、`Method2` に `BaseClass` メソッドを追加したことで警告が発生するようになります。 警告の内容は、`Method2` の `DerivedClass` メソッドが `Method2` の `BaseClass` メソッドを隠ぺいしているというものです。 それが意図する結果である場合は、`new` の定義で `Method2` キーワードを使うことをお勧めします。 または、どちらかの `Method2` メソッドの名前を変更して警告を解決することもできますが、実用的ではない場合があります。  
  
 `new` を追加する前に、プログラムを実行して、追加した呼び出しステートメントによって生成される出力を確認します。 次のような結果が表示されます。  
  
```csharp  
// Output:  
// Base - Method1  
// Base - Method2  
// Base - Method1  
// Derived - Method2  
// Base - Method1  
// Base - Method2  
```  
  
 `new` キーワードは、その出力を生成する関係を維持しますが、警告を抑制します。 `BaseClass` 型の変数は引き続き `BaseClass` のメンバーにアクセスし、`DerivedClass` 型の変数は引き続き最初に `DerivedClass` のメンバーにアクセスした後、`BaseClass` から継承されたメンバーを考慮します。  
  
 警告を抑制するには、次のコードで示すように、`new` での `Method2` の定義に `DerivedClass` 修飾子を追加します。 修飾子を追加する位置は、`public` の前でも後でもかまいません。  
  
```csharp  
public new void Method2()  
{  
    Console.WriteLine("Derived - Method2");  
}  
```  
  
 もう一度プログラムを実行し、出力が変わらないことを確認します。 また、警告が表示されなくなったことを確認します。 `new` を使うことにより、それによって修飾されるメンバーが基底クラスから継承されたメンバーを隠ぺいすることを了解していることを明示します。 継承による名前の非表示について詳しくは、「[new 修飾子](../../language-reference/keywords/new-modifier.md)」をご覧ください。  
  
 この動作を `override` を使ったときの効果と比較するため、次のメソッドを `DerivedClass` に追加します。 `override` 修飾子を追加する位置は、`public` の前でも後でもかまいません。  
  
```csharp  
public override void Method1()  
{  
    Console.WriteLine("Derived - Method1");  
}  
```  
  
 `virtual` での `Method1` の定義に `BaseClass` 修飾子を追加します。 `virtual` 修飾子を追加する位置は、`public` の前でも後でもかまいません。  
  
```csharp  
public virtual void Method1()  
{  
    Console.WriteLine("Base - Method1");  
}  
```  
  
 プロジェクトを再度実行します。 次の出力の最後の 2 行に特に注意してください。  
  
```csharp  
// Output:  
// Base - Method1  
// Base - Method2  
// Derived - Method1  
// Derived - Method2  
// Derived - Method1  
// Base - Method2  
```  
  
 `override` 修飾子を使うことで、`bcdc` は、`Method1` で定義されている `DerivedClass` メソッドにアクセスできます。 通常、これは継承階層での必要な動作です。 派生クラスから作成される値を持つオブジェクトには、派生クラスで定義されているメソッドを使うことが必要とされます。 そのような動作は、`override` を使って基底クラスのメソッドを拡張することで実現できます。  
  
 ここまでの例をすべて含んだコードを次に示します。  
  
```csharp  
using System;  
using System.Text;  
  
namespace OverrideAndNew  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            BaseClass bc = new BaseClass();  
            DerivedClass dc = new DerivedClass();  
            BaseClass bcdc = new DerivedClass();  
  
            // The following two calls do what you would expect. They call  
            // the methods that are defined in BaseClass.  
            bc.Method1();  
            bc.Method2();  
            // Output:  
            // Base - Method1  
            // Base - Method2  
  
            // The following two calls do what you would expect. They call  
            // the methods that are defined in DerivedClass.  
            dc.Method1();  
            dc.Method2();  
            // Output:  
            // Derived - Method1  
            // Derived - Method2  
  
            // The following two calls produce different results, depending
            // on whether override (Method1) or new (Method2) is used.  
            bcdc.Method1();  
            bcdc.Method2();  
            // Output:  
            // Derived - Method1  
            // Base - Method2  
        }  
    }  
  
    class BaseClass  
    {  
        public virtual void Method1()  
        {  
            Console.WriteLine("Base - Method1");  
        }  
  
        public virtual void Method2()  
        {  
            Console.WriteLine("Base - Method2");  
        }  
    }  
  
    class DerivedClass : BaseClass  
    {  
        public override void Method1()  
        {  
            Console.WriteLine("Derived - Method1");  
        }  
  
        public new void Method2()  
        {  
            Console.WriteLine("Derived - Method2");  
        }  
    }  
}  
```  
  
 次の例では、異なるコンテキストでの同様の動作を示します。 この例では、3 つのクラスが定義されています。基底クラス `Car` と、それから派生される 2 つのクラス `ConvertibleCar` と `Minivan` です。 基底クラスには、`DescribeCar` メソッドが含まれます。 このメソッドは、車の基本的な説明を表示した後、`ShowDetails` を呼び出して追加情報を提供します。 これら 3 つのクラスのそれぞれで、`ShowDetails` メソッドが定義されています。 `new` 修飾子は、`ShowDetails` クラスで `ConvertibleCar` を定義するために使われています。 `override` 修飾子は、`ShowDetails` クラスで `Minivan` を定義するために使われています。  
  
```csharp  
// Define the base class, Car. The class defines two methods,  
// DescribeCar and ShowDetails. DescribeCar calls ShowDetails, and each derived  
// class also defines a ShowDetails method. The example tests which version of  
// ShowDetails is selected, the base class method or the derived class method.  
class Car  
{  
    public void DescribeCar()  
    {  
        System.Console.WriteLine("Four wheels and an engine.");  
        ShowDetails();  
    }  
  
    public virtual void ShowDetails()  
    {  
        System.Console.WriteLine("Standard transportation.");  
    }  
}  
  
// Define the derived classes.  
  
// Class ConvertibleCar uses the new modifier to acknowledge that ShowDetails  
// hides the base class method.  
class ConvertibleCar : Car  
{  
    public new void ShowDetails()  
    {  
        System.Console.WriteLine("A roof that opens up.");  
    }  
}  
  
// Class Minivan uses the override modifier to specify that ShowDetails  
// extends the base class method.  
class Minivan : Car  
{  
    public override void ShowDetails()  
    {  
        System.Console.WriteLine("Carries seven people.");  
    }  
}  
```  
  
 この例では、どちらのバージョンの `ShowDetails` が呼び出されるかをテストします。 次の `TestCars1` メソッドでは、各クラスのインスタンスを宣言した後、各インスタンスの `DescribeCar` を呼び出します。  
  
```csharp  
public static void TestCars1()  
{  
    System.Console.WriteLine("\nTestCars1");  
    System.Console.WriteLine("----------");  
  
    Car car1 = new Car();  
    car1.DescribeCar();  
    System.Console.WriteLine("----------");  
  
    // Notice the output from this test case. The new modifier is  
    // used in the definition of ShowDetails in the ConvertibleCar  
    // class.
  
    ConvertibleCar car2 = new ConvertibleCar();  
    car2.DescribeCar();  
    System.Console.WriteLine("----------");  
  
    Minivan car3 = new Minivan();  
    car3.DescribeCar();  
    System.Console.WriteLine("----------");  
}  
```  
  
 `TestCars1` で生成される出力は次のとおりです。 `car2` の結果に特に注目してください。予測していた内容と違っている可能性があります。 オブジェクトの型は `ConvertibleCar` ですが、`DescribeCar` は `ShowDetails` クラスで定義されているバージョンの `ConvertibleCar` にアクセスしていません。これは、このメソッドが、`new` 修飾子ではなく `override` 修飾子を使って宣言されているためです。 結果として、`ConvertibleCar` オブジェクトでは `Car` オブジェクトと同じ説明が表示されます。 `car3` オブジェクトである `Minivan` の結果と比べてみてください。 この場合は、`ShowDetails` で宣言されている `Minivan` メソッドは、`ShowDetails` クラスで宣言されている `Car` メソッドをオーバーライドし、表示されるのはミニバンの説明です。  
  
```csharp  
// TestCars1  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Carries seven people.  
// ----------  
```  
  
 `TestCars2` は、`Car` 型のオブジェクトのリストを作成します。 オブジェクトの値は、`Car`、`ConvertibleCar`、`Minivan` の各クラスからインスタンス化されます。 `DescribeCar` は、リストの各要素に対して呼び出されます。 次のコードは、`TestCars2` の定義です。  
  
```csharp  
public static void TestCars2()  
{  
    System.Console.WriteLine("\nTestCars2");  
    System.Console.WriteLine("----------");  
  
    var cars = new List<Car> { new Car(), new ConvertibleCar(),
        new Minivan() };  
  
    foreach (var car in cars)  
    {  
        car.DescribeCar();  
        System.Console.WriteLine("----------");  
    }  
}  
```  
  
 次のような出力が表示されます。 `TestCars1` によって表示される出力と同じであることに注意してください。 オブジェクトの型が `ShowDetails` であるか (`ConvertibleCar` の場合)、`ConvertibleCar` であるか (`TestCars1` の場合) にかかわらず、`Car` クラスの `TestCars2` メソッドは呼び出されません。 逆に、`car3` は、型が `ShowDetails` でも `Minivan` でも、`Minivan` クラスの `Car` を呼び出します。  
  
```csharp  
// TestCars2  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Carries seven people.  
// ----------  
```  
  
 メソッド `TestCars3` と `TestCars4` でこの例は終わりです。 これらのメソッドは、最初は型 `ShowDetails` および `ConvertibleCar` として宣言されているオブジェクトから (`Minivan`)、次に型 `TestCars3` として宣言されているオブジェクトから (`Car`)、`TestCars4` を直接呼び出します。 次のコードは、これら 2 つのメソッドの定義です。  
  
```csharp  
public static void TestCars3()  
{  
    System.Console.WriteLine("\nTestCars3");  
    System.Console.WriteLine("----------");  
    ConvertibleCar car2 = new ConvertibleCar();  
    Minivan car3 = new Minivan();  
    car2.ShowDetails();  
    car3.ShowDetails();  
}  
  
public static void TestCars4()  
{  
    System.Console.WriteLine("\nTestCars4");  
    System.Console.WriteLine("----------");  
    Car car2 = new ConvertibleCar();  
    Car car3 = new Minivan();  
    car2.ShowDetails();  
    car3.ShowDetails();  
}  
```  
  
 これらのメソッドからは次のような出力が生成され、これはこのトピックの最初の例からの結果に対応します。  
  
```csharp  
// TestCars3  
// ----------  
// A roof that opens up.  
// Carries seven people.  
  
// TestCars4  
// ----------  
// Standard transportation.  
// Carries seven people.  
```  
  
 完全なプロジェクトとその出力を次に示します。  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
  
namespace OverrideAndNew2  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            // Declare objects of the derived classes and test which version  
            // of ShowDetails is run, base or derived.  
            TestCars1();  
  
            // Declare objects of the base class, instantiated with the  
            // derived classes, and repeat the tests.  
            TestCars2();  
  
            // Declare objects of the derived classes and call ShowDetails  
            // directly.  
            TestCars3();  
  
            // Declare objects of the base class, instantiated with the  
            // derived classes, and repeat the tests.  
            TestCars4();  
        }  
  
        public static void TestCars1()  
        {  
            System.Console.WriteLine("\nTestCars1");  
            System.Console.WriteLine("----------");  
  
            Car car1 = new Car();  
            car1.DescribeCar();  
            System.Console.WriteLine("----------");  
  
            // Notice the output from this test case. The new modifier is  
            // used in the definition of ShowDetails in the ConvertibleCar  
            // class.
            ConvertibleCar car2 = new ConvertibleCar();  
            car2.DescribeCar();  
            System.Console.WriteLine("----------");  
  
            Minivan car3 = new Minivan();  
            car3.DescribeCar();  
            System.Console.WriteLine("----------");  
        }  
        // Output:  
        // TestCars1  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Carries seven people.  
        // ----------  
  
        public static void TestCars2()  
        {  
            System.Console.WriteLine("\nTestCars2");  
            System.Console.WriteLine("----------");  
  
            var cars = new List<Car> { new Car(), new ConvertibleCar(),
                new Minivan() };  
  
            foreach (var car in cars)  
            {  
                car.DescribeCar();  
                System.Console.WriteLine("----------");  
            }  
        }  
        // Output:  
        // TestCars2  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Carries seven people.  
        // ----------  
  
        public static void TestCars3()  
        {  
            System.Console.WriteLine("\nTestCars3");  
            System.Console.WriteLine("----------");  
            ConvertibleCar car2 = new ConvertibleCar();  
            Minivan car3 = new Minivan();  
            car2.ShowDetails();  
            car3.ShowDetails();  
        }  
        // Output:  
        // TestCars3  
        // ----------  
        // A roof that opens up.  
        // Carries seven people.  
  
        public static void TestCars4()  
        {  
            System.Console.WriteLine("\nTestCars4");  
            System.Console.WriteLine("----------");  
            Car car2 = new ConvertibleCar();  
            Car car3 = new Minivan();  
            car2.ShowDetails();  
            car3.ShowDetails();  
        }  
        // Output:  
        // TestCars4  
        // ----------  
        // Standard transportation.  
        // Carries seven people.  
    }  
  
    // Define the base class, Car. The class defines two virtual methods,  
    // DescribeCar and ShowDetails. DescribeCar calls ShowDetails, and each derived  
    // class also defines a ShowDetails method. The example tests which version of  
    // ShowDetails is used, the base class method or the derived class method.  
    class Car  
    {  
        public virtual void DescribeCar()  
        {  
            System.Console.WriteLine("Four wheels and an engine.");  
            ShowDetails();  
        }  
  
        public virtual void ShowDetails()  
        {  
            System.Console.WriteLine("Standard transportation.");  
        }  
    }  
  
    // Define the derived classes.  
  
    // Class ConvertibleCar uses the new modifier to acknowledge that ShowDetails  
    // hides the base class method.  
    class ConvertibleCar : Car  
    {  
        public new void ShowDetails()  
        {  
            System.Console.WriteLine("A roof that opens up.");  
        }  
    }  
  
    // Class Minivan uses the override modifier to specify that ShowDetails  
    // extends the base class method.  
    class Minivan : Car  
    {  
        public override void ShowDetails()  
        {  
            System.Console.WriteLine("Carries seven people.");  
        }  
    }  
  
}  
```  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [クラスと構造体](./index.md)
- [Override キーワードと New キーワードによるバージョン管理](./versioning-with-the-override-and-new-keywords.md)
- [base](../../language-reference/keywords/base.md)
- [abstract](../../language-reference/keywords/abstract.md)
