# Data

Raw data files are not tracked in this repository (see root `.gitignore`) due to size and licensing.

Download all files from the project's Google Drive folder:
https://drive.google.com/drive/folders/13qEDS3YRXskJjXPesqNmPn9uImHmopVl?usp=drive_link

Place them here, in this `data/` folder, before running the notebook:

| File | Source |
|---|---|
| `wordbank_mandarin_items.csv` | Wordbank Beijing Mandarin CDI |
| `hsk1.csv` ... `hsk6.csv` | HSK vocabulary lists |
| `babylm_zh_frequencies.csv` | Precomputed Chinese BabyLM word frequencies |
| `Concretenss_Ratings_of_9877_Two_Character_Chinese_Words.xlsx` | Xu & Li (2020) concreteness norms |
| `liu_2007_single_char.txt` | Liu et al. (2007) concreteness norms |

Optional: `train-00000-of-00001.parquet` (raw Chinese BabyLM corpus), only needed if `babylm_zh_frequencies.csv` is missing and frequencies must be recomputed from scratch.
