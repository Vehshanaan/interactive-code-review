# Example: reviewing a retry loop

This is an illustrative, constructed example, not a transcript of a real user test.

## Source (Python)

```python
def send_with_retry(send, payload, max_attempts=3):
    for attempt in range(max_attempts):
        try:
            return send(payload)
        except TimeoutError:
            if attempt == max_attempts - 1:
                raise
```

## Initial walkthrough

`send_with_retry` calls the supplied `send` function with a payload, retrying on a timeout up to the configured attempt count (three by default). A successful return exits immediately. If the last attempt times out, the exception propagates. Other exceptions are not caught.

## Reader feedback (illustrative)

- Does a timeout mean the server did not receive the payload?
- Does the default mean three retries or three total attempts?
- What happens if max_attempts is zero?

## Evolved walkthrough (same document, revised in place)

`send_with_retry` wraps a caller-provided `send` function. The loop runs at most `max_attempts` times: the default is three total attempts, i.e. one initial attempt and at most two retries.

Each iteration calls `send(payload)`. If it returns normally, the wrapper immediately returns that value. If it raises `TimeoutError`, the wrapper retries unless this was the final iteration, in which case the original exception propagates. Other exceptions propagate immediately.

A timeout does **not** establish that the remote operation failed. For example, the remote service might apply the operation and then fail to deliver its response. Retrying could duplicate a side effect unless the operation is idempotent or another deduplication mechanism exists. This wrapper does not implement such a mechanism; whether one exists in `send` or the remote system remains unverified.

The loop also assumes a positive integer attempt count. With zero or a negative integer, `range(max_attempts)` is empty and the function implicitly returns `None` without calling `send`. The wrapper does not validate this input. Whether that is an acceptable contract depends on its callers.

## What changed?

The control-flow explanation stayed short. The reader's questions caused targeted expansion of retry semantics, remote side effects and the input boundary. The result integrates the answers instead of retaining a Q&A log, and it distinguishes confirmed behavior from an unresolved system-level risk. The reviewed source is unchanged.
