# Callio releases

Signed update artifacts and channel manifests for the Callio desktop app.

This repository is public so that installed copies of Callio can fetch updates
without carrying any credential. **It contains build output only** — no source
code, no keys, no internal assets. Callio's source lives in a separate private
repository.

## Channels

| Channel | Manifest | Who it is for |
| --- | --- | --- |
| Development | [`development/latest.json`](development/latest.json) | The team, frequent approved builds |
| Stable | [`stable/latest.json`](stable/latest.json) | Deliberately promoted releases only |

An installed Callio reads exactly one of these, chosen by the channel it was
built with and the preference in Settings → Updates. A development client never
falls through to the stable manifest, or the reverse.

## Verifying an artifact

Every installer is signed with Callio's updater key, and the matching public key
is embedded in the app. Tauri verifies the signature before installing; an
artifact that fails verification is refused. The `.sig` beside each installer is
the signature the manifest carries.

The signing private key is not in this repository, not in the app, and not in
any build log.

## Publishing

Releases are produced by the `Desktop release` workflow in the source
repository, triggered by a tag that names its channel:

    dev-v0.1.1-dev.2   ->  development
    v0.1.1             ->  stable

An ordinary push publishes nothing. Each release bundles the desktop frontend,
the Tauri shell and the `korelis-engine` sidecar built from the same commit, so
an update always installs a matching set.
