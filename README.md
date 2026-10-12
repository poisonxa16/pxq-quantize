# pxq-quantize

The tool that **writes** PXQ files for the [pxa engine](https://github.com/poisonxa16/pxa).
The engine reads and runs PXQ files; this writes them. It is a separate download because the
engine release carries no encoder. This repository holds releases only: grab the binary from the
[Releases page](https://github.com/poisonxa16/pxq-quantize/releases).

- **Use the matching pair.** `pxq-quantize vX` writes files for `pxa vX`. Files written by older
  versions keep loading in newer engines.
- **CPU only.** No GPU, no CUDA, no driver. Linux x86_64, Ubuntu 22.04 / Debian 12 / RHEL 9 or
  newer.

## Use

```bash
tar xzf pxq-quantize-v2026.09.20-linux-x86_64.tar.gz
cd pxq-quantize-v2026.09.20
./pxq-quantize model-f16.gguf model-PXQ4.gguf PXQ4
```

The arguments are the ones `llama-quantize` takes, unchanged:

```
./pxq-quantize [options] <model-f32-or-f16.gguf> <out.gguf> <TYPE> [nthreads]
```

Tiers: `PXQ4`, `PXQ4HQ`, `PXQ3`, `PXQ2`, `PXQ6`, `PXQ6R`, and the mixed `PXQU` recipes.
`./pxq-quantize --help` prints the full list the build supports. Everything that is not a PXQ tier
(`Q4_K_M` and the rest of the stock types) is still done by `llama-quantize` in the engine package.

## Better files from an importance matrix (optional)

Give the quantizer an importance matrix collected on **chat-formatted** text and the file comes out
the same size and the same speed, but closer to the original model:

```bash
PXA_PXQ_IMX=1 ./pxq-quantize --imatrix my-model-chat-calib.imatrix model-f16.gguf model-PXQ4.gguf PXQ4
```

On Qwen3-0.6B PXQ4 that moved KLD against Q8_0 from 0.0818 to 0.0609; on Gemma 4 26B-A4B PXQ3 from
0.383 to 0.320. It is off by default because collecting the matrix costs GPU time (about half an
hour for a mixture-of-experts model). The full walk-through is tutorial 04 in the engine's docs.

## Verify a download

Every release carries a `.sha256` file next to the tarball:

```bash
sha256sum -c pxq-quantize-v2026.09.20-linux-x86_64.tar.gz.sha256
```

## Community

- Discord: https://discord.pxanetwork.com
- Support the work: https://ko-fi.com/shatteredrealms1
