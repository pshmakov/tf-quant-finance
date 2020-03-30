<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.models.hull_white.VectorHullWhiteModel" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="dim"/>
<meta itemprop="property" content="drift_fn"/>
<meta itemprop="property" content="dtype"/>
<meta itemprop="property" content="fd_solver_backward"/>
<meta itemprop="property" content="fd_solver_forward"/>
<meta itemprop="property" content="name"/>
<meta itemprop="property" content="sample_paths"/>
<meta itemprop="property" content="volatility_fn"/>
</div>

# tff.models.hull_white.VectorHullWhiteModel

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/hull_white/vector_hull_white.py">View source</a>



Ensemble of correlated Hull-White Models.

Inherits From: [`GenericItoProcess`](../../../tff/models/GenericItoProcess.md)

```python
tff.models.hull_white.VectorHullWhiteModel(
    dim, mean_reversion, volatility, instant_forward_rate_fn, corr_matrix=None,
    dtype=None, name=None
)
```



<!-- Placeholder for "Used in" -->

Represents the Ito process:

```None
  dr_i(t) = (theta_i(t) - a_i(t) * r_i(t)) dt + sigma_i(t) * dW_{r_i}(t),
  1 <= i <= n,
```
where `W_{r_i}` are 1D Brownian motions with a correlation matrix `Rho(t)`.
For each `i`, `r_i` is the Hull-White process.
`theta_i`, `a_i`, `sigma_i`, `Rho` are positive functions of time.
`a_i` correspond to the mean-reversion rate, `sigma_i` is the volatility of
the process, `theta_i(t)` is the function that determines long run behaviour
of the process `r(t) = (r_1(t), ..., r_n(t))`
and is defined to match the market data through the instantaneous forward
rate matching:

```None
\theta_i = df_i(t) / dt + a_i * f_i(t) + 0.5 * sigma_i**2 / a_i
           * (1 - exp(-2 * a_i *t)), 1 <= i <= n
```
where `f_i(t)` is the instantaneous forward rate at time `0` for a maturity
`t` and `df_i(t)/dt` is the gradient of `f_i` with respect to the maturity.
See Section 3.3.1 of [1] for details.

If the parameters `a_i`, `sigma_i` and `Rho` are piecewise constant functions,
the process is sampled exactly. Otherwise, Euler sampling is be used.

For `n=1` this class represents Hull-White Model (see
tff.models.hull_white.HullWhiteModel1F).

## Example. Two correlated Hull-White processes.

```python
import numpy as np
import tensorflow.compat.v2 as tf
import tf_quant_finance as tff

dtype = tf.float64
# Mean-reversion is a piecewise constant function with different jumps for
# the two processes. `mean_reversion(t)` has shape `[dim] + t.shape`.
mean_reversion = tff.math.piecewise.PiecewiseConstantFunc(
    jump_locations=[[1, 2, 3, 4], [1, 2, 3, 3]],
    values=[[0.1, 0.2, 0.3, 0.4, 0.5], [0.5, 0.2, 0.3, 0.4, 0.4]],
    dtype=dtype)
# Volatility is a piecewise constant function with jumps at the same locations
# for both Hull-White processes. `volatility(t)` has shape `[dim] + t.shape`.
volatility = tff.math.piecewise.PiecewiseConstantFunc(
    jump_locations=[0.1, 2.],
    values=[[0.1, 0.2], [0.1, 0.2], [0.1, 0.2]],
    dtype=dtype)
# Correlation matrix is constant
corr_matrix = [[1., 0.1], [0.1, 1.]]
instant_forward_rate_fn = lambda *args: [0.01, 0.02]
process = tff.models.hull_white.VectorHullWhiteModel(
    dim=2, mean_reversion=mean_reversion,
    volatility=volatility, instant_forward_rate_fn=instant_forward_rate_fn,
    corr_matrix=None,
    dtype=dtype)
# Sample 10000 paths using Sobol numbers as a random type.
times = np.linspace(0., 1.0, 10)
num_samples = 10000  # number of trajectories
paths = process.sample_paths(
    times,
    num_samples=num_samples,
    initial_state=[0.1, 0.2],
    random_type=tff.math.random.RandomType.SOBOL)
# Compute mean for each Hull-White process at the terminal value
tf.math.reduce_mean(paths[:, -1, :], axis=0)
# Expected value: [0.09594861, 0.14156537]
# Check that the correlation is recovered
np.corrcoef(paths[:, -1, 0], paths[:, -1, 1])
# Expected value: [[1.       , 0.0914114],
#                  [0.0914114, 1.       ]]
```

