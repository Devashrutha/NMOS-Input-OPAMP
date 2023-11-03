
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
      <a href="#Introduction">Introduction</a>
      <ul>
        <li><a href="#requirements">Specifications</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
      </ul>
    </li>
    <li><a href="#Design of M1 and M2">Design of M1 and M2</a></li>
    <li><a href="#Design of M3 and M4">Design of M3 and M4</a></li>
    <li><a href="#Design of M5 and M8">Design of M5 and M8</a></li>
    <li><a href="#Design of M6">Design of M6</a></li>
    <li><a href="#Design of M7">Design of M7</a></li>
    <li><a href="Layout Design">Layout Design</a></li>
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

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mi>B</mi>
  <mi>e</mi>
  <mi>t</mi>
  <msub>
    <mi>a</mi>
    <mi>p</mi>
  </msub>
  <mo>=</mo>
  <msub>
    <mi>μ</mi>
    <mi>p</mi>
  </msub>
  <mtext></mtext>
  <mrow>
    <mi>C</mi>
    <mi>o</mi>
    <mi>x</mi>
  </mrow>
  <mfenced open="(" close=")" separators="|">
    <mrow>
      <mfrac>
        <mi>W</mi>
        <mi>L</mi>
      </mfrac>  
    </mrow> 
  </mfenced>
  <mtext></mtext>
  <mo></mo>
  <mi>a</mi>
  <mi>n</mi>
  <mi>d</mi>
  <mo></mo>
  <mtext></mtext>
  <mi>B</mi>
  <mi>e</mi>
  <mi>t</mi>
  <msub>
    <mi>a</mi>
    <mi>n</mi>
  </msub>
  <mo>=</mo>
  <msub>
    <mi>μ</mi>
    <mi>n</mi>
  </msub>
  <mtext></mtext>
  <mrow>
    <mi>C</mi>
    <mi>o</mi>
    <mi>x</mi>
  </mrow>
  <mfenced open="(" close=")" separators="|">
    <mrow>
      <mfrac>
        <mi>W</mi>
        <mi>L</mi>
      </mfrac>  
    </mrow>  
  </mfenced>
</math>

We end up getting: 
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <msub>
    <mi>μ</mi>
    <mi>p</mi>
  </msub>
  <mtext>&#xA0;Cox&#xA0;</mtext>
  <mo>=</mo>
  <mn>160</mn>
  <mi>μ</mi>
  <mrow>
    <mi mathvariant="normal">A</mi>
  </mrow>
  <mrow>
    <mo>/</mo>
  </mrow>
  <msup>
    <mi>V</mi>
    <mn>2</mn>
  </msup>
  <mo></mo>
  <mi>a</mi>
  <mi>n</mi>
  <mi>d</mi>
  <mo></mo>
  <msub>
    <mi>μ</mi>
    <mi>n</mi>
  </msub>
  <mtext>&#xA0;Cox&#xA0;</mtext>
  <mo>=</mo>
  <mn>315</mn>
  <mi>μ</mi>
  <mrow>
    <mi mathvariant="normal">A</mi>
  </mrow>
  <mrow>
    <mo>/</mo>
  </mrow>
  <msup>
    <mi>V</mi>
    <mn>2</mn>
  </msup>
</math>

Now let us consider the minimum ```L = 240nm``` (L>2*90nm) to avoid any effect of Channel Length Modulation, ```Cc = 600fF``` and the ```Slew Rate = 20uA``` (Ibias/Cc, ends up to be 12uA but its the minimum). Using the following schematic can find the W/L ratios.

<!-- USAGE EXAMPLES -->
## Design of M1 and M2

The design of M1 and M2 depends on the ```GBP``` and ```Cc```

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>g</mi>
              <mrow>
                <msub>
                  <mi>m</mi>
                  <mn>1</mn>
                </msub>
              </mrow>
            </msub>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>=</mo>
            <mi>G</mi>
            <mi>B</mi>
            <mi>P</mi>
            <mo>×</mo>
            <msub>
              <mi>C</mi>
              <mi>c</mi>
            </msub>
            <mo>×</mo>
            <mn>2</mn>
            <mi>π</mi>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>g</mi>
              <mrow>
                <msub>
                  <mi>m</mi>
                  <mn>1</mn>
                </msub>
              </mrow>
            </msub>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>=</mo>
            <mn>113.1</mn>
            <mi>μ</mi>
            <mrow>
              <mi mathvariant="normal">S</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
            <mo>∴</mo>
            <mi>L</mi>
            <mi>e</mi>
            <mi>t</mi>
            <mtext></mtext>
            <msub>
              <mi>g</mi>
              <mrow>
                <msub>
                  <mi>m</mi>
                  <mn>1</mn>
                </msub>
              </mrow>
            </msub>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>=</mo>
            <mn>120</mn>
            <mi>μ</mi>
            <mrow>
              <mi mathvariant="normal">s</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>
