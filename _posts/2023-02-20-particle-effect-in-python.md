---
title: Flocking simulation in python
author: subodh 
date: 2023-02-20 09:38:31
categories: [Tutorial, Diy]
tags: [Diy,Tutorial]
math: true
mermaid: true

---
<p style="text-align: justify"> 
Flocking simulation is a type of computer simulation that models the behavior of a group of autonomous agents, such as birds, fish, or insects, as they move and interact with one another in a flock or swarm. The simulation is based on the principles of emergent behavior, where the overall behavior of the group emerges from the individual interactions between each agent.
</p>

<p style="text-align: justify">
Flocking simulations can be used in a variety of applications, including robotics, computer graphics, and game development, to simulate the behavior of crowds, traffic, and other complex systems.
</p>

![Boid flocking particle effect](https://thumbs.gfycat.com/EcstaticFatArctichare-size_restricted.gif)


```python
import tkinter
import random
import math
import Boid
```
<p style="text-align: justify">
import required libraries to our python program.
in this example we have used tkinter,which is a standard Python library used for creating graphical user interfaces (GUIs).

math is another standard Python library that provides various mathematical functions and constants.

Boid is costom module which is called from a different python file and it consists all the functions required to control the individual boid.
</p>

```python
def initialise_canvas(window, screen_size):
    canvas = tkinter.Canvas(window, width=screen_size,height=screen_size)
    canvas.pack()
    window.resizable(True, True)
    return canvas
```

The first code blocks defines a function `initialise_canvas` that takes two parameter

1. `window`, which is a tkinter window object.
2. `screen_size`, an integer representing the desired size of the canvas in pixels.

function creates a new canvas object using the `tkinter.Canvas()` constructor, with the specified width and height, and adds it to the given window using the `pack()` method.

It also specifies that the window is `resizable` in both the horizontal and vertical directions.

Finally, the function returns the created canvas object.


```python
def create_boids(canvas, no_of_boids):
    list_of_boids = []
    for n in range(no_of_boids):
        boid = Boid.Boid("boid" + str(n))
        list_of_boids.append(boid)
        boid.draw_boid(canvas)
    return list_of_boids
```

Here we create another function `create_boids` which takes two parameters.

1. `canvas`, a canvas object which will be used to draw the boids.

2. `no_of_boids`,an integer value representing the number of boid value to create.

The function first creates an empty list called `list_of_boids`.

It then loops `no_of_boids` times using a for loop. In each iteration, the loop creates a new `Boid` object with a unique name using the string concatenation "boid" + n.

Then newly created boid object is appended to the `list_of_boids` using the append method.

Then function calls the `draw_boid` method of the boid object, which draws the boid on the specified canvas and the function returns `list_of_boids`.

```python
def separation(nearest_neighbour, boid):
    if nearest_neighbour is not None and boid.euclidean_distance(nearest_neighbour) < 35:
        if nearest_neighbour.x - boid.x == 0.0:
            angle = math.atan((nearest_neighbour.y - boid.y) / 0.0001)
        else:
            angle = math.atan((nearest_neighbour.y - boid.y) / (nearest_neighbour.x - boid.x))
        boid.angle -= angle
```

This code block defines a function called `separation` that takes in two arguments `nearest_neighbour` and `boid`. This function calculates the *angle* between the boid and its *nearest* neighbour and then moves the boid in the opposite direction.

The first condition checks if there is a nearest_neighbour and if the distance between the boid and the nearest_neighbour is less than 35. If this condition is met, the function calculates the *angle* between the boid and the `nearest_neighbour`.

> Euclidean distance between two points in Euclidean space is the length of a line segment between the two points. It can be calculated from the Cartesian coordinates of the points using the Pythagorean theorem, therefore occasionally being called the Pythagorean distance.
{: .prompt-tip }

If the `nearest_neighbour` is directly above or below the `boid`, the function sets the angle to `math.atan((nearest_neighbour.y - boid.y) / 0.0001)` to avoid _division by zero_. Otherwise, the function sets the angle to `math.atan((nearest_neighbour.y - boid.y) / (nearest_neighbour.x - boid.x))`.

then angle is subtracted from the boid's current angle. This means that the boid will move away from its nearest neighbour in the opposite direction of the calculated angle.

> when a number is divided by a zero, the result is an infinite number. It is impossible to write an Infinite number physically. Python interpreter throws “ZeroDivisionError: division by zero” error if the result is infinite number.
{: .prompt-info }

```python
def alignment(neighbours, boid):
    average_neighbours_angle = 0.0
    if neighbours:
        for neighbour_boid in neighbours:
            average_neighbours_angle += neighbour_boid.angle
        average_neighbours_angle /= len(neighbours)
        boid.angle -= (average_neighbours_angle-boid.angle)/100.0
        boid.angle = average_neighbours_angle
```

The `alignment` function aligns the boid with its `neighbours` by adjusting its *angle* towards the average angle of the `neighbours`.
The function calculates the average angle of the neighbours and adjusts the boid's angle towards that average.

The average angle of the `neighbours` is calculated by iterating over each `neighbour_boid` in `neighbours`, adding its angle to a running total, and then dividing the total by the number of neighbours.

The boid's angle is then adjusted towards the `average_neighbours_angle` by subtracting `(average_neighbours_angle-boid.angle)/100.0` from its current angle. Finally, the boid's angle is set to the `average_neighbours_angle`.

```python
def cohesion(neighbours, boid):
    if neighbours:
        avg_x = 0.0
        avg_y = 0.0
        for neighbour_boid in neighbours:
            avg_x += neighbour_boid.x
            avg_y += neighbour_boid.y
        avg_x /= len(neighbours)
        avg_y /= len(neighbours)
        if avg_x - boid.x == 0.0:
            angle = math.atan((avg_y - boid.y) / 0.00001)
        else:
            angle = math.atan((avg_y - boid.y) / (avg_x - boid.x))
        boid.angle -= angle / 20.0
```

The `cohesion` function moves the boid towards the average position of its `neighbours` by adjusting its `angle` towards that position.

code block defines a function called `cohesion` that takes in two arguments `neighbours` and `boid`. The function calculates the average position of the `neighbours` and adjusts the boid's angle towards that position.

If `neighbours` is not empty, the function calculates the average x and y position of the neighbours by iterating over each `neighbour_boid` in `neighbours`, adding its x and y position to running totals, and then dividing the totals by the number of `neighbours`.

If the `boid` is directly above or below the average position, the function sets the angle to `math.atan((avg_y - boid.y) / 0.00001)` to avoid division by zero. Otherwise, the function sets the angle to `math.atan((avg_y - boid.y) / (avg_x - boid.x))`.

> when a number is divided by a zero, the result is an infinite number. It is impossible to write an Infinite number physically. Python interpreter throws “ZeroDivisionError: division by zero” error if the result is infinite number.
{: .prompt-info }

Then angle is divided by 20.0 and subtracted from the boid's current angle. This means that the boid will move towards the average position of its `neighbours`, but not too quickly.

```python
def boid_behaviours(canvas, list_of_boids, screen_size):
    for boid in list_of_boids:
        neighbours = []
        for b in list_of_boids:
            if boid.euclidean_distance(b) < 75 and (not boid.euclidean_distance(b) == 0):
                neighbours.append(b)
        nearest_neighbour = None
        if neighbours:
            shortest_distance = 999999999
            for neighbour_boid in neighbours:
                d = boid.euclidean_distance(neighbour_boid)
                if d < shortest_distance:
                    shortest_distance = d
                    nearest_neighbour = neighbour_boid

        separation(nearest_neighbour, boid)
        alignment(neighbours, boid)
        cohesion(neighbours, boid)

    for boid in list_of_boids:
        boid.flock(canvas, screen_size)
    canvas.after(100, boid_behaviours, canvas, list_of_boids, screen_size)
```
the `boid_behaviours` function applies **separation**, **alignment**, and **cohesion** behaviors to each `boid` in `list_of_boids`, and updates their position and angle. The function is then scheduled to run again after 100 milliseconds.

A function called `boid_behaviours` that takes in three arguments `canvas`, `list_of_boids`, and `screen_size`. The function applies three behaviors to each `boid` in `list_of_boids`: *separation*, *alignment*, and *cohesion*.

To apply these behaviors, the function first finds the neighbours of each `boid`. A `neighbour` is defined as another boid that is within **75** units of distance, and not the current `boid`. The function iterates over all boids in `list_of_boids`, adds the `boids` that are `neighbours` to the `neighbours` list, and finds the nearest `neighbour` for each `boid`.

Then function calls the `flock` method for each `boid`, which updates the position and angle of the boid based on its current velocity, and schedules the `boid_behaviours` function to be called again after 100 milliseconds using the `canvas.after` method.


```python
def main():
    screen_size = 800
    no_of_boids = 100
    window = tkinter.Tk()
    canvas = initialise_canvas(window, screen_size)
    list_of_boids = create_boids(canvas, no_of_boids)
    boid_behaviours(canvas, list_of_boids, screen_size)
    window.mainloop()

main()
```

This code block sets up the screen size of GUI window, creates and initializes a set/number of `boids`, and applies **flocking behaviors** to them in an infinite loop until the user exits the program.

The `main` function is then called at the bottom of the script to execute the program.



```python
class Boid:
    def __init__(self, label):
        self.x = random.randrange(100, 900)
        self.y = random.randrange(100, 900)
        self.angle = random.uniform(0.0, 2.0 * math.pi)
        self.label = label
        self.color = ["blue","red","green"]

    def draw_boid(self, canvas):

        size = 10
        x1 = self.x + size * math.cos(self.angle)
        x2 = self.y + size * math.sin(self.angle)
        canvas.create_line(self.x, self.y, x1, x2, fill='black', arrow='last', arrowshape=(12.8,16,4.8), width=2, tags=self.label)

    def flock(self, canvas, screen_size):
        distance = 3
        self.x += distance * math.cos(self.angle)
        self.y += distance * math.sin(self.angle)

        self.x = self.x % screen_size
        self.y = self.y % screen_size
        canvas.delete(self.label)
        self.draw_boid(canvas)

    def euclidean_distance(self, neighbour_boid):
        return math.sqrt((self.x - neighbour_boid.x) * (self.x - neighbour_boid.x) + \
                         (self.y - neighbour_boid.y) * (self.y - neighbour_boid.y))
```

This is Boid class and python custom module which is imported to main program file.

This code snippet defines a `Boid` class with the following attributes and methods:

**Attributes**:

 1. `x`
 2. `y`
 3. `angle`
 4. `label`
 5. `color`

**Methods**:

1. `__init__`: initializes the Boid object with random values for x, y, and angle, and assigns a label and color to the Boid.

2. `draw_boid`: draws the Boid on the canvas.

3. `flock`: calculates the next position of the Boid and ensures that it stays within the screen, then redraws the Boid on the canvas.

4. `euclidean_distance`: calculates the Euclidean distance between the Boid and a neighboring Boid.

The code initializes the `Boid` object with random values for x, y, and angle and assigns a label to it. It then provides methods to draw the Boid on a canvas, move it within the screen, and calculate the Euclidean distance between it and neighboring Boids.

<br>

---
In This Example our boid has Three possible movements.
* move 1: move away from nearest (`separation`).

* move 2: Orient Towards the neighbours (`alignment`).

* move 3: Move Together (`cohesion`).


---
if any queries or suggestion feel free to write me a mail on subkamble@gmail.com