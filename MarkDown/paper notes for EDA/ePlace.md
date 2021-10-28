[toc]

# ePlace: Electrostatics based Placement using Fast Fourier Transform and Nesterov's Method 



## Abstract





a novel placement function ***eDensity***, which models every object as positive charge and the density cost as the potential energy of the electrostatic system.





The electric potential and field distribution are coupled with density using a ***well-defined Poisson’s equation**,* which is numerically solved by *spectral methods based on **fast Fourier transform*** (FFT).



***Nesterov's method*** can achieves faster convergence.

predicting the step length using a closed-form *equation of Lipschitz constant.*

## Introduction

The quality of placement results is usually evaluated by the total *half-perimeter wirelength* (HPWL)

Traditional placement methods can be generally divided into four categories, namely 

**(1) stochastic simulation** 

**(2) min-cut partition** 

**(3) quadratic minimization** 

**(4) nonlinear optimization**



#### stochastic simulation

usually based on simulated- annealing techniques. (SA)

Uphill climbing is probabilistically accepted to rescue the placer from local optima.

high complexity and low convergence rate, which induces poor scalability to large circuits.



#### min-cut 

Min-cut approaches recursively simplify the problem by partitioning the in- stance (netlist and placement region) into smaller sub-instances. Local optimum al- gorithms [Caldwell et al. 2000] are usually employed when the problem instance becomes sufficiently small.



However, improper partitioning at early stages could induce unrecoverable quality loss to the final solu- tion.



#### quadratic



**The differentiability enables gradient-based minimization technique.**



**Density equalization** is performed by adding pseudo pins and nets to the physically overlapped cells with a linear term introduced to the cost function.



Approximate the net length using a quadratic function, which can be linearized by various net models.



By solving the linear system, cells are iteratively dragged away from over-filled regions.



Despite high placement efficiency, the solution quality and robustness usually **lag** behind nonlinear placers.



#### nonlinear 

Algorithms based on a framework of nonlinear optimization



Wirelength and density are modeled using smooth mathematical functions thus gradients can be analytically calculated ---> log-sum-exp model / weighted-average model.



Despity models mainly include the bell-shaped function, Gaussion equation nad Helmholtz equation.



The partial differential equation(PDE) can be solved by Green's function or finite-difference method.



By Lagrange relaxation or penalty method, the grid density constraints are inte- grated into the objective function and solved by nonlinear CG method.



Due to the high complexity of modeling functions, nonlinear approaches employ multi-level cell clustering to simplify the problem and accelerate the algorithm. However, the quality overhead is not negligible.





### In this work



develop a novel density function eDensity modeling the placement instance as an electrostatic system for density equalization



##### 2.1 Essential Concepts of Placement

Macro and Cell

HPWL = half-perimeter Wire length 

<img src="/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919145513080.png" alt="image-20210919145513080" style="zoom:50%;" />

The total HPWL is computed as $HPWL(v) = \sum_{e \in E}HPWL_e(v)$

Placement problem defined :
$$
min_v HPWL(v) \space s.t.\space {\rm v\space \space is\space \space a \space legal \space solution}
$$


##### 2.2 Definition of Global Placement

regarded as a problem of  constarint optimization .



placement region is decompose into a set of $m \times m$ Rectangle girds(bins) denoted as $B$. Based on a placement solution $v$, let $\rho_b({\rm v})$ Denote the density of each grid $b$ as exoressed in :

![image-20210919151020881](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919151020881.png)

a global placement problem targets a solution v with minimum total HPWL subject to the constraint that the density ρb(v) of all the grids are equal or below a predetermined target placement density $\rho_t$.

![image-20210919151200979](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919151200979.png)

##### 2.3 Wirelength Smoothing



##### Log-Sum-Exp(LSE)



![image-20210919151823295](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919151823295.png)

##### Weight-Average (WA) 

is used for nonlinear placement optimiztion in this work. Eq below shows the horizontal function of net $e$.

![image-20210919151713398](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919151713398.png)

(This Modeling error is roughly half of that of LSE)

In this work, we use the WA wirelength model for our nonlinear placement optimization.

##### 2.4 Density Penalty

![image-20210919153531685](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919153531685.png)

