Minima
======

Minima is an experimental interpreter written in Go (the language is called the same).
We needed a way to massage our JSON data with a scripting language.

The syntax (or the lack of it) is inspired by Lisp, to be easy to parse for machines.
However, I tried to get rid of the zillions of parentheses to be easy to parse for humans too.
With significant whitespace and indentation, the outermost parentheses are there, but they are kinda transparent.

Everything is subject to change.

### General example
```go
-- This is a comment
set n (+ 2 1)
set x 8
if (< n x)
	run
		println "Multiline if."
		println "n is smaller than " x
	println "n is greater or equal than " x
for n
	println "n is " n
```

This above snippet will produce:

```
Multiline if.
n is smaller than 8
n is 3
n is 3
n is 3
```

### Newline shorthand
One can use the ";" as a shorthand for a newline with same indentation:
```go
set a 10; set b 20
```

### Function definition and call
```go
set k 10
func l (u) (run
	println k
	+ u u u u)
println (l 20) 
println u
```

Produces:
```
10
80
<nil>
```

### Closures
```go
func l (u) (run
	set m 9
	func (v) (+ v u m))
set p (l 10)
println (p 30)
println (p 40)
```

Produces:
```
49
59
```

### Recursion
```go
func fib (x)
	if (| (eq x 0) (eq x 1))
		get x
		+ (fib (- x 1)) (fib (- x 2))
println (fib 15)
```

Produces:
```
610
```

### Panic/recover
```go
func k (panic "OMG")
func f (run
	recover (run(println "Recovering from " prob) (+ 1 1))
	println "Panicking in next function call."
	k
	println "This shall not run.")
func l (run
	println "Just a casual println..."
	set ret (f)
	println "This shall run."
	get ret)
println (l)
println "Recovered"
```

Produces:
```
Just a casual println...
Panicking in next function call.
Recovering from OMG
This shall run.
2
Recovered
```

### Defer
```go
func f (run
	defer (println 0)
	defer (println 1)
	println "This shall run before the deferred functions.")
f
```

Produces:
```
This shall run before the deferred functions.
1
0
```

Goals
======
- Create a language in pure Go.
- Create a scripting language which is statically typed.

Latest additions
======
- Defer
- Panic/Recover
- Better recursion support.
- Eq, |, & operators.
- Variable scoping, functions, closures.

Roadmap
======
- Defer
- []interface{} and map[string]interface{} types to be able to handle JSON data.
- More syntactic sugar (expect some neat things here).
- More builtin goodies.
- Static typing.
- Packages.
- A mongodb driver ;)
- Optimizations.