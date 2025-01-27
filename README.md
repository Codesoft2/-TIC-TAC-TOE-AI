import math
Player_X = 'X'
Player_O = 'O'
Empty = ' '

def print_board(board):
    for row in board:
        print('|'.join(row))
        print('-' * 5)

def game_over(board):
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] != Empty:
            return True
        if board[0][i] == board[1][i] == board[2][i] != Empty:
            return True
    if board[0][0] == board[1][1] == board[2][2] != Empty:
        return True
    if board[0][2] == board[1][1] == board[2][0] != Empty:
        return True
    return False

def draw(board):
    return all(cell != Empty for row in board for cell in row) and not game_over(board)

def minimax(board, depth, alpha, beta, is_maximizing):
    if game_over(board):
        return -1 if is_maximizing else 1
    if draw(board):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == Empty:
                    board[i][j] = Player_O
                    score = minimax(board, depth + 1, alpha, beta, False)
                    board[i][j] = Empty
                    best_score = max(score, best_score)
                    alpha = max(alpha, score)
                    if beta <= alpha:
                        break
        return best_score
    else:
        best_score = math.inf
        for i in range(3):
            for j in range(3):
                if board[i][j] == Empty:
                    board[i][j] = Player_X
                    score = minimax(board, depth + 1, alpha, beta, True)
                    board[i][j] = Empty
                    best_score = min(score, best_score)
                    beta = min(beta, score)
                    if beta <= alpha:
                        break
        return best_score

def find_best_move(board):
    best_score = -math.inf
    best_move = (-1, -1)
    for i in range(3):
        for j in range(3):
            if board[i][j] == Empty:
                board[i][j] = Player_O
                score = minimax(board, 0, -math.inf, math.inf, False)
                board[i][j] = Empty
                if score > best_score:
                    best_score = score
                    best_move = (i, j)
    return best_move

def main():
    board = [[Empty for _ in range(3)] for _ in range(3)]
    print("Welcome to Tic-Tac-Toe!")
    print_board(board)

    while True:
        # Player X's turn
        while True:
            try:
                row = int(input("Enter row (0, 1, 2): "))
                col = int(input("Enter column (0, 1, 2): "))
                if board[row][col] == Empty:
                    board[row][col] = Player_X
                    break
                else:
                    print("Cell already taken. Try again.")
            except (ValueError, IndexError):
                print("Invalid input. Please enter row and column as 0, 1, or 2.")

        print_board(board)

        if game_over(board):
            print("Player X wins!")
            break
        if draw(board):
            print("It's a draw!")
            break

        print("AI is making a move...")
        ai_move = find_best_move(board)
        if ai_move != (-1, -1):
            board[ai_move[0]][ai_move[1]] = Player_O
            print_board(board)

            if game_over(board):
                print("Player O (AI) wins!")
                break
            if draw(board):
                print("It's a draw!")
                break

if __name__ == "__main__":
    main()














