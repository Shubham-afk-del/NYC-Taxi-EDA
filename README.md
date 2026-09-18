# EDA: optimising NYC taxis

Exploratory analysis of New York City's 2023 yellow cab trip records, to find when and where
demand actually is. From the IIIT Bangalore Data Science programme.

A single month of those records is 3,014,248 trips across 22 columns, which does not fit
comfortably in memory on a laptop. I sampled across all twelve months of 2023 rather than
taking one month whole, so seasonal variation survives, and worked with 489,542 trips.

## What the data shows

**Tipping is about when and how, not how far.** Tip percentages are highest between 10pm and
2am, and card payments tip substantially more than cash. It points at driver earnings rather
than company revenue, and the action it implies is free: a driver choosing between an early and
a late shift has a reason to take the late one.

**Demand has a shape a dispatcher can use.** Mornings load from the outer residential zones
into Midtown and the Financial District, evenings reverse it, late Friday and Saturday nights
concentrate in the nightlife zones, and 3am to 6am is airport traffic and shift workers.

**Fare per mile is not flat.** Across vendors and three distance bands (under 2 miles, 2 to 5,
over 5), short trips earn far more per mile, which is what a fixed base fare spread over more
miles produces, and why long airport runs are less attractive than they look.

## Method

Sampling, then cleaning, which took longer than the analysis: dropped unusable columns,
investigated negative fares by rate code before removing them (a negative fare is usually a
refund, not an error), filtered zero and negative fares, tips, totals and distances, bounded
trip duration to 1 to 180 minutes, distance to under 50 miles and passenger count to 1 to 6,
then joined trips to the city's taxi zone shapefile through `LocationID` so pickups became
places rather than integers.

## Corrections, September 2026

**Three figures in the submitted notebook were plotted from random numbers.**

```python
zones['trips'] = np.random.randint(0, 100, len(zones))             # "Number of Trips per Zone"
'Speed': np.random.uniform(20, 60, num_routes * num_time_periods)  # route speeds by time of day
```

Both came from the starter template as scaffolding for the plotting calls and were never
replaced with real counts. A later cell divided those random integers by the sampling fraction
to "scale up to the full population", scaling up noise. The charts rendered and looked
plausible.

Those cells are disabled in `EDA_Optimising_NYC_Taxis.ipynb` and every conclusion that rested
on them is withdrawn in place, with the reason stated: the routing and congestion
recommendations are gone, and the zone findings that remain come from `PULocationID` and
`DOLocationID` value counts, which are real.

Two other things fixed. The conclusions were written inside triple-quoted strings in code
cells, so they never rendered as text; they are markdown now. And the pricing recommendations
are relabelled as proposals, because every percentage in them was a suggestion with no revenue
estimate behind it.

One contradiction is left unresolved rather than guessed at: two conclusion cells disagreed
about peak hours, one saying 1pm to 9pm and the other 7am to 9am and 5pm to 7pm. Settling it
needs a re-run against the data.

The version submitted for assessment is unchanged, in the zip archive in this repository.

## Files

- `EDA_Optimising_NYC_Taxis.ipynb` - the corrected notebook, renders on GitHub
- `EDA_Assg_NYC_Taxi_Starter_Shubham_Kumar_Singh.zip` - the submitted version and report

Python, Pandas, NumPy, Matplotlib, Seaborn, GeoPandas.