As the original density function $ρ_b(v)$ is not differentiable and hard to optimize, a smoothed density function $\tilde\rho$  is used here by employing a “bell-shape” local smoothing technique.

In this work, we model the placement instance as an electrostatic system and devise the density penalty N(v) to be the system potential energy. In the remaining part of the paper, we will use N(v) to denote both density penalty and system energy. This modeling methodology is discussed in detail in Section 3 regarding how the density penalty and gradient are defined. A fast numerical solution to the density and potential related Poisson’s equation (Eq (20)) is proposed in Section 4. 

##### 2.5 Nonlinear optimization formulation

 ![image-20210919154104935](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919154104935.png)



Lagrange multiplers used:

![image-20210919154210388](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210919154210388.png)





### **3. EDensity: a novel density function by electrostatic**

##### 3.1 System Modeling Using Electrostatic Equilibrium



![image-20210918140942487](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210918140942487.png)

The target of density equalization is equivalent to the system state of electrostatic equilibrium.





![image-20210918143012495](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210918143012495.png)



black rectangles -> Macros

red dots  -> standard cells

blue dots -> filters



### 3.2 Density Penalty and Gradient Formulaiton

##### Filler insertion

The additional density force due to filler insertion will squeeze the cells to be placed closer to their connected neighbors with density constraint still satisfied.



##### Dark node insertion

As a result, the density force pushes the cells away from macros, inducing under-filled whitespace around macros and wirelength overhead



##### Density scaling

The electric quantity of each fixed or movable large macro is scaled down to the target placement density. Regions filled by small standard cells or covered by large macros will have the same charge density, there is no additional density force to drag cells away from macros.

##### Density energy computation





## 4 Possion's Equation and Numerical Solution

Based on our eDensity formulation in Section 3, we propose Poisson’s equation to couple the charge density with electric potential and field. Neumann boundary condition is used to enforce the legality of the global placement solution.

The Poisson’s equation is numerically solved using **spectral methods** with high accuracy yet low complexity. Moreover, we propose a technique to locally smooth the density over discrete grids.

### 4.1 Well-defined Possion's Equation

![image-20210924094304302](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210924094304302.png)



 When cells are moving towards the borderline of the placement region, the movement should be slowdown or stopped in order to prevent cells from moving outside. 



Nuemann bondary condition

![image-20210924095306277](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210924095306277.png)



Besides, the integral of the density function ρ(x, y) and the potential function ψ(x, y) over the entire placement region R is set to be zero, as Eq. (19) shows
![image-20210924095417188](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210924095417188.png)



### 4.2  Fast Numerical Solution using Spectral Methods

In our approach, the DCT and DST are used in spectral methods to generate the solution to the partial differential equations, where density penalty and gradient are modeled as system potential energy and electric force. 





### 4.3 Convergence

As a result, we have the respective electric field coefficients $\frac{a_{u,v}W_u}{w_u^2+w_v^2} \neq 0$,$\frac{a_{u,v}W_v}{w_u^2+w_v^2} \neq0$ which means that ξx and ξy are not zero. The
electric force will then keep pushing the system potential energy to drop by gradient descent till finally a globally even density distribution is achieved. As a result, our density function has guaranteed convergence



### 4.4 Behavior and Complexity Analysis

##### 

### Density Computation



### Potential and Field Computation







### 4.5 Local Smoothness Over Discrete Grids





## 5 Nonlinear Optimization :star2:

In this section, we first briefly introduce the Conjugate Gradi- ent (CG) method which is widely used in previous nonlinear placement works [Kahng and Wang 2006; Chen et al. 2008], and discuss the efficiency bottleneck on the line search. Then we propose Nesterov’s method to solve the nonlinear problem and illus- trate our technique of Lipschitz constant prediction, which determines the steplength in constant time. To the best of our knowledge.

**our work is the first one in literature to incorporate Nesterov’s method and Lipschitz constant prediction into global placement optimization.**



### 5.1  Conjugate Gradient Method with Line Search 





CG:







line search is usually time consuming and could dominate the efficiency of the en-
tire nonlinear placement



we use Nesterov’s method as the nonlinear solver and Lipschitz constant prediction to determine the steplength. The optimizer could be beneficial from both convergence rate and solution quality simultaneously.

