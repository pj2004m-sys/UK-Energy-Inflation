# How much did the 2021–23 energy price shock drive UK inflation?

## Question
UK inflation hit 11.1% in October 2022, and most of the coverage at the time blamed energy. I wanted to check that with the data: how much of the 2021–23 rise in inflation came from energy, and how long did wholesale gas price rises take to reach household bills (the Ofgem price cap) and CPI?

## Data
Monthly, January 2015 to September 2026.
- **ONS** MM23 consumer price inflation: headline CPI (D7G7), core CPI (DKO8), CPI electricity, gas & other fuels (04.5) and basket weights
- **Ofgem** energy price cap (typical dual-fuel bill, `data/ofgem_price_cap.csv`)
- **FRED**: European natural gas (PNGASEUUSDM), Brent crude (DCOILBRENTEU), USD/GBP (DEXUSUK), HICP 04.5 for France and Germany
- **Bank of England**: Bank Rate (IUDBEDR)
- **Our World in Data** (Ember / Energy Institute): electricity generation by source

## Method
1. Put every series on a monthly (month-start) calendar, converted wholesale gas to pence per therm at monthly exchange rates, and forward-filled the price cap.
2. Estimated pass-through lags by correlating 3-month changes in log prices at lags of 0–12 months (gas → cap, bill → energy CPI).
3. Fitted a 12-month distributed-lag regression of CPI energy prices on wholesale gas to measure the size of the pass-through.
4. Split headline inflation into core, the non-core gap and an estimate of energy's contribution (weight × rate).
5. Calculated the Energy Price Guarantee saving per household and a first-round estimate of CPI without the guarantee.
6. Compared the timing of Bank Rate rises with CPI, and compared the UK with France and Germany.

## Key findings
1. **There's a delay.** Wholesale gas price changes took about 6 months to show up in the Ofgem price cap. Once bills changed, inflation moved the same month. A sustained 10% rise in wholesale gas pushed household energy prices up by about 2% within a year.
2. **Energy was about a third of the peak.** Household energy added around 3.5 percentage points to inflation in October 2022, when it hit 11.1%. Without the £2,500 Energy Price Guarantee, I estimate inflation would have peaked at about 14%, and a typical household saved around £900.
3. **The Bank of England was slow to react.** Inflation went above the 2% target in May 2021, seven months before the first rate rise in December 2021. In real terms (Bank Rate minus inflation), interest rates fell to about −8.8%.

![Timeline](charts/01_timeline.png)

![CPI with and without the Energy Price Guarantee](charts/05_epg_counterfactual.png)

## Limitations
Correlation isn't causation: other shocks (post-Covid demand, supply chains, a weaker pound) overlapped with the energy shock. The counterfactual is first-round only, and Ofgem's typical-household consumption changed in 2020 and 2023.

## Run it
Install the packages with `pip install -r requirements.txt`, then open `uk_energy_inflation.ipynb` in Jupyter and run all the cells. The notebook uses the copies of the data saved in `data/`, and downloads fresh copies for anything that's missing.
