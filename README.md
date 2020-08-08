## Last Sex: Sex Ratio By Last Name

Using data from the [Indian Electoral Rolls](https://github.com/in-rolls/electoral_rolls) (parsed data [here](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/MUEGDT)), we estimate sex ratio by last name to pin down the kinds of people among whom sex-selective abortion and other sex-based discrimination is common. (We plan to augment the analysis with [SECC](https://github.com/in-rolls/secc) data soon.)

We ignore the long tail of last names because we cannot learn sex ratios reliably where we don't have a large enough n. Instead, we focus on last names shared by at least 50,000 people.

### Last Name

Indian names are famously complicated. To infer last names, we create rules that lead to us making fewer false positives than negatives.

Here's the algorithm we use:
1. Start with `orig_df`. Create a column called `birth_year` (2017 - age).

2. There are multiple steps to filtering to the base data frame:
* Split names by space and store each word in a separate column. Assume the last word to be the last name and store in column `last_name.` Filter out one-word names and where the last word is fewer than two characters.
* Filter out names with non-alphabetical characters. We lose XXX records as a result of that.
* Remove records of people who are recorded as being born before 1900 as we think those birth dates are unreliable. We lose XXX rows.
* Remove people of "Third Gender" because we only have 4 four rows of people recorded as third gender.
* Store the data frame as `base_df`.

3. Filter to households with a non-missing household number. Store the data frame as `last_name_hh_no_df`.

4. Among households with more than one person, filter to last words shared by two or more people. Store these names in a list called `potential_last_names`.

5. Go to the `base_df` and assign `potential_last_name` to any name that matches our list. Subset on `potential_last_name` that is shared by at least 100 households and create a list called `likely_last_name`.

6. Calculate sex-ratio by `likely_last_name` using `base_df`and filter out names where the sex ratio is over five or under .5 assuming that these ratios map to first names than last names. Store the list under `likely_last_name_sex_ratio_test.` (Here's a [list of names](data/potential_last_names_with_unlikely_sex_ratios.csv) that we filter as a result of this requirement.)

7. Go back to `base_df` and create a column called `best_guess_last_name` that matches any name in the `likely_last_name_sex_ratio_test.`

### Analysis

Group `orig_df` by `birth_year`. Filter to years with more than 10,000 records. Plot.

Filter `base_df` to names with more than 10,000 records. Group the filtered dataset by `best_guess_last_name` and produce dataset with four columns `last_name, n_male, n_female, sex_ratio` and store as [sex_ratio_by_last_name](data/sex_ratio_by_last_name.csv).

The top 50 most imbalanced ratios are plotted below.

Group `base_df`by `best_guess_last_name` and `birth_year`. Filter the dataset to names with at least 500 records in at least 25 years. Output the dataset with five columns `last_name, birth_year, n_male, n_female, sex_ratio` and store as [sex_ratio_by_last_name](data/sex_ratio_by_last_name.csv).

We plot this here.

We then estimate a linear regression: `sex_ratio ~ birth_year + best_guess_last_name + e` using this [data](data/sex_ratio_by_last_name.csv)

### Authors

Suriyan Laohaprapanon and Gaurav Sood