###  5.2 Nesterov’s Method with Lipschitz Constant Prediction 







### 5.3 preconditioning    :star:



Hessian Matrice一个[多元函数](https://baike.baidu.com/item/多元函数/2498728)的二阶[偏导数](https://baike.baidu.com/item/偏导数)构成的方阵，描述了函数的局部[曲率](https://baike.baidu.com/item/曲率/9985286).黑塞矩阵常用于[牛顿法](https://baike.baidu.com/item/牛顿法/1384129)解决优化问题，利用黑塞矩阵可判定多元函数的[极值](https://baike.baidu.com/item/极值/5330918)问题。在工程实际问题的优化设计中，所列的目标函数往往很复杂，为了使问题简化，常常将目标函数在某点邻域展开成泰勒多项式来逼近原函数，此时函数在某点泰勒展开式的矩阵形式中会涉及到黑塞矩阵。



预处理的基本思想是通过修改病态（ill-conditioned）系统

![[公式]](https://www.zhihu.com/equation?tex=Ax%3Db+%5Ctag%7B1%7D)

得到一个等价系统 ![[公式]](https://www.zhihu.com/equation?tex=%5Chat%7BA%7D%5Chat%7Bx%7D%3D%5Chat%7Bb%7D) 使得迭代算法的收敛速度更快。

常见的方法是使用一个非奇异矩阵 ![[公式]](https://www.zhihu.com/equation?tex=M) 使得

![[公式]](https://www.zhihu.com/equation?tex=M%5E%7B-1%7DAx%3DM%5E%7B-1%7Db+%5Ctag%7B2%7D)

其中预处理矩阵 ![[公式]](https://www.zhihu.com/equation?tex=M) 需要使得 ![[公式]](https://www.zhihu.com/equation?tex=%5Chat%7BA%7D%3DM%5E%7B-1%7DA) 的条件数变小，或者说避免病态问题。









## 6 Global Placement Alg.:star::star::star:	

![image-20210922161441015](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210922161441015.png)

- **Middle stage of global placement**

- ##### The global placement is based on the input solution $v_{ip}$  from the initial placement stage, where the quadratic wirelength is minimized using bound-to-bound (B2B) net .

- ##### A linear CG solver is used with Jacobi preconditioning for acceleration

- ##### A self-adaptive parameter adjustment method is incorporated to improve the quality and convergence rate.





### 6.1 Self-Adaptive Parameter Adjustment

##### Grid dimension



##### Steplength



##### Penalty factor



##### Density overflow



##### Wirelength coefficient

##### 

### 6.2 Global Placement

![image-20210925231459568](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210925231459568.png)



### 7. Experiment and Results

### 7.1 Results on ISPD 2005 Benchmark Suite

### 7.2 Results on ISPD 2006 Benchmark Suite 

### 7.3 Placement Runtime Breakdown

### Conclusion

##### Outline

- A flat nonlinear global placement algorithm *ePlace*
- eDensity:
  - converted to an **electrostatic system** to model the *density cost* as the *system potential energy.*
  - The electric potential and ﬁeld distribution are correlated with the *spatial density distribution* via a **well-deﬁned Poisson’s equation.**
  - use **spectral methods based on fast Fourier transform** to produce fast and accurate numerical solution.
- use Nesterov’s method as the nonlinear placement solver, which is better than CG solver.
  - A novel heuristic is developed to dynamically **approximate the Lipschitz constant** for *steplength prediction*.
- Our nonlinear preconditioning technique further enhances the solution quality with negligible runtime overhead.

##### Experiment 

The experimental results on the ISPD 2005 and ISPD 2006 bench- marks validate the high performance of ePlace.

##### Resolution

Resolve the traditional bottlenecks in nonlinear placement:

- line search
- Sub optimality of netlist clustering
- Quality degradation

Cause: coarse density at early stage   

 

**Nonlinear placement** can be better than cutting-edge quadratic placement alg.

 

eDensity is actually conducting a simulation on the behavior of the equivalent electro- static system,

so we could have global smoothness, fast convergence and high quality all be achieved in a promising way.



##### Future tasks

Parallel computing platform

Accelerate gradient computation  via distribution system or GPU

Enhance capability to handle mixed-size large-scale circuits