### References:
  [1]: D. Brigo, F. Mercurio. Interest Rate Models. 2007.

#### Args:


* <b>`dim`</b>: A Python scalar which corresponds to the number of correlated
  Hull-White Models.
* <b>`mean_reversion`</b>: A real positive `Tensor` of shape `[dim]` or a Python
  callable. The callable can be one of the following:
    (a) A left-continuous piecewise constant object (e.g.,
    <a href="../../../tff/math/piecewise/PiecewiseConstantFunc.md"><code>tff.math.piecewise.PiecewiseConstantFunc</code></a>) that has a property
    `is_piecewise_constant` set to `True`. In this case the object
    should have a method `jump_locations(self)` that returns a
    `Tensor` of shape `[dim, num_jumps]` or `[num_jumps]`
    In the first case, `mean_reversion(t)` should return a `Tensor`
    of shape `[dim] + t.shape`, and in the second, `t.shape + [dim]`,
    where `t` is a rank 1 `Tensor` of the same `dtype` as the output.
    See example in the class docstring.
   (b) A callable that accepts scalars (stands for time `t`) and returns a
   `Tensor` of shape `[dim]`.
  Corresponds to the mean reversion rate.
* <b>`volatility`</b>: A real positive `Tensor` of the same `dtype` as
  `mean_reversion` or a callable with the same specs as above.
  Corresponds to the lond run price variance.
* <b>`instant_forward_rate_fn`</b>: A Python callable that accepts expiry time as a
  scalar `Tensor` of the same `dtype` as `mean_reversion` and returns a
  `Tensor` of shape `[dim]`.
  Corresponds to the instanteneous forward rate at the present time for
  the input expiry time.
* <b>`corr_matrix`</b>: A `Tensor` of shape `[dim, dim]` and the same `dtype` as
  `mean_reversion` or a Python callable. The callable can be one of
  the following:
    (a) A left-continuous piecewise constant object (e.g.,
    <a href="../../../tff/math/piecewise/PiecewiseConstantFunc.md"><code>tff.math.piecewise.PiecewiseConstantFunc</code></a>) that has a property
    `is_piecewise_constant` set to `True`. In this case the object
    should have a method `jump_locations(self)` that returns a
    `Tensor` of shape `[num_jumps]`. `corr_matrix(t)` should return a
    `Tensor` of shape `t.shape + [dim]`, where `t` is a rank 1 `Tensor`
    of the same `dtype` as the output.
   (b) A callable that accepts scalars (stands for time `t`) and returns a
   `Tensor` of shape `[dim, dim]`.
  Corresponds to the correlation matrix `Rho`.
* <b>`dtype`</b>: The default dtype to use when converting values to `Tensor`s.
  Default value: `None` which means that default dtypes inferred by
    TensorFlow are used.
* <b>`name`</b>: Python string. The name to give to the ops created by this class.
  Default value: `None` which maps to the default name `hull_white_model`.


#### Raises:


* <b>`ValueError`</b>:   (a) If either `mean_reversion`, `volatility`, or `corr_matrix` is
    a piecewise constant function where `jump_locations` have batch shape
    of rank > 1.
  (b): If batch rank of the `jump_locations` is `[n]` with `n` different
    from `dim`.

## Methods

<h3 id="dim"><code>dim</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
dim()
```

The dimension of the process.


<h3 id="drift_fn"><code>drift_fn</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
drift_fn()
```

