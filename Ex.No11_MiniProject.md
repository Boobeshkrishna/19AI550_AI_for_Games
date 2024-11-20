# Ex.No: 11  Mini Project 
### DATE:                                                                            
### REGISTER NUMBER : 212222240043
### TITLE: Pathfinding and Game Mechanics Using A Algorithm*

### AIM:
To implement a grid-based game using the A* algorithm for pathfinding, combined with interactive elements like collectibles and enemies, while exploring AI techniques in games.

#### PREREQUISITES:
Familiarity with:
Python programming.
Game development using the Pygame library.
Pathfinding algorithms, particularly A*.
Installed packages:
pygame (for rendering and game mechanics).
pathfinding (for A* pathfinding).

#### HARDWARE/SOFTWARE REQUIREMENTS:
Hardware: PC with minimum 4GB RAM, GPU recommended.
Software: Python 3.7+, Pygame 2.0+, and supporting libraries.

#### THEORY:
Pathfinding in Games:
Pathfinding is a fundamental AI concept in games, enabling characters to navigate obstacles efficiently. The A* algorithm is widely used due to its heuristic-based approach, balancing optimality and computational efficiency.

#### Pygame Framework:
Pygame is a cross-platform library designed to create 2D games. It provides tools to handle graphics, sprites, and input events effectively.

#### PROCEDURE:
Step 1: Setup
Initialize the Pygame environment and create the game window.
Define a grid-based matrix representing the game map (1 = walkable, 0 = obstacle).
Step 2: Implement Pathfinder
Create a Pathfinder class to handle:
Pathfinding using A*.
Management of collectibles and scoring.
Step 3: Design Game Elements
Player (Roomba): Implement sprite-based movement along calculated paths.
Enemies: Create AI-driven enemy movement to add challenge.
Collectibles: Randomly spawn items on walkable tiles and manage scoring.
Step 4: Main Menu
Add a simple start menu allowing the user to begin the game.
Step 5: Game Loop
Handle user inputs (e.g., mouse clicks for setting path).
Update game elements (Roomba movement, enemy actions).
Render the grid, path, collectibles, and score in real-time.
Step 6: Test

#### SOURCE CODE:
```
import pygame
import sys


class Pathfinder:
    def __init__(self, matrix):
        self.matrix = matrix
        self.grid = Grid(matrix=matrix)
        self.select_surf = pygame.image.load ("C:\Users\coron\Documents\Ai for games\selection.png").convert_alpha()

        # Pathfinding
        self.path = []

        # Roomba
        self.roomba = pygame.sprite.GroupSingle(Roomba(self.empty_path))

        # Add a scoring system
        self.score = 0
        self.collectibles = self.place_collectibles()

    def place_collectibles(self):
        # Randomly place collectibles on the grid
        collectibles = []
        for _ in range(5):  # Place 5 collectibles
            x, y = random.randint(0, len(self.matrix[0]) - 1), random.randint(0, len(self.matrix) - 1)
            if self.matrix[y][x] == 1:  # Only place on walkable tiles
                collectibles.append((x, y))
        return collectibles

    def collect(self, pos):
        if pos in self.collectibles:
            self.collectibles.remove(pos)
            self.score += 10  # Add to score for each collected item
            print(f'Score: {self.score}')

    def draw_collectibles(self):
        for col, row in self.collectibles:
            rect = pygame.Rect((col * 32, row * 32), (32, 32))
            pygame.draw.rect(screen, (255, 215, 0), rect)  # Draw collectible in gold color

    def update(self):
        self.draw_active_cell()
        self.draw_path()
        self.draw_collectibles()

        # Roomba updating and drawing
        self.roomba.update()
        self.roomba.draw(screen)

class Roomba(pygame.sprite.Sprite):
    def update(self):
        self.pos += self.direction * self.speed
        self.check_collisions()
        self.rect.center = self.pos

        # Check for collectible pickup
        col, row = self.get_coord()
        pathfinder.collect((col, row))

class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.image.load('"C:\Users\coron\Documents\Ai for games\roomba.png"').convert_alpha()
        self.rect = self.image.get_rect(center=(400, 400))
        self.speed = 2
        self.target_pos = (600, 600)

    def update(self):
        if self.rect.center != self.target_pos:
            direction = pygame.math.Vector2(self.target_pos) - pygame.math.Vector2(self.rect.center)
            direction = direction.normalize() * self.speed
            self.rect.center += direction

        # Change target randomly to simulate movement
        if random.randint(0, 100) < 5:  # 5% chance to change direction
            self.target_pos = (random.randint(0, 1280), random.randint(0, 736))

# Adding enemies in the game loop
enemies = pygame.sprite.Group(Enemy(), Enemy())  # Add more enemies as needed

while True:
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.MOUSEBUTTONDOWN:
            pathfinder.create_path()

    screen.blit(bg_surf, (0, 0))
    pathfinder.update()

    # Update and draw enemies
    enemies.update()
    enemies.draw(screen)

    pygame.display.update()
    clock.tick(60)

def main_menu():
    while True:
        screen.fill((0, 0, 0))
        font = pygame.font.Font(None, 74)
        title = font.render('Super Game', True, (255, 255, 255))
        screen.blit(title, (540, 200))

        play_button = pygame.Rect(540, 400, 200, 50)
        pygame.draw.rect(screen, (255, 0, 0), play_button)
        font = pygame.font.Font(None, 50)
        screen.blit(font.render('Play', True, (255, 255, 255)), (600, 410))

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.MOUSEBUTTONDOWN:
                if play_button.collidepoint(event.pos):
                    return  # Start the game

        pygame.display.update()

# Game loop
while True:
    main_menu()
    game_loop()  # Call the actual game loop

from pathfinding.core.diagonal_movement import DiagonalMovement
from pathfinding.core.grid import Grid
from pathfinding.finder.a_star import AStarFinder

matrix = [
  [1, 1, 1, 1, 1, 1],
  [1, 0, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1]]

# 1. create the grid with the nodes 
grid = Grid(matrix=matrix)

# get start and end point 
start = grid.node(0, 0)
end = grid.node(5, 2)

# create a finder with the movement style 
finder = AStarFinder() # can add DiagonalMovement as argument here with never or always + more

# returns a list with the path and the amount of times the finder had to run to get the path 
path, runs = finder.find_path(start, end, grid)


# print result 
print(path)
```
#### Output :
![Screenshot 2024-11-20 195514](https://github.com/user-attachments/assets/7f668062-8f78-4faa-925a-10ca49849f34)
![Screenshot 2024-11-20 195526](https://github.com/user-attachments/assets/e2732ee7-1d85-4576-a58d-9e02ab849e4a)
![Screenshot 2024-11-20 195545](https://github.com/user-attachments/assets/d8312cfb-0ad4-46d3-b421-27ecd867a8ee)

#### RESULT:
The experiment successfully demonstrates the implementation of the A* algorithm in a grid-based game, integrating interactive AI mechanics for pathfinding, collectibles, and enemy behavior.

