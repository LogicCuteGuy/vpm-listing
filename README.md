# LogicCuteGuy VPM Listing

VPM repository listing for VCC / ALCOM, built with [vpm-listing-site-generator](https://github.com/Narazaka/vpm-listing-site-generator).

## Add to VCC / ALCOM

```
https://vpm.logiccuteguy.com/index.json
```

## Packages

| Package | Repo | Versions |
| --- | --- | --- |
| `com.logiccuteguy.blendproperty` (Blend Property) | [LogicCuteGuy/BlendProperty](https://github.com/LogicCuteGuy/BlendProperty) | 0.1.0, 0.1.1, 0.2.0, 0.2.1, **0.2.2** |
| `com.logiccuteguy.clientnetsim` (ClientSim Net) | [LogicCuteGuy/ClientSimNet](https://github.com/LogicCuteGuy/ClientSimNet) | 0.1.0, 0.1.1, **0.1.2** |
| `com.logiccuteguy.digholeit` (DigHoleIt - Diggable Voxel Terrain) | [LogicCuteGuy/DigHoleIt](https://github.com/LogicCuteGuy/DigHoleIt) | 0.4.0, 0.5.0, 0.6.0, 0.6.1, 0.7.0, 0.7.1, **0.7.2** |
| `com.logiccuteguy.lcglocalization` (LCG Localization) | [LogicCuteGuy/LCGLocalization](https://github.com/LogicCuteGuy/LCGLocalization) | **0.1.0** |
| `com.logiccuteguy.lcgudonsharp` (LCGUdonSharp) | [LogicCuteGuy/LCGUdonSharp](https://github.com/LogicCuteGuy/LCGUdonSharp) | 0.2.0, 0.3.2, 0.3.3, 0.3.4, 0.3.5, 0.3.6, 0.3.7, 0.3.8, 0.3.9, 0.3.10, 0.3.11, 0.3.12, **0.5.0** |
| `com.logiccuteguy.helptools` (LogicCuteGuy Help Tools) | [LogicCuteGuy/UnityHelpTools](https://github.com/LogicCuteGuy/UnityHelpTools) | 1.0.0, **1.0.1** |
| `com.logiccuteguy.openworld` (OpenWorld - Streamed Chunk World Engine) | [LogicCuteGuy/OpenWorld](https://github.com/LogicCuteGuy/OpenWorld) | **0.0.1** |
| `com.logiccuteguy.streamdata` (StreamData - Web Texture Streaming) | [LogicCuteGuy/StreamData](https://github.com/LogicCuteGuy/StreamData) | **0.0.1** |
| `com.logiccuteguy.zonedraw` (ZoneDraw - Click-to-Draw Zones & Barriers) | [LogicCuteGuy/ZoneDraw](https://github.com/LogicCuteGuy/ZoneDraw) | **0.1.0** |
| `com.logiccuteguy.uihud` (UIHUD) | [LogicCuteGuy/UIHUD](https://github.com/LogicCuteGuy/UIHUD) | 0.1.0, 0.1.1, 0.1.2, **0.2.0** |
| `com.logiccuteguy.wasm2hlsl` (WASM2HLSL for LCGUdonSharp) | [LogicCuteGuy/WASM2HLSL](https://github.com/LogicCuteGuy/WASM2HLSL) | 0.1.0, 0.2.0, 0.2.1, **0.2.2** |
| `com.unity.scriptablebuildpipeline` (LCGUdonSharp compatibility) | [LogicCuteGuy/LCGUdonSharp](https://github.com/LogicCuteGuy/LCGUdonSharp/releases/tag/sbp-compatibility-1.21.26) | **1.21.26** |

## Rebuild locally

```powershell
npm ci
$env:GITHUB_TOKEN = (gh auth token)
npx vpm-listing   # regenerates index.json from GitHub releases
npx vpm-site      # builds the site into dist/
```

## Package repo release requirements

Every GitHub release of a listed repo must include:

1. An asset named exactly `package.json`
2. A zip named `<package.name>-<package.version>.zip` (e.g. `com.logiccuteguy.helptools-1.0.2.zip`), with `package.json` at the zip root

After publishing a release, trigger a rebuild with a `repository_dispatch` of type `build-listing` to this repo, or run the **Build Repo Listing** workflow manually.

For LCGUdonSharp, push a `0.*` tag that matches `package.json`: the package repository's release workflow validates the metas, zips the package folder with `package.json` at the root, and publishes the zip plus `package.json`.
