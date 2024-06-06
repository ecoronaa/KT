# Static and dynamic casting

- Pros and cons of dynamic casting
- How to use dynamic casting

## [Using type dynamic](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/using-type-dynamic)

**In this article**
- Conversions
- Overload resolution with arguments of type dynamic
- Dynamic language runtime
- COM interop

The `dynamic` type is a static type, but an object of type `dynamic` bypasses static type checking. The compiler assumes a `dynamic` element supports any operation. However, if the code isn't valid, errors surface at run time.

```C#
static void Main(string[] args)
{
    ExampleClass ec = new ExampleClass();
    // The following call to exampleMethod1 causes a compiler error
    // if exampleMethod1 has only one parameter. Uncomment the line
    // to see the error.
    //ec.exampleMethod1(10, 4);

    dynamic dynamic_ec = new ExampleClass();
    // The following line is not identified as an error by the
    // compiler, but it causes a run-time exception.
    dynamic_ec.exampleMethod1(10, 4);

    // The following calls also do not cause compiler errors, whether
    // appropriate methods exist or not.
    dynamic_ec.someMethod("some argument", 7, null);
    dynamic_ec.nonexistentMethod();
}
```

```C#
class ExampleClass
{
    public ExampleClass() { }
    public ExampleClass(int v) { }

    public void exampleMethod1(int i) { }

    public void exampleMethod2(string str) { }
}
```

