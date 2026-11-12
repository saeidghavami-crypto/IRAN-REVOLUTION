# Iran’s unfinished revolutions

## Woman, Life, Freedom and the Lion-and-Sun Revolution

This is the reproducibility package for the research article examining why the two Iranian revolutions had not transferred state power by 15 September 2026 and which conditions merit assessment in a future transition. It also examines the wider relevance of these cases to challenges to dictatorship and the distinct problem of building durable democracy. It contains source records, analysis code, public historical campaign data, numerical results, references, and figures.

Authors: Iman Beheshti, Bogna Wach, Laurent Cherlonneix, Marek Łos, and Saeid Ghavami. Full affiliations are provided in the article and Supplementary Information.

The project has three connected components. Historical comparison examines documented events and possible mechanisms in Iran. Text analysis describes the topics and vocabulary of the assembled evidence. Comparative machine learning evaluates recorded early campaign conditions against observed historical outcomes. These components answer different parts of the research questions; their rows are not interchangeable observations.

## Get the files

Repository: [saeidghavami-crypto/IRAN-REVOLUTION](https://github.com/saeidghavami-crypto/IRAN-REVOLUTION).

To download without using Git, open the repository link, select **Code → Download ZIP**, and extract the ZIP file. Open a terminal inside the extracted folder containing this README.

If Git is installed, use:

```bash
git clone https://github.com/saeidghavami-crypto/IRAN-REVOLUTION.git
cd IRAN-REVOLUTION
```

## Choose a starting point

| Your aim | What to do | Additional source material needed? |
| --- | --- | --- |
| Inspect the reported results | Open the CSV files in `outputs/` and the figures in `figures/`; run `python code/check_package.py --public-package` to check the package. | No. |
| Reproduce the historical campaign models | Install the pinned Python environment, then run `python code/navco/run_analysis.py`. | No. The licensed input data are included. |
| Regenerate the figures | Install the pinned environment, then run `python code/make_figures.py`. | No. The numerical inputs are included. |
| Reproduce text extraction and text analysis | Obtain the registered source versions and follow the extraction commands below. | Yes. Copyrighted source texts are not distributed. |

Run all commands from the repository's top-level folder, alongside this README. Re-running an analysis writes to its existing output folder. Keep an unchanged copy of the supplied results if you want to compare them with a local rerun.

## What the different numbers mean

| Number | Meaning |
| --- | --- |
| **205 source identities** | The substantive and methodological sources accounted for in the source register. An identity refers to a particular source, not an independent political event. |
| **182 complete English texts** | The texts eligible for the computational mapping: 133 complete sources and 49 complete, explicitly bounded contributions selected from larger files. Abstracts, incomplete texts and non-English sources are not silently substituted for full English texts. |
| **208 bibliography entries** | The 205 source identities plus three linked correction notices. Each notice is placed immediately after its original article. These notices do not increase the text-analysis denominator. |
| **384 campaigns / 2,717 campaign-years** | The observations in the official deposited NAVCO 2.1 source dataset, covering 1945–2013. |
| **127 campaigns / 75 country groups** | The primary outcome-analysis cohort: campaigns entering with regime-change goals, continuing beyond their onset year, with no onset-year success and an onset no later than 2008. |
| **43 five-year successes** | NAVCO-coded attainment of a stated maximalist goal during the five calendar years after onset in the 127-campaign cohort. This does not establish democratic consolidation or permanent failure in the other campaigns. |

The stable source identifier, such as `R006`, is not necessarily its final citation number. `references/reference_crosswalk.csv` provides the mapping. The numerical citation order is shared by the crosswalk, numbered reference list, RIS file and CSL JSON file.

## Start here: check the package without installing scientific software

Unzip the package and open a terminal in its root folder, which contains this README and the `code` folder. Run:

```bash
python code/check_package.py
```

The checker uses Python’s standard library. It verifies public files, source and citation mappings, correction-notice adjacency, corpus cardinalities, numerical summaries, held-out prediction consistency, source-file checksums and figure-file existence. It does not require the private source texts, contact external websites, rerun the models or validate every historical claim.

To save the check results and a SHA-256 inventory of the public package:

```bash
python code/check_package.py --report package_check.json --manifest public_checksums.sha256
```

For a package being prepared for public release, add `--public-package`. This also checks that private recovery directories and identifiable source-text files have not been included:

```bash
python code/check_package.py --public-package --report package_check.json --manifest public_checksums.sha256
```

The checker returns exit code 0 when all checks pass and a nonzero exit code when a check fails. It reports the failed item instead of modifying inputs or suppressing the discrepancy. Checksums identify file versions; they do not establish that a scientific interpretation is correct. Private source hashes recorded in the register cannot be reverified without the lawful originals.

## Install the analysis environment

The supplied results used Python **3.12.14**. Exact scientific-package versions are pinned in the root `requirements.txt`.

On Linux or macOS:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

On Windows Command Prompt, activate the same environment with `.venv\Scripts\activate.bat`; in PowerShell, use `.\.venv\Scripts\Activate.ps1`. If activation is unavailable, call the environment's Python directly, for example `.venv\Scripts\python.exe -m pip install -r requirements.txt`, and use that executable for the analysis commands. Keep the folder structure unchanged. No paid account or API key is required for the public historical campaign analysis.

## Reproduce the historical campaign models

The public NAVCO input data and codebook are included under their **CC0 1.0** license. The optional download command verifies the existing files and downloads them only if absent:

```bash
python code/navco/download_data.py
python code/navco/run_analysis.py
```

The first command checks pinned official-file hashes. The second rebuilds campaign eligibility, onset-year features, future outcomes, country-held-out predictions, bootstrap intervals, sensitivity analyses and the NAVCO plots. The model settings and random seeds are fixed; there is no hyperparameter search.

Expected principal results:

| Analysis | Logistic regression AUROC | Restricted forest AUROC | Logistic / forest Brier score |
| --- | ---: | ---: | ---: |
| Primary: 127 campaigns, 43 successes | 0.632 | 0.742 | 0.234 / 0.194 |
| Separate nonviolent-only refits: 47 campaigns, 29 successes | 0.498 | 0.451 | 0.274 / 0.238 |

The primary prevalence benchmark has Brier score **0.224**; the nonviolent-only benchmark has Brier score **0.237**. Primary AUROC country-bootstrap intervals are **0.529–0.734** for logistic regression and **0.645–0.833** for the forest. Nonviolent-only intervals are **0.343–0.650** and **0.265–0.620**, respectively.

AUROC describes ranking. Brier score measures probability accuracy, with smaller values indicating better accuracy. The nonviolent-only refits did not demonstrate useful discrimination in this small subgroup; their wide intervals do not prove that predictive information is absent. Neither model produces a success probability for either contemporary Iranian revolution. Adding documents does not enlarge the 127-campaign prediction sample. NAVCO goal attainment is not a democracy measure: the democracy discussion uses separately identified comparative studies and institutional evidence without merging incompatible outcomes or claiming a new democracy model.

The temporal check trains on onsets through 1984, whose five-year labels are observable by 1989, and tests on onsets from 1990 onward. It excludes 1985–1989 onsets from training to avoid overlap between training-label windows and the later period. A stricter version also excludes training countries represented in the later test set.

See `code/navco/README.md` for complete model definitions, codebook meanings, uncertainty assumptions and interpretation. `data/navco/source_manifest.json` records the source DOI, license, release and original-file hashes. The deposited file has 384 campaigns; the associated journal abstract reports 389. The analysis is pinned to the deposited file and does not assume the reason for that difference.

## Reproduce the text analysis after obtaining lawful source texts

The public package does **not** distribute the copyrighted full-text corpus, downloaded articles, or extracted full texts. It supplies source identities, URLs, selected boundaries, extraction instructions, hashes and derived numerical results. Anyone rerunning text extraction must obtain the documented source versions under the terms applicable to those sources.

The numbered PDF collection used for extraction contains 118 files, with numbers 1–119 and 33 absent. PDF 89 duplicates the source represented by PDF 52 and is not ingested again. This is a file-recovery detail: 118 PDF files are neither the full source-register denominator nor the final text-analysis denominator. Additional sources are recovered through **72** explicit recipes. The 118 numbered files and 72 recipes describe input recovery, rather than the source-register or analytical denominators.

Place the lawfully obtained numbered PDFs in `private/original_pdfs/`. PDF extraction requires Poppler’s `pdftotext` executable and Tesseract with English language data for the recorded scanned contribution. The supplied extraction was run with **pdftotext 25.03.0** and **Tesseract 5.5.0**. These are separate system tools, not Python packages.

```bash
python code/corpus/extract_sources.py \
  --plan code/corpus/inputs/pdf_extraction_plan.json \
  --pdf-dir private/original_pdfs \
  --private-text-dir private/source_texts \
  --log-csv outputs/corpus/pdf_extraction_log.csv
```

Recover the additional documented sources from their registered public URLs, or place already obtained raw files in `private/retrieval/` and omit `--download`:

```bash
python code/corpus/recover_additional_sources.py \
  --recipes code/corpus/inputs/additional_source_recipes.json \
  --raw-dir private/retrieval \
  --output-dir private/source_texts \
  --download
```

Once the complete eligible texts are present with their expected hashes:

```bash
python code/corpus/analyze_corpus.py \
  --study-root . \
  --source-register code/corpus/inputs/source_register.csv \
  --lexicon code/corpus/inputs/dictionaries.json \
  --reference-crosswalk references/reference_crosswalk.csv \
  --publication-cutoff 2026-09-15 \
  --out outputs/corpus
```

Do not replace an inaccessible source with a title, abstract, search snippet or reconstructed text. If a source version or its extracted body has changed, investigate and document the difference. A changed source should not silently enter the registered analysis. The scripts reject missing eligible texts and mismatched hashes. OCR and PDF-tool differences can also affect exact recovery; consult the detailed extraction records before changing the source register.

Expected text-analysis results are **182 eligible source units**: **94** concern Woman, Life, Freedom, **40** concern the Lion-and-Sun Revolution, and **48** provide comparative or contextual evidence. They contain **1,809,753 raw words**, **1,799,131 normalized words**, and **7,000 TF-IDF features** reduced to **35 SVD dimensions**. The selected partition has **3 clusters**, containing **44, 22 and 116 sources**. Mean silhouette across five seeds is approximately **0.145457**, and median pairwise adjusted Rand agreement is approximately **0.977009**. Repression vocabulary appears in **151/182 sources (83.0%)**. These describe the selected texts, not causal effects or revolutionary capacity. High agreement across random seeds does not resolve weak cluster separation or source-composition sensitivity.

The primary corpus may use a later-published source retrospectively when it documents events within the fixed event interval. A publication-cutoff sensitivity retains **180** texts with documented publication intervals ending by 15 September 2026. R026 is excluded conservatively because the advance version's availability before the cutoff is unverified; the official index distinguishes a 14 September report date from 21 September issuance. R164 is undated. Both remain in the primary corpus. Month-only and year-only dates are handled as full intervals; no missing day is invented. Publication dates are not substituted for event dates.

See `code/corpus/README.md` for the normalization rules, exact phrase matching, cluster selection, source-composition sensitivities and extraction boundaries. An optional source-inspection step recovers the documented English abstract associated with R118:

```bash
python code/corpus/extract_auxiliary_texts.py \
  --plan code/corpus/inputs/auxiliary_text_extraction.json \
  --pdf-dir private/original_pdfs \
  --private-text-dir private/source_texts
```

This abstract is not an eligible full text and is not added to the 182-source analytical denominator.

## Regenerate the publication figures

The supplied numerical outputs are sufficient to regenerate the figures without obtaining copyrighted texts:

```bash
python code/make_figures.py
```

This writes the graphical abstract, Figures 1–7 and Supplementary Figures S1–S3 as PNG, PDF and SVG files in `figures/`. Conceptual panels use visual depth. Statistical plots retain undistorted axes; the three-dimensional source plot uses actual SVD coordinates. The NAVCO script separately saves its detailed validation and coefficient plots in `outputs/navco/`.

Do not rerender figures from modified data and retain the original numerical interpretation without checking the changes. PDF metadata or environment-specific rendering can alter image-file hashes even when plotted numbers are unchanged; numerical output verification and visual inspection serve different purposes.

## Where to find the results

| Folder or file | Contents |
| --- | --- |
| `code/corpus/` | Text recovery, source mapping, dictionary and clustering code; detailed README and extraction plans. |
| `code/navco/` | Official-data downloader, campaign analysis, fixed analysis plan and detailed README. |
| `code/make_figures.py` | Publication-figure generator using the supplied numerical outputs. |
| `code/check_package.py` | Offline checks of public-package identity, mappings, counts and numerical consistency. |
| `data/navco/` | Licensed historical data, official codebook, source manifest and predictor dictionary. |
| `outputs/corpus/` | Source accounting, numerical word/phrase counts, clusters, sensitivities and run summary. |
| `outputs/navco/` | Campaign-level analysis data, held-out predictions, metrics, bootstrap summaries and sensitivities. |
| `references/` | Final numbered references, stable-ID crosswalk, Zotero-compatible RIS and CSL JSON. |
| `references/audit/` | Source locators, case-mechanism framework, DOI metadata audit, reference-correction decisions and correction-notice records. |
| `figures/` | The graphical abstract and all main/supplementary figures. |

Import `references/references_zotero.ris` into Zotero using **File → Import**. The RIS and CSL JSON files contain bibliography records, not active citation fields inside Word. Use the crosswalk when checking final manuscript citation numbers; do not assume a source ID’s numerical suffix is its final bibliography position.

The audit folder includes `source_search_log.json`, `september_source_ledger.json`, `comparative_source_ledger.json`, `september_and_democracy_sources.json`, `source_extension_id_map.json`, `mechanism_source_locators.csv`, `mechanism_case_framework.csv`, `reference_doi_metadata_audit.csv`, `suggested_reference_patches.json`, and `proposed_correction_notices_csl.json`. These preserve the locations, metadata checks and decisions supporting the source audit. The files whose names contain “suggested” or “proposed” are audit records; the final bibliography and its three adjacent correction notices are already reflected in the crosswalk and Zotero files. Import the final RIS file when building the submission bibliography. The audit records contain metadata and source-linked summaries, not the copyrighted full corpus or a cache of article abstracts.

## Interpretation and reuse

The text component describes an evidence collection. The campaign component tests historical predictive associations. Neither a frequently mentioned term, a source cluster, a model coefficient nor a moderate AUROC identifies a necessary or sufficient condition for future political success. The Iranian cases remain documented comparative cases; no invented numerical profiles or model-based future-success forecasts are assigned to them.

Please cite the article and the original NAVCO dataset when reusing this work. `CITATION.cff` supplies the title, authors and repository citation metadata; it does not assign an article DOI or claim a journal publication status. The original data license is recorded in the data manifest. Copyright in separately published source texts remains with the applicable rights holders. Keep privately obtained texts and recovery caches out of a public GitHub repository; publish the code, metadata and derived outputs provided here.

The supplied `.gitignore` excludes private source folders, Python caches and local virtual environments. It does not replace review of the files before publication. No new blanket license is assigned to the author-created code by this package; the original NAVCO data’s CC0 license remains separately documented.
