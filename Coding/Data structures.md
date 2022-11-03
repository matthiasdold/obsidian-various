## List vs numpy
Here a collection of questions and test which I tried out to understand when to use what was better for me

#### Interesting results for accessing with a mask
#list_vs_numpy
```python
In [29]: import numpy as np
    ...: 
    ...: 
    ...: def test1(bf, msk):
    ...:     return [[e for e, m in zip(elm, msk) if m] for elm in bf]
    ...: 
    ...: def test2(bf, msk):
    ...:     return np.asarray(bf)[:, msk]
    ...: 
    ...: bf = np.random.randn(40000, 10).tolist()
    ...: 
    ...: msk = np.random.randn(10) > 10

In [30]: %timeit test1(bf, msk)
14.5 ms ± 6.28 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [31]: %timeit test2(bf, msk)
14.9 ms ± 19.1 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

In [32]: 
    ...: bf = np.random.randn(100000, 10).tolist()
    ...: 
    ...: msk = np.random.randn(10) > 10

In [33]: %timeit test1(bf, msk)
36.8 ms ± 48 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [34]: %timeit test2(bf, msk)
37.9 ms ± 63.4 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)

In [35]: bf = np.random.randn(1000000, 10).tolist()

In [36]: msk = np.random.randn(10) > 10

In [37]: %timeit test1(bf, msk)
373 ms ± 648 µs per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [38]: %timeit test2(bf, msk)
385 ms ± 1.21 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [39]: bf = np.random.randn(100, 10).tolist()

In [40]: msk = np.random.randn(10) > 10

In [41]: %timeit test1(bf, msk)
%timeit test2(bf, msk)
36.2 µs ± 125 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

In [42]: %timeit test2(bf, msk)
37.7 µs ± 55.2 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
```

##### However having it in numpy before accessing is as expected
```python
In [49]: bf = np.random.randn(1000000, 10).tolist()
    ...: 
    ...: msk = np.random.randn(10) > 10

In [50]: def test3(bf, msk):
    ...:     return bf[:, msk]

In [51]: %timeit test1(bf, msk)
375 ms ± 2.25 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [52]: %timeit test2(bf, msk)
384 ms ± 545 µs per loop (mean ± std. dev. of 7 runs, 1 loop each)

In [53]: bfa = np.asarray(bf)
    ...: %timeit test3(bfa, msk)
    ...: 
685 ns ± 2.7 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)
```