Python callable calculating instantaneous drift.

The callable should accept two real `Tensor` arguments of the same dtype.
The first argument is the scalar time t, the second argument is the value of
Ito process X - `Tensor` of shape `batch_shape + [dim]`. The result is the
value of drift a(t, X). The return value of the callable is a real `Tensor`
of the same dtype as the input arguments and of shape `batch_shape + [dim]`.

#### Returns:

The instantaneous drift rate callable.


<h3 id="dtype"><code>dtype</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
dtype()
```

The data type of process realizations.


<h3 id="fd_solver_backward"><code>fd_solver_backward</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
fd_solver_backward(
    start_time, end_time, coord_grid, values_grid, discounting=None,
    one_step_fn=None, boundary_conditions=None, start_step_count=0, num_steps=None,
    time_step=None, values_transform_fn=None, dtype=None, name=None, **kwargs
)
```

Returns a solver for Feynman-Kac PDE associated to the process.

This method applies a finite difference method to solve the final value
problem as it appears in the Feynman-Kac formula associated to this Ito
process. The Feynman-Kac PDE is closely related to the backward Kolomogorov
equation associated to the stochastic process and allows for the inclusion
of a discounting function.

For more details of the Feynman-Kac theorem see [1]. The PDE solved by this
method is:

```None
  V_t + Sum[mu_i(t, x) V_i, 1<=i<=n] +
    (1/2) Sum[ D_{ij} V_{ij}, 1 <= i,j <= n] - r(t, x) V = 0
```

In the above, `V_t` is the derivative of `V` with respect to `t`,
`V_i` is the partial derivative with respect to `x_i` and `V_{ij}` the
(mixed) partial derivative with respect to `x_i` and `x_j`. `mu_i` is the
drift of this process and `D_{ij}` are the components of the diffusion
tensor:

```None
  D_{ij}(t,x) = (Sigma(t,x) . Transpose[Sigma(t,x)])_{ij}
```

This method evolves a spatially discretized solution of the above PDE from
time `t0` to time `t1 < t0` (i.e. backwards in time).
The solution `V(t,x)` is assumed to be discretized on an `n`-dimensional
rectangular grid. A rectangular grid, G, in n-dimensions may be described
by specifying the coordinates of the points along each axis. For example,
a 2 x 4 grid in two dimensions can be specified by taking the cartesian
product of [1, 3] and [5, 6, 7, 8] to yield the grid points with
coordinates: `[(1, 5), (1, 6), (1, 7), (1, 8), (3, 5) ... (3, 8)]`.

This method allows batching of solutions. In this context, batching means
the ability to represent and evolve multiple independent functions `V`
(e.g. V1, V2 ...) simultaneously. A single discretized solution is specified
by stating its values at each grid point. This can be represented as a
`Tensor` of shape [d1, d2, ... dn] where di is the grid size along the `i`th
axis. A batch of such solutions is represented by a `Tensor` of shape:
[K, d1, d2, ... dn] where `K` is the batch size. This method only requires
that the input parameter `values_grid` be broadcastable with shape
[K, d1, ... dn].

The evolution of the solution from `t0` to `t1` is often done by
discretizing the differential equation to a difference equation along
the spatial and temporal axes. The temporal discretization is given by a
(sequence of) time steps [dt_1, dt_2, ... dt_k] such that the sum of the
time steps is equal to the total time step `t0 - t1`. If a uniform time
step is used, it may equivalently be specified by stating the number of
steps (n_steps) to take. This method provides both options via the
`time_step` and `num_steps` parameters. However, not all methods need
discretization along time direction (e.g. method of lines) so this argument
may not be applicable to some implementations.

The workhorse of this method is the `one_step_fn`. For the commonly used
methods, see functions in <a href="../../../tff/math/pde/steppers.md"><code>math.pde.steppers</code></a> module.

The mapping between the arguments of this method and the above
equation are described in the Args section below.

For a simple instructive example of implementation of this method, see
<a href="../../../tff/models/GenericItoProcess.md#fd_solver_backward"><code>models.GenericItoProcess.fd_solver_backward</code></a>.

