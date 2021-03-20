---
title: Approximating Pi
date: 2021-03-14 22:58:12
tags: -C Programming
---

## HAPPY PI DAY!

As it's pi day, and the weekend, I have spent some time thinking about pi (and pie). Being able to calculate pi to ridiculous levels is important, don't get me wrong, but not always useful. I would argue knowing a good approximation is probably more useful than an accurate value. When you are in need of an accurate value you are likely using a calculator which has it's own stored value. Therefore, any time you the maths is not important enough to get a calculator is when it's useful to have a good approximation of pi.

So far I have used the generally vague expression of "a good approximation". So maybe I should define that, "a good approximation" is one that finds a balance between memorability (it's not useful if you can't remember it) and accuracy. 3 is a very memorable approximation it is short, simple, and the butt of many engineering jokes, but its not the most accurate. Perhaps the quintisential appproximation of pi is 22/7 (I've even heard it called the exact value), it's memorable and accurate enough for napkin maths.

22/7 is a time tested approximations. However, I wanted a better approximation, so I created a program that would run through every rational number between 0 and 4 and check how accurate they are to pi.

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <math.h>

#define comparison ((double)22/(double)7)
#define threshold (fabs((comparison - M_PI)/M_PI)*100)

bool aboveThreshold(double value);

int main()
{
    printf("Threshold is: %f%%", threshold);

    for(int denominator = 0; ;denominator++)
    {
        for(int numorator = 0; numorator <= 4*denominator; numorator++)
        {
            double value = (double)numorator/(double)denominator;
            if(aboveThreshold(value) == true)
            {
                double ifit = fabs((value - M_PI)/M_PI)*100;
                printf("approximation %i/%i has %f%% error \n", numorator, denominator, ifit);
            }
        }
    }
}

bool aboveThreshold(double value)
{
    if(fabs((value - M_PI)/M_PI)*100 < threshold)
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

I should say that I wrote this code in five minutes, and I know it is not particularly well written. The positive is that it works, it can find increasdingly accurate values. However, there are a few issues:
- There are no comments.
- The #defines are sloppily made. They should be in all caps to distinguish them from variables, the casts look ugly, and calling functions in the define statement is lasy.
- I used float format specifiers instead of doubles in the print statements.
- This method assumes the knowledge that pi is less than 4. Although, not unreasonable it is an avoidable assumption.
- The method to calculate percentage errore is used three times, instead of being a function.
- The aboveThreshold function is used once, and requires the <stdbool.h> header to be included.
- **It's inefficient**, it checks more values than necessary and has no way of narrowing it's search as it finds more accurate approximations.

Needless to say I was not overwhelmingly pleased with the program had written. Spending a little bit longer to address the issues I had identified, I revised my program.

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

#define TARGET M_PI

int main(int argc, char **argv)
{
    double approx = 0;
    double approx_error = 100;

    //Luc @ https://stackoverflow.com/questions/9748393/how-can-i-get-argv-as-int/38669018
    int limit = strtol(argv[1], NULL, 10);
    int quantity = 0;

    //for every denominator check values close to the current approximation
    for (int den = 1;; den++)
    {
        for (int num = (approx - approx_error) * den; num <= (approx + approx_error) * den; num++)
        {
            double value = (double)num / (double)den;
            double error = fabs(((value - TARGET) / TARGET) * 100);

            //if the the value is a better approximation replace the current approximation with it
            if (error < approx_error)
            {
                printf("Approximation %9i / %9i has percentage error %.15lf%%\n", num, den, error);
                approx = value;
                approx_error = error;
                quantity++;
                if (quantity == limit)
                {
                    return 0;
                }
            }
        }
    }
}
```

In this program I used the same general method, but made it more efficient by implementing the changes I suggested. For this version I made some additional quality of life changes. I defined a target value instead of using M_PI, M_PI is defined in <math.h> but by defining a target I make the program flexible to look for any irrational number. I also modified it to read in a value, this value is used to limit the number of approximations the program finds.

My program found some good approximations of pi:
```
Approximation         3 /         1 has percentage error 4.507034144862795%
Approximation        13 /         4 has percentage error 3.450713009731973%
Approximation        16 /         5 has percentage error 1.859163578813025%
Approximation        19 /         6 has percentage error 0.798130624867045%
Approximation        22 /         7 has percentage error 0.040249943477070%
Approximation       179 /        57 has percentage error 0.039526970353452%
Approximation       201 /        64 has percentage error 0.030801370403238%
Approximation       223 /        71 has percentage error 0.023796311288284%
Approximation       245 /        78 has percentage error 0.018048570476005%
Approximation       267 /        85 has percentage error 0.013247516385752%
Approximation       289 /        92 has percentage error 0.009177057483145%
Approximation       311 /        99 has percentage error 0.005682219031411%
Approximation       333 /       106 has percentage error 0.002648963016705%
Approximation       355 /       113 has percentage error 0.000008491367877%
Approximation     52163 /     16604 has percentage error 0.000008473831161%
```

I believe 355/113 is the most accurate approximation that can be easily used. I also had my program search for approximations of e and sqrt(2):

```
Approximation         3 /         1 has percentage error 10.363832351432702%
Approximation         5 /         2 has percentage error 8.030139707139414%
Approximation         8 /         3 has percentage error 1.898815687615381%
Approximation        11 /         4 has percentage error 1.166846322146644%
Approximation        19 /         7 has percentage error 0.147008824894217%
Approximation        49 /        18 has percentage error 0.144958985559308%
Approximation        68 /        25 has percentage error 0.063207998632324%
Approximation        87 /        32 has percentage error 0.017223068485887%
Approximation       106 /        39 has percentage error 0.012254450838744%
Approximation       193 /        71 has percentage error 0.001031191673761%
Approximation       685 /       252 has percentage error 0.001024919667455%
Approximation       878 /       323 has percentage error 0.000572957112583%
```
```
Approximation         1 /         1 has percentage error 29.289321881345252%
Approximation         3 /         2 has percentage error 6.066017177982121%
Approximation         4 /         3 has percentage error 5.719095841793675%
Approximation         7 /         5 has percentage error 1.005050633883360%
Approximation        17 /        12 has percentage error 0.173460668094231%
Approximation        24 /        17 has percentage error 0.173160303075644%
Approximation        41 /        29 has percentage error 0.029730935695016%
Approximation        99 /        70 has percentage error 0.005101910668863%
Approximation       140 /        99 has percentage error 0.005101650387225%
Approximation       239 /       169 has percentage error 0.000875323322583%
Approximation       577 /       408 has percentage error 0.000150182509294%
Approximation       816 /       577 has percentage error 0.000150182283750%
```