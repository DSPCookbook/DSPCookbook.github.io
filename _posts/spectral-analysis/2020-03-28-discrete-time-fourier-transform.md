---
layout: recipe
title: Discrete-time Fourier transform (DTFT)
modified: 2020-03-28
excerpt: Something with sinusoids.
categories: [Spectral Analysis]
tags: [Fourier, Spectral Analysis, DTFT]
---


## 1) Definition

The discrete-time Fourier transform (DTFT) is the equivalent of the Fourier transform for discrete time-series. With the DTFT, the signal is discrete in time and continouos in frequency. The DTFT is defined as

$$ X(\omega) = \sum_{n=-\infty}^{\infty} x(n)\exp\left( - i \omega n \right),  $$

where $\omega$ is the angular velocity, $n$ is the time index, and $\sqrt{-1} = i$.


## 2) Step-by-step example: Windowed cosine function

1) We define the windowed cosine function as

  $$ x(n) = w(n)\cos(\omega_0 n)$$

  where $w(n)$ is a rectangular window defined as

  $$w(n) = \begin{cases} 
  1, \quad -N \leq n \leq N \\ 
  0, \quad \text{otherwise}
  \end{cases}$$

2) Transform $w(n)$ and $\cos(\omega_0 n)$ into the frequency using the DTFT

  $$W(\omega) = \text{DTFT}\lbrace w(n) \rbrace= \frac{\sin\left( \omega\left( N + \frac{1}{2} \right) \right)}{\sin\left(\frac{\omega}{2}\right)}$$

  and

  $$\text{DTFT}\lbrace \cos(\omega_0 n) \rbrace = \pi \sum_{k=-\infty}^{\infty} \left( \delta(\omega - \omega_0 - 2\pi k) - \delta(\omega + \omega_0 + 2\pi k)\right)  $$

3) Derive the expression of $X(\omega)$
	
  $$X(\omega) = W(\omega) \ast \left( \pi \sum_{k=-\infty}^{\infty} \left( \delta(\omega - \omega_0 - 2\pi k) - \delta(\omega + \omega_0 + 2\pi k)\right) \right)$$

  where $\ast$ is the convolution operator. As $\ast$ is a linear operator we may further reduce the expression to 

  $$\begin{split}
  X(\omega) &= \pi  \sum_{k=-\infty}^{\infty} W(\omega) \ast  \left( \delta(\omega - \omega_0 - 2\pi k) - \delta(\omega + \omega_0 + 2\pi k)\right)  \\
  &= \pi  \sum_{k=-\infty}^{\infty} \left( W(\omega - \omega_0 - 2\pi k) - W(\omega + \omega_0 + 2\pi k) \right)
  \end{split}
  $$

  Hence the DTFT of a windowed cosine, is a frequency shifted sinc-function by $\omega_0$ that repeats itself with a periodicity of $2\pi$.


## 3) Step-by-step practical example: Windowed cosine function 

1) We define the windowed cosine function as

  $$ x(n) = w(n)\cos(\omega_0 n)$$

  where $w(n)$ is a rectangular window defined as

  $$w(n) = \begin{cases} 
  1, \quad -N \leq n \leq N \\ 
  0, \quad \text{otherwise}
  \end{cases}$$

  Therefore, $x(n)$ may be expressed as

  $$x(n) = \begin{cases} 
  \cos(\omega_0 n), \quad &-N \leq n \leq N \\ 
  0, \quad &\text{otherwise}
  \end{cases}$$

2) Compute the DTFT in your desired frequency range e.g. $\omega \in [-4\pi,4\pi]$ as

  $$ X(\omega) = \sum_{n=-N}^{N} x(n)\exp\left( - i \omega n \right).$$


<figure>
  <img src="/images/spectral-analysis/dtft-cosine.png" alt="this is a placeholder image">
</figure>

## 4) MATLAB code


```matlab
clc, clear, close all

% Generate example signal
N       = 50;
n       = (-N:N)';
omega0  = pi/4;
x       = sum(cos(omega0.*n),2);
omega   = -4*pi:0.001:4*pi;

% Step 1 + 2
for k = 1:length(omega)
   X(k,1) = sum((x).*exp(-1j*omega(k)*n),1);
end

% Plot the DTFT
plot(omega,real(X))

grid, xlabel('\omega [rad/s]'),ylabel('Amplitude')
xticks([-4*pi -3*pi -2*pi -pi 0 pi 2*pi 3*pi 4*pi])
xticklabels({'-4\pi','-3\pi','-2\pi','-\pi','0','\pi','2\pi','3\pi','4\pi'})
xlim([-4*pi 4*pi])
```
