# Daily COVID-19 by Country (capstone.py, v4)

Genie Tang, 140.628.81 Data Science for Public Health in Python, Fall 2026

A Streamlit web app that shows daily or cumulative COVID-19 cases and deaths for countries the user selects, using the Johns Hopkins CSSE global time series:
https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series

## Run it

```
pip install -r requirements.txt
streamlit run capstone.py
```

The app opens at http://localhost:8501 and reads the data straight from GitHub, so no data files are needed. Keep the `.streamlit/config.toml` (theme) and `static/world_110m.json` (map outlines) files next to `capstone.py`.

## Use it

1. Sidebar: pick a country under **Add a country** and press **Add** (or type into **Selected countries**; click the x on a tag to remove it).
2. Choose **Cases** or **Deaths**, and **Daily new** or **Cumulative**.
3. Optional: **Per 100,000 people**, **7-day average**, handling of **negative daily values**, **One panel per country**, **Log scale**, and the **Date range** slider. Up to six countries; each keeps one color.
4. Tabs: **Chart** (headline tiles plus the chart), **Map** (bubbles sized by rate), **World map** (every country shaded by rate or count, selected countries outlined, ranked table), **Spatial statistics** (global Moran's I and LISA cluster map with a Moran scatterplot, PySAL), **Alerts** (EARS C2 aberration detection per selected country), **Summary table** (totals, rates, deaths per 100 cases), **How to use**, and **About the data** (cleaning steps and limits). The CSV button saves the plotted data.

## Data preparation

- Province and territory rows are summed to national totals.
- Daily new counts are the day-to-day change in the running total.
- Negative daily values are reporting corrections; hidden by default, with options to set to zero or keep.
- Population and map positions come from the CSSE `UID_ISO_FIPS_LookUp_Table.csv`.
- Data end on March 9, 2023, when CSSE stopped collecting.
- Spatial statistics use log10 rates per 100,000, k-nearest-neighbour weights on great-circle distance, 999 permutations. Alerts use EARS C2 (7-day baseline, 2-day lag, 3 SD) on daily counts with corrections set to zero.
