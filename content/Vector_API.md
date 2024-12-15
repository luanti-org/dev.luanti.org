# Vector API
The `vector` type used to be a simple `table` with `x`, `y`, and `z` values.
However it has more recently been given metatable methods for convenience.
Unless otherwise noted, the `vector` type always refers to the metatable-enhanced variety.
Do note that functions here will accept an old-style (non-metatable) `vector`, but you cannot perform metatable operations with said `vector`.

{{< notice tip >}}
The respective source code is located [here](https://github.com/minetest/minetest/blob/master/builtin/common/vector.lua).
{{< /notice >}}

## `vector` Namespace

### `vector.new(a, b, c)`
* `a`: `number`, `vector`, or `nil`
* `b`, `c`: `number` or `nil`

If `a`, `b`, and `c` are `number`: Returns a new `vector` where `{x = a, y = b, z = c}`

Deprecated behaviours:
- If `a` is a `vector`: Returns `vector.copy(a)`
- If all parameters are `nil`: Returns `vector.zero()`

### `vector.zero()`
Returns a new `vector` where `{x = 0, y = 0, z = 0}`

### `vector.copy(v)`
* `v`: `vector`

Returns a new `vector` where `{x = v.x, y = v.y, z = v.z}`. This is *not* equivalent to `table.copy`, as that does not set the `vector` metatable.

### `vector.from_string(s, init)`
* `s`: `string`
* `init`: `number`

Returns a new `vector` parsed from `s` using `string.match`, followed by the position of the first character following the parsed data.

Expects `s` to be in the following format: `(x, y, z)` where `x`, `y`, and `z` are all valid numbers passed to `tonumber`.
There are some allowances in the parsing rules for extra whitespace as padding around elements.
You may set the position in `s` where `string.match` will begin parsing via `init`.

### `vector.to_string(v)`
* `v`: `vector`

Returns a `string` representation of `v` in the format: `(v.x, v.y, v.z)`.

Each component is formatted with the printf-style `%g` flag (either floating-point or scientific notation, whichever is shorter).

### `vector.equals(a, b)`
* `a`, `b`: `vector`

Returns `true` if `a` is equivalent to `b` (all components are the same).

Returns `false` otherwise.

### `vector.length(v)`
* `v`: `vector`

Returns the vectorial length (total traveled traveled distance from the origin to the end) of `v`.

The formula for the length is: `sqrt(x^2 + y^2 + z^2)`.

### `vector.normalize(v)`
* `v`: `vector`

Returns a new `vector` which is the normalized form of `v` (the vectorial length is equal to 1).
Uses `vector.length(v)` to get the vectorial length.

Specifically, if the length is 0, returns a new `vector` with all components being `0`.
Otherwise, returns `vector.divide(v, length)`.

### `vector.floor(v)`
* `v`: `vector`

Returns a new `vector` where each component of `v` has had `math.floor` applied to it.

Literally `vector.apply(v, math.floor)`.

### `vector.round(v)`
* `v`: `vector`

Returns a new `vector` where each component of `v` has had `math.round` applied to it.

Equivalent to `vector.apply(v, math.round)`

### `vector.apply(v, func)`
* `v`: `vector`
* `func`: `function`

Returns a new `vector` where each component of `v` has had `func` applied to it.

### `vector.combine(v, w, func)`
* `v`: `vector`
* `w`: `vector`
* `func`: `function`

Returns a new `vector` where each pair of respective components of `v` and `w` has had `func` applied to it.

Example: `vector.combine(v, w, math.pow)` is the same as `vector.new(math.pow(v.x, w.x), math.pow(v.y, w.y), math.pow(v.z, w.z))`.

### `vector.distance(a, b)`
* `a`, `b`: `vector`

Returns a `number` which is equal to the distance between `a` and `b`.

Distance is equal to the scalar (single number) result of `|bar a - bar b|`.

### `vector.direction(pos1, pos2)`
* `pos1`, `pos2`: `vector`

Returns a new, normalized `vector` equal to the direction from `pos1` to `pos2`.

### `vector.angle(a, b)`
* `a`, `b`: `vector`

Returns a `number` which is equal to the angle (in radians) between `a` and `b`.

Formula used is `tan^-1(|bar a xx bar b|, bar a * bar b)`.

### `vector.dot(a, b)`
* `a`, `b`: `vector`

Returns a `number` equal to the dot product of `a` and `b`.

### `vector.cross(a, b)`
* `a`, `b`: `vector`

Returns a new `vector` which is equal to the cross product of `a` and `b`.

### `vector.add(a, b)`
* `a`: `vector`
* `b`: `vector` or `number`

If `b` is a `vector`: Returns a new `vector` where each component of `b` is added to each component of `a`

If `b` is a `number`: Returns a new `vector` where `b` is added to each component of `a`

### `vector.subtract(a, b)`
* `a`: `vector`
* `b`: `vector` or `number`

If `b` is a `vector`: Returns a new `vector` where each component of `b` is subtracted from each component of `a`

If `b` is a `number`: Returns a new `vector` where `b` is subtracted from each component of `a`

### `vector.multiply(a, b)`
* `a`: `vector`
* `b`: `vector` or `number`

If `b` is a `vector`: Returns a new `vector` where each component of `a` is multiplied by component of `b`

If `b` is a `number`: Returns a new `vector` where each component of `a` is multiplied by `b`

### `vector.divide(a, b)`
* `a`: `vector`
* `b`: `vector` or `number`

If `b` is a `vector`: Returns a new `vector` where each component of `a` is divided by component of `b`

If `b` is a `number`: Returns a new `vector` where each component of `a` is divided by `b`

### `vector.offset(v, x, y, z)`
* `v`: `vector`
* `x`, `y`, `z`: `number`

Returns a new `vector` where each component of `x`, `y`, and `z` are added to the respective components of `v`.

Equivalent to `vector.add(v, {x = x, y = y, z = z})`.

### `vector.sort(a, b)`
* `a`: `vector`
* `b`: `vector`

Returns two new `vector` values.

The first consists of the smaller components of `a` and `b`, where each is equal to `math.min(a.N, b.N)` (N being one of x, y, or z).

The second is similar to the first, but consisting of the larger components instead.

### `vector.check(v)`
* `v`: `vector`

Returns `true` if `v` is a valid, metatable-enhanced vector.

Returns `false` otherwise.

### `vector.rotate_around_axis(v, axis, angle)`
* `v`, `axis`: `vector`
* `angle`: `number`

Returns a new `vector` which is equal to `v` rotated around `axis` by `angle` radians counter-clockwise.

### `vector.rotate(v, rot)`
* `v`, `rot`: `vector`

Returns a new `vector` which is equal to `v` rotated by `rot` counter-clockwise.

The way that the components of `rot` map is as follows:

* `rot.x` is pitch
* `rot.y` is yaw
* `rot.z` is roll

### `vector.dir_to_rotation(forward, up)`
* `forward`: `vector`
* `up`: `vector` or `nil`

Returns a new rotational `vector` (the same kind as `rot` in `vector.rotate`) equal to the rotation from `up` to `forward`.

If `up` is `nil` then the returned rotational `vector` assumed that `y = 1` is up.

Both `up` and `forward` are normalized by the function before calculations are made with them, so a call with or without normalization by the caller will be the same.

## Metatable Functions
Metatable-enhanced `vector` values have some convenience features to help make vector math more readable. They can have be used with normal math operations rather than needing to call the equivalent namespaced function.

They also can be indexed either with named keys (`v.x` and `v["x"]`) or they can be indexed with numeric keys (`v[1]` being `v.x`, `v[2]` being `v.y`, and `v[3]` being `v.z`).

```lua
local v1 = vector.new(1, 2, 3)
local v2 = vector.new(4, 5, 6)
local v3

-- This:
v3 = vector.add(v1, v2)
-- Is equivalent to:
v3 = v1 + v2

-- You can also use do more lengthy calculations:
v3 = ((v1 + v2) / 2) * 3
-- The equivalent using the namespaced functions would be rather unwieldy:
v3 = vector.multiply(vector.divide(vector.add(v1, v2), 2), 3)
```

### `metatable.__eq(a, b)`
* `a`, `b`: `vector`

Literally `vector.equals(a, b)`.

### `metatable.__unm(v)`
* `v`: `vector`

Returns a new `vector` which is the inverse of `v`.

### `metatable.__add(a, b)`
* `a`, `b`: `vector`

Returns a new `vector` where each component of `a` is added to each respective component of `b`.

{{< notice note >}}
Unlike `vector.add()` this does not support adding `number` values.
{{< /notice >}}

### `metatable.__sub(a, b)`
* `a`, `b`: `vector`

Returns a new `vector` where each component of `a` is subtracted from each respective component of `b`.

{{< notice note >}}
Unlike `vector.subtract()` this does not support subtracting `number` values.
{{< /notice >}}

### `metatable.__mul(a, b)`
* `a`: `vector` or `number`
* `b`: `number` or `vector`

Returns a new `vector` where each component of `a` is multiplied by each respective component of `b`. Because of the way metatables work, either argument can be a `number` or a `vector` without any practical difference.

{{< notice info >}}
This function assumes that one just one argument is a number, and will probably return confusing errors about "accessing a `nil` value" if two `vector` arguments are passed to it.
{{< /notice >}}

### `metatable.__div(a, b)`
* `a`: `vector`
* `b`: `number`

Returns a new `vector` where each component of `a` is divided by `b`.

{{< notice info >}}
This function assumes `b` is a number, and will probably return confusing errors about "accessing a `nil` value" if it is not.
{{< /notice >}}
