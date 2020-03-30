<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.math.random.uniform" />
<meta itemprop="path" content="Stable" />
</div>

# tff.math.random.uniform

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/random_ops/uniform.py">View source</a>



Generates draws from a uniform distribution on [0, 1).

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.models.euler_sampling.random.uniform`, `tff.models.euler_sampling.utils.random.uniform`, `tff.models.heston_model.generic_ito_process.euler_sampling.random.uniform`, `tff.models.heston_model.generic_ito_process.euler_sampling.utils.random.uniform`, `tff.models.heston_model.random.uniform`, `tff.models.heston_model.utils.random.uniform`</p>
</p>
</section>

```python
tff.math.random.uniform(
    dim, sample_shape, random_type=None, dtype=None, seed=None, name=None, **kwargs
)
```



<!-- Placeholder for "Used in" -->

Allows generating either (pseudo) random or quasi-random draws based on the
`random_type` parameter. Dimension parameter `dim` is required since for
quasi-random draws one needs to know the dimensionality of the space as
opposed to just sample shape.

## Example:

```python
sample_shape = [10]  # Generates 10 draws.

# `Tensor` of shape [10, 1]
uniform_samples = uniform(1, sample_shape)

# `Tensor` of shape [10, 5]
sobol_samples = uniform(5, sample_shape, RandomType.SOBOL)
```

#### Args:


* <b>`dim`</b>: A positive Python `int` representing each sample's `event_size.`
* <b>`sample_shape`</b>: Rank 1 `Tensor` of positive `int32`s. Should specify a valid
  shape for a `Tensor`. The shape of the samples to be drawn.
* <b>`random_type`</b>: Enum value of `RandomType`. The type of draw to generate.
  Default value: None which is mapped to <a href="../../../tff/math/random/RandomType.md#PSEUDO"><code>RandomType.PSEUDO</code></a>.
* <b>`dtype`</b>: Optional `dtype` (eithier `tf.float32` or `tf.float64`). The dtype of
  the output `Tensor`.
  Default value: `None` which maps to `tf.float32`.
* <b>`seed`</b>: Seed for the random number generator. The seed is
  only relevant if `random_type` is one of
  `[PSEUDO, STATELESS, HALTON_RANDOMIZED]`. For `PSEUDO`, the seed should
  be a Python integer. For the other two options, the seed may either be
  a Python integer or a tuple of Python integers.
* <b>`name`</b>: Python `str` name prefixed to ops created by this class.
  Default value: `None` which is mapped to the default name
    `uniform_distribution`.
* <b>`**kwargs`</b>: parameters, specific to a random type:
  (1) `skip` is an `int` 0-d `Tensor`. The number of initial points of the
  Sobol or Halton sequence to skip. Used only when `random_type` is 'SOBOL',
  'HALTON', or 'HALTON_RANDOMIZED', otherwise ignored.
  (2) `randomization_params` is an instance of
  `tff.math.random.HaltonParams` that fully describes the randomization
  behavior. Used only when `random_type` is 'HALTON_RANDOMIZED', otherwise
  ignored (see halton.sample args for more details). If this parameter is
  provided when random_type is `HALTON_RANDOMIZED`, the `seed` parameter is
  ignored.
  Default value: `None`. In this case with randomized = True, the necessary
    randomization parameters will be computed from scratch.


#### Returns:


* <b>`samples`</b>: A `Tensor` of shape `sample_shape + [dim]`. The draws
  from the uniform distribution of the requested random type.