# arXiv heatmaps

The _[arXiv](https://arxiv.org)_ is the major open-access repository of electronic preprints for Physics, Mathematics and Computer Science, among other fields.  Preprints are classified into several categories, and have been posted since the late 80s.

The goal of this project is to study the interaction between categories, measured by cross-listings, and its evolution in time.  In particular, we would like to see if it is possible to predict increased activity of certain categories based on increase in activity in related categories.

As a follow-up, we would like to implement a "live forecast" for postings in the arXiv.

## Implementation

### Datasets
The complete arXiv metadata is freely available on [Kaggle](https://www.kaggle.com/datasets/Cornell-University/arxiv/data).  The dataset seems to cover the entirety of arXiv's history, and is maintained directly by the arXiv.

### Cleaning
- remove all unnecessary metadata, keeping only `id`, `version`, and `categories`
- extract the first upload date from `version`, put it into a new column `date`, remove the `version` column
- `categories` are listed in a single string, separated by white spaces: turn it into a list
- arXiv categories changed in 2007: find the legacy categories and replace them with the new ones

### Pre-process data
- list all possible combinations of categories
- group the listings by date
- for every `date`, count the cross-listings and the totals
- save the count into two new dataframes (`arxiv_snapshots` and `arxiv_totals`), indexed by `date`

### Data analysis
To be figured out.  We definitely need some smoothing, but I don't know yet how to do the forecasting part.

### Visualization
Also to be figured out.  We might use correlation-like heatmaps from `seaborn`, or maybe a network plot.  It would be nice to have  an interactive graph that evolves in time.

## Getting started
Create the Conda environment
```sh
conda env create --file=environment.yml
```

Activate the new environment
```sh
conda activate arxiv-heatmaps
```

Install the kernel in Jupyter
```sh
python -m ipykernel install --user --name arxiv-heatmaps
```

### Requirements
- Python 3.13
- `jupyter`
- `matplotlib` >= 3.8
- `pandas` >= 2.0
- `pip` >= 24.0
- `pyarrow` 
- `plotly`
- `engineering-notation==0.10.0`