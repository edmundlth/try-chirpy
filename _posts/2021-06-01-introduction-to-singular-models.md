---
title: Introduction to Singular Models
tags: [singular-learning-theory]
categories: [algebraic-geometry, machine-learning]
description: 
date: 2021-06-01
math: true
---

Introduction {#sec:introduction}
================

Let's first set the context. Imagine we are given a data generating
process $$q(x)$$ where we can ask for $$N \in { {\mathbb N}}$$ samples [^1], $$D_N = \{ X_1, \dots, X_N \}$$. Our goal is to *learn* a distribution $$p(x)$$ from which we can make inferences about
the data generating process itself.

-   **Deterministic data**
    If $$q$$ generates the result of "$$1 + 1$$", we can set $$p(x_1) = 1$$
    where $$x_1 = 2$$ is the first "measurement". Here the learning
    process recover everything we wish to know about $$q$$ just from the
    first data point, i.e. $$p = q$$. As such, there is no reason to
    deviate from this learning process.

-   **Deterministic with measurement error**
    If $$q$$ generates the results of ballot count by humans, the above
    learning process would still be reasonable, but we should perhaps
    account for human error. We could, for instance, ask for lots of
    recount set $$p(\text{most frequently occuring count}) = 1$$. Or
    perhaps a deterministic result doesn't sit well with us when we know
    that error can occur, we can set $$p(x) =$$ proportion of recount that
    turns out to be $$x$$.

-   **Experiments in empirical science**
    If $$q$$ generates experimental measurements of physical quantities
    $$(x, y)$$ that is governed by some law of nature $$y = f_\alpha(x)$$
    that depends on some parameter $$\alpha$$ and experimental
    measurements is marred by (normally distributed) random error, then
    we have $$Y_i - f_\alpha(X_i) \sim N(0, \sigma)$$. The value of
    $$\alpha$$ can be estimated given a learnt model $$p$$.

-   **Generalised Linear Models**

-   **AI agents**

-   etc [^2]

In general, we instantiate a large space of *hypothesis*,
$$\Delta = \left\{p = p(x|\omega) \, \, : \,\omega \in \Omega\right\}$$,
parametrised by $$\omega \in \Omega \subset { {\mathbb R}}^d$$
equipped with a prior $$\varphi(\omega)$$ and cast the learning process as
an *optimisation procedure* that finds the best hypothesis that explains
the observed samples. One way to define "best" is to select $$p$$ that
minimises the *Kullback-Leibler divergence* between $$q$$ and $$p$$, i.e.
choose $$p(x) = p(x | w^*)$$ such that $$w^*$$ minimises $$\begin{aligned}
    K(w) = { {\mathbb E}}_q\left[\log \frac{q(x)}{p(x| \omega)}\right] = \int_X q(x) \log \frac{q(x)}{p(x| \omega)}dx.\end{aligned}$$
We will investigate the properties of learning machine of this form.\
Properties of a learning machine that we might care about:

-   Error rate. Generalisation. Generalisation gap:
    $$B_g, G_g, B_t, G_t$$.

-   Data efficiency. Compute efficiency. Behaviour in overparametrised
    regime. Scaling laws. Double descent.

-   Training behaviour. Stochastic noise.

**Real Log Canonical Threshold**
================================

For a given statistical model $$(p(x|\omega), q(x), \varphi(\omega))$$,
the following are equivalent definitions for its real log canonical
threshold (RLCT), $$\lambda$$ and its order $$\theta$$.

1.  **Largest pole of zeta function of $$K$$**\
    Define the zeta function
    $$\zeta: { {\mathbb C}}\to { {\mathbb C}}$$ of
    $$K$$ as[^3] 
    
    \begin{aligned}
        \zeta(z) = \int_\Omega K(w)^z \varphi(\omega)d\omega. 
    \end{aligned}
    
    
    The RLCT $$\lambda$$ is the largest pole of
    $$\zeta$$ and $$\theta$$ the order of the pole at $$\lambda$$.

2.  **Convergence rate of Laplace integral of $$K$$**\
    $$(\lambda, \theta)$$ governs the asymptotic behaviour as
    $$n \to \infty$$ of the Laplace integral[^4]: $$\begin{aligned}
                \int_\Omega \exp\left(-nK(\omega)\right)\varphi(\omega) d\omega \stackrel{n \to \infty} \sim Cn^{-\lambda}\left(\log n\right)^{\theta -1}
            \end{aligned}$$ for some positive real constant $$C$$.

3.  **Convergence rate of free energy**\
    Taking the negative logarithm of the previous asymptotic expression
    gives[^5] $$\begin{aligned}
                \log \int_\Omega \exp\left(-nK(\omega)\right)\varphi(\omega) d\omega \stackrel{n \to \infty} \sim \lambda \log n - \left(\theta -1\right) \log \log n + O(1). 
            \end{aligned}$$

4.  **Asymptotic expansion of density of states near $$W_0$$**\
    The density of state $$\begin{aligned}
                v(t) = \int_\Omega \delta\left(t - K(\omega)\right) \varphi(\omega) d\omega
            \end{aligned}$$ has asymptotic expansion as $$t \to 0$$
    $$\begin{aligned}
                v(t) \sim C t^{\lambda -1} (- \log(t))^{\theta -1} 
            \end{aligned}$$ for some positive real constant $$C$$.[^6]

5.  **Volume codimension $$W_0$$** $$\begin{aligned}
                \lambda = \lim_{t \to 0^+} \log_a \frac{V(at)}{V(t)}
            \end{aligned}$$ where $$1 \neq a > 0$$ and
    $$V: { {\mathbb R}}_{\geq 0} \to { {\mathbb R}}_{\geq 0}$$
    is the volume measure of neighbourhoods of $$W_0$$ $$\begin{aligned}
                V(t) = \int_{K(w) < t} \varphi(\omega) d\omega. 
            \end{aligned}$$

6.  **From resolution of singularity**\
    Hironaka's resolution of singularity for the real analytic function
    $$K(\omega)$$ gives us a proper birational map[^7] $$g: U \to \Omega$$
    such that in the neighbourhood of $$\omega_0 \in W_0$$, the zero set
    of $$K$$ $$\begin{aligned}
                K(g(u) - \omega_0) &= u^{2k} = u_1^{2k_1}u_2^{2k_2} \dots u_d^{2k_d}\\
                g'(u) &= b(u)u^h = b(u)u_1^{h_1}u_2^{h_2} \dots u_d^{h_d}
            \end{aligned}$$ for some
    $$u, k \in { {\mathbb N}}^d$$ and analytic $$b(u) \neq 0$$.
    We then have $$\begin{aligned}
                \lambda = \inf_{\omega \in W_0} \min_{1 \leq j \leq d}\frac{h_j + 1}{2k_j}
            \end{aligned}$$ and $$\theta$$ is given by the number of times
    the above minimum is achieved.[^8]

7.  **RLCT of ideals of analytic functions**\
    TODO: there are various square roots involved in this that I don't
    really understand \...

**Toy Model: Coin Flip**
========================

Following a long tradition of first example in statistical pedagogy, we
will investigate coin flips: a single Bernoulli random variable
$$\left\{H, T\right\} \ni x \sim Bernoulli(\omega)$$ parametrised by a
single variable
$$\omega \in [0, 1] = \Omega \subset { {\mathbb R}}$$. In this
case, we have $$\begin{aligned}
    p(x| \omega) &= \begin{cases}
        \omega & x = H\\
        1 - \omega & x = T
    \end{cases}\\
    q(x) &= p(x| \omega_0)\\    
    K(\omega) &= \omega_0 \log \omega_0 + (1 - \omega_0) \log (1 - \omega_0) - \omega_0 \log \omega - (1 - \omega_0) \log (1 - \omega)\\
    K_N(\omega) &= \hat{\omega}_{MLE} \log \frac{\omega_0}{\omega} + (1 - \hat{\omega}_{MLE}) \log \frac{1 - \omega_0}{1 - \omega}, \quad \hat{\omega}_{MLE} = \frac{\# H}{N} \\
    I(\omega) &= \frac{1}{\omega( 1 - \omega)} > 0 \quad \forall \omega \in (0, 1)\end{aligned}$$
where the last expression is the (positive definite) Fisher information
matrix[^9]. This is clearly a regular model. Assuming uniform prior
$$\varphi = 1$$, the Laplace integral can be computed exactly as
$$\begin{aligned}
    L(n) 
    &= \int_0^1 \exp\left(-nK(\omega)\right)d\omega \\
    &= \left(\omega_0^{\omega_0}(1 - \omega_0)^{1 - \omega_0}\right)^{-n} \int_0^1 \omega^{n \omega_0}(1 - \omega)^{n(1 - \omega_0)}d\omega\\
    &= C(\omega_0)^{-n}B(n \omega_0 + 1, n(1 - \omega_0) + 1)\end{aligned}$$
where $$B(x,y) = \frac{\Gamma(x)\Gamma(y)}{\Gamma(x + y)}$$ is the beta
function[^10]. If the truth is that the coin is fair[^11], using
Stirling's approximation for $$\Gamma$$, we have $$\begin{aligned}
    L(n) 
    &\sim \frac{1}{8\sqrt{\pi}} \left(\frac{n}{2} + 1\right)^{-\frac{1}{2}}\end{aligned}$$
which tell us that the RLCT $$= 1/2$$ as expected for a regular model. The
posterior and Bayesian predictive distribution is given by[^12]
$$\begin{aligned}
    p(\omega | D_N) &= \frac{\omega^{\# H} ( 1 - \omega)^{N - \# H}}{B(\# H + 1, N - \# H + 1)} \sim \text{Beta distribution}\\
    p(x| D_N) &= \frac{1}{B(\# H + 1, N - \# H + 1)}\begin{cases}
        B(\# H + 2, N - \# H + 1) & x = H\\
        B(\# H + 1, N - \# H + 2) & x = T. 
    \end{cases}\end{aligned}$$ These can be simplified into binomial
coefficients. Finding the analytic continuation for the zeta
function[^13] $$\begin{aligned}
    \zeta(z) = \int_0^1 \left(-\log 2 - \frac{1}{2}\log \omega(1 - \omega)\right)^z d\omega\end{aligned}$$
is not trivial even for the $$\omega_0 = 1/2$$ case. Though this analysis
generalised to any finite number of discrete variables[^14], analytic
expression of quantities of interest are both hard to write down and
hard to compute[^15]. I suppose a lesson here is that Bayesian
statistics is HARD.

**RLCT for Regular Models** {#sec:regular models}
===========================

The RLCT of a regular realisable model is given by
$$\lambda = \frac{d}{2}$$ and $$\theta = 1$$.

We shall use the Laplace integral characterisation of RLCT. We want to
show that
$$Z^0_n = \int_\Omega \exp\left(-nK(\omega)\right) \varphi(\omega) d{\omega} \sim C n^{-\frac{d}{2}}$$
for some positive constant $$C$$ as $$n \to \infty$$.\
Since the model is realisable and identifiable, it has unique minimum at
$$\omega^* \in \mathrm{supp}(\varphi)$$. Taylor expansion of $$K$$ centered
around $$\omega^*$$ up to order 2 gives $$\begin{aligned}
        K(\omega) 
        &= K(\omega^*) + \nabla K(\omega^*) \cdot (\omega - \omega^*) + \frac{1}{2} (\omega - \omega^*)^T \nabla^2K(\omega^*)(\omega - \omega^*)  + O(\left|\, \omega - \omega^* \,\right|^3)
    \end{aligned}$$ where $$\nabla^2K(\omega^*)$$ is the Hessian of $$K$$ at
$$\omega^*$$. That $$\omega^*$$ realises the true model and is a local
minimum gives us $$K(\omega^*) = 0$$ and $$\nabla K(\omega^*) =0$$, reducing
the above to $$\begin{aligned}
        K(\omega) &= \frac{1}{2} (\omega - \omega^*)^T \nabla^2K(\omega^*)(\omega - \omega^*)  + O(\left|\, \omega - \omega^* \,\right|^3). 
    \end{aligned}$$

Substituting the above into the integral, we get, in the limit as
$$n \to \infty$$ $$\begin{aligned}
        Z^0_n 
        &\sim \int_\Omega \exp\left(-\frac{n}{2} (\omega - \omega^*)^T \nabla^2K(\omega^*)(\omega - \omega^*) \right) \varphi(\omega) d{\omega} 
    \end{aligned}$$ which we recognise as a Gaussian integral with
precision matrix $$n \nabla^2K(\omega^*)$$ which is positive definite by
assumption. Therefore, we conclude that $$\begin{aligned}
        Z^0_n 
        \sim \varphi(\omega^*)\sqrt{\frac{(2\pi)^d}{\det\left(n \nabla^2K(\omega^*)\right)}} 
        = \varphi(\omega^*)\sqrt{\frac{(2\pi)^d}{\det\left(\nabla^2K(\omega^*)\right)}} n^{-\frac{d}{2}}.
    \end{aligned}$$

We shall use the characterisation that for any positive $$a \neq 1$$,
$$\lambda = \lim_{t \to 0^+} \log_a \frac{V(at)}{V(t)}$$ where $$V$$ is the
volume function $$\begin{aligned}
        V(t) = \int_{K(\omega) \leq t} \varphi(\omega) d\omega. 
    \end{aligned}$$ By regularity assumption, we have that $$\omega^*$$ is
a non-degenerate critical point of $$K$$ and hence by Morse lemma, there
is a local chart $$x(\omega) = (x_i(\omega))_{i = 1, \dots, d}$$ in a
small enough neighbourhood of $$\omega^*$$ such that
$$K(\omega) = \cancelto{0}{K(\omega^*)} + \sum_i x_i(\omega)^2$$.
Therefore, for small enough $$t > 0$$, $$\begin{aligned}
        V(t) = \int_{\sum_i x_i^2 \leq t} \varphi(x) dx
    \end{aligned}$$ which is proportional to the volume of a
$$d$$-dimensional ball with radius $$\sqrt{t}$$, i.e.
$$V(t) \propto t^{d}$$.[^16] Finally, $$\begin{aligned}
        \lambda = \lim_{t \to 0^+} \log_a \frac{(at)^{d/2}}{t^{d/2}} = \frac{d}{2}. 
    \end{aligned}$$

**Some Singular Models** {#sec:simple singular models}
========================

Normal Mixtures
---------------

$$tanh$$ Perceptron with One Hidden Layer
---------------------------------------

$$\lambda = \frac{\floor{\sqrt{H}}^2 + \floor{\sqrt{H}} + H}{4\floor{\sqrt{H}} + 2}$$

Regularly Parametrised Models
-----------------------------

**Topics and Known Difficulties**
=================================

Characterisations of RLCT
-------------------------

We should give a proof of the equivalent definitions / characterisations
of RLCT. That would involve proof that various statement is independent
of charts / generators etc..

Blow up
-------

Realisability
-------------

From Analytic to Algebraic
--------------------------

[^1]: Throughout, we assume that the process generates i.i.d. samples.
    In particular, $$X_i \sim q$$ for all $$i$$, with $$q$$ unchanging as we
    ask for more samples. However, we note that this is a
    simplification: one would imagine that an (artificial) intelligent
    \"student\" would judiciously ask for more informative examples from
    a \"teacher\" process.

[^2]: TODO: more examples. Contrast different inference tasks.

[^3]: $$\zeta$$ analytically continues to a meromorphic function with
    poles on the negative real axis.

[^4]: which is the deterministic version of the (normalised) evidence
    $$Z^0_n = \int_\Omega \exp\left(-nK_N(\omega)\right)\varphi(\omega) d\omega$$.
    Note that the limiting variable $$n$$ is different from the number of
    training samples $$N$$. This is one place where inverse temperature
    $$\beta$$ can come in: set $$n = \beta k$$.

[^5]: the stochastic version translate as
    $$F^0_n = \lambda \log n - (\theta -1) \log \log n +$$ stochastic
    process of constant order.

[^6]: lots of fixing and clarification needed\...Mellin transform's
    involved.

[^7]: obtained via recursive blow up.

[^8]: This deep result shows that
    $$(\lambda, \theta) \in { {\mathbb Q}}\times { {\mathbb Z}}$$.

[^9]: admittedly not much of a matrix in 1-D.

[^10]: even in this extremely simple case, we still need special
    functions to express quantities of interest.

[^11]: $$\omega_0 = 1/2$$ and $$C(w_0) = 2$$ in this case.

[^12]: the form of the posterior shows that Beta and Bernoulli
    distributions forms a conjugate pair.

[^13]: I haven't been able to do it. The integral is singular near the
    integration terminals. This makes the evaluation using resolution of
    singularity seems magical to me.

[^14]: Beta distribution becomes Dirichlet distribution, binomial
    becomes multinomial etc\...

[^15]: not to mention issues with numerical stability\...

[^16]: Volume of $$d$$-dimensional ball with radius $$r$$ is given by
    $$\frac{\pi^{\frac{d}{2}}}{\Gamma(\frac{n}{2} + 1)}r^d$$.