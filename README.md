# Custom Python Platformer Game Project

## Project Goal

You have already built a simple platformer walkthrough. Now it is your turn to design and build your own unique platformer game.

Your game should not be an exact copy of the walkthrough. You can use the same coding ideas, but your game should have its own theme, layout, characters, obstacles, and goal.

By the end of this project, your game should feel like something you designed.

---

## What You Are Building

You are creating a 2D platformer game where the player can:

- move left and right
- jump
- land on platforms
- collect items
- avoid hazards or enemies
- reach a goal
- win or lose the game

You may use rectangles and colors, or you may add images if you know how.

---

## Project Requirements

Your finished game must include:

- A custom game title
- A custom theme
- A player character
- At least 5 platforms
- Gravity and jumping
- At least 3 collectibles
- At least 1 hazard or enemy
- A score system
- A goal or finish area
- A win condition
- A lose condition or reset condition
- Comments explaining important parts of your code

---

## Bonus Features

You can earn extra credit or challenge yourself by adding:

- Multiple levels
- Lives
- Timer
- Moving enemy
- Moving platform
- Power-up
- Sound effects
- Custom background
- Start screen
- Game over screen
- Secret area
- Double jump
- Checkpoints

---

# Step 1: Plan Your Game

Before coding, answer these questions.

## Game Idea

**Game Title:**  
Write your title here.

**Theme:**  
Examples: jungle, space, football, castle, underwater, school escape, robot factory, candy world.

**Player:**  
Who or what does the player control?

**Collectibles:**  
What does the player collect?

**Hazards:**  
What does the player avoid?

**Goal:**  
How does the player win?

---

## Level Sketch

Draw your level on paper before coding.

Your sketch should include:

- starting location
- platforms
- collectibles
- hazards
- goal
- any enemies or special areas

Think of your game screen as a coordinate grid.

- `x` controls left and right
- `y` controls up and down
- `(0, 0)` is the top-left corner
- bigger `x` moves right
- bigger `y` moves down

---

# Step 2: Basic Game Setup

Start with your game window and basic variables.

```python
WIDTH = 800
HEIGHT = 500

TITLE = "My Platformer Game"

player = Rect((100, 400), (40, 40))

velocity_y = 0
gravity = 1
jump_strength = -15
speed = 5

on_ground = False
score = 0
game_won = False
game_over = False
```

## What This Code Does

- `WIDTH` and `HEIGHT` set the size of the game window.
- `player` creates the character as a rectangle.
- `velocity_y` controls vertical movement.
- `gravity` pulls the player down.
- `jump_strength` controls how high the player jumps.
- `speed` controls left and right movement.
- `score` tracks collectibles.
- `game_won` and `game_over` help control the ending.

---

# Step 3: Draw the Player

```python
def draw():
    screen.clear()
    screen.fill("skyblue")
    screen.draw.filled_rect(player, "blue")
```

## Customize It

Change the color to match your theme.

Examples:

```python
screen.draw.filled_rect(player, "red")
screen.draw.filled_rect(player, "purple")
screen.draw.filled_rect(player, "orange")
```

You can pretend your rectangle is a character.

Examples:

- blue rectangle = robot
- red rectangle = lava runner
- green rectangle = alien
- orange rectangle = football player

---

# Step 4: Add Player Movement

```python
def update():
    global velocity_y

    if keyboard.left:
        player.x -= speed

    if keyboard.right:
        player.x += speed
```

## Keep the Player on the Screen

Add this inside `update()`:

```python
if player.left < 0:
    player.left = 0

if player.right > WIDTH:
    player.right = WIDTH
```

## What This Code Does

- Pressing the left arrow moves the player left.
- Pressing the right arrow moves the player right.
- The boundary checks stop the player from leaving the screen.

---

# Step 5: Add Gravity

Gravity makes the player fall down.

```python
velocity_y += gravity
player.y += velocity_y
```

This should go inside your `update()` function.

## What This Code Does

Each time the game updates:

1. Gravity increases the downward speed.
2. The player moves down by that speed.

Without platforms or a floor, the player would fall forever.

---

# Step 6: Add Platforms

Platforms are rectangles the player can stand on.

```python
platforms = [
    Rect((0, 470), (800, 30)),
    Rect((150, 390), (120, 20)),
    Rect((350, 320), (120, 20)),
    Rect((550, 250), (120, 20)),
    Rect((250, 180), (120, 20))
]
```

## Draw the Platforms

Add this inside your `draw()` function:

```python
for platform in platforms:
    screen.draw.filled_rect(platform, "green")
```

## Customize It

Change the platform colors based on your theme.

Examples:

```python
screen.draw.filled_rect(platform, "brown")   # wood
screen.draw.filled_rect(platform, "gray")    # stone
screen.draw.filled_rect(platform, "white")   # clouds
screen.draw.filled_rect(platform, "darkgreen") # jungle
```

