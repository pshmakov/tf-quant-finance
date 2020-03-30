<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.date_utils.days_in_leap_years_between" />
<meta itemprop="path" content="Stable" />
</div>

# tff.experimental.dates.date_utils.days_in_leap_years_between

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/date_utils.py">View source</a>



Calculates number of days between two dates that fall on leap years.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.experimental.dates.daycounts.dt.date_utils.days_in_leap_years_between`, `tff.experimental.dates.daycounts.du.days_in_leap_years_between`</p>
</p>
</section>

```python
tff.experimental.dates.date_utils.days_in_leap_years_between(
    start_date, end_date
)
```



<!-- Placeholder for "Used in" -->

'start_date' is included and 'end_date' is excluded from the period.

For example, for dates '(2019, 12, 24)' and '(2024, 2, 10)' the result is
406: 366 days in 2020, 31 in Jan 2024 and 9 in Feb 2024.

If `end_date` is earlier than `start_date`, the result will be negative or
zero.

#### Args:


* <b>`start_date`</b>: DateTensor.
* <b>`end_date`</b>: DateTensor compatible with `start_date`.


#### Returns:

Tensor of type 'int32'.
