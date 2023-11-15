---
title: Introduction to Semiconductor Devices
date: 2020-06-17T23:27:13-04:00
tags: ["semiconductor devices", "latex", "Multi-lingual"]
categories: ["Lecture notes"]

hiddenFromHomePage: true
hiddenFromSearch: true
twemoji: false
lightgallery: false
ruby: false
fraction: false
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: true
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: false
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---


This post contains my lecture notes of taking the course *VE320: Introduction to Semiconductor Devices* at UM-SJTU Joint Institute.

<!-- more -->

## Chapter 1: The Crystal Structure of Solids

- Unit cell vs. primitive cell
  - **primitive cell**: smallest unit cell possible
- Three lattice types
  - Simple cubic
  - Body-centered cubic
  - Face-centered cubic
- Volume density = $\dfrac{\text{number of atoms per unit cell}}{a^3}$
  - Unit: `1 Angstrom = 1e-10 m`
  - **Miller Index:** $(\frac{1}{x\text{-intersect}},\frac{1}{y\text{-intersect}},\frac{1}{z\text{-intersect}})\times\text{lcm}(x,y,z)$
    - (100) plane, (110) plane, (111) plane
    - \[100\] direction, \[110\] direction, \[111\] direction

## Chapter 2: Introduction to Quantum Mechanics

- **Plank's constant:** $h=6.625\times10^{-34}~\text{J-s}$
  - Photon's energy $E=h\nu=\hbar\omega$
  - Photon's momentum $p=\frac{h}{\lambda}$ (**de Broglie wavelength**)

### Schrodinger's Equation

Total wave function: $\Psi(x,t)=\psi(x)\phi(t)=\psi(x)e^{-j(E/\hbar)t}$.

### Time-Independent Wave Function

$$
\frac{\partial^2\psi(x)}{\partial x^2}+\frac{2m}{\hbar^2}(E-V(x))\psi(x)=0.
$$

- $|\psi(x)|^2$ is the probability density function. $\int_{-\infty}^{\infty}|\psi(x)|^2\mathrm{d}x=1$.
- Boundary conditions for solving $\psi(x)$:
  - $\psi(x)$ must be **finite,** **continuos,** **single-valued.**
  - $\partial\psi/\partial x$ must be **finite,** **continuos,** **single-valued.**
- $k$ is **wave number.** $p=\hbar k$.
- Steps of solving wave function:
  - Consider Schrodinger's equation in each region. Solve separately.
  - Apply boundary conditions to determine coefficients.

### Electron Energy in Single Atom

$$
V(r)=\frac{-e^2}{4\pi\epsilon_0 r},\quad \nabla^2\psi(r,\theta,\phi)+\frac{2m_0}{\hbar^2}(E-V(r))\psi(r,\theta,\phi)=0
$$

- Solution: $E_n=\dfrac{-m_0 e^4}{(4\pi\epsilon_0)^22\hbar^2n^2}$, $n$ is the **principal quantum number**.

## Chapter 3: Introduction to the Quantum Theory of Solids

### Formation of Energy Band

- Silicon: $1s^22s^22p^63s^23p^2$. Energy levels split and formed two large energy bands: valence band & conduction band.
- $E$ vs. $k$ curve for free particle: $E=\dfrac{k^2\hbar^2}{2m}$. (parabolic relation)
- $E$ vs. $k$ diagram in reduced zone representation (for Si crystal, derived from **Kronig-Penney model**).

### Electrical Conduction in Solids

- Drift Current:
  - $J=qNv_d$ $(\text{A}/\text{cm}^2)$, $N$ being the volume density.
  - If considering individual ion velocities, $J=q\sum_{i=1}^{N}v_i$.
- Electron **effective mass**:
  - $\dfrac{\mathrm{d}^E}{\mathrm{d}k^2}=\dfrac{\hbar^2}{m}$.

### Metals, Insulators, Semiconductors

- Metals:
  - Has a **partially filled** energy band.
- Insulators:
  - Has an **either completely filled or completely empty** energy band.

### Density of States Function

Consider 1-D crystal with $N$ quantum wells, each well of length $a$.

- Number of states of the whole crystal within $(0,\pi/a)$: $\dfrac{N}{\pi/a}$
- Number of states of the whole crystal within $\Delta k$: $\dfrac{N}{\pi/a}\times\Delta k$
- Number of states **per unit volume** within $\Delta k$: $\dfrac{N}{\pi/a}\times\Delta k\dfrac{1}{Na}=\dfrac{\Delta k}{\pi}$

For $E$ vs. $k$ relation:
$$
E=E_c+\frac{\hbar^2}{2m^*_n}k^2,\qquad k=\pm\frac{\sqrt{2m^*_n(E-E_c)}}{\hbar}
$$

