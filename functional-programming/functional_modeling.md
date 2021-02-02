- Encapsulation is not broken because of Pattern Matching because is merely a way to tear down a immutable structure

- Pattern matching works on `unnaply` method that is given by free when you declase a `final case class`

-
```
  final case class RequestTyped[Body](body: TypedEvent[Body], sender: String)
  final case class RequestTypedSecondOption[A](body: TypedEvent[A], sender: String)
```

Between those two option the latter is less complex and often prefered because you care about less structure

```
  final case class RequestTyped[A, Body <: Event[A]](body: Body, sender: String)
```

This option pays a lot of taxes and constraints to much the modeling and make it more difficult to work with. We don't gain anything with this.

Those constraints you can put it very very down when you need it; very on the edge. And this is very advanced technique:


```
 final case class Request[A](body: A, sender: String) {
    def time[B](implicit ev: A <:< Event[B]): java.time.Instant = 
      ev(body).time
  }
}
```

Which means that `time` could be only called when `A` parameter has certain defined structure


## Sum types

with `sealed traits` you avoid to add more terms

Either[Nothing, Int] isometring Either[String, Int]

## Recursive types

Recursive types happen on sum types wich give us the hability to cerate finite values

```
final case class Foo(foo: Option[Foo])
```

The Sum type here is the `Option` which gives you the hability to terminate

Always defer decissions, like for example throw an exception in a smart constructor.

The return type of an smart constructor decide for a type that carry the less
information like `Option[A]`. If you decide for `Try[]` then you have
Throwable or A and Throwable could be infinite, not like None

https://www.slideshare.net/normation/nrm-scala-iocorrectlymanagingerrorsinscalav13

## Algebra

Why a value that is Nothing could be returned as any type:

```
def something(val a: Nothing): String = a
```
?

// for all types A: A <: Any
// for all types A: Nothing <: A (Nothing is a subtype is for all types)

## Some patterns

```
trait Identified[A] {
    def idOf(a: A): java.util.UUID 
  }
  def idOf[A](a: A)(implicit identified: Identified[A]): java.util.UUID = identified.idOf(a)

  // printId(identified: Identified)
  def printId[A: Identified](a: A): Unit = 
    println(idOf(a))

``


