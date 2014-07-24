# Flow Control Structures

## if and unless

The `if` and `unless` macros are useful when you need to check for just one
condition.

The following are equivalent `if` control structures.

```elixir
if 1 == 1, do: "true part", else: "false part"
```

```elixir
if 1 == 1 do
  "true part"
else
  "false part"
end
```

The following are equivalent `unless` control structures.

```elixir
unless 1 == 2, do: "true part", else: "false part"
```

```elixir
unless 1 == 2 do
  "true part"
else
  "false part"
end
```

## cond

`cond` allows us to check different conditions and find the first one that is
truthy.

```elixir
cond do
  2 + 2 == 5 ->
    "This is never true"
  2 * 2 == 3 ->
    "Nor this"
  true ->
    "This is always true (equivalent to else)"
end
```

## case

`case` allows us to test a value against many patterns until we find a
matching one. The pattern may include guard clauses.

```elixir
x = 1

case {1, 2, 3} do
  {4, 5, 6} ->
    "This clause won't match"
  {1, y, 3} when y > 2 ->
    "This clause will match and bind y to 2 in this clause"
  {^x, 2, 3} ->
    "The ^ operator allows matching against an existing variable."
  _ ->
    "This clause would match any value"
end
```

