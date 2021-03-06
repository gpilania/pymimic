Tutorial
========

This tutorial explains the basic use of the GPE and EGO routines. More
thorough explanations are given in the User Documentation.


Basic use of Gaussian-process emulation
---------------------------------------

We will emulate the Forrester function (Forrester, 2008)

.. math::

   f(x) = (6x - 2)^{2} \sin(12x - 4),

which is defined on the unit interval, :math:`[0, 1]`. The example is
artifical, as we would never have any need to emulate this function. We may
always evaluate it directly. Suppose, however, that it were expensive to
evaluate and that we wished to make as few evaluations as possible. The
process is two-fold. First we must generate a set of training data for the
function. Second we must use these training data to estimate the value
:math:`f(x_*)`, for some arbitrary :math:`x_* \in [0, 1]`, called the \'test
point\' in the parlance of machine learning.

Generate the training input.

.. sourcecode:: python

   >>> import numpy as np
   >>> xtrain = np.linspace(0.0, 1.0, 10)

Generate the training output. The Forrester function is included in PyMimic's
``testfunc`` subpackage so we do not need to code it up.

.. sourcecode:: python
		
   >>> import pymimic as mim
   >>> ytrain = [mim.testfunc.forrester(xi) for xi in xtrain]
    
Generate the GPE prediction for :math:`f(x_{*})` for some random
:math:`x_{*}`. This is done by the function :func:`pymimic.gpe`, which returns
a ``GpeResult`` object, with attributes including ``y``, the prediction, and
``var``, the variance associated with this prediction.

.. sourcecode:: python
		
   >>> x = np.random.rand()
   >>> mim.gpe(x, xtrain, ytrain)
		  x: array([ 0.43132165])
		  y: array([ 0.32562232])
		var: array([ 0.00031421])
	     xtrain: array([ 0.        ,  0.11111111,  0.22222222,
	                     0.33333333,  0.44444444,  0.55555556,
			     0.66666667,  0.77777778,  0.88888889,
			     1.        ])
	     ytrain: array([  3.02720998, -0.81292911, -0.4319724 ,
	                      0.        ,  0.4319724 ,  0.81292911,
			     -3.02720998, -5.78367567,  4.1572359 ,
			     15.82973195])
	       yerr: array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,
	                     0.])
	    covfunc: <function se at 0x7f3c122ba840>
		hyp: array([ 69.02616448,  37.80109995])
	   K_xtrain: array([[ 6.90261645e+01, 5.46607283e+01, 2.71431615e+01,
	                      8.45218104e+00, 1.65044287e+00, 2.02094989e-01,
			      1.55179353e-02, 7.47197956e-04, 2.25611253e-05,
			      4.27178299e-07],
			    [ 5.46607283e+01, 6.90261645e+01, 5.46607283e+01,
			      2.71431615e+01, 8.45218104e+00, 1.65044287e+00,
			      2.02094989e-01, 1.55179353e-02, 7.47197956e-04,
			      2.25611253e-05],
			    [ 2.71431615e+01, 5.46607283e+01, 6.90261645e+01,
			      5.46607283e+01, 2.71431615e+01, 8.45218104e+00,
			      1.65044287e+00, 2.02094989e-01, 1.55179353e-02,
			      7.47197956e-04],
			    [ 8.45218104e+00, 2.71431615e+01, 5.46607283e+01,
			      6.90261645e+01, 5.46607283e+01, 2.71431615e+01,
			      8.45218104e+00, 1.65044287e+00, 2.02094989e-01,
			      1.55179353e-02],
			    [ 1.65044287e+00, 8.45218104e+00, 2.71431615e+01,
			      5.46607283e+01, 6.90261645e+01, 5.46607283e+01,
			      2.71431615e+01, 8.45218104e+00, 1.65044287e+00,
			      2.02094989e-01],
			    [ 2.02094989e-01, 1.65044287e+00, 8.45218104e+00,
			      2.71431615e+01, 5.46607283e+01, 6.90261645e+01,
			      5.46607283e+01, 2.71431615e+01, 8.45218104e+00,
			      1.65044287e+00],
			    [ 1.55179353e-02, 2.02094989e-01, 1.65044287e+00,
			      8.45218104e+00, 2.71431615e+01, 5.46607283e+01,
			      6.90261645e+01, 5.46607283e+01, 2.71431615e+01,
			      8.45218104e+00],
			    [ 7.47197956e-04, 1.55179353e-02, 2.02094989e-01,
			      1.65044287e+00, 8.45218104e+00, 2.71431615e+01,
			      5.46607283e+01, 6.90261645e+01, 5.46607283e+01,
			      2.71431615e+01],
			    [ 2.25611253e-05, 7.47197956e-04, 1.55179353e-02,
			      2.02094989e-01, 1.65044287e+00, 8.45218104e+00,
			      2.71431615e+01, 5.46607283e+01, 6.90261645e+01,
			      5.46607283e+01],
			    [ 4.27178299e-07, 2.25611253e-05, 7.47197956e-04,
			      1.55179353e-02, 2.02094989e-01, 1.65044287e+00,
			      8.45218104e+00, 2.71431615e+01, 5.46607283e+01,
			      6.90261645e+01]])
	 K_x_xtrain: array([[  2.05085779,  9.93954929, 30.20794494,
	                      57.57036918, 68.80186203, 51.56140154,
			      24.23106931,  7.14074937,  1.31958796,
			       0.15291717]])
		K_x: array([ 0.00031421])
      K_xtrain_cond: 2934.9963815557398
	      alpha: array([  0.45929272, -0.95810644,  1.18947654,
	                     -1.23180611,  1.26209391, -1.42601332,
			      1.70634775, -1.65324604,  0.78024104,
			      0.08330017])
               
