<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.math.gradients" />
<meta itemprop="path" content="Stable" />
</div>

# tff.math.gradients

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/gradient.py">View source</a>



Computes the gradients of `f` wrt to `*xs`.

```python
tff.math.gradients(
    f, xs, output_gradients=None, use_gradient_tape=False, name=None
)
```



<!-- Placeholder for "Used in" -->


#### Args:


* <b>`f`</b>: Python `callable` to be differentiated.
* <b>`xs`</b>: Python list of parameters of `f` for which to differentiate. (Can also
  be single `Tensor`.)
* <b>`output_gradients`</b>: A `Tensor` or list of `Tensor`s the same size as the
  result `ys = f(*xs)` and holding the gradients computed for each `y` in
  `ys`. This argument is forwarded to the underlying gradient implementation
  (i.e., either the `grad_ys` argument of `tf.gradients` or the
  `output_gradients` argument of `tf.GradientTape.gradient`).
  Default value: `None` which maps to a ones-like `Tensor` of `ys`.
* <b>`use_gradient_tape`</b>: Python `bool` indicating that `tf.GradientTape` should be
  used regardless of `tf.executing_eagerly()` status.
  Default value: `False`.
* <b>`name`</b>: Python `str` name prefixed to ops created by this function.
  Default value: `None` (i.e., `'gradients'`).


#### Returns:

A `Tensor` with the gradient of `y` wrt each of `xs`.
