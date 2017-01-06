---
layout: post
title: Calculation of magnetic phase shift from a 3D sample
subtitle: Following the method of <a href="http://www.sciencedirect.com/science/article/pii/S0304399113000764"> Humphrey <i>et al.</i></a>
---

There are a few ways to calculate the magnetic phase shift of a 3D magnetization configuration. One that is particularly tailored to calculation of tilted samples is the algorithm proposed by Humphrey and De Grafe. The idea behind their algorithm is to treat each voxel of the sample as if it were a sphere with constant magnetization. The Aharonov-Bohm phase shift caused by a sphere with constant magnetization is known analytically, so the full phase shift can be calculated as the sum of each of the individual voxels.

To start with the phase shift from a single sphere in Fourier space, from Humphrey *et al.* eq. H(8) is,

{% raw %}
$$
\varphi_m(k_z,k_y)=\frac{3 \pi \mathrm{i} B_0 V}{R\Phi_0}\frac{j_1(k_{\perp} R)}{k_{\perp}^{3}}(\boldsymbol{\mathbf{\hat{\mu}}}\times \mathbf{k})|_z.
$$
{% endraw %}

Above \\(B_0\\) is the saturation induction (\\(B_0 = \mu_0 M_s\\)), \\(V\\) is the volume of the sphere, \\(R\\) is the radius of the sphere on the cubic lattice (\\(R=a\left(\frac{6}{\pi} \right)^{1/3} \approx 1.2407a\\)) here \\(a\\) is the lattice constant, \\(\Phi_0\\) is the flux quantum \\(h/2e = 2070 \text{T nm}^2\\), \\(j_1(x) = \frac{\sin(x)}{x^2}-\frac{\cos(x)}{x}\\) is the first spherical Bessel function of the first kind, \\(\mathbf{k}\\) is the Fourier frequency vector (\\(k_x, k_y\\) and \\(k_{\perp}\\) are the components and magnitude of \\(\mathbf{k}\\) in the plane normal to the projection direction (\\(\boldsymbol{\mathbf{\hat{\omega}}}\\)) ), and \\(\boldsymbol{\mathbf{\hat{\mu}}} \\) is the normalized magnetization unit vector expressed in the beams coordinate system. This can be further reduced by splitting up the geometric portion (invariant under rotation) with the magnetic portion to yield H(10)

$$
\varphi_m(k_x,k_y)=\mu_x\mathcal{S}_y-\mu_y\mathcal{S}_x
$$

$$
\mathcal{S}_{\alpha} \equiv 4 \pi^2 R^2 \frac{\mathrm{i}B_0}{\Phi_0}\frac{j_1(k_{\perp}R)}{k_{\perp}^{3}}k_{\alpha}
$$

Equation H(10) is for a uniformly magnetized sphere at the origin, to add a shift in real space we just need to add a linear phase in Fourier space. Adding this in and realizing that when tilting about the \\(x\\) or \\(y\\) axis only the components of \\( \boldsymbol{\mathbf{\hat{\mu}}} \\) parallel to the detector contribute then summing over the full lattice yields H(12) and H(13) (see paper for more details).

H(12) for a counter clockwise tilt around the x axis \\( \gamma \\),

$$
\phi_m^{\gamma}(k_x,k_y) = \sum_{(i,j,k)\in D} e^{2 a \mathrm{i} (k_x i + k_y(j\cos{\gamma}-k \sin{\gamma}))}\left[ \mu_{ijk,x}\mathcal{S}_y-(\mu_{ijk,y}\cos{\gamma}-\mu_{ijk,z}\sin{\gamma})\mathcal{S}_x\right].
$$

H(13) for a counter clockwise tilt around the y axis \\(\delta\\),

$$
\phi_m^{\delta}(k_x,k_y) = \sum_{(i,j,k)\in D} e^{2 a \mathrm{i} (k_x(i\cos{\delta} + k \sin{\delta}) + k_y j)}\left[ (\mu_{ijk,x}\cos{\delta} + \mu_{ijk,z}\sin{\delta})\mathcal{S}_y-\mu_{ijk,y}\mathcal{S}_x\right].
$$

A Jupyter notebook demonstrating the use of my python implementation of this algorithm can be found [here](https://github.com/jordanchess/LTEM_tools/blob/master/notebooks/Calculating%20A-B%20phase%20shift%20from%20a%203D%20magnetic%20sample.ipynb). Below is a video of the in-plane (perpendicular to the electron beam) magnetic induction of a spherical particle approximated by 29080 individual spherical voxels rotating 360° about the \\(y\\)-axis.

<figure class="video_container">
  <iframe src="../video/magnetized_sphere.webm" allowfullscreen="true" width="420" height="420"> </iframe>
</figure>
