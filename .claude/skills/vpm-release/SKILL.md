---
name: vpm-release
description: Publish new versions of the LogicCuteGuy VPM packages and refresh the VCC listing at vpm.logiccuteguy.com. Use when the user says they updated package repos, wants packages updated in VCC, or asks to release/rebuild the VPM listing.
---

# VPM release + listing refresh

This repo (`LogicCuteGuy/vpm-listing`) only holds the listing. VCC reads
`https://vpm.logiccuteguy.com/index.json`, which the **Build Repo Listing**
workflow regenerates from the **GitHub releases** of the repos in `source.json`
(`githubRepos`). Commits and pushes in a package repo never reach VCC; only a
new release does.

## 1. Find what needs a release

For each repo in `source.json`, compare the latest release tag with `HEAD`:

```bash
gh release list --repo LogicCuteGuy/<Repo> --limit 1
gh api repos/LogicCuteGuy/<Repo>/compare/<tag>...HEAD --jq '"ahead \(.ahead_by): " + ([.commits[]|.commit.message|split("\n")[0]]|join(" | "))'
```

- `ahead 0`, or only README/docs commits: no release needed, say so.
- Code/package.json changes: release needed.
- Check `package.json` at HEAD: `name` must match the listing, `version` must be
  higher than the last release. If not bumped, bump it (pre-1.0: patch for
  fixes/compat, minor for features) and say which number you chose.

## 2. Release a package repo

Packages sit at the repo root, with no release workflow. Clone into the scratchpad, never into this repo.

```bash
# in the clone, after the package.json version bump is committed and pushed
name=$(node -p "require('./package.json').name"); ver=$(node -p "require('./package.json').version")
git archive --format=zip -o "../$name-$ver.zip" HEAD     # package.json at zip root
unzip -p "../$name-$ver.zip" package.json | grep "\"$ver\""
git tag "<tag>" && git push origin "<tag>"
gh release create "<tag>" "../$name-$ver.zip" package.json --repo LogicCuteGuy/<Repo> --title "<Repo> $ver" --notes "<summary>" --latest
```

Release requirements (the listing build fails or skips otherwise):

1. Asset named exactly `package.json`.
2. Zip named `<package.name>-<package.version>.zip` with `package.json` at the zip root.

Match each repo's existing tag style: DigHoleIt and BlendProperty use `v0.2.1`;
WASM2HLSL, UIHUD, UnityHelpTools and LCGUdonSharp use bare `0.2.1`.

**LCGUdonSharp is special.** Never use `git archive` for it. Use its own release
workflow or `Tools~/build_release.py` (needs `Payload~/UdonSharp`, `Samples~`).

## 3. Rebuild the listing

```bash
gh workflow run build-listing.yml --repo LogicCuteGuy/vpm-listing
id=$(gh run list --repo LogicCuteGuy/vpm-listing --workflow build-listing.yml --limit 1 --json databaseId -q '.[0].databaseId')
gh run watch $id --repo LogicCuteGuy/vpm-listing --exit-status
```

Verify the live index (wait ~20s for Pages):

```bash
curl -s https://vpm.logiccuteguy.com/index.json | node -e 'let s="";process.stdin.on("data",d=>s+=d).on("end",()=>{for(const[k,v]of Object.entries(JSON.parse(s).packages))console.log(k,Object.keys(v.versions).join(", "))})'
```

Adding a brand-new package repo: append it to `githubRepos` in `source.json`
and push to `main` (the workflow also runs on that push).

## 4. Wrap up

- Update the package table in `README.md` to the live versions (latest in bold).
- Report per package: released version, skipped and why, and any version number you picked.
- Releases, tags, and pushes to package repos are outward-facing: only do them when the user asked to publish/update.
