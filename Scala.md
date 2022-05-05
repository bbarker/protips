# Scala 3

## Refinement types

Currently, Opaque can't be used with inline, but this [may be changing](https://github.com/lampepfl/dotty-feature-requests/issues/82). The reason that it is difficult, [quoting Odersky](https://github.com/lampepfl/dotty-feature-requests/issues/82#issuecomment-599225281):

> This will be next to impossible to achieve. Opaque types rely on location for type-checking. Inline methods rely on moving code to the caller. I see no way to (re-)typecheck inlined code at the call site and at the same time keep typing as if it was at the original location.

# Generic programming

## Typeclasses

### Summoners

```scala
trait Summoner[D[_[_]]] {
  def apply[C[_]: D]: D[C] = implicitly[D[C]]
}

trait Functor[C[_]] {
    def map[A, B](ca: C[A])(ab: A => B): C[B]
}

object Functor extends Summoner[Functor]
// Without using a Summoner we would have had:
// object Functor {
//   def apply[C[_]: Functor]: Functor[C] = implicitly[Functor[C]]
// }
```
([source](https://torre.me.uk/docs/category_theory/))

# ZIO

## Concurrency

### Errors in Concurrent programs

It is important to `join` if we want to allow the completion of the child threads before the parent
thread finishes. I wasn't doing this, so we never saw `2` printed out below:

```scala
import scala.annotation.nowarn
import zio.*
import zio.clock.Clock
import zio.duration.*
import java.io.IOException

object ConcurrencyErrorExperiments {

  val eff1: RIO[Clock, Unit] = for {
    clock <- ZIO.service[Clock.Service]
    _     <- UIO(println("1"))
    _     <- clock.sleep(1.second)
    _     <- UIO(println("2"))
  } yield ()

  @nowarn
  val effWithError: RIO[Clock, Unit] = for {
    clock <- ZIO.service[Clock.Service]
    _     <- UIO(println("3"))
    _     <- clock.sleep(1.second)
    _     <- ZIO.fail(new NumberFormatException("boom"))
  } yield ()

  @nowarn
  val effWithDeffect: RIO[Clock, Unit] = for {
    clock <- ZIO.service[Clock.Service]
    _     <- UIO(println("3"))
    _     <- clock.sleep(1.second)
    _     <- UIO(throw new IOException("big boom"))
  } yield ()

  val eff = UIO(println("hello!"))
  val run: URIO[ZEnv, ExitCode] = ZIO
    .forkAll(
      List(
        eff1.retryN(3),
        effWithDeffect.retryN(3),
      ),
    )
    .flatMap(_.join)
    .exitCode

  def main(args: Array[String]): Unit = {

    val exitVal = zio.Runtime.default.unsafeRun(run)
    println(exitVal)
  }

}
```

## ZLayers

you can also express this:

```scala
  val layer: URLayer[Has[Session] & Has[CassandraBonusDataConfig], Has[BonusConfigStorage]] =
    ZLayer.fromServices[Session, CassandraBonusDataConfig, BonusConfigStorage] { (session, config) =>
      CassandraBonusConfigStorageAdapter(session, config.keyspace, config.bonusConfigTable)
    }
```

Like this:

```scala
val layer = ZIO.services[Session, CassandraBonusDataConfig, BonusConfigStorage].map { 
  case (session, config, storage) => CassandraBonusConfigStorageAdapter(session, config.keyspace, config.bonusConfigTable)
}.toLayer
```

## The cost of ZLayer construction

`provide` is providing a value directly, whereas `provideLayer` is like saying

```scala
resource.use { r =>
 ...
}
```

and paying for the cost of initializing and tearing down the resource. -- Calvin Fernandes

## Lawful reasoning

> In this case these guarantees were relatively obvious and we probably did not even need to think about them. But as we learn more in this book we will see that much of the power of ZIO comes from the guarantees it gives us that are less obvious, for example that if a resource is acquired it will always be released or that if our program is interrupted all parts of it will immediately be shut down as quickly as possible.

The point is, these "laws" can't be encoded in the type system. Some of them, such as resource usage, likely could be with linear types. But we as users of the APIs expect that the laws hold.

## zio-prelude

### Subtype and Newtype

#### Differences between Subtype and extend

Me: 
> Other than being a 3-character change from Newtype in a refactor from Subtype (which is nice), 
> is there any advantage or important distinction between using the ZIO-style `object Foo extends Subtype[Bar]` vs `class Foo extends Bar`?

Adam Fraser:
> You can use Subtype for types that could not normally be extended like `object Natural extends Subtype[Int]`.
> Also Subtype allows the creator of the type to view any instance of the supertype as an instance of the subtype
> , since they have the same underlying representation, which otherwise would not be true in general.

AF's first point relates to classes declared `final`: we can't extend them directly, but we can `Subtype` them.
The second point can be achieved via a call to `wrap`, e.g. `Foo.wrap(val)`. Both of these are what we would expect
from `newtype` in Haskell.

#### Other References

1. Francis Toth's blogpost: [Newtype](https://contramap.dev/posts/2020-04-11-newtypes/) - Introduces this style of Newtyping.

## zio-test

ZIO Test, a toolkit for testing ZIO applications that we will discuss in a later
chapter, provides a `TestRandom` that does exactly this. In fact, ZIO Test provides
a test implementation of each of the standard services, and you can imagine
wanting to provide other implementations, as well.


We shouldn’t read too much into the environment signature, because it relies on discipline; we could do:

```scala
val int: ZIO[Any, Nothing, Int] = ZIO.effectTotal(scala.util.Random.nextInt())
```

Ideally this would have type `ZIO[Clock, Nothing, Unit]`, but we circumvent that.

As such, we consider the ability to “see” what capabilities a computation uses
as a secondary and optional benefit of using services. The primary benefit is
just being able to plug in different implementations in testing and production
environments.

This is much like in Haskell - pretty much anything can be achieved by using the `IO` type.