<br/><br/>
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
            <msub>
              <mi>I</mi>
              <mi>D</mi>
            </msub>
            <mo>=</mo>
            <msub>
              <mi>μ</mi>
              <mi>n</mi>
            </msub>
            <mi>cox</mi>
            <mo data-mjx-texclass="NONE">⁡</mo>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mo>⋅</mo>
            <mfrac>
              <msup>
                <mfenced open="(" close=")" separators="|">
                  <mrow>
                    <msub>
                      <mi>V</mi>
                      <mrow>
                        <mi>G</mi>
                        <mi>S</mi>
                      </mrow>
                    </msub>                
                    <mo>−</mo>                
                    <msub>
                      <mi>V</mi>
                      <mi>t</mi>
                    </msub>                
                  </mrow>                
                </mfenced>
                <mn>2</mn>
              </msup>
              <mn>2</mn>
            </mfrac>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
            <msup>
              <mrow>
                <msub>
                  <mi>g</mi>
                  <mi>m</mi>
                </msub>
              </mrow>
              <mn>2</mn>
            </msup>
            <mo>=</mo>
            <mn>2</mn>
            <msub>
              <mi>I</mi>
              <mi>D</mi>
            </msub>
            <mo>×</mo>
            <msub>
              <mi>μ</mi>
              <mi>n</mi>
            </msub>
            <mi>cox</mi>
            <mo data-mjx-texclass="NONE">⁡</mo>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mo>=</mo>
            <mn>2.28575</mn>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
            <mo>∴</mo>
            <mi>L</mi>
            <mi>e</mi>
            <mi>t</mi>
            <mo></mo>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mo>=</mo>
            <mn>3</mn>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- Design of M3 and M4 -->
## Design of M3 and M4

The design of M3 and M4 depends on ```ICMR+```, consider only M3 and M1 from the OPAMP schematic.

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>I</mi>
              <mn>3</mn>
            </msub>
            <mo>=</mo>
            <msub>
              <mi>μ</mi>
              <mi>p</mi>
            </msub>
            <mrow data-mjx-texclass="OP">
              <msub>
                <mi mathvariant="normal">C</mi>
                <mrow>
                  <mi mathvariant="normal">o</mi>
                  <mi mathvariant="normal">x</mi>
                </mrow>
              </msub>
            </mrow>
            <mo data-mjx-texclass="NONE">⁡</mo>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mfrac>
              <msup>
                <mfenced open="(" close=")" separators="|">
                  <mrow>
                    <msub>
                      <mi>V</mi>
                      <mrow>
                        <mi>G</mi>
                        <mi>S</mi>
                      </mrow>
                    </msub>                
                    <mo>−</mo>                
                    <msub>
                      <mi>V</mi>
                      <mrow>
                        <mi>t</mi>
                      </mrow>
                    </msub>                
                  </mrow>                
                </mfenced>
                <mn>2</mn>
              </msup>
              <mn>2</mn>
            </mfrac>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>V</mi>
              <mrow>
                <mi>G</mi>
                <mi>S</mi>
              </mrow>
            </msub>
            <mo>=</mo>
            <msqrt>
              <mfrac>
                <mrow>
                  <mn>2</mn>
                  <msub>
                    <mi>I</mi>
                    <mn>3</mn>
                  </msub>
                </mrow>
                <mrow>
                  <msub>
                    <mi>μ</mi>
                    <mi>p</mi>
                  </msub>
                  <msub>
                    <mi>C</mi>
                    <mrow>
                      <mi>o</mi>
                      <mi>x</mi>
                    </mrow>
                  </msub>
                </mrow>
              </mfrac>
            </msqrt>
            <mo>+</mo>
            <mfenced open="|" close="|" separators="|">
              <mrow>
                <msub>
                  <mi>V</mi>
                  <mrow>
                    <mi>t</mi>
                    <mn>3</mn>
                  </mrow>
                </msub>            
              </mrow>            
            </mfenced>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mo>∴</mo>
  <msub>
    <mi>V</mi>
    <mrow>
      <mi>D</mi>
      <mn>1</mn>
    </mrow>
  </msub>
  <mo>=</mo>
  <msub>
    <mi>V</mi>
    <mrow>
      <mi>D</mi>
      <mi>D</mi>
    </mrow>
  </msub>
  <mo>−</mo>
  <msqrt>
    <mfrac>
      <mrow>
        <mn>2</mn>
        <msub>
          <mi>I</mi>
          <mn>3</mn>
        </msub>
      </mrow>
      <msub>
        <mi>β</mi>
        <mi>p</mi>
      </msub>
    </mfrac>
  </msqrt>
  <mo>−</mo>
  <mfenced open="|" close="|" separators="|">
    <mrow>
      <msub>
        <mi>V</mi>
        <mrow>
          <mi>t</mi>
          <mn>3</mn>
        </mrow>
      </msub>  
    </mrow>  
  </mfenced>
