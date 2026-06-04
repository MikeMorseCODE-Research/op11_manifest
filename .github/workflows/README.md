# CI — halium_salami build

## Requirements (self-hosted runner)

| Resource | Minimum | Recommended |
|---|---|---|
| Disk | 280 GB free | 400 GB |
| RAM | 32 GB | 64 GB |
| CPUs | 8 | 16–32 |
| OS | Ubuntu 22.04 | Ubuntu 22.04 / 24.04 |

## Registering a self-hosted runner

1. Go to **Settings → Actions → Runners → New self-hosted runner** on this repo (or org).
2. Follow the GitHub instructions to download and configure the runner agent.
3. Add the labels **`android-build`** and **`linux`** when prompted (the workflow uses `runs-on: [self-hosted, linux, android-build]`).
4. Start the runner: `./run.sh` (or install as a service with `./svc.sh install && ./svc.sh start`).

## Running a build

Go to **Actions → Build halium_salami → Run workflow**.  
Optional inputs:
- **build_type**: `userdebug` (default) or `eng`
- **sync_flags**: extra flags passed to `repo sync` (e.g. `--force-sync` after a manifest change)

## Output

On success, the workflow uploads a **`halium_salami-userdebug-<run>`** artifact containing:
```
boot.img          vendor_boot.img   dtbo.img
vbmeta.img        system.img        vendor.img
vendor_dlkm.img   odm.img           system_ext.img
product.img       flash-all.sh
```

Download the artifact, unzip it, and run:
```bash
bash flash-all.sh
```
with the device in fastboot mode.

## ccache

ccache is stored at `../ccache` relative to the workspace (persists across runs on the same runner). First build: ~3–5 hours. Subsequent builds with cache hits: ~45–90 min.
