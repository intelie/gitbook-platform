# Creating a pipe

This article shows how to implement the `Swinging Door` dynamic compression algorithm as a `pipe` on top of Pipes, using Intelie Live API.

```java
@Export("@compress.swingingDoor")
@Help(usage = "@compress.swingingDoor <number k> [<maxDeviation>] [<maxOutputPeriod>] [by <object...>]",
        description = "Compresses the input using the swinging door algorithm")
public class SwingingDoorPipe implements Pipe {
    private static final long serialVersionUID = 1L;
    private static final long BEST_DAY_TO_ESTIMATE_THINGS = 32503427999999L;

    private final GroupBy group;
    private final Double maxDeviation;
    private final Double maxOutputPeriod;
    private final Scalar<Double> expr;
    private final Metadata metadata;
    private final RowFields fields;
    private final Property<Double> timestamp;

    public SwingingDoorPipe(ArgQueue queue) throws PipeException {
        this.group = queue.groupBy();
        this.expr = queue.scalar(Type.NUMBER).getSafe();
        this.maxDeviation = queue.constantValue(Type.NUMBER)
                .getSafe(Q -> 1000d);
        this.maxOutputPeriod = queue.constantValue(Type.NUMBER)
                .getSafe(Q -> {
                    TimeSpan span = TimeSpan.parse(Q.compiler().expression().compileToQueue("@@fullspan").constantValue(Type.STRING).get());
                    return (span.end(BEST_DAY_TO_ESTIMATE_THINGS) - span.start(BEST_DAY_TO_ESTIMATE_THINGS)) / 1024d;
                });
        this.metadata = queue.context().metadata().resetWeights(expr.weight() * (group.isEmpty() ? 1 : Expression.GROUP_MEM_PENALTY));
        this.timestamp = queue.context().timestamp();
        this.fields = this.metadata.getRowFields();
    }

    //the following three methods are only used when running pipes
    //in the distributed mode, otherwise it does not matter what they return 
    @Override
    public boolean split() {
        return false;
    }

    @Override
    public Pipe mapper() {
        return null;
    }

    @Override
    public Pipe reducer() {
        return null;
    }

    @Override
    public Metadata metadata() {
        return metadata;
    }

    @Override
    public PipeInstance newInstance(final Sink listener) {
        return new SwingingDoorPipe.MyInstance(listener);
    }

    @Override
    public PropertyVisitor visit(Scope parent, PropertyVisitor visitor) {
        return visitor;
    }

    private class MyInstance implements PipeInstance, Serializable {
        private static final long serialVersionUID = 1L;

        private final GroupBy.State<SwingingDoorCompression> state;

        public MyInstance(Sink listener) {
            this.state = group.newState(key
                    -> new SwingingDoorCompression(listener, fields, maxDeviation, maxOutputPeriod));
        }

        public void flow(@Nullable Object obj) {
            if (obj == null)
                return;

            Double x = timestamp.eval(null, obj);
            if (x == null || Double.isNaN(x))
                return;

            SwingingDoorCompression compress = state.get(null, obj);

            Double y = expr.eval(null, obj);

            if (y != null && !Double.isNaN(y))
                compress.flow(x, y, obj);
        }

        @Override
        public void flowMany(Iterable iterable) {
            flowManyDefault(this, iterable);
        }

        @Override
        public void turnOn(SchedulerContext context) {
        }

        @Override
        public void destroy() {
            this.destroy(true);
        }

        @Override
        public void destroy(boolean flushTimers) {
        }

        @Override
        public void advanceTo(long timestamp) {
        }

        private void flowManyDefault(PipeInstance handle, Iterable it) {
            if (it == null) return;
            for (Object o : it)
                handle.flow(o);
        }
    }
}
```

And the compression implementation below.

```java
public class SwingingDoorCompression {

    @NotNull
    private final Sink listener;
    @NotNull
    private final RowFields fields;
    @NotNull
    private final Double maxDeviation;
    @NotNull
    private final Double maxOutputPeriod;

    @NotNull
    private double[] snapshot = new double[2]; // most up to date value that has passed exception
    private boolean dirtySnapshot = true;

    @NotNull
    private double[] archive = new double[2];
    private boolean dirtyArchive = true;

    @NotNull
    private double[] slope = new double[2];
    private boolean dirtySlope = true;

    @Nullable
    private Object acrhiveObj = null;
    @Nullable
    private Object snapshotObj = null;

    @Nullable
    private Object lastFlowed = null;

    public SwingingDoorCompression(Sink listener, RowFields fields, @NotNull Double maxDeviation, @NotNull Double maxOutputPeriod) {
        this.listener = listener;
        this.fields = fields;
        this.maxDeviation = maxDeviation;
        this.maxOutputPeriod = maxOutputPeriod;
    }

    public void flow(@NotNull Double x, @NotNull Double y, Object obj) {
        if (dirtyArchive) {
            setArchive(x, y, obj);
            flow(obj);
            return;
        }

        if (dirtySnapshot || dirtySlope) {
            setSnapshot(x, y, obj);
            setSlopes(x, y);
            return;
        }

        if (inSlope(x, y) && (x.longValue() - maxOutputPeriod) < archive[0]) {
            setSnapshot(x, y, obj);
            setSlopes(x, y);
            return;
        }

        setArchive(snapshot[0], snapshot[1], snapshotObj);

        dirtySnapshot = true;

        if (acrhiveObj != null && acrhiveObj != lastFlowed)
            flow(acrhiveObj);

        flow(obj);
    }

    private void flow(@NotNull Object obj) {
        lastFlowed = obj;
        if (fields != null) {
            if (obj instanceof Row) {
                listener.onEvent(fields, new ArrayRowList((Row) obj));
            }
        } else {
            listener.onRaw(ArrayRawEvent.fromArray(obj));
        }
    }

    @Nullable
    public double[] getSlope() {
        return slope;
    }

    private void setSlopes(@NotNull Double x, @NotNull Double y) {
        dirtySlope = false;
        slope[0] = calculateSlope(x, y + maxDeviation);
        slope[1] = calculateSlope(x, y - maxDeviation);
    }

    private void setArchive(@NotNull Double x, @NotNull Double y, Object obj) {
        dirtyArchive = false;
        archive[0] = x;
        archive[1] = y;
        acrhiveObj = obj;
    }

    private void setSnapshot(@NotNull Double x, @NotNull Double y, Object obj) {
        dirtySnapshot = false;
        snapshot[0] = x;
        snapshot[1] = y;
        snapshotObj = obj;
    }

    private boolean inSlope(@NotNull Double x, @NotNull Double y) {
        return ((slope[0] * x) >= y && (slope[1] * x) <= y);
    }

    @NotNull
    private Double calculateSlope(@NotNull Double x, @NotNull Double y) {
        return (y - archive[1]) / (x - archive[0]);
    }
}
```

The pipe can be exported using Live API, as follows.

```java
live.pipes().addConstructor(SwingingDoorPipe.class);
```

The function can then be used in Pipes.

```java
@compress.swingingDoor <number k> [<maxDeviation>] [<maxOutputPeriod>] [by <object...>]
```
