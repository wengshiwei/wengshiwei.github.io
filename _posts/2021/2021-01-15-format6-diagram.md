---
layout: post
title: Understanding `format6` in OCaml by diagrams
comments: true
category:
- ocaml
- english
---

# 1. Background

It's a pity that I had totally missed Gabriel's post [The 6 parameters of (’a, ’b, ’c, ’d, ’e, ’f) format6][6 Parameters] written after reviewing the GADT implementation of format functions, before my previous [post on format6]({% post_url 2021/2021-01-13-format6 %}). I am reluctant to rewrite when many new topics pending in my mind. However, just after publishing the previous blog, I am thinking of how to make it clear and concise. This time, I use diagrams to explain `format6`.

# 2. Prelude

To simplify the examples, I make the format string `"%d %a"` fixed for printing and `"%d %r"` for scanning. There is only one type `foo` and one variant `Foo`. All the user-defined reader and printers are here: `scan_foo` `out_foo` `pp_foo` `string_of_foo`, to satisfy low_level devices.

```ocaml
# open CamlinternalFormatBasics

# type foo = Foo
type foo = Foo

# let scan_foo ic = Scanf.bscanf ic "%s" (function | "Foo" -> Foo | _ -> raise (Scanf.Scan_failure "Foo"))  
val scan_foo : Scanf.Scanning.scanbuf -> foo = <fun>

# let out_foo oc foo = Printf.fprintf oc "%s" (match foo with | Foo -> "Foo" )
val out_foo : out_channel -> foo -> unit = <fun>

# let pp_foo fmt foo = Format.fprintf fmt "%s" (match foo with | Foo -> "Foo")
val pp_foo : Format.formatter -> foo -> unit = <fun>

# let string_of_foo () = function | Foo -> "Foo"
val string_of_foo : unit -> foo -> string = <fun>
```

In this post, I just use four functions. They are:

```ocaml
# Scanf.sscanf
- : string -> ('a, 'b, 'c, 'd) Scanf.scanner = <fun>

# Printf.printf
- : ('a, out_channel, unit) format -> 'a = <fun>

# Format.fprintf
- : Format.formatter -> ('a, Format.formatter, unit) format -> 'a = <fun>

# Format.ksprintf
- : (string -> 'a) -> ('b, unit, string, 'a) format4 -> 'b = <fun>
```

The related `format` types are

```ocaml
# #show Scanf.scanner
type nonrec ('a, 'b, 'c, 'd) scanner =
    ('a, Scanf.Scanning.scanbuf, 'b, 'c, 'a -> 'd, 'd) format6 -> 'c

# #show format
type nonrec ('a, 'b, 'c) format = ('a, 'b, 'c, 'c) format4

# #show format4
type nonrec ('a, 'b, 'c, 'd) format4 = ('a, 'b, 'c, 'c, 'c, 'd) format6

# #show format6
type nonrec ('a, 'b, 'c, 'd, 'e, 'f) format6 =
    Format of ('a, 'b, 'c, 'd, 'e, 'f) fmt * string
```

I think a better way to think of `('a, 'b, 'c, 'd, 'e, 'f) format6` is to categorize type parameters (always indexed by `format6`) by who determines it.

First, the <span style='background-color:coral'>first argument</span> and the <span style='background-color:coral'>return type</span> of user-define functions (appearing in `'a` `'d`) are determined by <span style='background-color:coral'>printf and scanf</span> functions

For scanf functions:
- 1st `'a` is determined by format strings
- <span style='background-color:coral'>2nd 'b</span> is determined by <span style='background-color:coral'>scanf</span> functions
- 4th `'d` is determined by format strings
- <span style='background-color:LightCyan'>6th 'f</span>  is determined by the <span style='background-color:LightCyan'>return type</span> of receiver functions

The _receiver function_ always matches the format string. It's less interesting to argue which one is _the first impetus_. I just choose format strings to determine for simplicity.

For printf functions:
- 1st `'a` are determined by format strings
- <span style='background-color:coral'>2nd 'b</span> are determined by <span style='background-color:coral'>printf</span> functions
- <span style='background-color:coral'>6th 'f</span> are determined by <span style='background-color:coral'>printf</span>

What's more, for `k*printf` functtion:
- <span style='background-color:coral'>3th 'c</span> are determined by `k*printf`
- <span style='background-color:LightCyan'>6th 'f</span> are determined by the <span style='background-color:LightCyan'>return type</span> of the continuation function.

