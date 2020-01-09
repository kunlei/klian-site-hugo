---
title: C++ - Exceptions
date: 2020-01-12
draft: false
tags: ["c++"]
---

Any program ought to have mechanism to deal with run time errors, for which C++ use exception handling technique to throw and process.
The exception handling process consists of three components:

- A `throw` expression that indicates an error occurs.
- A `try` block to encompass the statements that might throw exceptions.
- One or more `catch` clauses to address exceptions.

```cpp
try {
  throw runtime_error("error");
} catch (runtime_error err) {
  std::cout << err.what() << std::endl;
}
```

There are mainly four header files that C++ define exception classes:

- The `exception` header: it defines the `exception` class.
- The `stdexcept` header: it defines various general-purpose exception classes:
  - `exception`
  - `runtime_error`
  - `range_error`
  - `logic_error`
  - `out_of_range`
  - and others
- The `new` header: it defines the `bad_alloc` exception class.
- The `type_info` header: it defines the `bad_cast` exception class.
