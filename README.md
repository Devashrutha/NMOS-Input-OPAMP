
<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">NMOS input OPAMP</h3>

  <p align="center">
    Schematic and Layout design using Cadence Virtuoso at 90nm technology
    <br />
    <a href="https://github.com/Devashrutha/NMOS-Input-OPAMP/tree/main"><strong>Explore the docs »</strong></a>
    <br />
    <br />
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>INDEX</summary>
  <ol>
    <li>
      <a href="#introduction">Introduction</a>
      <ul>
        <li><a href="#requirements">Requirements</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#specifications">Specifications</a></li>
      </ul>
    </li>
    <li><a href="#design-of-m1-and-m2">Design of M1 and M2</a></li>
    <li><a href="#design-of-m3-and-m4">Design of M3 and M4</a></li>
    <li><a href="#design-of-m5-and-m8">Design of M5 and M8</a></li>
    <li><a href="#design-of-m6">Design of M6</a></li>
    <li><a href="#design-of-m7">Design of M7</a></li>
    <li><a href="layout-design">Layout Design</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## Introduction

[![OPAMP][product-screenshot]](https://upload.wikimedia.org/wikipedia/commons/9/97/Op-amp_symbol.svg)

The most popular approach for CMOS OPAMPs has been the two stage architecture. It consists of a differential input stage followed by a second gain stage. In some cases where the output is a resistive load a buffer is used. OPAMPs usually have low output impedance as they are used as amplifiers so that maximum power is transfered to the load. This design includes a NMOS input differential amplifier with NMOS current mirror and PMOS active load followed by a common source amplifier. There are several advantages for choosing NMOS input mosfets instead of PMOS. 

Here's why:
* NMOS input OPAMPS typically have low input impedance, which means they are less prone to input capacitance problems.
* They offer high speed operation for use in DSP circuits.
* They consume less power that their counterpart making them more power-efficient.
* Better noise performance.

Of course, the choice between NMOS input and PMOS input depends on the application and requirements.


<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Specifications

This section gives the required specifications that the final OPAMP design must satisfy:

* DC gain:
* Gain Bandwidth Product (GBP) : 30MHz
* Phase Margin (PM) : 60 degrees
* Slew Rate : 20V/us
* VDD : 1.8V
* Input Common Mode Randge High (ICMR+) : 1.6V
* Input Common Mode Randge Low (ICMR-) : 0.8V
* Load Capacitance (CL) : 2pF
* Power Consumption <= 300uW
* Coupling Capacitor (Cc) >= 0.22*CL



<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Using Cadence Virtuso with gpdk90 technology we will first use the above specifications to find the (W/L) ratios for the various mosfets. All the manual calculations is included in the project directory.

### Prerequisites

Before finding the (W/L) ratios we need to know the upCox and unCox values of PMOS and NMOS by driving them to saturation or region 2. Let us consider ```L = 1um, W = 10um, Ibias = 50uA, VDD = 3V```. Using the following schematic we find the DC operating points using the DC analysis in the ADE window.

The Beff(Effective beta) is found from each of the mosfets using the above W and L. Then we find the upCox and unCox using the formula: 

```math
\begin{aligned}
& Beta_p = \mu_p \ {Cox} \left(\frac{W}{L}\right)\ and\ Beta_n = \mu_n \ {Cox} \left(\frac{W}{L}\right)\\
& \mu_p \text { Cox }=160 \mu \mathrm{A} / V^2\ and \mu_n \text { Cox }=315 \mu \mathrm{A} / V^2\\
\end{aligned}
```

Now let us consider the minimum ```L = 240nm``` (L>2*90nm) to avoid any effect of Channel Length Modulation, ```Cc = 600fF``` and the ```Slew Rate = 20uA``` (Ibias/Cc, ends up to be 12uA but its the minimum). Using the following schematic can find the W/L ratios.

<!-- USAGE EXAMPLES -->
## Design of M1 and M2

The design of M1 and M2 depends on the ```GBP``` and ```Cc```

```math
\begin{aligned}
& g_{m_1} =GBP \times C_c \times 2 \pi \\
& g_{m_1} =113.1 \mu \mathrm{S}\\
& \therefore Let\ g_{m_1} =120 \mu \mathrm{s}\\
& I_D =\mu_n \text{Cox}\left(\frac{W}{L}\right) \cdot \frac{\left(V_{G S}-V_t\right)^2}{2} \\
& g_m =\frac{\partial I_{D S}}{\partial V_{G S}}=\mu_n \text{Cox}\left(\frac{W}{L}\right)\left(V_{G S}-V_T\right)\\
& {g_{m}}^2=\left[\mu_n \text{Cox}\left(\frac{w}{L}\right)\right]^2 \frac{\left(V_{G S}-V_{t}\right)^2}{2} \times 2 \\
& {g_m}^2=2 I_D \times \mu_n \text{Cox}\left(\frac{W}{L}\right)\\
& \left(\frac{W}{L}\right) = 2.28575\\
& \therefore Let\left(\frac{W}{L}\right) = 3\\
\end{aligned}
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- Design of M3 and M4 -->
## Design of M3 and M4

The design of M3 and M4 depends on ```ICMR+```, consider only M3 and M1 from the OPAMP schematic.
```math
\begin{align}
& I_3 = \mu_p \text{C}_{ox}\left(\frac{W}{L}\right) \frac{\left(V_{GS}-V_{t}\right)^2}{2} \\
& V_{GS} = \sqrt{\frac{2 I_3}{\mu_p \text{C}_{ox}}} + \left|V_{t3}\right| \\
& \therefore\, V_{D1} = V_{DD} - \sqrt{\frac{2 I_3}{\beta_p}} - \left|V_{t3}\right| \\
& ICMR^{+} &\leq V_{D1} + V_{t1} \\
& \left(\frac{W}{L}\right)_3 = \frac{2 I_{D3}}{\left.\mu_p \text{C}_{ox}\left[V_{DD} - ICMR^{+} - \left|V_{t3}\right|_{\text{max}} + V_{t1}\right]_{\text{min}}\right]^2}
\end{align}
```
Now we need to find Vt3max and Vt1min. This can be done using another schematic consisting of only the differential input stage of the OPAMP and simulating the DC oerating points in the ADE.

We then get:

```Vt3max``` = -227.588mV, Let it be ```-240mV```

```Vt1min``` = 178.866mV, Let it be ```160mV```

```math
\begin{aligned}
& \frac{W}{L}_{3,4}=8.681 \\
& \therefore \text { Let } \frac{W}{L}_{3,4}=9
\end{aligned}
```




See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M5 and M8 -->
## Design of M5 and M8

The design of M5 and M8 depends on ```ICMR-```, consider only M2 and M5 from the OPAMP schematic.

```math
\begin{aligned}
& \underset{\left(I C M R^{-}\right)}{V_{\min }} \geqslant{V_{GS1_{max}}+V_{\text {DSat5 }}} \\
& I C M R^{-} \geqslant\left[\sqrt{\frac{2 I_{D_1}}{\beta_1}}+V t_1\right]_{\max }+V_{\text {Dsat }} \\

