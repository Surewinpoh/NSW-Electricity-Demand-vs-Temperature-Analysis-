# NSW Electricity Demand vs Temperature Analysis (Feb 2025 – Jan 2026)

## Overview
This project investigates the relationship between daily maximum temperature 
and electricity demand in New South Wales (NSW), Australia. Using a full year 
of data from February 2025 to January 2026, the analysis reveals a U-shaped 
relationship between temperature and demand with peak demand occurring at 
both temperature extremes.

## Data Sources
- **Electricity Demand:** Australian Energy Market Operator (AEMO) via 
  [NEMOSIS](https://github.com/UNSW-CEEM/NEMOSIS) — `DISPATCHREGIONSUM` 
  table, filtered for NSW1 region
- **Temperature:** Bureau of Meteorology (BOM) daily maximum temperature 
  recorded at Sydney Observatory Hill weather station

## Tools & Libraries
- Python 3
- pandas
- numpy
- matplotlib
- plotly
- nemosis

## Methodology
1. Pulled 5-minute interval NEM dispatch data using NEMOSIS and aggregated 
   to daily maximum demand
2. Downloaded and combined 12 months of BOM temperature CSV files
3. Merged datasets on date and calculated Pearson and Spearman correlations
4. Visualised the relationship using both a static matplotlib chart 
   (quadratic trendline) and an interactive Plotly chart (LOWESS trendline, 
   colour-coded by month)
5. Split the dataset at 25°C to analyse the heating and cooling effects 
   separately

## Key Findings

### Overall Correlation
| Method | Correlation |
|--------|------------|
| Pearson | -0.23 |
| Spearman | -0.28 |

The weak overall correlation is misleading as it does not capture the 
true nature of the relationship.

### Split Analysis at 25°C
| Side | Pearson | Spearman |
|------|---------|----------|
| Cool days (≤25°C) | -0.83 | -0.82 |
| Hot days (>25°C) | +0.67 | +0.59 |

Splitting the data at 25°C reveals two strong relationships:
- **Cool days:** As temperature drops below 25°C, demand rises strongly likely
  driven by heating load in autumn and winter
- **Hot days:** As temperature rises above 25°C, demand rises again likely
  driven by air conditioning load in summer

The opposing directions of these two effects cancel each other out in the 
overall correlation, producing a misleadingly weak result. The scatter plot 
and quadratic trendline better represent the true U-shaped relationship.

## Limitations
- Temperature data uses Sydney Observatory Hill as a proxy for NSW-wide 
  temperature. While Sydney represents approximately 65% of NSW's population 
  and is a reasonable proxy, future work could incorporate a 
  population-weighted average across multiple BOM stations for improved 
  accuracy
- Analysis covers one year of data (Feb 2025 – Jan 2026) and may not 
  capture longer-term trends or anomalous weather years
- Demand is influenced by factors beyond temperature including day of week, 
  public holidays, and humidity etc. These variables were not 
  controlled for in this analysis

## Future Work
- Incorporate population-weighted temperature across multiple BOM stations
- Add day-of-week and public holiday controls
- Build an interactive Streamlit dashboard for dynamic month-by-month 
  filtering
- Extend analysis to multiple years to identify longer-term trends

## How to Reproduce
1. Clone this repository
2. Install dependencies: `pip install pandas numpy matplotlib plotly nemosis 
   statsmodels`
3. Download BOM temperature data for Sydney Observatory Hill and place CSV 
   files in the same folder
4. Run `demand_temp_analysis.ipynb` in Jupyter or VS Code

