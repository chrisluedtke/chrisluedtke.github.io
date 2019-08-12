---
title: Fibonacci Sequence
subtitle: A web application to explore earthquake events, prediction, and impact
forward_url: http://www.quakespect.com/
tags:
- algorithms
---

`o(2 ^ n)` runtime complexity.
```python
def fib(n):
    if n < 2:
        return n
    else:
        return fib(n - 1) + fib(n - 2)
```

`o(n)` runtime complexity.
```python
def fib(n, cache=dict()):
    if n < 2:
        return n
    elif n in cache:
        return cache[n]
    else:
        cache[n] = fib(n - 1, cache) + fib(n - 2, cache)
        return cache[n]
```

```python
```

```python
```

```python
```