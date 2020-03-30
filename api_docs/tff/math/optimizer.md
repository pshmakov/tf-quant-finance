<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.math.optimizer" />
<meta itemprop="path" content="Stable" />
</div>

# Module: tff.math.optimizer

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/optimizer/__init__.py">View source</a>



Optimization methods.



## Classes

[`class ConjugateGradientParams`](../../tff/math/optimizer/ConjugateGradientParams.md): Adjustable parameters of conjugate gradient algorithm.

## Functions

[`bfgs_minimize(...)`](../../tff/math/optimizer/bfgs_minimize.md): Applies the BFGS algorithm to minimize a differentiable function.

[`conjugate_gradient_minimize(...)`](../../tff/math/optimizer/conjugate_gradient_minimize.md): Minimizes a differentiable function.

[`converged_all(...)`](../../tff/math/optimizer/converged_all.md): Condition to stop when all batch members have converged or failed.

[`converged_any(...)`](../../tff/math/optimizer/converged_any.md): Condition to stop when any batch member converges, or all have failed.

[`differential_evolution_minimize(...)`](../../tff/math/optimizer/differential_evolution_minimize.md): Applies the Differential evolution algorithm to minimize a function.

[`differential_evolution_one_step(...)`](../../tff/math/optimizer/differential_evolution_one_step.md): Performs one step of the differential evolution algorithm.

[`lbfgs_minimize(...)`](../../tff/math/optimizer/lbfgs_minimize.md): Applies the L-BFGS algorithm to minimize a differentiable function.

[`nelder_mead_minimize(...)`](../../tff/math/optimizer/nelder_mead_minimize.md): Minimum of the objective function using the Nelder Mead simplex algorithm.

[`nelder_mead_one_step(...)`](../../tff/math/optimizer/nelder_mead_one_step.md): A single iteration of the Nelder Mead algorithm.