</math>

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
            <mi>I</mi>
            <mi>C</mi>
            <mi>M</mi>
            <msup>
              <mi>R</mi>
              <mrow>
                <mo>+</mo>
              </mrow>
            </msup>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>≤</mo>
            <msub>
              <mi>V</mi>
              <mrow>
                <msub>
                  <mi>D</mi>
                  <mn>1</mn>
                </msub>
              </mrow>
            </msub>
            <mo>+</mo>
            <msub>
              <mi>V</mi>
              <mrow>
                <msub>
                  <mi>t</mi>
                  <mn>1</mn>
                </msub>
              </mrow>
            </msub>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>
<br></br>
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <msub>
    <mfenced open="(" close=")" separators="|">
      <mrow>
        <mfrac>
          <mi>W</mi>
          <mi>L</mi>
        </mfrac>    
      </mrow>    
    </mfenced>
    <mn>3,4</mn>
  </msub>
  <mo>=</mo>
  <mfrac>
    <mrow>
      <mn>2</mn>
      <mi>I</mi>
      <msub>
        <mi>D</mi>
        <mn>3</mn>
      </msub>
    </mrow>
    <msup>
      <mfenced open="" close="]" separators="|">
        <mrow>
          <msub>
            <mi>μ</mi>
            <mi>p</mi>
          </msub>      
          <msub>
            <mi>C</mi>
            <mrow>
              <mi>o</mi>
              <mi>x</mi>
            </mrow>
          </msub>      
          <msub>
            <mfenced open="[" close="]" separators="|">
              <mrow>
                <msub>
                  <mi>V</mi>
                  <mrow>
                    <mi>D</mi>
                    <mi>D</mi>
                  </mrow>
                </msub>            
                <mo>−</mo>            
                <mi>I</mi>            
                <mi>C</mi>            
                <mi>M</mi>            
                <msup>
                  <mi>R</mi>
                  <mrow>
                    <mo>+</mo>
                  </mrow>
                </msup>            
                <mo>−</mo>            
                <msub>
                  <mfenced open="|" close="|" separators="|">
                    <mrow>
                      <msub>
                        <mi>V</mi>
                        <mrow>
                          <mi>t</mi>
                          <mn>3</mn>
                        </mrow>
                      </msub>                  
                    </mrow>                  
                  </mfenced>
                  <mrow>
                    <mtext>max&#xA0;</mtext>
                  </mrow>
                </msub>            
                <mo>+</mo>            
                <msub>
                  <mi>V</mi>
                  <mrow>
                    <mi>t</mi>
                    <mn>1</mn>
                  </mrow>
                </msub>            
              </mrow>            
            </mfenced>
            <mrow>
              <mtext>min&#xA0;</mtext>
            </mrow>
          </msub>      
        </mrow>      
      </mfenced>
      <mn>2</mn>
    </msup>
  </mfrac>
</math>
<br></br>

Now we need to find Vt3max and Vt1min. This can be done using another schematic consisting of only the differential input stage of the OPAMP and simulating the DC oerating points in the ADE.

We then get:

```Vt3max``` = -227.588mV, Let it be ```-240mV```