# TODO(b/142309558): Complete documentation.

#### Args:


* <b>`start_time`</b>: Real positive scalar `Tensor`. The start time of the grid.
  Corresponds to time `t0` above.
* <b>`end_time`</b>: Real scalar `Tensor` smaller than the `start_time` and greater
  than zero. The time to step back to. Corresponds to time `t1` above.
* <b>`coord_grid`</b>: List of `n` rank 1 real `Tensor`s. `n` is the dimension of the
  domain. The i-th `Tensor` has shape, `[d_i]` where `d_i` is the size of
  the grid along axis `i`. The coordinates of the grid points. Corresponds
  to the spatial grid `G` above.
* <b>`values_grid`</b>: Real `Tensor` containing the function values at time
  `start_time` which have to be stepped back to time `end_time`. The shape
  of the `Tensor` must broadcast with `[K, d_1, d_2, ..., d_n]`. The first
  axis of size `K` is the values batch dimension and allows multiple
  functions (with potentially different boundary/final conditions) to be
  stepped back simultaneously.
* <b>`discounting`</b>: Callable corresponding to `r(t,x)` above. If not supplied,
  zero discounting is assumed.
* <b>`one_step_fn`</b>: The transition kernel. A callable that consumes the following
  arguments by keyword:
    1. 'time': Current time
    2. 'next_time': The next time to step to. For the backwards in time
      evolution, this time will be smaller than the current time.
    3. 'coord_grid': The coordinate grid.
    4. 'values_grid': The values grid.
    5. 'boundary_conditions': The boundary conditions.
    6. 'quadratic_coeff': A callable returning the quadratic coefficients
      of the PDE (i.e. `(1/2)D_{ij}(t, x)` above). The callable accepts
      the time and  coordinate grid as keyword arguments and returns a
      `Tensor` with shape that broadcasts with `[dim, dim]`.
    7. 'linear_coeff': A callable returning the linear coefficients of the
      PDE (i.e. `mu_i(t, x)` above). Accepts time and coordinate grid as
      keyword arguments and returns a `Tensor` with shape that broadcasts
      with `[dim]`.
    8. 'constant_coeff': A callable returning the coefficient of the
      linear homogenous term (i.e. `r(t,x)` above). Same spec as above.
      The `one_step_fn` callable returns a 2-tuple containing the next
      coordinate grid, next values grid.
* <b>`boundary_conditions`</b>: A list of size `dim` containing boundary conditions.
  The i'th element of the list is a 2-tuple containing the lower and upper
  boundary condition for the boundary along the i`th axis.
* <b>`start_step_count`</b>: Scalar integer `Tensor`. Initial value for the number of
  time steps performed.
  Default value: 0 (i.e. no previous steps performed).
* <b>`num_steps`</b>: Positive int scalar `Tensor`. The number of time steps to take
  when moving from `start_time` to `end_time`. Either this argument or the
  `time_step` argument must be supplied (but not both). If num steps is
  `k>=1`, uniform time steps of size `(t0 - t1)/k` are taken to evolve the
  solution from `t0` to `t1`. Corresponds to the `n_steps` parameter
  above.
* <b>`time_step`</b>: The time step to take. Either this argument or the `num_steps`
  argument must be supplied (but not both). The type of this argument may
  be one of the following (in order of generality): (a) None in which case
    `num_steps` must be supplied. (b) A positive real scalar `Tensor`. The
    maximum time step to take. If the value of this argument is `dt`, then
    the total number of steps taken is N = (t0 - t1) / dt rounded up to
    the nearest integer. The first N-1 steps are of size dt and the last
    step is of size `t0 - t1 - (N-1) * dt`. (c) A callable accepting the
    current time and returning the size of the step to take. The input and
    the output are real scalar `Tensor`s.
* <b>`values_transform_fn`</b>: An optional callable applied to transform the
  solution values at each time step. The callable is invoked after the
  time step has been performed. The callable should accept the time of the
  grid, the coordinate grid and the values grid and should return the
  values grid. All input arguments to be passed by keyword.
* <b>`dtype`</b>: The dtype to use.
* <b>`name`</b>: The name to give to the ops.
  Default value: None which means `solve_backward` is used.
* <b>`**kwargs`</b>: Additional keyword args:
  (1) pde_solver_fn: Function to solve the PDE that accepts all the above
    arguments by name and returns the same tuple object as required below.
    Defaults to <a href="../../../tff/math/pde/fd_solvers/solve_backward.md"><code>tff.math.pde.fd_solvers.solve_backward</code></a>.


#### Returns:

A tuple object containing at least the following attributes:
  final_values_grid: A `Tensor` of same shape and dtype as `values_grid`.
    Contains the final state of the values grid at time `end_time`.
  final_coord_grid: A list of `Tensor`s of the same specification as
    the input `coord_grid`. Final state of the coordinate grid at time
    `end_time`.
  step_count: The total step count (i.e. the sum of the `start_step_count`
    and the number of steps performed in this call.).
  final_time: The final time at which the evolution stopped. This value
    is given by `max(min(end_time, start_time), 0)`.


<h3 id="fd_solver_forward"><code>fd_solver_forward</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
fd_solver_forward(
    start_time, end_time, coord_grid, values_grid, one_step_fn=None,
    boundary_conditions=None, start_step_count=0, num_steps=None, time_step=None,
    values_transform_fn=None, dtype=None, name=None, **kwargs
)
```

