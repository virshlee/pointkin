# Point kinetic reactor simulation
This is the description of my thesis done during my electrical engineering bachelor's degree at the Budapest University of Technology and Economics.
The integration of the module gives oppurtuntiy for 
simulating certain transients in the reactor of Paks Nuclear Power Plant (https://en.wikipedia.org/wiki/Paks_Nuclear_Power_Plant) with the help of a less detailed model, than the one used 
before. It is in C#

Neutron point kinetics model is a neutron physical approach that is capable of describing
the neutron kinetics proccesses taken place in a reactor core. By supposing the 
separability of the neutron flux in time and space, we can derive an ordinary differential 
equation system. This approach was more used for neutron kinetics simulation back in 
the days when the computing capacity was more limited, but today there is opportunity 
for using resource demanding solutions that give a more detailed description. That is why 
in the full scope simulator of Paks NPP, there has been such a solution for neutron kinetics 
that gives a detailed solution: KIKO3D is based on solving diffusion equation and 
nodalizing.

# Theoretical background
Separating the neutron flux to a time-dependent and a space-dependent term
```math
\Phi ({r}, E, \Omega,𝑡) = \phi(t)\Phi_0({r}, E, {\Omega})
```


the differential equation system can be derived:
```math 
{\frac{d\varphi(t)}{d t}}={\frac{\rho-\beta}{A}}\varphi(t)+\sum_{i=1}^{6}\lambda_{i}C_{i}(t)+S(t),
```
```math 
\frac{d C_{i}(t)}{d t}=\frac{\varphi(t)}{A}\beta_{i}-\lambda_{i}C_{i}(t),
```
```math 
\beta=\sum_{i=1}^{n}\beta_{i}.
```
# Solution
**4th order RK method**

I implemented 4th order Runge-Kutta method in C# using 

```math
\begin{array}{c}{{k_{1}:=f(x_{i},y_{i}),}}\\  {{k_{2}:=f(x_{i}+\frac{h}{2},y_{i}+\frac{h}{2}k_{2}),}}\\ {{k_{3}:=f(x_{i}+\frac{h}{2},y_{i}+\frac{h}{2} k_{3}),}}\\ {{k_{4}:=f(x_{i}+h,y_{i}+h k_{3}),}}\\\end{array}
```
```math
$$y_{i+1}=y_{i}+\frac{h}{6}\cdot(k_{1}+2k_{2}+2k_{3}+k_{4}).$$
```
**Prompt jump approximation**

The point kinetic differential equation system describes the emission of two types of neutrons: primary and secondary. The primary electrons time constant is roughly $$10^{-5}-10^{-3}$$ s. The simulators timestep is 0.2 s, so the primary neutrons effect can be approximated using a prompt jump in the number of neutrons, deriving to the new eq. system:
```math
$$\varphi(t)=-{\frac{\Lambda}{\rho-\beta}}\cdot\sum_{i=1}^{6}\lambda_{i}C_{i}(t),$$ 
$$\frac{d C_{i}(t)}{d t}=-\frac{\beta_{i}}{\rho-\beta}\cdot\sum_{j=1}^{6}\lambda_{j}C_{j}(t)-\lambda_{i}C_{i}(t)$$
```

**Feedback**
```math
$$\begin{array}{c}{{\frac{d T_{f}}{d t}=\mathrm{\mu}_{f}\left[f_{f}P_{0}\varphi(t)-\Omega(T_{f}-T_{c})\right]}}\\ {{\frac{d T_{i}}{d t}=\mathrm{u}_{c}\left[\left(1-f_{f}P_{0}\varphi(t)+\Omega(T_{f}-T_{c})-M(T_{i}-T_{c})\right)\right.}}\end{array}$$
$$T_{C}={\frac{T_{e}+T_{l}}{2}}$$
```

# Results
Here is the comparison between the "compact simulator" (other simulated departments of the power plant such as electricity, control engineering unit...) connected to my point kinetic module and the same connected to the KIKO3D model with respect to the control rods positions.

![](https://github.com/virshlee/pointkin/blob/main/comp1.png)
![](https://github.com/virshlee/pointkin/blob/main/comp2.png)
![](https://github.com/virshlee/pointkin/blob/main/comp3.png)

The point kinetic reacto simulation gives similar results to KIKO3D in 50-100% nominal performance.
# References
Sebestyén J. J., Gábor H., András K., & József P. (2013). Tapasztalatok csatolt 3D 
neutronkinetikai és termohidraulikai szimulációs modellekkel. 7.


J. R. Lamarsh, Introduction to Nuclear Reactor Theory, 2nd ed., Addison-Wesley, Reading, MA (1983)





