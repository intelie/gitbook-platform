# Histogram

A histogram is an approximate data visualization of the distribution of a numeric variable. The variable is cut into several bins, and the number of observations per bin is represented by the height of the bar. Histograms are used to study the distribution of one or a few variables. Checking the distribution of your variables one by one is probably the first task you should do when you get a new dataset.

{% hint style="info" %}
This functionality is provided by the **live-exploratory-viz** plugin. It is available on the [marketplace](https://marketplace.intelie.com/artifact/plugin-live-exploratory-viz/).
{% endhint %}

Is this plugin, there is an aggregation function for generate a histogram: `histogram(x, min, max, count)`, where the inputs are:

1. `x` - the numerical sequence to be binned
2. `min` - the minimum value to be binned
3. `max` - the maximum value to be binned
4. `count` - the number of bins

The histogram function returns an object with the following structure:

* There is an array of objects called bins, where each bin is an object, containing: &#x20;
  * bin number
  * minimum bin value
  * maximum bin value
  * count of values ​​existing in the bin.

And, outside the bins array, we have the following variables:

* totalCount: total count of values
* belowMinCount:  values ​​below the minimum existing in the input
* aboveMaxCount: values ​​above the maximum existing in the input
* invalidCount: count of invalid values

Example (return values will change according to the input):

```
HISTOGRAM_X_0_5_1_5_3:
{
  "bins": [
    {
      "bin": 0,
      "min": 0.5,
      "max": 0.8333333333333333,
      "count": 69
    },
    {
      "bin": 1,
      "min": 0.8333333333333333,
      "max": 1.1666666666666665,
      "count": 1291
    },
    {
      "bin": 2,
      "min": 1.1666666666666667,
      "max": 1.5,
      "count": 80
    }
  ],
  "totalCount": 1440,
  "belowMinCount": 0,
  "aboveMaxCount": 0,
  "invalidCount": 0
}
```

In this plugin, there is also a aggregation function for recommending the number of bins, called binscalculator, where the input is a numerical sequence. For aggregation function binscalculator, we used three different formulas, to recommend the number of bins to be used:

1.  Scott's rule:

    _Freedman, David, and Persi Diaconis. "On the histogram as a density estimator: L 2 theory." Zeitschrift für Wahrscheinlichkeitstheorie und verwandte Gebiete 57.4 (1981): 453-476._
2. Sturges' rule:\
   _Sturges, Herbert A. "The choice of a class interval." Journal of the American statistical association 21.153 (1926): 65-66._
3.  Freedman–Diaconis rule:

    _Scott, David W. "On optimal and data-based histograms." Biometrika 66.3 (1979): 605-610._

There is also a histogram chart type available on "query based charts", more specifically on "pipes charts''.

#### Functional Tutorial

*   We created the aggregation `binsCalculator` to suggest the number of bins to be used to generate the histogram. The input must be a numerical sequence.

    `=> normrandom(1, 0.1) as x every min` \
    `=> binsCalculator(x) over last 24 hours`

![binsCalculator return](<../../.gitbook/assets/image (23).png>)

*   As output, binsCalculator will return three calculated values. The first value is the result of the Freedman-Diaconis’ formula, the second value is the result of the Sturges’ formula, and the third is the result of the Scott’ formula.

    __
* After the result of the suggestion of the number of bins, we can now move to the main aggregation, which generates the pipes widget, called "histogram".  The input must be a numerical sequence, a minimum value, a maximum value and a bins quantity.\
  `=> normrandom(1, 0.1) as x every min` \
  `=> histogram(x, 0.5, 1.5, 18) over last 24 hours`

![Histogram with 18 bins](<../../.gitbook/assets/image (30).png>)

`=> normrandom(1, 0.1) as x every min` \
`=> histogram(x, 0.5, 1.5, 10) over last 24 hours`

![Histogram with 10 bins](<../../.gitbook/assets/image (123).png>)

`=> normrandom(1, 0.1) as x every min` \
`=> histogram(x, 0.5, 1.5, 14) over last 24 hours`

![Histogram with 14 bins](<../../.gitbook/assets/image (7).png>)

* As mentioned before, the histogram chart type can handle a query that outputs a seq of bin object:\
  `=> normrandom(1, 0.1) as x every min` \
  `=> histogram(x, 0.5, 1.5, 3) over last 24 hours` \
  `=> _->bins`

![Histogram is generated normally only with the bins object](<../../.gitbook/assets/image (44).png>)

*   Or just the array with min and count of the bins:

    `=> normrandom(1, 0.1) as x every min`\
    `=> histogram(x, 0.5, 1.5, 3) over last 24 hours`\
    `=> _->bins |> newmap('min', _->min, 'count', _->count)`

![Histogram is generated normally just using the array with min and count](<../../.gitbook/assets/image (157).png>)
