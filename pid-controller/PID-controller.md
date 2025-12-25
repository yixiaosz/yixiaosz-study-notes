# PID Controller

Notes created on 12/25/2025

![PID-controller-diagram](assets/960px-PID_en.jpg)

- Input reference signal = $r(t)$ 
- Error signal = $e(t)$ 
- Control signal = $u(t)$ 
- Output signal = $y(t)$ 
- K = gain

- Goal: ==$y(t)$ matches $r(t)$== 



Control Law: 

$u(t) = K_p \cdot e(t) + K_i \cdot \frac{1}{s} \text{(Integrator)} \cdot e(t) + K_d \cdot s \text{(differetiator)} \cdot e(t)$ 

$u(t) = \underbrace{K_p \cdot e(t)}_{u_p(t)} + \underbrace{K_i \cdot \int_{0}^{t} e(\tau)d\tau}_{u_i(t)} + \underbrace{K_d \cdot \frac{de(t)}{dt}}_{u_d(t)}$ 

 

## Proportional Control





## Reference

- [Introduction to PID Control by Christopher Lum - Youtube](https://www.youtube.com/watch?v=_VzHpLjKeZ8)

- [从本质上理解PID控制器，告别盲目调参 - Bilibili](https://www.bilibili.com/video/BV1NgnfzVERK/?share_source=copy_web&vd_source=70cc022a94cbbf6c817dca0ce94573e0)
- [Proportional–integral–derivative controller - Wikipedia](https://en.wikipedia.org/wiki/Proportional–integral–derivative_controller)
- 





