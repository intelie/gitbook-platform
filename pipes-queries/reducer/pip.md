# PIP

The Perceptually Important Points algorithm attempts to keep high visual similarity between a chart plotted after the compression and the hypothetically chart plotted with all the original points.

To achieve this goal, the algorithm divides the series into segments of data. Then, it chooses the perceptually important points, that is, the points that will be output, based on a distance algorithm between each point and its neighbors.

To calculate the distance, it is possible to use different algorithms. We choose the vertical distance algorithm because it is faster than the Euclidean distance without compromising the visual results.

The downside of this algorithm is that it tends to choose points in noisy areas of the chart, then it does not lead to the best fit on charts that display variables that change too fast.
