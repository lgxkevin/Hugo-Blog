---
title: "SOLID principle in C#"
date: 2021-02-27T15:52:24+13:00
draft: false
description: "SOLID principle in C#"
resources:
- name: "featured-image"
  src: "featured-image.jpg"

tags: ["SOLID", "SOLID principle", "C#"]
categories: ["SOLID principle"]

lightgallery: true
---

### Single Responsibility Principle
- A class should have one, and only one, reason to change
- A class should only be responsible for one thing.
- There's a place for everything and everything is in its place
- Find one reason to change and take everything else out of the class
- Very precise names for small classes > generic names for large classes


```C#
namespace DotNetDesignPatternDemos.SOLID.SRP
{
  // just stores a couple of journal entries and ways of
  // working with them
  public class Journal
  {
    private readonly List<string> entries = new List<string>();

    private static int count = 0;

    public int AddEntry(string text)
    {
      entries.Add($"{++count}: {text}");
      return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
      entries.RemoveAt(index);
    }

    public override string ToString()
    {
      return string.Join(Environment.NewLine, entries);
    }

    // breaks single responsibility principle
    public void Save(string filename, bool overwrite = false)
    {
      File.WriteAllText(filename, ToString());
    }

    public void Load(string filename)
    {
      
    }

    public void Load(Uri uri)
    {
      
    }
  }

  // handles the responsibility of persisting objects
  public class Persistence
  {
    public void SaveToFile(Journal journal, string filename, bool overwrite = false)
    {
      if (overwrite || !File.Exists(filename))
        File.WriteAllText(filename, journal.ToString());
    }
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var j = new Journal();
      j.AddEntry("I cried today.");
      j.AddEntry("I ate a bug.");
      WriteLine(j);

      var p = new Persistence();
      var filename = @"c:\temp\journal.txt";
      p.SaveToFile(j, filename);
      Process.Start(filename);
    }
  }
}
```

### Open/Closed Principle
- An entity should be open for extension but closed for modification.
- Extend functionality by adding new code instead of changing existing code.
- Separate the behaviors, so the system can easily be extended, but never broken
- Goal: get to a point where you can never break the core of your system
  
{{< image src="https://lgxkevin-blog.s3.us-east-2.amazonaws.com/SOLID_Principle/OCP.jpg" >}}

```C#
namespace DotNetDesignPatternDemos.SOLID.OCP
{
  public enum Color
  {
    Red, Green, Blue
  }

  public enum Size
  {
    Small, Medium, Large, Yuge
  }

  public class Product
  {
    public string Name;
    public Color Color;
    public Size Size;

    public Product(string name, Color color, Size size)
    {
      Name = name ?? throw new ArgumentNullException(paramName: nameof(name));
      Color = color;
      Size = size;
    }
  }

  public class ProductFilter
  {
    // let's suppose we don't want ad-hoc queries on products
    public IEnumerable<Product> FilterByColor(IEnumerable<Product> products, Color color)
    {
      foreach (var p in products)
        if (p.Color == color)
          yield return p;
    }
    
    public static IEnumerable<Product> FilterBySize(IEnumerable<Product> products, Size size)
    {
      foreach (var p in products)
        if (p.Size == size)
          yield return p;
    }

    public static IEnumerable<Product> FilterBySizeAndColor(IEnumerable<Product> products, Size size, Color color)
    {
      foreach (var p in products)
        if (p.Size == size && p.Color == color)
          yield return p;
    } // state space explosion
      // 3 criteria = 7 methods

    // OCP = open for extension but closed for modification
  }

  // we introduce two new interfaces that are open for extension

  public interface ISpecification<T>
  {
    bool IsSatisfied(Product p);
  }

  public interface IFilter<T>
  {
    IEnumerable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
  }

  public class ColorSpecification : ISpecification<Product>
  {
    private Color color;

    public ColorSpecification(Color color)
    {
      this.color = color;
    }

    public bool IsSatisfied(Product p)
    {
      return p.Color == color;
    }
  }

  public class SizeSpecification : ISpecification<Product>
  {
    private Size size;

    public SizeSpecification(Size size)
    {
      this.size = size;
    }

    public bool IsSatisfied(Product p)
    {
      return p.Size == size;
    }
  }

  // combinator
  public class AndSpecification<T> : ISpecification<T>
  {
    private ISpecification<T> first, second;

    public AndSpecification(ISpecification<T> first, ISpecification<T> second)
    {
      this.first = first ?? throw new ArgumentNullException(paramName: nameof(first));
      this.second = second ?? throw new ArgumentNullException(paramName: nameof(second));
    }

    public bool IsSatisfied(Product p)
    {
      return first.IsSatisfied(p) && second.IsSatisfied(p);
    }
  }

  public class BetterFilter : IFilter<Product>
  {
    public IEnumerable<Product> Filter(IEnumerable<Product> items, ISpecification<Product> spec)
    {
      foreach (var i in items)
        if (spec.IsSatisfied(i))
          yield return i;
    }
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var apple = new Product("Apple", Color.Green, Size.Small);
      var tree = new Product("Tree", Color.Green, Size.Large);
      var house = new Product("House", Color.Blue, Size.Large);

      Product[] products = {apple, tree, house};

      var pf = new ProductFilter();
      WriteLine("Green products (old):");
      foreach (var p in pf.FilterByColor(products, Color.Green))
        WriteLine($" - {p.Name} is green");

      // ^^ BEFORE

      // vv AFTER
      var bf = new BetterFilter();
      WriteLine("Green products (new):");
      foreach (var p in bf.Filter(products, new ColorSpecification(Color.Green)))
        WriteLine($" - {p.Name} is green");

      WriteLine("Large products");
      foreach (var p in bf.Filter(products, new SizeSpecification(Size.Large)))
        WriteLine($" - {p.Name} is large");

      WriteLine("Large blue items");
      foreach (var p in bf.Filter(products,
        new AndSpecification<Product>(new ColorSpecification(Color.Blue), new SizeSpecification(Size.Large)))
      )
      {
        WriteLine($" - {p.Name} is big and blue");
      }
    }
  }
}
```
