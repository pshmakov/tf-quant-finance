<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.experimental.dates.from_ordinals" />
<meta itemprop="path" content="Stable" />
</div>

# tff.experimental.dates.from_ordinals

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/experimental/dates/date_tensor.py">View source</a>



Creates DateTensor from tensors of ordinals.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.experimental.dates.daycounts.dt.from_ordinals`</p>
</p>
</section>

```python
tff.experimental.dates.from_ordinals(
    ordinals, validate=True
)
```



<!-- Placeholder for "Used in" -->


#### Args:


* <b>`ordinals`</b>: Tensor of type int32. Each value is number of days since 1 Jan
  0001. 1 Jan 0001 has `ordinal=1`.
* <b>`validate`</b>: Whether to validate the dates.


#### Returns:

DateTensor object.


## Example
```python

ordinals = tf.constant([
  735703,  # 2015-4-12
  736693   # 2017-12-30
], dtype=tf.int32)

date_tensor = from_ordinals(ordinals)
```