# arXiv heatmaps

The _[arXiv](https://arxiv.org)_ is the major open-access repository of electronic preprints for Physics, Mathematics and Computer Science, among other fields.  Preprints are classified into several categories, and have been posted since the late 80s.

The goal of this project is to create a forecasting algorithm of postings on the arXiv per category, both short-term (to optimize visibility) and long-term (to optimize audience size).


## Stakeholders and KPIs
### Short-term predictions (optimizing visibility)
**Stakeholders**: Researchers trying to optimize their preprint visibility.

**Idea**: Assuming that the probability a preprint being opened is inversely proportional to the number of papers listed in the same area on the same day, then one would aim to post papers on days when fewer papers are posted.

**KPIs**: The forecasting algorithm should significantly decrease the expected number of other papers posted on the same day as the stakeholder's paper, compared to average number of paper posted per day.


### Long-term predictions (optimizing audience size)
**Stakeholders**: Researchers trying to predict which field is going to become popular and in-demand, to increase their audience.

**Idea**: Given a starting category $C_0$, a starting time $t_0$, and a time interval $\Delta t$, the algorithm should recommend whether to stay in the field $c_0$ or try to move towards an adjacent field $C_1$ (adjacency could be measured by number of cross-listings), for the purpose of maximizing the audience of a paper posted at time $t_0 + \Delta t$.  A proxy for the audience of a category $C$ could be the number of monthly (?) postings in $C$.

**KPIs**: The recommended category should reliably have more monthly postings than the average of monthly postings of all the adjacent fields (possibly weighted by "adjacency", i.e. cross-listings).


## Implementation and checkpoints

### Datasets
The complete arXiv metadata is freely available on [Kaggle](https://www.kaggle.com/datasets/Cornell-University/arxiv/data).  The dataset seems to cover the entirety of arXiv's history, and is maintained directly by the arXiv.

### Cleaning (`cleaning.ipynb`)
- [x] remove all unnecessary metadata, keeping only `id`, `version`, and `categories`
- [x] extract the first upload date from `version`, put it into a new column `date`, remove the `version` column
- [x] `categories` are listed in a single string, separated by white spaces: turn it into a list
- [x] arXiv categories changed in 2007: find the legacy categories and replace them with the new ones

### Pre-process data (`crunching.ipynb`)
- [x] list all possible combinations of categories
- [x] group the listings by date
- [x] for every `date`, count the cross-listings and the totals
- [x] save the count into two new dataframes (`arxiv_snapshots` and `arxiv_totals`), indexed by `date`

### Data analysis
To be figured out.

### Visualization
Also to be figured out.

## Getting started
Create the Conda environment
```sh
conda env create --file=environment.yml
```

Activate the new environment
```sh
conda activate arxiv-heatmaps
```

Install the additional dependency for logarithmic heatmaps using `plotly`
```sh
pip3 install git+https://github.com/SengerM/plotly_utils
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