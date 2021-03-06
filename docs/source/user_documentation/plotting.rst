.. _plotting:

Plotting
========

The function :func:`pymimic.plots.marginal` creates a matrix of one- and
two-dimensional projections of a multivariate function.

Let us use it to plot projections of the Branin function. First, generate the
data.

>>> import numpy as np
>>> import pymimic as mim
>>> x0 = np.linspace(-5., 10., 25)
>>> x1 = np.linspace(0., 15., 25)
>>> x = [(xi, xj) for xi in x0 for xj in x1]
>>> y = [mim.testfunc.branin(xi) for xi in x]
>>> y = np.array(y).reshape(25, 25)

Second, generate the plot.

>>> import matplotlib.pyplot as plt
>>> fig = mim.plot.marginal(y, (x0, x1))
>>> plt.show()

.. figure:: branin_marg.jpg
   :figwidth: 100%
   :align: center
