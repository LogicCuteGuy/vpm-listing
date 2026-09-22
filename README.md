# LogicCuteGuy VPM Listing

VPM repository listing for VCC / ALCOM, built with [vpm-listing-site-generator](https://github.com/Narazaka/vpm-listing-site-generator).

## Add to VCC / ALCOM

```
https://vpm.logiccuteguy.com/index.json
```

## Packages

| Package | Repo | Versions |
| --- | --- | --- |
| `com.logiccuteguy.lcgudonsharp` (LCGUdonSharp) | [LogicCuteGuy/LCGUdonSharp](https://github.com/LogicCuteGuy/LCGUdonSharp) | 0.2.0 |
| `com.logiccuteguy.helptools` (LogicCuteGuy Help Tools) | [LogicCuteGuy/UnityHelpTools](https://github.com/LogicCuteGuy/UnityHelpTools) | 1.0.0, 1.0.1 |

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
