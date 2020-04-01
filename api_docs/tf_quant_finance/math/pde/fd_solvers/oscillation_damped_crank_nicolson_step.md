<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf_quant_finance.math.pde.fd_solvers.oscillation_damped_crank_nicolson_step" />
<meta itemprop="path" content="Stable" />
</div>

# tf_quant_finance.math.pde.fd_solvers.oscillation_damped_crank_nicolson_step

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/pde/steppers/oscillation_damped_crank_nicolson.py">View source</a>



Scheme similar to Crank-Nicolson, but ensuring damping of oscillations.

```python
tf_quant_finance.math.pde.fd_solvers.oscillation_damped_crank_nicolson_step(
    extrapolation_steps=1
)
```



<!-- Placeholder for "Used in" -->

Performs first (or first few) steps with Extrapolation scheme, then proceeds
with Crank-Nicolson scheme. This combines absence of oscillations by virtue
of Extrapolation scheme with lower computational cost of Crank-Nicolson
scheme.

See [1], [2] ([2] mostly discusses using fully implicit scheme on the first
step, but mentions using extrapolation scheme for better accuracy in the end).

#### References:
[1]: D. Lawson, J & Ll Morris, J. The Extrapolation of First Order Methods for
  Parabolic Partial Differential Equations. I. 1978 SIAM Journal on Numerical
  Analysis. 15. 1212-1224.
  https://epubs.siam.org/doi/abs/10.1137/0715082
[2]: B. Giles, Michael & Carter, Rebecca. Convergence analysis of
  Crank-Nicolson and Rannacher time-marching. J. Comput. Finance. 9. 2005.
  https://core.ac.uk/download/pdf/1633712.pdf

#### Args:


* <b>`extrapolation_steps`</b>: number of first steps to which to apply the
  Extrapolation scheme. Defaults to `1`.


#### Returns:

Callable to use as `one_step_fn` in fd_solvers.