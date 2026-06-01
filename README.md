# omni-scfm-results

Published **results** of the [omni-scfm](https://github.com/btraven00/omni-scfm)
perturbation-prediction benchmark — versioned data artifacts, **decoupled from the
benchmark code**.

The benchmark repo holds the *code* (OmniBenchmark modules, methods, envs). This repo
holds the *outputs* of a run, so they can be cited, diffed across runs, and consumed by
the dashboard (and anyone else) purely by URL — without cloning or rerunning the
benchmark. The dashboard fetches these files at:

```
https://raw.githubusercontent.com/btraven00/omni-scfm-results/main/<file>
```

## Layout

| file | what |
|---|---|
| `scores.parquet` | per-`(dataset, method, perturbation, seed)` metrics — the leaderboard table |
| `scatter.parquet` | per-gene predicted-Δ vs observed-Δ points (the scatter grid) |
| `reference/published_single.csv` | the paper's published single-perturbation numbers (overlay) |
| `reference/published_double.csv` | the paper's published double-perturbation numbers (overlay) |
| `reference/gene_centrality.csv` | OmniPath gene-network centralities (extensions tab) |
| `manifest.json` | provenance: benchmark commit, date, datasets/methods/seeds, file sizes + md5 |

## Data contract

`scores.parquet` columns: `run_id, dataset, seed, method, perturbation, split,
n_genes, pearson, pearson_delta, l2`. Headline metric is `pearson_delta` on the
`test` split. `scatter.parquet` columns: `dataset, method, perturbation, gene,
observed_delta, predicted_delta`.

## Provenance

Every publish records the exact benchmark commit + file md5s in `manifest.json`, so a
result set is traceable to the code that produced it.

## Refreshing

From an omni-scfm checkout, after `ob run` + `pixi run collect`:

```bash
python scripts/publish_results.py /path/to/omni-scfm-results
cd /path/to/omni-scfm-results && git add -A && git commit -m "results: <run>" && git push
```
