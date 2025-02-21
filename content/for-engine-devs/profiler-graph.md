---
title: Profiler graph
aliases:
- /Profiler_graph
- /profiler-graph
---

# Profiler graph

The profiler graphs show the performance of Luanti in a more detailed fashion. This information is useful for engine developers. The profiler graphs are shown when you hit the debug key (F5 by default) twice.

[![Profiler graph](/images/Profiler_graph.webp)](/images/Profiler_graph.webp)

The following graphs are available:

* **rudp\_rtt**: ???
* **rudp\_jitter**: ???
* **packets\_lost**: ???
* **num\_processed\_meshes**: The engine generates geometric meshes from MapBlock data for drawing. This is the number of those meshes that finished generating in each frame.
* **mainloop\_sleep**: If the game runs at a faster rate than `wanted_fps`, a sleep is inserted into each frame after drawing in order to not consume excess resources; this is that sleep time in seconds.
* **mainloop\_other**: Time (in seconds) spent in each frame for everything else than drawing.
* **mainloop\_dtime**: Total time (in seconds) spent per frame (`mainloop_other + mainloop_draw + mainloop_sleep`); FPS = 1 divided by this, averaged.
* **mainloop\_draw**: Time (in seconds) spent in each frame for drawing (rendering).
* **client\_received\_packets**: Number of received high-level protocol packets in each frame.

See also
--------

* [Debug](/for-creators/debug)