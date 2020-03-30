<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.math.random.stateless_random_shuffle" />
<meta itemprop="path" content="Stable" />
</div>

# tff.math.random.stateless_random_shuffle

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/random_ops/stateless.py">View source</a>



Produces stateless random shuffle of the 1st dimension of an input Tensor.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.models.euler_sampling.random.stateless_random_shuffle`, `tff.models.euler_sampling.utils.random.stateless_random_shuffle`, `tff.models.heston_model.generic_ito_process.euler_sampling.random.stateless_random_shuffle`, `tff.models.heston_model.generic_ito_process.euler_sampling.utils.random.stateless_random_shuffle`, `tff.models.heston_model.random.stateless_random_shuffle`, `tff.models.heston_model.utils.random.stateless_random_shuffle`</p>
</p>
</section>

```python
tff.math.random.stateless_random_shuffle(
    input_tensor, seed, name=None
)
```



<!-- Placeholder for "Used in" -->

This is a stateless version of `tf.random_shuffle`. If run twice with the same
seed, produces the same result.

Example
```python
identity_shuffle = tf.range(100)
random_shuffle = stateless_random_shuffle(identity_shuffle, seed=(42, 2))
```

#### Args:


* <b>`input_tensor`</b>: float32, float64, int32 or int64 1-D Tensor.
* <b>`seed`</b>: int32 or int64 Tensor of shape [2].
* <b>`name`</b>: Python `str` name prefixed to ops created by this function.


#### Returns:

A Tensor of the same shape and dtype as `input_tensor`.
