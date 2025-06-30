---
title: Search Patterns
sidebar_position: 6
---

Tv uses a fuzzy matching algorithm to filter the list of entries. Its behavior depends on the input pattern you provide.

| Matcher | Pattern |
| --- | :---: |
| Fuzzy | `foo` |
| Substring | `'foo` / `!foo` to negate |
| Prefix | `^foo` / `!^foo` to negate |
| Suffix | `foo$` / `!foo$` to negate |
| Exact | `^foo$` / `!^foo$` to negate |

These patterns (and matchers) can be associated to express complex search queries such as:
```
car 'bike !^car !bike$
```
which translates to anything that:
- fuzzy matches `car`
- contains the exact substring `bike`
- does not start with `car`
- does not end with `bike`

And will produce the following results:
| haystack | match | explanation |
| :------- | :---: | :------: |
| *the car drove past the bike* | ❌ | ends with bike |
| *car, bike or bus?* | ❌ | starts with car |
| *the black motorbike flew past the tourists* | ✅ | |
| *the motorbike flew past the tourists* | ❌ | doesn't contain 'car' |


For more information on the matcher behavior, see the
[nucleo-matcher](https://docs.rs/nucleo-matcher/latest/nucleo_matcher/pattern/enum.AtomKind.html) documentation.