
# Arrays

## Array Slices

> Appending to the parent array [may or may not make it independent](https://github.com/vlang/v/blob/master/doc/docs.md#array-slices) from its child slices. The behaviour depends on the parent's capacity and is predictable:

```v
mut a := []int{len: 5, cap: 6, init: 2}
mut b := a[1..4]
a << 3
// no reallocation - fits in `cap`
b[2] = 13 // `a[3]` is modified
a << 4
// a has been reallocated and is now independent from `b` (`cap` was exceeded)
b[1] = 3 // no change in `a`
println(a) // `[2, 2, 2, 13, 2, 3, 4]`
println(b) // `[2, 3, 13]`
```

This might be surprising from one standpoint, but is actually more functional in nature, *if* we only uses mutable slices locally, or immutable slices otherwise.

# Filter and Map

`filter` and `map` are "compiler magic".  You can't add anything else like them unless you modify the compiler.
 Alex M: 
 > yes they need to be compiler magic in order to have optimizations
 > otherwise filter/map chains would be very inefficient

# Maps

Accessing non-existent keys in a map appears to recursively [fill in the "0" values](https://github.com/vlang/v/blob/master/doc/docs.md#maps) for the primitive types.
```v
fn main() {
  numbers := map{
    'one': Point{1, 1}
    'two': Point{2, 2}
  }
  println(numbers['bad_key'])
}

struct Point {
	x int
	y int
}
```

```
$ v run hello.v 
Point{
    x: 0
    y: 0
}
```

# Modules

## Init functions

If you want a module to automatically call some setup/initialization code when it is imported, you can use a [module init function](https://github.com/vlang/v/blob/master/doc/docs.md#init-functions):

```v
fn init() {
	// your setup code here ...
}
```

The `init` function cannot be public - it will be called automatically. This feature is particularly useful for initializing a C library.

# Purity

V is not actually pure when it comes to IO, as it notes in the docs. For exampe, this is valid:

```v
fn get_input() string {
  return os.input('Enter a string')
}
```


# Libraries

## Data

- [viter](https://github.com/mcastorina/viter) 
	- > I wrote a pure V implementation of iterators awhile back if you're interested. It uses code generation because at the time generics weren't powerful enough. I'm still waiting for generic interfaces to be supported to rewrite it.
