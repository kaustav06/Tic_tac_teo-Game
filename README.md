import math
def print_board(board):
    print(f"\n{board[0]} | {board[1]} | {board[2]}")
    print("--+---+--")
    print(f"{board[3]} | {board[4]} | {board[5]}")
    print("--+---+--")
    print(f"{board[6]} | {board[7]} | {board[8]}\n")

def check_win(board, player):
    win_conditions = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], 
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  
        [0, 4, 8], [2, 4, 6]              
    ]
    for condition in win_conditions:
        if board[condition[0]] == board[condition[1]] == board[condition[2]] == player:
            return True
    return False

def check_draw(board):
    return all(cell in ['X', 'O'] for cell in board)

def minimax(board, depth, is_maximizing):
    player = 'X'
    computer = 'O'

   if check_win(board, computer):
        return 1
    elif check_win(board, player):
        return -1
    elif check_draw(board):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = computer
                score = minimax(board, depth + 1, False)
                board[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for i in range(9):
            if board[i] == ' ':
                board[i] = player
                score = minimax(board, depth + 1, True)
                board[i] = ' '
                best_score = min(score, best_score)
        return best_score

def computer_move(board):
    best_score = -math.inf
    best_move = None
    computer = 'O'

    for i in range(9):
        if board[i] == ' ':
            board[i] = computer
            score = minimax(board, 0, False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                best_move = i

    return best_move

def play_game():
    board = [' ' for _ in range(9)] 
    player = 'X'
    computer = 'O'

    while True:
        print_board(board)

    
        try:
            move = int(input(f"Player {player}, enter your move (1-9): ")) - 1
            if move < 0 or move >= 9 or board[move] != ' ':
                print("Invalid move. Try again.")
                continue
        except ValueError:
            print("Invalid input. Please enter a number between 1 and 9.")
            continue

        board[move] = player

        if check_win(board, player):
            print_board(board)
            print(f"Player {player} wins!")
            break
        elif check_draw(board):
            print_board(board)
            print("It's a draw!")
            break

        
        print("Computer's turn...")
        comp_move = computer_move(board)
        board[comp_move] = computer

        if check_win(board, computer):
            print_board(board)
            print("Computer wins!")
            break
        elif check_draw(board):
            print_board(board)
            print("It's a draw!")
            break

if __name__ == "__main__":
    play_game()