```Vt1min``` = 178.866mV, Let it be ```160mV```

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
              <msub>
    <mfenced open="(" close=")" separators="|">
      <mrow>
        <mfrac>
          <mi>W</mi>
          <mi>L</mi>
        </mfrac>    
      </mrow>    
    </mfenced>
    <mn>3,4</mn>
  </msub>
            <mo>=</mo>
            <mn>8.681</mn>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
            <maligngroup/>
            <mo>∴</mo>
            <mi>L</mi>
            <mi>e</mi>
            <mi>t</mi>
            <mo></mo>
    <msub>
    <mfenced open="(" close=")" separators="|">
      <mrow>
        <mfrac>
          <mi>W</mi>
          <mi>L</mi>
        </mfrac>    
      </mrow>    
    </mfenced>
    <mn>3,4</mn>
  </msub>
            <mo>=</mo>
            <mn>9</mn>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>




See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M5 and M8 -->
## Design of M5 and M8

The design of M5 and M8 depends on ```ICMR-```, consider only M2 and M5 from the OPAMP schematic.

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <munder accent="true" accentunder="false">
              <mrow>              
                <msub>
                  <mi>V</mi>
                  <mrow>
                    <mo data-mjx-texclass="OP" movablelimits="true">min</mo>
                  </mrow>
                </msub>              
              </mrow>              
              <mfenced open="(" close=")" separators="|">
                <mrow>
                  <mi>I</mi>              
                  <mi>C</mi>              
                  <mi>M</mi>              
                  <msup>
                    <mi>R</mi>
                    <mrow>
                      <mo>−</mo>
                    </mrow>
                  </msup>              
                </mrow>              
              </mfenced>
            </munder>
            <mo>⩾</mo>
            <mrow>
              <msub>
                <mi>V</mi>
                <mrow>
                  <mi>G</mi>
                  <mi>S</mi>
                  <msub>
                    <mn>1</mn>
                    <mrow>
                      <mi>m</mi>
                      <mi>a</mi>
                      <mi>x</mi>
                    </mrow>
                  </msub>
                </mrow>
              </msub>
              <mo>+</mo>
              <msub>
                <mi>V</mi>
                <mrow>
                  <mtext>DSat5&#xA0;</mtext>
                </mrow>
              </msub>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi>I</mi>
            <mi>C</mi>
            <mi>M</mi>
            <msup>
              <mi>R</mi>
              <mrow>
                <mo>−</mo>
              </mrow>
            </msup>
            <mo>⩾</mo>
            <msub>
              <mfenced open="[" close="]" separators="|">
                <mrow>
                  <msqrt>
                    <mfrac>
                      <mrow>
                        <mn>2</mn>
                        <msub>
                          <mi>I</mi>
                          <mrow>
                            <msub>
                              <mi>D</mi>
                              <mn>1</mn>
                            </msub>
                          </mrow>
                        </msub>
                      </mrow>
                      <msub>
                        <mi>β</mi>
                        <mn>1</mn>
                      </msub>
                    </mfrac>
                  </msqrt>              
                  <mo>+</mo>              
                  <mi>V</mi>              
                  <msub>
                    <mi>t</mi>
                    <mn>1</mn>
                  </msub>              
                </mrow>              
              </mfenced>
              <mrow>
                <mo data-mjx-texclass="OP" movablelimits="true">max</mo>
              </mrow>
            </msub>
            <mo>+</mo>
            <msub>
              <mi>V</mi>
              <mrow>
                <mtext>Dsat&#xA0;</mtext>
              </mrow>
            </msub>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>V</mi>
              <mrow>
                <mtext>Dsat&#xA0;</mtext>
              </mrow>
            </msub>
            <mo>≤</mo>
            <mi>I</mi>
            <mi>C</mi>
            <mi>M</mi>
            <msup>
              <mi>R</mi>
              <mrow>
                <mo>−</mo>
              </mrow>
            </msup>
            <mo>−</mo>
            <msqrt>
              <mfrac>
                <mrow>
                  <mn>2</mn>
                  <msub>
                    <mi>I</mi>
                    <mrow>
                      <mi>D</mi>
                      <mn>1</mn>
                    </mrow>
                  </msub>
                </mrow>
                <msub>
                  <mi>β</mi>
                  <mn>1</mn>
                </msub>
              </mfrac>
            </msqrt>
            <mo>−</mo>
            <msub>
              <mi>V</mi>
              <mrow>
                <msub>
                  <mi>t</mi>
                  <mn>1</mn>
                </msub>
                <mo data-mjx-texclass="OP" movablelimits="true">max</mo>
              </mrow>
            </msub>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>V</mi>
              <mrow>
                <mtext>Dsat&#xA0;</mtext>
              </mrow>
            </msub>
            <mo>=</mo>
            <mn>468.521</mn>
            <mrow>
              <mi mathvariant="normal">m</mi>
              <mi mathvariant="normal">V</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mtext>But VDsat minimum is&#xA0;</mtext>
            <mn>184</mn>
            <mrow>
              <mi mathvariant="normal">m</mi>
              <mi mathvariant="normal">V</mi>
            </mrow>
            <mtext>. Let us take&#xA0;</mtext>
            <mn>200</mn>
            <mrow>
              <mi mathvariant="normal">m</mi>
              <mi mathvariant="normal">V</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>∴</mo>
            <mtext>&#xA0;VDsat&#xA0;</mtext>
            <mo>=</mo>
            <mn>200</mn>
            <mrow>
              <mi mathvariant="normal">m</mi>
              <mi mathvariant="normal">V</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mfenced open="(" close=")" separators="|">
                <mrow>
                  <mfrac>
                    <mi>W</mi>
                    <mi>L</mi>
                  </mfrac>              
                </mrow>              
              </mfenced>
              <mrow>
                <mn>5</mn>
                <mo>,</mo>
                <mn>8</mn>
              </mrow>
            </msub>
            <mo>=</mo>
            <mfrac>
              <mrow>
                <mn>2</mn>
                <msub>
                  <mi>I</mi>
                  <mrow>
                    <mi>D</mi>
                    <mn>5</mn>
                  </mrow>
                </msub>
              </mrow>
              <mrow>
                <msub>
                  <mi>μ</mi>
                  <mi>n</mi>
                </msub>
                <msub>
                  <mi>C</mi>
                  <mrow>
                    <mi>o</mi>
                    <mi>x</mi>
                  </mrow>
                </msub>
                <msup>
                  <mfenced open="(" close=")" separators="|">
                    <mrow>
                      <msub>
                        <mi>V</mi>
                        <mrow>
                          <mtext>Dsat&#xA0;</mtext>
                        </mrow>
                      </msub>                  
                    </mrow>                  
                  </mfenced>
                  <mn>2</mn>
                </msup>
              </mrow>
            </mfrac>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>∴</mo>
            <msub>
              <mfenced open="(" close=")" separators="|">
                <mrow>
                  <mfrac>
                    <mi>W</mi>
                    <mi>L</mi>
                  </mfrac>              
                </mrow>              
              </mfenced>
              <mrow>
                <mn>5</mn>
                <mo>,</mo>
                <mn>8</mn>
              </mrow>
            </msub>
            <mo>=</mo>
            <mn>3</mn>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>



