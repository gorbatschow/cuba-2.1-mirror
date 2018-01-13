# Cuba - a library for multidimensional numerical integration
Developer's site http://www.feynarts.de/cuba/

The Cuba library offers a choice of four independent routines for multidimensional numerical integration: Vegas, Suave, Divonne, and Cuhre. They work by very different methods, summarized in the following table:

| Routine | Basic integration method                  | Algorithm type	| Variance reduction
| ------- | ----------------------------------------- | --------------- | --------------------
| Vegas   | Sobol quasi-random sample                 | Monte Carlo     | importance sampling
|         | or Mersenne Twister pseudo-random sample  | Monte Carlo     |
|         | or Ranlux pseudo-random sample            | Monte Carlo     |
|         |                                           |                 |
| Suave   | Sobol quasi-random sample                 | Monte Carlo     | globally adaptive subdivision
|         | or Mersenne Twister pseudo-random sample  | Monte Carlo     | + importance sampling
|         | or Ranlux pseudo-random sample            | Monte Carlo     |
|         |                                           |                 |
| Divonne | Korobov quasi-random sample               | Monte Carlo     | stratified sampling,
|         | or Sobol quasi-random sample              | Monte Carlo     | aided by methods from
|         | or Mersenne Twister pseudo-random sample  | Monte Carlo     | numerical optimization
|         | or Ranlux pseudo-random sample            | Monte Carlo     | 
|         | or cubature rules                         | deterministic   |
|         |                                           |                 |
| Cuhre   | Cubature rules                            | deterministic   | globally adaptive subdivision

All four have a C/C++, Fortran, and Mathematica interface and can integrate vector integrands. 
Their invocation is very similar, so it is easy to substitute one method by another for cross-checking. 
For further safeguarding, the output is supplemented by a chi-square probability which quantifies the reliability of the error estimate.
The source code compiles with gcc, the GNU C compiler. 
The C functions can be called from Fortran directly, so there is no need for adapter code. 
Similarly, linking Fortran code with the library is straightforward and requires no extra tools.

**Vegas** is the simplest of the four.It uses importance sampling for variance reduction, 
but is only in some cases competitive in terms of the number of samples needed to reach a prescribed accuracy. 
Nevertheless, it has a few improvements over the original algorithm and comes in handy for cross-checking the results of other methods.

**Suave** is a new algorithm which combines the advantages of two popular methods: 
importance sampling as done by Vegas and subregion sampling in a manner similar to Miser. 
By dividing into subregions, Suave manages to a certain extent to get around Vegas' difficulty 
to adapt its weight function to structures not aligned with the coordinate axes.

**Divonne** is a further development of the CERNLIB routine D151. 
Divonne works by stratified sampling, where the partitioning of the integration region is aided by methods 
from numerical optimization. A number of improvements have been added to this algorithm, 
the most significant being the possibility to supply knowledge about the integrand. 
Narrow peaks in particular are difficult to find without sampling very many points, especially in high dimensions. 
Often the exact or approximate location of such peaks is known from analytic considerations, 
however, and with such hints the desired accuracy can be reached with far fewer points.

**Cuhre** employs a cubature rule for subregion estimation in a globally adaptive subdivision scheme.
It is hence a deterministic, not a Monte Carlo method. 
In each iteration, the subregion with the largest error is halved along the axis where the integrand has the largest fourth difference. 
Cuhre is quite powerful in moderate dimensions, and is usually the only viable method to obtain high precision, 
say relative accuracies much below 1e-3.
