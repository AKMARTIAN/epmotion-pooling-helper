# PoolMate

**PoolMate** is a browser-based equimolar pooling helper for planning **library
pools** and **dilutions**, and generating the Eppendorf **epMotion** worklist
CSVs (`DNA.csv` / `Buffer.csv`). Everything runs locally in the browser — **no
install, no server, and no data ever leaves your machine** (entries are kept
only in your browser's local storage).

## ▶ Use it now

**https://akmartian.github.io/poolmate/**

Open the link in any modern browser (Chrome, Edge, Safari, Firefox) on Windows,
macOS, or Linux. Nothing to install.

## What it does

- **Pooling** — enter sample concentrations (ng/µL + fragment size, or direct
  nM, with replicate-read averaging) and a target (nM or fmol). It computes the
  equal-mole transfer volume for each sample, the buffer top-up, and a
  post-rounding equimolar-deviation (CV%) check, then exports the epMotion
  `DNA.csv` and `Buffer.csv`.
- **Serial dilution** — when a sample is too concentrated to pipette within the
  plate/tube and tip limits, it automatically lays out the dilution steps. For
  pooling, this is exported as a separate ordered `Dilution.csv` that runs first
  (the robot mixes each well), and the pool then draws from the diluted wells.
- **Standalone dilution calculator** — dilute one sample or a whole **list** to
  a target concentration (nM or ng/µL). Shows DNA + buffer/TE/water volumes per
  sample, serial steps where needed, and the total diluent for the batch. Every
  parameter (target, final volume, min transfer, max well capacity, rounding)
  has a shared default that can be **overridden per row**.
- **Quality of life** — platform presets (Illumina / ONT) with editable loading
  targets, a live source-rack plate map, run metadata for records, print / save
  to PDF, save & reopen runs as JSON, import instrument CSVs, auto-named
  downloads so runs never overwrite, per-section and whole-page clear, and a
  fresh blank start each time you open it (with a one-click recovery banner if a
  previous session was left unsaved).

> ⚠️ The built-in platform presets are vendor starting points. **Confirm all
> volumes and loading concentrations against your own kit / instrument SOP
> before running anything on the robot.**

## Repository layout

| Path | What it is |
|------|------------|
| `index.html` | The live web app (this is what GitHub Pages serves). |
| `versions/` | Archived snapshots of each working version (`pool.html` → `pool_enhanced_v9.html` → `PoolMate_v10.html`). The newest snapshot mirrors `index.html` and serves as a fallback. |
| `Buffer.csv`, `DNA.csv` | The original Eppendorf epMotion worklist templates this matches. |
| `*.pdf` | User guides. |

## Editing

The web app is a single self-contained file: **`index.html`** (HTML + CSS +
vanilla JS, no build step, no dependencies). Edit it directly and refresh the
browser. The calculation engine is the `PoolCore` block near the top of the
`<script>`; it has no DOM dependencies and can be unit-tested under Node:

```bash
# extract PoolCore and run a quick check
node -e "$(awk '/const PoolCore=\(function/{f=1} f; /^}\)\(\);$/{exit}' index.html); console.log(PoolCore.fmt(2500,0))"
```

To publish a change: edit `index.html`, copy the working file to the next
`versions/PoolMate_v<N>.html` snapshot (so the last good version is always a
fallback), then `git commit` and `git push` — GitHub Pages rebuilds in under a
minute.

## License

MIT — see `LICENSE`.
