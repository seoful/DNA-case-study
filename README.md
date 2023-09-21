# Finding the origin of replication of virus using statistical methods
The purpose of the following case study is analyzing the genome of the human cytomegalovirus (CMV).

In researching the virusis, one of the main things that is important for scientists is finding the origin of replication (the place in the DNA that stores information on how to replicate creating new cells). The DNA can represented as a string of 4 letters that define the sequence of 4 types of nucleotides. It was found that some viruses from the family of CMV has the origin of replication in the largest DNA palyndrome and others have the one in the location where there are a lot of smaller palyndromes.

As the data input we have an already found locations of startings of all palyndromes that have the length greater or equal than 10.

## Reading data
From here we can see that in area around (90000, 95000) and (194000, 196000) we have big clusters of palindromes. This can be our places of interest to find the origin of replication. Also, we can notice a smaller but quite significant cluster around 75500.

![](https://hackmd.io/_uploads/HJE0w2Y1p.png)

## Locations
Here I generate 5 random samples of 296 numbers from uniform distribution to compare them with the CMV palindrome palindromes.

I generated only 5 random samples because it is hard to visually compare things on a lot of graphs. And I did not use only one sample to have some difference and not get some deviated result just by chance. We always can rerun generation function and see plots with other generated data. We cannot just run the generator e.g. 1000 times and average the spacings because due to a uniform distribution which we take samples from the spacings tend to form just a straight line as well as counts and locations will become uniform.


![](https://hackmd.io/_uploads/S1XGO3Yk6.png)


![](https://hackmd.io/_uploads/BJCGu2KJa.png)

### Observations
We can see that there is no much difference between CMV and random sample points locations and I cannot make inferences from these graphical representations only.

## Spaces
Let us analyze the spacings between the palyndromes from the input data and random scatter ones.

```convolve_size``` variable is responsible for the numbers of neighboring spacings we sum. If it is set to 1, it is the same as simply calculate spacings on their own


![](https://hackmd.io/_uploads/Skid52K1T.png)



### Observations
We cannot see significant tendency in spacings above, as both random samples and CMV has sharp peaks, falls, some more uniform "mountains". Though, random samples dont tend to show us such long low platos as CMV. It means that CMV has several segments with quite big sets of palindromes sitting closely to each other. See (90, 94 - [75622 - 76043 DNAs nucleotides]),(111, 120 - [91637 - 92859]), (251, 255 - [195032 - 195221]), (223, 226 - [173863 - 174185]) segments of the orange plots.

## Counts
Counts
Here, we will divide the DNA into parts with equal length and count how many palyndromes we have in each part. Then, run tests to see if the CMV distribution is got by chance.

![](https://hackmd.io/_uploads/BJfqOhF16.png)


Before we proceed splitting and running tests, we should tune the number of samples so that we balance between undetecting deviations due to low variance (big segments) and dividing clusters into different groups (small segments).

I stopped at dividing our DNA into 59 slices of equal length, and on the graph above we can see that major clusters (that we obsereved earlier) of palyndromes are not splitted into neighboring groups.

To test if the CMV dataset with such clusters was just a matter of chance or not, we will proceed a chi-square test where the null hypothesis claims that our CMV slices counts (the number of palyndromes in each cluster) are taken from Poisson distribution with the parameter lambda = DNA_length / number_of_slices.

We take a Poisson distribution, since taking random samples from uniform distribution (as was done earlier) leads to the Poisson distribution of counts of observations in slices.

```statistic=91.88513513513514, pvalue=0.0030479467913747038```

Here, we can see that the p-value is really small (p-value < 0.005). So, we have a strong reason to reject the null hypothesis and agree that such CMV DNA palyndromes clusters were not a matter of chance and may be significant places to start searching for the origin of replication.

```
alpha = 0.05 : 0.053599999999999995
alpha = 0.01 : 0.0082
alpha = 0.005 : 0.005600000000000001
```

If we run the same procedure on random samples from uniform distribution, we get at average ~ 5% of samples that reject the null hypothesis on alpha = 0.05, ~ 1.4% for alpha = 0.01 and ~ 0.7% for alpha = 0.005 that is explained by type 1 error. (Numbers may chage by different runs)

## Conclusion
The case study showed that the locations of palyndromes discovered in CMV DNA are not there by chance (by chi-square test) and differ from random scatters taken from uniform distribution. For the biologists to find an origin of replication I would recommend start by picking a high density palindromes areas which are [91637 - 92859], [195032 - 195221], [173863 - 174185], [75622 - 76043] ordered by decreasing of count of observations found in a region. If researching these regions give no result, one may continue search in regions that have above average density of palindromes shown on histogram in "Counts" section.

