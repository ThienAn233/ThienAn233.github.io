---
layout: page
title: My sample project
description: with background image
img: assets/img/12.jpg
importance: 1
category: work
related_publications: true
---

# Simplify CART-POLE Simulation project.

The purpose of this project is to create an environment that simulates the cart-pole dynamic system. The original problem is related to the [inverted pendulum](https://en.wikipedia.org/wiki/Inverted_pendulum), which consists of a pendulum with its center of mass above its pivot point and a cart that moves the pendulum horizontally. This system is unstable and falls over without additional help. The inverted pendulum is a classic problem in **dynamics** and **control** theory and is used as a benchmark for testing control strategies. It is often implemented with the pivot point mounted on a cart that can move horizontally under control.

## Limitations and assumptions.

The assumptions are also the limitations of this project, aside from computational efficiency. This simulated environment assumes that:
* There is no interaction force from the pole back to the cart(no joint reaction force).
* The pole is cylindrical, and mass is evenly distributed in both cart and pole.
* The torque caused by friction of revolute joint: $T=\mu_{rev}\cdot \omega$ where $\omega$ is the angular velocity, $\mu_{rev}$ is the friction coefficient.

## Modelling the cart.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/simple_cart_pole/CART.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Cart model.
</div>

By Newton's second law:

$$\sum\vec{F}=m\vec{a}$$

$$\Leftrightarrow\sum\vec{F}=(m_{cart}+m_{pole})\cdot\vec{a}$$

$$\Leftrightarrow \vec{F_{in}}+\vec{F_{friction}}=(m_{cart}+m_{pole})\cdot\vec{a}$$

Projecting the above equation onto the x-axis, we get:

$$F_{in}-\mu_{lin}\cdot N=(m_{cart}+m_{pole})\cdot a$$

$$\Leftrightarrow F_{in}-\mu_{lin}\cdot (m_{cart}+m_{pole})=(m_{cart}+m_{pole})\cdot a$$

Isolating the acceleration of the cart:

$$a=\frac{F_{in}}{m_{cart}+m_{pole}}-\mu_{lin}$$

This resulting equation is used to simulate the motion of the cart given the input force $F_{in}$, the masses of both the cart and pole, and the friction coefficient $\mu$. Note that the force caused by friction is opposite to the direction of movement (velocity).

## Modelling the pole

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/simple_cart_pole/POLE.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pole model.
</div>

The case of the pole is a bit more complicated since it involves angular form of Newton's second law. $T$ is the torque applied to the object, $I$ is the moment of inertia, and $\alpha$ is the angular acceleration.

$$ \sum \vec{T}=I\cdot \vec{\alpha}$$

Considering the coordinate frame of the pole, the pole withstands a fictitious force caused by the cart and its own weight. The  left-hand side (LHS) can be written as:

$$\vec{F_{fictitious}}\times \vec{r}+\vec{P_{pole}}\times\vec{r}+\vec{T_{friction}}=I\cdot \vec{\alpha}$$

Choosing the anti-clockwise direction as positive:

$$F_{fictitious}\cdot r\cdot\sin{\theta}-P_{pole}\cdot r\cdot\sin{\theta'}-\mu_{rev}\cdot\omega=I\cdot \alpha$$

Recalling that we have the moment of inertia of a cylinder is $I=\frac{1}{3}mL^2$. Expanding the terms, we get:

$$m_{pole}\cdot a\cdot r\cdot\sin{\theta}-m_{pole}\cdot g\cdot r\cdot\cos{\theta}-\mu_{rev}\cdot\omega= \frac{4}{3}m_{pole}\cdot r^2\cdot \alpha$$

Simplifying the above equation give us:

$$\alpha = \frac{3}{2L}\left(a\cdot\sin{\theta}-g\cdot\cos{\theta}-2\frac{\mu_{rev}\cdot\omega}{m_{pole}\cdot L}\right)$$

## Discretizing for simulation
Based on the definition of acceleration and velocity in continious time:

$$x(t)=\int_0^tv(t)dt \Rightarrow x_t =\sum_{i=0}^tv_i\cdot dt$$

$$ x(t+1)=\int_0^{t+1}v(t)dt\Rightarrow x_{t+1} =\sum_{i=0}^{t+1}v_i\cdot dt$$

$$\Rightarrow x_{t+1}=x_t+v_{t+1}\cdot dt$$

Similarly, we also have:

$$v_{t+1}=v_t+a_{t+1}\cdot dt$$

$$\theta_{t+1}=\theta_t+\omega_{t+1}\cdot dt$$

$$\omega_{t+1}=\omega_t+\alpha_{t+1}\cdot dt$$

By choosing a small enough $dt$, we can achieve an accurate simulation of the system.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="https://github.com/ThienAn233/CART-POLE/assets/90390412/3f2c3d2d-3fa2-4edd-8b22-df1ccd2f9fcc" class="img-fluid rounded z-depth-1" %}
    </div>
</div>