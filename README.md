# Snake Game Project Using Python:-
A classic Snake Game implemented using Python and the turtle graphics library.

# Overview:
The Snake Game is a classic arcade game where the player controls a snake that moves around the screen, consuming food to grow longer. The game ends if the snake collides with itself or the screen boundaries. This explains the implementation of the Snake Game using Python and the turtle graphics library. The code consists of four main components: main.py, snake.py, food.py, and scoreboard.py.

# Project Structure:

The project is structured into four main Python scripts:

-**main.py** - The entry point of the game.

-**snake.py** - Defines the behavior and properties of the snake.

-**food.py** - Manages the creation and placement of food on the screen.

-**scoreboard.py** - Handles the game's score display and the game over message.

# 1. main.py:

This script sets up the game environment and contains the main game loop.

Code Breakdown:
from turtle import Screen
from snake import Snake
from food import Food
from scoreboard import Scoreboard
import time

   #Set up the screen
screen = Screen()
screen.setup(width=600, height=600)
screen.bgcolor("black")
screen.title("My Snake Game")
screen.tracer(0)

   #Initialize game objects
snake = Snake()
food = Food()
scoreboard = Scoreboard()

   #Control the snake
screen.listen()
screen.onkey(snake.up, "Up")
screen.onkey(snake.down, "Down")
screen.onkey(snake.left, "Left")
screen.onkey(snake.right, "Right")

   # Main game loop
game_is_on = True
while game_is_on:
    screen.update()
    time.sleep(0.1)
    snake.move()

    # Detect collision with food
    if snake.head.distance(food) < 15:
        food.refresh()
        snake.extend()
        scoreboard.increase_score()

    # Detect collision with wall
    if snake.head.xcor() > 280 or snake.head.xcor() < -280 or snake.head.ycor() > 280 or snake.head.ycor() < -280:
        game_is_on = False
        scoreboard.game_over()

    # Detect collision with tail
    for segment in snake.segments[1:]:
        if snake.head.distance(segment) < 10:
            game_is_on = False
            
            scoreboard.game_over()

screen.exitonclick()

# 2. snake.py:
This script defines the Snake class, responsible for the snake's creation, movement, and growth.

**Code Breakdown:**
from turtle import Turtle

   #Constants
STARTING_POSITIONS = [(0, 0), (-20, 0), (-40, 0)]
MOVE_DISTANCE = 20
UP = 90
DOWN = 270
LEFT = 180
RIGHT = 0

class Snake:

    def __init__(self):
        self.segments = []
        self.create_snake()
        self.head = self.segments[0]

    def create_snake(self):
        for position in STARTING_POSITIONS:
            self.add_segment(position)

    def add_segment(self, position):
        new_segment = Turtle("square")
        new_segment.color("white")
        new_segment.penup()
        new_segment.goto(position)
        self.segments.append(new_segment)

    def extend(self):
        self.add_segment(self.segments[-1].position())

    def move(self):
        for seg_num in range(len(self.segments) - 1, 0, -1):
            new_x = self.segments[seg_num - 1].xcor()
            new_y = self.segments[seg_num - 1].ycor()
            self.segments[seg_num].goto(new_x, new_y)
        self.head.forward(MOVE_DISTANCE)

    def up(self):
        if self.head.heading() != DOWN:
            self.head.setheading(UP)

    def down(self):
        if self.head.heading() != UP:
            self.head.setheading(DOWN)

    def left(self):
        if self.head.heading() != RIGHT:
            self.head.setheading(LEFT)

    def right(self):
        if self.head.heading() != LEFT:
            self.head.setheading(RIGHT)

# 3. food.py:
This script defines the Food class, responsible for creating and placing food items randomly on the screen.

- **Code Breakdown:**
from turtle import Turtle
import random

class Food(Turtle):

    def __init__(self):
        super().__init__()
        self.shape("circle")
        self.color("blue")
        self.penup()
        self.shapesize(stretch_len=0.5, stretch_wid=0.5)
        self.speed("fastest")
        self.refresh()

    def refresh(self):
        random_x = random.randint(-280, 280)
        random_y = random.randint(-280, 280)
        self.goto(random_x, random_y)
        
# 4. scoreboard.py:
This script defines the Scoreboard class, which manages the game's score display and the game over message.

- **Code Breakdown:**
from turtle import Turtle

   #Constants
ALIGNMENT = "center"
FONT = ("Courier", 24, "normal")

class Scoreboard(Turtle):

    def __init__(self):
        super().__init__()
        self.score = 0
        self.color("white")
        self.penup()
        self.hideturtle()
        self.goto(0, 260)
        self.update_scoreboard()

    def update_scoreboard(self):
        self.write(f"Score: {self.score}", align=ALIGNMENT, font=FONT)

    def increase_score(self):
        self.score += 1
        self.clear()
        self.update_scoreboard()

    def game_over(self):
        self.goto(0, 0)
        self.write("GAME OVER", align=ALIGNMENT, font=FONT)

# Game Execution:

1. **Screen Setup:**
The game begins by setting up the screen with a black background and a size of 600x600 pixels. The screen title is set to "My Snake Game" and the tracer(0) method is used to turn off automatic screen updates for smoother animation.

2. **Game Objects Initialization:**
Three main game objects are initialized:

~Snake - The snake that the player controls.

~Food - The food item that the snake consumes to grow.

~Scoreboard - The display for the current score and game over message.

3. **Snake Control:**

The screen.listen() method listens for key presses to control the snake's direction. The onkey method maps the arrow keys to the corresponding methods in the Snake class (up, down, left, right).

# Main Game Loop:
The main game loop is controlled by the game_is_on flag. Inside the loop:

1. **The screen is updated.**

2. **The snake moves forward.**

3. **Collision detection checks are performed:**
   - **~Food Collision:** If the snake's head is within 15 pixels of the food, the food is repositioned, the snake grows, and the score is increased.
    
    - **~Wall Collision:** If the snake's head moves beyond the screen boundaries, the game ends.

    - **~Tail Collision:** If the snake's head collides with any part of its tail, the game ends.

# Collision Detection:

- Collision detection is critical to the game's logic:
The distance between the snake's head and the food is calculated using the distance method.
Wall collisions are detected by comparing the snake's head position with the screen boundaries.
Tail collisions are detected by checking the distance between the snake's head and each segment of its body.

# Game Over:

When a collision is detected, the game_is_on flag is set to False, and the game_over method of the Scoreboard class displays the "GAME OVER" message.

# End of the Game:

The game loop ends when the game_is_on flag is set to False, and the game waits for a click on the screen to close.

# Conclusion: 
The Snake Game implemented using Python and the turtle graphics library provides a simple yet engaging game that demonstrates fundamental programming concepts such as object-oriented programming, collision detection, and event-driven programming. By breaking down the game into modular components (Snake, Food, Scoreboard), the code is organized, readable, and easy to maintain or extend.
# Output1:
![output1](https://github.com/user-attachments/assets/e3cf7f29-877b-490e-837d-6709a85920eb)

# Output2: 
