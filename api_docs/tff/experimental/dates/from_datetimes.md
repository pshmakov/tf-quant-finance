<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.from_datetimes" />
<meta itemprop="path" content="Stable" />
</div>

# tff.experimental.dates.from_datetimes

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/date_tensor.py">View source</a>



Creates DateTensor from a sequence of Python datetime objects.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.experimental.dates.daycounts.dt.from_datetimes`</p>
</p>
</section>

```python
tff.experimental.dates.from_datetimes(
    datetimes
)
```



<!-- Placeholder for "Used in" -->


#### Args:


* <b>`datetimes`</b>: Sequence of Python datetime objects.


#### Returns:

DateTensor object.


## Example
'''python
import datetime

dates = [datetime.date(2015, 4, 15), datetime.date(2017, 12, 30)]
date_tensor = from_datetimes(dates)
'''