## Last Sex: Sex Ratio By Last Name

Using data from the [Indian Electoral Rolls](https://github.com/in-rolls/electoral_rolls) (parsed data [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/MUEGDT)), we estimate sex ratio by last name to further pin down the kinds of people among whom sex selective abortion, etc. is particularly common. 

We ignore the long tail of last names because we cannot learn sex ratios reliably where we don't have a large enough n. Instead, we focus on last names shared by at least 50,000 people. 

We split people's names into first name and last name and then group by last name and birth_year (2017 - age) and produce four columns per state: `last_name, n_male, n_female, birth_year`. We then combine data from across all the states for which we have the data. And again group by sum by `last_name` to produce `last_name, n_male, n_female, birth_year`.

We then aggregate over `birth_year` to get firstly an all up number by `last_name.` We then filter to last names where sum of `n_male` and `n_female` is over 50,000. We then estimate the sex ratio (n_male/n_female) per last name. We then reverse sort by sex ratio.

The final data is [here](data/). 

The top 50 most imbalanced ratios are plotted below.

We then group by birth_year (collapsing over last_name) to see how sex_ratios have evolved over time.

We then estimate within_last_name regression and estimate which last_names have seen sex ratios worsen over time.


