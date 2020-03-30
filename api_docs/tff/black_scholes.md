<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tff.black_scholes" />
<meta itemprop="path" content="Stable" />
</div>

# Module: tff.black_scholes

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/black_scholes/__init__.py">View source</a>



TensorFlow Quantitative Finance volatility surfaces and vanilla options.



## Modules

[`approximations`](../tff/black_scholes/approximations.md) module: Approximations to the black scholes formula.

## Classes

[`class ImpliedVolMethod`](../tff/black_scholes/ImpliedVolMethod.md): An enumeration.

## Functions

[`binary_price(...)`](../tff/black_scholes/binary_price.md): Computes the Black Scholes price for a batch of binary call or put options.

[`implied_vol(...)`](../tff/black_scholes/implied_vol.md): Finds the implied volatilities of options under the Black Scholes model.

[`implied_vol_approx(...)`](../tff/black_scholes/implied_vol_approx.md): Approximates the implied vol using the Stefanica-Radiocic algorithm.

[`implied_vol_newton(...)`](../tff/black_scholes/implied_vol_newton.md): Computes implied volatilities from given call or put option prices.

[`option_price(...)`](../tff/black_scholes/option_price.md): Computes the Black Scholes price for a batch of call or put options.

[`option_price_binomial(...)`](../tff/black_scholes/option_price_binomial.md): Computes the BS price for a batch of European or American options.

