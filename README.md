# String Matching

A small Python toolkit for robust string comparison and matching, aimed at names, person records, and other textual identifiers. It is built around three functions — string normalization, pairwise name distance, and optimal list-to-list matching — packaged in a single, self-contained [Jupyter notebook](string-matching.ipynb).

Typical use cases include:

* Data cleaning and preprocessing
* Record linkage / entity resolution
* Fuzzy matching of names across datasets
* Deduplication tasks
* General natural language preprocessing

## Functions

### `str_normalize`

Normalizes a string for comparison: lowercases it, strips or replaces specified characters, removes diacritics (e.g. "José" → "jose"), and collapses extra whitespace.

### `name_distance`

Computes a distance score between two names using a chosen metric — [Levenshtein](https://en.wikipedia.org/wiki/Levenshtein_distance), Damerau-Levenshtein, Hamming, or [Jaro](https://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance) — with optional normalization and optimal word reordering (via the [Hungarian algorithm](https://en.wikipedia.org/wiki/Hungarian_algorithm)), so that names differing in word order or formatting (e.g. `"Robert Kennedy Jr."` vs. `"Kennedy, Robert Jr"`) can still be compared meaningfully.

### `match_strings`

Matches elements between two lists of strings by computing pairwise distances and applying optimal assignment, with an optional distance `cutoff` to exclude poor matches. Returns either matched string pairs or index pairs, and is the main entry point for linking two sources of names or identifiers.

## Example

The notebook walks through matching two lists of city names that overlap only partially and differ in spelling, spacing, and punctuation:

```python
str_matches = match_strings(
    strlist1, strlist2,
    metric = "levenshtein", cutoff = 5,
    normalize = True, reorder = True,
    remove = ["-", ".", "'"]
)
```

which correctly pairs entries such as `"Lakewood Heights"` with `"Lakewood Hts."` and the `"North Elmsfield"`-style variants across the two lists, while leaving unmatched cities unpaired.

## Requirements

The notebook relies on [NumPy](https://numpy.org/), [pandas](https://pandas.pydata.org/), [SciPy](https://scipy.org/) (for `linear_sum_assignment`), and [textdistance](https://github.com/life4/textdistance). All but `textdistance` are preinstalled in [Google Colab](https://colab.research.google.com/); the notebook installs `textdistance` itself when needed.

## Running the notebook

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dimitrisilva/string-matching/blob/main/string-matching.ipynb)

The notebook can also be run locally in Jupyter, provided the packages above are installed.
