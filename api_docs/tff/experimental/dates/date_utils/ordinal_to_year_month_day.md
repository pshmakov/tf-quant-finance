<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.date_utils.ordinal_to_year_month_day" />
<meta itemprop="path" content="Stable" />
</div>

# tff.experimental.dates.date_utils.ordinal_to_year_month_day

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/date_utils.py">View source</a>



Calculates years, months and dates Tensor given ordinals Tensor.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.experimental.dates.daycounts.dt.date_utils.ordinal_to_year_month_day`, `tff.experimental.dates.daycounts.du.ordinal_to_year_month_day`</p>
</p>
</section>

```python
tff.experimental.dates.date_utils.ordinal_to_year_month_day(
    ordinals
)
```



<!-- Placeholder for "Used in" -->


#### Args:


* <b>`ordinals`</b>: Tensor of int32 type. Each element is number of days since 1 Jan
 0001. 1 Jan 0001 has `ordinal = 1`.


#### Returns:

Tuple (years, months, days), each element is an int32 Tensor of the same
shape as `ordinals`. `months` and `days` are one-based.
