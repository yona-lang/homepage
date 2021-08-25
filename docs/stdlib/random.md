---
title: "Random"
tags: stdlib
---

Random number generation. Not cryptographically secure.

## Usage
Use functions for different types of random values.

### Random integer from a range
Function `integer_lt` takes an integer as an argument and generates a random number from 0(inclusive) to this upper limit(exclusive).

```haskell
Random::integer_lt 5  # 0, 1, 2, 3 or 4
```
