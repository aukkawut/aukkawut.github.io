# How much data do we need for machine learning?

This note is under construction

## Problem statement

Let say we have a dataset $$\mathcal{D} = \{\mathbf{x}_1,\mathbf{x}_2,\dots,\mathbf{x}_n\}$$. What would be the optimal $$n$$ such that for a given model $$\mathcal{M}:\mathcal{X}\rightarrow\mathcal{Y}$$ will have its error measurement (i.e MSE) less or equal to $$\varepsilon$$?

## Simple idea

Let say we start with probably approximately correct (PAC) model and let say our *instance space* is bernuolli trial outcome $$\{0,1\}$$. The goal is to find the approximately correct hypothesis from the sample that the *learning agent* see and infer that hypothesis to find the distribution of the data.

Ok, it might be hard to take in so let say our sample data is the length (in pages) of the paper that is needed to read each day that we asked couple of data science students who take the similar classes. We then ask them to label their length that is it too long or not. The result is as followed.

| Value | Label |
|-------|-------|
| 13    | 0     |
| 10    | 0     |
| 25    | 1     |
| 24    | 1     |
| 15    | 0     |
| 21    | 1     |
| 22    | 1     |
| 16    | 0     |
| 14    | 0     |
| 28    | 1     |

Let say the "too long" tag is 1 and else is 0. We can see here that if we want to cut out the pages that would *probably* be classified as "too long" it would be around 20 more or less. I know nothing about the data but I can say it with some confident that the probability that the class "else" is being classified to the data point $$x>20$$ is almost zero given this sample point. But how can we decide the *hypothesis* out of this observation?
