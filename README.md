# KUKA Inverse Dynamics Data
kuka inverse dynamics datasets that was used in the paper "Incremental Local Gaussian Regression"

```
@incollection{NIPS2014_5594,  
title = {Incremental Local Gaussian Regression},  
author = {Meier, Franziska and Hennig, Philipp and Schaal, Stefan},
booktitle = {Advances in Neural Information Processing Systems 27},
editor = {Z. Ghahramani and M. Welling and C. Cortes and N. D. Lawrence and K. Q. Weinberger},
pages = {972--980},
year = {2014},
publisher = {Curran Associates, Inc.},
url = {http://papers.nips.cc/paper/5594-incremental-local-gaussian-regression.pdf}
}
```

# Data Description

total 28 dimensions
input dimensions 1:21 (7 joints x (pos,vel,acc))
output dimensions 22:28 (torques of the 7 joints)

NOTE:
- data does not necessarily need to lie on a trajectory from beginning to end.
- kuka_sim collected by Franziska Meier using SL simulation software (http://www-clmc.usc.edu/publications/S/schaal-TRSL.pdf)
- kuka1 and kuka2 collected by Jeannette Bohg at MPI for Intelligent Systems

The inputs/outputs are normalized. The outputs are normalized to be roughly between -1 and 1 
(not exactly though, basically we roughly know what the maximum torques are per joint, and we normalize by that). 
The numbers with which I have normalized the outputs:

Kuka_real data:  
`norm_out = [40.0, 40.0, 20.0, 20.0, 4.0, 4.0, 2.0]`

Kuka_sim data (In simulation everything is a bit different :) ):
`norm_out = [50, 70.0, 20, 20, 4,  4.0, 2.0]`

So, to get the torque MSE error in original torque space for joint 1 (real data),
 one has to multiple the achieved prediction error with 40^2, for joint 2 (real data) with 40^2, joint 3 with 20^2, and so on.

### Splits used in the NeurIPS paper and what was tested

for the 2 kuka_real datasets we have offline training (because I-SSGPR requires it) that we use to get an initial model. 
During online training one data point at the time to is used to update the models. We check at the end how well we have 
adapted to the new data by testing on it. We do this because after the offline training phase we might be doing well
 on the offline data, but not so well on the online training data set. This basically shows how well we can do on a 
 specific batch of data, after adapting to it incrementally. 
 
On the Kuka_sim data we have no offline training phase and just start with incremental training, here we have held out some data 
to test on during the incremental learning process - the test data in this case has been randomly sampled from all the data.
This shows us how fast we adapt to new data.