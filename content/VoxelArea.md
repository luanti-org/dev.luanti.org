# VoxelArea
The `VoxelArea` metatable provides an OOP-like utility for dealing with LuaVoxelManip (specifically, for doing the index math).

## `VoxelArea.new(self, o)`

Sets `self.__index` to `self` and `self` as the metatable of `o`, which can have the following fields:

* `MinEdge`: Minimum position of the area, inclusive. Defaults to `vector.new(1, 1, 1)` if `nil`.
* `MaxEdge`: Maximum position of the area, inclusive. Defaults to `vector.new(0, 0, 0)` if `nil`.

Both positions should be `vector`s of integer numbers; each component of `MinEdge` should be smaller than the respective component of `MaxEdge` if the `VoxelArea` is to be non-empty. You can use `minp, maxp = vector.sort(minp, maxp)` to achieve this.

Calculates `ystride` and `zstride` and stores both in `o`.

Common usage:
```lua
local voxelmanip = core.get_voxel_manip(pos_min, pos_max)
local emin, emax = voxelmanip:read_from_map(pos_min, pos_max)
local voxelarea = VoxelArea:new{ MinEdge = emin, MaxEdge = emax }
```

{{< notice warning >}}
Always pass the actual emerged min & max positions. Do not pass the desired min & max positions.
{{< /notice >}}

{{< notice warning >}}
Never pass fractional values as min- or max edge.
{{< /notice >}}

{{< notice tip >}}
You can use `vector.floor`, `vector.round` or `vector.apply(vec, math.ceil)` to guarantee integer values.
{{< /notice >}}

The following methods can both be called in an imperative manner (`VoxelArea.<method>(self, ...)`) or an OOP manner (recommended): `self:<method>(...)`. The below examples are documented using the latter style, where `area` is a valid table with `VoxelArea` as the metatable.

## `area:getExtent()`

Returns the dimensions of `area` as integer `vector`.

## `area:getVolume()`

Returns the volume of `area` as integer.

## `area:index(x, y, z)`

`x`, `y`, `z` are absolute coordinates of a node within the area. Returns an integer index to be used for data tables returned by VoxelManip objects.

{{< notice warning >}}
This will silently `floor` the returned index instead of throwing an error. Make sure that the coordinates you pass are (1) not fractional and (2) within the area.
{{< /notice >}}

## `area:indexp(p)`

Shorthand for `area:index(p.x, p.y, p.z)`.

## `area:position(index)`

Inverse to `area:indexp`. Returns the absolute node position corresponding to the `index` as a table with `x`, `y` and `z` fields.

{{< notice tip >}}
The returned table is missing the `vector` metatable. If it is not performance-critical, use `p = vector.new(area:position(index))` to create a copied vector with metatable.
{{< /notice >}}

## `area:contains(x, y, z)`

Returns `true` if all `x`, `y`, `z` coordinates are between the respective `MinEdge` and `MaxEdge` coordinates, both inclusive.

## `area:containsp(p)`

Shorthand for `area:contains(p.x, p.y, p.z)`

## `area:containsi(i)`

Returns `true` if `i` is between `1` and the `area` volume, both inclusive.

{{< notice warning >}}
`area:containsi(area:indexp(p))` *is not equivalent to* `area:containsp(p)`, as `area:indexp` will happily produce valid indices for some out-of-area positions.
{{< /notice >}}

## `area:iter(minx, miny, minz, maxx, maxy, maxz)`

Returns an iterator (a function that returns the index of the current position and advances to the next one) that iterates in XYZ order (first incrementing X until the line has been finished, then Y until the plane has been finished, then Z until the cuboid has been finished).

## `area:iterp(minp, maxp)`

Shorthand for `area:iter(minp.x, minp.y, minp.z, maxp.x, maxp.y, maxp.z)`.

Example:

```lua
for index in area:iterp(pos_min, pos_max) do
	content_id_data[index] = ...
end
```

which is equivalent to (but shorter & faster than):

```lua
for z = pos_min.z, pos_max.z do
	for y = pos_min.y, pos_max.y do
		for x = pos_min.x, pos_max.x do
			local index = area:index(x, y, z)
			content_id_data[index] = ...
		end
	end
end
```
