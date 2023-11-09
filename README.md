import pygame

# Define as constantes
WIDTH = 640
HEIGHT = 480

# Define as classes
class Mario(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.image.load("mario.png")
        self.rect = self.image.get_rect()
        self.rect.centerx = WIDTH / 2
        self.rect.bottom = HEIGHT - 100

    def update(self):
        # Move o Mario
        self.rect.x += self.velx
        self.rect.y += self.vely

        # Verifica se o Mario atingiu o limite da tela
        if self.rect.left < 0:
            self.rect.left = 0
        elif self.rect.right > WIDTH:
            self.rect.right = WIDTH
        if self.rect.top < 0:
            self.rect.top = 0
        elif self.rect.bottom > HEIGHT:
            self.rect.bottom = HEIGHT

    def jump(self):
        # Dá um salto para cima
        self.vely = -10


class Bloco(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.image.load("bloco.png")
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y


# Inicializa o Pygame
pygame.init()

# Cria a tela
screen = pygame.display.set_mode((WIDTH, HEIGHT))

# Cria os sprites
mario = Mario()
blocos = pygame.sprite.Group()
for x in range(0, WIDTH, 100):
    bloco = Bloco(x, 100)
    blocos.add(bloco)

# Inicia o loop do jogo
running = True
while running:
    # Processa os eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        # Verifica as teclas pressionadas
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                mario.velx = -5
            elif event.key == pygame.K_RIGHT:
                mario.velx = 5
            elif event.key == pygame.K_SPACE:
                mario.jump()

    # Atualiza os sprites
    blocos.update()
    mario.update()

    # Desenha os sprites
    screen.fill((0, 0, 0))
    blocos.draw(screen)
    mario.draw(screen)

    # Atualiza a tela
    pygame.display.flip()

# Finaliza o Pygame
pygame.quit()
