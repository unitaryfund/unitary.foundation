---
title: Optimizing the Molecular geometry of the Haber-Bosch Process with Pennylane
author: Minh Triet Chau
day: 27
month: 4
year: 2025
tags: 
  - simulator
  - pennylane
---

**TLDR**: We contribute an approach to simulate chemistry reaction with (1) catalyst and (2) endothermicity in 3D.


# Introduction

$$\text{NH}_3$$ production uses 1% of the world's energy. The most widely adopted synthesis method, the Haber-Bosch process, is $$\text{N}_2 + 3\text{H}_2 \overset{\text{Fe}}{\longrightarrow} 2\text{NH}_3$$.

However, it is energy-intensive and relies on catalysts. Understanding the reaction pathway with catalyst hopes to gain insights into the process, enhancing its efficiency. [1] has simulated the reaction using VASP using Fe211 as a catalyst as shown in Fig. 1.

<figure>
  <img src="/images/c124-4041-9f75-2d0e40e0a42d.png">
  <figcaption>Fig 1. Reaction pathway of the Harber-Bosch process in [1] </figcaption>
</figure>

Since VASP is computationally expensive, what can we achieve with a less computationally expensive, hence faster method? This post explores how we can use Pennylane to execute the first steps with limited computer resources. Concretely, we replicate the first three steps of [1] in Fig. 2, which are the most complex reactions in the whole pathway. It is computationally cheaper than the method used in [1].

# Method

