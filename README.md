# dspico

GitHub Actions workflow that automatically builds all DSpico release artifacts when a new GitHub Release is published.

## Artifacts produced

| File | Source |
|------|--------|
| `DSpico.dldi` | [dspico-dldi](https://github.com/LNH-team/dspico-dldi) |
| `default.nds` | dspico-bootloader → DLDI-patched → DSRomEncryptor |
| `DSpico.uf2` | [dspico-firmware](https://github.com/LNH-team/dspico-firmware) |
| `LAUNCHER.nds` | [pico-launcher](https://github.com/LNH-team/pico-launcher) |
| `uartBufv060.bin` | [dspico-wrfuxxed](https://github.com/LNH-team/dspico-wrfuxxed) (Wrfuxxed exploit) |
| `picoLoader7.bin` | [pico-loader](https://github.com/LNH-team/pico-loader) |
| `picoLoader9_DSPICO.bin` | pico-loader |
| `_pico/data/aplist.bin` | pico-loader |
| `_pico/data/savelist.bin` | pico-loader |

## Required secrets

Before publishing a release, add these two secrets to the repository
(**Settings → Secrets and variables → Actions → New repository secret**):

| Secret | Description |
|--------|-------------|
| `NTR_BLOWFISH_B64` | Base64-encoded `ntrBlowfish.bin` (SHA1: `84E467F2485078E401A17A5F231E3FE6E9686648`) or `biosnds7.rom` |
| `TWL_BLOWFISH_B64` | Base64-encoded `twlBlowfish.bin` (SHA1: `2DEA11191F28C6CC1956DADB8941AFFD4B2B5102`) or `biosdsi7.rom` |

Encode a file on macOS:

```bash
gzip -c ntrBlowfish.bin | base64 -i - | pbcopy   # copies to clipboard, paste as NTR_BLOWFISH_B64
gzip -c twlBlowfish.bin | base64 -i - | pbcopy   # copies to clipboard, paste as TWL_BLOWFISH_B64
```

These are proprietary Nintendo files and cannot be included in the repository.
See the [DSRomEncryptor README](https://github.com/Gericom/DSRomEncryptor) for details.

## Optional: Wrfuxxed support

To include the Wrfuxxed exploit for unmodified DSi/3DS systems, edit the
**"Prepare firmware ROM assets"** step in `.github/workflows/build.yml`:

1. Add a `WRFU_TESTER_B64` secret containing the base64-encoded WRFU Tester v0.60 ROM
   (SHA1: `2d65fb7a0c62a4f08954b98c95f42b804fccfd26`).
2. Uncomment the three lines inside that step.

## Triggering a build

[Publish a GitHub Release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository).
The workflow triggers on `release: published` and uploads all artifacts directly to that release.
