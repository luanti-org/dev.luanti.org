---
title: Linux - headless server
aliases:
  - /compiling-a-headless-linux-server/
  - /compiling/linux-server
---

# Compiling a headless Linux server

While a regular Linux client build can be run as a dedicated server using `luanti --server` and give the same experience, a headless server build is useful if you wish to run a Luanti server on a headless system, as it will pull in less dependencies.

This tutorial assumes your server is running Debian or some derivative. For other distributions, the package names may be different.

## Building on a separate machine

It may be useful to build the server on a separate machine from the one you deploy it to. If your host machine runs the same version of Debian as your server (i.e. both are running Debian stable) then you can simply compile there, otherwise you will have to set up a virtual build environment.

A virtual machine works well even if you are on a Windows host, but if you are on another Linux distribution you may want to use `systemd-nspawn` which can create a more functional chroot-like environment. The following commands will create a base Debian stable root filesystem with `debootstrap` and then run `systemd-nspawn` on it.

```bash
cd /var/lib/machines
debootstrap --include=dbus-broker,systemd-container --components=main,universe stable debian https://deb.debian.org/debian/
systemd-nspawn -D debian
```

Do note this only works well if your host machine and server has the same architecture.

## Dependencies

The base dependencies for a server build are as follows:

{{% comment %}} cspell:disable {{% /comment %}}

```bash
sudo apt install g++ ninja-build cmake libsqlite3-dev libcurl4-openssl-dev zlib1g-dev libgmp-dev libjsoncpp-dev libzstd-dev libncurses-dev
```

{{% comment %}} cspell:enable {{% /comment %}}

This dependency list includes cURL for announcing to the serverlist and ncurses for the interactive server terminal (`--terminal`).

Some further optional dependencies for the server:

| Dependency   | Development package | Runtime package | Purpose                        |
| ------------ | ------------------- | --------------- | ------------------------------ |
| LevelDB      | `libleveldb-dev`    | TODO            | LevelDB database backend       |
| PostgreSQL   | TODO                | TODO            | PostgreSQL database backend    |
| Redis        | TODO                | TODO            | Redis database backend         |
| Prometheus   | TODO                | TODO            | Prometheus statistics logging  |
| Spatialindex | TODO                | TODO            | Spatialindex AreaStore backend |

## Build LuaJIT from source

It is recommended to build LuaJIT from source, as the version found in repositories of Debian-based distributions can be quite old.

```bash
git clone https://github.com/LuaJIT/LuaJIT luajit
cd luajit
make amalg
```

(`luajit` should be a sibling directory to the `luanti` directory cloned below)

## Download

Clone Luanti with Git. `-b stable-5` will checkout the latest stable version to build, which may be recommended to run a server for stability reasons over the latest development version. Omitting this will clone the latest development version instead.

```bash
git clone -b stable-5 --depth 1 https://github.com/luanti-org/luanti.git
cd luanti
```

## Build

Generate a build folder set to build a headless server and not a client, as well as specifying paths to the LuaJIT we built previously.

```bash
mkdir build; cd build
cmake .. -G Ninja -DBUILD_CLIENT=0 -DBUILD_SERVER=1 -DRUN_IN_PLACE=1 -DBUILD_UNITTESTS=0 \
	-DLUA_INCLUDE_DIR=../../luajit/src/ -DLUA_LIBRARY=../../luajit/src/libluajit.a
ninja
```

The resulting server binary can be found at `./bin/luantiserver`. You can move the binary out of the source tree to somewhere cleaner to run your server out of, only other thing that is required to copy over for the server binary to work is the `builtin` folder.

If you are building on a separate machine from where you would want to run the server, you can run `ninja package` which will package up the server binary along with necessary files into a `.tar.gz` archive that can be deployed onto your server.
