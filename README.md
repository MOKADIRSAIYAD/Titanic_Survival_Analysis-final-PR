# Titanic Survival Analysis (EDA)



## About the Dataset (please read)
`titanic.csv` follows the **same structure as the Kaggle Titanic `train.csv`**: 891 passengers and the columns `PassengerId, Survived, Pclass, Name, Sex, Age, SibSp, Parch, Ticket, Fare, Cabin, Embarked`.

The file in this repository is a **generated (simulated) Titanic-style dataset**. Its group-level statistics were matched to the well-known historical figures: 891 passengers, 38.4% survival, survivors per class and gender group, the same share of missing `Age` (177), `Cabin` (687) and `Embarked` (2) values. Individual details (names, exact ages, fares, ticket numbers, cabins) are **simulated and are not real passenger records**.

To run the analysis on the **original Kaggle data**, download `train.csv` from the [Titanic dataset on Kaggle](https://www.kaggle.com/c/titanic/data), rename it to `titanic.csv` (replacing this file) and run the notebook again. No code change is needed. The numbers in the "Key Findings" section below belong to the dataset included here and will change slightly with the original data.

## Data Dictionary
| Column | Description |
|--------|-------------|
| PassengerId | Unique passenger number |
| Survived | 0 = did not survive, 1 = survived |
| Pclass | Ticket class (1 = upper, 2 = middle, 3 = lower) |
| Name | Passenger name (includes title) |
| Sex | male / female |
| Age | Age in years |
| SibSp | Siblings / spouses aboard |
| Parch | Parents / children aboard |
| Ticket | Ticket number |
| Fare | Ticket price |
| Cabin | Cabin number |
| Embarked | Port: C = Cherbourg, Q = Queenstown, S = Southampton |

## Analysis Steps
1. **Load and understand the data**: shape, data types, statistical summary
2. **Missing values and duplicates**: measure and visualize
3. **Data cleaning and feature engineering**: new columns `Title`, `FamilySize`, `IsAlone`, `FamilySizeGroup`, `AgeGroup`, `FareBand`, `Deck`, `HasCabin`
4. **Univariate analysis**: overall survival, passenger profile, age and fare distributions
5. **Survival rate by factor**: gender, class, age group, port, family size, title, fare band, deck
6. **Multivariate analysis**: class x gender, gender x age group, age/fare vs survival, correlation heatmap
7. **Insights and conclusions** calculated directly from the data
8. **Save** the cleaned dataset and all charts

## Data Cleaning Decisions
| Column | Problem | Solution |
|--------|---------|----------|
| Age | 177 missing (19.9%) | Filled with the **median age of the same Pclass and Sex group** |
| Embarked | 2 missing | Filled with the most frequent port (mode) |
| Cabin | 687 missing (77.1%) | Not imputed; converted into `Deck` ("Unknown" if missing) and `HasCabin` |
| Name | Contains title | Extracted `Title` (Mr, Mrs, Miss, Master, Rare) |
| Fare | Strongly right-skewed | Kept as is; log scale used in charts, quartile `FareBand` created |

## Key Findings (for the included dataset)
| Factor | Result |
|--------|--------|
| Overall survival | **38.4%** |
| Gender | Women **74.2%** vs men **18.9%** |
| Passenger class | 1st **63.0%**, 2nd **47.3%**, 3rd **24.2%** |
| Class + gender | 1st-class women 96.8%, 2nd-class women 92.1%; 3rd-class men only 13.5% |
| Age | Children (12 and under) 44.0% vs 37.9% for older passengers |
| Family size | Small families (2-4) 47.7%, travelling alone 33.2%, large families (5+) 11.1% |
| Fare | Highest fare quartile 58.7% vs lowest quartile 28.3% |
| Cabin | Cabin recorded 58.8% vs no cabin 32.3% |
| Port | Cherbourg 49.7%, Southampton 36.5%, Queenstown 30.4% (linked to class mix) |

**Conclusion:** gender and passenger class were the strongest factors, in line with the "women and children first" rule and the advantage of upper-class passengers. Family size, fare and cabin also show clear patterns, mostly connected with class.

## Sample Charts
| Survival by Gender | Survival by Class |
|---|---|
| ![Gender](images/05_survival_by_gender.png) | ![Class](images/06_survival_by_class.png) |

![Class and Gender](images/13_class_gender.png)

![Gender and Age](images/14_gender_age_heatmap.png)

![Correlation](images/17_correlation.png)

## How to Run
1. Install the libraries (Anaconda already includes them): `pip install -r requirements.txt`
2. Open **Jupyter Notebook** inside this folder.
3. Open `Titanic_Survival_Analysis.ipynb` and choose **Kernel > Restart & Run All**.
4. Charts are saved in `images/` and the cleaned data in `titanic_cleaned.csv`.

## Assumptions
- Missing ages are replaced by the median of the passenger's class and gender, so age-based results are approximate.
- `Cabin` is mostly missing and is not imputed; deck results use only passengers with a recorded cabin.
- Age groups: Child 0-12, Teen 13-18, Young Adult 19-35, Adult 36-60, Senior 60+.
- Family size = `SibSp + Parch + 1`; family groups: Alone (1), Small (2-4), Large (5+).
- Fare bands are quartiles (Low, Medium, High, Very High).
- Zero fares are kept (they can represent free tickets).
- Results show association, not causation; class, fare, cabin and deck overlap with each other.

## Limitations and Future Work
- A prediction model (logistic regression, random forest) could be added and its feature importance compared with these findings.
- Small groups (for example passengers aged 60+) give unstable percentages.

## Author
*(Write your name here)*
