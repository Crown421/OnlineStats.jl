```@raw html
<div style="width:100%; height:150px;border-width:1px;border-style:solid;padding-top:25px;
        border-color:#000;border-radius:10px;text-align:center;background-color:#B3D8FF;
        color:#F000">
    <h3 style="color:white">Star us on GitHub!</h3>
    <a class="github-button" href="https://github.com/joshday/OnlineStats.jl" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star joshday/OnlineStats.jl on GitHub" style="margin:auto">Star</a>
    <script async defer src="https://buttons.github.io/buttons.js"></script>
</div>
```

# Welcome!

**OnlineStats** does statistics and data visualization for big/streaming data via [**online algorithms**](https://en.wikipedia.org/wiki/Online_algorithm).  Each algorithm **processes data one observation at a time** and **all algorithms use O(1) memory**.

## Getting Started

```
import Pkg
Pkg.add("OnlineStats")
```

## Basics

### Every stat is `<: OnlineStat{T}`

(where `T` is the type of a single observation)

```@repl index
using OnlineStats
m = Mean()
supertype(Mean)
```

### Stats can be updated

!!! note
    `fit!` can be used to update the stat with a **single observation** or **multiple observations**: 
    ```
    fit!(stat::OnlineStat{T}, y::S)
    ``` 
    will iterate through `y` and `fit!` each element if `T != S`.

```@repl index
y = randn(100);
fit!(m, y)
```

### Stats can be merged

```@repl index
y2 = randn(100);
m2 = fit!(Mean(), y2)
merge!(m, m2)
```

### Stats have a value

```@repl index
value(m)
```

## Collections of Stats

```@raw html
<img src="https://user-images.githubusercontent.com/8075494/57342826-bf088c00-710e-11e9-9ac0-f3c1e5aa7a7d.png" style="width:400px">
```

```@setup collections
using OnlineStats
```

### `Series`
A [`Series`](@ref) tracks stats that should be applied to the **same** data stream.

```@example collections
y = rand(1000)
s = Series(Mean(), Variance())
fit!(s, y)
```


### `FTSeries`

An [`FTSeries`](@ref) tracks stats that should be applied to the **same** data stream, but filters and transforms (hence `FT`) the input data before it is sent to its stats.

```@example collections
s = FTSeries(Mean(), Variance(); filter = x->true, transform = abs)
fit!(s, -y)
```


### `Group`

A [`Group`](@ref) tracks stats that should be applied to **different** data streams.

```@example collections
g = Group(Mean(), CountMap(Bool))
itr = zip(randn(100), rand(Bool, 100))
fit!(g, itr)
```

## Additional Resources

- [OnlineStats Demos](https://github.com/joshday/OnlineStatsDemos)
