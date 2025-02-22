---
title: ItemStackMetaData
aliases:
  - /api/classes/itemstackmetadata
---

# ItemStackMetaData

ItemStackMetaData is a subclass of [MetaData](/for-creators/api/classes/metadata/) obtained via `stack:get_meta()` allowing for persistent storage of key-value pairs tied to ItemStacks.

{{< notice warning >}}
ItemStack metadata is serialized with ItemStacks, increasing the ItemString length. Inventories have to store multiple ItemStrings, all of which an attacker will try to get to maximum length. Always limit the size of your ItemStackMetaData to keep inventories sendable.
{{< /notice >}}

## Special Fields

### Description

- `description`: The description to be shown when hovering over the item (see `ItemStack:get_description`).
- `short_description`: A short description of the item (see `ItemStack:get_short_description`).

### Hardware Colorization

- `color`: ColorString to use for hardware colorization of the stack.
- `palette_index`: Palette index to use for hardware colorization of the stack (if the stack has a palette).

### Count

Requires Luanti 5.6 clients for the count override (older clients will simply show the item count according to the item definition); works on all 5.x servers since it only uses ItemStackMetaData server-side.

#### `count_meta`

String to show in inventory lists instead of the item count.

#### `count_alignment`

Integer (use with `:get_int` and `:set_int`).

Alignment of the displayed item count value is encoded as `x_align + 4 * y_align` where `x_align` and `y_align` are one of:

| Value | X-alignment (horizontally) | Y-alignment (vertically) |
| ----- | -------------------------- | ------------------------ |
| `0`   | Default (same as `3`)      | Default (same as `3`)    |
| `1`   | Left                       | Top/Up                   |
| `2`   | Centered/Middle            | Centered/Middle          |
| `3`   | Right                      | Bottom/Down              |

{{< notice tip >}}
Magic numbers make code unreadable. Add an explanatory comment when setting alignment:
{{< /notice >}}

```lua
local meta = stack:get_meta()
meta:set_string("count_alignment", 3 + 4 * 1) -- aligned to the top-right corner
```

or use well-named (constant) local variables:

```lua
local meta = stack:get_meta()
local align_top_right = 3 + 4 * 1
meta:set_int("count_alignment", align_top_right)
```

or perhaps wrap this all up in a useful helper:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
local vert_align = {
	default = 0,
	top = 1,
	center = 2,
	bottom = 3
}

local horz_align = {
	default = 0,
	left = 1,
	center = 2,
	right = 3
}

local function set_stack_cnt_align(stack, vertically, horizontally)
	stack:get_meta():set_int("count_alignment", horz_align[horizontally] + 4 * vert_align[vertically])
end

set_stack_cnt_align(stack, "top", "right")
```

{{% comment %}} cspell:enable {{% /comment %}}

## Methods

### `:set_tool_capabilities`

Allows overriding the tool capabilities specified in the item definition.

**Arguments**

- `tool_capabilities` - `nil` or a tool capabilities table: Either:
  - `nil`: Clears the tool capability override, or
  - Tool capabilities table: Overrides the defined tool capabilities (see ItemDefinition for the table format)

{{< notice note >}}
The corresponding `:get_tool_capabilities` is not a method of ItemStackMetaData but rather of the "parent" ItemStack.
{{< /notice >}}
