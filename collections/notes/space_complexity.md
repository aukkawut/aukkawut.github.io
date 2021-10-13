# How much data do we need for machine learning?

This note is under construction

## Problem statement

Let say we have a dataset $$\mathcal{D} = \{\mathbf{x}_1,\mathbf{x}_2,\dots,\mathbf{x}_n\}$$. What would be the optimal $$n$$ such that for a given model $$\mathcal{M}:\mathcal{X}\rightarrow\mathcal{Y}$$ will have its error measurement (i.e MSE) less or equal to $$\varepsilon$$?

## Simple idea

Let say we start with probably approximately correct (PAC) model and let say our *instance space* is bernuolli trial outcome $$\{0,1\}$$. The goal is to find the approximately correct hypothesis from the sample that the *learning agent* see and infer that hypothesis to find the distribution of the data.
