# Sampling a high dimensional space under non-linear constraints.  
Initially composed as a technical challenge for Citrine. This includes a package with an example 
[jupyter notebook](notebooks/Adaptive%20Displacement.ipynb),
 [python script](scripts/sampler.py), [installed script](bin/sampler) , and [unit tests](test). 
 The principle functionality of the package is to use a Markov chain
 Monte Carlo to sample a unit hypersphere subject to constraints. The approach by default uses an adaptive displacement procedure
 for a variable rate of exploration dependent on the acceptance rate of points. 
 Example input files are available in the [examples](examples) folder, 
 with corresponding outputs in the [data](data) folder.  
 For more information on the input format, please consult
 the [prompt](prompt.pdf).  

## Install from source
Clone the git repository located at 
[github.com/maffettone/constrained_sampler](https://github.com/maffettone/constrained_sampler.git). 

Navigate to the cloned repository and run `pip install .`

From source, the python script can be run using `python scripts/sampler.py`, or using the command `sampler` after install.
Note that this package is dependent on [numpy](https://docs.scipy.org/doc/numpy/reference/). 

## Install from PyPy
`pip install constrained-sampler-maffettone`

This will download the source and install into the current environment. The script `sampler` will then be 
available in the path for use.

## Operation
Several additional optional parameters are included in the script to allow for more/less thorough sampling of space. 

| Command | Description |
| --- | --- |
| -s --seed | Optional integer for seeding the random number generator. |
| -m --max_steps | Maximum number of steps for internal Markov chain. Defaults to 1e7. | 
| -t --timeout | Timeout for sampler in minutes. Sampler will exit if exploration of max_steps is incomplete in timeout minutes. Defaults to 4.5 minutes.'|
| -a --acceptance_rate | Desired acceptance rate for internal Markov chain. This impacts the adaptive displacement algorithm. Defaults to 0.01. |


## Scaling and future work
* Given the security considerations of `eval` in the provided Constraint class, and the nature of the functions evaluated,
this could be replaced with a Pandas based evaluation function that is more secure. 
* There are a number of special cases have analytic solutions (linear constraints defining a simplex), that can be hard-coded 
in a switch case for speed in these circumstances. 
D. B. Rubin, The Bayesian Bootstrap *Ann. Statist.*, **9** 1981, 130-134.
* This approach takes a memory intensive approach in assembling a broad sampling of space, then randomly sampling a subset
of that broad sample as needed. More time expensive processes (below) can and be implemented that do not retain a broad sample
during the Markov process. 
* Usage of the autocorrelation function would allow for sampling from the Markov chain dynamically only as samples become 
sufficiently uncorrelated. This would work in a less memory intensive way than the implemented adaptive displacement + resampling.
D. Chandler, Introduction to Modern Statistical Mechanics (Oxford University Press, 1987).
