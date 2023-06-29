import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the game window
width = 400
height = 600
window = pygame.display.set_mode((width, height))
pygame.display.set_caption("Flappy Bird")

# Load game assets
background_image = pygame.image.load("background.png")
bird_image = pygame.image.load("bird.png")
pipe_image = pygame.image.load("pipe.png")
game_over_image = pygame.image.load("game_over.png")

# Bird properties
bird_x = 50
bird_y = 300
bird_y_velocity = 0
gravity = 0.5

# Pipe properties
pipe_x = width
pipe_width = 70
pipe_height = random.randint(100, 400)
pipe_gap = 150
pipe_speed = 3

# Game variables
score = 0
game_over = False

# Main game loop
while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird_y_velocity = -10
    
    # Update bird position
    bird_y += bird_y_velocity
    bird_y_velocity += gravity
    
    # Check collision with ground
    if bird_y > height:
        game_over = True
    
    # Update pipe position
    pipe_x -= pipe_speed
    
    # Check collision with pipe
    if bird_x + 20 > pipe_x and bird_x < pipe_x + pipe_width:
        if bird_y < pipe_height or bird_y + 20 > pipe_height + pipe_gap:
            game_over = True
    
    # Check if pipe passed bird
    if pipe_x + pipe_width < bird_x:
        score += 1
        pipe_x = width
        pipe_height = random.randint(100, 400)
    
    # Draw game objects
    window.blit(background_image, (0, 0))
    window.blit(bird_image, (bird_x, bird_y))
    window.blit(pipe_image, (pipe_x, pipe_height))
    window.blit(pipe_image, (pipe_x, pipe_height + pipe_gap))
    pygame.display.update()
    
# Display game over screen
window.blit(game_over_image, (50, 200))
pygame.display.update()

# Quit Pygame
pygame.quit()
