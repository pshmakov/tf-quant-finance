<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.math.pde.steppers.composite_stepper.composite_scheme_step" />
<meta itemprop="path" content="Stable" />
</div>

# tff.math.pde.steppers.composite_stepper.composite_scheme_step

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/pde/steppers/composite_stepper.py">View source</a>



Composes two time marching schemes.

<section class="expandable">
  <h4 class="showalways">View aliases</h4>
  <p>
<b>Main aliases</b>
<p>`tff.math.pde.steppers.oscillation_damped_crank_nicolson.composite_scheme_step`</p>
</p>
</section>

```python
tff.math.pde.steppers.composite_stepper.composite_scheme_step(
    first_scheme_steps, first_scheme, second_scheme
)
```



<!-- Placeholder for "Used in" -->

Applies a step of parabolic PDE solver using `first_scheme` if number of
performed steps is less than `first_scheme_steps`, and using `second_scheme`
otherwise.

#### Args:


* <b>`first_scheme_steps`</b>: A Python integer. Number of steps to apply
  `first_scheme` on.
* <b>`first_scheme`</b>: First time marching scheme (see `time_marching_scheme`
  argument of `parabolic_equation_step`).
* <b>`second_scheme`</b>: Second time marching scheme (see `time_marching_scheme`
  argument of `parabolic_equation_step`).


#### Returns:

Callable to be used in finite-difference PDE solvers (see fd_solvers.py).
