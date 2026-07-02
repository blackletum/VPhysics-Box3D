# Building Vox3D

Vox3D builds inside a Source SDK tree with Valve's VPC, like Volt.

## Set up the tree

Clone [mini-source-sdk](https://github.com/Joshua-Ashton/mini-source-sdk), pick a branch (`sdk2013-mp`, `sdk2013-sp`, `asw`), and clone this repo into `src` with submodules:

```bash
git clone https://github.com/Joshua-Ashton/mini-source-sdk.git
cd mini-source-sdk/sdk2013-mp/src
git clone --recursive https://github.com/Asphaltian/VPhysics-Box3D.git vphysics_box3d
```

mini-source-sdk is wired for Volt, not Vox3D. Add two lines to `src/vpc_scripts/default.vgc`, projects first:

```
$Include "vphysics_box3d\vpc_scripts\vbox3d_projects.vgc"
$Include "vphysics_box3d\vpc_scripts\vbox3d_groups.vgc"
```

Everything below runs from `src`.

## Windows

Run `fix_registry.bat` once as admin, then generate the solution:

```bash
.\fix_registry.bat
devtools\bin\vpc.exe +vox3d /define:GAME_SDK2013 /mksln vox3d.sln
```

Build `vox3d.sln` in Visual Studio. Output lands in `game/bin`: `vphysics.dll` (a loader that picks the SSE2/SSE4.2/AVX2 variant per CPU) and the three `vphysics_box3d_*.dll`.

## Other targets

**Alien Swarm**: `asw` branch, `/define:GAME_ASW` instead. `compat/compat_asw.h` covers the interface gap.

**x64 and Garry's Mod**: mini-source-sdk's VPC can't emit x64. Use the full [source-sdk-2013](https://github.com/ValveSoftware/source-sdk-2013), wired the same way, plus `/define:VBOX_FULL_SDK`. GMod also needs `/define:GAME_GMOD` and CS:GO/GMod VPhysics + appframework headers, which aren't redistributable.
