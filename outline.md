# Introduction
## Literature review
## Contribution

# Preliminaries

## Notation

## Concepts in Category Theory

## Markov Transition Kernels

Markov transition kernels represent functions taking \emph{elements} from X to \emph{distributions} on Y.
This fact isn't often emphasized in engineering literature, but it is an important one.
Kernels are often mistakenly thought to take distributions to distributions.
This confusion is often exacerbated by a confusion between random variables and distributions themselves.

In this framework, where we try to provide an alternative and \emph{precise} description of uncertainty, we will try to move away from discussing random variables and only formulate things in terms of distributions.

\newcommand{\reals}{\mathbb{R}}
\newcommand{\naturals}{\mathbb{N}}
\newcommand{\gauss}{\mathcal{N}}
\newcommand{\prob}{\mathfrak{P}}

Say we have a state space that we call $X$.
We must emphasize, this could be any kind of space, be it a finite set, generic set, a vector space, a manifold, a function space, or some other exotic family of elements.
In building a representation for this framework, one necessity will be to specify the family of state spaces and to be clear about how they are represented in code.
Many problems model their spaces as a vector space, where the family under consideration is the collection $\{\reals^n:n\in \naturals\}$. 
Later we will try to demonstrate how the family of differentiable manifolds applies as well.

Given this space $X$, we then want to specify a unique set that represents a family of distributions on $X$.
In this paper, we will typically give it a letter in either mathcal or fraktur that relates it to $X$, such as $\prob(X)$ or $\gauss(X)$.
This is different than random variables, which are often somewhat viewed either as functions from a probability space to $X$ or informally as elements of $X$ that have a different value following a probability distribution every time they're observed.
Instead, elements of $\prob(X)$ straight up are distributions on $X$.

The word distribution here is loose.
This can mean probability disribution, or probability measure, but doesn't have to.
Simply put, a distribution on $X$ is *some* representation of someone's uncertainty in what a value in $X$ could be.
When talking about state spaces of dynamical systems, a distribution on the state space could be some representation of uncertainty in a system's state.
A distribution could well be a probability measure in the measure theoretic sense, meaning if $X$ represents the measurable space $(\Omega, \Sigma)$, then $\prob(X)$ could be the family of probability measures $\{\mu : \Sigma \rightarrow \reals\}$.
Or, if $X$ is a vector space $\reals^n$, then the distribution family could be $\gauss^n$.
As stated, however, distributions are not restricted to probability.
They could be outer probability measures, or possibility.
For instance, a distribution on $X$ could just be a subset $D\subset X$, indicated that the state has the potential to exist anywhere within this subset.
Or, a distribution could be an outer probability measure \cite{delande}, meaning 

## Markov Categories

For our purposes, Markov categories simply give the minimum set of axioms required for Markov transition kernels to behave the way we expect.
To fully describe a framework of stochastic kernels (and to encode them in our library), one must specify a family of state spaces, specify a parametrization of kernels between these spaces, and finally encode the axioms to describe the behavior of both.

Before reviewing the axioms in detail, we'll list them here:

* The family of state spaces must form a symmetric monoid. This means that:
  * For two state spaces $X$ and $Y$, there must be a joint space $X\oplus Y$
  * There must be a singleton space $I$ that contains one element
  * $X\oplus Y$ must be isomorphic to $Y\oplus X$
* Each space must be a comonoid, meaning:
  * Each space must have a duplication function $\Delta_X: X\rightarrow X\oplus X, x\mapsto (x,x)$
  * Each space must have a deletion function $\delta_X: X\rightarrow I$
* Stochastic kernels must form a category, meaning:
  * Kernels must compose, if their types line up, as specified through uncertainty propagation
  * Every state space must have an identity kernel
  * Every kernel must have conditionals. We will go into detail with this later.

There is an elegence to these axioms that must be appreciated.
Monoids seem to show up everywhere in category theory, and here is no exception.

### Review of Monoids

As a review, a monoid is a set $M$ with a single operation, $\oplus: M\times M \rightarrow M$. The operation must be associative, ie. $(a\oplus b) \oplus c = a \oplus (b\oplus c)$, and $M$ must have an identity element $e$ that cancels with $\oplus$ on either side, ie. $e\oplus a = a\oplus e = a$.
For the monoid to be symmetric, we simply mean that $\oplus$ has the extra property that $a\oplus b = b\oplus a$.