Returns a solver for the Fokker Plank equation of this process.

The Fokker Plank equation (also known as the Kolmogorov Forward equation)
associated to this Ito process is given by:

```None
  V_t + Sum[(mu_i(t, x) V)_i, 1<=i<=n]
    - (1/2) Sum[ (D_{ij} V)_{ij}, 1 <= i,j <= n] = 0
```

with the initial value condition $$V(0, x) = u(x)$$.

This method evolves a spatially discretized solution of the above PDE from
time `t0` to time `t1 > t0` (i.e. forwards in time).
The solution `V(t,x)` is assumed to be discretized on an `n`-dimensional
rectangular grid. A rectangular grid, G, in n-dimensions may be described
by specifying the coordinates of the points along each axis. For example,
a 2 x 4 grid in two dimensions can be specified by taking the cartesian
product of [1, 3] and [5, 6, 7, 8] to yield the grid points with
coordinates: `[(1, 5), (1, 6), (1, 7), (1, 8), (3, 5) ... (3, 8)]`.

Batching of solutions is supported. In this context, batching means
the ability to represent and evolve multiple independent functions `V`
(e.g. V1, V2 ...) simultaneously. A single discretized solution is specified
by stating its values at each grid point. This can be represented as a
`Tensor` of shape [d1, d2, ... dn] where di is the grid size along the `i`th
axis. A batch of such solutions is represented by a `Tensor` of shape:
[K, d1, d2, ... dn] where `K` is the batch size. This method only requires
that the input parameter `values_grid` be broadcastable with shape
[K, d1, ... dn].

The evolution of the solution from `t0` to `t1` is often done by
discretizing the differential equation to a difference equation along
the spatial and temporal axes. The temporal discretization is given by a
(sequence of) time steps [dt_1, dt_2, ... dt_k] such that the sum of the
time steps is equal to the total time step `t1 - t0`. If a uniform time
step is used, it may equivalently be specified by stating the number of
steps (n_steps) to take. This method provides both options via the
`time_step` and `num_steps` parameters. However, not all methods need
discretization along time direction (e.g. method of lines) so this argument
may not be applicable to some implementations.

The workhorse of this method is the `one_step_fn`. For the commonly used
methods, see functions in <a href="../../../tff/math/pde/steppers.md"><code>math.pde.steppers</code></a> module.

The mapping between the arguments of this method and the above
equation are described in the Args section below.

