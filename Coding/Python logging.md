I wonder what the best practice for logging in python would be. Working with `import logging` seems to offer many possibilities. What would be the best approach?

[Best resource I have read so far](https://docs.python.org/3/howto/logging.html)

Other resources mainly covered always the same, and ended with "you can use our companies tool for more workflow planning etc...".

Be aware that the call to `fileConfig()` will use ``disable_existing_loggers=True`` which will disable other root loggers before the call.

- **Interesting idea** : There is a `SockerHandler` which can send messages to TCP/IP


### Do's
- Initialise `logging.getLogger(__name__)` to get a logger for each module
- Config either in source code or init/yaml/toml file ... -> I will go for source code as the config is python specific anyways
- If you set up a logger for a library read [Configuring Logging for a Library](https://docs.python.org/3/howto/logging.html) -> trick is the use of the `NullHandler` 

### Idea for a strategy for DAREPLANE
- setup logging with a `NullHandler` first, depending on `kwargs` for the main script, overwrite this.
- If the API is initialised spawn up a file handler.

#### What it should be able of doing
- Should be well configurable also from a CLI from the main script level
- Multiple loggers would potentially write to a single log file. The file would kind of made accessible for all docker.

#### Potential read-ups
- [ ] [RotatingFileHandlers](https://docs.python.org/3/library/logging.handlers.html) as recommended [here](https://blog.sentry.io/2022/07/19/logging-in-python-a-developers-guide/) -> as the log names would not have a date with the name. So this would roll over to a new file after a threshold. Thats what I understand atm.

### Overhead
tags: #logging #overhead #performance
It seems like adding a single line of a `logger.debug()` introduces about  `60ms` overhead:

```python
In [54]: 
    ...:     def test1():
    ...:         a = 1
    ...:         return a
    ...: 
    ...:     def test2():
    ...:         a = 1
    ...:         LOGGER.debug("Did random")
    ...:         return a
    ...: 

In [55]: %timeit test2()

88.7 ns ± 0.676 ns per loop (mean ± std. dev. of 7 runs, 10,000,000 loops 
each)

In [56]: 

In [56]: %timeit test1()
23.1 ns ± 0.175 ns per loop (mean ± std. dev. of 7 runs, 10,000,000 loops 
each)
```
