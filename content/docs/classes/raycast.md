---
title: Raycast
aliases:
- /Raycast
---

# Raycast
Raycasts are used to simulate rays and determine which selection boxes they intersect with.

With some limitations, they work almost exactly like the player pointing at things.

## Pointed Thing
Pointed things in-game as passed to various callbacks are either

* Nothing (`{type = "nothing"}`), or
* Objects (entities or players), or
* Nodes (solids or liquids)

Pointed thing tables are thus a "tagged union" type, taking the following forms for objects & nodes:

### Objects
- `type` - `{type-string}`: `"object"`
- `ref` - ObjectRef: The pointed ObjectRef

### Nodes
- `type` - `{type-string}`: `"node"`
- `under` - `{type-vector}`: The position of the pointed node (the node which would be dug if using an appropriate tool)
- `above` - `{type-vector}`: The position of the pointed node plus the pointed face normal (the position of the node which would be placed if you were building)

### Additional Raycast Fields
These fields are only available in pointed things returned by raycasts.

- `box_id` - `{type-number}`: Only relevant for nodes: 1-indexed ID of the pointed selection box of the node
- `intersection_normal` - `{type-vector}`: Normal vector pointing out of the face of the pointed thing the ray first hit
- `intersection_point` - `{type-vector}`: Point on both face & ray where the ray & the selection box(es) of the pointed thing first intersect (may be in the selection box if the raycast starts in the box)

Raycasts do not emit "nothing" pointed things.

## `core.line_of_sight(pos1, pos2)`
Checks whether any non-air node is blocking the line of sight between two positions.

You may use this to determine e.g. whether a target would be visible to a mob. Raycasts will often be preferable since they provide more fine-grained control (objects, liquids, looping over possibly blocking things in the line of sight).

**Arguments:**
- `pos1` - `{type-vector}`: Starting position of the line ("segment") of sight
- `pos2` - `{type-vector}`: Ending position of the line ("segment") of sight

**Returns:**
- `free_sight` - `{type-bool}`: Whether any node is blocking the sight
- `pos` - `{type-vector}`: Position of the "first" node (the node closest to `pos1`) blocking the sight (only returned if `free_sight` is `false`)

## `Raycast(from_pos, to_pos, include_objects, include_liquid_nodes)`
Creates a raycast object; OOP-constructor-style alias for `core.raycast`.

{{< notice info >}}
[Raycasts work on selection-, not collision boxes, making them coherent with player pointing but not physics (collisions) unless selection- and collision boxes are identical.](https://github.com/luanti-org/luanti/issues/12673)
{{< /notice >}}

{{< notice tip >}}
Have selection boxes, collision boxes (and ideally even visuals) match for all nodes & entities if possible.
{{< /notice >}}

{{< notice info >}}
[Serverside raycasts do not support attachments properly; the server is unaware of model specificities, doesn't keep track of automatic rotation etc.](https://github.com/luanti-org/luanti/issues/10304)
{{< /notice >}}

**Arguments:**
- `from_pos` - `{type-vector}`: Starting position of the raycast (usually eye position)
- `to_pos` - `{type-vector}`: Ending position of the raycast (usually eye position + look direction times range)
- `include_objects` - `{type-bool}`: Whether objects are included (considered pointable). Optional, defaults to `true`.
- `include_liquids` - `{type-bool}`: Whether liquids are included (considered pointable). Optional, defaults to `true`.

**Returns:**
- `ray` - Raycast iterator object: Non-restartable, stateful iterator of pointed things

The Raycast iterator object is callable. Calling `ray()` is syntactic sugar for `ray:next()`.

`ray:next()` returns the next pointed thing or `{type-nil}` if there is none and advances the iterator state.

Pointed things are returned sorted by the intersection point, from `from_pos` to `to_pos`, from nearest to farthest.

### Usage

Since raycasts are iterators, you can loop over them using a `for`-loop. You will usually use the concise

```lua
local ray = Raycast(...)
for pointed_thing in ray do
	-- do something
end
```

which is just shorthand for

```lua
local ray = Raycast(...)
for pointed_thing in ray.next, ray do
	-- do something
end
```

which again is syntactic sugar for

```lua
local ray = Raycast(...)
local pointed_thing = ray:next()
while pointed_thing ~= nil do
	-- do something
	pointed_thing = ray:next()
end
```

There is absolutely no need to manually step through raycasts using the latter two more verbose loops.

{{< notice info >}}
Restartability is the ability of an iterator to start again. For example, `ipairs` is resumable:

```lua
local t = {1, 2, 3}
for _ = 1, 3 do
	for i, v in ipairs(t) do
		print(i, v)
	end
end
```

will work just expected, printing the contents of `t` three times.

*Raycasts are not restartable.* Iterating them _"consumes"_ the pointed things; they can't be iterated again. The following will not work as expected:

```lua
local ray = Raycast(...)
for _ = 1, 3 do
	for pt in ray do
		print(pt)
	end
end
```

this will print the pointed things (table addresses) exactly once: after the first run of the inner `for` loop, all pointed things have been consumed; subsequent runs exit immediately, not entering the `for` loop at all.

One advantage of this is that Raycasts remember their looping state. The following is thus possible:

```lua
local ray = Raycast(...)
for pt in ray do
	if ... then break end
end
for pt in ray do
	-- will resume looping with the next pointed thing
end
```
{{< /notice >}}

## Examples

### Redo tool raycasts

Useful to obtain the additional raycast pointed thing fields, to dynamically decide what is pointable and what is not, to determine the pointed thing periodically ("what am I looking at?") etc.

```lua
-- Calculate eye position including eye offset
local eye_pos = player:get_pos()
eye_pos.y = eye_pos.y + player:get_properties().eye_height
local first, third = player:get_eye_offset()
if not vector.equals(first, third) then
	core.log("warning", "First & third person eye offsets don't match, assuming first person")
end
eye_pos = vector.add(eye_pos, vector.divide(first, 10)) -- eye offsets are in block space (10x), transform them back to metric

local def = player:get_wielded_item():get_definition()

for pointed_thing in core.raycast(eye_pos, vector.add(eye_pos, vector.multiply(player:get_look_dir(), def.range or 4)), true, def.liquids_pointable) do
	if pointed_thing.ref ~= player then -- exclude the player
		-- do something
	end
end
```

(TODO: compare players by player name rather than by reference in the above example?)