<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M6 -->
## Design of M6

The design of M6 depends on ```PM```, to achieve 60 degrees PM:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnspacing="1em" rowspacing="3pt">
    <mtr>
      <mtd>
        <msub>
          <mi>g</mi>
          <mrow>
            <mi>m</mi>
            <mn>6</mn>
          </mrow>
        </msub>
        <mo>⩾</mo>
        <mn>10</mn>
        <mo>⋅</mo>
        <msub>
          <mi>g</mi>
          <mrow>
            <mi>m</mi>
            <mn>1</mn>
          </mrow>
        </msub>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <msub>
          <mi>g</mi>
          <mrow>
            <mi>m</mi>
            <mn>6</mn>
          </mrow>
        </msub>
        <mo>⩾</mo>
        <mn>1200</mn>
        <mi>μ</mi>
        <mo>.</mo>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mfrac>
          <msub>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mn>6</mn>
          </msub>
          <msub>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mn>4</mn>
          </msub>
        </mfrac>
        <mo>=</mo>
        <mfrac>
          <msub>
            <mi>I</mi>
            <mn>6</mn>
          </msub>
          <msub>
            <mi>I</mi>
            <mn>4</mn>
          </msub>
        </mfrac>
        <mo>=</mo>
        <mfrac>
          <msub>
            <mi>g</mi>
            <mrow>
              <mi>m</mi>
              <mn>6</mn>
            </mrow>
          </msub>
          <msub>
            <mi>g</mi>
            <mrow>
              <mi>m</mi>
              <mn>4</mn>
            </mrow>
          </msub>
        </mfrac>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mi>q</mi>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <msub>
          <mi>g</mi>
          <mrow>
            <mi>m</mi>
            <mn>4</mn>
          </mrow>
        </msub>
        <mo>=</mo>
        <msqrt>
          <msub>
            <mi>μ</mi>
            <mi>p</mi>
          </msub>
          <msub>
            <mi>C</mi>
            <mrow>
              <mi>o</mi>
              <mi>x</mi>
            </mrow>
          </msub>
          <mo>⋅</mo>
          <msub>
            <mfenced open="(" close=")" separators="|">
              <mrow>
                <mfrac>
                  <mi>W</mi>
                  <mi>L</mi>
                </mfrac>            
              </mrow>            
            </mfenced>
            <mn>4</mn>
          </msub>
          <mo>⋅</mo>
          <mn>2</mn>
          <msub>
            <mi>I</mi>
            <mi>D</mi>
          </msub>
        </msqrt>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <msub>
          <mi>g</mi>
          <mrow>
            <msub>
              <mi>m</mi>
              <mn>4</mn>
            </msub>
          </mrow>
        </msub>
        <mo>=</mo>
        <mn>169.11</mn>
        <mi>μ</mi>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mo>∴</mo>
        <msub>
          <mfenced open="(" close=")" separators="|">
            <mrow>
              <mfrac>
                <mi>W</mi>
                <mi>L</mi>
              </mfrac>          
            </mrow>          
          </mfenced>
          <mn>6</mn>
        </msub>
        <mo>=</mo>
        <mn>64</mn>
      </mtd>
    </mtr>
  </mtable>