For a simple instructive example of implementation of this method, see
<a href="../../../tff/models/GenericItoProcess.md#fd_solver_forward"><code>models.GenericItoProcess.fd_solver_forward</code></a>.

# TODO(b/142309558): Complete documentation.

#### Args:


* <b>`start_time`</b>: Real positive scalar `Tensor`. The start time of the grid.
  Corresponds to time `t0` above.
* <b>`end_time`</b>: Real scalar `Tensor` smaller than the `start_time` and greater
  than zero. The time to step back to. Corresponds to time `t1` above.
* <b>`coord_grid`</b>: List of `n` rank 1 real `Tensor`s. `n` is the dimension of the
  domain. The i-th `Tensor` has shape, `[d_i]` where `d_i` is the size of
  the grid along axis `i`. The coordinates of the grid points. Corresponds
  to the spatial grid `G` above.
* <b>`values_grid`</b>: Real `Tensor` containing the function values at time
  `start_time` which have to be stepped back to time `end_time`. The shape
  of the `Tensor` must broadcast with `[K, d_1, d_2, ..., d_n]`. The first
  axis of size `K` is the values batch dimension and allows multiple
  functions (with potentially different boundary/final conditions) to be
  stepped back simultaneously.
* <b>`one_step_fn`</b>: The transition kernel. A callable that consumes the following
  arguments by keyword:
    1. 'time': Current time
    2. 'next_time': The next time to step to. For the backwards in time
      evolution, this time will be smaller than the current time.
    3. 'coord_grid': The coordinate grid.
    4. 'values_grid': The values grid.
    5. 'quadratic_coeff': A callable returning the quadratic coefficients
      of the PDE (i.e. `(1/2)D_{ij}(t, x)` above). The callable accepts
      the time and  coordinate grid as keyword arguments and returns a
      `Tensor` with shape that broadcasts with `[dim, dim]`.
    6. 'linear_coeff': A callable returning the linear coefficients of the
      PDE (i.e. `mu_i(t, x)` above). Accepts time and coordinate grid as
      keyword arguments and returns a `Tensor` with shape that broadcasts
      with `[dim]`.
    7. 'constant_coeff': A callable returning the coefficient of the
      linear homogenous term (i.e. `r(t,x)` above). Same spec as above.
      The `one_step_fn` callable returns a 2-tuple containing the next
      coordinate grid, next values grid.
* <b>`boundary_conditions`</b>: A list of size `dim` containing boundary conditions.
  The i'th element of the list is a 2-tuple containing the lower and upper
  boundary condition for the boundary along the i`th axis.
* <b>`start_step_count`</b>: Scalar integer `Tensor`. Initial value for the number of
  time steps performed.
  Default value: 0 (i.e. no previous steps performed).
* <b>`num_steps`</b>: Positive int scalar `Tensor`. The number of time steps to take
  when moving from `start_time` to `end_time`. Either this argument or the
  `time_step` argument must be supplied (but not both). If num steps is
  `k>=1`, uniform time steps of size `(t0 - t1)/k` are taken to evolve the
  solution from `t0` to `t1`. Corresponds to the `n_steps` parameter
  above.
* <b>`time_step`</b>: The time step to take. Either this argument or the `num_steps`
  argument must be supplied (but not both). The type of this argument may
  be one of the following (in order of generality): (a) None in which case
    `num_steps` must be supplied. (b) A positive real scalar `Tensor`. The
    maximum time step to take. If the value of this argument is `dt`, then
    the total number of steps taken is N = (t1 - t0) / dt rounded up to
    the nearest integer. The first N-1 steps are of size dt and the last
    step is of size `t1 - t0 - (N-1) * dt`. (c) A callable accepting the
    current time and returning the size of the step to take. The input and
    the output are real scalar `Tensor`s.
* <b>`values_transform_fn`</b>: An optional callable applied to transform the
  solution values at each time step. The callable is invoked after the
  time step has been performed. The callable should accept the time of the
  grid, the coordinate grid and the values grid and should return the
  values grid. All input arguments to be passed by keyword.
* <b>`dtype`</b>: The dtype to use.
* <b>`name`</b>: The name to give to the ops.
  Default value: None which means `solve_forward` is used.
* <b>`**kwargs`</b>: Additional keyword args:
  (1) pde_solver_fn: Function to solve the PDE that accepts all the above
    arguments by name and returns the same tuple object as required below.
    Defaults to <a href="../../../tff/math/pde/fd_solvers/solve_forward.md"><code>tff.math.pde.fd_solvers.solve_forward</code></a>.


#### Returns:

A tuple object containing at least the following attributes:
  final_values_grid: A `Tensor` of same shape and dtype as `values_grid`.
    Contains the final state of the values grid at time `end_time`.
  final_coord_grid: A list of `Tensor`s of the same specification as
    the input `coord_grid`. Final state of the coordinate grid at time
    `end_time`.
  step_count: The total step count (i.e. the sum of the `start_step_count`
    and the number of steps performed in this call.).
  final_time: The final time at which the evolution stopped. This value
    is given by `max(min(end_time, start_time), 0)`.


<h3 id="name"><code>name</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
name()
```

