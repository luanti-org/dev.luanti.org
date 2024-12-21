---
title: PlayerMetaData
aliases:
- /PlayerMetaData
---

# PlayerMetaData
PlayerMetaData is a per-world, per-player persistent string key-value store implementing all methods of [MetaData](/docs/classes/metadata/).

The granularity of the persisted snapshots is determined by the `map_save_interval` setting.

{{< notice note >}}
Since PlayerMetaData is shared across all mods, it is considered good practice to prefix the keys set by your mod with your mod name plus a delimiter such as `:` to avoid collisions: The `score` field of a quest mod and a mobs mod f.E. might be called `fancy_quests:score` and `fancy_mobs:score` respectively.
{{< /notice >}}

## `player:get_meta()`
Used to obtain a mutable PlayerMetaData reference.

**Arguments:**
- `player`: A valid Player object

{{< notice note >}}
Luanti requiring a valid `player` object means PlayerMetaData is only accessible while players are online; if you need per-player storage while players are offline, you can use ModStorage and save either a serialized table per-player or concatenate the keys of the per-player entries with the playername using a delimiter. Reusing the above example, `fancy_quests:score` might be stored as `<playername>:score` or as key-value pair with key `<playername>` and value `core.write_json{score = ...}` (or `core.serialize`)  in ModStorage.
{{< /notice >}}

A [feature request](https://github.com/minetest/minetest/issues/6193) to make PlayerMetaData available for offline players exists;

**Returns:**
- `meta` - PlayerMetaData: Public & shared PlayerMetaData object for the player
