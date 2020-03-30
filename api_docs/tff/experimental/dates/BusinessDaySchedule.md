<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.BusinessDaySchedule" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="dates"/>
</div>

# tff.experimental.dates.BusinessDaySchedule

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/schedules.py">View source</a>



Generates schedules containing every business day in a period.

```python
tff.experimental.dates.BusinessDaySchedule()
```



<!-- Placeholder for "Used in" -->


#### Attributes:

* <b>`end_date`</b>
* <b>`generate_backwards`</b>
* <b>`holiday_calendar`</b>
* <b>`start_date`</b>


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




