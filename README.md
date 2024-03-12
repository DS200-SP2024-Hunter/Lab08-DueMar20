# Lab Assignment 08, Due on [Canvas](https://psu.instructure.com/courses/2306358/assignments/16003002?module_item_id=41285277), Mar. 20 at 11:59pm
## Examine Gender Difference in Mean Finishing Time for a 5K Road Race

**On this and all labs for the rest of the semester: When you have a choice between the [datascience package](https://www.data8.org/datascience/) and the [pandas library](https://pandas.pydata.org/docs/), you are free to use either method to complete your work.**

The main objective of today's lab is to use a dataset gleaned from publicly-available data to compare two groups using two different methods: Hypothesis tests and confidence intervals.
The data come from race results that can be scraped from the website of the Nittany Valley Running Club at [https://www.nvrun.com/](https://www.nvrun.com/).
We will assume that the runners in this race are a representative sample, some male and some female, from hypothetical populations of all recreational male 5K runners and all female 5K runners.  (You might ponder whether this is a reasonable assumption, but it's not necessary for you to comment on this question for this assignment.)


**Objective**: Compare males with females for a particular 5K race using both a randomization-based hypothesis test ([Chapter 12](https://inferentialthinking.com/chapters/12/Comparing_Two_Samples.html)) and a bootstrapped confidence interval ([Chapter 13](https://inferentialthinking.com/chapters/13/Estimation.html)).
In the dataset, each runner identified as either female or male at the time of race registration.  The cleaned dataset may be found here:
```
https://raw.githubusercontent.com/DS200-SP2024-Hunter/Lab08-DueMar20/main/FiveKResults.csv
```

**Your assignment** is as follows:

1. Load the dataset (the URL is given above) into python. Give it a sensible name that you can remember, like `fiveK`.

2. Perform an A/B test of the null hypothesis that the hypothetical average male time in this race equals the hypothetical average female time in this race, among all 5K runners. For this purpose, you can either use the `difference_of_means` function from [Section 12.1](https://inferentialthinking.com/chapters/12/1/AB_Testing.html) or you can create your own version of this function using `pandas`.

3. Plot the histogram of mean differences that you obtain by simulation under the null hypothesis of no mean difference. Compare this histogram to the value observed in the sample, calculate a p-value, and comment on what this p-value tells you in this case about whether we should reject the null hypothesis. _Write a conclusion sentence that conveys what you have discovered to an audience unfamiliar with statistical terminology._

4. Construct a confidence interval for the true mean difference (male mean minus female mean) using bootstrapping.  Just as in [Section 13.2](https://inferentialthinking.com/chapters/13/2/Bootstrap.html), it will be possible to use the `sample()` method with no arguments to accomplish the bootstrapping.  However, there is a subtle point in this case:  We should make sure that each bootstrap sample has the same number of males and females as the original, since we're finding the difference of two sample means (one female, one male) and the sample sizes are important in determining the behavior of sample means.  Therefore, if you're using `datascience` then you'll want to create two separate `Table` objects, one for females and one for males, using code such as this:
```
fiveK_Male = fiveK.where('Identifies As Female', False)
fiveK_Female = fiveK.where('Identifies As Female', True)
```
Make sure that the pdf file you turn in somehow indicates how many female and male cases there are in the dataset.

5. Rewrite the function `one_bootstrap_median` in [Section 13.2.6](https://inferentialthinking.com/chapters/13/2/Bootstrap.html). so that it selects one bootstrap sample for females, another for males, then returns the difference of the two mean finishing times.  Here is code that accomplishes this if you're using the `datascience` package:
```
def one_bootstrap_mean_difference():
    resampled_Females = fiveK_Female.sample()
    resampled_Males = fiveK_Male.sample()
    Female_mean = np.mean(resampled_Females.column('Finishing Time In Minutes'))
    Male_mean = np.mean(resampled_Males.column('Finishing Time In Minutes'))
    bootstrapped_mean_difference =  Male_mean - Female_mean
    return bootstrapped_mean_difference
 ```

6. Reproduce the analysis of [Section 13.2.7](https://inferentialthinking.com/chapters/13/2/Bootstrap.html) by creating a histogram of 5000 bootstrapped mean differences.  You will have to choose histogram bins that are appropriate for this example AND there will not be a green dot showing the position of the population parameter (since in this case we do not know the population parameter).

7. Find the 2.5 and 97.5 percentiles of your mean difference distribution, and use these values as a 95% confidence interval.  You should include a text box in your output that says "We are 95% confident that the true difference in mean 5K road race finishing times between females and males is between ----- and -----." (You will fill in the blanks.)

8.  Finally, make sure that your Jupyter notebook only includes code and text that is relevant to this assignment.  For instance, if you have been completing this assignment by editing some original code from Section 12.1 or Section 13.2, make sure to delete the material that isn't relevant before turning in your work.

When you've completed this, you should select "Print" from the File menu, then save to pdf using this option.  The pdf file that you create in this way is the file that you should upload to Canvas for grading.  We have found that if you can select the "A3" paper size from the advanced options, this seems to solve the problems that are sometimes encountered in this step.


