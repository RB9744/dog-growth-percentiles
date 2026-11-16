# Dog growth weight percentiles by size group (8 to 104 weeks)

Weekly body weight percentiles for puppies and young dogs: the 10th, 25th, 50th, 75th and 90th percentile in kilograms, for every week of age from 8 to 104, in six size groups. 582 rows in total.

The numbers were computed by the [CritterCalc](https://crittercalc.com/puppy-growth-chart/) team from the open records behind Salt et al. 2017, the study that led to the WALTHAM puppy growth charts. They are the lines our [puppy growth chart](https://crittercalc.com/puppy-growth-chart/), [puppy weight calculator](https://crittercalc.com/puppy-weight-calculator/) and [dog size calculator](https://crittercalc.com/dog-size-calculator/) run on.

## Files

| File | What it is |
|---|---|
| `data/dog-growth-percentiles.csv` | one row per size group and week of age, 582 rows |
| `data/dog-growth-percentiles.json` | the same numbers grouped by size, with the source and licence fields |

## Columns

| Column | Meaning |
|---|---|
| `size_group` | `toy`, `small`, `medium`, `large`, `extra_large` or `giant` |
| `adult_weight_min_kg`, `adult_weight_max_kg` | expected adult weight of the group: under 6.5, 6.5 to 9, 9 to 15, 15 to 30, 30 to 40 and over 40 kg |
| `age_weeks` | age in weeks, 8 to 104 |
| `p10_kg` ... `p90_kg` | weight at that percentile, in kg |

## Quick use

Raw CSV:

```
https://raw.githubusercontent.com/RB9744/dog-growth-percentiles/main/data/dog-growth-percentiles.csv
```

Python:

```python
import csv, urllib.request
url = 'https://raw.githubusercontent.com/RB9744/dog-growth-percentiles/main/data/dog-growth-percentiles.csv'
rows = list(csv.DictReader(urllib.request.urlopen(url).read().decode('utf-8').splitlines()))
medium_26 = [r for r in rows if r['size_group'] == 'medium' and r['age_weeks'] == '26'][0]
print(medium_26['p50_kg'])   # 7.3
```

JavaScript:

```js
const r = await fetch('https://raw.githubusercontent.com/RB9744/dog-growth-percentiles/main/data/dog-growth-percentiles.json');
const data = await r.json();
const week12 = data.groups.large.weeks.find(w => w.age_weeks === 12);
// { age_weeks: 12, p10: ..., p25: ..., p50: 7.56, p75: ..., p90: ... }
```

## Median weight at a few ages (kg)

| Size group | 8 weeks | 12 weeks | 26 weeks | 52 weeks | 104 weeks |
|---|---|---|---|---|---|
| Toy | 1.23 | 1.68 | 2.81 | 3.63 | 4.08 |
| Small | 2.00 | 2.83 | 5.31 | 6.85 | 7.39 |
| Medium | 2.27 | 3.48 | 7.30 | 9.48 | 10.52 |
| Large | 4.67 | 7.56 | 17.30 | 23.71 | 25.54 |
| Extra large | 5.67 | 9.08 | 20.82 | 29.85 | 32.36 |
| Giant | 6.12 | 10.86 | 27.14 | 40.94 | 46.31 |

## Where the data comes from

The raw records are weigh-ins from Banfield Pet Hospitals in the United States, 1994 to 2013. The University of Liverpool shares them under CC BY 4.0:

- Dataset: [Growth standard charts for monitoring bodyweight in dogs of different sizes, supporting data](https://doi.org/10.17638/datacat.liverpool.ac.uk/377)
- Paper: Salt C, Morris PJ, German AJ, Wilson D, Lund EM, Cole TJ, Butterwick RF (2017). Growth standard charts for monitoring bodyweight in dogs of different sizes. PLOS ONE 12(9): e0182064. [doi:10.1371/journal.pone.0182064](https://doi.org/10.1371/journal.pone.0182064)

## How we worked it out

1. We kept only check-up or healthy-pet visits where the vet recorded a normal body condition. That left 124,316 weigh-ins from 73,694 dogs.
2. For each size group and each week of age we took the 10th, 25th, 50th, 75th and 90th percentile of the weights.
3. We smoothed each line so that it never goes down from one week to the next.

Both sexes are pooled. Breeds are disguised in the public records, so there are no single-breed lines, only size groups.

## Limits you should know

- These are descriptive percentiles from many dogs weighed once or a few times. They are not a growth standard and not a tool for diagnosis.
- They are not the WALTHAM charts. WALTHAM kept only dogs that stayed in ideal body condition up to the age of 3 and draws separate charts for males and females. On the toy and small lines our median is about 10 to 30% above the median of the printed WALTHAM charts.
- The 2017 paper gave no standard chart for dogs over 40 kg. The giant group here comes from the same records, but treat it with extra care.
- The dogs were patients of one US hospital chain. Dogs elsewhere may grow a little differently.
- For a puppy that worries you, the answer is a vet, not a table.

## Licence and credit

[CC BY 4.0](LICENSE). Please credit: "Dog growth weight percentiles, CritterCalc (https://crittercalc.com), computed from the open data of Salt et al. 2017".

Found a mistake? Open an issue or write to hello@crittercalc.com.
