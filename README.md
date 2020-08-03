## Last Sex: Sex Ratio By Last Name

Using data from the [Indian Electoral Rolls](https://github.com/in-rolls/electoral_rolls) (parsed data [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/MUEGDT)), we estimate sex ratio by last name to further pin down the kinds of people among whom sex selective abortion, etc. is particularly common.

We ignore the long tail of last names because we cannot learn sex ratios reliably where we don't have a large enough n. Instead, we focus on last names shared by at least 50,000 people.

We first solve name parsing. We split people's names by 'location'---first_word, second_word, etc. Then within households with more than 1 person, for all words greater than length 1, we look for words that are shared by 2 or more people. We create a new column called potential_last_name based on that. We then go back to the original dataset (including single member households) and allocate 'potential_last_name' to any name that matches our list. We then subset on potential_last_name that is shared by at least 100 households and come up with a column that is likely_last_name.

Of the households where we have members with likely_last_name, we group by on likely last name and birth_year (2017 - age) and produce four columns per state: `last_name, n_male, n_female, birth_year`. We then combine data from across all the states for which we have the data. And again group by sum by `last_name` to produce `last_name, n_male, n_female, birth_year`.

We then aggregate over `birth_year` to get firstly an all up number by `last_name.` We then filter to last names where sum of `n_male` and `n_female` is over 50,000. We then estimate the sex ratio (n_male/n_female) per last name. We then reverse sort by sex ratio.

The final data is [here](data/).

The top 50 most imbalanced ratios are plotted below.

We then group by birth_year (collapsing over last_name) to see how sex_ratios have evolved over time.

We then estimate within_last_name regression and estimate which last_names have seen sex ratios worsen over time.
