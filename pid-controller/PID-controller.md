# PID Controller

Notes created on 12/25/2025

![PID-controller-diagram](assets/960px-PID_en.jpg)

> A block diagram of a PID controller in a **feedback loop**. (source: Wikipedia)

- Input reference signal $r(t)$ 
- Error signal $e(t) = r(t) - y(t)$ 
- Control signal $u(t)$ 
- Output signal $ y(t) $ 
- Controller gain $K_p,K_i,K_d$ 

- Goal: $e(t) \rarr 0$, $ y(t) $ matches $r(t)$



### Control Law

$u(t) = K_p \cdot e(t) + K_i \cdot \frac{1}{s} \text{(Integrator)} \cdot e(t) + K_d \cdot s \text{(differentiator)} \cdot e(t)$ 

$ u(t) = \underbrace{K_p \cdot e(t)}_{u_p(t)} + \underbrace{K_i \cdot \int_{0}^{t} e(\tau)d\tau}_{u_i(t)} + \underbrace{K_d \cdot \frac{de(t)}{dt}}_{u_d(t)} $ 



### 1. Proportional Control (P)

![P-controller-diagram](assets/proportional.jpg)

- The larger the error, the stronger the control signal $U_p(t)$
- Provides stability against small disturbances, but insufficient for dealing with a steady disturbance (leaves a steady-state error 稳态误差)
  - most systems leaves a non-zero s.s. error with only the P controller

- Think of the P Controller as a **rubber band** connecting a weight’s actual position and the target position. 

  - The stiffness of the rubber band is the Proportional Gain $K_p$. 

  - The rubber band oscillates up and down until it settles at a point where the rubber band is stretched *just enough* to counteract gravity. That stretch is the steady-state error. 

  - Ideally, the larger $K_p$, the smaller the steady-state error: $e_{ss} \approx \frac{1}{1 + K_p}$. 

  - However, high gain causes violent oscillation and the system will be unstable. 



### 2. Integral Control (I)

![I-controller-diagram](assets/integral.jpg)

- The main controlling force
- **Eliminates the steady-state error**
  - If a non-zero s.s. error exists, the control signal $u_i(t)$ will increase towards infinity! 
- **Oscillates** the error signal above and below zero until it reaches the zero s.s. error. 



### 3. Derivative Control (D)

![derivative](assets/derivative.jpg)

- The **damper of the system**, reducing oscillations. 
- The more dynamic the error signal $e(t)$ is, the larger its rate of change $\frac{de(t)}{dt}$ is, so the stronger the D control signal $K_d \cdot \frac{de(t)}{dt}$ is. 
- When the error reaches steady-state (flat), no matter zero or non-zero, the D control signal becomes zero too. 



## Practical Implementations

### 1. Amplified noise in pure derivative $s$

- ⚠️ With just **a standalone pure derivative** $[s]$ , **any noise will be amplified and it corrupts the output signal**. 
  - D controller output $u_d(t) = K_d \cdot s \cdot e(t) \text{(Laplace domain)} = K_d \cdot \frac{de(t)}{dt} \text{(pure derivative form)}$ 
  - ⛔️ Predictive (forward), **non-causal derivative**:  $\dot{e}(k) = \frac{e(k+1) - e(k)}{T}$, $T$ is the small time step. However, $e(k+1)$ is in the future, we don’t know. 
  - ✅ Approximate (backward), **causal derivative**: $\dot{\tilde{e}}(k) = \frac{e(k)-e(k-1)}{T}$. We use the known prior time fraction instead. 
- Real-life scenario: $\dot{\tilde{e}}(k) = \frac{e(k)-e(k-1) + n(k)}{T} = \frac{e(k)-e(k-1)}{T}+\frac{n(k)}{T}$, $n(k)$ is the amount of random noise in the time fraction.
  - **The smaller the time fraction $T$, the more amplified the noise $\frac{n(t)}{T}$ is.** 

- Best robust practice: adding built-in **low-pass filters** $\frac{a}{s+a}$. 

  - $a$ is the cutoff frequency, $s$ is the pure derivative. 
  - At low frequency (mostly the derivative we want): $s \approx 0$, so $ \frac{a}{0 + a} = \frac{a}{a} = 1 $. 
  - At high frequency (mostly the noise): $s \approx 1$, so $ \frac{a}{\infty + a} \approx \frac{a}{\infty} \approx 0 $. 

- $u_d(t) = K_d \cdot s \cdot e(t) \cdot \frac{a}{s+a} = K_d \cdot e(t) \cdot \frac{as}{s+a}$. 

  - $\frac{as}{s+a}$ is a **pseudo derivative**, turning the input signal $e(t)$ into an approximate derivative $\dot{\tilde{e}}$. 

    ![pseudo-vs-true-signal](assets/pseudo-vs-true-signal.jpg)

- Daisy chaining several low-pass filters: $u_d(t) = K_d \cdot e(t) \cdot \frac{as}{s+a} \cdot \frac{s}{s+a} \cdot \frac{s}{s+a}... = K_d \cdot e(t) \cdot \frac{as}{s+a} \cdot (\frac{s}{s+a})^n$ can further clean-up the noise. 

  - ⚠️ Adding low-pass filters introduce **phase lags**, especially when there’re multiple low-pass filters and the frequency is high (e.g. $\omega = 50$). 

    ![lagged-low-pass-filter](assets/lagged-low-pass-filter.jpg)

- Extra benefit of using pseudo derivative $\frac{as}{s+a}$: Avoids infinite $U_d(t)$ output when there’s a step change in the reference signal $r(t)$. 
  - Example: a user adjusts the oven temperature from 120ºC to 140ºC. ⚠️ Pure derivative $s = \infin$. 

### 2. Integrator Issues





## Reference

- [Introduction to PID Control by Christopher Lum - Youtube](https://www.youtube.com/watch?v=_VzHpLjKeZ8)

- [Practical Implementation Issues with a PID Controller by Christopher Lum - Youtube](https://youtu.be/yr6om0e0oAQ?si=rU-oW-7QXwFinE3-)

- [从本质上理解PID控制器，告别盲目调参 - Bilibili](https://www.bilibili.com/video/BV1NgnfzVERK/?share_source=copy_web&vd_source=70cc022a94cbbf6c817dca0ce94573e0)
- [Proportional–integral–derivative controller - Wikipedia](https://en.wikipedia.org/wiki/Proportional–integral–derivative_controller)
- 