</math>


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Design of M7 -->
## Design of M7

The design of M6 depends on ```W/L of M5```, to achieve 60 degrees PM:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left right left right left right left right left right left" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mfrac>
              <msub>
                <mi>I</mi>
                <mn>6</mn>
              </msub>
              <msub>
                <mi>I</mi>
                <mn>4</mn>
              </msub>
            </mfrac>
            <mo>=</mo>
            <mfrac>
              <mrow>
                <mo stretchy="false">(</mo>
                <mi>W</mi>
                <mrow>
                  <mo>/</mo>
                </mrow>
                <mi>L</mi>
                <msub>
                  <mo stretchy="false">)</mo>
                  <mn>6</mn>
                </msub>
              </mrow>
              <mrow>
                <mo stretchy="false">(</mo>
                <mi>W</mi>
                <mrow>
                  <mo>/</mo>
                </mrow>
                <mi>L</mi>
                <msub>
                  <mo stretchy="false">)</mo>
                  <mn>4</mn>
                </msub>
              </mrow>
            </mfrac>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <msub>
              <mi>I</mi>
              <mn>6</mn>
            </msub>
            <mo>=≃</mo>
            <mn>72</mn>
            <mi>μ</mi>
            <mrow>
              <mi mathvariant="normal">A</mi>
            </mrow>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mfrac>
              <msub>
                <mfenced open="(" close=")" separators="|">
                  <mrow>
                    <mfrac>
                      <mi>W</mi>
                      <mi>L</mi>
                    </mfrac>                
                  </mrow>                
                </mfenced>
                <mn>7</mn>
              </msub>
              <msub>
                <mfenced open="(" close=")" separators="|">
                  <mrow>
                    <mfrac>
                      <mi>W</mi>
                      <mi>L</mi>
                    </mfrac>                
                  </mrow>                
                </mfenced>
                <mn>5</mn>
              </msub>
            </mfrac>
            <mo>=</mo>
            <mfrac>
              <msub>
                <mi>I</mi>
                <mn>6</mn>
              </msub>
              <msub>
                <mi>I</mi>
                <mn>5</mn>
              </msub>
            </mfrac>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mrow>
          <mrow>
            <maligngroup/>
          </mrow>
          <mrow>
            <maligngroup/>
            <mi></mi>
            <mo>∴</mo>
            <msub>
              <mfenced open="(" close=")" separators="|">
                <mrow>
                  <mfrac>
                    <mi>W</mi>
                    <mi>L</mi>
                  </mfrac>              
                </mrow>              
              </mfenced>
              <mn>7</mn>
            </msub>
            <mo>=</mo>
            <mn>12</mn>
          </mrow>
        </mrow>
      </mtd>
    </mtr>
  </mtable>
</math>

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Layout Design -->
## Layout Design


<p align="right">(<a href="#readme-top">back to top</a>)</p>




