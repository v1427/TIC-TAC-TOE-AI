# TIC-TAC-TOE-AI
# Tic-Tac-Toe board representation
board = [" " for _ in range(9)]  # 3x3 board

# Function to display the board
def display_board():
    print("\n")
    print(f" {board[0]} | {board[1]} | {board[2]} ")
    print("-----------")
    print(f" {board[3]} | {board[4]} | {board[5]} ")
    print("-----------")
    print(f" {board[6]} | {board[7]} | {board[8]} ")
    print("\n")

# Function to check if the board is full
def is_board_full():
    return " " not in board

# Function to check if a player has won
def check_winner(player):
    # Check rows, columns, and diagonals
    winning_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Columns
        [0, 4, 8], [2, 4, 6]              # Diagonals
    ]
    for combo in winning_combinations:
        if board[combo[0]] == board[combo[1]] == board[combo[2]] == player:
            return True
    return False

# Minimax algorithm
def minimax(board, depth, is_maximizing):
    # Base cases: check for terminal states
    if check_winner("X"):
        return -1  # Human wins
    if check_winner("O"):
        return 1   # AI wins
    if is_board_full():
        return 0   # Draw

    if is_maximizing:
        best_score = -float("inf")
        for i in range(9):
            if board[i] == " ":
                board[i] = "O"
                score = minimax(board, depth + 1, False)
                board[i] = " "
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = float("inf")
        for i in range(9):
            if board[i] == " ":
                board[i] = "X"
                score = minimax(board, depth + 1, True)
                board[i] = " "
                best_score = min(score, best_score)
        return best_score

# Function for AI's move using Minimax
def ai_move():
    best_score = -float("inf")
    best_move = None
    for i in range(9):
        if board[i] == " ":
            board[i] = "O"
            score = minimax(board, 0, False)
            board[i] = " "
            if score > best_score:
                best_score = score
                best_move = i
    board[best_move] = "O"

# Main game loop
def play_game():
    print("Welcome to Tic-Tac-Toe! You are X, and the AI is O.")
    display_board()

    while True:
        # Human's move
        try:
            move = int(input("Enter your move (0-8): "))
            if board[move] != " ":
                print("Invalid move! Try again.")
                continue
            board[move] = "X"
        except (ValueError, IndexError):
            print("Invalid input! Enter a number between 0 and 8.")
            continue

        display_board()

        # Check if human wins
        if check_winner("X"):
            print("Congratulations! You win!")
            break

        # Check for a draw
        if is_board_full():
            print("It's a draw!")
            break

        # AI's move
        print("AI is making a move...")
        ai_move()
        display_board()

        # Check if AI wins
        if check_winner("O"):
            print("AI wins! Better luck next time.")
            break

        # Check for a draw
        if is_board_full():
            print("It's a draw!")
            break

# Run the game
play_game()