We may compare the predicted value (attribute ``y``) to the true value.

.. sourcecode:: python
		
   >>> mim.forrester(x)
   0.31905288120742992

Furthermore, we may compute the one-sigma confidence region for the
prediction, as follows.

.. sourcecode:: python

   >>> result = mim.gpe(x, xtrain, ytrain)
   >>> sigma = numpy.sqrt(result.var)
   >>> result.y + (- sigma, sigma)
   array([ 0.30789635,  0.34334829])

The true value is well inside this confidence region.

We are not limited to computing one prediction at a time. In fact, we may
compute any number of predictions.  Let us generate 50 predictions, spaced
evenly across the function's domain.

.. sourcecode:: python
		
   >>> x = np.linspace(0.0, 1.0)
   >>> result = mim.gpe(x, xtrain, ytrain)
   >>> result.y
   array([  3.02720998e+00,   2.02330280e+00,   1.10733656e+00,
	    3.30166619e-01,  -2.73581102e-01,  -6.88585000e-01,
	   -9.19285922e-01,  -9.87695727e-01,  -9.28961199e-01,
	   -7.85452535e-01,  -6.00364032e-01,  -4.11845157e-01,
	   -2.48522267e-01,  -1.26965776e-01,  -5.12769410e-02,
	   -1.45980727e-02,  -2.06661893e-03,   5.41381852e-03,
	    2.72140499e-02,   7.93447797e-02,   1.71485234e-01,
	    3.04843473e-01,   4.70905240e-01,   6.51097577e-01,
	    8.17430270e-01,   9.34237544e-01,   9.61164198e-01,
	    8.57471036e-01,   5.87549211e-01,   1.27248359e-01,
	   -5.29701407e-01,  -1.36616944e+00,  -2.33813403e+00,
	   -3.37426324e+00,  -4.37927809e+00,  -5.24126533e+00,
	   -5.84243871e+00,  -6.07218465e+00,  -5.84070940e+00,
	   -5.09134895e+00,  -3.80967769e+00,  -2.02795765e+00,
	    1.75860302e-01,   2.68455988e+00,   5.35267745e+00,
	    8.02062752e+00,   1.05300496e+01,   1.27384064e+01,
	    1.45310857e+01,   1.58297319e+01])

Now plot these predictions along with the true values of the function. First,
plot the predictions.

.. sourcecode:: python

   >>> import matplotlib.pyplot as plt
   >>> plt.set_xlabel('$x$')
   >>> plt.set_ylabel('$y$')
   >>> plt.plot(result.x, result.y)

Second, plot true values of the function, along with the training
data.

.. sourcecode:: python

   >>> y = mim.forrester(x)
   >>> plt.plot(x, y)
   >>> plt.scatter(xtrain, ytrain)

Also plot the one-sigma confidence region.

.. sourcecode:: python

   >>> sigma = numpy.sqrt(result.var)
   >>> plt.fill_between(x, result.y + sigma, result.y - sigma)
   >>> plt.show()

.. figure:: forrester.jpg
   :figwidth: 50%
   :align: center

The standard deviation is so small that it is hidden by the width of the
line. 


Basic use of efficient global optimization
------------------------------------------

Now let us find the global minimum of the Forrester function using EGO. Note
that the Forrester function has two local minima, only one of which is global.

Generate the training data, as before.

.. sourcecode:: python

   >>> import numpy as np
   >>> import pymimic as mim
   >>> xtrain = np.linspace(0.0, 1.0, 10)
   >>> ytrain = mim.testfunc.forrester(xtrain)

Now perform the optimization. This returns an
``scipy.optimize.OptimizeResult`` object, just as Scipy's optimization
routines do. The objective function must return an error along with a
value. For the Forrester function we may enable this by setting the keyword
``error`` to ``True``. We may pass such keywords to the objective function
using the keyword ``args`` in :func:`pymimic.ego`.

.. sourcecode:: python

   >>> mim.ego(mim.testfunc.forrester, ((0., 1.),), xtrain, ytrain, args=(True,))
   fun: -6.0193230848474188
     x: array([ 0.75561598])

The attributes ``x`` and ``fun`` give the location of the minimum and its
value. Compare these with the true location and minimum, 0.75724876 and
-6.02074006. We may increase the accuracy of the result by reducing the
tolerance (keyword ``tol``), which is set to 0.001 by default.


References
----------
Forrester, A., Sobester, A., and A. Keane, A. 2008. *Engineering design via
surrogate modelling: a practical guide*. Chichester: Wiley.
