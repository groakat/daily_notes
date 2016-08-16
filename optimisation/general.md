# When posing an optimisation problem, watch out for

## Value ranges of parameters
When optimising arbitrary functions, make sure that all variables are normalised, ideally between 0..1. Otherwise the optimisation will tweak the parameters with the narrow range too aggressively and neglect the parameters with the large range.
