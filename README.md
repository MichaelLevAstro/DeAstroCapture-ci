# DeAstroCapture-ci

Build runner for DeAstroCapture. No application source lives here.

GitHub bills Actions minutes to the repository a workflow runs in, and standard
runners are free on a public repository. This one checks the application out of
its own private repository with a read-only token, builds it, and leaves the
result as a workflow artifact - or, with `release`, publishes it.

Six legs: Windows (`Setup.exe`), macOS Apple Silicon and Intel (`.dmg`), Linux
x86_64 and 64-bit Raspberry Pi (`.tar.gz`), and the Android phone app (`.apk`,
from DeCapture-Mobile; `mobile_ref` picks its branch).

Manual trigger only, from the Actions tab or:

    gh workflow run build.yml -R MichaelLevAstro/DeAstroCapture-ci -f ref=main

A release (what the app's update check and dekoiastro.com read):

    gh workflow run build.yml -R MichaelLevAstro/DeAstroCapture-ci -f ref=main -f release=true

The release is named with its stage (`stage`, Alpha by default). Artifacts are
kept for fourteen days.

## Setup

`SOURCE_TOKEN`: a fine-grained personal access token limited to the
DeAstroCapture and DeCapture-Mobile repositories with `Contents: Read` and
nothing else. Fine-grained tokens expire, and an expired one shows up as a
checkout failure rather than anything more helpful.

The APK's signing key, the same one for every version (Android installs an
update only over an app signed with the same key - keep a copy of it):

- `ANDROID_KEYSTORE`: the PKCS12 keystore, base64
- `ANDROID_KEYSTORE_PASSWORD`: its password (the key's too)
- `ANDROID_KEY_ALIAS`: the key's alias
