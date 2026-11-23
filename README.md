# Resonance: A Plague Tale Legacy on AMD Polaris (Linux / Proton)

Patched **vkd3d-proton v3.1** (two DLLs) that lets *Resonance: A Plague Tale Legacy* run on AMD Polaris GPUs (RX 470/480/570/580/590) under Proton. No shader replacement, no proxy DLLs, no .ini files.

## Install
1. Download `d3d12.dll` and `d3d12core.dll` from [Releases](../../releases).
2. Copy both next to `Resonance.exe`. Proton loads them automatically.
3. Launch the game. The first start is slow (pipelines are compiled), later launches are faster.

If the patch doesn't load (no `POLARIS:` lines in the vkd3d log), set `WINEDLLOVERRIDES="d3d12,d3d12core=n,b"`.
To uninstall, delete the two DLLs.

## Tested
RX 580 8GB, CachyOS, Proton-CachyOS and GE-Proton11-12, Mesa 26.2.x / 26.3.1. Full playthrough, ~45 FPS.
Other Polaris cards, distros and Proton versions are untested.

## Known issues
- None

## Build
```
git clone --branch v3.1 --recurse-submodules https://github.com/HansKristian-Work/vkd3d-proton.git
cd vkd3d-proton && git apply ../patch/RX580-resonance-v3.1.patch
meson setup --cross-file build-win64.txt --buildtype release -Denable_tests=false -Denable_extras=false build
ninja -C build
```

## Thanks
Big thanks to [njtechyt](https://github.com/njtechyt) for the Crimson Desert Polaris fix this work is based on, and to [Vidith007](https://github.com/Vidith007) whose scheme it builds on. Thanks also to [ibr3himkhaled](https://github.com/ibr3himkhaled) for the Windows Polaris fix for this game, whose documentation helped to understand what Resonance needs on Polaris.

## License
LGPL-2.1-or-later (same as vkd3d-proton). See `NOTICE.md`. Unofficial, unaffiliated with the game's developers or publishers.