# 3. Scanf

```ocaml
# Scanf.sscanf
- : string -> ('a, 'b, 'c, 'd) Scanf.scanner = <fun>

# let fmt = format_of_string "%d %r" in let _ = Scanf.sscanf "42 Foo" fmt scan_foo (fun d r -> true) in fmt
- : (int -> foo -> bool, Scanf.Scanning.scanbuf, '_weak1,
     (Scanf.Scanning.scanbuf -> foo) -> (int -> foo -> bool) -> bool,
     (int -> foo -> bool) -> bool, bool)
    format6
=
Format
 (Int (Int_d, No_padding, No_precision,
   Char_literal (' ', Reader End_of_format)),
 "%d %r")
```

<div class="mermaid">
flowchart LR
  ssf[Scanf.sscanf]
  fmt["'%d %r'"]

  subgraph 4th-'d-format
    subgraph foo_reader
        scanbuf --> foo4[foo]
    end
    foo4 --> int4
    subgraph 5th-'e-format
      subgraph 1st-'a-format
        int4[int] --> foo4_2[foo]
        foo4_2 --> bool4_1[bool]
      end
      bool4_1 --> bool4_2
      subgraph 6th-'f-format
        bool4_2[bool]
      end
    end
  end

  subgraph Scanf
    ssf --> s["'42 Foo'"]
    s --> fmt
    fmt --> scan_foo
    scan_foo --> farg1
    subgraph receiver function
      farg1[fun d -> ] --> farg2[fun r -> ] --> true
    end
  end

  scan_foo -.-> foo_reader
  farg1 -.-> int4
  farg2 -.-> foo4_2
  true -.-> bool4_1
  true ---> bool

  classDef coral fill:coral
  class ssf coral
  class scanbuf coral
  class s coral

  classDef cyan fill:LightCyan
  class true cyan
  class bool cyan
  class bool4_1 cyan
  class bool4_2 cyan
</div>

The diagram shows that
- <span style='background-color:coral'>sscanf</span> determines the types of both input source <span style='background-color:coral'>"42 Foo"</span> and low-level device <span style='background-color:coral'>scanbuf</span> of `foo_reader`
- 4th type parameter `'d` matches the arguments after the format strings
- `'d` ends with `'a -> 'f`
- the <span style='background-color:LightCyan'>return type</span> of the receiver function determines the <span style='background-color:LightCyan'>return type</span> of `'d` ,`'f`, and even <span style='background-color:coral'>Scanf.sscanf</span>

# 4. Printf

```ocaml
# Printf.printf
- : ('a, out_channel, unit) format -> 'a = <fun>

# let fmt = format_of_string "%d %a" in let _ = Printf.printf fmt 42 out_foo Foo in fmt
42 Foo
- : (int -> (out_channel -> foo -> unit) -> foo -> unit, out_channel,
     unit, unit, unit, unit)
    format6
=
Format
 (Int (Int_d, No_padding, No_precision,
   Char_literal (' ', Alpha End_of_format)),
 "%d %a")
```

<div class="mermaid">
flowchart LR
  u[unit]
  u1[unit]
  u2[unit]
  foo1[foo]
  foo2[foo]

  subgraph 1st-'a-format
    int --> oc[out_channel]
    subgraph foo_printer
        oc --> foo1 --> u1
    end
    u1 --> foo2 --> u2
    subgraph 6th-'f-format
      u2
    end
  end

  subgraph Printf
    ppf[Print.printf] --> fmt['%d %a'] --> 42 --> out_foo --> Foo
  end

  42 -.-> int
  out_foo -.-> foo_printer
  Foo -.-> foo2
  Foo ---> u

  classDef coral fill:coral
  class ppf coral
  class u coral
  class u1 coral
  class u2 coral
  class oc coral
</div>

The diagram shows <span style='background-color:coral'>printf</span> determines the type of <span style='background-color:coral'>out_channel</span> and <span style='background-color:coral'>unit</span> appearing in 1st type parameter `'a` and 6th type parameter `'f`.

# 5. Format

