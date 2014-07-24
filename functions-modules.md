# Functions and Modules

## Anonymous Functions

Anonymous functions are closures and will capture their surrounding scope. The
following function definitions are all equivalent.

```elixir
sum = fn (a, b) -> a + b end
sum = fn a, b -> a + b end    # Parens are optional.
sum = &(&1 + &2)              # Using the capture operator &.
```

E.g. with guard clauses:

```elixir
sum = fn (a, b)
  when is_number(a) and is_number(b) -> a + b
  when is_list(a) and is_list(b) -> a ++ b
end
```

Calling our anonymous function:

```elixir
sum.(1, 2)
sum.([1, 2], [3, 4, 5])
```

Anonymous functions are useful for passing functions as arguments to other
functions.

## Named Functions and Modules

Named functions are not closures. They must be written inside of modules.
Modules are used to encapsulate and structure our code. By convention, we would
write a module `BasicMath` in a file named `basic_math.ex`.

```elixir
defmodule BasicMath do
  def sum(a, b), do: a + b
end
```

The `def` keyword is used to define named functions. We can also use the `defp`
keyword for private funtions that cannot be accessed from outside of our
module.

We can call our sum function like so:

```elixir
BasicMath.sum(1, 2)
BasicMath.sum 1, 2   # Parens are optional when calling named functions.
sum 1, 2             # While the module is in scope.
```

We refer to the sum function as `BasicMath.sum/2`, 2 being its arity. We are
allowed to create another sum function with the same name but with different
arity.

Another way to write a named function is on multiple lines:

```elixir
def sum(a, b) do
  a + b
end
```

E.g. with guard clause:

```elixir
def sum(a, b) when is_list(a) and is_list(b) do
  a ++ b
end
```

The function's return value is the result of the last evaluated expression,
`a + b`.

### Default Values for Arguments

Named functions can specify default parameter values using `\\`.

```elixir
def join(s1, s2, joiner \\ " "), do: s1 <> joiner <> s2

join("hello", "world")       # "hello world"
join("hello", "world", "-")  # "hello-world"
```

### Capturing a Function

We can capture a function using the `&` operator.

```elixir
add = &BasicMath.sum/2
result = add.(1, 2)

# result equals 3.
```

### Inline Documentation
We can document our module using `@moduledoc`, `@doc`, `@spec` (parameter and
return types) and `@vsn` (version number).

```elixir
defmodule BasicMath do
  @moduledoc """
  A simple module example.
  
  Multi-line documentation can be written between the two sets of triple double
  quotes. We can also use markdown syntax here.
  """
  @vsn "0.1"

  @doc "Calculate the sum of the arguments"
  @spec sum(number(), number()) :: number()
  def sum(a, b), do: a + b
end
```

### Using Erlang Modules

We can access Erlang modules from Elixir. For example, we can use the `sqrt/1`
function in Erlang's `math` module like so:

```elixir
:math.sqrt(4)
```

## The Pipe Operator

The pipe operator `|>` takes the result of the expression on its left and
inserts it as the first parameter of the function invocation on its right.

```elixir
six_times = (1..10)
  |> Enum.map(&(&1 * 3))              # Multiply each element by 3.
  |> Enum.filter(&(rem(&1, 2) == 0))  # Pick only even numbers.

# six_times = [6, 12, 18, 24, 30]
```

The above is equivalent to
```elixir
six_times = Enum.Filter(Enum.map(1..10, &(&1 * 3)), &(rem(&1, 2) == 0))
```

