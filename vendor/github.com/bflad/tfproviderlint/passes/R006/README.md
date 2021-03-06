# R006

The R006 analyzer reports when `RetryFunc` declarations are missing retryable errors (e.g. `RetryableError()` calls) and should not be used as `RetryFunc`.

## Flagged Code

```go
err := resource.Retry(1 * time.Minute, func() *RetryError {
  // Calling API logic, e.g.
  _, err := conn.DoSomething(input)

  if err != nil {
    return resource.NonRetryableError(err)
  }

  return nil
})
```

## Passing Code

```go
_, err := conn.DoSomething(input)

if err != nil {
  return err
}

// or

err := resource.Retry(1 * time.Minute, func() *RetryError {
  // Calling API logic, e.g.
  _, err := conn.DoSomething(input)

  if /* check err for retryable condition */ {
    return resource.RetryableError(err)
  }

  if err != nil {
    return resource.NonRetryableError(err)
  }

  return nil
})
```
