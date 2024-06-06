# The programming language ecosystem

- Files generated during compilation
- Virtual machine/platform of execution

## [Glossary](https://learn.microsoft.com/en-us/dotnet/standard/glossary)

AOT - Ahead-of-time compiler
- in contrast to JIT compilation, AOT happens before the application is executed  and occurs on different machine.
- can spend more time optimizing.

App model
- a workload-specific API

ASP.NET
- The original ASP.NET implementation that ships with the .NET Framework, also known as ASP.NET 4.x and ASP.NET Framework.

ASP.NET Core
- Open-source implementation of ASP.NET.

Assembly
- A .dll or .exe file
- An assembly contains interfaces, classes, structures, enumerations, and delegates.
- Assemblies in a project's `bin` folder are sometimes referred to as **binaries**.

BCL - Base Class Library
- A set of libraries that comprise the System.

CLR - Common Language Runtime
- Common Language Runtime usually refers to the runtime of .NET Framework or the runtime of .NET.
- handles memory allocation and management.
- virtual machine that not only executed apps but also generates and compiles code on the-fly using a JIT compiler.
- Windows only.

Core CLR
- The Common Language Runtime for .NET.

CoreRT
- In contrast to the CLR is not a virtual machine.
- doesn't include a JIT

Cross-platform
- enables code reuse and consistency between applications on different platforms.

ecosystem
- runtime software, dev tools, and community resources that are used to build and run applications.
- includes third-party apps and libraries.

framework
- A collection of APIs that facilitates development and deployment of applications
- sometimes *framework* refers to an implementation of .NET.

framework libraries
- Might refer to the framework libraries for .NET (same libraries that BCL refers to)
- Might also refer to the ASP.NET Core framework libraries.

GC - Garbage collector
- The GC frees memory occupied by objects that are no longer in use.

IL - Intermediate language
- Higher-level languages compile down to a hardware-agnostic instruction set (IL)
- IL is sometimes referred to as MSIL of CIL.

JIT - Just-in-time compiler
- JIT compilation happens on demand and is performed on the same machine.
- have to balance time spent optimizing code against the savings that the resulting code can produce.

Implementation of .NET
- .NET Framework
- .NET
- Universal Windows Platform (UWP)
- Mono

Library
- A collection of APIs that can be called by apps or other libraries.

Mono
- Open source
- Cross-platform .NET implementation that's used when a small runtime is required.

Native AOT
- A deployment mode where the app is self-contained and is ahead-of-time compiled to native code at the time of publish.
- They can run on machines that don't have the .NET runtime installed.

.NET
- .NET can be used as umbrella term for .NET Standard and all .NET implementations and workloads.
- .NET more frequently refers to the implementation of .NET that used to be called .NET Core. It can also be referred to as .NET 5 and later versions.

.NET CLI, aka .NET Core CLI
- A cross-platform toolchain for developing applications and libraries for .NET.

.NET Core
- .NET 5 and later versions.

.NET Framework 
- An implementation of .NET that runs only on Windows.

.NET Native
- A compiler tool chain that produces native code AOT
- UWP (Universal Windows Platform) is the application framework supported by .NET Native.

.NET SDK
- A set of libraries and tools that enable developers to create applications and libraries for .NET

.NET Standard
- A formal specification of .NET APIs that are available in each .NET implementation.

NGen
- Native (image) generation.
- You can think of this technology as a persistent JIT compiler. *Compilation typically occurs at install time*

Package --or just a package--
- A NuGet package is a .zip file with one or more assemblies of the same name along with additional metadata such ass the author name.

Platform
- An operating system and the hardware it runs on.

POCO --or a plain old class/CLR object--
- A POCO is a .NET data structure that contains only public properties or fields. A POCO shouldn't contain any other members, such as methods, events, delegates.
- These objects are used primarily as data transfer objects (DTOs).
- A pure POCO won't inherit another object or implement an interface.

runtime
- In general, the execution environment for a managed program. The OS is part of the runtime environment but is not part of the .NET runtime.

shared framework
- common libraries

stack
- A set of programming technologies that are used together to build and run applications.

target framework
- The collection of APIs that a .NET app or library relies on.

TFM - Target Framework Moniker
- Target frameworks are typically referenced by a short name, such as `net462` instead of `.NETFramework,Version=4.6.2`

UWP - Universal Windows Platform
- An implementation of .NET that is used for building touch-enabled Windows applications and software for the IoT. 
- UWP provides many services, such as a centralized app store, an execution environment (AppContainer), and a set of Windows APIs to use instead of Win32 (WinRT).

workload
- A type of app someone is building. More generic than app model. For example, at the top of every .NET documentation page, including this one, is a drop-down list for Workloads, which lets you switch to documentation for Web, Mobile, Cloud, Desktop, and Machine Learning & Data.

## [Basic Concepts](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts)

**In this article**
- Application startup
- Application termination
- Declarations
- Members
- Member access
- Signatures and overloading
- Scopes
- Namespace and type names
- Automatic memory management
- Execution order

### [Declared accessibility](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#752-declared-accessibility)

- `public` - access not limited.
- `protected`- access limited to the containing class or types derived from the containing class.
- `internal` - access limited to this assembly.
- `protected internal` - accessible within this assembly as well as types derived from the containing class.
- `private protected` - accessible within this assembly by the containing class and types derived from the containing class.
- `private` - access limited to the containing type.

> Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted. For further details review original article.

### [Protected access](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#754-protected-access)

```C#
public class A
{
    protected int x;

    static void F(A a, B b)
    {
        a.x = 1; // Ok
        b.x = 1; // Ok
    }
}

public class B : A
{
    static void F(A a, B b)
    {
        a.x = 1; // Error, must access through instance of B
        b.x = 1; // Ok
    }
}
```

### [Scopes](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#77-scopes)

**[General](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#771-general)**

```C#
class A
{
    void F()
    {
        i = 1;
    }

    int i = 0;
}
```

### [Name hiding](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#772-name-hiding)

**[Hiding through inheritance](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/basic-concepts#7723-hiding-through-inheritance)**

Hiding a visible name from an inherited scope causes a warning to be reported:

```C#
class Base
{
    public void F() {}
}

class Derived : Base
{
    public void F() {} // Warning, hiding an inherited name
}
```

The waring caused by hiding an inherited name can be eliminated through use of the `new` modifier:

```C#
class Base
{
    public void F() {}
}

class Derived : Base
{
    public new void F() {}
}
```