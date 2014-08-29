Recent version of V8 started to implement ES6 features and thus have become available in recent versions of Node, with a watershed moment when [ES6 generator functions](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-14.4) [have become](http://wingolog.org/archives/2013/05/08/generators-in-v8) [available in Node 0.11](http://blog.alexmaccaw.com/how-yield-will-transform-node).
This page is meant to track and list these features, and how to use them.

## Promises (as of 0.11.13)
* [Implicit API from the V8 test suite](https://github.com/v8/v8/blob/master/test/mjsunit/es6/promises.js)
* [spec draft](http://people.mozilla.org/~jorendorff/es6-draft.html#sec-operations-on-promise-objects)

## Generators (`--harmony_generators`)

## Collections (`--harmony_collections` sets, maps, and weak maps)

## Proxies (`--harmony_proxies`)

## Modules (`--harmony_modules`)

## Scoping (`--harmony_scoping`)

## Observation (`--harmony_observation`)

## Symbols (`--harmony_symbols`)

## Iteration (`--harmony_iteration`)

# Other
## V8 flags; meaning and implications
```c++
// Flags for language modes and experimental language features.
DEFINE_bool(use_strict, false, "enforce strict mode")
DEFINE_bool(es_staging, false, "enable upcoming ES6+ features")

DEFINE_bool(harmony_typeof, false, "enable harmony semantics for typeof")
DEFINE_bool(harmony_scoping, false, "enable harmony block scoping")
DEFINE_bool(harmony_modules, false, "enable harmony modules (implies block scoping)")
DEFINE_bool(harmony_symbols, false, "enable harmony symbols (a.k.a. private names)")
DEFINE_bool(harmony_proxies, false, "enable harmony proxies")
DEFINE_bool(harmony_collections, false, "enable harmony collections (sets, maps)")
DEFINE_bool(harmony_generators, false, "enable harmony generators")
DEFINE_bool(harmony_iteration, false, "enable harmony iteration (for-of)")
DEFINE_bool(harmony_numeric_literals, false, "enable harmony numeric literals (0o77, 0b11)")
DEFINE_bool(harmony_strings, false, "enable harmony string")
DEFINE_bool(harmony_arrays, false, "enable harmony arrays")
DEFINE_bool(harmony_maths, false, "enable harmony math functions")
DEFINE_bool(harmony, false, "enable all harmony features (except typeof)")

DEFINE_implication(harmony, harmony_scoping)
DEFINE_implication(harmony, harmony_modules)
DEFINE_implication(harmony, harmony_symbols)
DEFINE_implication(harmony, harmony_proxies)
DEFINE_implication(harmony, harmony_collections)
DEFINE_implication(harmony, harmony_generators)
DEFINE_implication(harmony, harmony_iteration)
DEFINE_implication(harmony, harmony_numeric_literals)
DEFINE_implication(harmony, harmony_strings)
DEFINE_implication(harmony, harmony_arrays)
DEFINE_implication(harmony_modules, harmony_scoping)

DEFINE_implication(harmony, es_staging)
DEFINE_implication(es_staging, harmony_maths)
```

#A glimpse into the future (v8 3.29)
## Iteration protocol.
Demonstrated here over a generator: 

![iteration_over_generator](https://cloud.githubusercontent.com/assets/96947/4081915/5869cf04-2eec-11e4-90ff-fa3e9971ef64.gif)

## Fat arrow functions (lambdas):
![fatarrowftw](https://cloud.githubusercontent.com/assets/96947/3881865/7b9935c2-2191-11e4-9e62-26ac8c080bee.gif)