Extend to 3-D sphere:
$$
g(E)=\frac{1}{8}\frac{\mathrm{d}(4\pi/3\times(k/\pi)^3)}{\mathrm{d}E}
$$

Final conclusion:
$$
\text{Conduction band: }g_c(E)=\frac{4\pi(2m^*_n)^{3/2}}{h^3}\sqrt{E-E_c}
$$

$$
\text{Valence band: }g_v(E)=\frac{4\pi(2m^*_p)^{3/2}}{h^3}\sqrt{E_v-E}
$$

- $m^*_n$ is the **density of states effective mass** for electrons.
- $m^*_p$ is the **density of states effective mass** for holes.
- $E_c$ is the bottom edge of conduction band.
- $E_v$ is the top edge of valence band.

### Statistical Mechanics

Fermi-Dirac distribution:
$$
f_F(E)=\frac{1}{1+\exp\left(\dfrac{E-E_F}{kT}\right)}
$$

- represents the probability of a quantum state is **occupied** by an electron.
- $k$ is the Boltzmann Constant. `k = 8.62e-5 eV/K`.

When $E-E_F\gg kT$, 1 at the denominator is ignored. Fermi-Dirac distribution becomes Maxwell-Boltzmann distribution.
$$
f_F(E)=\frac{1}{\exp\left(\dfrac{E-E_F}{kT}\right)}=\exp\left(-\frac{E-E_F}{kT}\right)
$$

## Chapter 4: Semiconductor in Equilibrium

### Thermal-Equilibrium Carrier Concentration

Electron (**use Maxwell-Boltzmann approximation**):
$$
n_0=\int g_c(E)f_F(E)\mathrm{d}E\quad\Rightarrow\quad n_0=2\left(\frac{2\pi m^*_n kT}{h^2}\right)^{3/2}\exp\left[-\frac{E_c-E_F}{kT}\right]
$$

For simplicity,
$$
n_0=N_c\exp\left[-\frac{E_c-E_F}{kT}\right]
$$

- $N_c$ is called **effective density of states function in the conduction band.**

Hole (**use Maxwell-Boltzmann approximation**):
$$
p_0=2\left(\frac{2\pi m^*_p kT}{h^2}\right)^{3/2}\exp\left[-\frac{E_F-E_v}{kT}\right]
$$

For simplicity,
$$
p_0=N_v\exp\left[-\frac{E_F-E_v}{kT}\right]
$$

- $N_v$ is called **effective density of states function in the valence band.**

### Intrinsic Carrier Concentration

$n_i=n_0=p_0$ for intrinsic semiconductors. Therefore,
$$
n_i=\sqrt{n_0p_0}=\sqrt{N_c N_v}\exp\left(-\frac{E_g}{2kT}\right)
$$

### Intrinsic Fermi Level Position

For intrinsic semiconductors, $n_0=p_0$, $E_F=E_{Fi}$. Therefore, equating the previous $n_0$ and $p_0$ expressions, we get
$$
E_{Fi}-E_\text{midgap}=\frac{3}{4}kT\ln\left(\frac{m^*_p}{m^**n}\right),\quad\text{where}\quad \frac{1}{2}(E_c+E_v)=E*\text{midgap}
$$

### Dopant Atoms and Energy Levels

Introduce $E_d$ in $n$-type semiconductor, which is the discrete donor energy state. Therefore, $E_c-E_d$ is the **ionization energy.**

Introduce $E_a$ in $p$-type semiconductor, which is the discrete acceptor energy state. Therefore, $E_a-E_v$ is the **ionization energy.**

### The Extrinsic Semiconductor

An *extrinsic semiconductor* is defined as a semiconductor in which controlled amounts of specific dopant or impurity atoms have been added so that the thermal-equilibrium electron and hole concentrations are different from the intrinsic carrier concentration.

#### Equilibrium Distribution of Electrons and Holes

$$
n_0=n_i\exp\left[\frac{E_F-E_{Fi}}{kT}\right]
$$

$$
p_0=n_i\exp\left[-\frac{E_F-E_{Fi}}{kT}\right]
$$

- $n_0p_0=n_i^2$ still holds.

#### Degenerate and Non-degenerate Semiconductor

When the donor concentration is high enough to split the discrete donor energy state, the semiconductor is called a *degenerate $n$-type semiconductor*.

When the acceptor concentration is high enough to split the discrete acceptor energy state, the semiconductor is called a *degenerate $p$-type semiconductor*.

#### Statistics of Donors and Acceptors

$$
f_D(E)=\frac{1}{1+\dfrac{1}{2}\exp\left(\dfrac{E_d-E_F}{kT}\right)},\qquad n_d=N_d-N_d^+.
$$