The name to give to ops created by this class.


<h3 id="sample_paths"><code>sample_paths</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/hull_white/vector_hull_white.py">View source</a>

```python
sample_paths(
    times, initial_state, num_samples=1, random_type=None, seed=None, skip=0,
    time_step=None, name=None
)
```

Returns a sample of paths from the correlated Hull-White process.

Uses exact sampling if `self.mean_reversion`, `self.volatility` and
`self.corr_matrix` are all `Tensor`s or piecewise constant functions, and
Euler scheme sampling if one of the arguments is a generic callable.

#### Args:


* <b>`times`</b>: Rank 1 `Tensor` of positive real values. The times at which the
  path points are to be evaluated.
* <b>`initial_state`</b>: A `Tensor` of the same `dtype` as `times` and shape
  broadcastable with `[num_samples, self._dim]`
* <b>`num_samples`</b>: Positive scalar `int32` `Tensor`. The number of paths to
  draw.
* <b>`random_type`</b>: Enum value of `RandomType`. The type of (quasi)-random
  number generator to use to generate the paths.
  Default value: `None` which maps to the standard pseudo-random numbers.
* <b>`seed`</b>: Python `int`. The random seed to use.
  Default value: None, i.e., no seed is set.
* <b>`skip`</b>: `int32` 0-d `Tensor`. The number of initial points of the Sobol or
  Halton sequence to skip. Used only when `random_type` is 'SOBOL',
  'HALTON', or 'HALTON_RANDOMIZED', otherwise ignored.
  Default value: `0`.
* <b>`time_step`</b>: Scalar real `Tensor`. Maximal distance between time grid points
  in Euler scheme. Used only when Euler scheme is applied.
Default value: `None`.
* <b>`name`</b>: Python string. The name to give this op.
  Default value: `sample_paths`.


#### Returns:

A `Tensor`s of shape [num_samples, k, dim] where `k` is the size
of the `times`, `dim` is the dimension of the process. For each sample and
time the first dimension represents the simulated log-state trajectories
of the spot price `X(t)`, whereas the second one represents the simulated
variance trajectories `V(t)`.



#### Raises:


* <b>`ValueError`</b>:   (a) If `times` has rank different from `1`.
  (b) If Euler scheme is used by times is not supplied.

<h3 id="volatility_fn"><code>volatility_fn</code></h3>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/generic_ito_process.py">View source</a>

```python
volatility_fn()
```

Python callable calculating the instantaneous volatility.

The callable should accept two real `Tensor` arguments of the same dtype and
shape `times_shape`. The first argument is the scalar time t, the second
argument is the value of Ito process X - `Tensor` of shape `batch_shape +
[dim]`. The result is value of volatility `S_ij`(t, X). The return value of
the callable is a real `Tensor` of the same dtype as the input arguments and
of shape `batch_shape + [dim, dim]`.

#### Returns:

The instantaneous volatility callable.