---

# Step 7: Platform Collision

Collision means two objects touch.

This code lets the player land on platforms.

```python
def check_platform_collision():
    global velocity_y, on_ground

    on_ground = False

    for platform in platforms:
        if player.colliderect(platform) and velocity_y >= 0:
            player.bottom = platform.top
            velocity_y = 0
            on_ground = True
```

Call this inside `update()` after gravity:

```python
check_platform_collision()
```

## What This Code Does

- It checks each platform.
- If the player touches a platform while falling, the player lands on it.
- `velocity_y = 0` stops the player from falling through.
- `on_ground = True` means the player is allowed to jump again.

---

# Step 8: Add Jumping

```python
if keyboard.space and on_ground:
    velocity_y = jump_strength
```

## What This Code Does

- The player can jump when the spacebar is pressed.
- The player can only jump if `on_ground` is `True`.
- This prevents unlimited jumping in the air.

---

# Step 9: Add Collectibles

Collectibles can be coins, stars, gems, footballs, keys, or anything that fits your theme.

```python
collectibles = [
    Rect((180, 350), (20, 20)),
    Rect((390, 280), (20, 20)),
    Rect((590, 210), (20, 20))
]
```

## Draw Collectibles

Add this inside `draw()`:

```python
for item in collectibles:
    screen.draw.filled_rect(item, "yellow")
```

## Collect the Items

```python
def check_collectibles():
    global score

    for item in collectibles[:]:
        if player.colliderect(item):
            collectibles.remove(item)
            score += 1
```

Call it inside `update()`:

```python
check_collectibles()
```

## Draw the Score

Add this inside `draw()`:

```python
screen.draw.text("Score: " + str(score), (10, 10), fontsize=30, color="black")
```

---

# Step 10: Add Hazards

Hazards are dangerous objects. They might be lava, spikes, enemies, water, lasers, or defenders.

```python
hazards = [
    Rect((300, 450), (80, 20)),
    Rect((500, 450), (80, 20))
]
```

## Draw Hazards

Add this inside `draw()`:

```python
for hazard in hazards:
    screen.draw.filled_rect(hazard, "red")
```

## Check Hazard Collision

```python
def check_hazards():
    global game_over

    for hazard in hazards:
        if player.colliderect(hazard):
            game_over = True
```

Call it inside `update()`:

```python
check_hazards()
```

## Easier Version: Reset the Player Instead

Instead of ending the game, you can reset the player.

```python
def check_hazards():
    for hazard in hazards:
        if player.colliderect(hazard):
            player.x = 100
            player.y = 400
```

---

# Step 11: Add a Goal

The goal is how the player wins.

```python
goal = Rect((720, 420), (50, 50))
```

## Draw the Goal

Add this inside `draw()`:

```python
screen.draw.filled_rect(goal, "gold")
```

## Check for Winning

```python
def check_goal():
    global game_won

    if player.colliderect(goal):
        game_won = True
```

Call it inside `update()`:

```python
check_goal()
```

---

# Step 12: Add Win and Game Over Screens

Inside `draw()`, add messages for winning or losing.

```python
if game_won:
    screen.draw.text("YOU WIN!", center=(WIDTH / 2, HEIGHT / 2), fontsize=60, color="yellow")

if game_over:
    screen.draw.text("GAME OVER", center=(WIDTH / 2, HEIGHT / 2), fontsize=60, color="red")
```

## Stop the Game After Winning or Losing

At the top of your `update()` function, add:

```python
if game_won or game_over:
    return
```

This stops the game from continuing after the player wins or loses.

---

# Step 13: Add Lives

Lives give the player more than one chance.

```python
lives = 3
```

Change your hazard code:

```python
def check_hazards():
    global lives, game_over

    for hazard in hazards:
        if player.colliderect(hazard):
            lives -= 1
            player.x = 100
            player.y = 400

            if lives <= 0:
                game_over = True
```

Draw lives on the screen:

```python
screen.draw.text("Lives: " + str(lives), (10, 40), fontsize=30, color="black")
```

---

# Step 14: Add a Moving Enemy

A moving enemy makes your game more interesting.

```python
enemy = Rect((400, 440), (40, 30))
enemy_speed = 3
```

Move the enemy inside `update()`:

```python
global enemy_speed

enemy.x += enemy_speed

if enemy.left < 300 or enemy.right > 600:
    enemy_speed *= -1
```

Draw the enemy inside `draw()`:

```python
screen.draw.filled_rect(enemy, "purple")
```

Check collision:

```python
if player.colliderect(enemy):
    game_over = True
```

## What This Code Does

- The enemy moves left and right.
- When it reaches the edge of its patrol area, it turns around.
- If the player touches the enemy, the game ends.

