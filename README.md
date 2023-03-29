# arxiv-track-author-preferences 

 
## Summary 
#### Motivation
What do scientific authorship trends tells us about the importance of different scientific fields? 
Can this information help a young scientist choose their field?
- Measuring authors who *switch between fields* tells us how scientists rank the relative importance of fields. 
More importantly, this measures preference *for people who have seen both fields*, and then made the monumental decision to switch or stay.
This is a much stronger signal of value than just measuring author number over time, which includes first time authors. 

#### Methods
Here we take the dataset of all ArXiv papers ever published, and extract the way that authors transition between scientific fields over time. 

#### Metrics
- The author count within each field in each year
- The author transition counts between fields during each year
- The author transition rate between fields for a given year.
 
> Example data for the major fields on ArXiv.
![Alt text](plots/major_categories/Fields_summary.png?raw=true "Optional") 

## Files

1. arxiv-metadata-extraction.ipynb 
   - This file takes the raw arxiv data 
   (downloaded from https://www.kaggle.com/datasets/Cornell-University/arxiv)
   and extracts how authors publish (for details, see file). 
   Data are saved as .json files (ie. author_counts.json) for quick future analysis.
2. arxiv-metadata-plotting.ipynb 
   - This file reads in the processed .json files to display information. 
3. basic_utils.py 
   - This file contains a few helper functions, and a list of all ArXiv categories.

## How to use

### Explore how authors transition between your own ArXiv fields
If you want to play with the data (already extracted author counts/transitions over time), 
for example to see how your own field changes over time, just use arxiv-metadata-plotting.ipynb. 
This notebook should just run if you download the repository, then you can change what types of fields you look at, etc.


### Modify the base data extraction
If you want to double check the extraction of data, and exactly what quantities are being pulled
to the .json files, use arxiv-metadata-extraction.ipynb.
Doing this requires you to have a few other things downloaded and set up in various folders.


## More explanation of metrics
The main metrics are:
- The author count within each field in each year 
  - A single author is assumed to correspond to each unique name (not perfect)
  - An author who publishes in multiple categories is counted as being fractionally located in each field,
  given by 1/(the number of fields in the union of all fields they published in during that year).
- The author transition counts between fields during each year
  - For each unique author name, I take the difference between the current year author categories, and the
  previous year author categories.
  - The losses in some categories and gains in others for a specific author are counted as transitions from
  the fields with losses to the fields with gains (and these gains and losses are attributed uniformly 
  across all the gains and losses, proportionally to how much each field is lost out of the total loss, for example).
- The author transition rate between fields for a given year.
  - This is just the total transition counts, divided by the field size of the source field.
  - In other words, it is something like the probability, conditioned on being an author in a specific field, that you transition
  from that field to another field during that year.
