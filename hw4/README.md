# VP4: Collision and pendulum [function]

## [Video and HW PDF](http://tcjd71.wixsite.com/vpython/copy-of-5)  
+ [Video](https://goo.gl/y2mXQn)
+ [HW pdf](https://drive.google.com/file/d/1-E3WHWCGvNeV9AbpLoaaf4g7wfBUXuik/view)

## Contents

+ [Collision](#i-collision)
+ [Pendulum](#ii-pendulum)
+ [Homework](#iii-homework)

## I. Collision

```python
from vpython import *

size = [0.05, 0.04]     					   # ball radius
mass = [0.2, 0.4]       					   # ball mass
colors = [color.yellow, color.green]           # ball color
position = [vec(0, 0, 0), vec(0.2,-0.35, 0)]   # ball initial position
velocity = [vec(0, 0, 0), vec(-0.2, 0.30, 0)]  # ball initial velocity

scene = canvas(width=800, height=800, center=vec(0, -0.2, 0), background=vec(0.5, 0.5, 0)) # open window
ball_reference = sphere(pos=vec(0,0,0), radius=0.02, color=color.red)

def af_col_v(m1, m2, v1, v2, x1, x2):          # function after collision velocity
    v1_prime = v1 + 2*(m2/(m1+m2))*(x1-x2) * dot (v2-v1, x1-x2) / dot (x1-x2, x1-x2)
    v2_prime = v2 + 2*(m1/(m1+m2))*(x2-x1) * dot (v1-v2, x2-x1) / dot (x2-x1, x2-x1)
    return (v1_prime, v2_prime)

balls = []
for i in [0, 1]:
    balls.append(sphere(pos=position[i], radius=size[i], color=colors[i]))
    balls[i].v = velocity[i]
    balls[i].m = mass[i]

dt = 0.001
while True:
    rate(1000)

    for ball in balls:
        ball.pos += ball.v*dt
    if (mag(balls[0].pos - balls[1].pos) <= size[0]+size[1] and dot(balls[0].pos-balls[1].pos, balls[0].v-balls[1].v) <= 0) :
        balls[0].v, balls[1].v = af_col_v (balls[0].m, balls[1].m, balls[0].v, balls[1].v, balls[0].pos, balls[1].pos)
```

This program demonstrates an elastic collision event between two balls.

### Function

There are generally two cases in which we want to write a section of codes as a function. In one, we want to reuse many times the same section of codes. In the other, we want to make the entire codes more readable. Here is the example function that yields the velocities of two spherical objects after an elastic collision.

```python
def af_col_v(m1, m2, v1, v2, x1, x2):          # function after collision velocity
    v1_prime = v1 + 2*(m2/(m1+m2))*(x1-x2) * dot (v2-v1, x1-x2) / dot (x1-x2, x1-x2)
    v2_prime = v2 + 2*(m1/(m1+m2))*(x2-x1) * dot (v1-v2, x2-x1) / dot (x2-x1, x2-x1)
    return (v1_prime, v2_prime)
```

The first line `def af_col_v(m1, m2, v1, v2, x1, x2):` declares the function name `as af_col_v` by `def`. The function name is better to have meaning, here it means "after collision velocities". After the function name is the
parentheses and a colon. Inside the parentheses can be empty or any parameters which pass information from the main program to the function. In this case, they are `m1`, `m2`, `v1`, `v2`, `x1`, `x2` for masses, velocities, and positions of the two objects, respectively.

The subordinate codes below the colon are the major part of the function that handles the job. Here, they calculate the velocities after collision from the masses, velocities, and positions of the two spherical objects and put them into two variables, `v1_prime` and `v2_prime`. In the last line, it **return** the values of the two variables back to where the function is called.

Say if we have the following code

```python
V1, V2 = after_col_v(0.5, 0.6, vec(1, 2, 3), vec(4, 5, 6), vec(1, 0, 1), vec(0, 1, 1))
```

It will call the `after_col_v` function and let `m1` = 0.5, `m2`= 0.6, `v1` = vec(1, 2, 3)..., and so on. And after the function is executed, `V1` and `V2` will be the values of `v1_prime` and `v2_prime`, respectively. Note that this way of calling function is ‘call by position’, i.e. the parameters are matched by position order. We can also do this by ‘call by keywords’, such as

```python
V1, V2 = after_col_v(v1=vec(1, 2, 3), v2=vec(4, 5, 6), m2=0.6, x1=vec(1, 0, 1), x2=vec(0, 1, 1), m1=0.5)
```

Within the parentheses, the parameter order is not at all important, since the parameter is assigned explicitly.

## II. Pendulum

```python
from vpython import *

g = 9.8
size, m = 0.02, 0.5
L, k = 0.5, 15000

scene = canvas(width=500, height=500, center=vec(0, -0.2, 0), background=vec(0.5, 0.5, 0))
ceiling = box(length=0.8, height=0.005, width=0.8, color=color.blue)
ball = sphere(radius=size, color=color.red)
spring = cylinder(radius=0.005)   # default pos = vec(0, 0, 0)
ball.v = vec(0.6, 0, 0)
ball.pos = vec(0, -L - m*g/k, 0)

dt = 0.001
t = 0
while True:
    rate(1000)
    t += dt
    spring.axis = ball.pos - spring.pos                              # spring extended from endpoint to ball
    spring_force = - k * (mag(spring.axis) - L) * spring.axis.norm() # to get spring force vector
    ball.a = vector(0, - g, 0) + spring_force / m                    # ball acceleration = - g in y + spring force /m

    ball.v += ball.a*dt
    ball.pos += ball.v*dt
```

Does this program look familiar? Yes, it is only different from the ‘simple harmonic motion’ program in homework 3 by 4 lines: `ball.v = ...`, `ball.pos = ...`, `spring = ...`, and `L, k = 0.5, 15000`. But now, instead of an oscillating spring-ball system, we have a pendulum. Let’s see how
the difference of the program works.

Since we want to have a pendulum, i.e. a massive object attached to a rope, we need to have a rope. `spring` is therefore changed from a helix to a cylinder for the shape of a rope. More, when you try use a force to stretch a rope, there is a tension on the rope, and the tension is equal to the force. Also, the extension of the rope is very tiny, this means that the rope is just like a spring but with a very large spring constant. Thus, we set `k = 15000`, any large number will do, but do not let it be too large or too small, which will cause the simulation to be very unstable. Due to gravitation, the rope is stretched a bit from its original length, therefore `ball.pos = vec(0, -L - m*g/k, 0)`. The ball is set to have an initially horizontal velocity, `ball.v = vec(0.6, 0, 0)`. Then, we have a pendulum simulation.

This simulation is more “fundamental” than the pendulum analysis in the high school physics. The ball is exerted by two forces, one gravitational force and the other is the tension from the string. As the ball is swinging at the bottom of the string, it is doing a circular motion that requires a centripetal force. But how does the string “know” exactly how much force to exert on the string to make the ball to move in a circular motion as the ball velocity is changing all the time? A traditional way to analyze the pendulum never mentions this. By a careful examination, we will find actually the string must be stretched a little bit to provide the extra force as the centripetal force. This is what we do in this simulation.

## III. Homework

### MUST (5%)  

Combine I. and II., and the list in VP3 to write a program for Newton’s cradle with 5 balls. The program need to have a variable `N` that indicates how many balls (1 - 4) are lifted at the beginning.  
> You can use function `input()`, which allows taking in the variable `N` from `stdin`  
```python
N = int(input('how many balls to be lifted? [1-4]'))
```
> Will work like this:  

![hw4_input_tutorial](../graph/hw4_input_tutorial.png)
### BONUS (1%)  

We can add Coriolis force to the pendulum system. We assume the pendulum is set at latitude of φ, and the direction to the east is toward +x, the direction to the north is toward –z, and the direction to the sky is
toward +y (**Notice: it is different from the following figure**). For a quicker and better observation, we can let the simulation run at `rate(10000)`. If we project the swing onto the x-z plane, we will see the motion of the projection similar to a harmonic oscillation, originally oscillating in x-axis. Find the angle deviation of the swing projection from its original direction (+x axis) after 1000 periods of the pendulum if the pendulum is
in Taipei and of 2m long and with a still start angle at 30 degrees.

Notice: When we have a coordinate attached on a rotating sphere as in the figure, the Coriolis acceleration is shown as the equation below the figure, where <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dpi{150}&space;v_n" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\dpi{150}&space;v_e" title="v_e" /></a> , <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dpi{150}&space;v_n" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\dpi{150}&space;v_u" title="v_u" /></a>, and <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dpi{150}&space;v_n" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\dpi{150}&space;v_n" title="v_n" /></a> are the velocity to the east, to the sky, and to the north.

<img width="286.8" height="276" src="https://upload.wikimedia.org/wikipedia/commons/8/8a/Earth_coordinates.PNG"/>

(https://upload.wikimedia.org/wikipedia/commons/8/8a/Earth_coordinates.PNG)

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\vec{a_C}&space;=&space;-2\vec{\Omega}&space;\times&space;\vec{v}&space;=&space;2&space;\omega&space;\begin{pmatrix}&space;v_n&space;\sin&space;\phi&space;-&space;v_u&space;\cos&space;\phi\\&space;-v_e&space;\sin&space;\phi\\&space;v_e&space;\cos&space;\phi&space;\end{pmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{a_C}&space;=&space;-2\vec{\Omega}&space;\times&space;\vec{v}&space;=&space;2&space;\omega&space;\begin{pmatrix}&space;v_n&space;\sin&space;\phi&space;-&space;v_u&space;\cos&space;\phi\\&space;-v_e&space;\sin&space;\phi\\&space;v_e&space;\cos&space;\phi&space;\end{pmatrix}" title="\vec{a_C} = -2\vec{\Omega} \times \vec{v} = 2 \omega \begin{pmatrix} v_n \sin \phi - v_u \cos \phi\\ -v_e \sin \phi\\ v_e \cos \phi \end{pmatrix}" /></a>

### HW submission (PLZ be particularly aware!!)

+ Please upload a `zip` file (a compressed file) to CEIBA. Note that the filename extension should be `.zip`, and other format (e.g. `.rar` , `.tar` ...) is not allowed!!  
+ In the zip file, there will be **a directory whose name is your student ID.** The directory should contain 1 or 2 python scripts. Please name the script of must part: `must.py`, and bonus part: `optional.py`.  
+ Please also upload the `zip` file, even if you only plan to submit `must.py`. (This would help ease the burden for me to grade the hw. Thanks for the cooperation \~\~ :grin:)  

Example of submitted format: 
```
homework.zip
└── r07222016
    ├── must.py
    └── optional.py
```
### Deadline  
`11/3 SUN 22:00`  

### Grading Criteria (For Reference)  
Must  
    0: No Submission.  
    1: The program is totally not runnable.  
    2: The program is totally not physically correct.  
    3, 4: There are some subtle bugs so that the animation is not physically correct. Or you fail to fulfill some requirements (for ex: your code can only run with a specific number of balls...)  
    5: The animation is correct.  
