.. _hyperparameters:

Optimizing the hyperparameters
==============================

Introduction to hyperparameter optimization
-------------------------------------------

Recall from :ref:`gpe` that we must specify a covariance function in order to
perform GPE. We choose this covariance function from a model of such
functions, indexed by a set of *hyperparameters*. We may optimize this
hyperparameter using one of two methods: maximum-likelihood and leave-one-out
maximum-pseudolikelihood.


Maximum-likelihood method
-------------------------

From equation :eq:`sample_distribution` we know the joint PDF of the sampled
random variables is

.. math::

   f_{\boldsymbol{Y}}(\boldsymbol{y}; \boldsymbol{\theta}) =
   \dfrac{1}{\sqrt{(2 \pi)^{N} |\boldsymbol{K}|}} \exp \left(-\dfrac{1}{2}
   (\boldsymbol{y} - \boldsymbol{r})^{\mathrm{t}} \boldsymbol{K}^{-1}
   (\boldsymbol{y} - \boldsymbol{r}) \right)

where the hyperparameter vector is :math:`\boldsymbol{\theta} =
(\boldsymbol{\mu}, \boldsymbol{\nu})`. (Note that :math:`\boldsymbol{K}` and
:math:`\boldsymbol{r}` depend on :math:`\boldsymbol{\theta}`.) By definition,
the likelihood is

.. math::
   
    L_{\boldsymbol{\Theta}}(\boldsymbol{\theta}; \boldsymbol{y}) =
    f_{\boldsymbol{Y}_{N}}(\boldsymbol{y}; \boldsymbol{\theta})

and hence (up to an additive constant),

.. math::
   
    \ln L_{\boldsymbol{\Theta}}(\boldsymbol{\theta}; \boldsymbol{y}) = -
    \dfrac{1}{2} (\boldsymbol{y} - \boldsymbol{r})^{\mathrm{t}}
    \boldsymbol{K}^{-1} (\boldsymbol{y} - \boldsymbol{r}) - \dfrac{1}{2} \ln
    |\boldsymbol{K}|

(Rasmussen and Williams, 2006). To find the maximum-likelihood estimate of
:math:`\boldsymbol{\theta}`, namely :math:`\hat{\boldsymbol{\theta}} =
(\hat{\boldsymbol{\mu}}, \hat{\boldsymbol{\nu}})`, we may maximize this
function subject to the constraint that :math:`\boldsymbol{K}` is
positive-semidefinite (positive-semidefiniteness being a necessary property of
covariance matrices). The equation for :math:`\ln
L_{\boldsymbol{\Theta}}(\boldsymbol{\theta}; \boldsymbol{y})` consists of
three terms. The first is a measure of fit quality, the second is a complexity
penalty, and the third a normalization constant.  The function
:math:`L_{\boldsymbol{\Theta}}` will in general have multiple maxima, each
maximum giving a different tradeoff between fit quality and complexity.


Leave-one-out maximum-pseudolikelihood method
---------------------------------------------

We may alternatively optimize the hyperparameter by maximizing the
leave-one-out cross-validation likelihood (see :ref:`validating`). The idea is
that we omit the :math:`i`-th pair :math:`(\boldsymbol{x}_i,
y(\boldsymbol{x}_i))` from the training data and use this reduced training
data to compute :math:`\hat{r}(\boldsymbol{x}_i)`. We expect that
:math:`\hat{r}(\boldsymbol{x}_i)` is approximately equal to
:math:`y(\boldsymbol{x}_i)`. The logarithm of the likelihood of the
hyperparameter when leaving out the :math:`i`-th pair of data is (up to an
additive constant)

.. math::

   \ln L_{-i}(\boldsymbol{\theta}; \boldsymbol{x}) = -\dfrac{1}{2} \ln
   \hat{\sigma}_{-i}^{2} (\boldsymbol{x}_i) - \dfrac{1}{2} \left(
   \dfrac{y(\boldsymbol{x}_i) -
   \hat{r}_{-i}(\boldsymbol{x}_i)}{\sigma_{-i}(\boldsymbol{x}_i)} \right)^2.

Hence, the logarithm of the joint leave-one-out pseudo-likelihood is

.. math::

   \ln L_\mathrm{LOO}(\boldsymbol{\theta}; \boldsymbol{x}) = \sum_{i = 1}^{N}
   \ln L_{-i}(\boldsymbol{\theta}; \boldsymbol{x}),
   
which we may maximize to give the *maximum-leave-one-out pseudo-likelihood
estimate* of the hyperparameters.

The leave-one-out maximum-pseudolikelihood method of hyperparameter
optimization may make the result of GPE more robust against
model-misspecification (Bachoc 2013).

References
----------

Bachoc F. 2013. `Cross validation and maximum likelihood estimations of
hyper-parameters of Gaussian processes with model misspecification' in
*Computer statistics & data analysis* 66, 55--69.

Rasmussen, C.E., and K.I. Williams. 2006. *Gaussian processes for machine
learning*. Cambridge, MA: MIT Press.

