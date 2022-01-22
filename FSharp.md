# Educational Resources

1. [Get Started with F#](https://docs.microsoft.com/en-us/dotnet/fsharp/get-started/)
    1. [Fable REPL](https://fable.io/repl/) (runs F# online)
    2. [Introduction to F# on Binder](https://mybinder.org/v2/gh/dotnet/interactive/main?urlpath=lab) (runs F# online) is 
       [Jupyter notebook](https://jupyter.org/) on hosted via the free Binder service.
    3. [Tour of F#](https://docs.microsoft.com/en-us/dotnet/fsharp/tour) - can run these in one of the above services.
    4. [MS Learn](https://docs.microsoft.com/en-us/learn/) has multiple modules on F#, and tracks progress.

# Effect systems and type classes

[From discord](https://discord.com/channels/196693847965696000/196693847965696000/929787262231793675):

chethusk:
> f#+ is the go-to for all sorts of statically-resolved type-level programming/tracking in F#, yes. There's also [typeshape](https://github.com/eiriktsarpalis/TypeShape) for datatype-generic tasks. most effects systems I've seen rely on a couple of F# features around interfaces - notably the 'flexible type' syntax #<some interface/base type>, which expands to 't when 't :> <some interface/base type>. They use interface implementations + object expressions (for inline/anonymous implementation of interfaces) for adding/handling effects. This can often not have a huge perf impact if done correctly since the .NET JIT can devirtualize interface calls in some cases.
> that's about all I know wrt effects systems.  they haven't really taken off in F# (OSS projects at least), but there's nothing aside from network effects preventing that I think.


# Running a program and loading it in FSI

With a program like:

```fsharp
module Program
// Define a new function to print a name.
let printGreeting name = printfn $"Hello {name} from F#!"


[<EntryPoint>]
let main argv =
    // Call your new function!
    printGreeting "Brandon"
    0
```

We can run it on the command line with `dotnet run`. We can also load it in FSI using `dotnet fsi .\Program.fs`,
or if we're already in FSI: `#load "Program.fs";;`. Then run it with `Program.main [||];;` (`[||]` is our empty
array).

