# THIS IS A BETA VERSION - ONLY USE FOR TESTING PURPOSES

# perfect-t-test

Correctly comparing two groups is remarkably challenging. When performing a *t*-test researchers rarely manage to follow all recommendations that statisticians have made over the years. For example, it is still uncommon to see confidence intervals around effect sizes, robust statistics when the normality assumption is violated, or Welch's t-test instead of Student's t-test when sample sizes are unequal, or Bayesian statistics. Researchers should test whether their data is normally distributed, but it is unclear how often people actually do this, or whether they use the best tests for normality. Even though it has been repeatedly suggested that researchers should improve the way they report statistics, progress is slow.

Here, I propose a way to speed up the widespread adoption of the state-of-the-art by providing researchers with a single script that will perform and report all statistical analyses. The script performs all the recommended tests, creates all required plots of the data, and writes the results section, including the minimally required interpretation of the statistical results. As an example, I have created two R scripts that will allow researchers to compare dependent and independent groups. Automated analyses might strike readers as a bad idea because it facilitates mindless statistics. Having performed statistics mindlessly myself for most of my professional career, I doubt having had access to this script would have reduced my level of understanding. If anything, reading an automatically generated results section of your own data that includes statistics you were not accustomed to calculate or report is likely to make you think more about the usefulness of these statistics, not less. In the end, whether or not automated analyses improve or deteriorate researchers' understanding of statistics is an empirical question. However, the ultimate goal of providing this script is not educational - it is simply to get researchers to report the analyses they should, and do so efficiently.

To use this script, **first [install R](http://cran.r-project.org/)**. Then, **[install R studio](http://www.rstudio.com/products/rstudio/download/)**. Then, **[download the R markdown files](https://github.com/Lakens/perfect-t-test/archive/master.zip)**. In the .zip file, there is one script for dependent t-tests, and one for independent t-tests. If you use the script for the first time, also **[install the latest version of JAGS](http://sourceforge.net/projects/mcmc-jags/files/JAGS/)**. Open either of the two .Rmd files, depending on the test you want to perform. You need to make sure you have all the required packages installed. This can be accomplished by running the following code:

```{r}
install.packages(c("MASS", "akima", "robustbase", "cobs", "robust", "mgcv", "scatterplot3d", "quantreg", "rrcov", "lars", "pwr", "trimcluster", "multicore", "mc2d", "psych", "Rfit","MBESS", "BayesFactor", "PoweR", "ggplot2", "reshape2", "plyr", "devtools","rmarkdown","gmodels", "HLMdiag", "car", "gridExtra", "bootES", "BEST", "plyr", "reshape2"))

install.packages("devtools")
library("devtools")
install_github("mrxiaohe/WRScpp")
install_github("nicebread/WRS", subdir="pkg")
```

You can run this by removing the # in front to the R markdown script, selecting the lines of code, and clicking 'Run' button (or simply press CTRL+ENTER). Don't forget to put back the # in front of these lines of code, or R will install all packages each time your run the code. Alternatively, **copy paste the code above to a new R script and run it**.

Now you are ready to define the necessary variables to adapt the code to the data you have collected. Your data should be read into R. The current script is set up to read inyour data as a **tab-delimited text file** (see the two demo-files in the .zip file). In SPSS, you can select 'save as' and select 'save as type: Tab deleminited (*.dat)'.

**Make sure your datafile is in the same folder as the R markdown (.Rmd) file**. If .Rmd files are not associated with R Studio, open the R markdown file by right-clicking, and choosing 'Open With' - and browse to the rstudio.exe.

For an independent *t*-test **two** columns are required, one with the independent variable, and one with the dependent variable. For a dependent *t*-test, **three** columns are required: one with the participant identifier, one with the first measurement, and one with the second measurement. **All columns should have headers** - labels that specify the name of each column. See the demo data files in the .zip file (the independent *t*-test data is fake data, the dependent *t*-test data are results from a Stroop experiment).

You need to specify the headers in your text file in the R markdown file (as xlabel (e.g., Congruent), ylabel (e.g., Incongruent), and subject (e.g., PPNR)). R is case-sensitive (so "Congruent" is not the same as "congruent"). In addition, specify what you have measured and manipulated - these names are used in the running text output the Markdown file creates, and as axis titles of the figures that are created. You can change the alpha level, confidence interval, and sidedness of the test if requires, as well as the Bayesian prior. Finally, some calculations require time (the Bayesian HDI and robust effect size estimates) which you can choose not to calculate if you are in a hurry. In the independent *t*-test you can change the number of bootstraps for the robust effect size, which might be necessary in large samples (try it if you get an error).

If all this is done, simply hit the **Knit Word** button to create an automatically generated word document that reports the results. 

#The Output

The output you get contains a boxplot, four tests for normality, histograms with kernel density plots, and Q-Q plots to check the normality of the data. It provides a test for equality of variances, but for independent *t*-tests Welch's *t*-test is report by default, regardless of whether the equality of variances assumption is violated. Frequentist statistics return the means, standard deviations, mean difference with confidence intervals, Welch's *t*-test with an effect size and the 95% confidence interval, as well as the common language effect size. Bayesian statistics are provided, including the Bayes Factor and the Highest Density Interval. Robust statistics are provided, using 20% trimmed means. Finally, a number of figures is provided that illustrate the 95% confidence intervals (within and between for dependent *t*-tests) as well as violin plots that show how the data was distributed.