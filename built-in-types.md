# Built-in Types

## Value Types

 * Integers can be any size, with no maximum value. E.g. `42`, `1_000_000`,
   `063` (octal), `0xf4d` (hex), `0b1010` (binary)
 * Floating point numbers are IEEE 754 double precision
 * Atoms are constants that represent the name of something.
   The name is the value. E.g. `:message`, `:error`.
   Atoms are useful for tagging values.
 * Regular expressions have the syntax `~r{regexp}opts`
 * Ranges, e.g. `1..10` (includes 1 and 10)

## Collection Types

Elixir has the following built-in collection types:

 * Tuples
 * Records
 * Lists (and keyword lists)
 * Maps
 * Binaries

### Tuples

Tuples are ordered collections of values. E.g. `{:ok, 201, "Asset created"}`

### Records

Records are tuples with named fields.

```elixir
# Defining a record.
defrecord User, first_name: nil, twitter: nil

# Creating a user.
user = User.new first_name: "Vy-Shane", twitter: "@vyshane"

# Accessing fields using dot notation.
user.first_name  # "Vy-Shane"

# Accessing fields via pattern matching.
User[first_name: name, twitter: "@vyshane"] = user  # name = "Vy-Shane"

# Updating a field.
user.twitter("@vy")  # Returns a new record with the updated field.
```

`defrecord` works by creating a module for each record type. Elixir
automatically adds functions into the module that let you create new records,
access and update record values.

### Lists

Lists are immutable and may either be empty `[]` or have a head and a tail.

E.g. for the list literal `[1, 2, 3]`, the head is `1` and the tail is the list
`[2, 3]`:

```elixir
[head | tail] = [1, 2, 3]

# head = 1
# tail = [2, 3]
```

Single-quoted 'string' literals are character lists:

```elixir
'dog' = [100, 111, 103]
```

### Keyword Lists

`[x: 100, y: 0]` is syntatic sugar for `[{:x, 100}, {:y, 0}]`

We can leave off the square brackets if a keyword list is the last argument in
a function call.

We can also leave off the brackets if a keyword list appears as the last item
in any context where a list of values is expected.


### Maps

Maps are collections of key/value pairs. Keys are unique and can be of any
type. E.g.

```elixir
states = %{"WA" => "Western Australia", "NSW" => "New South Wales"}
user = %{:first_name => "Vy-Shane", :twitter => "@vyshane"}
```

If the key is an atom, we can use a similar shortcut as for keyword lists. We
can also access elements using dot notation.

```elixir
user = %{first_name: "Vy-Shane", twitter: "@vyshane"}
user.first_name
```

Otherwise we use bracktet notation.

```elixir
states['WA"]
```

To update a map:

```elixir
alter_ego = %{user | twitter: "@vy"}
```

### Structs

A struct is a module that wraps a limited form of a map.

A struct is used when we need a typed map with a fixed set of fields. The keys
must be atoms. We can pattern match on a struct by type as well as content.

E.g.

```elixir
defmodule StockItem do
  defstruct [:name, :description, :price, :in_stock]

  # Since this is a module, we can add related functionality here.
end
```

We can also define default values for each field.

```elixir
defmodule StockItem do
  defstruct name: "", description: "", price: 0, in_stock: false
end
```

```elixir
pen = %StockItem{
  name: "Lamy 2000",
  description: "Makrolon Body",
  price: 159_00,
  in_stock: true
}

pen.name  # "Lamy 2000"
sold_out = %StockItem{pen | in_stock: false}
```

### Binaries

Binaries are sequences of bytes, e.g. `<<1, 2, 3>>`. Strings are UTF-8 encoded
binaries in Elixir and string literals are written like this: `"a string"`.

## System Types

 * Pids
 * Ports

## Special Values

 * `true`
 * `false`
 * `nil`, the absence of something

Anything other than `false` or `nil` is truthy.

