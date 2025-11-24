# --- Simplified Tic-Tac-Toe with Unification + Alpha-Beta + User Input (fixed) ---

def unify(a, b):
    """Very simple unification for small terms like ('line', [X,O,X])"""
    if a == b:
        return {}
    if isinstance(a, str) and a.islower():  # variable
        return {a: b}
    if isinstance(b, str) and b.islower():
        return {b: a}
    if isinstance(a, tuple) and isinstance(b, tuple):
        if a[0] != b[0] or len(a[1]) != len(b[1]):
            return None
        subs = {}
        for x, y in zip(a[1], b[1]):
            s = unify(x, y)
            if s is None:
                return None
            subs.update(s)
        return subs
    return None


# Winning triples (rows, cols, diagonals)
WIN_TRIPLES = [
    (0,1,2), (3,4,5), (6,7,8),
    (0,3,6), (1,4,7), (2,5,8),
    (0,4,8), (2,4,6)
]

def winner(board):
    """Return 'X' or 'O' if someone has won, else None."""
    for i, j, k in WIN_TRIPLES:
        term = ('line', [board[i], board[j], board[k]])
        if unify(term, ('line', ['X','X','X'])):
            return 'X'
        if unify(term, ('line', ['O','O','O'])):
            return 'O'
    return None

def is_full(board):
    return all(c != '_' for c in board)

def evaluate(board):
    w = winner(board)
    if w == 'X': return 1
    if w == 'O': return -1
    if is_full(board): return 0
    return None

def alpha_beta(board, player, alpha=-float('inf'), beta=float('inf')):
    """Alpha-beta minimax search."""
    val = evaluate(board)
    if val is not None:
        return val, None

    # Prefer center first
    moves = sorted([i for i,c in enumerate(board) if c == '_'],
                   key=lambda i: (i != 4, i))

    best_move = None
    if player == 'X':
        max_eval = -float('inf')
        for m in moves:
            new_board = board[:]
            new_board[m] = 'X'
            eval_, _ = alpha_beta(new_board, 'O', alpha, beta)
            if eval_ > max_eval:
                max_eval, best_move = eval_, m
            alpha = max(alpha, eval_)
            if beta <= alpha:
                break
        return max_eval, best_move
    else:
        min_eval = float('inf')
        for m in moves:
            new_board = board[:]
            new_board[m] = 'O'
            eval_, _ = alpha_beta(new_board, 'X', alpha, beta)
            if eval_ < min_eval:
                min_eval, best_move = eval_, m
            beta = min(beta, eval_)
            if beta <= alpha:
                break
        return min_eval, best_move


def print_board(b):
    print()
    for i in range(0, 9, 3):
        print(' '.join(b[i:i+3]))
    print("\nIndexes: 0 1 2 | 3 4 5 | 6 7 8\n")


# --- Main Game Loop ---
def play():
    board = ['_'] * 9
    print_board(board)

    while True:
        # --- AI Move (X) ---
        print("AI (X) thinking...")
        _, move = alpha_beta(board, 'X')
        board[move] = 'X'
        print_board(board)

        w = winner(board)
        if w:
            print("X wins!")
            break
        if is_full(board):
            print("Draw!")
            break

        # --- Player Move (O) ---
        while True:
            try:
                move = int(input("Your move (0–8): "))
                if 0 <= move <= 8 and board[move] == '_':
                    board[move] = 'O'
                    break
                else:
                    print("Invalid or occupied. Try again.")
            except ValueError:
                print("Please enter a number 0–8.")

        print_board(board)

        w = winner(board)
        if w:
            print("O wins! Well played.")
            break
        if is_full(board):
            print("Draw!")
            break


# --- Run the game ---
if __name__ == "__main__":
    play()
