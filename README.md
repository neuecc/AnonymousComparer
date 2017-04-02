# AnonymousComparer
Lambda compare selector for Linq

Info
---
Archive, import from Codeplex.

How To Use
---
var comparer = AnonymousComparer.Create(compareKeySelector) or use added Standard Query Operator's Extension Methods.

* Available in NuGet [url:Install-Package AnonymousComparer|http://nuget.org/List/Packages/AnonymousComparer]

Example
---
```csharp
class MyClass
{
    public int MyProperty { get; set; }
}

static void Main()
{
    // for example.
    var mc1 = new MyClass { MyProperty = 3 };
    var mc2 = new MyClass { MyProperty = 3 };
    var array = new[] { mc1, mc2 };

    array.Distinct().Count(); // 2
    array.Distinct(mc => mc.MyProperty).Count(); // 1
    // full write.
    array.Distinct(AnonymousComparer.Create((MyClass mc) => mc.MyProperty));

    // example2, with anonymous type sequence
    var anonymous = new[] 
    {
        new { Foo = "A", Key = 10 },
        new { Foo = "B", Key = 15 }
    };
    anonymous.Contains(new { Foo = "dummy", Key = 10 }, a => a.Key); // true
}
```

Full IEqualtyComparer<T> overload
---
```csharp
var myClassComparer = AnonymousComparer.Create<MyClass>(
    (x, y) => x.MyProperty == y.MyProperty, // Equals
    obj => obj.MyProperty.GetHashCode()); // GetHashCode
```

Anonymous IComparer<T>
---
```csharp
var seq = new[] { 1, 2, 3 };
// Create IComparer<T>
var comparer = AnonymousComparer.Create<int>((x, y) => y - x);
seq.OrderBy(x => x, comparer);
// OrderBy/ThenBy Extension Methods
seq.OrderBy(x => x, (x, y) => y - x); // 3, 2, 1
seq.OrderByDescending(x => x, (x, y) => y - x); // 1, 2, 3
```
