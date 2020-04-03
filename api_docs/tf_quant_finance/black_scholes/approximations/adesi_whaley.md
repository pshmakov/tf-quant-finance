<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf_quant_finance.black_scholes.approximations.adesi_whaley" />
<meta itemprop="path" content="Stable" />
</div>

# tf_quant_finance.black_scholes.approximations.adesi_whaley

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/black_scholes/approximations/american_option.py">View source</a>



Computes American option prices using the Baron-Adesi Whaley approximation.

```python
tf_quant_finance.black_scholes.approximations.adesi_whaley(
    *, volatilities, strikes, expiries, spots=None, forwards=None,
    discount_rates=None, continuous_dividends=None, cost_of_carries=None,
    discount_factors=None, is_call_options=None, max_iterations=100,
    tolerance=1e-08, dtype=None, name=None
)
```



<!-- Placeholder for "Used in" -->

#### Example

```python
spots = np.array([80.0, 90.0, 100.0, 110.0, 120.0])
strikes = np.array([100.0, 100.0, 100.0, 100.0, 100.0])
volatilities = np.array([0.2, 0.2, 0.2, 0.2, 0.2])
expiries = 0.25
cost_of_carries = -0.04
discount_rates = 0.08
computed_prices = adesi_whaley(
    volatilities,
    strikes,
    expiries,
    discount_rates,
    cost_of_carries,
    spots=spots,
    dtype=tf.float64)
# Expected print output of computed prices:
# [0.03, 0.59, 3.52, 10.31, 20.0]
```

#### References:
[1] Baron-Adesi, Whaley, Efficient Analytic Approximation of American Option
  Values, The Journal of Finance, Vol XLII, No. 2, June 1987
  https://deriscope.com/docs/Barone_Adesi_Whaley_1987.pdf

#### Args:


* <b>`volatilities`</b>: Real `Tensor` of any shape and dtype. The volatilities to
  expiry of the options to price.
* <b>`strikes`</b>: A real `Tensor` of the same dtype and compatible shape as
  `volatilities`. The strikes of the options to be priced.
* <b>`expiries`</b>: A real `Tensor` of same dtype and compatible shape as
  `volatilities`. The expiry of each option. The units should be such that
  `expiry * volatility**2` is dimensionless.
* <b>`spots`</b>: A real `Tensor` of any shape that broadcasts to the shape of the
  `volatilities`. The current spot price of the underlying. Either this
  argument or the `forwards` (but not both) must be supplied.
* <b>`forwards`</b>: A real `Tensor` of any shape that broadcasts to the shape of
  `volatilities`. The forwards to maturity. Either this argument or the
  `spots` must be supplied but both must not be supplied.
* <b>`discount_rates`</b>: An optional real `Tensor` of same dtype as the
  `volatilities`. If not `None`, discount factors are calculated as e^(-rT),
  where r are the discount rates, or risk free rates.
  Default value: `None`, equivalent to r = 0 and discount factors = 1 when
  discount_factors also not given.
* <b>`continuous_dividends`</b>: An optional real `Tensor` of same dtype as the
  `volatilities`. If not `None`, cost_of_carries is calculated as r - q,
  where r are the discount rates and q is continuous_dividends. Either this
  or cost_of_carries can be given.
  Default value: `None`, equivalent to q = 0.
* <b>`cost_of_carries`</b>: An optional real `Tensor` of same dtype as the
  `volatilities`. Cost of storing a physical commodity, the cost of
  interest paid when long, or the opportunity cost, or the cost of paying
  dividends when short. If not `None`, and `spots` is supplied, used to
  calculate forwards from spots: F = e^(bT) * S. If `None`, value assumed
  to be equal to the discount rate - continuous_dividends
  Default value: `None`, equivalent to b = r.
* <b>`discount_factors`</b>: An optional real `Tensor` of same dtype as the
  `volatilities`. If not `None`, these are the discount factors to expiry
  (i.e. e^(-rT)). Mutually exclusive with discount_rate and cost_of_carry.
  If neither is given, no discounting is applied (i.e. the undiscounted
  option price is returned). If `spots` is supplied and `discount_factors`
  is not `None` then this is also used to compute the forwards to expiry.
  At most one of discount_rates and discount_factors can be supplied.
  Default value: `None`, which maps to -log(discount_factors) / expiries
* <b>`is_call_options`</b>: A boolean `Tensor` of a shape compatible with
  `volatilities`. Indicates whether the option is a call (if True) or a put
  (if False). If not supplied, call options are assumed.
* <b>`max_iterations`</b>: positive `int`. The maximum number of iterations of Newton's
  root finding method to find the critical spot price above and below which
  the pricing formula is different.
  Default value: 100
* <b>`tolerance`</b>: Positive scalar `Tensor`. As with max_iterations, used with the
  Newton root finder to find the critical spot price. The root finder will
  judge an element to have converged if `|f(x_n) - a|` is less than
  `tolerance` (where `f` is the target function as defined in [1] and
  `x_n` is the estimated critical value), or if `x_n` becomes `nan`. When an
  element is judged to have converged it will no longer be updated. If all
  elements converge before `max_iterations` is reached then the root finder
  will return early.
  Default value: 1e-8
* <b>`dtype`</b>: Optional `tf.DType`. If supplied, the dtype to be used for conversion
  of any supplied non-`Tensor` arguments to `Tensor`.
  Default value: None which maps to the default dtype inferred by
   TensorFlow.
* <b>`name`</b>: str. The name for the ops created by this function.
  Default value: None which is mapped to the default name `adesi_whaley`.


#### Returns:

A 3-tuple containing the following items in order:
   (a) option_prices: A `Tensor` of the same shape as `forwards`. The Black
     Scholes price of the options.
   (b) converged: A boolean `Tensor` of the same shape as `option_prices`
     above. Indicates whether the corresponding adesi-whaley approximation
     has converged to within tolerance.
   (c) failed: A boolean `Tensor` of the same shape as `option_prices`
     above. Indicates whether the corresponding options price is NaN or not
     a finite number. Note that converged being True implies that failed
     will be false. However, it may happen that converged is False but
     failed is not True. This indicates the search did not converge in the
     permitted number of iterations but may converge if the iterations are
     increased.



#### Raises:


* <b>`ValueError`</b>:   (a) If both `forwards` and `spots` are supplied or if neither is supplied.
  (b) If both `continuous_dividends` and `cost_of_carries` are supplied.