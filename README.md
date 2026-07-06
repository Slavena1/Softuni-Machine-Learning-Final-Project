# Between Child and Machine: A Mandarin Case Study

Comparing word acquisition patterns across three "learners" of Mandarin — children (L1),
adult L2 learners, and a language model's frequency signal — to test whether adult L2
acquisition sits structurally closer to model-style statistical learning than child L1
acquisition does.

## Project structure

```
mandarin-acquisition/
├── data/                     # raw data files (not tracked — see "Data" below)
├── src/
│   ├── data_prep.py          # loading + cleaning for all 5 raw sources, and build_dataset()
│   ├── features.py           # spectrum_score()
│   └── modeling.py           # train/test splitting, model pipelines, error analysis
├── notebook/
│   └── analysis.ipynb        # the full analysis: EDA, models, results, discussion
├── requirements.txt
└── README.md
```

The notebook is the narrative and results layer. All data loading, cleaning, merging, and
modeling logic lives in `src/` as reusable, importable functions — it does not live inline
in notebook cells, so the same logic can be tested, reused, or re-run outside the notebook.

## Data

Place these five files in `data/` (not included in this repo due to size/licensing):

| File | Source |
|---|---|
| `wordbank_mandarin_items.csv` | Wordbank Beijing Mandarin CDI, https://wordbank.stanford.edu |
| `hsk1.csv` ... `hsk6.csv` | HSK vocabulary lists, https://github.com/plaktos/hsk_csv |
| `babylm_zh_frequencies.csv` | Precomputed word frequencies from the Chinese BabyLM corpus |
| `Concretenss_Ratings_of_9877_Two_Character_Chinese_Words.xlsx` | Xu & Li (2020) |
| `liu_2007_single_char.txt` | Liu et al. (2007), Chinese Single-character Word Database |

All files available to download here:
https://drive.google.com/drive/folders/13qEDS3YRXskJjXPesqNmPn9uImHmopVl?usp=drive_link

If `babylm_zh_frequencies.csv` is missing, `data_prep.load_babylm_frequencies()` will
recompute it from the raw BabyLM parquet corpus using `jieba` segmentation — this requires
`jieba` (see `requirements.txt`) and takes several minutes.

## Reproducing the analysis

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebook/analysis.ipynb
```

This runs the entire pipeline end-to-end: data loading → merging → EDA → Ridge/Lasso/Random
Forest models → feature importance comparison → error analysis → discussion. No manual steps
or hardcoded paths — all paths are relative to `notebook/`.

## Methodology notes

Train/test splitting is performed on raw, unscaled features. Feature scaling is fit only
inside `sklearn.Pipeline` objects (see `src/modeling.py`), so `StandardScaler` is refit on
each cross-validation fold's training data and never sees the held-out test set until the
final evaluation. `split_raw()` also returns train/test row indices, so predictions can be
traced back to specific words for the error analysis in Section 5.5.

## Key finding

Frequency is a substantially stronger predictor of adult L2 (HSK) acquisition order than of
child L1 (AoA) acquisition order — consistent across Spearman correlation, Ridge, Lasso, and
Random Forest. Concreteness, unexpectedly, does *not* show the reverse pattern as cleanly
predicted; see Section 6 of the notebook for a full discussion of why.
