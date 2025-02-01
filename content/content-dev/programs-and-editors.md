---
title: Programs and Editors
aliases:
- /Programs_and_Editors
- /programs-and-editors
---

# Programs and Editors

This is a list of unofficial [Luanti](/Luanti)-related software.

This is not a comprehensive list, for more see [Luanti-related projects](https://forum.minetest.net/viewforum.php?f=14) in the forum.

Mapping
-------

Name | Description | Info/webpage | Author
--|--|--|--
[Minetest Mapper](https://github.com/luanti-org/minetestmapper) | Creates 2D map images of an already existing world. One block corresponds to one pixel. There is also a [Minetest Mapper GUI](https://forum.luanti.org/viewtopic.php?t=12139) available. | [minetestmapper](/minetestmapper) | [Many contributors](https://github.com/luanti-org/minetestmapper/graphs/contributors)
[Amidst for Minetest](https://github.com/Treer/Amidst-for-Minetest) | Creates a map of biomes without actually creating the world. This tool just needs a world type, biome profile and seed. Can also view the biomes as Voronoi diagrams. | [Forum](https://forum.luanti.org/viewtopic.php?t=19869) | [Dr. Frankenstone](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=19464)
[MTSatellite](https://bitbucket.org/s_l_teichmann/mtsatellite/src/master/) | A real-time Web mapping system. Play on your world and share a map of it in the Web, live. | [Forum](https://forum.luanti.org/viewtopic.php?t=10278) | [s-l-teichmann](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=11044)
[Minetest mapserver](https://github.com/minetest-mapserver/mapserver) | A real-time Web mapping system with POI, player, shop and item-search support. Play on your world and share a map of it in the Web, live. written in Go. | [Forum](https://forum.luanti.org/viewtopic.php?t=21999) | [BuckarooBanzay](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=22999)
[Onomatopoeia](https://github.com/HybridDog/onomatopoeia) | Creates isometric images of already existing worlds. | [Forum](https://forum.luanti.org/viewtopic.php?t=18698) | [Zeg9 and HybridDog](https://github.com/HybridDog/onomatopoeia/graphs/contributors)

World editing
-------------

Name | Description | Info/webpage | Author
--|--|--|--
[Geo mapgen](https://github.com/gaelysam/geo-mapgen) | Generate worlds from real-world topographical data. Reads GeoTiff land cover data and digital elevation models such as those provided by [SRTM](http://srtm.csi.cgiar.org/SELECTION/inputCoord.asp) | [Forum](https://forum.luanti.org/viewtopic.php?t=19387) | [Gael de Sailly](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=9067)
[Real Terrain](https://github.com/bobombolo/realterrain) | Use image files as heightmaps and biome-maps to generate worlds. | [Forum](https://forum.luanti.org/viewtopic.php?t=12666) | [bobomb](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=15865)
[mtmapprune](https://github.com/minetest-tools/mtmapprune) | Prunes the map.sqlite file of a world and deletes blocks outside a specified range. | [Forum](https://forum.luanti.org/viewtopic.php?t=18579) | [sofar](https://github.com/sofar)
[WorldPainter](https://www.worldpainter.net/) | WorldPainter is a Minecraft tool that provides Luanti support via a plugin. WorldPainter generates the world according to your instructions about the terrain height and type, and any additional "layers" you've painted which can add things like trees and other vegetation, caves or any type of custom object. You can paint and sculpt the terrain by hand, but you can also use height maps and/or image masks. | [Forum](https://forum.luanti.org/viewtopic.php?t=16649) | [Captain Chaos](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=20528)
[MapEditr](https://github.com/random-geek/MapEditr) | MapEditr is a command-line tool for fast manipulation of Luanti map database files. Functionally similar to WorldEdit, but runs outside of the game and is designed for handling larger tasks. | [Forum](https://forum.luanti.org/viewtopic.php?t=26803) | [Random Geek](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=23195)
[MapEdit](https://github.com/random-geek/MapEdit) | Superseded by MapEditr (above) | [Forum](https://github.com/random-geek/MapEdit) | [Random Geek](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=23195)

Convert data from _Minecraft_
-----------------------------


Name | Description | Info/webpage | Author
--|--|--|--
[mcimport](https://github.com/minetest-tools/mcimport) | A world converter for whole Minecraft maps (saves), outputting a new, playable Luanti world. For GNU/Linux. | [Forum](https://forum.luanti.org/viewtopic.php?f=14&t=13709) | [many contributors](https://github.com/minetest-tools/mcimport/graphs/contributors)
[mcresconvert](https://github.com/minetest-tools/mcresconvert) | Convert Minecraft resource packs and texture packs to Luanti texture packs | [Forum](https://forum.luanti.org/viewtopic.php?f=4&t=14283) | [many contributors](https://github.com/minetest-tools/mcresconvert/graphs/contributors)

Twoelk also describes [other converters for importing Minecraft data into Luanti](https://forum.minetest.net/viewtopic.php?p=251194&sid=8558c08027ecfd8d6f08620c9344882f#p251194).

Servers
-------

Name | Description | Info/webpage | Author
--|--|--|--
[mtmediasrv](https://github.com/minetest-tools/mtmediasrv) | Minetest Remote Media Server: Distribute Luanti media (textures, models, sounds) to Luanti clients for multiplayer server owners that wish to have their media hosted on a remote media server URL. | [Forum](https://forum.luanti.org/viewtopic.php?f=14&t=17411) | [sofar](https://github.com/sofar)

Development
-----------

See [Development Tools](/development-tools) for tools used to create Luanti mods, games, schematics, models, and more.
