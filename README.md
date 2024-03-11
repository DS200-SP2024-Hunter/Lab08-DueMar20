# Lab Assignment 08, Due on [Canvas](https://psu.instructure.com/courses/2306358/assignments/16003002?module_item_id=41285277), Mar. 20 at 11:59pm
## Examine Gender Difference in Median Finishing Time for a 5K Road Race

The main objective of today's lab is to use a dataset gleaned from publicly-available data to compare two groups using two different methods: testing and confidence intervals.
The data come from race results that can be scraped from the website of the Nittany Valley Running Club at [https://www.nvrun.com/](https://www.nvrun.com/).


**Objective**: Compare males with females for a particular 5K race using both a hypothesis test (Chapter 12) and a bootstrapped confidence interval (Chapter 13).
In the dataset, each runner identified as either F for female or M for male at the time of race registration.  The cleaned dataset may be found here:
```
https://raw.githubusercontent.com/DS200-SP2024-Hunter/Lab08-DueMar20/main/FiveKResults.csv
```

## To edit below.  Idea:  Ask for both an A/B randomization test and a bootstrap confidence interval

**OLD Objective**:  Modify the code in [Section 13.2](https://inferentialthinking.com/chapters/13/2/Bootstrap.html)
so that it loads the dataset represented by the `http://personal.psu.edu/drh20/200DS/assets/data/FiveKResults.csv` 
file as a `Table` object and implement bootstrapping to obtain a confidence interval for the
population parameter (&mu;<sub>F</sub> – &mu;<sub>M</sub>), the difference between mean 5 kilometer road race finishing time for females and mean finishing time for males.
(NB:  In the original data, each runner identified as either F for female or M for male at the time of race registration.)

**Your assignment** is as follows:

1. Load the Jupyter notebook for [Section 13.2](https://inferentialthinking.com/chapters/13/2/Bootstrap.html) of the textbook from GitHub as you've done in the past. You might want to change the `path_data` object so that it points to the URL where the dataset can be found:  `http://personal.psu.edu/drh20/200DS/assets/data/`

2. Read the dataset from the `FiveKResults.csv` file as a `Table` object and give it a name like `fiveK`.  For this purpose, use the `read_table` method as in the first code block of Section 13.2.1.

3. Just as in Section 13.2.6, it will be possible to use the `sample()` method with no arguments to accomplish the bootstrapping.  However, there is a subtle point in this case:  We should make sure that each bootstrap sample has the same number of females as the original, since we're finding the difference of two sample means (one female, one male) and the sample sizes are important in determining the behavior of sample means.  Therefore, you'll want to create two separate Tables, one for females and one for males.  You can use code such as this:
```
fiveK_Male = fiveK.where('Identifies As Female', False)
fiveK_Female = fiveK.where('Identifies As Female', True)
```

4. In the pdf file you turn in, include python code that reveals how many cases there are in both the female and the male Tables.

5. Rewrite the function `one_bootstrap_median` in Section 13.2.6. so that it selects one bootstrap sample for females, another for males, then returns the difference of the two mean finishing times.  Here is code that accomplishes this:
```
def one_bootstrap_mean_difference():
    resampled_Females = fiveK_Female.sample()
    resampled_Males = fiveK_Male.sample()
    Female_mean = np.mean(resampled_Females.column('Finishing Time In Minutes'))
    Male_mean = np.mean(resampled_Males.column('Finishing Time In Minutes'))
    bootstrapped_mean_difference =  Female_mean - Male_mean
    return bootstrapped_mean_difference
 ```

6. Reproduce the analysis of Section 13.2.7 by creating a histogram of 5000 bootstrapped mean differences.  You will have to choose histogram bins that are appropriate for this example AND there will not be a green dot showing the position of the population parameter (since in this case we do not know the population parameter).

7. Find the 2.5 and 97.5 percentiles of your mean difference distribution, and use these values as a 95% confidence interval.  You should include a text box in your output that says "We are 95% confident that the true difference in mean 5K road race finishing times between females and males is between ----- and -----." (You will fill in the blanks.)

8. Discuss which population you think the statement in step 7 might be valid for.  In other words, what populuation do you think our original sample can reasonably be assumed to represent?

9. _(Optional, for an extra point):_ The original dataset covers two different races, the First Night 5K and the Arts Festival 5K.  There are 24 people who ran both of them.  Create a new dataset with these 24 people, and give a bootstrapped 95% confidence interval for the mean difference between an individual's First Night finishing time and that individual's Arts Fest finishing time.  As above, discuss which population you think this result might apply to.

10.  Finally, make sure that your Jupyter notebook only includes code and text that is relevant to this assignment.  For instance, if you have been completing this assignment by editing the original code from Section 13.2, make sure to delete the material that isn't relevant before turning in your work.

When you've completed this, you should select "Print" from the File menu, then save to pdf using this option.  The pdf file that you create in this way is the file that you should upload to Canvas for grading.  We have found that if you can select the "A3" paper size from the advanced options, this seems to solve the problems that are sometimes encountered in this step.


