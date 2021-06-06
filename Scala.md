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