- Reason for $1/2$: in conduction band energy states, each state can be occupied by at most 2 electrons (spin up & spin down); while in donor energy state, each state can only be occupied by 1 electron (either spin up or spin down).

$$
n_d=\frac{N_d}{1+\dfrac{1}{2}\exp\left(\dfrac{E_d-E_F}{kT}\right)},\qquad n_d=N_d-N_d^+.
$$

- $n_d$ represents **the *density* of electrons occupying the donor state**.
- $n_d=N_d\times f_D(E_d)$.

$$
p_a=\frac{N_a}{1+\dfrac{1}{g}\exp\left(\dfrac{E_F-E_a}{kT}\right)},\qquad p_a=N_a-N_a^+.
$$

- $p_a$ represents **the probability of holes occupying the acceptor state**.
- $g$, the *degeneracy factor*, is **normally taken as 4**.

*Complete ionization*: all donor/acceptor atoms have donated an electron/a hole to the conduction band/valence band.

*Freeze-out*: all donor/acceptor energy states are filled with electrons/holes.

### Charge Neutrality

A *compensated semiconductor* is one that contains both donor and acceptor impurity atoms in the same region.

- $n$-type compensated: $N_d>N_a$
- $p$-type compensated: $N_a>N_d$

#### Equilibrium Electron and Hole Concentrations

At equilibrium, overall charge is neutral.
$$
n_0+N_a^-=p_0+N_d^+\quad\Leftrightarrow\quad n_0+(N_a-p_a)=p_0+(N_d-n_d)
$$

*Assume complete ionization*:
$$
n_0+N_a=\frac{n_i^2}{n_0}+N_d\quad\Rightarrow\quad n_0=\frac{(N_d-N_a)}{2}+\sqrt{\left(\frac{N_d-N_a}{2}\right)^2+n_i^2}.
$$

- Only valid for an **$n$-type semiconductor**!

*Assume complete ionization*:
$$
\frac{n_i^2}{p_0}+N_a=p_0+N_d\quad\Rightarrow\quad p_0=\frac{(N_a-N_d)}{2}+\sqrt{\left(\frac{N_a-N_d}{2}\right)^2+n_i^2}.
$$

- Only valid for an **$p$-type semiconductor**!

*Incomplete ionization:*

- When $T$ is very high, $n_i\gg N_d^+$. Therefore $n_0=n_i=\sqrt{N_cN_v}\exp\left(\dfrac{-E_g}{2kT}\right)$;
- When $T$ is not high:

$$
n_0=N_d^+=N_d\left(1-\frac{1}{1+\dfrac{1}{2}\exp\left(\dfrac{E_d-E_F}{kT}\right)}\right)
$$

$$
n_0=\frac{N_d}{1+2\exp\left(-\dfrac{E_d-E_F}{kT}\right)}
$$

$$
n_0=\frac{N_d}{1+2\exp\left(\dfrac{E_c-E_d}{kT}\right)\exp\left(\dfrac{E_F-E_c}{kT}\right)}
$$

$$
n_0=\frac{N_d}{1+2\exp\left(\dfrac{E_c-E_d}{kT}\right)\dfrac{n_0}{N_c}}
$$

$$
2\exp\left(\frac{E_c-E_d}{kT}\right)n_0^2+N_cn_0-N_dN_c=0
$$

Final conclusion:
$$
n_0=N_c\times\frac{-1+\sqrt{1+\dfrac{8N_d}{N_c}\exp\left(\dfrac{E_c-E_d}{kT}\right)}}{4\exp\left(\dfrac{E_c-E_d}{kT}\right)}.
$$

### Position of Fermi Energy Level

With respect to intrinsic Fermi energy level:
$$
n_0=n_i\exp\left(\dfrac{E_F-E_{Fi}}{kT}\right)\quad\Rightarrow\quad E_F-E_{Fi}=kT\ln\left(\frac{n_0}{n_i}\right)
$$
$$
p_0=n_i\exp\left(\dfrac{E_{Fi}-E_F}{kT}\right)\quad\Rightarrow\quad E_{Fi}-E_F=kT\ln\left(\frac{p_0}{n_i}\right)
$$

With respect to valence band energy level:
$$
p_o=N_v\exp\left(\dfrac{E_F-E_v}{kT}\right)\quad\Rightarrow\quad E_F-E_v=kT\ln\left(\frac{N_v}{p_0}\right)
$$

- if we assume $N_a\gg n_i$, then $E_F-E_v=kT\ln\left(\dfrac{N_v}{N_a}\right)$

With respect to conduction band energy level:
$$
E_F-E_c=kT\ln\left(\frac{n_0}{N_c}\right).
$$

- **Note.** $n_0$ can be derived from complete ionization **OR** incomplete ionization formulas.
