# Ai4
import pygame
import random

# 初始化 Pygame
pygame.init()

# 设置窗口
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption('简单射击游戏')

# 颜色
black = (0, 0, 0)
white = (255, 255, 255)

# 玩家设置
player_width, player_height = 50, 50
player_x = width // 2
player_y = height - player_height - 10
player_speed = 5

# 子弹设置
bullet_width, bullet_height = 5, 10
bullets = []

# 敌人设置
enemy_width, enemy_height = 50, 50
enemies = [[random.randint(0, width - enemy_width), 0]]

# 游戏循环
running = True
while running:
    screen.fill(black)
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < width - player_width:
        player_x += player_speed
    if keys[pygame.K_SPACE]:
        bullets.append([player_x + player_width // 2, player_y])

    # 更新子弹位置
    for bullet in bullets:
        bullet[1] -= 10
        if bullet[1] < 0:
            bullets.remove(bullet)

    # 更新敌人位置
    for enemy in enemies:
        enemy[1] += 2
        if enemy[1] > height:
            enemies.remove(enemy)
            enemies.append([random.randint(0, width - enemy_width), 0])

    # 绘制玩家
    pygame.draw.rect(screen, white, (player_x, player_y, player_width, player_height))

    # 绘制子弹
    for bullet in bullets:
        pygame.draw.rect(screen, white, (bullet[0], bullet[1], bullet_width, bullet_height))

    # 绘制敌人
    for enemy in enemies:
        pygame.draw.rect(screen, white, (enemy[0], enemy[1], enemy_width, enemy_height))

    pygame.display.flip()
    pygame.time.Clock().tick(30)

pygame.quit()