### The Monoid of State Spaces
For the family of state spaces, the rules are only slightly more generalized, but this causes some nuance:
For any two state spaces $X$ and $Y$, there must be a third state space that represents $X\oplus Y$, the space of joint distributions 

The most basic way to combine blocks is to compose them.

GIVE EXAMPLE WITH x_k = Fx_k-1 + v AND y_k = Hx_k + w

Edges that are side-by-side represent joint spaces, and blocks side-by-side represent their independent tensor product.
On that note, an edge can represent either a state space or its identity kernel.

GIVE EXAMPLE WITH ABOVE KERNELS

## Problem Formulation

## An explanation of string diagrams

String diagrams are the primary visual tool for manipulating objects in Markov categories.
These diagrams have many benefits, one being that visual representations are much more intuitive to work with than equations.
These string diagrams also prove to be powerful, as they can represent generic algorithms that apply to several different representations of probability.

String diagrams work much like block diagrams, ie. signal flow diagrams in that nodes represent transformations or functions (in most cases Markov kernels), whereas edges represent state spaces.

Remark: In fact, Baez contains a thorough exposition of the links between signal flow and string diagrams.
This paper, which isn't thoroughly cited, contains a treasure trove of new tools that unify signal flow and strings.
The paper treats categories as representing continuous time signals that can be added, but the categorical notions are nearly equivalent to those constructed for Markov categories.
These new string diagram constructs have not yet been defined in the categorical probability literature, so we believe that there is much to be explored here in terms of bringing more controls intuition into the Markov framework to be unified with stochastic processes as for instance defined in Fritz.
Two particularly useful tools are the "cups" and "caps," which tie controller state feedback into the string diagram interpretation.

### Interpreting String Diagrams

The details of string diagrams are explained extensively in Cho and Jacobs.
We give a recap of their elements here.

Fundamentally, a string diagram represents a composition of distribution and transitions that form a new compound function or distribution.
Depending on the convention used, they often are to be read from bottom to top, or sometimes there are arrowheads used to indicate their direction.
In this paper we will use arrow heads.

Similar to how the edges of a signal flow diagram represent the "wires" that carry the signal, the edges in a string diagram represent the state space or configuration space that carries the state.
Nodes, often drawn as blocks, represent transitions.

Note: in this way string diagrams can kind of act like the "dual" to commutative diagrams in which nodes are state spaces and edges are morphisms.

### Translating Diagrams into Functions

# Proposed Solution

## General Framework
The overarching organization to this framework in python involves 3 sections: abstract category class, info-representations, and abstract algorithms.

## Potential Representations of Information

### Gaussian
Gaussian probability has already been extensively studied in this framework, both by Fritz, Cho & Jacobs, and the generic filter algorithm has been demonstrated to resolve to a Kalman filter in my girypy library.
We review the basics here.

### Unscented Approximations of Nonlinear Models

Here we will shall extensively develop the adaptation of unscented transform theory to this framework.
We use the results developed by Menegaz et al because in general this framework requires and encourages a heavy systematization of any formulation in a rigorous sense.
We find that Menegaz sufficiently formalizes and systematizes the wide range of UT papers varying in rigor.

There is potential to go beyond vector spaces using this framework and to boost it into Riemannian manifold spaces.
This is a topic for future development.

### Gaussian Mixtures
Gaussian mixture models can potentially be composed as compositional data types from Extended Gaussian and finite stochastic constructions.
What do I mean here?
Think about this: a discrete distribution is just a weighted list of points. But a gaussian mixture is a weighted list of gaussians.
Could we somehow compose these types? Could we set up a discrete framework as some kind of type transformer or some kind of Markov functor?
There's a paper by some dude that talks about monad transformers in this way, and I would like to look into him more.

## Potential Generic Algorithms

### Recursive estimation filter

~~~
prediction = dynamics @ prior
joint = (instrument & id(x)) @ copy(x) @ prediction
updater = condition(joint, y)
posterior = updater @ measurement
~~~

### Batch filter
### Stochastic control

There is much potential to simplify this construction given Fritz and Baez.


# Simulation Results

# Conclusion
## Drawbacks
## Future Work
