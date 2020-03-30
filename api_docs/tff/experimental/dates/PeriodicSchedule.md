<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.PeriodicSchedule" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="dates"/>
</div>

# tff.experimental.dates.PeriodicSchedule

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/schedules.py">View source</a>



Defines an array of dates specified by a regular schedule.

```python
tff.experimental.dates.PeriodicSchedule()
```



<!-- Placeholder for "Used in" -->


#### Args:


* <b>`start_date`</b>: <a href="../../../tff/experimental/dates/DateTensor.md"><code>dates.DateTensor</code></a>. Defines the lower boundary of schedule. If
  `backward=True` must be broadcastable to `end_date`, otherwise has
  arbitrary shape.
* <b>`end_date`</b>: <a href="../../../tff/experimental/dates/DateTensor.md"><code>dates.DateTensor</code></a>. Defines the upper boundary of the schedule.
  If `backward=False` must be broadcastable to `start_date`, otherwise has
  arbitrary shape.
* <b>`tenor`</b>: <a href="../../../tff/experimental/dates/periods/PeriodTensor.md"><code>periods.PeriodTensor</code></a>. Defines the frequency of the schedule. Must
  be broadcastable to `start_date` if `backward=False`, and to `end_date`
  if `backward=True`.
* <b>`holiday_calendar`</b>: <a href="../../../tff/experimental/dates/HolidayCalendar.md"><code>dates.HolidayCalendar</code></a>. If `None`, the dates in the
  schedule will not be rolled to business days.
* <b>`roll_convention`</b>: BusinessDayConvention. Defines how dates in the schedule
  should be rolled to business days if they fall on holidays. Ignored if
  `holiday_calendar = None`.
  Default value: BusinessDayConvention.NONE (i.e. no rolling).
* <b>`backward`</b>: Python `bool`. Whether to build the schedule from the
  `start_date` moving forwards or from the `end_date` and moving
  backwards.

#### Attributes:

* <b>`end_date`</b>
* <b>`generate_backwards`</b>:   Returns whether the schedule is generated from the end date.
* <b>`holiday_calendar`</b>
* <b>`roll_convention`</b>
* <b>`start_date`</b>
* <b>`tenor`</b>


## Methods

<h3 id="dates"><code>dates</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/schedules.py">View source</a>

```python
dates()
```

Returns the dates as computed from the schedule as a DateTensor.

Constructs the date schedule from the supplied data. For more details see
the initializer docstring.

#### Returns:

`DateTensor` of rank one more than `start_date` or `end_date`
(depending on `backwards`), representing schedules for each element
of the input.




