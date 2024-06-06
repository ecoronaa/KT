# Annotations/attributes/markes

- What are annotations/attibutes/markes
- How to use them
- Predefined ones

## [Attributes](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/)

**In this article:**
- Using attributes
- Common uses for attributes
- Reflection overview

Attributes provide a powerful method of associating metadata with code.After an attribute is associated with a program entity, the attribute can be queried at run time by using a technique called *reflection*

### Using attributes

In C#, you specify an attribute by placing the name of the attribute enclosed in square brackets (`[]`) above the declaration of the entity to which it applies.

In this example, the `Serializable` attribute is used to apply a specific characteristic to a class:

```C#
[Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
```

A method example:

```C#
[System.Runtime.InteropServices.DllImport("user32.dll")]
extern static void SampleMethod();
```

More than one attribute can be placed on a declaration as the following example shows:

```C#
void MethodA([In][Out] ref double x) { }
void MethodB([Out][In] ref double x) { }
void MethodC([In, Out] ref double x) { }
```

### Attribute targets

By default, an attribute applies to the element that follows it. But you can also explicitly identify.

```C#
// applies to return value
[return: ValidatedContract]
int Method4() { return 0; }
```

The list of possible target values is shown [here](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/#attribute-targets)

## [Common attributes](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/common-attributes)

**In this article**
- Assembly identity attributes
- Informational attributes
- Assembly manifest attributes

Most attributes are applied to specific language elements such as classes or methods, however, some attributes are global. For example the `AssemblyVersionAttribute` attribute can be used to embed version information into an assembly:

```C#
[assembly: AssemblyVersion("1.0.0.0")]
```

Visual Studio adds global attributes to the AssemblyInfo.cs file in .NET Framework projects. These attributes aren't added to .NET Core projects.

Assembly attributes are values that provide information about an assembly. They fall into the following categories:
- [Assembly identity attributes](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/global#assembly-identity-attributes): name, version, and culture.
- [Informational attributes](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/global#informational-attributes): company or product information.
- [Assembly manifest attributes](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/attributes/global#assembly-manifest-attributes): title, description, default alias, and configuration.

