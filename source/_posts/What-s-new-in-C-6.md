---
title: "What's new in C# 6"
date: 2016-03-18 14:53:21
tags: C#
---

## Getter-only auto-properties

Auto-properties can now be declared without a setter.

``` bash
public class Customer
{
    public string First { get; } = "Jane";
    public string Last { get; } = "Doe";
}
```

## Expression bodies on method-like members

Methods as well as user-defined operators and conversions can be given an expression body by use of the “lambda arrow”:

``` bash
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public static implicit operator string(Person p) => p.First + " " +
```

## Using static

The feature allows all the accessible static members of a type to be imported, making them available without qualification in subsequent code:

``` bash
using static System.Console;
using static System.Math;
using static System.DayOfWeek;
class Program
{
    static void Main()
    {
        WriteLine(Sqrt(3*3 + 4*4));
        WriteLine(Friday - Monday);
    }
}
```

## Null-conditional operators

Sometimes code tends to drown a bit in null-checking. The null-conditional operator lets you access members and elements only when the receiver is not-null, providing a null result otherwise:

``` bash
int? length = customers?.Length; // null if customers is null
Customer first = customers?[0];  // null if customers is null
```

## String interpolation

String.Format and its cousins are very versatile and useful, but their use is a little clunky and error prone. Particularly unfortunate is the use of {0} etc. placeholders in the format string, which must line up with arguments supplied separately:

``` bash
var s = String.Format("{0} is {1} year{{s}} old", p.Name, p.Age);
```

## nameof expressions

Occasionally you need to provide a string that names some program element: when throwing anArgumentNullException you want to name the guilty argument; when raising a PropertyChangedevent you want to name the property that changed, etc.
Using string literals for this purpose is simple, but error prone. You may spell it wrong, or a refactoring may leave it stale. nameof expressions are essentially a fancy kind of string literal where the compiler checks that you have something of the given name, and Visual Studio knows what it refers to, so navigation and refactoring will work:

``` bash
if (x == null) throw new ArgumentNullException(nameof(x));
```

## Index initializers

``` bash
Object and collection initializers are useful for declaratively initializing fields and properties of objects, or giving a collection an initial set of elements. Initializing dictionaries and other objects with indexers is less elegant. We are adding a new syntax to object initializers allowing you to set values to keys through any indexer that the new object has:
var numbers = new Dictionary<int, string> {
    [7] = "seven",
    [9] = "nine",
    [13] = "thirteen"
};
```

## Exception filters

``` bash
VB has them. F# has them. Now C# has them too. This is what they look like:
try { … }
catch (MyException e) when (myfilter(e))
{
    …
}
```

## Await in catch and finally blocks

In C# 5 we don’t allow the await keyword in catch and finally blocks, because we’d somehow convinced ourselves that it wasn’t possible to implement. Now we’ve figured it out, so apparently it wasn’t impossible after all.
This has actually been a significant limitation, and people have had to employ unsightly workarounds to compensate. That is no longer necessary:

``` bash
Resource res = null;
try
{
    res = await Resource.OpenAsync(…);       // You could do this.
    …
}
catch(ResourceException e)
{
    await Resource.LogAsync(res, e);         // Now you can do this …
}
finally
{
    if (res != null) await res.CloseAsync(); // … and this.
}

```
