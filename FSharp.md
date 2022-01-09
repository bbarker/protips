# Effect systems and type classes

[From discord](https://discord.com/channels/196693847965696000/196693847965696000/929787262231793675):

chethusk:
> f#+ is the go-to for all sorts of statically-resolved type-level programming/tracking in F#, yes. There's also [typeshape](https://github.com/eiriktsarpalis/TypeShape) for datatype-generic tasks. most effects systems I've seen rely on a couple of F# features around interfaces - notably the 'flexible type' syntax #<some interface/base type>, which expands to 't when 't :> <some interface/base type>. They use interface implementations + object expressions (for inline/anonymous implementation of interfaces) for adding/handling effects. This can often not have a huge perf impact if done correctly since the .NET JIT can devirtualize interface calls in some cases.
> that's about all I know wrt effects systems.  they haven't really taken off in F# (OSS projects at least), but there's nothing aside from network effects preventing that I think.
