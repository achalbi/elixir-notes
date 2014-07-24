# Error Handling

In Elixir, we tend to return tuples, e.g. `{:ok, result}`/`{:error, message}`,
from functions to let the caller know whether an operation was successful or
not. The caller then uses pattern matching to handle the possible outcomes of
calling our function.

Exceptions are usually reserved for truly exceptional events. We also often
prefer to "let it crash", meaning that we don't handle exceptions, instead
preferring to to let a supervisor process take care of recovering from crashes.

## Exceptions

### Try and Rescue

While exceptions are rare, should we want to handle them, we can use the
`try/rescue` construct. E.g. usage:

```elixir
try do
  # Do stuff that might fail.
rescue
  ArithmeticError -> {:error, "Example arithmetic failure"}
  CaseClauseError -> {:error, "Example of case not matching"}
after
  # Perform any necessary cleanups.
end

```

### Throw and Catch

`throw` and `catch` are reserved for situations where it is not possible to
retrieve a value unless by using throw and catch.

```elixir
try do
  # Do stuff that might fail.
  throw(something)  # Opps, a bad thing happened, we throw something.
catch
  something -> "Got #{something}"
after
  # Perform any necessary cleanups.
end
```

Variables defined inside `try/catch/rescue/after` blocks do not leak to the
outer context.

## Logging Errors

We can log events using `:error_logger.info_msg/1`,
`:error_logger.warning_msg/1`, and `:error_logger.error_msg/1`.

## Tracing

Use the `:dgb` module to trace messages between processes as well as function
calls.

