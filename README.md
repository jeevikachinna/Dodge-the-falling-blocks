# Dodge-the-falling-blocks
import pygame
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
WIDTH, HEIGHT = 600, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Dodge the Falling Blocks")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (200, 0, 0)
BLUE = (0, 0, 255)

# Clock and FPS
clock = pygame.time.Clock()
FPS = 60

# Player settings
player_width = 50
player_height = 10
player_x = WIDTH // 2 - player_width // 2
player_y = HEIGHT - player_height - 10
player_speed = 7

# Block settings
block_width = 50
block_height = 20
block_speed = 5
block_list = []

# Score
score = 0
font = pygame.font.SysFont(None, 36)

# Function to create blocks
def create_block():
    x_pos = random.randint(0, WIDTH - block_width)
    block_list.append(pygame.Rect(x_pos, 0, block_width, block_height))

# Main game loop
running = True
block_timer = 0

while running:
    clock.tick(FPS)
    screen.fill(WHITE)

    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
        player_x += player_speed

    # Create new block periodically
    block_timer += 1
    if block_timer > 30:
        create_block()
        block_timer = 0

    # Move blocks and check for collision
    player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
    for block in block_list[:]:
        block.y += block_speed
        if block.y > HEIGHT:
            block_list.remove(block)
            score += 1
        if block.colliderect(player_rect):
            running = False  # Game Over

    # Draw player
    pygame.draw.rect(screen, BLUE, player_rect)

    # Draw blocks
    for block in block_list:
        pygame.draw.rect(screen, RED, block)

    # Draw score
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))

    # Update display
    pygame.display.flip()

# Game over screen
screen.fill(WHITE)
game_over_text = font.render("Game Over!", True, RED)
final_score = font.render(f"Final Score: {score}", True, BLACK)
screen.blit(game_over_text, (WIDTH // 2 - 80, HEIGHT // 2 - 30))
screen.blit(final_score, (WIDTH // 2 - 90, HEIGHT // 2 + 10))
pygame.display.flip()
pygame.time.wait(3000)

pygame.quit()
