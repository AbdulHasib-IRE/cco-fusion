# Class-conditional observability for multimodal fusion

Code and results for *Width-confounded modality importance in multimodal
fusion: per-class estimation before training and the width bias of learned
attention*.

The study asks whether a modality's **feature width** — how many columns it
contributes — changes which modality an importance estimator, or a trained
fusion gate, reports as more useful, when the information the modalities carry
is held equal by construction.

## What is here

```
CCO_Fusion.ipynb         every experiment, in execution order, outputs stripped
protocol.json            cross-validation protocol, grouping source, seeds
results/                 one JSON per experiment: the exact arrays the paper reports
results/_PROVENANCE.json generation stamp, corpus inventory, superseded-file list
```

`results/` is the authoritative record. Every number in the article is read from
these files; none is typed in by hand.

## What is deliberately not here

- No manuscript source, bibliography, class file, compiled PDF or submission
  bundle, and no copy of the article's figures or tables.
- No raw corpora. CMU-MOSEI, TCGA and the LAN-SDN-NIDS flow corpus are
  third-party resources with their own licences and are obtained from their
  original distributors. `protocol.json` and the notebook carry the
  identifiers, the feature definitions and the exact splits needed to
  reconstruct every array used in the study.

The synthetic generator is included in full, so every ground-truth result —
the five-operator comparison, the calibration, the gate arms and the rank
interventions — reproduces with no external download.

## Reproducing

1. Open `CCO_Fusion.ipynb` (Google Colab or a local Jupyter server).
2. Run the generator and operator sections end to end; they need only `numpy`,
   `scipy`, `scikit-learn`, `torch` and `dit`.
3. For the corpus sections, place the third-party feature files where the
   notebook's path cells expect them, then run those sections.
4. Compare against `results/`: each notebook section writes the same JSON keys.

`dit` pins matter. Install it with a constraints file that holds `numpy` and
`scipy` at the versions already in the environment, or the BROJA solver will
pull an incompatible `numpy`.

## Operators

| Operator | What it does to the block | Width held fixed |
|---|---|---|
| Deletion | drops every column | no |
| Width-matched deletion | drops a matched number of random columns | partly |
| Nullification `U*` | permutes the block's rows within itself | yes |
| Discretize-then-PID | bins the block, reads the BROJA unique atom | no |
| Equalise-by-subsampling | subsamples every block to a common width | no |

## Pushing this to GitHub

```bash
unzip cco-fusion-code.zip -d cco-fusion
cd cco-fusion
git init -b main
git add .
git commit -m "Code and results for the class-conditional observability study"
git remote add origin [https://github.com/GITHUB_USER/cco-fusion.git](https://github.com/AbdulHasib-IRE/cco-fusion)
git push -u origin main
```

Then connect the repository to Zenodo (Zenodo → GitHub → flip the switch for
`cco-fusion`), publish a release on GitHub, and Zenodo mints a DOI. Paste that
DOI into the manuscript's Data availability section and into `CITATION.cff`,
replacing `ZENODO_ID` and `GITHUB_USER` in both.

`.gitignore` blocks `*.tex`, `*.bib`, `*.pdf` and `data/`, so a paper file or a
raw corpus cannot be committed here by accident.

## Citation

See `CITATION.cff`. Please cite the article, and cite this deposit by its
Zenodo DOI when you use the code or the result files.

## Licence

MIT, see `LICENSE`. The third-party corpora keep their own licences.
