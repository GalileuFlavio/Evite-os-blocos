# Evite-os-blocos
"Evite os blocos" com PyGame

import pygame
import random

# Inicializando o PyGame
pygame.init()

# Definindo cores
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Dimensões da tela
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))

# Definindo a velocidade do jogo
clock = pygame.time.Clock()

# Definindo o jogador
player_width = 50
player_height = 50
player_x = SCREEN_WIDTH // 2 - player_width // 2
player_y = SCREEN_HEIGHT - player_height - 10
player_speed = 5

# Definindo os blocos inimigos
enemy_width = 50
enemy_height = 50
enemy_speed = 5

# Função para criar blocos
def create_enemy():
    x = random.randint(0, SCREEN_WIDTH - enemy_width)
    y = -enemy_height
    return [x, y]

# Lista de inimigos
enemies = [create_enemy()]

# Função para mover os inimigos
def move_enemies(enemies):
    for enemy in enemies:
        enemy[1] += enemy_speed
    return enemies

# Função para detectar colisão
def detect_collision(player_rect, enemies):
    for enemy in enemies:
        enemy_rect = pygame.Rect(enemy[0], enemy[1], enemy_width, enemy_height)
        if player_rect.colliderect(enemy_rect):
            return True
    return False

# Função principal do jogo
def game_loop():
    global player_x, player_y
    running = True
    while running:
        # Tratando eventos
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Movendo o jogador
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and player_x > 0:
            player_x -= player_speed
        if keys[pygame.K_RIGHT] and player_x < SCREEN_WIDTH - player_width:
            player_x += player_speed

        # Movendo os inimigos
        move_enemies(enemies)

        # Adicionando novos inimigos
        if len(enemies) < 10:
            enemies.append(create_enemy())

        # Checando colisão
        player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
        if detect_collision(player_rect, enemies):
            running = False

        # Atualizando a tela
        screen.fill(WHITE)
        pygame.draw.rect(screen, BLACK, player_rect)

        for enemy in enemies:
            pygame.draw.rect(screen, RED, (enemy[0], enemy[1], enemy_width, enemy_height))

        pygame.display.update()
        clock.tick(30)

    pygame.quit()

# Iniciando o jogo
game_loop()