& V_{\text {Dsat }} \leq I C M R^{-}-\sqrt{\frac{2 I_{D 1}}{\beta_1}}-V_{t_1 \max } \\

& V_{\text {Dsat }}=468.521 \mathrm{mV} \\
& \text {But VDsat minimum is } 184 \mathrm{mV} \text {. Let us take } 200 \mathrm{mV} \\
& \therefore \text { VDsat }=200 \mathrm{mV} \\
& \left(\frac{W}{L}\right)_{5,8}=\frac{2 I_{D 5}}{\mu_n C_{ox}\left(V_{\text {Dsat }}\right)^2} \\
& \therefore\left(\frac{W}{L}\right)_{5,8}=3 \\
&
\end{aligned}
```



<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M6 -->
## Design of M6

The design of M6 depends on ```PM```, to achieve 60 degrees PM:
```math
\begin{gathered}
& g_{m6} \geqslant 10 \cdot g_{m 1} \\
& g_{m6} \geqslant 1200 \mu . \\
& \frac{\left(\frac{W}{L}\right)_6}{\left(\frac{W}{L}\right)_4}=\frac{I_6}{I_4}=\frac{g_{m 6}}{g_{m 4}} \\
& q \\
& g_{m4}=\sqrt{\mu_p C_{ox}\cdot\left(\frac{W}{L}\right)_4 \cdot 2 I_D} \\

& g_{m_4}=169.11 \mu \\

& \therefore\left(\frac{W}{L}\right)_6=64
\end{gathered}
```


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M7 -->
## Design of M7

The design of M6 depends on ```W/L of M5```, to achieve 60 degrees PM:
```math
\begin{aligned}
& \frac{I_6}{I_4}=\frac{(W / L)_6}{(W / L)_4} \\
& I_6=\simeq 72 \mu \mathrm{A} \\
& \frac{\left(\frac{W}{L}\right)_7}{\left(\frac{W}{L}\right)_5}=\frac{I_6}{I_5} \\
& \therefore\left(\frac{W}{L}\right)_7=12
\end{aligned}
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Layout Design -->
## Layout Design


<p align="right">(<a href="#readme-top">back to top</a>)</p>




