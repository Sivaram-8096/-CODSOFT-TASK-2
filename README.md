# -CODSOFT-TASK-2
import math

# Create the game board
board = [' ' for _ in range(9)]

def print_board():
    print("\n")
    for row in [board[i:i+3] for i in range(0, 9, 3)]:
        print("| " + " | ".join(row) + " |")
    print("\n")

def is_winner(player):
    win_conditions = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Columns
        [0, 4, 8], [2, 4, 6]              # Diagonals
    ]
    return any(all(board[i] == player for i in combo) for combo in win_conditions)

def is_draw():
    return ' ' not in board

def minimax(is_maximizing):
    if is_winner('O'):
        return 1
    elif is_winner('X'):
        return -1
    elif is_draw():
        return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'O'
                score = minimax(False)
                board[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'X'
                score = minimax(True)
                board[i] = ' '
                best_score = min(score, best_score)
        return best_score

def best_move():
    best_score = -math.inf
    move = 0
    for i in range(9):
        if board[i] == ' ':
            board[i] = 'O'
            score = minimax(False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                move = i
    return move

# Game loop
def play_game():
    print("Welcome to Tic-Tac-Toe! You are 'X'. AI is 'O'.")
    print_board()

    while True:
        # Human Move
        move = int(input("Enter your move (0-8): "))
        if board[move] != ' ':
            print("Invalid move! Try again.")
            continue
        board[move] = 'X'
        print_board()

        if is_winner('X'):
            print("You win!")
            break
        elif is_draw():
            print("It's a draw!")
            break

        # AI Move
        ai = best_move()
        board[ai] = 'O'
        print("AI played:")
        print_board()

        if is_winner('O'):
            print("AI wins!")
            break
        elif is_draw():
            print("It's a draw!")
            break

# Start game
play_game()
