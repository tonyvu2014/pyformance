# PyFormance

    pip install pyformance

A Python port of the core portion of a [Java Metrics library by Coda Hale](http://metrics.codahale.com/), with inspiration by [YUNOMI - Y U NO MEASURE IT?](https://github.com/richzeng/yunomi)

PyFormance is a toolset for performance measurement and statistics, with a signaling mechanism that allows to issue events in cases of unexpected behavior

## Core Features

### Counter
Simple interface to increment and decrement a value. For example, this can be used to measure the total number of jobs sent to the queue, as well as the pending (not yet complete) number of jobs in the queue. Simply increment the counter when an operation starts and decrement it when it completes.

### Meter
Measures the rate of events over time. Useful to track how often a certain portion of your application gets requests so you can set resources accordingly. Tracks the mean rate (the overall rate since the meter was reset) and the rate statistically significant regarding only events that have happened in the last 1, 5, and 15 minutes (Exponentally weighted moving average).

### Histogram
Measures the statistical distribution of values in a data stream. Keeps track of minimum, maximum, mean, standard deviatoin, etc. It also measures median, 75th, 90th, 95th, 98th, 99th, and 99.9th percentiles. An example use case would be for looking at the number of daily logins for 99 percent of your days, ignoring outliers.

### Timer
A useful combination of the Meter and the Histogram letting you measure the rate that a portion of code is called and a distribution of the duration of an operation. You can see, for example, how often your code hits the database and how long those operations tend to take.


## Examples
### Decorators
The simplest and easiest way to use the PyFormance library.
##### Counter
You can use the 'count_calls' decorator to count the number of times a function is called.

    >>> from pyformance import counter, count_calls
    >>> @count_calls
    ... def test():
    ...     pass
    ... 
    >>> for i in xrange(10):
    ...     test()
    ... 
    >>> print counter("test_calls").get_count()
    10

##### Timer
You can use the 'time_calls' decorator to time the execution of a function and get distributtion data from it.

    >>> import time
    >>> from pyformance import timer, time_calls
    >>> @time_calls
    ... def test():
    ...     time.sleep(0.1)
    ... 
    >>> for i in xrange(10):
    ...     test()
    ... 
    >>> print timer("test_calls").get_mean()
    0.100820207596

### With statement
You can also use a timer using the with statement
##### Timer

    >>> import time
    >>> from pyformance import timer
    >>> with timer("test").time():
    ...    time.sleep(0.1)
    >>> print timer("test_calls").get_mean()
    0.10114598274230957