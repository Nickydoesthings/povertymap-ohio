# PovertyMap Ohio

This is a county-level exploration of poverty throughout Ohio's 88 counties. Pulling from multiple public data sources, it combines a variety of factors into a single composite measure of poverty, and then produces an interactive visualization.

![Map Preview](media/interactiveMap.gif)

## What it does

Many discussions of poverty rely on a single number: the federal poverty rate - but this number only indicates the percentage of people whose income fails to meet a certain threshold. It's not a bad metric for poverty, but it doesn't necessarily tell the whole story. This project seeks to expand our understanding of the distribution of poverty by combining multiple data sources, so as to better understand how the geography of economic hardship shifts depending on what you're measuring. It combines poverty rate, unemployment, food insecurity, and severe housing cost burdens.

The data used:

| Measure | Source | Where it comes from | What it captures |
|---|---|---|---|
| Poverty rate | American Community Survey Census 5-Year | US Census Bureau | What percent of population is below official income-based poverty thresholds |
| Unemployment rate | Local Area Unemployment Statistics | Bureau of Labor Statistics | What percent of the eligible workforce are unemployed |
| Food insecurity | County Health Rankings - Food Insecurity | Univ. of Wisconsin Population Health Institute | What percent of households lack reliable access to enough food |
| Severe housing cost burden | County Health Rankings - Housing Cost Burden | Univ. of Wisconsin Population Health Institute | What percent of households spend over 50% of monthly income on housing costs |


## Exploring different weights

We can choose to prioritize each measurement differently to explore how shifting our perspective alters the picture. The notebook comes pre-loaded with the following viewpoints (and you can easily define your own):
* **Balanced (default)**, *an attempt to get the 'full picture'* : 40% weight to poverty rate, 20% weight to everything else.
* **Income focused**, *a stronger emphasis on below-poverty income levels* : 70% weight to poverty rate, 10% weight to everything else.
* **Material hardship**, *a stronger emphasis on the inability to get food and shelter* : 40% weight to food insecurity and housing burden, 10% weight to everything else.
* **Labor market**, *a stronger emphasis on unemployment* : 70% weight on unemployment, 10% weight to everything else.

As discussed below in the **Limitations** section, switching between these different weights doesn't change the picture as much as you might expect. That's notable because it partly undercuts the premise of this feature in the first place. The key point is that these particular measures are correlated (at least in Ohio), and some of that is by nature of the data sources selected: food insecurity is modeled party from poverty and unemployment. That gives us a clear next step for future iterations of PovertyMap Ohio: add sources with genuinely independent provenance so that agreement between measures means something deeper rather than being an artifact of the data itself.

## The structure of PovertyMap Ohio

The main part of the project exists in a Jupyter Notebook - an interactive environment for executing code. Most of the notebook deals with getting the data into a useable format and visualizing it. Here's how everything is set up:

* Section 1 — Setup & Configuration
* Section 2 — Data Acquisition : Get the individual data components, taking it from public sources.
* Section 3 — Clean & Combine : Identify the data we need and strip what we don't, then combine everything together into a single table.
* Section 4 — Composite Index : Define initial weights and see how they affect measurements of poverty.
* Section 5 — Visualize: Put it all together in an interactive map of Ohio's counties, with selections for different weightings.

## Limitations

There are a few things worth knowing about to understand what this visualization can and can't tell you. Some of these are methodological choices, but others are simply a reality of the available data.

* **The 4 measures of poverty explored in PovertyMap Ohio are not independent.** This is the most significant one. Carrying out a [Spearman rank correlation](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient) shows strong-to-very-strong correlation between different weighting presets. Put plainly: the weighting feature moves the picture less than you might expect because the underlying measurements are strongly collinear. Whether this correlation holds across additional metrics or geographies is an open question.
* **Food insecurity is a calculated metric,** and is partly modeled from other metrics (namely, poverty rate and unemployment). This reinforces the point above.
* **There is no account of uncertainty in the visualization.** Some measures of poverty, like the ACS 5-year estimates for county-level poverty rate, carry a sampling error that may be quite significant for small counties. A stronger analysis might decline to consider differences between counties whose intervals overlap.
* **Data sources don't share a common time span.** For instance, ACS covers 2019-2023, while the BLS numbers reflect 2024. These gaps are small enough that I don't expect them to be hugely consequential, but anything spanning 2020 might carry some distortion or noise from COVID and the related economic effects.
* **The measures share a unit but not a denominator.** Each is a percentage, but of different things: percentage of population (ACS), percentage of labor force (BLS), or percentage of households (County Health Rankings) meeting certain criteria. Rescaling puts them on a common range, but it does *not* mean that a percentage point in one metric equals a percentage point in another.
* **Min-max normalization makes the index local to Ohio**. A score of 100 simply means "the worst poverty in Ohio" while 0 means "the least poverty in Ohio", and everything else is placed relative to these two endpoints. This upside is that it's straightforward and legible. The downside is that it's sensitive to outliers, and that it lacks comparability across states without redefining the bounds.
* **No accounting for differences in population.** Each county is treated the same, regardless of the number of people living there (which ranges significantly between about ten thousand to over a million). This is fine for a county-specific map, but you should *not* read the map as answering the question, "where do most poor Ohioans live?"


## Want to run it yourself?

Open up a command line and run the following:

````bash
git clone https://github.com/Nickydoesthings/povertymap-ohio.git
cd povertymap-ohio
pip install -r requirements.txt
jupyter notebook PovertyMap_Ohio.ipynb
````

## Tech stack

Python | pandas | GeoPandas | matplotlib | Plotly | Jupyter
