import pygame

# Initialize Pygame
pygame.init()

# Define colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)
GRAY = (200, 200, 200)

# Set up the game window
WINDOW_SIZE = 300
window = pygame.display.set_mode((WINDOW_SIZE, WINDOW_SIZE + 100))
pygame.display.set_caption("Tic Tac Toe")

# Define the game board
board = [[None, None, None],
         [None, None, None],
         [None, None, None]]

# Define the player's turn
player = 'X'

# Define the font
font = pygame.font.Font(None, 72)
score_font = pygame.font.Font(None, 36)
button_font = pygame.font.Font(None, 24)

# Initialize scores
score_x = 0
score_o = 0

# Define the function to draw the board
def draw_board():
    window.fill(WHITE)
    line_width = 6
    pygame.draw.line(window, BLACK, (0, WINDOW_SIZE // 3), (WINDOW_SIZE, WINDOW_SIZE // 3), line_width)
    pygame.draw.line(window, BLACK, (0, 2 * WINDOW_SIZE // 3), (WINDOW_SIZE, 2 * WINDOW_SIZE // 3), line_width)
    pygame.draw.line(window, BLACK, (WINDOW_SIZE // 3, 0), (WINDOW_SIZE // 3, WINDOW_SIZE), line_width)
    pygame.draw.line(window, BLACK, (2 * WINDOW_SIZE // 3, 0), (2 * WINDOW_SIZE // 3, WINDOW_SIZE), line_width)

    for row in range(3):
        for col in range(3):
            if board[row][col] == 'X':
                text = font.render('X', True, RED)
            elif board[row][col] == 'O':
                text = font.render('O', True, BLUE)
            else:
                continue

            text_rect = text.get_rect()
            text_rect.centerx = col * WINDOW_SIZE // 3 + WINDOW_SIZE // 6
            text_rect.centery = row * WINDOW_SIZE // 3 + WINDOW_SIZE // 6
            window.blit(text, text_rect)

    # Draw scoreboard
    score_x_text = score_font.render(f"X: {score_x}", True, BLACK)
    score_o_text = score_font.render(f"O: {score_o}", True, BLACK)
    window.blit(score_x_text, (10, WINDOW_SIZE + 10))
    window.blit(score_o_text, (WINDOW_SIZE - 50, WINDOW_SIZE + 10))

    # Draw restart button
    restart_button = pygame.Rect(WINDOW_SIZE // 2 - 50, WINDOW_SIZE + 50, 100, 30)
    pygame.draw.rect(window, GRAY, restart_button)
    restart_text = button_font.render("Restart", True, BLACK)
    restart_text_rect = restart_text.get_rect()
    restart_text_rect.center = restart_button.center
    window.blit(restart_text, restart_text_rect)

    pygame.display.update()

# Define the function to check for a winner
def check_winner():
    global score_x, score_o

    # Check rows
    for row in range(3):
        if board[row][0] == board[row][1] == board[row][2] != None:
            if board[row][0] == 'X':
                score_x += 1
                winning_animation(row, 0, row, 1, row, 2)
            else:
                score_o += 1
                winning_animation(row, 0, row, 1, row, 2)
            return board[row][0]

    # Check columns
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] != None:
            if board[0][col] == 'X':
                score_x += 1
                winning_animation(0, col, 1, col, 2, col)
            else:
                score_o += 1
                winning_animation(0, col, 1, col, 2, col)
            return board[0][col]

    # Check diagonals
    if board[0][0] == board[1][1] == board[2][2] != None:
        if board[0][0] == 'X':
            score_x += 1
            winning_animation(0, 0, 1, 1, 2, 2)
        else:
            score_o += 1
            winning_animation(0, 0, 1, 1, 2, 2)
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0] != None:
        if board[0][2] == 'X':
            score_x += 1
            winning_animation(0, 2, 1, 1, 2, 0)
        else:
            score_o += 1
            winning_animation(0, 2, 1, 1, 2, 0)
        return board[0][2]

    # Check for a tie
    if None not in [cell for row in board for cell in row]:
        return 'Tie'

    return None

# Define the function for winning animation
def winning_animation(x1, y1, x2, y2, x3, y3):
    line_color = RED if player == 'X' else BLUE
    line_width = 5
    pygame.draw.line(window, line_color, (y1 * WINDOW_SIZE // 3 + WINDOW_SIZE // 6, x1 * WINDOW_SIZE // 3 + WINDOW_SIZE // 6),
                     (y3 * WINDOW_SIZE // 3 + WINDOW_SIZE // 6, x3 * WINDOW_SIZE // 3 + WINDOW_SIZE // 6), line_width)
    pygame.display.update()
    pygame.time.delay(500)

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.MOUSEBUTTONDOWN:
            mouse_pos = pygame.mouse.get_pos()
            row = mouse_pos[1] // (WINDOW_SIZE // 3)
            col = mouse_pos[0] // (WINDOW_SIZE // 3)

            # Check if the restart button was clicked
            restart_button = pygame.Rect(WINDOW_SIZE // 2 - 50, WINDOW_SIZE + 50, 100, 30)
            if restart_button.collidepoint(mouse_pos):
                board = [[None, None, None],
                         [None, None, None],
                         [None, None, None]]
                player = 'X'
                score_x = 0
                score_o = 0

            elif board[row][col] is None:
                board[row][col] = player
                winner = check_winner()
                if winner:
                    if winner == 'Tie':
                        print("It's a tie!")
                    else:
                        print(f"Player {winner} wins!")
                    board = [[None, None, None],
                             [None, None, None],
                             [None, None, None]]
                else:
                    player = 'O' if player == 'X' else 'X'

    draw_board()

# Quit Pygame
pygame.quit()

daaaa kivi dabla

from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.config import Config

# Set the window size
Config.set('graphics', 'width', '400')
Config.set('graphics', 'height', '400')

class TicTacToeButton(Button):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.font_size = 30

class TicTacToeGame(GridLayout):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.cols = 3
        self.rows = 4
        self.player_turn = 'X'
        self.buttons = []
        self.score_x = 0
        self.score_o = 0

        for _ in range(9):
            button = TicTacToeButton()
            self.add_widget(button)
            button.bind(on_press=self.button_pressed)
            self.buttons.append(button)

        self.score_label = Label(text=f'X: {self.score_x}  O: {self.score_o}', font_size=20)
        self.add_widget(self.score_label)

    def button_pressed(self, instance):
        if instance.text == '':
            instance.text = self.player_turn
            self.check_winner()
            self.toggle_player_turn()

    def toggle_player_turn(self):
        self.player_turn = 'O' if self.player_turn == 'X' else 'X'

    def check_winner(self):
        buttons = self.buttons
        winning_combinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ]

        for combination in winning_combinations:
            button1, button2, button3 = [buttons[i] for i in combination]
            if button1.text == button2.text == button3.text != '':
                winner = button1.text
                print(f'Winner: {winner}')
                self.update_score(winner)
                self.reset_game()
                return

        if all(button.text != '' for button in buttons):
            print('It\'s a tie!')
            self.reset_game()

    def update_score(self, winner):
        if winner == 'X':
            self.score_x += 1
        else:
            self.score_o += 1
        self.score_label.text = f'X: {self.score_x}  O: {self.score_o}'

    def reset_game(self):
        for button in self.buttons:
            button.text = ''
        self.player_turn = 'X'

class TicTacToeApp(App):
    def build(self):
        game = TicTacToeGame()
        return game

if __name__ == '__main__':
    TicTacToeApp().run()
