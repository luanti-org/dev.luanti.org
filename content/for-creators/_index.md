---
title: For Creators
aliases:
- /Modding_Intro
- /modding-intro
- /content-dev
bookCollapseSection: true
---

# For Creators

Luanti is a platform for creating and sharing games, mods, texture packs, and more. This section also hosts information about translating content for other audiences.

Luanti has a scripting API, which is used to program games and mods, creating whole new experiences or extending existing ones. The API is accessed using Lua, an easy-to-use programming language. Version 5.1 of Lua is used, but many people run LuaJIT for greater performance. The only thing you will need is _basic_ programming knowledge.

Documentation
-------------

### Tutorials

The [Luanti Modding Book](https://rubenwardy.com/minetest_modding_book/) is a friendly introduction to Luanti modding and game creation, introducing you to various aspects of the API.

It is recommended you start here, even if you are already apt at programming, to get a good understanding of how Luanti mods work and are structured.

### Lua API Reference

The official Lua API documentation is `lua_api.md`. It's available as [markdown](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md) or [HTML](https://api.luanti.org/). You can find the plaintext version in your Luanti installation, in the `doc` directory.

This is a concise description of the entire API, explaining functions, data structures, registration templates & more. The core developers of Luanti maintain it, changes going through a quality control process.

Any functions not listed here are subject to change and not guaranteed to be compatible across versions, though usually they are.

### The minetest_docs Project

minetest_docs was a project to create new, more detailed documentation. The final contents from the project can currently be found under the [/api/](/for-creators/api/) directory here.

Useful tools
------------

Here are some useful tools that most modders use when making Luanti mods:

* [Visual Studio Code](https://code.visualstudio.com/)/[VSCodium](https://vscodium.com/), powerful code editor with a [Minetest extension](https://marketplace.visualstudio.com/items?itemName=GreenXenith.minetest-tools) available for code completion.
* [luacheck](https://github.com/lunarmodules/luacheck), static analysis tool for Lua. See [this modding book chapter](https://rubenwardy.com/minetest_modding_book/en/quality/luacheck.html) for more information on how to use it with Luanti.

Other useful links
------------------

* Check out [ContentDB](https://content.luanti.org/) to see mods that have been published by the community.
* Get mod help from the community:
    * [Forums](https://forum.luanti.org/viewforum.php?f=47)
    * [Discord](https://discord.gg/minetest)
    * [Matrix](https://matrix.to/#/#minetest:tchncs.de)
    * ...[more](https://www.luanti.org/get-involved/)
* Suggest a mod idea in the [mod request thread](https://forum.luanti.org/viewtopic.php?f=9&t=2434).
