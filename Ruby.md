# Ruby

- Parenthesis optional for nullary function invocation
- `def hi(name) puts "Hello #{name}"
- Methods, classes, blocks terminate with `end`
- Instance variables marked with `@` prefix
- Constructors invoked with `.new()` method on ClassName
- Methods reflection 
  - `ClassName.instance_methods(includeAncestors)`
  - `.respond_to?`
- `["a", "b"].each do |name| puts "Hello #{name}" end
- `attr_accessor :v1, :v2` defines `@v1`, `@v2`, `.v1`, `.v2`, `.v1=`, `.v2=` methods
  - also `attr_reader` or even `attr_writer`

## Syntax

- Sigils
  - `$` -- global variables
  - `@` -- instance variables
  - `@@` -- class variables
- `&.`
  - `if obj&.foo&.bar`  vs `if obj && obj.foo && obj.foo.bar`
- Line breaks and semicolons terminate statements
  - Blocks surrounded by `{}` or `do end`
  - Block arguments surrounded by `||`, and comma-delimited
- Strings delimited by single or double quotes
- Symbols start with `:` -- useful for keys
- Uppercase variables are "constants"
- Method names may contain `?` and `!`
  - `s.gsub!(from, to)`
- Instance methods invoked with `.`
  - Class methods invoked with `:`
    - `::methods`, `::class_variables`, `::constants`
  - Invoking "kernel" methods don't require an object or class
- Ranges 
  - Closed -- `('a'..'z')` 
  - Half open -- `(0...256)`
  - One may modify strings by assigning to a range -- `s[2..4] = "newSubstring"`
- Arrays surrounded by `[]`, and comma-delimited
  - Dictionaries surrounded by `{}`. Keys separated by `=>` from values. Comma-delimited pairs.
- Regexps surrounded with `//`
- Operators: `*` `!` `~` `*` `/` `%` `+` `-` `&` `<<` `>>` `|` `^`  `>` `>=` `<` `<=` `<=>` `||` `!=` `=~` `!~` `&&` `+=` `-=` `==` `===` `..` `...` `not` `and` `or`
  - `.[]` -- Shortcut for indexing method
  - `<<` -- Concatenation, e.g. `msg << "And something else"`
  - `===` -- More flexible comparison, e.g. `(0..10) === 5`
  - `<` -- Inheritance, e.g. `class MyClass < MySuperClass;`
- Keywords: `alias` `and` `BEGIN` `begin` `break` `case` `class` `def` `defined` `do` `else` `elsif` `END` `end` `ensure` `false` `for` `if` `in` `module` `next` `nil` `not` `or` `redo` `rescue` `retry` `return` `self` `super` `then` `true` `undef` `unless` `until` `when` `while` `yield`
  - Trinary if/else/end
  - `.nil?` is a useful method
  - `case when when when else end` expression
  - `include` and `require` are global methods -- not keywords!
  - `raise` accepts the class of exception and arguments thereof. `rescue` catches exceptions. `ensure` is a "finally" block. `retry` reenters at `begin`
  - `next` is like "continue"
  - `self` is like "this", but at least `super` is "super"
  - `module` contains variables and classes
    - Of course one cannot instantiate a module
- `p` is like print with implied .toString

## Semantics (similar to Smalltalk)

- Every value is an object, including numbers, booleans, and `null`
  - Rational numbers, complex numbers, arbitrary-precision 
- Every function is a method
  - Top-level functions are methods on `Object` class
  - `.class` method on all objects
  - Methods return the last statement
- Inheritance with dynamic dispatch
- Mixins
- Singleton methods (defined for single instance)
- Anonymous functions
- Lexical closures
- Continuations, iterators, generators
- Introspection, reflection, metaprogramming
- Statements are expressions. Declarations are executed imperatively.
  - Just-in-time compilation
- Default arguments
  - Varargs with `*` ("splat")
- Threads, GC, Exceptions
- Only `false` and `nil` are false
- Operator overloading
- Unicode and encodings
- C API
- RubyGems package management

## Examples

- `5.times { print "Something" }`
- `exit unless "restaurant".include? "aura"`
- `['toast', 'cheese', 'wine'].each { |food| print food.capitalize }`
