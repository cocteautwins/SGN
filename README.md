# Stochastic Generalized Gauss-Newton (SGN) Method for Traning Deep Neural Networks

By [Matilde Gargiani](https://scholar.google.com/citations?user=gejXFzwAAAAJ&hl=en).

Albert-Ludwigs-Universität Freiburg.

### Table of Contents
0. [Introduction](#introduction)
0. [Why Theano](#why-theano)
0. [Citation](#citation)
0. [Installation](#installation)
0. [Usage](#usage)
0. [Algorithm](#algorithm)
0. [Results](#results)

### Introduction
This repository contains an efficient and flexible implementation of SGN method for training deep neural networks as described in the paper "Stochastic generalized Gauss-Newton method (SGN): an efficient stochastic second-order method for deep learning" (http://arxiv.org/abs/TODO). 

**Note**

0. 

### Why Theano  
Despite the undisputable popularity of Tensorflow and Pytorch as deep learning frameworks, only Theano allows an efficient and fully optimized computation of some operations such as Jacobian-vector, vector-Jacobian and Hessian-vector products (see the Theano awesome documentation at http://deeplearning.net/software/theano/tutorial/gradients.html). Since the numerical performance of SGN highly relies on the computational efficiency of such operations, here we go with a Theano implementation! :stuck_out_tongue_winking_eye:

### Citation
If you use this code or this method in your research, please cite:


	@article{Gargiani2018,
		author = {Matilde Gargiani and Andrea Zanelli and Frank Hutter and Moritz Diehl},
		title = {Stochastic generalized Gauss-Newton method (SGN): an efficient stochastic second-order method for deep learning},
		journal = {arXiv preprint arXiv:TODO},
		year = {2018}
	}
	
### Installation
0. Clone the repository on your machine.
0. Create a virtual environment for python with conda, e.g. ```conda create -n sgn_env python=3.6 anaconda```, and activate it, e.g.  ```source activate sgn_env```.
0. Access the cloned repository on your machine with ```cd <root>/SGN```.
0. Run the following command ```python setup.py install```.
0. If you have access to a gpu and want to use it to speedup the benchmarks, also run the following command ```conda install pygpu=0.7```.


**Note**

To check your installation, open a terminal from your conda environment, access the folder ``` scripts``` and type the following command: 
```python mlp.py --dataset mnist --layers 20 20 20 --solver SGN --rho 0.0001 --cg_min_iter 3 --cg_max_iter 3 --f res_mnist --verbose 1 --epochs 5```.


### Usage

The python files ```mlp.py```, and ```conv_net.py``` available in the folder ```scripts``` offer an example of how to use this package. 

### Algorithm
Please have a look at our paper http://arxiv.org/abs/TODO for a full mathematical description of SGN. The current implementation includes also the possibility of using backtracking line search to automatically adjust the step-size (see Algorithm 3.1 in http://bme2.aut.ac.ir/~towhidkhah/MPC/Springer-Verlag%20Numerical%20Optimization.pdf) and/or using a trust region approach to automatically adapt the Levenberg-Marquardt regularization parameter (see Algorithm 4.1 in http://bme2.aut.ac.ir/~towhidkhah/MPC/Springer-Verlag%20Numerical%20Optimization.pdf). 

**SGN Adjustable Hyperparameters**
hyperparameter|description|default value
:------------:|:---------:|:-----------:
DAMP_RHO|Levenberg-Marquardt regularization parameter |10**-3
BETA|momentum parameter|0.0
CG_PREC|boolean for activation of diagonal preconditer|False
ALPHA_EXP|exponent for the diagonal preconditioner (in case it is active)|1
TR|boolean for activation of trust region heuristic|True
RHO_DECAY|in case the trust region heuristic is not active, this decaying schedule is used for adjusting the Levenberg-Marquardt parameter|'const'
K|final iteration for the decay schedule of the Levenberg-Marquardt parameter|10
ALPHA|step-size|1
PRINT_LEVEL|it regulates the level of verbosity|1
CG_TOL|accuracy of CG|10**-6
MAX_CG_ITERS|maximum number of CG iterations|10
LS|boolean for activation of line search method|True
C_LS|parameter for the line search|10**-4
RHO_LS|parameter for the line search|0.5
MAX_LS_ITERS|maximum number of line search iterations|10

### Results
**Boston Housing Regression with MLP**

0. Boston Housing regression task with a simple 2 layers MLP (solid lines: SGD; dashed lines: SGN):

Train Loss vs Seconds             |  Test Loss vs Seconds
:-------------------------:|:-------------------------:
![GitHub Logo](/figures/boston_loss_time.svg)  |  ![GitHub Logo](/figures/boston_testloss_time.svg)

**Sine Wave Regression with MLP**

0. Sine wave regression task with a simple 3 layers MLP (solid lines: SGD; dashed lines: SGN):

Train Loss vs Seconds             |  Test Loss vs Seconds
:-------------------------:|:-------------------------:
![GitHub Logo](/figures/sine_loss_time.svg)  |  ![GitHub Logo](/figures/sine_testloss_time.svg)
	
**MNIST Classification with MLP**

0. MNIST classification task with a simple 2 layers MLP (solid lines: SGD; dashed lines: SGN):

Train Loss vs Seconds             |  Test Accuracy vs Seconds
:-------------------------:|:-------------------------:
![GitHub Logo](/figures/mnist_loss_time.svg)  |  ![GitHub Logo](/figures/mnist_testacc_time.svg)

0. Table with test accuracies after 25 seconds of training (in parenthesis the value of learning rate and CG iterations for SGD and SGN respectively):
	
	<table>
	<tr> </th></tr>
	<tr><td>

	 algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (0.001)|17.4%  | 36    
	  SGD (0.01)|35.5%   | 36      
	  SGD (0.1)|72.3%    | 36      
	  SGD (1)|89.9%      | 36  


	</td><td>

	  algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (10)|15.1% | 36   
	  SGN (3) |93.0% | 8   
	  SGN (5) |93.1% | 5      
	  SGN (10)|92.1% | 3  


	</td></tr> </table>

0. Table with test accuracies after 200 seconds of training (in parenthesis the value of learning rate and CG iterations for SGD and SGN respectively):
	
	<table>
	<tr> </th></tr>
	<tr><td>

	 algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (0.001)|32.3%  | 290    
	  SGD (0.01)|68.8%   | 290      
	  SGD (0.1)|89.5%    | 288      
	  SGD (1)|93.8%      | 289  


	</td><td>

	  algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (10)|93.8% | 289   
	  SGN (3) |94.0% | 66   
	  SGN (5) |93.6% | 45      
	  SGN (10)|93.1% | 25  


	</td></tr> </table>
	
**FashionMNIST Classification with VGG-type network**


0. FashionMNIST classification task with a simple VGG-type network (solid lines: SGD; dashed lines: SGN):

Train Loss vs Seconds             |  Train Loss vs Epochs
:-------------------------:|:-------------------------:
![GitHub Logo](/figures/fashion_loss_time.svg)  |  ![GitHub Logo](/figures/fashion_loss_epochs.svg)

0. Table with test accuracies after 90 seconds of training (in parenthesis the value of learning rate and CG iterations for SGD and SGN respectively):

	<table>
	<tr> </th></tr>
	<tr><td>

	 algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (0.001)|59.5%  | 34    
	  SGD (0.01)|68.1%   |34     
	  SGD (0.1)|70.9%    | 34      
	  SGN (1)|84.4%      | 9  


	</td><td>

	  algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGN (2)|85.7% | 6   
	  SGN (3)|86.7% | 4    
	  SGN (5)|87.7% | 3      
	  SGN (10)|86.0% | 1   


	</td></tr> </table>
	
**CIFAR10 Classification with VGG-type network**

0. CIFAR10 classification task with a simple VGG-type network (solid lines: SGD; dashed lines: SGN):

Train Loss vs Seconds             |  Train Loss vs Epochs
:-------------------------:|:-------------------------:
![GitHub Logo](/figures/cifar10_loss_time.svg)  |  ![GitHub Logo](/figures/cifar10_loss_epochs.svg)

0. Table with test accuracies after 250 seconds of training (in parenthesis the value of learning rate and CG iterations for SGD and SGN respectively):


	<table>
	<tr> </th></tr>
	<tr><td>

	 algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGD (0.001)|30.5%  | 67    
	  SGD (0.01)|52.4%   |67     
	  SGD (0.1)|57.9%    | 67      
	  SGN (1)|49.6%      | 14  


	</td><td>

	  algorithm|test acc | epochs							      
	  :-------:|:-------:|:-------:|
	  SGN (2)|52.7% | 9   
	  SGN (3)|55.5% | 7    
	  SGN (5)|52.6% | 4      
	  SGN (10)|57.6% | 2   


	</td></tr> </table>
