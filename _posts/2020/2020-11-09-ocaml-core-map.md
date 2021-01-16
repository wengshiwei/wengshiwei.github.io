---
layout: post
title: Two map modules in OCaml Core (or one difference between Base and Core)
comments: true
category:
- ocaml
- english
---

In OCaml Core (or Base), the recommended way to use create a map for a type is to make a module for that type, then to create a map for that module

```ocaml
module Foo = struct
  module T =
  struct type t = int * int [@@deriving sexp, compare] end
  include T
  include Comparable.Make(T)
end
```

This code behaves different in Base and Core due to `Comparable`.

In Base, `Comparable`(v0.14.0 [doc](https://ocaml.janestreet.com/ocaml-core/latest/doc/base/Base/Comparable/index.html) [source](https://github.com/janestreet/base/blob/master/src/comparable.ml))) generates comparison and sexp related functions. In Core, `Comparable`(v0.14.0 [doc](https://ocaml.janestreet.com/ocaml-core/latest/doc/core_kernel/Core_kernel/Comparable/index.html) [source](https://github.com/janestreet/core_kernel/blob/master/src/comparable.ml)) generates extra module `Foo.Set` and `Foo.Map` with connection to `*_binable` (_binary-serializable_).

This can explain why the compile complains about this code after trying `open Foo`:

```ocaml
let play_foo set =
  let open Foo in
  ...
  Set.add set foo
  ...
```

`Set`, which supposes to be `Core.Set` is shadowed by `Foo.Set` after `open Foo`. Unfortunately, `Foo.Set` is incompatible with `Core.Set.M(Foo)`. The parameter `set` is usually the latter. That's the reason the compile complains and I wrote this blog in the end.

```ocaml
utop # Foo.Set.empty;;
- : Foo.Set.t = <abstr>
utop # Set.empty (module Foo);;
- : (Foo.t, Foo.comparator_witness) Set.t = <abstr>
```

The solution is easy. One can use `Core.Set` instead of `Set` to avoid shadowing, or to use `Hashtbl` if it's [suitable](https://dev.realworldocaml.org/maps-and-hashtables.html#hash-tables).

p.s.

The first code snipper contains `[@@deriving sexp ...]`. Both doc linked in the blog use `deriving sexp`. The section [Maps and Hash Tables](https://dev.realworldocaml.org/maps-and-hashtables.html#hash-tables) in *Real World OCaml* demonstrates the usage of `sexp_of`. Under my testing, when using this idiom in `Base`, `deriving sexp_of` is sufficient, but when using it in `Core`, one must use `deriving sexp` since `t_of_sexp` is required in `Core.Comparable.Make`. Or you will see the error message like this:

```ocaml
utop # 
module Foo = struct
  module T =
  struct  type t = int * int [@@deriving sexp_of, compare]  end
  include T
  include Comparable.Make(T)
end;;
Line 5, characters 26-27:
Error: Signature mismatch:
       ...
       The value `t_of_sexp' is required but not provided
       File "src/sexpable.ml", line 4, characters 2-29: Expected declaration
```