# jaxnuts
Python implementation of the No-U-Turn Sampler from [Hoffman and Gelman](https://arxiv.org/pdf/1111.4246.pdf) (Algorithm 6) leveraging JAX.

## Usage

Import libraries
```python
import jax
import jax.numpy as jnp
import jax.random as random

from jaxnuts.sampler import NUTS
```

For low dimensional problems such as this simple example, force JAX to use the CPU (avoid GPU overhead)
```python
jax.config.update('jax_platform_name', 'cpu')
```

Define a log-probability to sample from
```python
def logprob(x):
  """Standard normal"""
  return -.5 * jnp.dot(x, x)
```

Generate samples
```python
key = random.PRNGkey(0)
sampler = NUTS(jnp.ones(2), logp=logprob, target_acceptance=.5, M_adapt=1000)
key, samples, step_size = sampler.sample(1000, key)
```
