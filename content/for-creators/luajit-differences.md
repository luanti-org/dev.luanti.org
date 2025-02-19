---
title: LuaJIT differences
aliases:
- /LuaJIT_differences
- /luajit-differences
- /content-dev/luajit-differences
---

# LuaJIT differences
Most of the time Luanti is built with LuaJIT which aims to be faster than the reference Lua 5.1 implemention (referred to as "PUC Lua") due to its JIT compiler. However it is also possible that Luanti is built with PUC Lua 5.1, if Luanti cannot find LuaJIT when building or when explicitly built with `-DENABLE_LUAJIT=OFF`.

There are some reasons one would want to use PUC Lua over LuaJIT such as for stability reasons or if someone is on an obscure architecture that doesn't have a LuaJIT compiler available yet. As such, you should always support both implementations in your mods and be aware of some of the pitfalls that can cause crashes with a mod developed on LuaJIT being run with PUC Lua.

## Iteration order of tables
The Lua reference manual **does not guarantee an iteration order of tables**:
`pairs` (and `next`) are allowed to visit the table entries in **any order**.
You should **never** rely on a table being iterated in a certain order,
regardless of the Lua implementation you are using.

The iteration order of LuaJIT and PUC Lua will usually be different because they
use different implementations internally.
In particular, LuaJIT for security reasons uses a random seed in its hashing,
causing iteration orders to differ across different runs.

## Goto
LuaJIT implements the `goto` statement along with labels that can be jumped to as an extension to the language. This is not available in PUC Lua, so you shouldn't use `goto` in your mods.

## Hexadecimal escape codes

LuaJIT supports hexadecimal escape codes in strings. For example the string `"\x41"` is equivalent to `"A"` on LuaJIT.
PUC Lua however simply ignores the backslash; it will not throw an error: `"\x41"` is suddenly equivalent to `"x42"`,
which is certainly not what you want.

Instead, you should use PUC Lua's 3-digit [^1] decimal escapes: `"\06542"` is `"A42"`.

## xpcall()

LuaJIT allows you to pass extra arguments to the function `f` to be called in a protected context.
For example `xpcall(print, err_func, "hello world")` would pass `"hello world"` to `print`,
whereas on PUC Lua, the `"hello world"` would just be ignored.

## math.log()

LuaJIT lets you optionally specify a base for the logarithm, PUC Lua only knows the natural logarithm
and ignores extraneous arguments.

Hence `math.log(x, base)` should be written as `math.log(x) / math.log(base)` to be portable.

## math.random()

### Use of a different generator

LuaJIT brings its own pseudo-random number generator (PRNG) whereas PUC Lua just uses a PRNG provided by your system.
This means that given the same seed via `math.randomseed`, they will almost certainly produce different sequences.

In order for mapgen code to be portable (to yield consistent, reproducible results on PUC Lua and LuaJIT),
you **must not use `math.random()` in mapgen code**.

### Argument order independence

LuaJIT is less strict about the order of arguments passed to `math.random`. If you pass a min value that is larger than the max value, it will switch these around. For instance `math.random(3,1)` will swap around the ranges and generate the same random values `math.random(1,3)` would generate.

In PUC Lua doing this will throw an error "bad argument #2 to 'random' (interval is empty)", so make sure you clamp or otherwise make sure that the min value is lower or equal to the max value, swapping them yourself if necessary.

## More...
An exhaustive list of the extensions to the Lua language that LuaJIT provides can be found on [LuaJIT.org's extensions page](https://luajit.org/extensions.html).

## For older versions of Luanti
Since 5.5.0 the [bitop](https://bitop.luajit.org/index.html) library is included when building Luanti with PUC Lua so if your mod targets Luanti 5.5+ it is safe to use the bitop library. This has always been available on LuaJIT, but you cannot rely on it being available on Luanti 5.4 and earlier.

[^1]: Leading zeros can be omitted, but as this example demonstrates it may be necessary to keep them in case the escape sequence is followed by literal digits.
