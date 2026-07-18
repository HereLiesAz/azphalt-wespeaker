# WeSpeaker

**WeSpeaker (speaker embedding)** — Maps a speech segment to a speaker-identity embedding vector.

An **azphalt** AI-model plugin, packaged as a `.azp` (the azphalt analogue of a VS Code `.vsix`). It is
named for the *model*, not a single feature — the same model powers many tools, and it is **host-neutral**:
any azphalt host that understands its role can use it, not just one app. Install it from any host's
**Azphalt Storefront**.

## What it can do

- speaker diarization
- speaker verification / matching
- grouping clips by who is talking

## Roles (host-neutral routing)

This plugin contributes the role(s): `speaker-embedding`. A host routes the model by role — it carries no
`targetApps`, so it is not tied to any single application.

**Example host — [Guillotine](https://github.com/HereLiesAz/Guillotine):** Desktop `diarizeEmbedModelPath` — `diarize_clip` (pair with pyannote-segmentation).

## Model file(s)

- **`embedding.onnx`** (role `speaker-embedding`) — [upstream](https://huggingface.co/onnx-community/wespeaker-voxceleb-resnet34-LM/resolve/main/embedding.onnx)

Model license: **Apache-2.0 (WeSpeaker, ResNet34 VoxCeleb)**. This plugin's manifest/packaging is `Apache-2.0`.

## How it works — the VSCode Header Pattern

The `.azp` does **not** bundle the weights. The manifest declares each model as a *remote asset*
(`"path": ""` + `remoteUrl` + `checksum` + `byteSize`); the host downloads the weights on install and
verifies them against the pinned SHA-256 — exactly how a large VS Code extension fetches its language
server instead of shipping it inside the `.vsix`. `remoteUrl` points at this repo's own GitHub **Release**
asset (named the exact filename the host expects); the `release` workflow fetches the upstream model,
renames it, checksums it, and publishes it beside the packed `.azp`.

## Build / release

```sh
npm install && npm run build     # packs com.hereliesaz.azphalt.wespeaker-1.0.0.azp
git tag v1.0.0 && git push --tags   # runs the release workflow: hosts the model + .azp
```
