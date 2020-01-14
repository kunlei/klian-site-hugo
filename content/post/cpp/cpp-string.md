---
title: C++ - string
date: 2020-01-14
draft: false
tags: ["c++"]
---

The `string` is a library class defined by C++ to represent and manipulate characters.
It's defined in header `string` and included in namespace `std`:

```cpp
#include <string>
using std::string;
```

We'll look at some of the most useful operations for string.

## Creation

There exist a number of constructors for string.

```cpp
string(); // default constructor
string(const string &str); // copy constructor
string(const char * c); // c-string constructor
string(size_t n, char c); // fill constructor
```

```cpp
string a; // define empty string using default constructor
string b(a); // copy construction
string c("hello world"); // c-string constructor
string d(10, d); // fill construction
```

## Access

The library defines a number of iterators to iterate through a string object.

```cpp
string s("hello word");
for (auto itr = s.begin(); itr != s.end(); ++itr) {
  cout << *itr << endl;
}
```

Elements in a string can also be accessed directly.

```cpp
string s(10, 'a');
cout << s[1] << endl;
cout << s.at(2) << endl;
```

Other available iterators include `rbegin()`, `rend()`, `cbegin()`, `cend()`, `crbegin()` and `crend()`.

## Capacity

We can use functions to inquiry the capacity of a string.

```cpp
string s("string");
cout << s.size() << endl;
cout << s.empty() << endl;
```

## `getline()`

`getline()` is a useful function to retrieve string from input stream.

```cpp
std::istream& getline(std::istream& istrm, string& str);
```

For example, the following code reads one line from the input stream and saves it into a string.

```cpp
string s;
getline(cin, s);
cout << s << endl;
```
