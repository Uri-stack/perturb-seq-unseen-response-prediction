# perturb-seq-unseen-response-prediction
A lightweight machine learning pilot using Norman et al. (2019) Perturb-seq data to evaluate transcriptional response prediction for unseen genetic perturbations using a mean-response baseline and Ridge regression.
# Predicting Transcriptional Responses to Unseen Genetic Perturbations Using Perturb-seq

## Overview

This project investigates whether machine learning can predict transcriptional responses to genetic perturbations whose target genes were not included during model training.

Using a subset of the Norman et al. (2019) Perturb-seq dataset, the project compares a simple mean-response baseline with Ridge regression using co-expression features derived from control cells.

The work was developed as an application-oriented mini-project for **PBMG607L — Artificial Intelligence in Healthcare**.

## Research Question

Can a machine learning model generalize to unseen single-gene perturbation targets, and do control-derived co-expression features improve prediction over a simple mean-response baseline?

## Dataset

- **Source:** Norman et al. (2019), *Exploring genetic interaction manifolds constructed from rich single-cell phenotypes*
- **Experimental system:** K562 human leukemia-derived cell line
- **Perturbation approach:** CRISPR-based gene activation
- **Analysed subset:** 10,600 cells and 582 genes
- **Response representation:** Mean expression in perturbed cells minus mean expression in control cells
- **Response panel:** 500 genes
- **Targets used in model comparison:** 102 matched perturbation targets

The analysis uses a manageable subset of the public research dataset to support CPU-based execution in Google Colab.

## Methodology

### 1. Data preprocessing

The dataset is processed to retain control cells and single-target perturbations for the analysis. A sample of control cells is used to estimate the reference expression profile.

### 2. Response calculation

For each perturbation target, a response vector is calculated as:

`Response = Mean expression in perturbed cells − Mean expression in control cells`

Each vector represents the average transcriptional response across the selected 500-gene panel.

### 3. Models

**Mean-response baseline**

Predicts the average response vector across training perturbations for every held-out target. This establishes a simple reference for evaluating whether a more complex model adds predictive value.

**Ridge regression with co-expression features**

Uses target-gene co-expression profiles derived from control cells to learn a regularized mapping from target features to transcriptional responses.

### 4. Evaluation

The models are evaluated using target-level train/test splits, with approximately 80% of matched targets used for training and 20% held out for testing. The comparison is repeated across five random seeds.

Evaluation metrics:

- **Pearson correlation:** Measures linear agreement between predicted and observed responses.
- **Spearman correlation:** Measures rank agreement.
- **Root mean squared error (RMSE):** Measures prediction error; lower values are preferable.

## Results

The mean-response baseline outperformed Ridge regression on all three reported metrics.

| Metric | Mean-response baseline | Ridge regression |
|---|---:|---:|
| Pearson correlation | 0.5222 ± 0.0234 | 0.3408 ± 0.0548 |
| Spearman correlation | 0.4827 ± 0.0163 | 0.3396 ± 0.0307 |
| RMSE | 0.0865 ± 0.0088 | 0.1060 ± 0.0095 |

Values are reported as mean ± standard deviation across five random seeds.

These results indicate that the tested co-expression features did not improve prediction of unseen perturbation responses in this pilot.

## Key Takeaway

The project demonstrates the importance of evaluating machine learning models against simple baselines. More complex models do not necessarily perform better, and feature representations should demonstrate measurable predictive value.

The results are exploratory and do not establish that biological features are universally unhelpful for perturbation-response prediction.

## Limitations

- The analysis uses a subset of the original dataset.
- Three target labels were excluded from the model comparison because they did not match gene names in the expression matrix.
- Feature selection was fixed globally before target-level splitting rather than performed entirely within each training split.
- Only five random splits were evaluated.
- The model has not been externally validated.
- The analysis predicts average transcriptional responses in a cell line and does not establish causal mechanisms or clinical utility.

## Tools and Technologies

- Python
- Google Colab
- AnnData
- NumPy and pandas
- SciPy
- scikit-learn
- Matplotlib

## Reproducibility

The accompanying Jupyter notebook contains the analysis workflow. For reproducible results, execute the intended pipeline from beginning to end and verify that the displayed outputs, saved metrics, figures, and report correspond to the same run.

## Ethical Considerations

This project uses public research data from experimentally perturbed cell-line samples. It is intended for educational and hypothesis-generation purposes only.

The model is **not clinically validated** and must not be used for diagnosis, treatment selection, or patient-care decisions.

## References

1. Dixit, A. et al. (2016). Perturb-Seq: Dissecting molecular circuits with scalable single-cell RNA profiling of pooled genetic screens. *Cell*, 167(7), 1853–1866.e17. https://doi.org/10.1016/j.cell.2016.11.038
2. Norman, T. M. et al. (2019). Exploring genetic interaction manifolds constructed from rich single-cell phenotypes. *Science*, 365(6455), 786–793. https://doi.org/10.1126/science.aax4438
3. Hoerl, A. E., & Kennard, R. W. (1970). Ridge regression: Biased estimation for nonorthogonal problems. *Technometrics*, 12(1), 55–67. https://doi.org/10.1080/00401706.1970.10488634
