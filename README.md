# Towards fast weak adversarial training to solve high dimensional parabolic partial differential equations using XNODE-WAN

Due to the curse of dimensionality, solving high dimensional parabolic partial differential equations (PDEs) has been a challenging problem for decades. Recently, a weak adversarial network (WAN) proposed in [1] offers a flexible and computationally efficient approach to tackle this problem defined on arbitrary domains by leveraging the weak solution. WAN reformulates the PDE problem as a generative adversarial network, where the weak solution (primal network) and the test function (adversarial network) are parameterized by multi-layer deep neural networks (DNNs). 

In our work, we design a novel so-called XNODE model built on the neural ODE model with additional spatial dependency for a universal and effective representation for the parabolic PDE solution. The proposed hybrid method (XNODE-WAN) by integrating the XNODE model within the WAN framework leads to significant improvement on the performance and efficiency of training. Numerical results show that our method can reduce the training time to a fraction of that of the WAN model. 

More specifically, our XNODE-WAN algorithm aims to solve the following BVP PDE:

![equation](https://latex.codecogs.com/gif.latex?%5Cleft%5C%7B%5Cbegin%7Barray%7D%7Bll%7D%20u_%7Bt%7D-%5Csum_%7Bi%3D1%7D%5Ed%20%5Cpartial_i%20%28%5Csum_%7Bj%3D1%7D%5Ed%20a_%7Bij%7D%20%5Cpartial_j%20u%29%20&plus;%20%5Csum_%7Bi%3D1%7D%5Ed%20b_i%20%5Cpartial_i%20u%20&plus;%20cu%20%3D%20f%2C%20%26%20%5Ctext%20%7B%20in%20%7D%20%5COmega%20%5Ctimes%5B0%2C%20T%5D%20%5C%5C%20u%28x%2C%20t%29%3Dg%28x%2C%20t%29%2C%20%26%20%5Ctext%20%7B%20on%20%7D%20%5Cpartial%20%5COmega%20%5Ctimes%5B0%2C%20T%5D%20%5C%5C%20u%28x%2C%200%29%3Dh%28x%29%2C%20%26%20%5Ctext%20%7B%20in%20%7D%20%5COmega%20%5Cend%7Barray%7D%5Cright.)

for a d-dimensional domain. It is also able to handle time-varying domains, denoted $\Omega_t$.

This repository is the official implementation of the paper entitled "Towards fast weak adversarial training to solve high dimensional parabolic partial differential equations using XNODE-WAN".

# Running Codes
Requirements for a successful implementation of the codes can be found in `requirements.txt`.

To solve a PDE one can input all the known functions of the problem in the file `main.py` which will run the algorithm. An example of this in action can be found in the `example.py` file in which our test problem from the paper is implemented.

In the `config` dictionary one can specify the hyperparameters to be used by the algorithm to solve the problem. 

In the `setup` dictionary the problem specific information is included:
- `dim`: the dimension of our problem's domain (excluding time)
- `N_t`: the number of time points sampled for each path
- `N_r`: the number of paths in the interior of the domain
- `N_b`: the number of paths on the boundary of the domain
- `T0`: the minimum time at which our domain exists
- `T`: the maximum time at which our domain exists

## Directly evaluating points
Note that the data structure is `[N, L, C]` where `N` is the number of different points, `L` is the number of time points at which they are evaluated (these have to be the same for all points) and `C` is the axis of the dimensions where time is the top dimension (`[:, :, 0]` is the index for all times).

There are certain complications in evaluating points and therefore the best method is to feed the network points to be evaluated individually. The form for single points (to ensure accurate computations) needs to be `torch.tensor([[x0, x]])` where `x` is the point you want to evaluate and `x0` has the same coordinates in $\mathbb{R}^d$ but the time coordinate is that at which this point in space is on $\patial\Omega_t$ (this includes all points with time `T0`).

# Domains
The algorithm supports a wide variety of domains, including time-varying ones, and these can be specified in the `dataset.py` file which already contains some examples. It is important to conform to the structure highlighted in this file to guarantee that the algorithm works.

# Reference papers
1. [Weak Adversarial Networks for High-dimensional Partial
Differential Equations](https://arxiv.org/pdf/1907.08272.pdf)
2. [Solving high-dimensional partial differential equations using deep learning](https://www.pnas.org/content/115/34/8505)
