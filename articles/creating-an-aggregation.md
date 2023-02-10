# Creating an aggregation

This article shows an example of creating a function to calculate the harmonic mean of a sequence in INTELIE Pipes.

## The aggregation

To create the aggregation, the interface `SimpleAggregation` should be implemented.

The implementation contains the state and the intermediate representation of the aggregation.

The method marked with `@Export` is exposed as a Pipes function. Its body updates the state of the aggregation.

The `eval()` method provides the result of the calculation from its state.

The `merge()` method allows combining many states into one. This is used mainly by aggregation windows, to combine partial results produced inside it, but it can also be used to run Pipes' aggregations in a distributed system. Note that this means the aggregation should ideally be an associative operation.&#x20;

```java
import net.intelie.pipes.Export;
import net.intelie.pipes.Help;
import net.intelie.pipes.SimpleAggregation;

public class HarmonicMean implements SimpleAggregation.Full<HarmonicMean, Double> {
    private long count;
    private double inverseSum;
    
    @Help(key = "aggregation-harmonicMean") // key is used for standard library functions
    @Export("harmonicMean")
    public void harmonicMean(double x) {
        count++;
        inverseSum += 1 / x;
    }
    
    @Override
    public Double eval() {
        if (count == 0) return 0.0;
        return count / inverseSum;
    }
    
    @Override
    public void clear() {
        count = 0;
        inverseSum = 0;
    }
    
    @Override
    public void merge(HarmonicMean tree) {
        count += tree.count;
        inverseSum += tree.inverseSum;
    }
    
    @Override
    public void unmerge(HarmonicMean tree) {
        count -= tree.count;
        inverseSum -= tree.inverseSum;
    }
}
```

{% hint style="info" %}
The `unmerge` method, defined by the `SimpleAggregation.Full` class, can be implemented as an optimization for sliding windows intermediate representation calculation, if the aggregation function is invertible. Otherwise, Pipes uses a unoptimized 2-stack-based algorithm to calculate sliding windows, which may use more resources.
{% endhint %}

## Testing

Pipes provides some tools that help unit testing. An easy way is to validate the results after some state flips.

```java
import net.intelie.pipes.Aggregation;
import net.intelie.pipes.State;
import net.intelie.pipes.Tree;
import net.intelie.pipes.support.Assertions;
import net.intelie.pipes.support.EqualsWithDelta;
import net.intelie.pipes.support.TestArgs;
import org.junit.Test;
import java.util.Collections;

public class HarmonicMeanTest {
    @Test
    public void testAggregateTwoValues() throws Exception {
        Aggregation<Double> aggregation = TestArgs.makeSimple("harmonicMean", HarmonicMean.class, "x#");
        
        State state = aggregation.newState(0);
        state.yield(null, Collections.singletonMap("x", 1));
        state.yield(null, Collections.singletonMap("x", 3));
        state.yield(null, Collections.singletonMap("x", 8));
        
        Tree tree1 = state.flip();
        state.yield(null, Collections.singletonMap("x", 2));
        state.yield(null, Collections.singletonMap("x", 5));
        state.yield(null, Collections.singletonMap("x", 13));
        
        Tree tree2 = state.flip();
        
        Assertions.assertMerge(aggregation, tree1, tree2,
                new EqualsWithDelta(0.0, 1e-6),
                new EqualsWithDelta(3 / (1 + 1 / 3.0 + 1 / 8.0), 1e-6),
                new EqualsWithDelta(3 / (1 / 2.0 + 1 / 5.0 + 1 / 13.0), 1e-6),
                new EqualsWithDelta(2.6842558072842, 1e-6)
        );
    }
}
```

