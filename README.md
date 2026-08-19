# Indian Flights Price Analysis

Exploratory data analysis of Indian domestic flight pricing, looking at how airline, class, stops, and route affect ticket prices - and how Air India compares to IndiGo in market presence.

## Business Questions

1. How does ticket price vary by airline, and does that ranking hold across all routes? How much does number of stops affect price?
2. How does Air India's route/market share compare to IndiGo's, especially given IndiGo's domestic dominance vs Air India's stronger international position?

## Dataset
This project uses two datasets, one for each business question.

**1. Flight Price Data** (for Q1 - airline/route price comparison)
- Source: [Goibibo Flight Data](https://www.kaggle.com/datasets/iamavyukt/goibibo-flight-data?select=goibibo_flights_data.csv) on Kaggle
- ~300,000 rows, 11 columns
- Covers flights between 6 major Indian Metro Cities (Delhi, Mumbai, Bangalore, Chennai, Kolkata, Hyderabad)
- Time period: June 26 - August 13, 2023 (~7 weeks)

**2. Flight Schedule Data** (for Q2 - Air India vs IndiGo route/market share)
- Source: [Indian Domestic Airline Dataset](https://www.kaggle.com/datasets/kabil007/indian-domestic-airline-dataset?select=Air-Clean.csv) on Kaggle
- Covers airline, origin, destination, days of week, and scheduled times
- Time period: 2018-2025

**Note on scope:** the original goal was to study how prices change month-to-month across a full year, and whether "monopoly routes" (routes with only one airline) see higher prices. The price dataset didn't support either - it only spans ~7 weeks (too short for seasonality), and every route in it is served by 6+ airlines (no true monopoly routes exist in this data, since it only covers major metro-to-metro routes where competition is heaviest). The question was adapted to focus on airline price differences and route consistency instead, which the data does support well.

## Process
- Converted `price` from text (with comma separators) to numeric
- Parsed `flight date` with explicit `dayfirst=True` to avoid silent date-parsing errors (Indian DD-MM-YYYY format was being misread)
- Dropped 2 duplicate rows and two empty placeholder columns
- Filtered to airlines still operation today: Air India, IndiGo, Spicejet, Vistara, StarAir. Excluded GO FIRST, AirAsia, and Trujet (no longer operating)
- Cleaned the `stops` column, which had layover city details merged into the same field (e.g. "1-stop via BBI"), collapsing it to just stop count
- Split all price comparisons by `class` (economy/business) after finding Air India and Vistara sell substantial business class while IndiGo, SpiceJet, and StarAir sell economy only — comparing raw averages without this split would have been misleading
- Flagged sample size alongside every airline comparison — StarAir (61 records) and SpiceJet (9,011 records) have far less data than Air India (80,892) and Vistara (127,859), so findings involving them are treated with appropriate caution

## Key Findings

**1. IndiGo is the consistent, reliable budget leader.**
Across every one of the 30 metro-to-metro routes in the dataset, IndiGo has the lowest average economy fare among airlines with meaningful sample sizes — no exceptions. SpiceJet is usually a close second where it flies (its network only covers 5 of the 6 cities), occasionally undercutting IndiGo narrowly. StarAir shows an even lower average fare (₹4,982) than IndiGo (₹5,377), but this is based on just 61 flight records versus IndiGo's 43,000+, so it's noted as a data curiosity rather than a reliable finding.

**2. Air India and Vistara are in a similar price tier, and neither consistently beats the other.**
Overall, Air India averages cheaper economy fares (₹7,387 vs ₹7,885) and cheaper business fares (₹47,838 vs ₹56,309) than Vistara. But at the route level, this flips — e.g. Vistara is cheaper than Air India on Delhi-Kolkata, while Air India is cheaper on Bangalore-Kolkata. This nuance would be lost by only looking at the overall average. Neither IndiGo, SpiceJet, nor StarAir offer business class in this dataset.

**3. More stops = meaningfully higher price, not lower.**
Non-stop is the cheapest option in both classes. Economy: non-stop ₹4,153 → 1-stop ₹7,234 (+74%) → 2+ stops ₹10,439 (+151%). Business shows the same pattern, roughly doubling to tripling from non-stop to 2+ stops.

*(Q2 findings to be added)*

## Tools Used

Python, pandas, matplotlib/seaborn, Jupyter Notebook

## How to Run

\`\`\`bash
git clone [repo-url]
cd indian-flight-price-analysis
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
\`\`\`