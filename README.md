# arXiv forecasts

The [arXiv](https://arxiv.org) is a major open-access repository for preprints in fields like Physics, Mathematics, and Computer Science. Given the high and varying volume of daily submissions, the visibility of a preprint can depend heavily on when it is posted.

The goal of this project is to develop a short-term forecasting algorithm that predicts the number of preprints to be posted per category in the upcoming week. By identifying days with the lowest expected submission counts, the model would help researchers to strategically select submission dates to maximize visibility and audience reach.

The strategy is based on the assumption that preprint visibility is inversely related to the number of submissions on a given day --- i.e. the fewer competing papers, the higher chance of being noticed. This is supported by the analysis of the arXiv usage data, which shows that usage remains fairly stable throughout the week and does not increase proportionally with the number of submissions.

A more detailed description of the project is available in the [executive summary]().

## Repository structure
[`results`](results) contains the complete annotated pipeline, divided into a series of seven notebooks of notebooks:
- [`1_cleaning.ipynb`](results/1_cleaning.ipynb): data cleaning
- [`2_pre-processing.ipynb`](results/2_pre-processing.ipynb): data pre-processing (makes dataset of total posts per day)
- [`3_usage-summary.ipynb`](results/3_usage-summary.ipynb): study daily usage data
- [`4_baseline-models.ipynb`](results/4_baseline-models.ipynb): compare four baseline models and selects the best performing one
- [`5_model-training.ipynb`](results/5_model-training.ipynb): trains Holt-Winters, SARIMA, and Facebook Prophet models, optimizing parameters
- [`6_model-testing.ipynb`](results/6_model-testing.ipynb): tests the best models of the previous steps on the testing sample
- [`7_results-analysis.ipynb`](results/7_results-analysis.ipynb): summarizes the results of the model testing

[`data`](data) contains all the datasets used in the project:
- [`arxiv-categories.json`](data/arxiv-categories.json): list of all current categories.
- [`arxiv-metadata-id-versions-categories.parquet`](data/arxiv-metadata-id-versions-categories.parquet): the arXiv metadata stripped of all columns except for `id` (`string` - the arXiv ID), `versions` (a dictionary containing data about the published versions), and `categories` (`list(string)` - the list of categories the entry is posted in).  No date extraction or cleaning of missing/legacy categories yet.
- [`arxiv-metadata-id-date-categories.parquet`](data/arxiv-metadata-id-date-categories.parquet): same as the previous, but with `date` (`datetime` - the publishing date of the first version v1) in place of `versions`.
- [`arxiv-metadata-cleaned.parquet`](data/arxiv-metadata-cleaned.parquet): same as the previous but with cleaned categories.  This is the starting point for pre-processing.
- [`arxiv-totals.parquet`](data/arxiv-totals.parquet): a date-indexed dataset recording the number of preprints submitted per category on each day. This dataset is the primary input for forecasting models.
- [`arxiv-snapshots.parquet`](data/arxiv-snapshots.parquet): a date-indexed dataset recording cross-listings for each day (for future usage).
- [`arxiv-usage.parquet`](data/arxiv-usage.parquet): a date-indexed dataset of daily connections to the arXiv server.

[`notes`](notes) contains drafts.

## Getting started
Create the Conda environment
```sh
conda env create --file=environment.yml
```

Activate the new environment
```sh
conda activate arxiv-heatmaps
```

Install the additional dependency for `plotly` logarithmic heatmaps
```sh
pip3 install git+https://github.com/SengerM/plotly_utils
```

Install the kernel in Jupyter
```sh
python -m ipykernel install --user --name arxiv-heatmaps
```

### [Requirements](environment.yml)
- Python 3.13
- `jupyter`
- `matplotlib` >= 3.8
- `pandas` >= 2.0
- `pip` >= 24.0
- `pyarrow` 
- `plotly`
- `statsmodels`
- `scikit-learn`
- `prophet`
- `kagglehub`
- `engineering-notation==0.10.0`