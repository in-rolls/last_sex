## Last Sex: Sex Ratio By Last Name

Using data from the [Indian Electoral Rolls](https://github.com/in-rolls/electoral_rolls) (parsed data [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/MUEGDT)), we estimate sex ratio by last name to further pin down the kinds of people among whom sex selective abortion, etc. is particularly common.

We ignore the long tail of last names because we cannot learn sex ratios reliably where we don't have a large enough n. Instead, we focus on last names shared by at least 50,000 people.

We first solve name parsing. We split people's names by word 'location'---first_word, second_word, etc. Then within households with more than 1 person, for all words greater than length 1, we look for words that are shared by 2 or more people. We create a new column called potential_last_name based on that. We then go back to the original dataset (including single member households) and allocate 'potential_last_name' to any name that matches our list. We then subset on potential_last_name that is shared by at least 100 households and come up with a column that is likely_last_name.

Of the households where we have members with likely_last_name, we group by on likely last name and birth_year (2017 - age) and produce four columns per state: `last_name, n_male, n_female, birth_year`. We then combine data from across all the states for which we have the data. And again group by sum by `last_name` to produce `last_name, n_male, n_female, birth_year`.

We then aggregate over `birth_year` to get firstly an all up number by `last_name.` We then filter to last names where sum of `n_male` and `n_female` is over 50,000. We then estimate the sex ratio (n_male/n_female) per last name. We then reverse sort by sex ratio.

Sex Ratio by Year
Data
Unreliable Birth Year Data

We remove records of people who are recorded as being born before 1900 as we think those birth dates are unreliable. We hardly lose any data with this---we lost 115 of the 23.6M we started with. (We don't explicitly do it here but our filtering procedures on number of rows per last name per year removes anyone over 90.)
Removing People of "Third Gender"

Solely because of data paucity, we remove these records. There are only 4, which speaks to measurement issues.
Enough Data

We filter on years where we have more than 5,000 observations.
Result
The sex ratio had two periods where the trend was clear upward: between 1920s and 1960 and starting 1990s.

Sex Ratio by Last Name
Next, we analyze by last name.

Data
We filter the data as follows:

We remove last names with non-alphabetical characters in them as we don't think they are . We lose 63,435 records as a result of that.

We first calculate sex ratio by last_name. We filter the data where the sex ratio is under .5 and over 5 because we think those last names are likely first names. This means we lose XXX records.

Next, we filter out the long tail of last names because we cannot learn sex ratios reliably where we don't have a large enough n. Instead, we focus on last names shared by at least 25,000 people. This leaves us with XXX rows.

Next we group by year and only produce sex ratio estimates for last_names where the number of people with a particular last name for each of the years was 500 or more.

The final data is [here](data/).

The top 50 most imbalanced ratios are plotted below.

We then group by birth_year (collapsing over last_name) to see how sex_ratios have evolved over time.

We then estimate within_last_name regression and estimate which last_names have seen sex ratios worsen over time.
