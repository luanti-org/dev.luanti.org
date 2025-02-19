---
title: Object Properties
aliases:
- /api/object-properties
---

# Object Properties
This page lists the properties an object may have.

## Properties Applying Only to Players

### `hp_max`
* internal type: `u16`
* default value: `20`

Sets the max HP of a player.

{{< notice note >}}
Decreasing the `hp_max` value to below the current hp will trigger a damage flash.
{{< /notice >}}

### `breath_max`
* internal type: `u16`
* default value: `10`

Sets the max breath of a player.

### `eye_height`
* internal type: `f32`
* default value: `1.625`

Sets the camera height. Measured in nodes above the player's feet.

### `zoom_fov`
* internal type: `f32`
* units: `degrees`
* default value: `0`

Sets the field of view of the player when they zoom, in degrees. Setting to `0` disables zooming.

## Properties Applying to All Objects

### `physical`
* field type: `bool`
* default value: `false`

Controls whether the object collides with nodes that are `walkable`.

### `collide_with_objects`
* field type: `bool`
* default value: `true`

Controls whether the object collides with other objects.

{{< notice info >}}
`physical` must be set to `true` for this to take effect!
{{< /notice >}}

{{< notice note >}}
This works with players as well but you may experience [strange bugs](https://github.com/luanti-org/luanti/issues/11783).
{{< /notice >}}

### `collisionbox`
//This could maybe be linked somewhere else.
* field type: `cuboid_def`
* default value: `{-0.5, 0.0, -0.5, 0.5, 1.0, 0.5}`

The collision box of an object. Wanted value is `{xmin, ymin, zmin, xmax, ymax, zmax}` (in nodes from the center of the object/ object position).

### `selectionbox`
* field type: `cuboid_def`
* default value: `collisionbox's value`

The selection box of an object, if not set, uses `collisionbox`. Wanted value is `{xmin, ymin, zmin, xmax, ymax, zmax}` (in nodes from the center of the object/ object position).

{{< notice note >}}
This uses the same format as a nodebox cuboid.
{{< /notice >}}

### `pointable`
* field type: `bool`
* default value: `true`

Controls whether the object can be pointed at.

### `visual`
* field type: `string`
* default value: `"sprite"`

Possible values:

* `"cube"`: A cube that has the same size as a normal node
* `"item"`: similar to `wielditem`, but ignores the `wield_item` property
* `"mesh"`: Use a mesh model, see the `mesh` property
* `"sprite"`: A texture that is flat and always faces the player
* `"upright_sprite"`: A texture that is flat and faces upright
* `"wielditem"`: Used for dropped items, see the `wield_item` property

Sets what the object will look like.

{{< notice tip >}}
The `"wielditem"` value supports item hardware colorization
{{< /notice >}}

### `mesh`
* field type: `string`
* default value: `""`

Name of the file of the mesh, only when using "mesh" visual.

### `visual_size`
* field type: [`vector`](/for-creators/api/classes/vector)
* default value: `{x = 1, y = 1, z = 1}`

Visual size multipliers. Wanted value is `{x = x, y = y, z = z}`. If `z` is not provided, `x` will be used for the `z` value.

### `textures`
* field type: `string list`
* default value: `{"no_texture.png"}`

Wanted value is `{"texture.png", "texture.png", ...}`. The number of textures depends on `visual`:

* `"cube"`: 6 textures
* `"item":` First texture is itemstring. Deprecated.
* `"mesh"`: 1 texture for each mesh material
* `"sprite"`: 1 texture
* `"upright_sprite"`: 2 textures
* `"wielditem"`: Textures are ignored.

### `colors`
* internal type: `ColorSpec list`
* default value: `{{r = 255, g = 255, b = 255, a = 255}}`

{{< notice info >}}
This feature is not functional.
{{< /notice >}}

### `spritediv`
* field type: `2d_vector`
* default value: `{x = 1, y = 1}`

Determines the number of columns, rows in a spritesheet texture, which is used for animations. Wanted value is `{x = columns, y = rows}`.

### `initial_sprite_basepos`
* field type: `2d_vector`
* default value: `{x = 0, y = 0}`

Determines the position of the first frame to use in the spritesheet texture. Wanted value is `{x = column, y = row}`.

### `is_visible`
* field type: `bool`
* default value: `true`

Whether the object is visible and can be pointed at, this overrides `pointable` if set to `false`.

### `makes_footstep_sound`
* field type: `bool`
* default value: `false`

Whether the object makes a footstep sound when it steps on nodes that have footstep sounds.

### `stepheight`
* internal type: `f32`
* default value: `0`

This sets the maximum height (in nodes) that an object can travel up if it collides with something. This allows it to "step" over obstacles and continue traveling.

### `automatic_rotate`
* internal type: `f32`
* default value: `0`

How fast the object automatically rotates along the `y` axis in radians/second.

{{< notice note >}}
Does not (yet) work for attached entities
{{< /notice >}}

### `automatic_face_movement_dir`
* field type: `int` or `false`
* unit: `degrees`
* default value: `0`

{{< notice tip >}}
This property can be set to `false` to disable it.
{{< /notice >}}

Make the object automatically face the direction it is moving in, the value is the offset in degrees.

### `backface_culling`
* field type: `bool`
* default value: `true`

Whether a model is backface culled (the faces of the model whose backs are towards the camera aren't rendered).

### `glow`
* field type: `int`
* default value: `0`

When calculating the texture color, add this much lighting. Maximum value is `core.LIGHT_MAX`. A value lower than `0` disables lighting.

### `nametag`
* field type: `string`
* default value: `""`

Text displayed above an object, typically used for names. If the object is a player and this value is `nil` or empty, it will be replaced by the player's name. To remove the nametag for a player, set the `nametag_color` alpha value to `0`. If the object is not a player and this value is `nil` or empty, the nametag will be removed.

### `nametag_color`
* field type: `ColorSpec` or `false`
* default value: `{a = 255, r = 255, g = 255, b = 255}`

The color that the nametag text will be.

{{< notice tip >}}
This property can be set to `false` to disable it.
{{< /notice >}}

### `nametag_bgcolor`
* field type: `ColorSpec` or `false`

The color that the nametag's background will be.

{{< notice tip >}}
This property can be set to `false` to not show a background.
{{< /notice >}}

### `automatic_face_movement_max_rotation_per_sec`
* field type: `f32`
* default value: `-1`

Cap the automatic rotation to this value. If the value is less than or equal to `0`, there will be no maximum.

### `infotext`
* field type: `string`

When a player points at the object, they will be shown the text.

### `static_save`
* field type: `bool`
* default value: `true`

Whether the object is saved statically, on the map, if not, it will be deleted when the world block is unloaded.

### `wield_item`
* internal type: `ItemString`

Only when using "wielditem" visual.

### `use_texture_alpha`
* field type: `bool`
* default value: `false`

Whether buggy semi-transparency is enabled for the texture.

{{< notice warning >}}
Faces, entities and other semitransparent world elements might not be rendered in the right order for semi-transparency to work.
{{< /notice >}}

### `shaded`
* field type: `bool`
* default value: `true`

Controls whether or not diffuse lighting is applied to the object.

### `show_on_minimap`
* field type: `bool`
* default value: `false`

Controls whether or not the object is visible on the minimap.

### `damage_texture_modifier`
* field type: `string`
* default value: `"^[brighten"`

A texture modifier that will be appended to the object's current textures for the duration of the damage flash when the object is damaged.

## Layout of Object Properties

The way these properties are laid out is in a table.

{{< notice note >}}
Extraneous properties will be ignored, but might be interpreted incorrectly by future versions of the engine, which is why it is recommended to prefix them with an underscore.
{{< /notice >}}

{{< notice note >}}
This uses regular indexing so metatables work as expected.
{{< /notice >}}

{{< notice warning >}}
For all string properties, the maximum number of characters is the `u16` max value.
{{< /notice >}}

{{< notice note >}}
Number properties will be clamped to their value ranges.
{{< /notice >}}

## Example

```lua
{
    physical = true,
    collide_with_objects = true,
    collisionbox = {-0.3, -0.3, -0.3, 0.3, 0.3, 0.3},
    selectionbox = {-0.3, -0.3, -0.3, 0.3, 0.3, 0.3},
    pointable = true,
    visual = "mesh",
    mesh = "mesh.obj",
    visual_size = {x = 1, y = 1, z = 1},
    textures = {"texture.png", "texture.png"},
    initial_sprite_basepos = {x = 0, y = 0},
    is_visible = true,
    makes_footstep_sound = true,
    stepheight = 0.4,
    automatic_rotate = 0.0,
    automatic_face_movement_dir = 0,
    automatic_face_movement_max_rotation_per_sec = 0,
    backface_culling = true,
    glow = 0,
    nametag = "My object",
    nametag_color = {r = 255, g = 255, b = 255},
    nametag_bgcolor = {r = 0, g = 0, b = 0},
    infotext = "Hello there!",
    static_save = true,
    use_texture_alpha = true,
    shaded = true,
    show_on_minimap = true,
    damage_texture_modifier = "^[colorize:#FF0000:150",
}
```
