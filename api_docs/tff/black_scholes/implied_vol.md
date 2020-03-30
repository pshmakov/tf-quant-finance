<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.black_scholes.implied_vol" />
<meta itemprop="path" content="Stable" />
</div>

# tff.black_scholes.implied_vol

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/black_scholes/implied_vol_lib.py">View source</a>



Finds the implied volatilities of options under the Black Scholes model.

```python
tff.black_scholes.implied_vol(
    **kwargs
)
```



<!-- Placeholder for "Used in" -->

#### Examples
```python
forwards = np.array([1.0, 1.0, 1.0, 1.0])
strikes = np.array([1.0, 2.0, 1.0, 0.5])
expiries = np.array([1.0, 1.0, 1.0, 1.0])
discount_factors = np.array([1.0, 1.0, 1.0, 1.0])
option_signs = np.array([1.0, 1.0, -1.0, -1.0])
volatilities = np.array([1.0, 1.0, 1.0, 1.0])
prices = black_scholes.option_price(
    forwards,
    strikes,
    volatilities,
    expiries,
    discount_factors=discount_factors,
    is_call_options=is_call_options)
implied_vols = newton_vol.implied_vol(forwards,
                                      strikes,
                                      expiries,
                                      discount_factors,
                                      prices,
                                      option_signs)
with tf.Session() as session:
  print(session.run(implied_vols[0]))
# Expected output:
# [ 1.  1.  1.  1.]

#### Args:


* <b>`prices`</b>: A real `Tensor` of any shape. The prices of the options whose
  implied vol is to be calculated.
* <b>`strikes`</b>: A real `Tensor` of the same dtype as `prices` and a shape that
  broadcasts with `prices`. The strikes of the options.
* <b>`expiries`</b>: A real `Tensor` of the same dtype as `prices` and a shape that
  broadcasts with `prices`. The expiry for each option. The units should be
  such that `expiry * volatility**2` is dimensionless.
* <b>`spots`</b>: A real `Tensor` of any shape that broadcasts to the shape of the
  `prices`. The current spot price of the underlying. Either this argument
  or the `forwards` (but not both) must be supplied.
* <b>`forwards`</b>: A real `Tensor` of any shape that broadcasts to the shape of
  `prices`. The forwards to maturity. Either this argument or the `spots`
  must be supplied but both must not be supplied.
* <b>`discount_factors`</b>: An optional real `Tensor` of same dtype as the `prices`.
  If not None, these are the discount factors to expiry (i.e. e^(-rT)). If
  None, no discounting is applied (i.e. it is assumed that the undiscounted
  option prices are provided ). If `spots` is supplied and
  `discount_factors` is not None then this is also used to compute the
  forwards to expiry.
  Default value: None, equivalent to discount factors = 1.
* <b>`is_call_options`</b>: A boolean `Tensor` of a shape compatible with `prices`.
  Indicates whether the option is a call (if True) or a put (if False). If
  not supplied, call options are assumed.
* <b>`method`</b>: Enum value of ImpliedVolMethod to select the algorithm to use to
  infer the implied volatility.
* <b>`validate_args`</b>: A Python bool. If True, indicates that arguments should be
  checked for correctness before performing the computation. The checks
  performed are: (1) Forwards and strikes are positive. (2) The prices
    satisfy the arbitrage bounds (i.e. for call options, checks the
    inequality `max(F-K, 0) <= Price <= F` and for put options, checks that
    `max(K-F, 0) <= Price <= K`.). (3) Checks that the prices are not too
    close to the bounds. It is numerically unstable to compute the implied
    vols from options too far in the money or out of the money.
* <b>`dtype`</b>: `tf.Dtype` to use when converting arguments to `Tensor`s. If not
  supplied, the default TensorFlow conversion will take place. Note that
  this argument does not do any casting for `Tensor`s or numpy arrays.
  Default value: None.
* <b>`name`</b>: (Optional) Python str. The name prefixed to the ops created by this
  function. If not supplied, the default name 'implied_vol' is used.
  Default value: None
* <b>`**kwargs`</b>: Any other keyword arguments to be passed to the specific
  implementation. (See black_scholes.implied_vol_approx and
  black_scholes.implied_vol_newton for details).


#### Returns:


* <b>`implied_vols`</b>: A `Tensor` of the same dtype as `prices` and shape as the
  common broadcasted shape of `(prices, spots/forwards, strikes, expiries)`.
  The implied volatilities as inferred by the chosen method.


#### Raises:


* <b>`ValueError`</b>: If both `forwards` and `spots` are supplied or if neither is
  supplied.