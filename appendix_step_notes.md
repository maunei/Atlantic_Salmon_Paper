# Appendix A. The step notes (README of each step)

The notes are reproduced as written in the step folders, one collapsible block per step. They hold the details that the step sections summarise: exact file names, parameters, check results and run times.


<details>
<summary>Step 00: Raw inputs (<code>00_raw_inputs/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Fix, in one place, everything that is taken as given: the raw table, the two hand-made lists, the authors' release, the deposited derived files (for comparison only) and the authors' parameters, with checksums |
| **Reads** | the extracted deposit (`00_atlantic_salmon_paper_reproducibility/zips/`) and the paper |
| **Scripts** | `scripts/00_make_inputs.sh` (class D, shell), `scripts/00_check.sh` (class D), `scripts/run_00_raw_inputs.sbatch`; `authors_parameters.tsv` and the `new_*` columns of `lists/reassigned_ids.tsv` are written by hand from the scripts and the paper |
| **Parameters we chose** | none |
| **Provenance** | `[AUTHORS']` the data, the lists, the scripts and every value in `authors_parameters.tsv`; `[OURS]` the links, the checksum list, `reassigned_ids.tsv` (old values looked up in the raw table, new values read from `reassign_ind()`), the check |

**Contents**


* `raw/AllDataCombined_NotFiltered.txt`: link to the raw table (1,612 fish, 51 loci).
* `lists/PUY_Ind_Cluster_With_ROM_To_Remove.txt`: the 9 Puyjalon IDs; `lists/reassigned_ids.tsv`: the 4 reassigned IDs with old and new values.
* `authors_release/`: `Scripts/`, `Functions/`, `README.md`, `zip_link.txt` (links).
* `deposit_for_comparison/`: the 26 derived files of the deposit's `Data/` and the saved `divMigrate` result (`AnalysisResults/`); used only for checks, never as inputs.
* `paper/`: the paper and the supplement.
* `authors_parameters.tsv`: every parameter, with the file and lines (or the paper pages) where it is stated, a token that the check looks for there, and the step that uses it.
* `SHA256SUMS.txt`, `SOURCES.md`, `output/` (check table and logs).

**Check and level of reproducibility**


`output/check_00.tsv`: the links resolve; the checksums equal those of the earlier list (`01_planning/prelim_checks/authors_release_SHA256SUMS.txt`); the raw table has 1,612 fish and 122 columns; the 9 listed IDs and the 4 reassigned IDs are found in the raw table, and the old ecotype of each reassigned fish agrees with the direction of its move; every cited script line contains the cited token and every cited paper sentence is in the text of PDF pages 4–6. Level of reproducibility: not applicable.

**Status**


See `STATUS.tsv` of the workflow folder.

</details>


<details>
<summary>Step 01: Filter the main data set (<code>01_filter_main/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Filter the genotypes, balance Puyjalon, impute the gaps and move the four migrants |
| **Reads** | step 00: the raw table, the 9-ID list, the authors' script and functions (`input/`, relative links) |
| **Scripts** | `input/01_FilteringData.R` (authors', **class A**: only the `base_path` line is changed, by the staging harness; the change is in `output/script_changes.diff`); `scripts/01_post_counts.R` (class D, run right after the authors' script in the same R session: numbers that it only prints); `scripts/01_imputation_evidence.R` (D); `scripts/01_check_against_deposit.R` (D); `scripts/run_01_filter_main.sbatch`; `../_common/stage_run.sh` |
| **Software** | R 4.2.1 (`renv_r421`): dplyr, tidyr, ggplot2, ggpubr, pegas, reshape2, stringr; versions in `output/sessionInfo.txt` |
| **Parameters** | all the authors' (`authors_parameters.tsv` of step 00): individuals with ≥ 0.15 missing removed; loci with > 0.15 missing overall or in any River_Site removed; 50 Puyjalon fish with `set.seed(5)`; imputation with `set.seed(2)`; the four reassignments. Nothing chosen by us |
| **Provenance** | `[AUTHORS']` the script and its parameters; `[OURS]` the harness, the post-run counts, the evidence and the check |

**Outputs (`output/`)**


* the 8 tables `All_Rivers_*` (4 before and 4 after the reassignment of the four fish);
* `figures.pdf` (6 pages: the diagnostic plots that the script only draws on screen), `run_stdout.log` (its console output), `sessionInfo.txt`, `run_info_step01.txt`, `counts_step01.tsv`;
* `imputation_evidence.tsv`, `imputation_summary.txt`; `check_01.tsv`; `logs/<jobid>_…/` (copies of the scripts and the job logs).
* Issue note: `IMPUTATION_issue.md` (five parts).

**Check and level of reproducibility**


`output/check_01.tsv` (26 checks): R exit code 0; inputs unchanged (md5 before and after); 6 figure pages; 32 individuals and 8 loci removed (as the paper); 9 IDs in the list; 50 Puyjalon fish sampled; 885 fish in the main table, 987 in the all-Puyjalon table, 43 loci; **each of the 8 tables identical to the deposit**, in the text (after normalising the Windows line endings of the deposit) and when parsed. **Level of reproducibility T1.**

**Differences from the authors**


None in the analysis. The deposit's files have CRLF line endings, ours LF; the comparison normalises them. The authors' script prints counts to the console only; `01_post_counts.R` writes them to a file.

**Status**


Run 63751953 (13 s); see `STATUS.tsv`.

</details>


<details>
<summary>Step 02: Filter with the Rimouski outgroup (<code>02_filter_rimouski/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | The same filtering as step 01 with the 50 Rimouski (RIM_A) fish added as the outgroup of the trees |
| **Reads** | step 00: raw table, 9-ID list, authors' script and functions (`input/`) |
| **Scripts** | `input/02_FilteringData_Rimouski.R` (authors', **class A**: only the `base_path` line changes, `output/script_changes.diff`); `scripts/02_post_counts.R` (D); `scripts/02_check_against_deposit.R` (D); `scripts/run_02_filter_rimouski.sbatch`; `../_common/stage_run.sh` |
| **Software** | R 4.2.1 (`renv_r421`); versions in `output/sessionInfo.txt` |
| **Parameters** | all the authors' (`authors_parameters.tsv`): Rimouski fish with ≥ 0.08 missing removed; individuals ≥ 0.15 and loci > 0.15 missing removed; 50 Puyjalon and 50 Rimouski fish with `set.seed(5)`; imputation with `set.seed(2)`; the four reassignments. Nothing chosen by us |
| **Provenance** | `[AUTHORS']` script and parameters; `[OURS]` harness, counts, check |

**Outputs (`output/`)**


The four `All_Rivers_*_Rimouski.txt` tables (not imputed and imputed, before and after the reassignment of the four fish), `figures.pdf`, `run_stdout.log`, `sessionInfo.txt`, `counts_step02.tsv`, `run_info_step02.txt`, `check_02.tsv`, `logs/`.

**Check and level of reproducibility**


`output/check_02.tsv` (18 checks): exit code 0; inputs unchanged; figure pages written; 936 fish in the main table; 50 Rimouski and 50 Puyjalon fish; 41 loci counted from `NGS_SSsp2210`; **the four tables identical to the deposit** (text after normalising line endings, and parsed); the first locus `NGS_SSsp2210` keeps its missing cells in the imputed table (the script takes the allele frequencies from the second locus on), whereas the other loci have none left. **Level of reproducibility T1.**

**Notes**


* Counts (`counts_step02.tsv`): 30 individuals and 7 loci removed for missing data; 3 loci have no data in Rimouski and are removed first (`Ssa_10_1`, `Ssa_20_d56`, `Ssa_22_d31`).
* The first locus is not imputed because `allele_freq()` starts at `NGS_SsaD486`; what this means for the genind of step 03 and the trees of step 11 (the "40 versus 41 loci" point of the plan) is recorded in those steps.
* No issue note: the imputation issue of step 01 applies to the imputed tables of this step as well (`../01_filter_main/IMPUTATION_issue.md`).

</details>


<details>
<summary>Step 03: Formats (<code>03_formats/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Write the allele, genotype and genind files and the GENEPOP file used by `divMigrate` |
| **Reads** | the tables of steps 01 and 02 (`input/`), the authors' script 03 and functions (step 00) |
| **Scripts** | **03a** `input/03_Create_Format_Data.R` (authors', **class B**: only the `base_path` line and the removal of lines 67 to the end, the GENEPOP block, which is run in 03b; both changes are in `output/script_changes.diff`); `scripts/03_check_against_deposit.R` (D); `scripts/run_03a_formats.sbatch`. **03b** `output/03_b_genepop.R` (**class B**: our header with `library(graph4lg)` and the reading of the genind, followed by lines 71–89 of the authors' script copied verbatim; the job checks the copy with `diff`); `scripts/03_b_genepop_header.R`, `scripts/03_b_check.R`, `scripts/run_03b_genepop.sbatch` |
| **Software** | 03a: R 4.2.1 (`renv_r421`); 03b: R 4.2.3 with GDAL and `graph4lg` 1.8.0 (`r423_gdal`, `r423_lib`); versions in `output/sessionInfo.txt` and `output/sessionInfo_03b.txt` |
| **Parameters** | none to choose |
| **Provenance** | `[AUTHORS']` the conversion functions and the GENEPOP block; `[OURS]` the cut, the header of 03b, the checks |

**Outputs (`output/`)**


4 `ALLELE_*` tables, 3 `GENOTYPE_*` tables, 3 `GENIND_*.RDS` objects (885 fish × 43 loci, not reassigned and reassigned; 936 fish × 40 loci with Rimouski), the GENEPOP file (385 fish, 9 populations, Musquaro residents pooled), `check_03a.tsv`, `check_03b.tsv`, logs.

**Check and level of reproducibility**


* 03a (`check_03a.tsv`): all 7 tables identical to the deposit in the text (after normalising the deposit's Windows line endings); the 3 genind objects identical in `@tab`, strata, population, locus, individual and allele names; 10 files made. **T1**.
* 03b (`check_03b.tsv`): the GENEPOP file identical to the deposited one in all 396 lines; 9 populations, 385 individuals. **T1**.

**Differences from the authors**


* The authors' script 03 never loads `graph4lg`, whose `genind_to_genepop()` the function `genepop_creator()` calls; `03_b_genepop.R` adds `library(graph4lg)`. The genind made in R 4.2.1 reads in R 4.2.3 without change.
* Two deposited files are not made by script 03: `GENIND_Filtered_Individuals936_Loci41_IMPUTED_FINAL.RDS` and `GENOTYPE_Filtered_Individuals936_Loci41_IMPUTED_FINAL.txt` (Rimouski, 41 loci). No code of the deposit produces them; the script makes only the 40-locus Rimouski genind, because its genotype conversion starts at the second locus (`NGS_SsaD486`). Step 11 uses the 40-locus genind and, separately, the 41-locus allele table; the discrepancy with the 41 loci stated in the paper is recorded there. The deposited GENEPOP file of the data without reassignment is also not made by script 03; step 13 writes the files it needs.
* The two deposit-only files and the GENEPOP of the not-reassigned data are never inputs.

**Status**


03a run 63753755, 03b run 63753793; see `STATUS.tsv`. A first run of 03b (63753756) failed one of its own checks because of a wrong pattern in the check (logs in `output/failed_attempts/`; `DEVIATIONS.tsv`).

</details>


<details>
<summary>Step 04: Audit of the nine Puyjalon fish (<code>04_audit_puyjalon_ids/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Show, from the data, how the nine listed Puyjalon fish (`PUY_Ind_Cluster_With_ROM_To_Remove.txt`) look next to the Romaine fish and the other Puyjalon fish. The list remains a declared input in every other step; here it is used **only to mark fish**. **No pass/fail verdict.** The paper says the PUY-A smolts, caught downstream of the Puyjalon–Romaine confluence, were "assigned to their river using the R package assignPOP with the same markers (Fauchet et al. unpublished)"; that analysis and its baselines are not in the deposit, so no published number can be matched |
| **Reads** | raw table `AllDataCombined_NotFiltered.txt` (all fish of PUY_A, ROM_A and RIM_A), the 43 loci of the main table of step 01, the 9-ID list, the authors' function `p100_missing.R` (step 00) |
| **Scripts** | all **class D** (ours): `04_a_prepare_audit_data.R`, `04_b_run_task.sbatch` (array of 6 tasks), `04_c_pca_and_assignment.R`, `04_d_tables_and_figures.R`, `04_check.R`, `run_04_collect.sbatch` |
| **Software** | R 4.2.1 (adegenet 2.1.10, ade4, assignPOP 1.2.4, starmie, ggplot2, ggpubr); STRUCTURE 2.3.4 |
| **Parameters** | fish: all PUY_A, ROM_A and RIM_A fish, with the authors' thresholds (individuals with ≥ 0.15 missing, ≥ 0.08 for RIM_A as in script 02; computed over the 43 loci). STRUCTURE as in step 08 (admixture, correlated frequencies, burn-in 50,000, 100,000 steps, RANDOMIZE 0 with recorded seeds), two data sets (Puyjalon + Romaine, 278 fish; with Rimouski, 495 fish), K = 2 to 4, ten replicates. PCA: `dudi.pca`, 10 axes, missing values replaced by the allele mean. assignPOP: 10-fold cross-validation (`assign.kfold`), all loci, default model (support vector machine, linear kernel, cost 1), `set.seed(1)`; Romaine and Puyjalon fish are the two baselines |
| **Provenance** | `[OURS]` throughout (thresholds and function `p100_missing` are the authors') |

**Outputs (`output/`)**


`data/` (fish table with the filters and the listed flag, STRUCTURE inputs, GENEPOP file, genind), `pca_assign/` (PCA scores and percentage of total variation, assignPOP outputs), `results/audit_per_fish_table.tsv` (278 fish: membership in the Romaine-like cluster at K = 2, 3, 4 with and without Rimouski, assignPOP probability of Romaine, PCA scores, rank, listed flag), `results/audit_summary_by_cutoff.tsv`, `results/structure_romaine_cluster_summary.tsv`, `results/structure_lnP_by_K.tsv`, `results/romaine_reference_summary.tsv`, `results/figures/fig_04_puyjalon_romaine_audit.{png,pdf}`, `check_04.tsv`, `logs/`. STRUCTURE runs: `<scratch>/763_salmon_work/04_audit/runs/` (60 runs, 2.2 core-hours; tasks of 1.5 to 3 min).

**Check and level of reproducibility: **T3** (`output/check_04.tsv`: 9 integrity checks pass, 8 notes; there is no published number to compare)**


Integrity: 166, 113 and 471 fish in the raw table; the 495 fish kept by the filters recomputed independently; the nine listed fish present and kept; 60 STRUCTURE runs, distinct seeds, RANDOMIZE 0, expected fish and K; assignPOP assigns each fish once with probabilities summing to 1; PCA scores for 278 and 495 fish.

**What the figure shows (description, not a verdict)**


* **Fish kept by the filters:** PUY_A 165 of 166, ROM_A 113 of 113, RIM_A 217 of 471; all nine listed fish are among the 165.
* **STRUCTURE, K = 2 (Puyjalon + Romaine, all ten replicates agree; ln P(D) −24,833 to −24,838):** the Romaine fish have a mean membership of 0.963 in the Romaine-like cluster (one fish at 0.49); the Puyjalon fish a mean of 0.060. The nine listed fish have **0.867, 0.701, 0.556, 0.521, 0.459, 0.442, 0.424, 0.418 and 0.198**: intermediate, not Romaine-like throughout. Ranked among the 165 Puyjalon fish they are 1 to 8 and 16; **seven other Puyjalon fish** (0.20 to 0.38; `AN_13120_PUY_2013`, `AN_13025_PUY_2013`, `AN_13027_PUY_2013`, `AN_14097_PUY_2014`, `AN_13051_PUY_2013`, `AN_13105_PUY_2013`, `AN_13064_PUY_2013`) have a higher membership than the ninth listed fish, and the other 156 fish are all ≤ 0.384. At a cut-off of 0.5: 4 of 9 listed fish and none of the other 156 reach it; at 0.8: 1 of 9 and none of the others.
* **With Rimouski (K = 3, six replicates in the major mode; the Romaine cluster holds 0.97 of the Romaine fish):** 3 of the nine at ≥ 0.5, none at ≥ 0.8; the other 156 fish none at ≥ 0.5. (At K = 2 with Rimouski the two clusters are Rimouski and Puyjalon + Romaine; the "Romaine-like" membership of that run is not informative and is only in the per-fish table.)
* **K = 3 and K = 4 without Rimouski:** K = 3 has seven replicates in the major mode (membership of the listed fish 0.14 to 0.87, 5 of 9 at ≥ 0.5; other Puyjalon fish ≤ 0.375); K = 4 is not stable (the highest ln P(D) belongs to a single replicate; the other nine are 208 to 290 ln units lower) and is reported without interpretation.
* **assignPOP (ten-fold cross-validation, Romaine vs Puyjalon):** 98.2 % of the 278 fish are assigned to their site (three Romaine fish are assigned to Puyjalon, with probabilities of Romaine of 0.37 to 0.40). The listed fish have held-out probabilities of Romaine **0.886, 0.848, 0.479, 0.442, 0.286, 0.243, 0.164, 0.161 and 0.086**: two exceed 0.8, none of the other 156 Puyjalon fish exceeds 0.33.
* **PCA** (PC1 6.25 %, PC2 2.74 %): Puyjalon and Romaine fish separate on PC1 (means −2.99 and 4.37); the listed fish lie between them (PC1 −0.34 to 3.71).
* The two measures agree on the order of the two most Romaine-like fish (`AN_13063_PUY_2013`, `AN_14041_PUY_2014`) and disagree for others (e.g. `AN_14042_PUY_2014`: STRUCTURE 0.556, assignPOP 0.161).

The listed fish are therefore a group that is distinct from the other Puyjalon fish in all three analyses (all lie between the two groups or toward the Romaine fish), but the individual evidence is graded and depends on the method and the cut-off; whether the authors' removal of exactly these nine is the right set cannot be decided from these analyses, and no other fish were removed in any other step.

**Limits and choices**


* The authors' assignPOP analysis (baselines, cross-validation) is unpublished; ours is a description with Romaine as one baseline and all Puyjalon fish (including the listed ones) as the other, so the listed fish contaminate their own baseline; cross-validation holds out the fish being assigned.
* The missing-data filter is computed over the 43 loci that the main data set keeps; the authors compute it over all 51 loci first (and apply 0.08 to Rimouski before removing loci). The difference only affects which fish pass: 165 of 166 PUY_A and all ROM_A fish pass, and the 217 RIM_A fish are our count.
* The membership of a fish is averaged over the replicates whose ln P(D) is within 0.1 % of the best; this rule was adopted after seeing that one K = 3 replicate with Rimouski had a ln P(D) of −275,392 (failure of mixing) against −46,294 for the others; it changes only the K = 3 and K = 4 columns.

</details>


<details>
<summary>Step 05: Principal component analyses (<code>05_pca/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce the principal component analyses of the paper: Fig. 2 (385 fish of known tactic, reassigned; all sites and per watershed), Fig. S2 (before reassignment), Fig. S3 (with the Corneille juveniles), Fig. S4 (effect of sampling year) |
| **Reads** | `GENIND_…885_Loci43_REASSIGNED_IND_IMPUTED_FINAL.RDS` and `GENIND_…885_Loci43_IMPUTED_FINAL.RDS` of step 03 (identical to the deposit); the authors' scripts `04_PCA_MS.R`, `05_PCA_MatSupp.R` and functions `kp_ind.R`, `pca.R` (step 00); `input/inertia_in_figures_transcribed.tsv` (our transcription of the percentage of total variation printed on the axes of Figs. 2, S2, S3 and S4, for the check only) |
| **Scripts** | `input/04_PCA_MS.R` and `input/05_PCA_MatSupp.R` (authors', **class B**: only the `base_path` line changes, `output/script0{4,5}/script_changes.diff`; the PCAs that the scripts keep in memory are written by `scripts/05_post_04.R` and `05_post_05.R`, which change no calculation); `scripts/05_c_year_effect.R` (**class D**, Fig. S4: the deposit has no code for it); `scripts/05_check.R` (D); `scripts/run_05_pca.sbatch` |
| **Software** | R 4.2.1: adegenet 2.1.10 (`dudi.pca` of ade4), ggplot2, ggpubr, ggnewscale (`output/script04/sessionInfo.txt`) |
| **Parameters** | all the authors': `dudi.pca(as.data.frame(tab(genind)), scannf = FALSE, nf = 10)` (centred, scaled allele-count table), 95 % normal ellipses by site |
| **Provenance** | `[AUTHORS']` scripts, functions and parameters; `[OURS]` harness, post-run writers, year-effect script (same `pca()` and `kp_ind()`), check, transcription |

**Outputs (`output/`)**


* `script04/`, `script05/`: `figures.pdf` of the authors' scripts (Fig. 2; Figs. S2 and S3), `pca_inertia_script0{4,5}.tsv` (10 axes of every PCA, as rounded by the script and to three decimals), `pca_scores_script0{4,5}_<analysis>.tsv` (scores of all fish on 10 axes), `migrants_found_by_the_script_conditions.tsv`, `run_stdout.log`, `script_changes.diff`, `sessionInfo.txt`.
* `year_effect/`: `year_effect_inertia.tsv`, `year_effect_scores.tsv`, `year_effect_tests_kruskal.tsv`, `fig_S4_year_effect_ours.{png,pdf}`.
* `figures/`: PNG renderings of the authors' figures (`fig_05_authors_script04_fig2-1.png` = Fig. 2; `fig_05_authors_script05-1.png` = Fig. S2; `-2` = Fig. S3; `-3` = Corneille only, with juveniles) and of our Fig. S4 (`fig_05_year_effect_ours.png`).
* `check_05.tsv`, `logs/`. The run takes 16 to 32 s.

**Check and level of reproducibility: **T1** (`output/check_05.tsv`: 6 integrity checks pass, 17 comparisons agree, 2 notes)**


* The percentage of total variation of PC1 and PC2 of the 16 analyses we could read in the figures agrees with the axis labels: Fig. 2A–E (6.44/4.07; 22.17/3.91; 10.60/4.93; 13.96/3.67; 11.90/4.63), Fig. S2A–E (the same five values), Fig. S3A and B (5.72/2.61 and 3.80/2.45) and Fig. S4A–D to **three** decimals (14.874/5.969; 22.167/3.906; 10.600/4.933; 3.263/2.479). The PCA is deterministic, so exact agreement is expected once the input genind is the deposited one.
* The four fish that the paper reassigns are the four that the script marks with red circles: `AN_1532_UN_2018` (COR), `LA_0146_VIC_2004` (AGU), `LA_0021_MUR_2004` and `LA_0029_MUR_2004` (MUS). The script lists them by the conditions `Axis1 > 5` (COR), `Axis1 < 0` (AGU–VIC) and `Axis1 < -5` (MUS) on the scores, and then names them in a comment; the conditions select **the whole cluster in which the migrants lie** (12, 47 and 34 fish), not four fish. In each cluster the fish whose identifier carries the other tactic prefix (AN_ or LA_) are exactly the four (1 of 12, 1 of 47, 2 of 34). The conditions depend on the sign of the axes, which is arbitrary in a PCA; in our run the signs coincide with those the script assumes (the figures have the same orientation as the paper's).
* The PCAs of the not-reassigned and of the reassigned data have **identical scores and percentage of total variation** (maximum difference 0): reassignment only changes the site label of four fish, and none of the five subsets changes its members. Figs. 2 and S2 therefore differ only in the colour/ellipse assignment of the four fish.

**What the supplementary year-effect figure shows (Fig. S4, class D, not part of the check)**


The paper states that sampling year had no effect on the genetic structure of the sites sampled in several years. The statement rests on the figure alone; the deposit has no code. We reproduce the four panels (orientation as in the paper except that PC2 of panel D has the opposite sign, which is arbitrary) and add a Kruskal–Wallis test of the PC1 and PC2 scores of each site among years (`year_effect_tests_kruskal.tsv`; no correction for multiple tests):

| Site | Years | n | smallest P (PC1, PC2) |
|---|---|---|---|
| COR-A (panel C) | 2015, 2018 | 41 | 0.19 |
| COR-R (panel C) | 2015, 2018 | 15 | 0.11 |
| PUY-A (panel B) | 2013, 2014, 2015 | 50 | 0.034 (PC1) |
| COR-JUV-DOWN (panel D) | 2014, 2015, 2019, 2020 | 229 | 1.7e-09 (PC2); PC1 6.0e-06 |
| COR-JUV-UP (panel D) | 2012, 2014, 2015, 2019, 2020 | 271 | 1.3e-10 (PC2); PC1 8.2e-04 |

No year difference is seen in the adults. In the juveniles the scores differ strongly among years. That is not the same as an effect of the year on the "genetic structure" in the sense of the paper: the juveniles were sampled in different years and, possibly, as cohorts and families (relatedness) that differ among years, which the PCA cannot separate from a year effect. The paper's sentence is therefore supported for the adults and not for the juveniles by this test. We did not pursue this (no sensitivity analysis was planned for it).

**Notes**


* The PCA uses the imputed genind (imputation of missing genotypes by one random draw per locus column; this finding, `../01_filter_main/IMPUTATION_issue.md`). The effect of this imputation on the PCA was not tested; the objects are identical to the deposit.
* The authors' script 05 also writes a figure of Corneille with the juveniles on page 3 of its `figures.pdf` that has no counterpart in the paper.

</details>


<details>
<summary>Step 06: Audit of the four reassigned fish (<code>06_audit_reassignment/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Show how the four fish that the paper reassigns look in an assignment test, compared with the 381 other fish of known tactic. The paper identifies them by eye on the PCA (Fig. S2); no code or statistic exists in the deposit. **No pass/fail verdict.** |
| **Reads** | `GENIND_…885_Loci43_IMPUTED_FINAL.RDS` of step 03 (the authors' genind **before** reassignment; restricted to the 385 fish with Ecotype Anadromous or Landlock); `output/script05/pca_scores_script05_not_reass_eco_sure.tsv` of step 05 (PC1 and PC2 for the figure) |
| **Scripts** | all **class D** (ours): `06_a_loo_assignment.R`, `06_b_figures.R`, `06_check.R`, `run_06_audit_reassignment.sbatch` (17 s); `06_c_figure_bold.py` (post-processing run by hand in the `python` environment, version 3 of the report: the same figure with larger symbols, `output/figures/fig_06_loo_assignment_bold.png`; it reads the tables of the step and changes none) |
| **Software** | R 4.2.1, adegenet 2.1.10 (allele table), ggplot2, ggpubr |
| **Parameters (ours, fixed before the run)** | frequency-based likelihood with 0.5 added to every allele count (frequency of allele *a* at locus *l*: (n_a + 0.5) / (2n + 0.5 k_l), k_l = number of alleles of the locus in the 885 fish), Hardy–Weinberg, 86 allele columns of 43 loci, equal prior; the focal fish is removed from its own reference group; two units: the 10 sites (and the 9 groups obtained by pooling MUS-LAK-R and MUS-RIV-R, as the paper does in most analyses) and, for the four watersheds with both tactics, the two tactics within the watershed; cut-offs 0.5 and 0.8 on the posterior probability |
| **Provenance** | `[OURS]`; the labels, the genind and the four identifiers are the authors' |

**Outputs (`output/`)**


`loo_assignment_per_fish.tsv` (385 fish: label, assigned site, posteriors for the 10 sites, posterior of the label, tactic-within-watershed posterior and log10 odds, PC1 and PC2, flag for the four), `loo_loglik_10sites.tsv`, `loo_summary_by_site.tsv` (counts by site and cut-off), `loo_fish_assigned_elsewhere_or_reassigned.tsv` (the seven fish assigned elsewhere at 0.5), `figures/fig_06_loo_assignment.{png,pdf}`, `check_06.tsv`, `sessionInfo.txt`, `logs/`.

**Check and level of reproducibility: **T3** (`output/check_06.tsv`: 6 integrity checks pass, 2 comparisons agree, 1 differs, 3 notes)**


Integrity: 385 fish; the sample sizes obtained after moving the four fish equal those of the other steps; posteriors sum to 1; ten fish (the four and six random ones) recomputed with a second implementation that removes the focal fish explicitly (maximum difference 5e-7, rounding of the output).

**What the audit shows (description, not a verdict)**


* **The four fish are the four most extreme fish of the 385.** Each is assigned, with posterior ≥ 0.9999999, to the site the paper gives it (AN_1532_UN_2018 COR-A → COR-R; LA_0146_VIC_2004 AGU-R → AGU-A; LA_0021_MUR_2004 and LA_0029_MUR_2004 MUS-RIV-R → MUS-A) and to the other tactic of its watershed (posterior of its own label between 5e-23 and 5e-46 in tactic units: 4.7e-23, 5.3e-46, 3.9e-33, 1.8e-29). They have the four lowest posteriors for their own label of all 385 fish (ranks 0 to 3).
* **A fifth fish is as clearly assigned elsewhere and was not reassigned**: `LA_0012_UN_2015` (COR-R) is assigned to COR-A (posterior ≈ 1.0; posterior of its own label 2.2e-8, the fifth lowest of the 385). In the PCA of step 05 it lies at the edge of the COR-A cluster. The paper's rule ("reassigned when biologically and geographically consistent", Results) does not say why a COR-R fish assigned to COR-A, the reverse of the COR-A fish AN_1532 that was reassigned, was kept. We do not decide; the table lists it.
* **Two ETA-A fish** (`AN_0033_ETA_2004`, `AN_0018_ETA_2004`) are assigned to MUS-A (posterior 0.976 and 0.964). ETA-A has no resident counterpart and is an anadromous site close to MUS-A in the PCA; the paper does not discuss it.
* All other fish have a posterior ≥ 0.58 for their own label (9 groups). 376 of 385 fish have ≥ 0.8 for their own group (9 groups; 378 at ≥ 0.5). In the tactic-within-watershed analysis 357 of 362 fish have posterior ≥ 0.8 for their own tactic; the five that do not are the four and `LA_0012_UN_2015`.
* **MUS-LAK-R and MUS-RIV-R cannot be distinguished** (40 of 82 fish are assigned to the other resident site in the 10-site analysis), which agrees with the paper's pooling of the two sites; at the level of the 9 groups no resident fish is assigned elsewhere except the two reassigned ones.

Limits: the assignment uses all 43 loci, the same reference samples as the labels (leave-one-out removes only the focal fish), and a likelihood that treats the loci as independent, so the posteriors of fish that are clearly assigned are extreme; the values are for ranking and for the 0.5/0.8 cut-offs, not for a probability statement. No other loci subset, pseudocount or method was tried (no sensitivity analysis was planned).

</details>


<details>
<summary>Step 07: Linkage disequilibrium and missingness (<code>07_ld_missingness_checks/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce the two filtering checks that the paper reports without showing: (a) pairwise linkage disequilibrium among loci ("between −0.071 and 0.006, no locus removed"), (b) identity by missingness with PLINK ("no cluster created by missing data, data not shown") |
| **Reads** | the raw table `AllDataCombined_NotFiltered.txt` and `PUY_Ind_Cluster_With_ROM_To_Remove.txt` (step 00), the authors' script `01_FilteringData.R` and functions (step 00) for (a); `All_Rivers_Dataset_Filtered_Balanced.txt` of step 01 (885 fish, 43 loci, **not imputed**) for (b) |
| **Scripts** | (a) `input/01_FilteringData.R` (authors', **class B**: the base_path line and **one switch**, `run_ld <- FALSE` → `run_ld <- TRUE; bd_at_ld <- bd`, applied by the staging harness; `output/script01_ld/script_changes.diff`); `scripts/07_post_ld.R` (writes what the block keeps in memory); `scripts/07_a2_ld_valid_order.R` (**class D**: the same pairs in both orders); `scripts/07_c_ld_toy_example.R`, `07_c_check.R`, `run_07c_ld_toy.sbatch` (**class D**, version 3 of the report: a made-up data set with two loci that shows what Δ is, that its table sums to zero, and what `LD2` returns in column order and in reverse order; 11 s); (b) `scripts/07_b_missingness_plink.R` and `scripts/07_b_run_plink.sh` (**class D**); `scripts/07_check.R`; `scripts/run_07_ld_missingness.sbatch` (35 s); the figure was drawn in the `python` environment (code in the session; panels A and B of `output/figures/fig_07_ld_and_missingness.png` from `ld_pairs_column_order_vs_reversed.tsv`, C and D from `plink/mds_with_labels.tsv`) |
| **Software** | R 4.2.1, pegas 1.2 (`LD2`); PLINK v1.9.0-b.8 (22 Oct 2024) as installed in `tools/bin/plink` (the plan named 1.90b7.7) |
| **Parameters** | (a) the authors' (`ld_test_locus(data = bd, id_col = "ID", pop_col = "River_Site", loci_col = "NGS_SSsp2210")`, same-chromosome pairs, mean per pair); (b) ours: every called genotype becomes the dummy genotype "1 2", every missing genotype "0 0"; `--cluster missing` (PLINK defaults: complete-linkage on the identity-by-missingness distance, no constraint) and `--mds-plot 4 eigvals` |
| **Provenance** | `[AUTHORS']` script and function for (a); `[OURS]` switch, post-run writer, column-order script, whole of (b), check |

**Read this first**


The linkage statistic of the paper **reproduces exactly but is not a measure of linkage disequilibrium**: the authors' function calls `pegas::LD2` for every ordered pair of loci, and the call with the loci in reverse column order returns an invalid matrix (the allele correlations do not sum to zero); the range quoted in the paper comes from those incalls in column order, and the mean of the correct calls is zero by construction. See `LD_statistic_issue.md`.

**The data of the LD block (a deviation from the plan)**


The plan assumed that the LD test ran on the 885-fish table. The script runs it earlier, on the data frame after the removal of 32 fish and 8 loci for missing data and **before** the removal of the Puyjalon fish that cluster with Romaine and the sampling of 50 Puyjalon fish: **996 fish, 43 loci** (`script01_ld/bd_at_ld_block.tsv`). We reproduce that. Missingness (b) uses the 885-fish table, as in the plan; the paper does not say which table it used.

**Outputs (`output/`)**


* `script01_ld/`: the outputs of script 01 with the switch on (the eight tables are identical to step 01), `ld_summary.tsv`, `ld_mean_per_locus_pair.tsv` (the authors' statistic, 72 ordered pairs), `ld_allele_pairs_same_chromosome.tsv` (5,474 allele pairs), `ld_loci_with_same_chromosome_partner.tsv` (32 loci), `bd_at_ld_block.tsv`.
* `ld_pairs_column_order_vs_reversed.tsv` (36 pairs, both orders).
* `toy_example/`: `toy_genotypes.tsv` (40 made-up fish), `toy_ld_results.tsv`, `toy_ld_example.txt` (printed in the report), `check_07c.tsv` (8 integrity checks pass, 16 notes).
* `plink/`: `ibm.ped`, `ibm.map` (dummy genotypes), `fish_table.tsv`, PLINK results (`ibm.imiss`, `ibm.lmiss`, `ibm.cluster1`, `ibm.cluster2`, `ibm.mds`, `ibm.mds.eigvals`, `ibm.log`), `mds_with_labels.tsv`, `mds_axes_association.tsv`, `missingness_by_site.tsv`, `missingness_by_year.tsv`, `plink_clusters_by_site.tsv`.
* `figures/fig_07_ld_and_missingness.{png,pdf}`, `check_07.tsv`, `logs/`.

**Check and level of reproducibility: **T1** (LD, as coded) and **T2** (missingness) (`output/check_07.tsv`: 11 integrity checks pass, 3 comparisons agree, 1 differs, 6 notes)**


* **LD, as coded: T1.** Range of the mean pairwise LD of the 36 same-chromosome pairs: **−0.0710 to 0.0057** (paper: −0.071 and 0.006). The comment in the script says −0.0730 (the minimum differs; stale comment).
* **LD, calls in column order (ours; T3, descriptive).** In column order the sum of Δ is 0 (|sum| ≤ 2e-16) for all 36 pairs and the means are −0.0009 to 0.0001; reversed, 35 of 36 sums differ from zero (up to −1.98). With the authors' T2 test in column order, 35 of 36 pairs have P < 0.05 (Holm 35), in the pooled sample of 996 fish (twelve sites); this reflects allele-frequency differences among sites as much as linkage.
* **Missingness: T2.** 139 of 38,055 genotypes are missing (0.365 %); 793 fish have no missing genotype, 92 have between one and four. PLINK's clustering with its default options puts all 885 fish in **one cluster** ("no cluster created by missing data", in agreement, but this is also what PLINK does by default whatever the data). The MDS of the identity by missingness (figure panels C and D) is not structured by site (adjusted R² of site on MDS 1 to 4: −0.001, 0.014, −0.002, −0.006) or by year (0.019, 0.071, 0.012, −0.002); MDS 2 is the number of missing loci per fish (R² 0.83). The criterion for the comparison on MDS (adjusted R² of site < 0.25 on the four axes) is ours and was fixed before the run.

**Notes**


* Missingness is not constant among sites: the juveniles (COR-JUV-DOWN 0.28 and COR-JUV-UP 0.16 missing loci per fish) and COR-R (0.29) have the highest mean, MUS-A and MUS-RIV-R none (`plink/missingness_by_site.tsv`); by year, 2019 (0.53 per fish; 37 of 111 fish) is the highest. None of this forms clusters in the MDS.
* The PLINK clustering result (one cluster) is the default; no `--K` or `--ppc` options were tried (not planned).
* Cause of the invalid reversed calls, traced in version 3 of the report (see `LD_statistic_issue.md`): `LD2` reads the genotypes in the requested order but the allele names and frequencies in the original order; and it stores its table by columns although it fills it by rows, so its cells are misplaced when the loci have different numbers of alleles (the total, mean, T2 and P are not affected).
* Time: 35 s (job of the step); 11 s (the made-up example, job 64158937).

</details>


<details>
<summary>Step 08: STRUCTURE (<code>08_structure/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Re-run the Bayesian clustering of the paper: all 385 fish of known tactic (K = 1–12), the anadromous cluster (K = 1–8) and the resident cluster (K = 1–8) of that first analysis, and each watershed with both tactics (K = 1–5); Evanno's ΔK; mean membership |
| **Reads** | the 385-fish tables of step 03 (not imputed; STRUCTURE takes missing data as −9); the authors' `mean_membership.R` and `delta_K.R` (the second is only run to show what it returns) |
| **Scripts** | all **class D** (the deposit contains no STRUCTURE input, parameter file or script): `08_a_make_inputs.R` (7 data sets, 48 tasks), `08_b_run_task.sbatch` (array; 10 replicates in parallel per task), `08_c_collect.R/.sbatch`, `08_d_modes.R/.sbatch`, `08_check.R/.sbatch`; `08_f_figure_deltaK_large.py` (post-processing run by hand in the `python` environment, version 3 of the report: `output/results/fig_08_deltaK_large.png` and `fig_08_lnP_large.png`, the seven panels of the paper's Fig. S5 enlarged, with a text panel that says how to read them; it reads the tables of the step and changes none); `[AUTHORS']` function `mean_membership.R` is sourced unchanged |
| **Software** | STRUCTURE 2.3.4 (our build, `RANDOMIZE` switched off, see below); R 4.2.1 with starmie 0.1.2 (`loadStructure`, `bestK(method = "evanno")`, `clumpp`) |
| **Parameters** | from the paper: admixture model, burn-in 50,000, 100,000 MCMC steps, 10 replicates per K. Not stated in the paper, so STRUCTURE's defaults (ours, logged): correlated allele frequencies, α inferred, no sampling-location prior (`LOCPRIOR = 0`), no population information (`USEPOPINFO = 0`), `ADMBURNIN` 2,500. Seeds `100000·round + 10000·dataset + 100·K + replicate`, all 480 distinct and written in the run files |
| **Provenance** | `[AUTHORS']` the K ranges, the replicate number, the burn-in and run length (from the paper) and `mean_membership.R`; `[OURS]` all else |

**What we found while running it**


The default `RANDOMIZE = 1` makes STRUCTURE ignore the seed given with `-D` and seed from the clock, so replicates started in the same second were identical (round 1 of the first attempt). We compiled with `#define RANDOMIZE 0`, re-ran everything, and kept the discarded runs on scratch (`discarded_same_clock_seed/`). The check now reads the parameter block of all 480 run files and confirms `RANDOMIZE=0` and 480 distinct seeds.

**Outputs (`output/`)**


`results/`: `summary_best_K.tsv`, `evanno_tables.tsv` (mean ln P(D), L′, |L″|, ΔK per K), `deltaK_diagnostics.tsv` (mean, sd, min, max of ln P(D) by K; ΔK by starmie and by the authors' `delta_K()`), `lnP_all_runs.tsv`, `mean_membership_by_site.tsv`, `mean_membership_overall.tsv`, `modes_by_run.tsv` and `modes_summary.tsv` (partition found by each run), `membership_*.rds`, `bars_<data set>_K<k>.{png,pdf}` (K = 2, our best K and the paper's K), `deltaK_all.{png,pdf}` (Fig. S5-like), `check_08.tsv`, `archives/` (the 480 run files in three tarballs; the files stay on scratch), `logs/`, `collect_round1/` (first collection of the all-fish runs), `failed_attempts/`.

**Result against the paper**


| quantity | paper | ours |
|---|---|---|
| best K, all fish (Fig. S5A) | 2 | **2** (ΔK 672.5; the paper's curve reads about 670) |
| best K, resident cluster (S5C) | 3 | **3** (ΔK 2017.6) |
| best K, each watershed (S5D–G) | 2 | **2** in all four (PUY 6704, COR 782, AGU 2815, MUS 1247) |
| best K, anadromous cluster (S5B) | **4** | **2** (ΔK 1049; ΔK(4) = 1.7) |
| mean membership, all fish, K = 2 | 96.70 % | 96.92 % |
| first cluster (anadromous sites and COR-R) | 95.59 % | 95.65 % |
| second cluster (residents without COR-R) | 98.38 % | 98.47 % |
| COR-R in the Corneille watershed analysis (Fig. S9) | 89.80 % | 89.88 % |
| "above 97 % in all other sites" (watershed analyses) | | all above 97 % except COR-A 95.47 % |

`output/check_08.tsv`: 12 integrity checks pass; of 12 comparisons with the paper 10 agree and 2 differ (best K of the anadromous cluster; COR-A membership).

**The one difference that matters: the anadromous cluster**


Our ΔK curve for the anadromous cluster peaks at K = 2 because the ten K = 2 runs agree to within 0.73 ln P(D) units (sd), so ΔK(2) = |L″(2)| / sd = 761/0.73 ≈ 1049. The paper's Fig. S5B shows ΔK(2) close to zero and a maximum of about 8.7 at K = 4, which needs a spread of ln P(D) at K = 2 of hundreds of units (we compute: more than about 280) between replicate runs; none of our runs shows it. The other six panels peak at the same K as ours (the heights differ, as ΔK depends on the replicate spread).

The partitions differ too (`modes_summary.tsv`). At K = 4, nine of our ten runs give AGU-A | COR-A + COR-R | ETA-A + MUS-A | PUY-A (mean ln P(D) −19,707), one gives another split; the paper's Fig. 3B shows PUY-A | COR-A | COR-R | AGU-A + MUS-A + ETA-A. In our runs COR-R separates from COR-A only at K = 5 (three of ten runs; ln P(D) −19,271 against −19,537 for the other K = 5 mode), and AGU-A separates from ETA-A + MUS-A at K = 4 (nine runs). The bar plot at the paper's K, `results/bars_grpA_K4.png`, therefore shows our modal partition and not the paper's. The all-fish K = 2 (Fig. 3A) and the resident cluster at K = 3 (Fig. 3C) have the same partitions as the paper. We did not test whether a different setting (for instance a sampling-location prior, which tends to separate small groups such as COR-R, 15 fish) produces the paper's K = 4; the plan has no sensitivity runs, so this is left as an option (about 2.5 core-hours for a 10-replicate K = 1–8 run on the 211 fish).

The authors' own `delta_K()` is not Evanno's ΔK (mean|ln P| / sd of |ln P|) and is not called by any deposited script; on our runs it has its maximum at K = 1 for five of the seven data sets, among them the anadromous cluster, and at K = 2 for PUY and AGU (`deltaK_diagnostics.tsv`), so it does not give the paper's K = 4 either.

**Level of reproducibility**


**T2** (approximately reproduced): six of seven best-K values and every quoted membership value agree; the anadromous cluster's K and partition at K = 4 do not.

**Notes**


* Compute: 480 runs, 13.5 allocated core-hours (sacct; the 80 runs of the anadromous cluster 2.5); reading, ΔK and CLUMPP for all data sets take 26 s.
* First check (job 63756653) had five FAIL rows that were errors of my check, not of the runs (see `../DEVIATIONS.tsv`); the check now separates integrity rows from comparisons with the paper.

</details>


<details>
<summary>Step 09: FST as coded by the authors (<code>09_fst_as_coded/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce Fig. S6 and the Wilcoxon tests exactly as the authors computed them |
| **Reads** | `ALLELE_…885_Loci43_REASSIGNED_IND_FINAL.txt` (not imputed) of step 03; the authors' script `08_FST_MS.R` and functions (step 00); `input/figS6B_transcribed_values.tsv` (our transcription of Fig. S6B, for the check only) |
| **Scripts** | `input/08_FST_MS.R` (authors', **class B**: only the `base_path` line changes, `output/script_changes.diff`; the results the script only prints or keeps in memory are written by `scripts/09_post_results.R`, which repeats the authors' three `wilcox.test` calls to store them); `scripts/09_check.R` (D); `scripts/run_09_fst_as_coded.sbatch` |
| **Software** | R 4.2.1: hierfstat 0.5-11, pegas 1.2, ggplot2, ggpubr, ggtext, cowplot, tidyverse (`output/sessionInfo.txt`) |
| **Parameters** | all the authors' (`set.seed(33)`, 10,000 bootstraps, `method = "nei"`) |
| **Provenance** | `[AUTHORS']` script, function and parameters; `[OURS]` harness, post-run writer, check, transcription of Fig. S6B |

**Read this first**


The numbers of this step are **not FST in the usual sense**. `fst_calculation()` passes the allele table (two columns per locus) to `pairwise.neifst()`, which expects one genotype column per locus; and `pairwise.neifst()` returns Fst′. Step 10 (`../10_fst_corrected/FST_encoding_issue.md`) explains the problem and gives the our computation. The values here are reproduced exactly because the deposit's figure was made this way.

**Outputs (`output/`)**


`fst_as_coded_all_cells.tsv` (matrix in long form with the 95 % limits), `fst_as_coded_pairs.tsv` (the 45 pairs with their tactic class), `fst_as_coded_class_means.tsv`, `fst_as_coded_wilcoxon.tsv`, `fst_per_locus_pegas.tsv` (per-locus FST, pegas, Fig. S6A), `figures.pdf` (histogram of Fig. S6A and the heat map of Fig. S6B), `run_stdout.log`, `sessionInfo.txt`, `check_09.tsv`, `logs/`.

**Check and level of reproducibility**


`output/check_09.tsv` (13 checks): the 45 pairwise values agree with Fig. S6B to within 0.0005 (largest difference; mean 0.0003); class means A–A 0.040, A–R 0.112, R–R 0.117; Wilcoxon W = 10 (A–A vs R–R), 0 (A–A vs A–R) and **94 (R–R vs A–R, as printed in the paper)**, P = 0.27 for R–R vs A–R (the "no difference" of the paper); mean 95 % limits by class A–A 0.029–0.053, A–R 0.087–0.139, R–R 0.092–0.147 (paper 0.03–0.05, 0.09–0.14, 0.09–0.15). **Level of reproducibility T1.**

**Notes**


* The run takes 30 s (10,000 bootstraps); the plan's estimate of 20 to 30 minutes came from a different measurement.
* The 95 % limits come from `boot.ppfst()` and are Weir–Cockerham limits attached to a Fst′ point estimate (shown in step 10).

</details>


<details>
<summary>Step 10: FST after our correction (<code>10_fst_corrected/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Compute pairwise differentiation from genotypes that hierfstat can read, with the authors' pipeline, and compare with step 09 |
| **Reads** | `ALLELE_…885_Loci43_REASSIGNED_IND_FINAL.txt` (step 03; not imputed; the 385 fish of known tactic are used, the 885 for the Corneille juveniles); `All_Rivers_Dataset_Filtered_Balanced.txt` (step 01; reassignment not applied); `fst_as_coded_pairs.tsv` (step 09, for comparison only) |
| **Scripts** | all **class D** (ours): `10_common.R` (helpers), `10_a_fst_corrected.R`, `10_b_class_tests.R`, `10_c_juvenile_balanced_reassignment.R`, `10_e_demonstrate_encoding.R`, `10_g_fake_data_test.R`, `10_g_check.R`, `run_10g_fake_data_test.sbatch` (the test with made-up data of version 3 of the report: two data sets whose true FST is known by construction, computed with the documented layout, the authors' layout and the authors' function; jobs 64155662 first run and 64155763 final; 20 s), `10_check.R`, `run_10_fst_corrected.sbatch`; `10_d_figures.py` and `10_f_paper_pairs.py` (post-processing, run by hand in the `python` environment) |
| **Software** | R 4.2.1: hierfstat 0.5-11, adegenet 2.1.10 (`output/sessionInfo.txt`); Python 3.11 for the figures |
| **Parameters** | bootstraps 10,000 (loci for Fst′ and plain Fst; `boot.ppfst` for θ), `set.seed(33)` as the authors; 100 balanced subsamples, `set.seed(2026)`; 252 permutations (exact) |
| **Provenance** | `[AUTHORS']` the estimator call and the seed; `[OURS]` everything else |

The finding is written up in `FST_encoding_issue.md`.

**Outputs (`output/`)**


`fake_data_test/` (`fake_data_test_results.tsv`, `fake_data_examples.txt`, `check_10g.tsv`: 8 integrity checks pass, 4 notes, `sessionInfo_10g.txt`), `fst_corrected_pairs.tsv` (45 pairs: Fst′, θ, plain Fst with limits, and the as-coded value), `fst_corrected_class_means.tsv`, `class_tests_wilcoxon.tsv`, `class_tests_permutation.tsv`, `class_tests_jackknife_sites.tsv`, `juvenile_fst.tsv`, `balanced_subsamples_{pairs,class_means}.tsv`, `reassignment_comparison_{pairs,class_means}.tsv`, `paper_cited_pairs.tsv`, `encoding_demo.txt`, `encoding_alleles_per_locus.tsv`, `encoding_equivalence.txt`, `perlocus_components_by_pair.rds`, `figures/fig_10_as_coded_vs_corrected.{png,pdf}`, `figures/fig_10_matrices_as_coded_vs_corrected.{png,pdf}`, `check_10.tsv`, `logs/`.

**Check and level of reproducibility**


`output/check_10.tsv`: 29 of 29 checks pass (45 pairs in classes 10/25/10; the two codings of the genotypes give the same genotypes and the same Fst′; Fst′ of `pairwise.neifst` equals Fst′ rebuilt from per-locus components to 1e-4, the rounding of `basic.stats`; limits contain the estimates; class means equal the values found in the earlier assessment to 0.002; R–R vs A–R Wilcoxon P = 0.0074 (earlier assessment about 0.009) and 0.27 as coded; 252 permutations; 40 jackknife rows; inputs unchanged). Run time 3 min 47 s on 4 cores. **Level of reproducibility T3** (our computation, no published value to match; the as-coded counterpart is T1).

**Notes**


* Version 3 of the report (10 October): the legend of panel b of `fig_10_as_coded_vs_corrected` now reads 'FST′, our correction' (it said 'correct input'); `10_d_figures.py` was run again for this figure and the label of `fig_10_panel_c_values.tsv`; the figure of the matrices (`fig_10_matrices_as_coded_vs_corrected`) was left as it was. No table of the step changed.

* Fst′ of `pairwise.neifst()` is rounded to four decimals by `basic.stats()`; the unrounded value from the components is in `Fst_prime_from_components`.
* The first submission of this step (job 63757883) failed two checks of my own (an `identical()` that compared row names, a tolerance that ignored the rounding of `basic.stats`); both were corrections to the check, not to the results; the logs are kept.

</details>


<details>
<summary>Step 11: Trees as coded by the authors (<code>11_trees_as_coded/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce the three trees (Edwards-distance NJ, Edwards-distance UPGMA, and the "FST" NJ tree) with their 10,000-replicate bootstrap values, and show on the real data what the third tree is |
| **Reads** | `GENIND_…936_Loci40_REASSIGNED_IND_IMPUTED_FINAL_Rimouski.RDS` (imputed, 936 fish incl. RIM-A) and `ALLELE_…936_Loci40_REASSIGNED_IND_FINAL_Rimouski.txt` (not imputed) of step 03; `input/node_labels_transcribed.tsv` (our transcription of the bootstrap values of Fig. 4 and Fig. S7, for the check only) |
| **Scripts** | `input/06_Tree_MS.R` (authors', **class B**: only the `base_path` line changes, `output/script_changes.diff`); `scripts/11_post_results.R` (D; writes the trees, node tables and matrix and runs the demonstration); `scripts/11_check.R` (D); `scripts/run_11_trees_as_coded.sbatch` |
| **Software** | R 4.2.1: poppr 2.9.4, ape 5.7-1, ggtree 3.6.2, tidytree 0.4.5, adegenet 2.1.10, hierfstat 0.5-11 (`output/sessionInfo.txt`) |
| **Parameters** | all the authors' (`set.seed(5)`, 10,000 bootstraps, Edwards distance, outgroup RIM-A) |
| **Provenance** | `[AUTHORS']` script, function and parameters; `[OURS]` harness, post-run writer, demonstration, check, transcription |

**Outputs (`output/`)**


`tree_edwards_nj_rerooted.nwk`, `tree_edwards_upgma_rerooted.nwk`, `tree_fst_nj_rerooted.nwk` (Newick with the bootstrap values as node labels), `tree_node_labels.tsv` (each node with its label and the tips below it), `trees_rerooted.rds`, `fst_matrix_used_for_fst_tree.tsv`, `figures.pdf` (the three trees, as the script draws them), `figures/tree_page-*.png`, `F15_aboot_on_matrix_demonstration.tsv`, `check_11.tsv`, `run_stdout.log`, `sessionInfo.txt`, `logs/`.

**Result against the paper**


* **UPGMA tree (Fig. S7A): all 10 bootstrap values equal those of the figure** (42.71, 92.95, 74.61, 77.62, 71.56, 65.9, 100, 100, 42.29, 100), at the same nodes. T1.
* **Edwards NJ tree (Fig. 4) and "FST" NJ tree (Fig. S7B): same topology, bootstrap values equal or within 0.2 at 7 of 9 nodes** (for example 44.04, 65.01, 100, 100 exactly; 27.94 against 27.93, 35.74 against 35.66, 54.55 against 54.71, 50.41 against 50.35 for Fig. 4; 88.5 against 88.47, 88.87 against 88.85, 54.6 exactly for Fig. S7B). **At two neighbouring nodes the values differ**: the clade {ETA-A, MUS-A} has 100 in our trees (Fig. 4: 44.47; Fig. S7B: 14.81) and the clade {AGU-A, ETA-A, MUS-A} has 17.73 (Fig. 4: 100) and 20.02 (Fig. S7B: 100). In both figures the "100" sits one node higher than in ours. Same code, same seed; the UPGMA tree, which shares the random draws, is reproduced exactly, so the difference arises after the bootstrap values are computed, most probably in how `root()` carries node labels when the tree is re-rooted on RIM-A in other versions of ape/poppr (`root()` moves labels with nodes unless `edgelabel = TRUE`). We have not found which version produces the figure's values; step 12 re-computes the supports on the unrooted trees to show which label belongs to which split. **T2** for both NJ trees (topology and 7 of 9 values).
* **on the real tree (`F15_aboot_on_matrix_demonstration.tsv`)**: the authors' "FST" tree has exactly the topology of `nj(nei.dist(matrix rows))` (Robinson–Foulds distance 0), that is, the matrix was treated as a data table whose rows are compared, not as distances; the tree that `nj(as.dist(matrix))` gives has Robinson–Foulds distance 4 from it. The issue note is `../12_tree_variants/FST_TREE_issue.md` (written in step 12).

**Check and level of reproducibility**


`output/check_11.tsv`: 12 integrity checks pass; of 3 comparisons with the paper 1 agrees (UPGMA) and 2 differ in two nodes each (NJ trees; topology the same). The run takes 32 s, not the 12 minutes of the plan (which extrapolated from 100 bootstraps on a different dry run). **Level of reproducibility T2 overall** (UPGMA T1).

</details>


<details>
<summary>Step 12: Tree variants (<code>12_tree_variants/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Show the effect of the points found in the trees: the support of rooted clades on NJ trees, the displaced labels after `root()`, 40 against 41 loci, and the construction and the encoding of the "FST tree" |
| **Reads** | the 40-locus genind, the imputed genotype table and the not-imputed allele table (936 fish) of step 03; the three trees of step 11 (for comparison and as a reproducibility check) |
| **Scripts** | all **class D**: `12_a_variants.R` (the data preparation of the authors' script 06, lines 49–99, is reused; the authors' `create_genind()` and `kp_ind()` are sourced), `12_check.R`, `run_12_tree_variants.sbatch`; helpers `10_common.R` of step 10. **Rerun on 10 October (job 64157365, version 3 of the report):** only names and labels were changed (`valid` → `corrected` in file names such as `tree_FST_corrected_*.nwk` and `fst_matrix_corrected_Fstprime_11sites.tsv`, and in the panel titles of the figure); the 12 trees are identical to those of the previous run (same newick files) and the check table has the same 25 rows with the same results |
| **Software** | R 4.2.1: poppr 2.9.4, ape 5.7-1, ggtree 3.6.2, hierfstat 0.5-11, adegenet 2.1.10, cowplot (`output/sessionInfo.txt`) |
| **Parameters** | the authors' (`set.seed(5)`, 10,000 replicates, Edwards distance, outgroup RIM-A) for every tree; our own bootstrap over loci for the distance trees |
| **Provenance** | `[AUTHORS']` the calls to `aboot()`, `edwards.dist`, `root()` in the variants that repeat them, the preparation of the data; `[OURS]` the variants, the unrooted supports, the bootstrap over loci, the check |

The finding is written up in `FST_TREE_issue.md`.

**Outputs (`output/`)**


`tree_<id>.nwk` (12 trees), `variants_catalog.tsv` (what each tree is), `supports_by_split.tsv` (support of every split in every tree), `robinson_foulds_unrooted.tsv`, `fst_matrix_as_coded_11sites.tsv`, `fst_matrix_corrected_Fstprime_11sites.tsv`, `fig_12_tree_variants.{png,pdf}` (six trees), `all_variant_trees.pdf` (all twelve), `data_checks.txt`, `reproducibility_checks.txt`, `check_12.tsv`, `logs/`.

**Check and level of reproducibility**


`output/check_12.tsv`: 13 of 13 integrity checks pass (12 trees with 11 sites, RIM-A on the root; the Edwards NJ tree with 40 loci recomputed here has the node labels of step 11 exactly; the genind rebuilt from the genotype table with the authors' `create_genind()` equals the deposited 40-locus genind; the 41-locus genind has 41 loci and 936 fish; Fst′ from components equals `pairwise.neifst()` to the four-decimal rounding; supports within 0 to 100). The notes of the check hold the numbers quoted in the issue note. The run takes 95 s. **Level of reproducibility T3** (our variants).

**Notes**


* The first three runs of this step failed my own check (the check could not load ape; `md5sum -L` is not an option of this `md5sum`; a rooting test that assumed a two-way root; a logical stored as the number 1); results unchanged.
* The same `md5sum -L` slip was present in step 10's job script: its input check compared two empty files. Corrected in the script; step 10 was re-run (see `DEVIATIONS.tsv`).

</details>


<details>
<summary>Step 13: Directional gene flow (divMigrate, Fig. 5) (<code>13_divmigrate/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | (a) Recompute the relative gene flow and its significance as the authors did and compare with the saved result; (b) search a fixed grid for the settings that reproduce the 22 numbers printed on Fig. 5; (c) draw Fig. 5 with the authors' code from each result that matters |
| **Reads** | the reassigned GENEPOP file of step 03 (9 populations); the reassigned and not-reassigned genind objects and the not-imputed table of step 01/03 (variants); the saved result and the deposited not-reassigned GENEPOP file (comparison and one diagnostic only) |
| **Scripts** | `13_a_run_divmigrate.R` (D), `13_b0_imputation_seeds.R` (D; the authors' `impute()`, reassignment function, `allele2genotype()` and `create_genind()`), `13_b_make_variant_files.R` (D; the authors' `genepop_creator()` in R 4.2.3 with graph4lg), `13_c_search_grid.R` (D), `13_d_post_results.R` with `input/07_MigrationDirectionality_MS.R` (authors', **class B**: the `base_path` line only), `13_e_read_fig5.py`, `13_f_variant_result.R`, `13_g_fig5_label_comparison.py`, `13_h_tables_figures.py`, `13_check.R`; sbatch files `run_13a_divmigrate`, `run_13bc_grid`, `run_13d_plot` |
| **Software** | R 4.2.1: diveRsity 1.9.90, igraph, ggraph, reshape2, tibble; R 4.2.3 with graph4lg (variant files); Python for the figure reading (`output/sessionInfo*.txt`) |
| **Parameters** | the authors': GST, 10,000 bootstraps, `set.seed(24)`, `para = FALSE` (ours: one core, repeatable) |
| **Provenance** | `[AUTHORS']` the call, the data, the plot script; `[OURS]` the grid, the transcription of the 22 values, the comparisons |

The finding is written up in `FIG5_inconsistency.md`.

**Outputs (`output/`)**


`divmigrate_authors_run/` (our result as `.RData`, `gRelMig_ours/saved.tsv`, `gRelMigSig_ours/saved.tsv`, `comparison_with_saved_result.tsv`, `arrows_ours_vs_saved.tsv`), `fig5_saved/`, `fig5_ours/`, `fig5_noreass_pooled_385/`, `fig5_deposited_noreass_pooled/` (each: the authors' script 07 run, `figures.pdf` with the drawing, the arrows drawn, the matrices), `variants.tsv`, `grid_summary.tsv`, `grid_all_arrows.tsv`, `variant_files_check.txt`, `imputation_seeds_check.txt`, `fig5_arrows_figure_vs_sources.tsv`, `fig5_label_comparison_by_position.tsv`, `fig_13_fig5_vs_sources.{png,pdf}`, `check_13.tsv`, `logs/`. The 28 GENEPOP variants are on scratch (`<scratch>/763_salmon_work/13_divmigrate/variants/`).

**Result**


* **(a) T1.** Relative gene flow equals the saved matrix (largest difference 0, with 0 and with 10,000 bootstraps); significance matrix identical in all 72 cells; the 10,000 bootstraps take 131 s on one core.
* **(b) Fig. 5 is not reproduced from the grid.** Rule fixed in advance: all 22 values within 0.0015. Best variant: not reassigned, pooled, 385 fish, `gst`: 16 of 22; the data version of the saved result: 1 of 22. Recorded as not identified within the grid.
* **Diagnostic.** Run on the deposited not-reassigned GENEPOP file (which has a different random sample of 30 of the 50 PUY-A fish), the authors' plot code puts the labels of the paper's figure at 20 of 22 positions; the remaining two are one pair of arrows with its two numbers exchanged.
* **Level of reproducibility.** (a) T1; (b) Fig. 5 **T4** for our pipeline (not reproduced from any variant of the grid), with the figure explained by the diagnostic.

**Check**


`output/check_13.tsv`: 10 integrity checks pass (runs, exit codes, inputs unchanged, 28 variant files, the seed-2 draw equal to the imputed table of step 01, the reassigned pooled file equal to the file of step 03, 44 grid runs of 22 arrows); comparisons with the saved result: 3 of 3 agree; comparisons with the figure and the deposited file: 5 differ (the not-reassigned file of the deposit, the decision rule, and the three drawings), as described above. Run times: the authors' run 2 min 21 s; grid 46 s; plots 4 to 7 s each.

**Notes**


* The plan estimated 5 minutes for 10,000 bootstraps; it took 131 s.
* A diagnostic on a deposited derived file is outside the plan's rule that such files are never inputs; it is labelled as a diagnostic and does not enter the decision rule.

</details>


<details>
<summary>Step 14: Genetic diversity (<code>14_diversity/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce Table S2 (raw and extrapolated allele richness, expected and observed heterozygosity, mean and SD over 43 loci, ten sites) and the tests of the Results (Genetic diversity); the balanced subsamples; the comparison with and without the four reassignments. Ne of Table S2 is step 15 |
| **Reads** | `ALLELE_Filtered_Individuals885_Loci43_REASSIGNED_IND_FINAL.txt` (step 03; **not imputed**; the 385 fish of known tactic), `All_Rivers_Dataset_Filtered_Balanced.txt` (step 01; same fish, original labels), `…REASSIGNED_IND_IMPUTED_FINAL.txt` (step 03; labelled diagnostic only), the authors' functions `het.R` and `allele_rich.R` (step 00); `input/tableS2_transcribed.tsv`, `input/tableS3_transcribed.tsv`, the tests of the Results (`input/README_transcribed.txt`) for the check only |
| **Scripts** | all **class D** (ours) calling the authors' functions unchanged (class A): `14_a_diversity.R`, `14_b_tests.R`, `14_c_balanced.R`, `14_d_without_reassignment.R`, `14_check.R`, `run_14_diversity.sbatch` (18 s) |
| **Software** | R 4.2.1, pegas 1.2 |
| **Parameters** | the authors': pegas `H(observed = TRUE)` and `allelicrichness(method = "raw")` and `"extrapolation"` (the extrapolation takes the ten sites as the whole data set). Ours (the paper does not state them): not imputed data; the **unit of replication is the locus** (43 per group); tactics combined means one population of the five anadromous sites and one of the five resident sites; Welch t test; ANOVA and Tukey HSD with 43 loci per site (all ten sites in one model for the pairs within a watershed); 100 subsamples with `set.seed(1)` |
| **Provenance** | `[AUTHORS']` functions; `[OURS]` driver, tests, subsampling, check, transcription |

**Outputs (`output/`)**


`diversity_per_locus_*.tsv` and `diversity_summary_*.tsv` (reassigned, not_reassigned, imputed_reassigned, missing_aware), `tests_tactic_welch_pooled.tsv` (primary), `tests_tactic_welch.tsv` (variant: per-locus means), `tests_tukey_within_watershed.tsv`, `tests_anova_among_sites_of_a_tactic.tsv`, `tests_tukey_among_resident_sites.tsv`, `balanced_subsamples_*.tsv`, `with_vs_without_reassignment.tsv`, `figures/fig_14_diversity_vs_tableS2.{png,pdf}`, `check_14.tsv`, `logs/`. Issue note: `DIVERSITY_missing_issue.md`.

**Check and level of reproducibility: **T2** (`output/check_14.tsv`: 7 integrity checks pass, 11 comparisons agree, 9 differ, 8 notes)**


* **Table S2 (one-decimal cells that our value rounds to):** raw richness 6 of 10 sites (largest difference 0.10), its SD 8, extrapolated richness 8 (0.06), its SD 6, He 8, He SD 10, Ho 10 (0.05), Ho SD 10. The two He mismatches are **MUS-LAK-R, printed 0.6 against our 0.355, and MUS-RIV-R, printed 0.4 against 0.347**; their Ho (0.3, 0.3) agree, and a mean He of 0.6 with Ho of 0.3 at a site with a He SD of 0.2 cannot come from these data. The Results text gives PUY-R raw richness as 2.4 ± 1.0 and Table S2 as 2.3 (1.0); we obtain 2.349 (1.021).
* **Tactics combined** (Welch t, 43 loci per group): raw richness t = 1.48 (paper 1.44, P = 0.14 against 0.15), extrapolated 0.55 (0.54), He 3.80 (3.73), Ho 8.40 (8.10); same decisions, t within 5 %; the degrees of freedom are 82 to 84 against 79 to 81.5, so the data differ slightly from the authors' (with the original, not reassigned, labels He gives 3.73 exactly and Ho 8.15). The other unit that we tried first, the per-locus mean over the sites of a tactic, gives t = 4.4, 0.7, 6.6 and 7.7: it does not fit the paper (richness would differ significantly).
* **Tukey and ANOVA statements of the Results:** reproduced: raw richness lower in residents in PUY and AGU (P = 6e-5 and 2e-7; paper < 0.0003); only COR-R higher than PUY-R and AGU-R in He and Ho (P < 0.005, no other resident pair); anadromous He and Ho higher in PUY, AGU and both MUS pairs (P ≤ 0.0035; paper < 0.009). **Differ in the threshold:** the other watersheds' raw richness P > 0.2 (COR 0.26, MUS-A–MUS-LAK-R 0.16, MUS-A–MUS-RIV-R 0.40); ANOVA among anadromous sites P > 0.7 (He 0.67, Ho 0.75); COR He and Ho P > 0.3 (He 0.57, Ho 0.18). The direction and significance are those of the paper in every case.
* **Balanced subsamples (100 × 25 fish):** He and Ho within 0.0011 of the unbalanced values at every site ("virtually the same"); raw richness falls by 0.18 to 0.61 at the sites with more than 25 fish (the paper uses the extrapolated value for this reason).
* **With and without the four reassignments:** He and Ho change by at most 0.019 and 0.014 (MUS-RIV-R), **raw allelic richness by 0.61 at AGU-R and 0.54 at MUS-RIV-R** (one fish of AGU-R and two of MUS-RIV-R are carriers of many alleles of the anadromous site), extrapolated richness by 0.12 (MUS-RIV-R). "No meaningful difference" holds for He and Ho, not for raw richness of these two sites (our criterion, fixed before the run: He and Ho ≤ 0.02, richness ≤ 0.1).
* **Missing genotypes** (`DIVERSITY_missing_issue.md`): the authors' functions count a missing genotype as an allele "NA" (21 locus × site cells); effect on the site means up to +0.12 in raw richness.

**Notes**


* The differences from the printed Table S2 of up to 0.1 in raw richness are of the size of the missing-genotype artifact and of the random 50 of PUY-A (the deposited balanced files differ in 30 of the 50 PUY-A fish, step 13); we did not search further.
* Not imputed data are used throughout, as the paper states for FST, diversity and ABC; the imputed variant is kept only as a diagnostic (`diversity_summary_imputed_reassigned.tsv`) and agrees less with Table S2.

**Figure**


`output/figures/fig_14_diversity_vs_tableS2.png` (`scripts/14_e_figure.py`, run in the `python` environment from the tables of this step; eight panels): a-d our value against the printed value of Table S2 (raw richness, extrapolated richness, He, Ho; resident sites orange, anadromous blue; the two Musquaro residents are labelled in c); e-f effect of counting a missing genotype as an allele (the authors' function minus the missing-aware value); g-h reassigned minus not reassigned data (richness; He and Ho).

</details>


<details>
<summary>Step 15: Effective population size (<code>15_ne_neestimator/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Estimate Ne of each of the ten sites with the linkage-disequilibrium method and the minor-allele-frequency criterion 0.05 (Table S2), and the numbers of individuals, loci and alleles used (Table S3) |
| **Reads** | the imputed genind of step 03 (`GENIND_…REASSIGNED_IND_IMPUTED_FINAL.RDS`) and the authors' `genepop_creator()` for the imputed GENEPOP file; the not imputed allele table of step 03 (reassigned labels) and of step 01 (original labels) for the not imputed GENEPOP files; the deposit-like nine-population GENEPOP file of step 03 (run for comparison); `input/tableS2_transcribed.tsv` and `tableS3_transcribed.tsv` (step 14) for the check only |
| **Scripts** | `15_a_make_genepop.R` (**class C**: the authors' GENEPOP block of script 03, lines 71–89, without its two pooling statements; R 4.2.3 with graph4lg), `15_a2_make_genepop_not_imputed.R` (D, base R), `15_b_run_neestimator.sh` (D), `15_c_collect.R` (D), `15_check.R` (D), `run_15_ne.sbatch` (8 s) |
| **Software** | NeEstimator 2.1 (`Ne2-1L`, Linux binary; copied to the work directory and made executable, as the shipped file is not), R 4.2.3 + graph4lg, R 4.2.1 |
| **Parameters** | the authors': LD method, lowest allele frequency 0.05 (the output also contains 0.02, 0.01 and 0, kept and not used). Ours: random mating; one population per site with the fish sampled there (reassigned labels); no `option` file (preflight: an option file of ours made the program run only the first population) |
| **Provenance** | `[AUTHORS']` function and parameters; `[OURS]` the rest |

**Outputs (`output/`)**


`genepop/` (ten-population GENEPOP files: imputed, not imputed, not imputed with the original labels), `neestimator/<run>_neestimator_output.txt` (the program's text output), `<run>_ne_long.tsv` (one row per population and critical value: Ne, parametric and jackknife limits, harmonic mean sample size, independent comparisons, r²), `<run>_loci_alleles_removed.tsv`, `<run>_info.txt`; four runs: `ten_sites_not_imputed` (**primary**), `ten_sites` (imputed GENEPOP file), `deposit_9pops_like` (the nine-population file with MUS-R pooled) and `ten_sites_not_imputed_original_labels`; `figures/fig_15_ne_vs_tableS2.{png,pdf}`; `check_15.tsv`; `logs/`.

**Check and level of reproducibility: **T1 for 8 of 10 sites, T2 overall** (`output/check_15.tsv`: 6 integrity checks pass, 10 comparisons agree, 6 differ, 4 notes)**


* **Ne and the parametric limits of Table S2 are reproduced exactly at eight of ten sites with the not imputed data:** PUY-R 290.2 (101.1 – infinite; the paper prints NA), COR-R 13.2 (10.3–17.5), AGU-R 161.3 (79.3–1633.2), AGU-A 87.0 (71.8–108.7), MUS-LAK-R 8.5 (7.1–10.0), MUS-RIV-R 19.8 (16.7–23.5), MUS-A 41.6 (35.2–50.0), ETA-A 44.9 (34.2–63.1). **Two sites differ:** PUY-A 54.8 (46.9–65.0) against 56.8 (48.5–67.8) and COR-A 146.2 (100.8–252.8) against 139 (96.6–236.2). With the imputed GENEPOP file (the only GENEPOP file of the deposit, with the Musquaro residents pooled) only 2 of 10 sites agree (the two sites without any missing genotype, MUS-A and MUS-RIV-R); with the not imputed data and the original labels 3 of 10. We therefore take the **not imputed data as the primary run**; this choice was made after seeing the imputed run (the authors do not state the input of the Ne analysis) and rests on the exact agreement at eight sites.
* The two differing sites are those with random choices in the data: the 50 PUY-A fish are a random sample (the deposited balanced files differ from ours in some fish, see step 13), and COR-A is the site that contains the reassigned fish; we did not search for a combination that reproduces them.
* **Claims of the Results:** resident Ne 8.5 (7.1–10) to 19.8 at the three reliable sites agrees; the anadromous range (41.6–139.0 in the paper) is 41.6–146.2 here because of COR-A; the parametric intervals of the reliable resident and anadromous sites do not overlap (largest resident upper limit 23.5, smallest anadromous lower limit 34.2); PUY-R and AGU-R are the two sites without a reliable estimate (upper limit infinite and 10 times Ne), the paper's asterisks.
* **Table S3 does not match.** The number of individuals NeEstimator uses equals the number of fish of the site (49, 50, 15, 41, 45, 47, 36, 44, 35, 23); Table S3 prints 47, 120, 91, 118, 54, 141, 69, 70, 122, 118 (**more individuals than are available at 9 of 10 sites**, by 26 to 95 at eight of them, up to 95 more at ETA-A, which has 23 fish). We cannot say what the printed numbers count and did not search for a grouping that would give them. Our count of the loci for which fewer than two alleles reach a frequency of 0.05 equals the printed number of loci removed at 5 of 10 sites (16, 1, 7, 1, 18, 1, 8, 12, 5, 5 against 16, 1, 5, 1, 17, 1, 8, 11, 4, 3); our count of alleles below 0.05 (of 326 alleles of the 385 fish; the paper has 348 alleles for the whole data set) does not equal any printed value. What "removed" counts in Table S3 is therefore not established.

**Notes**


* NeEstimator reproduces itself across inputs: the nine-population (pooled) run and our ten-population imputed run give identical Ne and limits for the eight shared populations (check).
* The jackknife limits are in `*_ne_long.tsv`; Table S2 prints the parametric ones. Estimates with an infinite upper limit are at PUY-R and (jackknife) AGU-R.
* The Ne values of MUS-A and MUS-RIV-R do not depend on the imputation because those two sites have no missing genotype.

</details>


<details>
<summary>Step 16: Private alleles of residents (<code>16_private_alleles/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Reproduce the two alleles of Fig. S8 (`Ssa_15_3`.64 and `Ssa_26_1`.51), list every allele that meets the paper's definition, and compare the number with what a random assignment of the fish to sites gives |
| **Reads** | `ALLELE_Filtered_Individuals885_Loci43_REASSIGNED_IND_FINAL.txt` of step 03 (not imputed; the 385 fish of known tactic; reassigned labels) |
| **Scripts** | all **class D** (ours): `16_a_private_alleles.R` (base R), `16_check.R`, `run_16_private_alleles.sbatch` (8 s); the figure was drawn in the `python` environment from `output/allele_copies_and_frequencies_all.tsv` and `permutation_counts.tsv` |
| **Software** | R 4.2.1 |
| **Parameters** | the paper's sentence only: "private alleles that were shared between two or more populations of this tactic" (Methods) and "two alleles from two loci exclusive to resident sites AGU-R, MUS-LAK-R, and MUS-RIV-R, ... absent from anadromous populations" (Results). Definitions used (ours): **A** (primary, literal) present in at least two of the five resident sites and in none of the five anadromous sites; **B** the same with the two Musquaro resident sites as one population; **C** (a reading written after the first run) present in AGU-R, MUS-LAK-R and MUS-RIV-R and in no other site. Permutation: fish reassigned to the ten sites at random (site sizes kept), 10,000 times, `set.seed(16)` |
| **Provenance** | `[OURS]` |

**Outputs (`output/`)**


`allele_copies_and_frequencies_all.tsv` (326 alleles × ten sites: copies and frequencies), `private_allele_candidates.tsv`, `private_alleles_definitions_and_permutation.tsv`, `permutation_counts.tsv`, `alleles_absent_from_anadromous_in_one_resident_site.tsv` (22 alleles), `figures/fig_16_private_alleles.{png,pdf}`, `check_16.tsv`, `logs/`.

**Check and level of reproducibility: **T1 for the two reported alleles, T3 for the rest** (`output/check_16.tsv`: 4 integrity checks pass, 4 comparisons agree, 2 differ, 3 notes)**


* **The two alleles of the paper are reproduced.** `Ssa_15_3`.64 occurs in AGU-R (1 copy, frequency 0.011), MUS-LAK-R (4; 0.056) and MUS-RIV-R (5; 0.057); `Ssa_26_1`.51 in AGU-R (2; 0.022), MUS-LAK-R (1; 0.014) and MUS-RIV-R (2; 0.023); both are absent from the five anadromous sites and from PUY-R and COR-R. The frequency ranges (0.01–0.06 and 0.01–0.02) and the three sites are those printed.
* **They are not the only ones.** Under definition A **five** alleles qualify (`Ssa_11_8`.67: COR-R 1 copy, AGU-R 12, MUS-LAK-R 1; `Ssa_14_5`.45: MUS-LAK-R 3, MUS-RIV-R 3; `Ssa_19_1`.54: MUS-LAK-R 3, MUS-RIV-R 6; and the two reported); with the Musquaro residents as one population (B) three (`Ssa_11_8`.67 and the two reported). Only definition C gives exactly the two reported alleles. The paper does not say which definition it used and does not mention the others.
* **Permutation baseline (descriptive).** With the fish assigned to the ten sites at random, the mean number of alleles meeting A is **4.99** (observed 5; P of at least 5 = 0.56), B 4.60 (observed 3; P = 0.84), C 0.17 (observed 2; P = 0.016). The existence of alleles shared by resident sites and absent from the anadromous ones is therefore **what random labelling gives** under the literal definitions, because these alleles are rare (5 to 14 copies in the 385 fish). Only the pattern of the three named sites (C, written after the first run and one of three readings, so P is not corrected for the choice) is more frequent than expected.
* The paper uses the two alleles "as a possible indication of ancestral gene flow between resident sites". With 5 to 10 copies each, in 3 of 5 resident sites, the data do not distinguish this from chance for the literal definition; we draw no conclusion from definition C.

**Notes**


* Frequencies are copies divided by the alleles called at the locus in the site (missing genotypes left out), so they differ slightly from frequencies computed on imputed data.
* 22 further alleles occur in a single resident site and in no anadromous site; they are not private in the paper's sense (shared between populations of the tactic).

</details>


<details>
<summary>Step 17: Approximate Bayesian computation (<code>17_abc_diyabc/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Re-implement the ABC analysis: choose between the two scenarios of Fig. S1 and estimate the parameters of the preferred one, from 5,000,000 simulations and the 144 summary statistics of the paper |
| **Reads** | `input/All_Rivers_Dataset_Filtered_Balanced_Reassigned.txt` (the 885-fish allele table of step 01 with the reassignment, not imputed; 385 fish of known tactic in the six populations of the paper: PUY-A, PUY-R, AGU-A, AGU-R, MUS-A and the two Musquaro residents pooled; 306 fish used); our transcription of Table S1 (`input/tableS1_posterior_published.tsv`, for the comparison only) |
| **Scripts** | all **class D** (the deposit has no DIYABC file): `17_a_make_project.R` with `run_17a_make_project.sbatch` (data file, header, settings), `17_b_simulate.sbatch` (first batch of 3 tasks, then an array of 47), `17_check_first_batch.sbatch`, `17_b_merge.sbatch` (hard-links and merges the 50 tables), `17_c_analyse.sbatch/.R` (scenario choice, parameter estimation, PCA, figures, comparison), `17_d_raw_rejection.py` and `17_e_tables_figures.py` (post-processing) |
| **Software** | DIYABC 2.1 command-line engine `general` (built from the source in `02_envs/external`, `-t` threads); R 4.2.1 for parsing and figures; Python for the post-processing |
| **Parameters** | from the paper and Table S1: scenarios, priors (N1–N6, NA1, t1, t2 log-uniform; mutation rate 3e-4 to 9e-4), stepwise mutation model, 144 statistics, 5 million simulations. Not stated in the paper (ours, logged in `output/project/settings.tsv`): GUI-default mutation-model parameters, motif size 2 and range 40 for every locus, ordering condition t1 < t2, 50,000 nearest records, logit transformation |
| **Provenance** | `[AUTHORS']` scenarios, priors, statistics, number of simulations; `[OURS]` everything else |

**Outputs (`output/`)**


`project/` (data, header, settings, `loci_alleles_and_range.tsv`, RNG states), `merge_check.tsv`, `analysis_check.tsv`, `first_batch_*` (the check of the first three tasks), `engine_outputs/` (posterior summaries, adjusted vectors, scenario choice, first PCA components), `results/` (`summary_step17.tsv`, `scenario_choice_all_n.tsv`, `parameters_vs_tableS1.tsv`, `parameters_published_adjusted_raw.tsv`, `time_order_raw_vs_adjusted.tsv`, figures: `scenario_choice_logistic`, `prior_posterior_densities`, `pca_simulated_vs_observed`, `fig_17_t1_t2_raw_vs_adjusted`), `raw_rejection/`, `logs/`, `failed_attempts/`. The reference table (3.1 GB, 5,000,000 records) stays on scratch (`<scratch>/763_salmon_work/17_abc/merged/`, md5 in `reftable_merged.md5`). The issue note is `ABC_time_order_issue.md`.

**Results against the paper**


* **Scenario choice.** Scenario 2 (common residency origin) is preferred, as in the paper: logistic regression gives P = 1.0000 (95 % limits 0.9998–1.0000) at 50,000 records and 1.0000 at every size from 5,000 (paper 0.9971, limits 0.9966–0.9977). The direct approach, which the paper does not report, gives only 0.72 (0.68–0.76) at 50,000 nearest records (0.89 at 500). The two methods agree on the ranking, not on its strength.
* **Observed statistics.** The observed point lies within the cloud of simulated statistics (PCA on the first 100,000 records: PC1 8.73, PC2 5.23; 21.8 % of simulated records farther from the centre), the check of Fig. S11.
* **Parameters.** Posterior 95 % limits of 10 of 10 parameters lie inside the prior (the paper: all except the mutation rate, Fig. S10). The adjusted posterior means differ from Table S1: population sizes 0.20–0.51 times the published (N1 120 against 607, N2 57 against 122, N3 2,520 against 4,980, N4 179 against 452, N5 605 against 1,870, N6 779 against 2,270), NA1 1,650 against 7,250, t1 102 against 103, t2 68.7 against 37.3, mutation rate 4.56 × 10⁻⁴ against 4.2 × 10⁻⁴. The raw (unadjusted) means are closer for some (N2 110, N4 390, N1 816) and farther for others (N3 1,420, NA1 2,055), so the difference is not a simple scale.
* **Time order.** See `ABC_time_order_issue.md`.

**Check and level of reproducibility**


`merge_check.tsv`: 5 of 5 pass (50 tables, all of 100,000 records with distinct checksums, merge exit 0, 5,000,000 records). `analysis_check.tsv`: 14 of 14 pass (the first run failed one row, a wrong expectation of mine: the engine uses only the 2,498,090 records of the chosen scenario, not all 5,000,000; analysis re-run, job 63760711). **Level of reproducibility T3**: our re-implementation; the scenario ranking agrees with the paper, the parameter values do not reproduce Table S1.

**Notes**


* Cost: 50 tasks of 100,000 simulations, 26 min each on 8 threads (about 175 core-minutes per task, 146 core-hours in total; the plan said 190), 130 MB of memory per task; merge 19 s; analysis 2 min 37 s.
* The preflight dry run had shown logistic regression at n ≤ 3000 to be unstable on a 300,000-record table; on the full table the results are stable at all ten sizes.
* The allele sizes are in base pairs with steps of 3 at 37 loci (steps of 4, 2, 1 at the others); with motif 2 the engine simulates steps of 2 base pairs. This mismatch between the data and the assumed motif is a possible reason for the difference of the population sizes; it is not tested here.

</details>


<details>
<summary>Step 18: Synthesis (<code>18_synthesis/README.md</code>)</summary>

| | |
|---|---|
| **Purpose** | Bring the results of steps 00-17 together: claims C1-C9 with the paper's number, our number, level of reproducibility and status; each supplementary figure beside our reproduction; an index of the issue notes and of every comparison that differs; the sites of Table 1; the final report with a discussion paragraph on the corrected FST pattern, the ABC and drift; the deviations; what was not done; and the **comprehensive report** (one self-contained document, `../comprehensive_report/comprehensive_report.{md,html}`) |
| **Reads** | the output tables of steps 00-17 (`output/*check*.tsv`, `fst_corrected_*.tsv`, `fst_as_coded_class_means.tsv`, `summary_best_K.tsv`, `summary_step17.tsv`, step 14 and 15 tables, step 16 table), `STATUS.tsv`, `DEVIATIONS.tsv`, the figures of steps 05-17, the supplement (`00_raw_inputs/paper/cjfas-2025-0204suppla.docx`, read into memory for the comparison figures only), `input/table1_n_transcribed.csv` (n of Table 1, copied from `01_planning/prelim_checks/`), `input/issues_catalog.tsv` (written by hand from the step READMEs and issue notes) |
| **Scripts** | all **class D** (ours): `18_a_claims_table.py`, `18_b_figures_side_by_side.py`, `18_c_report.py`, `18_e_diagrams.py` (the workflow maps and the two ABC scenario trees drawn from `header.txt`; version 3 has a larger map with the claims under each box, thick green arrows for the three pairs of steps that read an earlier result and grey lines for the data of steps 01-03), `18_d_comprehensive_report.py` with `report_lib.py` and `report_content/` (`part_a.py`, `part_b1.py`, `part_b2.py`, `part_c1.py`, `part_c2.py`, `part_d.py`, `steplib.py`, `methods_text.py`, `wording.py`, `datasummary.py` (the tables of the descriptive columns, of the 43 loci and of the made-up-data test), `glossary.py` (the definitions of section 3), `captions.py` (the captions of all figures)), `18_check.py`, `run_18_synthesis.sbatch` (about 2 minutes). The plan named R scripts; Python was used because the step only reads tables and arranges images (`DEVIATIONS.tsv`) |
| **Software** | the Claude Science `python` environment (`<conda>/envs/python`; Python 3.11, pandas 2.3.3, numpy 2.4.6, scipy 1.17.1, matplotlib 3.11.0, pillow 12.3.0, markdown-it-py 4.2.0; versions in `output/logs/<jobid>_*/versions.txt`). It is not in `02_envs/lockfiles/`: the step needs no statistical package beyond `scipy.stats.spearmanr` |
| **Provenance** | `[OURS]`; the paper's numbers in `claims_table.tsv` are transcribed from the PDF text |
| **Check** | `18_check.py` (25 integrity rows; the last nine concern the comprehensive report (version 2, and that version 1 is unchanged; no user name or private path in the text): files larger than 1 MB, one section for each of the 19 steps, nine claims each with a collapsible block, every image embedded as a data URI with at least 25 figures, html with as many images as the markdown and balanced collapsible tags and at least 100 tables, the totals of the final runs stated, no placeholder or internal finding number; the first 16: all steps checked, no failed integrity row anywhere, counts per step equal the counts in the STATUS notes, class means recomputed from the pairs, 385 and 885 fish, site counts equal Table 1, evidence files exist, seven issue notes with five sections, figures present, authors' images not copied) |

**Outputs (`output/`)**


* `reproduction_report.md`: summary, claims, discussion paragraph (FST, ABC, drift), step overview, sites, figures, deviations, what was not done, how to continue
* `claims_table.tsv`, `claims_table.md`: C1-C9; columns claim, statement, paper_number, our_number, level of reproducibility, steps, status, comment
* `issues_index.md`: seven issue notes (A), ten smaller discrepancies and audits (B), the 28 comparison rows that differ (C)
* `deviations_summary.md`: the rows of `DEVIATIONS.tsv`
* `site_table.tsv`: the twelve groups of Table 1 (our n before and after the reassignment of four fish, Table 1's n, years, life stage)
* `step_overview.tsv`, `all_checks_long.tsv`, `differences_index.tsv`: counts per step and every check row of steps 00-17
* `figures/fig_18_side_by_side_{S1C,S2,...,S11}.png` and `fig_18_side_by_side_all.pdf`: the authors' figure (left) and ours (right)
* `figures/fig_18_authors_S5_upright.png` (the authors' Fig. S5 turned upright, read in memory from the supplement and written once as a single panel for the report), `figures/fig_18_workflow_map_claims.png` (version 3; `fig_18_workflow_map_as_run.png` is the version 1 map), `fig_18_abc_scenarios_drawn.png`: the diagrams of the comprehensive report
* `check_18.tsv`
* **Outside `output/`:** `../comprehensive_report/comprehensive_report_v2.md` and `.html` (same content; about 4.7 MB each; every figure embedded, so they can be shown without any other file)

**Results**


* **Claims.** C1 supported (PCA percentage of total variation identical, STRUCTURE best K = 2 in all four watersheds, FST of our correction within watersheds 0.169-0.398); C2 supported with exceptions (largest FST in PUY, smallest in COR, the Musquaro samples are one population); C3 supported, numbers within rounding (Welch t 1.48 / 0.55 / 3.80 / 8.40 against 1.44 / 0.54 / 3.73 / 8.10; Ne equal at 8 of 10 sites); C4 supported, K = 4 of the anadromous cluster not reproduced; **C5 not supported by our FST computation** (R-R 0.351, A-R 0.275, Wilcoxon P 0.0074, site permutation P 0.22; PUY-R is not closer to other residents than to PUY-A); C6 direction supported (logistic 1.0000 against paper 0.9971; direct 0.72); C7 partly supported (saved result reproduced exactly, Fig. 5 drawn from something else); C8 supported (FST 0.0029 as coded, 0.0073 our correction); C9 not testable.
* **Counts over steps 00-17** (final runs): 302 integrity rows pass, none fail; 61 comparisons agree, 28 differ.
* **Issues.** Seven issue notes and ten smaller discrepancies in `issues_index.md`.
* **Discussion paragraph** (report section 3): the corrected FST (residents not closer to each other than to anadromous fish) and the ABC (common origin preferred) are compatible if the residents share ancestry but have drifted apart; with the Ne of three resident sites at 8.5-19.8, drift alone reaches F_t above 0.7 within 50 generations. High R-R FST therefore does not separate common from independent origin, so the FST argument of C5 cannot carry the inference; the ABC is the only analysis built for it and has the limits listed (no gene flow, rebuilt project, method dependence, time order).

**The comprehensive report**


`../comprehensive_report/comprehensive_report_v3.{md,html}` (version 3) is built by `18_d_comprehensive_report.py` from the tables, figures and notes of steps 00-18 (nothing is recomputed except the elementary drift formula of section 8). **Versions 1 and 2** (`comprehensive_report.md` and `.html`; `comprehensive_report_v2.md` and `.html`) are kept unchanged (their md5 sums are in `comprehensive_report/v1_md5.txt` and `v2_md5.txt`, checked by `18_check.py`; the scripts that built them are in `v1_scripts/` and `v2_scripts/`). **Version 3 follows comments on version 2:** a note on the collapsible blocks before the first one; the 20 descriptive columns and the 43 loci with their alleles (section 2.2); the R objects of the deposit (section 2.4); definitions that say how each quantity is computed (section 3, e.g. allele frequency with an example, private allele, null allele, genind); the test with made-up data that checks the correction of FST (step 10 and section 3.3); a map with explained arrows; whole folder trees of the deposit and of every step (`tree -l` without the targets of the links); captions without numbers that say whether the figure has a counterpart in the paper, what is plotted and what the axes and symbols are; a summary table for step 06; a didactic account of linkage disequilibrium, with a made-up example, and of the T2 test (step 07); STRUCTURE and Evanno's second difference explained in simple words, and the RANDOMIZE setting (step 08); how FST is measured, with a worked example (step 09); the statements of the Results that change, as paper statement, our computation and proposed correction (step 10); the difference between steps 11-12 and 09-10 (trees); a simpler Fig. 5 table with a plain message (step 13); null alleles; the t1/t2 issue in simple words (step 17). Version 2 followed comments on version 1: private paths removed, 'level of reproducibility' instead of 'level of reproducibility', 'our correction' instead of 'valid' for FST, explanations of every method with how to read its values (`report_content/methods_text.py`), the tree of the deposit, claims under each box of the workflow map, the data layouts and the function documentation for FST (step 10), a table of the Table S3 values, the arrows of Fig. 5 by source, and the wording rules of `report_content/wording.py`. Contents: the paper in brief (sites and codes, claims C1-C9), the data, a key to terms and notations (levels of reproducibility, script classes, 'as coded by the authors' and 'our correction', reassigned and not reassigned, percentage of total variation), the workflow map as run, then one section per step (what we set out to do, the method in brief in a collapsible block, inputs of our run separated from the authors' files used only for comparison, what we found, the comparison with the paper, the problem found, scripts and folder tree in collapsible blocks), the claims revisited, the seven problems in plain words, the discussion of FST and ABC, what was not done and the deviations, and the step notes as Appendix A. Collapsible blocks are HTML `details` elements (they work in the html file and in markdown viewers that allow inline HTML); figures are palette PNG data URIs (31 figures in version 3). The section texts are in `scripts/report_content/`; edit those files and re-run the job to change the document.

**What this step does not do**


No new analysis of the genotypes. The status words of the claims table describe our numbers; they are not a verdict on the authors' intent. Fig. 1 (map) was not made. The authors' supplement images are used for the comparison figures only and are not copied to the output.

</details>