1. Varies the coordinates of the reactant: The transition in $$x,y,z$$ axis and rotation angle around its center.
   - We use two optimization methods: Gradient descent and Bayesian optimization (BO). The former method has good coverage in these [Pennylane](https://pennylane.ai/qml/demos/tutorial_chemical_reactions/) [demoes](https://pennylane.ai/qml/demos/tutorial_mol_geo_opt/) in a 1D setup.
   - In this work, we focus on the latter in a 3D setup. For a comprehensive review of BO, we refer readers to [3].
3. Construct the Hamiltonian
4. Measure and Optimize: Measure the expectation value of the Hamiltonian using the quantum circuit and optimize the parameters using a classical optimizer to minimize this value.

## Motivation
To define a 3D affine transformation for a molecule, we need to define three translation parameters $$t_x, t_y, t_z$$ and three rotational parameters $$\theta_x,\theta_y,\theta_z$$. Calculating the gradient needs $$t_\bullet \pm \Delta_t$$ and $$\theta_\bullet \pm \Delta_\theta$$. Therefore, each learning step requires $$6 \times 2$$ Hamiltonian, which is expensive.

## Bayes Optimization (BO)
BO, being a gradient-less method, does not have these requirements. Here is how it works. After the initial samplings $$\pmb{X}=x_1, x_2, ... , x_n$$ , Gaussian processing (GP) assume that 

$$\pmb{X} \sim \mathscr{N}\begin{bmatrix}\begin{pmatrix} \mu_1 \\ \vdots \\ \mu_n\end{pmatrix},\begin{pmatrix}1 & K_{12} & \cdots & K_{1n}\\ K_{21} & \vdots & \ddots & \vdots \\ K_{n1} & K_{n2} & \cdots & K_{nn} \end{pmatrix} \end{bmatrix}$$ 

and fixes a regression curve through these data points. Only the observed data points have absolute certainty (variance = 0). For an unknown point $$x'$$, GP returns a prediction from the distribution of $$\mathscr{N}(\mu,\Sigma\lvert\pmb{X})$$.

A beautiful property of Gaussian is that when conditioning the Gaussian on a set of variables, the result is also a Gaussian $$\mathscr{N}(\mu',\Sigma')$$. In Fig.2, the prediction value of the unknown point is $$\mu'$$, whereas the confidence interval is $$\Sigma'$$.

<figure>
<div class="row">	
	<div class="column">
 	<figure>
		<img src="/images/af199fd0-5dbe-4a2c-addb-e9fb6f2ecd2b.png">
  		<figcaption>A</figcaption>
  	</figure>
	</div>
	<div class="column">
 	<figure>
		<img src="/images/358421422-033cceb6-5ea3-4663-af1f-b17e4708a085.png">
  		<figcaption>B</figcaption>
  	</figure>
	</div>
</div>
<figcaption>Figure 2: A. Given initial samplings (red points), how do we predict an unknown point? B. How GP fits a regression line given these ground truths. The confidence interval is the variances of the Gaussian at an arbitrary point, and the prediction is the means</figcaption>
</figure>

Here, we face a dilemma. In $$\pmb{X}$$ there is a point with the smallest value $$x_{min}$$. Therefore, the next minima should be somewhere close to it. However, it is also possible that points in the wide confidence interval have smaller values than the current minima. To quantify this trade-off, we define an acquisition function $$\alpha$$ to obtain new positions to sample. There are several approaches to define $$\alpha$$, we are using the Expected Improvement (EI) approach here.

EI works by generating random functions that lie inside the confident intervals, as demonstrated in Figure 3. The intuition is a larger confidence interval would have more diverse sampling functions. Afterward, we sample the point at the extrema and update the prior. The process continues for a predefined number of steps.
<div class="row">
	<div class="column">
<figure>
<img src="/images/a3cd9cbd-da05-40a2-a61d-8f18760cda38.png">
<figcaption>Figure 3. Given the prior in solid blue, EI generates two candidates (dashed red and green lines) that satisfy the confidence interval requirements. Consequently, we sample the real value at the point marked with "?"</figcaption>
</figure>
	</div>
</div>

## Simplification
To facilitate the calculation with our limited computing resources we made the following simplification
1. Calculating the Hamiltonian with $$\text{STO-3G}$$, a minimal orbital basis.
2. Simplyfing the catalyst substrate.
<div class="row">
	<div class="column">
		<figure>
	        <img alt="image" src="/images/c981aea2-1fb2-4cad-95b8-0ba84b8cee11.png">
		    <figcaption>The Fe211 catalyst substrate used in [1] </figcaption>
		</figure>
  	</div>
	<div class="column">
		<figure>
	<img src="/images/aa6a75ec-c8f5-406b-82df-9de6bb4838a6.png">
 	<figcaption>Our simplified catalyst substrate</figcaption>
		</figure>
	</div>
</div>

# Visualization
We will replicate the first three steps of Figure 2, whose Hamiltonians are the most complex in the whole pathway. 
 
In the below visualization, we color-coded the atoms as 
<span class="boxed fe">
      $$\text{Fe}$$
</span>
<span class="boxed n">
      $$\text{N}$$
</span>, and
<span class="boxed h">
       $$\text{H}$$
</span>. Please open the GIFs/videos in a new tab for a full-resolution version.

## Step 1
We fix the coordinates of the catalyst substrate and use the gradient descent method to optimize the coordinates of the reactants. The result is as follows.
<figure>
<div class="row">
	<div class="column">
		<figure>
		<img style="scale: 80%" src="/images/355275392-0463d31a-b969-42af-9cef-c7c4c7cbbb72.png">
		<figcaption>A</figcaption>
		</figure>
	</div>
	<div class="column">
		<figure>
		<img src="/images/355273309-3213af70-d246-43c7-859d-5c12cb607d5e.gif">
		<figcaption>B</figcaption>
		</figure>
	</div>
	<div class="column">
		<figure>
		<img src="/images/356906338-ad7863f8-26a2-4ebd-93fa-83bdc119fc14.gif">
		<figcaption>C</figcaption>
		</figure>
  	</div>
</div>
	<figcaption>A. The ground truth from [1] B. Our simulation from a randomized position C. The ground state energy of the Hamiltonian</figcaption>
</figure>
Even with the same learning rate, the speed of the reactants slows down greatly compared to the beginning. Therefore, we conclude that in this step, the learning converged.

## Step 2
In this step, we add $$H_2$$ as a reactant. To minimize the time to build the Hamiltonian, we fix the coordinates of all the reactants of the previous step. Since gradient descent works before, let's apply the same method.
<div class="row">
<div class="column">
 	<img alt="image" src="/images/354735960-e02f4911-6a03-4fde-82bf-d81f0250f94f.png">
 </div>
 <div class="column">
 	<img alt="image" src="/images/354742674-523ecb70-6cf6-4c20-a077-0f957f0b255e.gif">
 </div>
</div>
Note the moving speed of the reactants is much slower than the previous step. Therefore, it is clear that we are stuck at a local minimum, and gradient descent would have a hard time escaping it without modeling the endothermicity of this reaction ($$0.98$$ below the arrow). Let us try with BO to see if it can overcome this minima.

<div class="row">
 <div class="column">
        <img alt="image" src="/images/356371643-513c5f1e-ec65-4a97-b6d7-403902fb096a.gif">
</div>
<div class="column">
        <img src="/images/356761793-ede2698a-2db0-41d1-a3c5-27c8bb818aa9.gif">
</div>
</div>
Due to the nature of the acquisition function, the plot of the ground state energy does not decrease nicely as seen in the gradient descent. Most of the $$\text{H}$$ atoms went too far away from the catalyst substrate, due to our initial setup of the search margin, which is $$1 nm$$ bigger than the catalyst substrate. Here we filter only what went inside the substrate.

 <div class="row">
<div class="column">
 <img alt="image" src="/images/356394575-3673603c-4900-4453-84b8-e9cfd8ece620.gif">
 </div>
  <div class="column">
 <img src="/images/356399358-2017c7e1-885e-4265-a1cf-1852f65c67a1.gif">
</div>
</div>

We conclude that the search boundary condition is of utmost importance. Accordingly, we modify the search space as below.
<figure>
   <img src="/images/0fd1987e-55d7-40f3-ac2d-6231d561a95d.png">
  <figcaption>The size of the search boundary in comparison with a $$\text{Fe}$$ atom</figcaption>
</figure>

Here is the optimization after setting the search boundary. We made these demonstration videos instead of GIFs so that readers can control the frame shown.
 <div class="row">
<div class="column">
 <video controls autoplay mute loop>
 <source src="/images/357620305-58da4eaa-548d-4c2b-8c57-eb7368073973.mp4" type="video/mp4"/>
</video>
</div>
<div class="column">
 <video controls autoplay mute loop>
 <source src="/images/357975419-000ac2a4-21fe-4055-b48f-da7da6998d4f.mp4" type="video/mp4"/>
</video>
 </div>
 </div>

$$\text{NH}_3$$ molecule is produced in a total of 6 trials (namely 12th, 13th, 25th, 26th, 27th, and 28th), which coincidentally matches the efficiency of the Harber-Bosch process of about 20%[^a]. It also offers an explanation for that number, which is the coordinates that create $$\text{NH}_3$$ have higher ground state energy than the other configurations where $$\text{H}$$ atoms float away. Note that there are times that the BO chooses to sample two very close points consecutively (12th and 13th frame for example), it is because of the exploitation and exploration trade-of that we mentioned earlier

[^a]: [https://en.wikipedia.org/wiki/Haber_process#Ammonia_production](https://en.wikipedia.org/wiki/Haber_process#Ammonia_production)

## Conclusion
This blog post provides an alternate method to optimize the geometry of the Haber-Bosch process. Due to the lack of computational resources we have to reduce the catalyst platform, number of active orbitals, and electrons. These parameters are visible in `chem_config.yaml`. Even with a simple orbital basis and reduced numbers of active electrons and orbitals, the experiments still take up a lot of time.

- Gradient descent: 32 CPU, 256 GB RAM, 50h runtime
- BO: 8 CPU, 64 GB RAM, 60h runtime

The parameters of free electrons and free orbitals are crucial parameters to make this project possible as a full configuration interaction can take up to TBs of RAM to calculate. That being said, our current simple setup can replicate the location where $$\text{NH}_3$$ is produced on the catalyst surface.

Comments? Questions? Please let us know at [Issues page](https://github.com/minhtriet/minhtriet.github.io/issues)

## Special thanks
This work done in QOSF cohort 9 Quantum mentorship program. My gratitude to my mentor Danial Motlagh and to the organizer QOSF

# References 
[1] Reaction mechanism and kinetics for ammonia synthesis on the Fe(211) reconstructed surface. Jon Fuller, Alessandro Fortunelli, William A. Goddard III, and  Qi An. Physical Chemistry Chemical Physics Issue 21, 2019

[2] Reiher Markus, Nathan Wiebe, Krysta M. Svore, Dave Wecker and Matthias Troyer. Elucidating reaction mechanisms on quantum computers. Proceedings of the National Academy of Sciences 2017

[3] Kevin Patrick Murphy. Machine Learning: a Probabilistic Perspective. MIT Press, 2012

<style>
.boxed {      
    color: black;
    border: 3px solid #535353;        
    padding: 5px 20px;
    border-radius: 10px;
    margin: 7px;
}

.fe {
  background: #e06633;
}
.n{
    background: #3050f8;
}
.h{
    background: #fefefe;
}
div.flex {
  display:flex;
  justify-content:space-between;
  flex-wrap:wrap;
  align-items:center
}
figcaption {
  text-align:center
}
div.row {
  display:flex;
  flex-direction:row;
  flex-wrap:wrap;
  width:100%;
  align-items:center;
  justify-content:space-between
}
figure image {
  margin-left:auto;
  display:block;
  margin-right:auto
}
div.column {
  display:flex;
  flex-direction:column;
  flex-basis:100%;
  flex:1;
  align-items:center;
  justify-content:space-between
}
</style>