tags: #performance #overhead 
## If statements
It seems like an if based on a float vs int costs about `600ns`

```python
In [18]: def test1():
    ...:     a = np.random.randn(1)
    ...:     if a > 0:
    ...:         pass
    ...:     a += 1
    ...: 

In [19]: def test2():
    ...:     a = np.random.randn(1)
    ...:     a += 1
    ...: 

In [20]: %timeit test1()

1.97 µs ± 4.09 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)

In [21]: %timeit test2()
1.33 µs ± 1.45 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)
```

Int comparisons are much cheaper on the other hand
```python
In [28]: def test2():
    ...:     a = np.random.randint(1)
    ...:     if a > 0:
    ...:         pass
    ...:     a += 1
    ...: 

In [29]: %timeit test2()
1.08 µs ± 1.76 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [30]: def test1():
    ...:     a = np.random.randint(1)
    ...:     a += 1
    ...: 

In [31]: %timeit test2()
1.08 µs ± 0.803 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)
```

## Logging
![[Python logging#Overhead]]

## Boolean
### Numpy boolean arrays
The numpy built in function for any is much faster than boolean logic or the builtin `any`

```python
In [3]: a = np.ones(1000) == 0

In [4]: def test1():
   ...:     return sum(a)
   ...: 

In [5]: def test2():
   ...:     return a.any()
   ...: 

In [6]: def test3():
   ...:     return any(a)
   ...: 

In [7]: %timeit test1()
50.9 µs ± 231 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

In [8]: %timeit test2()
629 ns ± 0.521 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [9]: %timeit test3()
6.16 µs ± 3.36 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)
```

Another very important aspect: Never work with a plain sum over a numpy boolean array:
#To_ask_StackOverflow #TODO_blog_post
```python
In [2]: a = np.ones(1000) == 0

In [8]: a[10:100] = True

In [9]: %timeit np.ones(a.shape)[a].shape[0]
1.36 µs ± 16.7 ns per loop (mean ± std. dev. of 7 runs, 1,00
0,000 loops each)

In [10]: %timeit np.sum(a)
1.87 µs ± 2.9 ns per loop (mean ± std. dev. of 7 runs, 1,000
,000 loops each)

In [11]: %timeit sum(a)
50.7 µs ± 694 ns per loop (mean ± std. dev. of 7 runs, 10,00
0 loops each)


# ------ Smaller array
In [12]: a = np.ones(100) == 0

In [13]: a[10:100] = True

In [14]: %timeit sum(a)
5.86 µs ± 71.9 ns per loop (mean ± std. dev. of 7 runs, 100,
000 loops each)

In [15]: %timeit np.sum(a)
1.42 µs ± 9.01 ns per loop (mean ± std. dev. of 7 runs, 1,00
0,000 loops each)

In [16]: %timeit np.ones(a.shape)[a].shape[0]
968 ns ± 2.6 ns per loop (mean ± std. dev. of 7 runs, 1,000,
000 loops each)

In [17]: %timeit a.sum()
769 ns ± 12.7 ns per loop (mean ± std. dev. of 7 runs, 1,000
,000 loops each)

In [18]: %timeit np.count_nonzero(a)
227 ns ± 0.223 ns per loop (mean ± std. dev. of 7 runs, 1,00
0,000 loops each)
```