```ocaml
# Format.fprintf
- : Format.formatter -> ('a, Format.formatter, unit) format -> 'a = <fun>

# let fmt = format_of_string "%d %a" in let _f fmter = Format.fprintf fmter fmt 42 pp_foo Foo in fmt
- : (int -> (Format.formatter -> foo -> unit) -> foo -> unit,
     Format.formatter, unit, unit, unit, unit)
    format6
=
Format
 (Int (Int_d, No_padding, No_precision,
   Char_literal (' ', Alpha End_of_format)),
 "%d %a")
```

<div class="mermaid">
flowchart LR
  u[unit]
  u1[unit]
  u2[unit]
  foo1[foo]
  foo2[foo]

  subgraph 1st-'a-format
    int --> oc[Format.formatter]
    subgraph foo_printer
        oc --> foo1 --> u1
    end
    u1 --> foo2 --> u2
    subgraph 6th-'f-format
      u2
    end
  end

  subgraph Printf
    fpf[Format.fprintf] --> fmter --> fmt[%d %a] --> 42 --> out_foo --> Foo
  end

  42 -.-> int
  out_foo -.-> foo_printer
  Foo -.-> foo2
  Foo ---> u

  classDef coral fill:coral
  class fpf coral
  class fmter coral
  class ld coral
  class oc coral
  class u coral
  class u1 coral
  class u2 coral
</div>

The diagram for `Format.fprintf` doesn't differ much with `Printf.printf`. <span style='background-color:coral'>fprintf</span> determines the type of <span style='background-color:coral'>fmter</span>, <span style='background-color:coral'>Format.formatter</span>, and all <span style='background-color:coral'>unit</span>s.

# 6. kprintf

```ocaml
# Format.ksprintf
- : (string -> 'a) -> ('b, unit, string, 'a) format4 -> 'b = <fun>

# let fmt = format_of_string "%d %a" in let _ = Format.ksprintf (fun _s -> true) fmt 42 string_of_foo Foo in fmt
- : (int -> (unit -> foo -> string) -> foo -> bool, unit, string, string,
     string, bool)
    format6
=
Format
 (Int (Int_d, No_padding, No_precision,
   Char_literal (' ', Alpha End_of_format)),
 "%d %a")
```

<div class="mermaid">
flowchart LR
  ou[unit]
  foo1[foo]
  foo2[foo]

  subgraph 1st-'a-format
    int --> ou
    subgraph foo_printer
        ou --> foo1 --> string
    end
    string --> foo2 --> bool
    subgraph 6th-'f-format
      bool
    end
  end

  subgraph Printf
    fksp[Format.ksprintf] --> farg
    subgraph kont function
      farg[fun _s ->] --> true
    end
    true --> fmt[%d %a] --> 42 --> string_of_foo --> Foo
  end

  42 -.-> int
  string_of_foo -.-> foo_printer
  Foo -.-> foo2

  classDef coral fill:coral
  class fksp coral
  class ou coral
  class string coral
  class ld coral
  class string3 coral
  class farg coral

  classDef cyan fill:LightCyan
  class true cyan
  class bool cyan
</div>

The diagram shows <span style='background-color:coral'>Format.ksprintf</span> determines the type of the <span style='background-color:coral'>input</span> of the continuation and <span style='background-color:coral'>two heads</span> of foo_printer. The <span style='background-color:LightCyan'>return types</span> of the continuation determines 6th type parameter <span style='background-color:LightCyan'>'f</span>.

# 7. Others

Most time of this blog is spent on taming [mermaid](https://mermaid-js.github.io/mermaid/#/), the diagram library. You can still see unsolved artifacts like single quotes for strings, diagrams without titles, and unhappy overlappings. The choice of dot lines or color nodes for presenting connections (determining), is to reduce total lines and to keep the layout engine happy. I had (nicer) hand-painting diagrams on iPad before these two posts on `format6`.

```quote
The catch-22 of Diagrams and Charts:
Diagramming and charting is a gigantic waste of developer time, but not having diagrams 
ruins productivity.
-- mermaid
```

# Reference

[6 Parameters]: http://gallium.inria.fr/blog/format6/ "The 6 parameters of (’a, ’b, ’c, ’d, ’e, ’f) format6"
[mermaid]: https://mermaid-js.github.io/mermaid/#/

- The 6 parameters of (’a, ’b, ’c, ’d, ’e, ’f) format6 <http://gallium.inria.fr/blog/format6/>
- Mermaid document <https://mermaid-js.github.io/mermaid/#/>