---

# Step 15: Add a Timer

A timer can make the game more challenging.

```python
time_left = 60
```

In Pygame Zero, you can use the clock to count down once per second.

```python
def countdown():
    global time_left, game_over

    if time_left > 0:
        time_left -= 1
    else:
        game_over = True

clock.schedule_interval(countdown, 1.0)
```

Draw the timer:

```python
screen.draw.text("Time: " + str(time_left), (10, 70), fontsize=30, color="black")
```

---

# Step 16: Add a Start Screen

A start screen makes your game feel more complete.

```python
game_started = False
```

At the top of `update()`:

```python
global game_started

if not game_started:
    if keyboard.RETURN:
        game_started = True
    return
```

Inside `draw()`:

```python
if not game_started:
    screen.clear()
    screen.fill("black")
    screen.draw.text("MY PLATFORMER", center=(WIDTH / 2, 180), fontsize=60, color="white")
    screen.draw.text("Press ENTER to Start", center=(WIDTH / 2, 260), fontsize=35, color="yellow")
    return
```

---

# Step 17: Add a Power-Up

A power-up can temporarily help the player.

Example: a jump boost.

```python
powerup = Rect((650, 210), (25, 25))
has_jump_boost = False
```

Draw it:

```python
if powerup:
    screen.draw.filled_rect(powerup, "cyan")
```

Check collision:

```python
def check_powerup():
    global has_jump_boost, jump_strength, powerup

    if powerup and player.colliderect(powerup):
        has_jump_boost = True
        jump_strength = -20
        powerup = None
```

Call inside `update()`:

```python
check_powerup()
```

---

# Step 18: Add Multiple Levels

A simple way to make a second level is to change the platforms and reset the player when the player reaches the goal.

```python
level = 1
```

```python
def load_level_2:
    global platforms, collectibles, hazards, goal

    platforms = [
        Rect((0, 470), (800, 30)),
        Rect((100, 360), (120, 20)),
        Rect((300, 280), (120, 20)),
        Rect((500, 200), (120, 20))
    ]

    collectibles = [
        Rect((130, 320), (20, 20)),
        Rect((330, 240), (20, 20)),
        Rect((530, 160), (20, 20))
    ]

    hazards = [
        Rect((400, 450), (100, 20))
    ]

    goal = Rect((700, 150), (50, 50))
```

Important: the function above is missing something. Look carefully at the first line. What punctuation does a Python function need?

When the player reaches the goal on level 1, you could load level 2.

```python
if player.colliderect(goal):
    if level == 1:
        level = 2
        player.x = 100
        player.y = 400
        load_level_2()
    else:
        game_won = True
```

---

# Debugging Checklist

If your game is not working, check these common problems.

## My player falls through the platforms.

Check:

- Did you call `check_platform_collision()` inside `update()`?
- Is your collision code after gravity?
- Did you set `player.bottom = platform.top`?
- Did you set `velocity_y = 0`?

## My player cannot jump.

Check:

- Is `on_ground` spelled the same everywhere?
- Did you use `global velocity_y, on_ground`?
- Is the player actually touching a platform?

## My score does not change.

Check:

- Did you use `global score` in the function?
- Did you call `check_collectibles()` inside `update()`?
- Are the collectibles close enough for the player to touch?

## My game crashes.

Check:

- Did you forget a colon `:`?
- Did you indent your code correctly?
- Did you spell variable names the same way every time?
- Did you use parentheses correctly?

## My game says a variable does not exist.

Check:

- Did you create the variable before using it?
- Did you spell it the same way?
- Is it inside a function when it should be outside?

---

# Project Submission Requirements

Before turning in your game, make sure you have:

- A working game file
- A custom title
- A custom theme
- At least 5 platforms
- At least 3 collectibles
- At least 1 hazard or enemy
- A goal
- A score display
- A win or game over screen
- Comments in your code

---

# Code Comment Examples

Good comments explain why the code matters.

```python
# Gravity makes the player fall back down after jumping
velocity_y += gravity
```

```python
# If the player touches a coin, remove it and add to the score
if player.colliderect(item):
    collectibles.remove(item)
    score += 1
```

Avoid comments that only repeat the code.

Not helpful:

```python
# Add 1 to score
score += 1
```

Better:

```python
# Give the player a point after collecting an item
score += 1
```

---

# Final Reflection Questions

Answer these after finishing your game.

1. What is the title of your game?
2. What makes your game unique?
3. What was the hardest part to code?
4. What bug did you fix?
5. What feature are you most proud of?
6. What would you add if you had more time?

---

# Helpful Reminder

Your game does not have to be perfect.

A good game project should show that you understand:

- movement
- gravity
- jumping
- collision
- score
- hazards
- win or lose conditions

Focus on making a small game that works first. Then add creative features after the basics are working.