# Snake Game
A classic Snake game empowered by Python.

---

This code is a Python script that runs a simple Snake game using the Turtle graphics library. The game allows the player to control a snake on the screen, collecting food to increase its length while avoiding collisions with walls and itself. 

Here's a breakdown of the code:
- main.py:
  - Initializes the game's main components such as the screen, snake, food, and scoreboard.
  - Contains a start_game() function that controls the game loop and the logic for updating the snake's movement, detecting collisions, and managing user input.
  - Sets up the screen and its properties, then listens for the "s_" key to start the game loop.
  - Exits the game when the screen is clicked.
- food.py:
  - Defines a Food class that represents the food the snake collects. 
  - Contains a refresh() method that randomly positions the food on the screen.
- snake.py:
  - Defines the Snake class that represents the snake the player controls.
  - Contains methods for moving the snake, controlling its direction, detecting collisions with itself and walls, and adding segments.
- scoreboard.py:
  - Defines the Scoreboard class that manages and displays the game's score.
  - Contains methods for updating the score, increasing the score, retrieving the highest score from a file, resetting the score and etc.

Each module (main.py, food.py, snake.py, scoreboard.py) contributes to a specific aspect of the game's functionality, resulting in a classic Snake game.

## Final Outcome
<img width="555" alt="snake_game" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/ce3c3814-fd3f-4750-aa98-64d5b0d621b9">

## Project Code
main.py

```PYTHON
from turtle import Screen
from snake import Snake
from food import Food
from scoreboard import Scoreboard
import time

def start_game():
    is_game_on = True
    while is_game_on:
        screen.update()
        time.sleep(0.1)  # to control the speed of animation

        screen.onkey(fun=snake.left, key="Left")
        screen.onkey(fun=snake.right, key="Right")
        screen.onkey(fun=snake.up, key="Up")
        screen.onkey(fun=snake.down, key="Down")

        snake.move()

        # detect collisions with the food
        if snake.head.distance(food) < 15:
            scoreboard.increase_score()
            food.refresh()
            # append a segment to the snake
            snake.append_segment()

        # detect collisions with the wall
        if (
            snake.head.xcor() >= 300
            or snake.head.xcor() <= -300
            or snake.head.ycor() >= 300
            or snake.head.ycor() <= -300
        ):
            is_game_on = False
            scoreboard.reset()
            snake.reset()

        # detect collisions with tail
        if snake.is_collision_tail():
            is_game_on = False
            scoreboard.reset()
            snake.reset()


screen = Screen()
screen.setup(width=600, height=600)
screen.bgcolor("black")
screen.title("Greedy snake")
screen.tracer(0)

snake = Snake()
food = Food()
scoreboard = Scoreboard()

screen.listen()
screen.onkeypress(fun=start_game, key="s_")

screen.exitonclick()
```
food.py

```PYTHON
from turtle import Turtle
import random

FOOD_COLOR = "red"


class Food(Turtle):
    def __init__(self):
        super().__init__()
        self.shape("circle")
        self.shapesize(stretch_len=0.8, stretch_wid=0.8)
        self.penup()
        self.color(FOOD_COLOR)
        self.speed(0)
        self.refresh()

    def refresh(self):
        new_x = random.randint(-280, 280)
        new_y = random.randint(-280, 280)
        self.setposition(x=new_x, y=new_y)

```
snake.py

```PYTHON
from turtle import Turtle

SNAKE_COLOR = "green"
DISTANCE = 20
RIGHT = 0
UP = 90
LEFT = 180
DOWN = 270


class Snake:
    def __init__(self) -> None:
        self.snake = []
        self.create_snake()
        self.head = self.snake[0]
        self.last_position = None

    def create_snake(self):
        start_x = 0
        for i in range(3):
            segment = Turtle(shape="square")
            segment.penup()
            segment.color(SNAKE_COLOR)
            segment.setposition(x=start_x, y=0)
            start_x -= 20
            self.snake.append(segment)

    def append_segment(self):
        segment = Turtle(shape="square")
        segment.penup()
        segment.color(SNAKE_COLOR)
        segment.setposition(x=self.last_position[0], y=self.last_position[1])

        self.snake.append(segment)

    def reset(self):
        for segment in self.snake:
            segment.goto(1000, 1000)
        self.snake.clear()
        self.create_snake()
        self.head = self.snake[0]
        self.last_position = None

    def is_collision_tail(self):
        for segment in self.snake[1:]:
            if self.head.distance(segment) <= 10:
                return True
        return False

    def move(self):
        self.last_position = self.snake[-1].pos()
        for i in range(len(self.snake) - 1, 0, -1):
            # set the position of i-th segment to the position of its previous segment
            new_pos = self.snake[i - 1].position()
            self.snake[i].setposition(new_pos)

        self.head.forward(DISTANCE)

    def left(self):
        if self.head.heading() != RIGHT:
            # snake is not allowed to move in opposite direction
            self.head.setheading(LEFT)

    def right(self):
        if self.head.heading() != LEFT:
            # snake is not allowed to move in opposite direction
            self.head.setheading(RIGHT)

    def up(self):
        if self.head.heading() != DOWN:
            # snake is not allowed to move in opposite direction
            self.head.setheading(UP)

    def down(self):
        if self.head.heading() != UP:
            # snake is not allowed to move in opposite direction
            self.head.setheading(DOWN)

```
scoreboard.py

```PYTHON
from turtle import Turtle

ALIGNMENT = "center"
FONT = ("candara", 24, "bold")
SCORE_COLOR = "royal blue"


class Scoreboard(Turtle):
    def __init__(self):
        super().__init__()
        self.color(SCORE_COLOR)
        self.penup()
        self.hideturtle()
        self.setposition(0, 270)
        self.score = 0
        self.highest_score = self.get_highest_score()
        self.update_score()

    def update_score(self):
        self.clear()
        self.write(
            f"Score: {self.score}   Highest score: {self.highest_score}",
            align=ALIGNMENT,
            font=FONT,
        )

    def increase_score(self):
        self.score += 1
        self.update_score()

    def get_highest_score(self):
        with open("data.txt") as file:
            highest_score = int(file.read())

        return highest_score

    def reset(self):
        if self.score > self.highest_score:
            self.highest_score = self.score
            with open("data.txt", mode="w") as file:
                file.write(f"{self.score}")

        self.score = 0
        self.update_score()

    def game_over(self):
        self.color('red')
        self.setposition(x = 0, y = 0)
        self.write(f"Game over!", align= ALIGNMENT, font= FONT)
```
